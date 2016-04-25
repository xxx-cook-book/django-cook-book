# Persistent Connections

## CONN_MAX_AGE

```
CONN_MAX_AGE

Default: 0

The lifetime of a database connection, in seconds. Use 0 to close database connections at the end of each request — Django’s historical behavior — and None for unlimited persistent connections.
```

* The value of ``CONN_MAX_AGE`` should be set less than the value of DB's IDLE/TIMEOUT
* MySQL is wait_timeout
  * http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_wait_timeout

##  Settings.py

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'chipdb',
        'USER': 'chip',
        'PASSWORD': 'mypassword',
        'HOST': 'chip.mysql.rds.aliyuncs.com',
        'PORT': '3306',
        'CONN_MAX_AGE': 60,  # The lifetime of a database connection, in seconds. Default 0.
      }
 }
```

## Problems

* MySQL server has gone away

  * Reference

    * [Django, after upgrade: MySQL server has gone away](http://stackoverflow.com/questions/26958592/django-after-upgrade-mysql-server-has-gone-away)

    * [(2006, 'MySQL server has gone away') in django1.6 when wait_timeout passed](https://code.djangoproject.com/ticket/21597#comment:12)

* python manage.py runserver

  * The value of ``CONN_MAX_AGE`` is more than the value of DB's IDLE/TIMEOUT

* python manage.py shell

  * Old django would close every connection right away, django 1.6 checks with CONN_MAX_AGE

    1. It gets CONN_MAX_AGE from DATABASES, sets close_at

       ```python
       max_age = self.settings_dict['CONN_MAX_AGE']
       self.close_at = None if max_age is None else time.time() + max_age
       ```

    2. Actually the code above affects close_if_unusable_or_obsolete, which closes the connection if 'self.close_at is not None and time.time() >= self.close_at'

    3. close_if_unusable_or_obsolete itself is being called by close_old_connections, which in turn is a request handler for signals.request_started and signals.request_finished

  * We have a worker, which is effectively a django app but it doesn't process any HTTP requests. In fact that makes all connections persistent because close_old_connections never gets called

* Solution

  * python manage.py runserver

    * Set the value of ``CONN_MAX_AGE`` less than the value of DB's IDLE/TIMEOUT

  * python manage.py shell

    * Close the connection when you know that your program is going to be idle for a long time

      ```python
      try:
          from django.db import close_old_connections as close_connection
      except ImportError:
          from django.db import close_connection

      close_connection()
      ```

      * https://github.com/django/django/blob/fc94944183f1f1325824ee0ef1f49d737ec86d4a/django/db/__init__.py#L101-L112
      * close_connection is superseded by close_old_connections. RemovedInDjango18Warning


​