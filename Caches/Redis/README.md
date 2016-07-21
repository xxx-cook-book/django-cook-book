# Redis

## Settings

* func_settings.py

  ```python
  # -*- coding: utf-8 -*-

  import redis

  def redis_connect(conf):
      return redis.StrictRedis(connection_pool=redis.ConnectionPool(**{
          'host': conf.get('HOST', 'localhost'),
          'port': conf.get('PORT', 6379),
          'password': '{}:{}'.format(conf.get('USER', ''), conf.get('PASSWORD', '')) if conf.get('USER') else '',
          'db': conf.get('db', 0),
      }))
  ```

* settings.py

  ```python
  # -*- coding: utf-8 -*-

  # Redis 设置
  REDIS = {
      'default': {
          'HOST': '127.0.0.1',
          'PORT': 6379,
          'USER': '',
          'PASSWORD': '',
          'db': 0,
      }
  }

  # Redis 缓存时间设置
  REDIS_EXPIRED_HALF_HOUR = 1800  # 0.5 * 60 * 60
  REDIS_EXPIRED_HOUR = 3600  # 60 * 60
  REDIS_EXPIRED_DAY = 86400  # 24 * 60 * 60
  REDIS_EXPIRED_WEEK = 604800  # 7 * 24 * 60 * 60
  REDIS_EXPIRED_MONTH = 2678400  # 31 * 24 * 60 * 60
  REDIS_EXPIRED_YEAR = 31622400  # 366 * 24 * 60 * 60

  # End of settings.py
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

## Usage

* [Redis Cook Book](https://xxx-cook-book.gitbooks.io/redis-cook-book/content/Python/redis-py/)

## References

