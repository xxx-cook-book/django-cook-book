# Redis

## Settings

* func_settings.py

  ```python
  # -*- coding: utf-8 -*-

  import redis

  def redis_conf(conf):
      return {
          'host': conf.get('HOST', 'localhost'),
          'port': conf.get('PORT', 6379),
          'password': '{}:{}'.format(conf.get('USER', ''), conf.get('PASSWORD', '')) if conf.get('USER') else '',
          'db': conf.get('db', 0),
      }

  def redis_connect(conf):
      return redis.StrictRedis(connection_pool=redis.ConnectionPool(**redis_conf(conf)))
  ```

* settings.py

  ```python
  # -*- coding: utf-8 -*-

  # Redis Settings
  REDIS = {
      'default': {
          'HOST': '127.0.0.1',
          'PORT': 6379,
          'USER': '',
          'PASSWORD': '',
          'db': 0,
      }
  }

  # Redis Expired Time Settings
  REDIS_EXPIRED_HALF_HOUR = 1800  # 0.5 * 60 * 60
  REDIS_EXPIRED_HOUR = 3600  # 60 * 60
  REDIS_EXPIRED_DAY = 86400  # 24 * 60 * 60
  REDIS_EXPIRED_WEEK = 604800  # 7 * 24 * 60 * 60
  REDIS_EXPIRED_MONTH = 2678400  # 31 * 24 * 60 * 60
  REDIS_EXPIRED_YEAR = 31622400  # 366 * 24 * 60 * 60

  try:
      from local_settings import *
  except ImportError:
      pass

  try:
      from func_settings import redis_connect
      REDIS_CACHE = redis_connect(REDIS.get('default', {}))
  except ImportError:
      REDIS_CACHE = None
  ```

## Import

```python
from django.conf import settings

r = settings.REDIS_CACHE

# r.set('foo', 'bar')
```

* Advanced Encapsulation

  * utils/redis/connect.py

    ```python
    # -*- coding: utf-8 -*-

    from django.conf import settings

    r = settings.REDIS_CACHE
    ```

  * Usage

    ```python
    from utils.redis.connect import r
    ```

## Usage

* [Redis Cook Book](https://xxx-cook-book.gitbooks.io/redis-cook-book/content/Python/redis-py/)

## Problems

* If conf not have db, will raise ``KeyError: 'db'``
  * [fix bug: KeyError for description_format #768](https://github.com/andymccurdy/redis-py/pull/768)

## References

