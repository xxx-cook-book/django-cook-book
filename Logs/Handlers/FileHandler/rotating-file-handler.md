# RotatingFileHandler

## RotatingFileHandler

The [`RotatingFileHandler`](https://docs.python.org/2/library/logging.handlers.html#logging.handlers.RotatingFileHandler) class, located in the [`logging.handlers`](https://docs.python.org/2/library/logging.handlers.html#module-logging.handlers) module, supports rotation of disk log files.

```python
class logging.handlers.RotatingFileHandler(filename, mode='a', maxBytes=0, backupCount=0, encoding=None, delay=0)
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

## Problems

* Logs missing when host in uwsgi with multiple process

## References

[2] Docs@Python, [15.9. logging.handlers — Logging handlers — 15.9.5. RotatingFileHandler](https://docs.python.org/2/library/logging.handlers.html#rotatingfilehandler)

[2] user1157751@StackOverflow, [Django - Rotating File Handler stuck when file is equal to maxBytes](http://stackoverflow.com/questions/26682413/django-rotating-file-handler-stuck-when-file-is-equal-to-maxbytes)

[3] Blog@azalea, [Django logging with RotatingFileHandler error](http://azaleasays.com/2014/05/01/django-logging-with-rotatingfilehandler-error)

[4] Blog@xiaorui, [聊聊logging的RotatingFileHandler切割日志](http://xiaorui.cc/2016/04/01/%E8%81%8A%E8%81%8Alogging%E7%9A%84rotatingfilehandler%E5%88%87%E5%89%B2%E6%97%A5%E5%BF%97/)