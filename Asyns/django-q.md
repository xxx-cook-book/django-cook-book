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
      ...
      'django_q',
      ...
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

    * test_func.py

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

    async('test_func.django_q_test_func', 10)
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

    taskid = async('test_func.django_q_test_func', 10)
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

    taskid = async('test_func.django_q_test_func', 10, hook='test_func.print_result')
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

## Supervisor

```python
[program:djangoq]
command=/home/diors/env/bin/python /home/diors/work/tt4it/manage.py qcluster
autostart=true
autorestart=true
startretries=3
exitcodes=0,1,2
stopsignal=INT
stdout_logfile=/home/diors/supervisorlog/supervisor_djangoq_access.log
stderr_logfile=/home/diors/supervisorlog/supervisor_djangoq_error.log
user=diors
```

* ``stopsignal=QUIT`` can't kill as expect

* Django Q stop

  ```python
  signal.signal(signal.SIGTERM, self.sig_handler)
  signal.signal(signal.SIGINT, self.sig_handler)

  def sig_handler(self, signum, frame):
      logger.debug(_('{} got signal {}').format(current_process().name, Conf.SIGNAL_NAMES.get(signum, 'UNKNOWN')))
      self.stop()
  ```

  * [django-q/django_q/cluster.py](https://github.com/Brightcells/django-q/blob/master/django_q/cluster.py#L48)

* [Examples of Program Configurations —— Postgres 8.X](http://supervisord.org/subprocess.html#postgres-8-x)

## Admin

## ``String`` vs. ``Instance``

```python
# Get the function from the task
logger.info(_('{} processing [{}]').format(name, task['name']))
f = task['func']
# if it's not an instance try to get it from the string
if not callable(f):
    try:
        module, func = f.rsplit('.', 1)
        m = importlib.import_module(module)
        f = getattr(m, func)
    except (ValueError, ImportError, AttributeError) as e:
        result = (e, False)
        if rollbar:
            rollbar.report_exc_info()
```

* Right

  ```python
  async('test_func.django_q_test_func', 10)

  or

  from test_func import django_q_test_func
  async(django_q_test_func, 10)
  ```

* Wrong

  ```python
  from test_func import django_q_test_func
  async('django_q_test_func', 10)
  # module, func = f.rsplit('.', 1)
  # 19:08:58 [Q] ERROR Failed [xxx] - need more than 1 value to unpack
  ```

* Solution

  ```python
  f = locals().get(f)
  if not f:
      module, func = f.rsplit('.', 1)
      m = importlib.import_module(module)
      f = getattr(m, func)
  ```

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