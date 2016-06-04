# Password

## PASSWORD_HASHERS

* [PASSWORD_HASHERS@Settings.py](https://docs.djangoproject.com/en/dev/ref/settings/#password-hashers)

* Default

  ```python
  In [3]: from django.conf import settings

  In [4]: settings.PASSWORD_HASHERS
  Out[4]:
  ['django.contrib.auth.hashers.PBKDF2PasswordHasher',
   'django.contrib.auth.hashers.PBKDF2SHA1PasswordHasher',
   'django.contrib.auth.hashers.BCryptSHA256PasswordHasher',   
   'django.contrib.auth.hashers.BCryptPasswordHasher',
   'django.contrib.auth.hashers.SHA1PasswordHasher',
   'django.contrib.auth.hashers.MD5PasswordHasher',
   'django.contrib.auth.hashers.UnsaltedSHA1PasswordHasher',
   'django.contrib.auth.hashers.UnsaltedMD5PasswordHasher',
   'django.contrib.auth.hashers.CryptPasswordHasher']
  ```

* Support

  ```python
  PASSWORD_HASHERS = [
    'django.contrib.auth.hashers.PBKDF2PasswordHasher',
    'django.contrib.auth.hashers.PBKDF2SHA1PasswordHasher',
    'django.contrib.auth.hashers.Argon2PasswordHasher',
    'django.contrib.auth.hashers.BCryptSHA256PasswordHasher',
    'django.contrib.auth.hashers.BCryptPasswordHasher',
    'django.contrib.auth.hashers.SHA1PasswordHasher',
    'django.contrib.auth.hashers.MD5PasswordHasher',
    'django.contrib.auth.hashers.UnsaltedSHA1PasswordHasher',
    'django.contrib.auth.hashers.UnsaltedMD5PasswordHasher',
    'django.contrib.auth.hashers.CryptPasswordHasher',
  ]
  ```

* default: PBKDF2PasswordHasher

## functions

```python
from django.contrib.auth.hashers import make_password, check_password
```

* make_password

  ```python
  make_password(password, salt=None, hasher='default')
  ```

  * Example

    ```python
    In [45]: make_password('tt4it')
    Out[45]: u'pbkdf2_sha256$12000$cyP69ZOroWb5$bbDgwYwNdHyZ/qT5EKv/ozbKIVtoeuHTnZtagLPRV+E='

    In [46]: make_password('tt4it', None, 'pbkdf2_sha256')
    Out[46]: u'pbkdf2_sha256$12000$imk3IJOm6SjH$iZJVZEcM7lKYH+3qQ+WqEgoVElbgYzueiNKHYe+Dzsw='

    In [47]: make_password('tt4it', 'qwertyuiop', 'pbkdf2_sha256')
    Out[47]: u'pbkdf2_sha256$12000$qwertyuiop$It5NyYM5F0/me0q9wgcGwsy6efzyR7/OQHBQ7DNcP3Q='
    ```

  * hasher

    ```python
    The corresponding algorithm names are:
      
    pbkdf2_sha256
    pbkdf2_sha1
    argon2
    bcrypt_sha256
    bcrypt
    sha1
    md5
    unsalted_sha1
    unsalted_md5
    crypt
    ```

  * encoded = algorithm + iterations + salt + hash

    ```python
    algorithm, iterations, salt, hash = encoded.split('$', 3)
    ```

* check_password

  ```python
  check_password(password, encoded)
  ```

  * Example

    ```python
    In [50]: check_password('tt4it', u'pbkdf2_sha256$12000$qwertyuiop$It5NyYM5F0/me0q9wgcGwsy6efzyR7/OQHBQ7DNcP3Q=')
    Out[50]: True
    ```



## salt

```python
algorithm, iterations, salt, hash = encoded.split('$', 3)
```

## constant_time_compare

```python
if hasattr(hmac, "compare_digest"):
    # Prefer the stdlib implementation, when available.
    def constant_time_compare(val1, val2):
        return hmac.compare_digest(force_bytes(val1), force_bytes(val2))
else:
    def constant_time_compare(val1, val2):
        """
        Returns True if the two strings are equal, False otherwise.
        The time taken is independent of the number of characters that match.
        For the sake of simplicity, this function executes in constant time only
        when the two strings have the same length. It short-circuits when they
        have different lengths. Since Django only uses it to compare hashes of
        known expected length, this is acceptable.
        """
        if len(val1) != len(val2):
            return False
        result = 0
        if six.PY3 and isinstance(val1, bytes) and isinstance(val2, bytes):
            for x, y in zip(val1, val2):
                result |= x ^ y
        else:
            for x, y in zip(val1, val2):
                result |= ord(x) ^ ord(y)
        return result == 0
```

* [django/django/utils/crypto.py](https://github.com/django/django/blob/ecb59cc6579402b68ddfd4499bf30edacf5963be/django/utils/crypto.py)
* [Purpose of constant_time_compare?](https://groups.google.com/forum/#!topic/django-developers/iAaq0pvHXuA)
  * These are a class of attacks known as timing attacks: [http://en.wikipedia.org/wiki/Timing_attack](http://en.wikipedia.org/wiki/Timing_attack). That said I don’t know of any actual real world attacks using these, but better safe than sorry.

## References

[1] Docs@DjangoProject, [Password management in Django](https://docs.djangoproject.com/en/dev/topics/auth/passwords/)