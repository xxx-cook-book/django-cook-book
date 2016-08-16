# ConcurrentLogHandler

## Overview

Concurrent logging handler (drop-in replacement for RotatingFileHandler) Python 2.6+.

This module provides an additional log handler for Python’s standard logging package (PEP 282). This handler will write log events to log file which is rotated when the log file reaches a certain size. Multiple processes can safely write to the same log file concurrently.

## Installation

```shell
pip install ConcurrentLogHandler
```

## LOGGING

```python
from cloghandler import ConcurrentRotatingFileHandler

LOGGING = {
    'version': 1,
    'disable_existing_loggers': False,
    'formatters': {
        'verbose': {
            'format': '%(levelname)s %(asctime)s %(module)s %(process)d %(thread)d %(message)s'
        },
        'simple': {
            'format': '%(levelname)s %(message)s'
        },
    },
    'handlers': {
        'logit': {
            'level': 'DEBUG',
            'class': 'logging.handlers.ConcurrentRotatingFileHandler',
            'filename': '/tmp/logit.log',
            'maxBytes': 15728640,  # 1024 * 1024 * 15B = 15MB
            'backupCount': 10,
            'formatter': 'verbose',
        },
    },
    'loggers': {
        'logit': {
            'handlers': ['logit'],
            'level': 'DEBUG',
            'propagate': True,
        },
    },
}
```

## Problems

```python
Traceback (most recent call last):
  File "/usr/lib/python2.7/atexit.py", line 24, in _run_exitfuncs
    func(*targs, **kargs)
  File "/usr/lib/python2.7/logging/__init__.py", line 1638, in shutdown
    h.acquire()
  File "/home/diors/env/local/lib/python2.7/site-packages/cloghandler.py", line 204, in acquire
    lock(self.stream_lock, LOCK_EX)
  File "/home/diors/env/local/lib/python2.7/site-packages/portalocker.py", line 115, in lock
    fcntl.flock(file.fileno(), flags)
KeyboardInterrupt
```

* Connection timed out
* [Bug 952929 - Bad file descriptor from ConcurrentRotatingFileHandler.stream_lock in beakerd](https://bugzilla.redhat.com/show_bug.cgi?id=952929)

## References

[1] PyPI, [ConcurrentLogHandler — Concurrent logging handler (drop-in replacement for RotatingFileHandler) Python 2.6+](https://pypi.python.org/pypi/ConcurrentLogHandler)