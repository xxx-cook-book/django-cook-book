# TimedRotatingFileHandler

## TimedRotatingFileHandler

The [`TimedRotatingFileHandler`](https://docs.python.org/2/library/logging.handlers.html#logging.handlers.TimedRotatingFileHandler) class, located in the [`logging.handlers`](https://docs.python.org/2/library/logging.handlers.html#module-logging.handlers) module, supports rotation of disk log files at certain timed intervals.

```python
class logging.handlers.TimedRotatingFileHandler(filename, when='h', interval=1, backupCount=0, encoding=None, delay=False, utc=False)
```

## When

You can use the *when* to specify the type of *interval*. The list of possible values is below. Note that they are not case sensitive.

| Value        | Type of interval      |
| ------------ | --------------------- |
| `'S'`        | Seconds               |
| `'M'`        | Minutes               |
| `'H'`        | Hours                 |
| `'D'`        | Days                  |
| `'W0'-'W6'`  | Weekday (0=Monday)    |
| `'midnight'` | Roll over at midnight |

When using weekday-based rotation, specify ‘W0’ for Monday, ‘W1’ for Tuesday, and so on up to ‘W6’ for Sunday. In this case, the value passed for *interval* isn’t used.

When computing the next rollover time for the first time (when the handler is created), the last modification time of an existing log file, or else the current time, is used to compute when the next rotation will occur.

**PS:** ``the next rollover time`` = (``last modification time`` or ``the current time``) + when * interval

## LOGGING

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

## Problems

* Logs missing when host in uwsgi with multiple process

## References

[1] Docs@Python, [15.9. logging.handlers — Logging handlers — 15.9.6. TimedRotatingFileHandler](https://docs.python.org/2/library/logging.handlers.html#timedrotatingfilehandler)
