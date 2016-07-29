# Django Q

## Requirement

- [Django](https://www.djangoproject.com/) >= 1.8

## Installation

- Install the latest version with pip:

  ```shell
  $ pip install django-q
  ```

- Add django_q to your INSTALLED_APPS in your projects settings.py:

  ```python
  INSTALLED_APPS = (
      # other apps
      'django_q',
  )
  ```

- Run Django migrations to create the database tables:

  ```shell
  $ python manage.py migrate
  ```

## Settings.py

```python
try:
    from func_settings import redis_conf, redis_connect
    REDIS_CACHE = redis_connect(REDIS.get('default', {}))

    Q_CLUSTER = {
        'name': 'hongbaoyun',
        'workers': 8,
        'recycle': 500,
        'timeout': 60,
        'compress': True,
        'cpu_affinity': 1,
        'save_limit': 250,
        'queue_limit': 500,
        'label': 'Django Q',
        'redis_conn': REDIS_CACHE,
        # 'redis': redis_conf(REDIS.get('default', {})),
    }
except ImportError:
    REDIS_CACHE = None
```

* See [Redis_conn and ConnectionPool for Redis Broker](https://github.com/Koed00/django-q/pull/185)

## Usage

* Management Commands
  * Start a cluster with:
    ```
    $ python manage.py qcluster
    ```
  * Monitor your clusters with:
    ```
    $ python manage.py qmonitor
    ```
  * Check overall statistics with:
    ```
    $ python manage.py qinfo
    ```


* Tasks
  * Functions

    ```python
    def django_q_test_func(value):
        for i in range(1, value + 1):
            print i
            time.sleep(1)
        return 'Success'

    def print_result(task):
        print(task.result)
    ```

  * Just Async Exec

    ```python
    from django_q.tasks import async

    async('django_q_test_func', 10)
    ```

    * qcluster

      ```
      15:40:00 [Q] INFO Process-1:1 processing [spaghetti-yellow-ink-mirror]
      1
      2
      3
      4
      5
      6
      7
      8
      9
      10
      15:40:10 [Q] INFO Processed [spaghetti-yellow-ink-mirror]
      ```

  * Get Task_id/Result

    ```python
    from django_q.tasks import async

    taskid = async('django_q_test_func', 10)
    # 0a364fb6faad47a89523d1013ca8ad59
    ```

    * Get Result

      ```python
      In [1]: from django_q.tasks import result

      In [2]: result('0a364fb6faad47a89523d1013ca8ad59')
      Out[2]: 'Success'
      ```

  * Hook After Executed

    ```python
    from django_q.tasks import async

    taskid = async('django_q_test_func', 10, hook='print_result')
    ```

    * qcluster

      ```
      15:49:57 [Q] INFO Process-1:2 processing [sad-football-fix-floor]
      1
      2
      3
      4
      5
      6
      7
      8
      9
      10
      Success
      15:50:07 [Q] INFO Processed [sad-football-fix-floor]
      ```

* Schedules
  * TODO

## Admin

## Redis Connect from Django Q

```python
$ python manage.py shell
Python 2.7.5 (default, Mar  9 2014, 22:15:05)
Type "copyright", "credits" or "license" for more information.

IPython 4.0.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]: from django_q.brokers.redis_broker import Redis

In [2]: r = Redis()

In [3]: r.connection
Out[3]: StrictRedis<ConnectionPool<Connection<host=localhost,port=6379,db=1>>>
```

## References

[1] Koed00/django-q@Github, [A multiprocessing distributed task queue for Django](https://github.com/Koed00/django-q)