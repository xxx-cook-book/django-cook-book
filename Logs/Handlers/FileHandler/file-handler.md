# FileHandler

## FileHandler

The [`FileHandler`](https://docs.python.org/2/library/logging.handlers.html#logging.FileHandler) class, located in the core [`logging`](https://docs.python.org/2/library/logging.html#module-logging) package, sends logging output to a disk file. It inherits the output functionality from[`StreamHandler`](https://docs.python.org/2/library/logging.handlers.html#logging.StreamHandler).

```python
class logging.FileHandler(filename, mode='a', encoding=None, delay=False)
```

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
            'class': 'logging.FileHandler',
            'filename': '/tmp/logit.log',
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

[1] Docs@Python, [15.9. logging.handlers — Logging handlers — 15.9.2. FileHandler](https://docs.python.org/2/library/logging.handlers.html#filehandler)