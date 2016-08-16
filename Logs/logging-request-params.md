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

def loggerit(func, flag, content):
    logger.debug('func=%s&flag=%s&content=%s', func, flag, content)

def xxx(request):
    # Get Method Name
    name = xxx.__name__
    # Log body content into file
    try:
        loggerit(name, 'body', request.body)
    except Exception as e:
        loggerit(name, 'error', e.message)
    # Log get content into file
    loggerit(name, 'get', request.GET)
    # Log post content into file
    loggerit(name, 'post', request.POST)
    
    # Some Codes
    res = '``string`` or ``stringify json``'
    
    # Log response content into file
    try:
        loggerit(name, 'res', res)
    except Exception as e:
        loggerit(name, 'error', e.message)
```
* Why try ``logger.debug(request.body)``
  * Sometimes access ``request.body`` will raise error
  * [Exception: You cannot access body after reading from request's data stream](http://stackoverflow.com/questions/19581110/exception-you-cannot-access-body-after-reading-from-requests-data-stream)

## References

[1] django-xxx@Github, [django-logit — Django Decorator of Logging Request Params](https://github.com/django-xxx/django-logit)

