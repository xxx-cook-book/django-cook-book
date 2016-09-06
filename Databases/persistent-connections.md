# Persistent Connections

## Since Django 1.6

* _New In Django 1.6_

* See https://docs.djangoproject.com/en/dev/releases/1.6/#persistent-database-connections

  Django now supports reusing the same database connection for several requests. This avoids the overhead of re-establishing a connection at the beginning of each request. For backwards compatibility, this feature is disabled by default. See [Persistent connections](https://docs.djangoproject.com/en/dev/ref/databases/#persistent-database-connections) for details.

## CONN_MAX_AGE

```
CONN_MAX_AGE

Default: 0

The lifetime of a database connection, in seconds. Use 0 to close database connections at the end of each request — Django’s historical behavior — and None for unlimited persistent connections.
```

* The value of ``CONN_MAX_AGE`` should be set less than the value of DB's IDLE/TIMEOUT
* MySQL's wait_timeout
  * http://dev.mysql.com/doc/refman/5.5/en/server-system-variables.html#sysvar_wait_timeout

## Thread Safety

```
Since each thread maintains its own connection, your database must support at least as many simultaneous connections as you have worker threads.

The development server creates a new thread for each request it handles, negating the effect of persistent connections. Don’t enable them during development.
```
* Source Code

  ```python
  from threading import local

  class ConnectionHandler(object):
      def __init__(self, databases=None):
          """
          databases is an optional dictionary of database definitions (structured
          like settings.DATABASES).
          """
          self._databases = databases
          self._connections = local()

      def __getitem__(self, alias):
          if hasattr(self._connections, alias):
              return getattr(self._connections, alias)

          self.ensure_defaults(alias)
          db = self.databases[alias]
          backend = load_backend(db['ENGINE'])
          conn = backend.DatabaseWrapper(db, alias)
          setattr(self._connections, alias, conn)
          return conn
  ```

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

  * References

    * [Django, after upgrade: MySQL server has gone away](http://stackoverflow.com/questions/26958592/django-after-upgrade-mysql-server-has-gone-away)

    * [(2006, 'MySQL server has gone away') in django1.6 when wait_timeout passed](https://code.djangoproject.com/ticket/21597#comment:12)

  * Reasons

    * ``uWSGI start``

      * The value of ``CONN_MAX_AGE`` is more than the value of DB's IDLE/TIMEOUT

    * ``python manage.py shell``

      * Old django would close every connection right away, django 1.6 checks with CONN_MAX_AGE

        1. It gets CONN_MAX_AGE from DATABASES, sets close_at

           ```python
           max_age = self.settings_dict['CONN_MAX_AGE']
           self.close_at = None if max_age is None else time.time() + max_age
           ```

        2. Actually the code above affects ``close_if_unusable_or_obsolete``, which closes the connection ``if 'self.close_at is not None and time.time() >= self.close_at'``

        3. ``close_if_unusable_or_obsolete`` itself is being called by ``close_old_connections``, which in turn is a request handler for ``signals.request_started`` and ``signals.request_finished``

      * We have a worker, which is effectively a django app but it doesn't process any HTTP requests. In fact that makes all connections persistent because ``close_old_connections`` never gets called

  * Solutions

    * ``uWSGI start``

      * Set the value of ``CONN_MAX_AGE`` less than the value of DB's IDLE/TIMEOUT

    * ``python manage.py shell``

      * Close the connection when you know that your program is going to be idle for a long time

        ```python
        from django_six import close_old_connections

        close_old_connections()

        # Some Query Codes

        close_old_connections()
        ```

        * https://github.com/django/django/blob/fc94944183f1f1325824ee0ef1f49d737ec86d4a/django/db/__init__.py#L101-L112
        * close_connection is superseded by close_old_connections. RemovedInDjango18Warning
        * [Django-six — Django compatibility library](https://github.com/django-xxx/django-six)

* Too many connections

  * Reasons

    * ``uWSGI start``

      * One uWSGI Thread One Connection, by default uWSGI starts with a single process and a single thread

        ```
        # number of worker processes
        processes       = 10
        # number of threads for each worker processes
        threads         = 5
        ```

      * connections = processes * threads = 10 * 5 = 50

    * ``python manage.py runserver``

      * The development server creates a new thread for each request it handles

## References

[1] Docs@DjangoProject, [Databases](https://docs.djangoproject.com/en/dev/ref/databases/)

[2] django-developers@GoogleGroup, [Persistent connections, take 2](https://groups.google.com/forum/#!topic/django-developers/rH0QQP7tI6w)