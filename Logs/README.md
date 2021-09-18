# Logs

* [Logging Request Params](logging-request-params.md)


## LOGGING

* [Django logging settings](https://gist.github.com/st4lk/6725777)
* [Logging to rotating files](https://djangosnippets.org/snippets/2980/)


## Multiple Processes

Although logging is thread-safe, and logging to a single file from multiple threads in a single process *is* supported, logging to a single file from *multiple processes* is *not* supported, because there is no standard way to serialize access to a single file across multiple processes in Python. If you need to log to a single file from multiple processes, the best way of doing this is to have all the processes log to a [SocketHandler](https://docs.python.org/2.6/library/logging.html#logging.SocketHandler), and have a separate process which implements a socket server which reads from the socket and logs to file. (If you prefer, you can dedicate one thread in one of the existing processes to perform this function.) The following section documents this approach in more detail and includes a working socket receiver which can be used as a starting point for you to adapt in your own applications.

* See [Logging to a single file from multiple processes](https://docs.python.org/2/howto/logging-cookbook.html#logging-to-a-single-file-from-multiple-processes)
* Logs missing when host in uwsgi with multiple process
  * [Django rotated log files data is lost](http://stackoverflow.com/questions/32896314/django-rotated-log-files-data-is-lost)
  * [TimedRotatingFileHandler doesn't work fine in Django with multi-instance](http://stackoverflow.com/questions/18840785/timedrotatingfilehandler-doesnt-work-fine-in-django-with-multi-instance)
  * [Some django's logs are missing when host in uwsgi with multiple process](http://stackoverflow.com/questions/9206802/some-djangos-logs-are-missing-when-host-in-uwsgi-with-multiple-process)
  * [Python Logging from Multiple Processes](http://thinlight.org/2011/08/10/python-logging-from-multiple-processes/)