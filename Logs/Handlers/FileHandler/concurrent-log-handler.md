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

## References

[1] PyPI, [ConcurrentLogHandler — Concurrent logging handler (drop-in replacement for RotatingFileHandler) Python 2.6+](https://pypi.python.org/pypi/ConcurrentLogHandler)