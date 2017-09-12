# Redis

## Requirements
```
hiredis==0.2.0
redis==2.10.6
redis-extensions==1.1.1
```

## Settings

* func_settings.py

  ```python
  # -*- coding: utf-8 -*-

  import redis_extensions as redis

  def redis_conf(conf):
      return {
          'host': conf.get('HOST', 'localhost'),
          'port': conf.get('PORT', 6379),
          'password': '{0}:{1}'.format(conf.get('USER', ''), conf.get('PASSWORD', '')) if conf.get('USER') else '',
          'db': conf.get('db', 0),
      }

  def redis_connect(conf):
      return redis.StrictRedisExtensions(connection_pool=redis.ConnectionPool(**redis_conf(conf)))
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
[1] redisclub/redis-extensions-py@Github, [Redis-extensions is a collection of custom extensions for Redis-py.](https://github.com/redisclub/redis-extensions-py)

