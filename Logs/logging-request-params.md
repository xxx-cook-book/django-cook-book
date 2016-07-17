# Logging Request Params

## Installation

```shell
pip install django-logit
```

## Settings.py

```python
# logger setting
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
            'class': 'logging.FileHandler',
            'filename': '/tmp/logit.log',
            'formatter': 'verbose'
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
* Use RotatingFileHandler to supports rotation of disk log files.

  ```python
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
              'class': 'logging.handlers.RotatingFileHandler',
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

* Use TimedRotatingFileHandler to support rotation of disk log files at certain timed intervals.

  ```python
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
              'class': 'logging.handlers.TimedRotatingFileHandler',
              'filename': '/tmp/logit.log',
              'when': 'midnight',
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

## Usage

```python
from logit import logit

@logit
def xxx(request):
    xxx
```
Then the logs will be stored in logfile /tmp/logit.log

## Advantage

Using logit decorator is a shortcut for below

```python
import logging

logging.getLogger('logit')

def xxx(request):
    logger.debug(func.__name__)
    try:
        logger.debug(request.body)
    except Exception as e:
        logger.debug(e.message)
    logger.debug(request.GET)
    logger.debug(request.POST)
```
* Why try ``logger.debug(request.body)``
  * Sometimes access ``request.body`` will raise error
  * [Exception: You cannot access body after reading from request's data stream](http://stackoverflow.com/questions/19581110/exception-you-cannot-access-body-after-reading-from-requests-data-stream)

## References

[1] Brightcells@Github, [django-logit — Django Decorator of Logging Request Params](https://github.com/Brightcells/django-logit)

