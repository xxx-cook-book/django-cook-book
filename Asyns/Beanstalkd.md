# Beanstalkd

## Beanstalkd

* Mac OS X

  * Installation

    ```shell
    brew install beanstalkd
    ```

  * Script

    * [OSX script for easy start & stop beanstalkd (brew version)](https://gist.github.com/finger-berlin/1942295)

* Ubuntu

  * Installation

    ```shell
    sudo aptitude install -y beanstalkd
    ```

  * Start

    ```shll
    # To start the service:
    service beanstalkd start
    ```

  * Stop

    ```shell
    # To stop the service:
    service beanstalkd stop
    ```

  * Restart

    ```shell
    # To restart the service:
    service beanstalkd restart
    ```

  * Status

    ```shell
    # To check the status:
    service beanstalkd status
    ```

## django-beanstalkd

* Installation

  ```shell
  pip install -e git+https://github.com/jonasvp/django-beanstalkd.git#egg=django-beanstalkd
  ```

* Settings.py

  * INSTALLED_APPS

    ```python
    INSTALLED_APPS = (
        ...
        'django_beanstalkd',
        ...
    )
    ```

  * BEANSTALK_SERVER

    ```python
    # My beanstalkd server
    BEANSTALK_SERVER = '127.0.0.1:11300'  # the default value, change 127.0.0.1 to ip
    ```


* Register Jobs

  * beanstalk_jobs.py
    * Create ``beanstalk_jobs.py`` under Django APP's dir
    * Use ``django_beanstalkd.beanstalk_job`` as decorator
    * [Example Beanstalk Job File](https://github.com/jonasvp/django-beanstalkd/blob/master/beanstalk_example/beanstalk_jobs.py)
  * Warning
    * [The job must accept a single string argument as passed by the caller](https://github.com/jonasvp/django-beanstalkd#registering-jobs)
    * Pass Multi Arguments
      * ``json.dumps`` & ``json.loads``

* Start Worker

  ```shell
  python manage.py beanstalk_worker -w 5
  ```

* Clients

  ```python
  from django_beanstalkd import BeanstalkClient

  client = BeanstalkClient()
  client.call('beanstalk_example.background_counting', '5')
  ```

## Start Order

- Beanstalkd ==> Worker ==> Django

## References

[1] beanstalkd@TT4IT, [beanstalkd —— Beanstalk is a simple, fast work queue.](http://tt4it.com/resources/discuss/116/)

[2] django-beanstalkd@Github, [django-beanstalkd —— A convenience wrapper for beanstalkd clients and workers in Django using the beanstalkc library for Python](https://github.com/jonasvp/django-beanstalkd)