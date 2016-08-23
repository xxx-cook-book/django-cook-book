# Beanstalkd

## beanstalkd

* Mac OS X

  * Installation

    ```shell
    brew install beanstalkd
    ```

  * Version

    ```shell
    $ beanstalkd -v
    beanstalkd 1.10
    ```

  * Script

    * [OSX script for easy start & stop beanstalkd (brew version)](https://gist.github.com/finger-berlin/1942295)

* Ubuntu

  * Installation Using Aptitude

    * Installation

      ```shell
      aptitude install -y beanstalkd
      ```

    * Version

      ```shell
      $ beanstalkd -v
      beanstalkd 1.4.6
      $ /usr/bin/beanstalkd -v
      beanstalkd 1.4.6
      ```

    * *Tips: Version 1.4.6 had an issue with delayed jobs*

  * Installation From Source

    * Installation

      ```shell
      git clone https://github.com/kr/beanstalkd
      cd beanstalkd
      make
      make install
      # install -d /usr/local/bin/
      # install beanstalkd /usr/local/bin/beanstalkd
      ```

    * Version

      ```shell
      $ /usr/local/bin/beanstalkd -v
      beanstalkd 1.10+21+gb7b4a6a
      ```

    * Tips: If you want to use ``service beanstalkd xxx``, you should first ``Installation Using Aptitude``, then ``Installation From Source``

    * ``/etc/init.d/beanstalkd``

      ```shell
      # Default is ``/usr/bin/beanstalkd``
      # You can change it to use other server's location
      # DAEMON=/usr/bin/beanstalkd # Introduce the server's location here
      DAEMON=/usr/local/bin/beanstalkd # Introduce the server's location here
      ```

  * Start

    ```shll
    # To start the service:
    service beanstalkd start
    ```
    * Error

      ```shell
      $ service beanstalkd start
       * Starting in-memory queueing server  beanstalkd
      beanstalkd not configured to start, please edit /etc/default/beanstalkd to enable
      ```

    * /etc/default/beanstalkd

      ```shell
      ## Defaults for the beanstalkd init script, /etc/init.d/beanstalkd on
      ## Debian systems. Append ``-b /var/lib/beanstalkd'' for persistent
      ## storage.
      BEANSTALKD_LISTEN_ADDR=0.0.0.0
      BEANSTALKD_LISTEN_PORT=11300
      DAEMON_OPTS="-l $BEANSTALKD_LISTEN_ADDR -p $BEANSTALKD_LISTEN_PORT"
      ## Uncomment to enable startup during boot.
      START=yes
      ```

    * Start

      ```shell
      $ service beanstalkd start
       * Starting in-memory queueing server  beanstalkd                                                     start-stop-daemon: unable to open pidfile '/var/run/beanstalkd.pid' for writing (Permission denied)
                                                                                                     [fail]
      $ sudo service beanstalkd start
       * Starting in-memory queueing server  beanstalkd                                              [ OK ]
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
  * ``One Server`` vs. ``Multi Server``
    * If ``Multi Server`` may cause ``Multi Server get Same Job``, lead to ``CommandFailed``

* Clients

  ```python
  from django_beanstalkd import BeanstalkClient

  client = BeanstalkClient()
  client.call('beanstalk_example.background_counting', '5')
  ```

## Start Order

- Beanstalkd ==> Worker ==> Django

## Delayed Job

```python
client.call('beanstalk_example.background_counting', '5', delay=120, ttr=TTR)
```

* ``def call(self, func, arg='', priority=DEFAULT_PRIORITY, delay=0, ttr=DEFAULT_TTR):``
  * priority: an integer number that specifies the priority. Jobs with a smaller priority get executed first

  * delay: how many seconds to wait before the job can be reserved

  * ttr: how many seconds a worker has to process the job before it gets requeued

  * DEFAULT_TTR

    ```python
    In [1]: from beanstalkc import DEFAULT_TTR
    In [2]: DEFAULT_TTR
    Out[2]: 120
    ```

## Problems

* Delayed Job Not Execute

  * [Beanstalkd “delayed job” doesn't execute](http://stackoverflow.com/questions/26547091/beanstalkd-delayed-job-doesnt-execute)

  * [Beanstalkd Queue - Delayed jobs not getting processed](http://laravel.io/forum/02-23-2014-beanstalkd-queue-delayed-jobs-not-getting-processed)


* SocketError

  * SocketError

    ```python
    File "/home/diors/env/src/django-beanstalkd-master/django_beanstalkd/__init__.py", line 42, in call
        self._beanstalk.use(func)
      File "/home/diors/env/local/lib/python2.7/site-packages/beanstalkc.py", line 190, in use
        return self._interact_value('use %s\r\n' % name, ['USING'])
      File "/home/diors/env/local/lib/python2.7/site-packages/beanstalkc.py", line 110, in _interact_value
        return self._interact(command, expected_ok, expected_err)[0]
      File "/home/diors/env/local/lib/python2.7/site-packages/beanstalkc.py", line 87, in _interact
        status, results = self._read_response()
      File "/home/diors/env/local/lib/python2.7/site-packages/beanstalkc.py", line 98, in _read_response
        raise SocketError()
    SocketError
    ```

  * SocketError: [Errno 104] Connection reset by peer

    ```python
        File "/home/diors/env/src/django-beanstalkd-master/django_beanstalkd/__init__.py", line 42, in call
        self._beanstalk.use(func)
      File "/home/diors/env/local/lib/python2.7/site-packages/beanstalkc.py", line 190, in use
        return self._interact_value('use %s\r\n' % name, ['USING'])
      File "/home/diors/env/local/lib/python2.7/site-packages/beanstalkc.py", line 110, in _interact_value
        return self._interact(command, expected_ok, expected_err)[0]
      File "/home/diors/env/local/lib/python2.7/site-packages/beanstalkc.py", line 87, in _interact
        status, results = self._read_response()
      File "/home/diors/env/local/lib/python2.7/site-packages/beanstalkc.py", line 96, in _read_response
        line = SocketError.wrap(self._socket_file.readline)
      File "/home/diors/env/local/lib/python2.7/site-packages/beanstalkc.py", line 43, in wrap
        raise SocketError(err)
    SocketError: [Errno 104] Connection reset by peer
    ```


* CommandFailed

  ```python
    File "/home/diors/env/local/lib/python2.7/site-packages/beanstalkc.py", line 91, in _interact
      raise CommandFailed(command.split()[0], status, results)
  beanstalkc.CommandFailed: ('delete', 'NOT_FOUND', [])
  ```

  * [Beanstalkd (via pheanstalk) allowing duplicate, simultaneous reserves?](http://stackoverflow.com/questions/15468850/beanstalkd-via-pheanstalk-allowing-duplicate-simultaneous-reserves)
  * [Cannot delete a job #255](https://github.com/kr/beanstalkd/issues/255)

## References

[1] beanstalkd@TT4IT, [beanstalkd — Beanstalk is a simple, fast work queue](http://tt4it.com/resources/discuss/116/)

[2] beanstalkc@Github, [beanstalkc — A simple beanstalkd client library for Python](https://github.com/earl/beanstalkc)

[3] django-beanstalkd@Github, [django-beanstalkd — A convenience wrapper for beanstalkd clients and workers in Django using the beanstalkc library for Python](https://github.com/jonasvp/django-beanstalkd)

[4] O.S. Tezer@DigitalOcean Community, [How To Install and Use Beanstalkd Work Queue on a VPS](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-beanstalkd-work-queue-on-a-vps)
