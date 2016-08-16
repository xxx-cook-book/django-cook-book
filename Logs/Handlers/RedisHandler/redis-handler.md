# RedisHandler

## rlog

* Installation

  ```shell
  pip install rlog
  ```

* Usage

  ```python
  from rlog import RedisHandler

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
              'class': 'rlog.RedisHandler',
              'redis_client': REDIS_CACHE,
              'channel': 'django-logit',
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

## django-rlog

* Redis Config in Django

  * See Caches/Redis/

  * Settings.py

    ```python
    try:
        from func_settings import redis_connect
        REDIS_CACHE = redis_connect(REDIS.get('default', {}))
        DJLOGIT = {
            'level': 'DEBUG',
            'class': 'rlog.RedisHandler',
            'redis_client': REDIS_CACHE,
            'channel': 'django-logit',
            'formatter': 'verbose',
        }
    except ImportError:
        REDIS_CACHE = None
        DJLOGIT = {
            'level': 'DEBUG',
            'class': 'logging.FileHandler',
            'filename': '/tmp/logit.log',
            'formatter': 'verbose',
        }
        
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
            'logit': DJLOGIT,
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

[1] lobziik@Github, [rlog — Small handler and formatter for using python logging with Redis](https://github.com/lobziik/rlog)

[2] django-xxx@Github, [django-rlog — Save rlog's Pub/Sub Log to Disk](https://github.com/django-xxx/django-rlog)



