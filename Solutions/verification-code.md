# Verification Code

## Randint

```python
# setting.py
VERIFICATION_CODE_RANGE = [100000, 999999]

# views.py
import random
from django.conf import settings

code = random.randint(*settings.VERIFICATION_CODE_RANGE)
```

## verification-code

* Installation

  ```shell
  pip install verification-code
  ```

* Usage

  ```python
  In [1]: import vcode

  In [2]: vcode.digits()
  Out[2]: '249792'

  In [3]: vcode.digits(ndigits=4)
  Out[3]: '6092'

  In [4]: vcode.digits(code_cast_func=int)
  Out[4]: 997531
  ```

## redis-extensions

* Installation

  ```shell
  pip install redis-extensions
  ```

* Usage

  ```python
  In [1]: import redis_extensions as redis

  In [2]: r = redis.StrictRedisExtensions(host='localhost', port=6379, db=0)

  In [3]: phone = '18888888888'

  In [4]: r.vcode(phone)
  Out[4]: ('678366', False, False)

  In [5]: r.vcode_exists(phone, '678366')
  Out[5]: True

  In [6]: r.vcode_delete(phone)
  Out[6]: 1
  ```

## References

[1] vcodeclub/verification-code@Github, [Verification Code](https://github.com/vcodeclub/verification-code)

[2] redisclub/redis-extensions-py@Github, [Redis-extensions is a collection of custom extensions for Redis-py.](https://github.com/redisclub/redis-extensions-py)
