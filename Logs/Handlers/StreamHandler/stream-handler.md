# StreamHandler

## StreamHandler

The [`StreamHandler`](https://docs.python.org/2/library/logging.handlers.html#logging.StreamHandler) class, located in the core [`logging`](https://docs.python.org/2/library/logging.html#module-logging) package, sends logging output to streams such as *sys.stdout*, *sys.stderr* or any file-like object (or, more precisely, any object which supports `write()` and `flush()` methods).

```python
class logging.StreamHandler(stream=None)
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
            'class': 'logging.StreamHandler',
            # 'stream': sys.stdout,
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

[1] Docs@Python, [15.9. logging.handlers — Logging handlers — 15.9.1. StreamHandler](https://docs.python.org/2/library/logging.handlers.html#streamhandler)

