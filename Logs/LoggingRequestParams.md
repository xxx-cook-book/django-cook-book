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
    logger.debug(request.GET)
    logger.debug(request.POST)
```
## References

[1] Brightcells@Github, [django-logit —— Django Decorator of Logging Request Params](https://github.com/Brightcells/django-logit)

