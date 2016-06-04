# Session

## Enabling Sessions

* INSTALLED_APPS

  ```python
  INSTALLED_APPS = [
      ...
      'django.contrib.sessions',
      ...
  ]
  ```

* MIDDLEWARE_CLASSES

  ```python
  MIDDLEWARE_CLASSES = [
      ...
      'django.contrib.sessions.middleware.SessionMiddleware',
      ...
  ]
  ```

## Usage

* Get

  ```python
  def xxx(request):
      request.session.get('key', 'default_value')
  ```

* Set

  ```python
  def xxx(request):
      request.session['key'] = 'value'
  ```

* Delete

  ```python
  def xxx(request):
      try:
          del request.session['key']
      except KeyError:
          pass
  ```

* Exists

  ```python
  def xxx(request):
      if 'key' in request.session:
          xxx
  ```

* Clear Session Store

  ```shell
  # Can be run as a cron job or directly to clean out expired sessions.
  python manage.py clearsessions
  ```

## Session Engine

```python
# The module to store session data
SESSION_ENGINE = 'django.contrib.sessions.backends.db'
```

* Store in Table ``django_session``

  ```mysql
  mysql> select * from django_session order by expire_date desc limit 1\G;
  *************************** 1. row ***************************
   session_key: s5i6dgry7gcaprja6geltrkx24yqo5wq
  session_data: NzE3NDA5MmFlMDE4MjY5MGU0Yzc5M2UyODVmZmU0ZWVlM2E4ODk4Njp7ImFjY291bnRfaWQiOiJ2RmVmV1JyZVFINUZTellkUm1Rc21nIn0=
   expire_date: 2016-06-17 05:38:31.344553
  ```

## Session Cookie Age

```python
# Age of cookie, in seconds (default: 2 weeks).
SESSION_COOKIE_AGE = 60 * 60 * 24 * 7 * 2
```

* Chrome 浏览器中 Session 所在位置

  ![](http://ww1.sinaimg.cn/large/c05783a7gw1f4j2336b5dj21i80deq6r.jpg)

## Session Cookie HTTPOnly

```python
# Whether to use the non-RFC standard httpOnly flag (IE, FF3+, others)
SESSION_COOKIE_HTTPONLY = True
```

* The HTTPOnly cookie attribute can help to mitigate this attack by preventing access to cookie value through Javascript. Read more about [Cookies and Security](http://www.nczonline.net/blog/2009/05/12/cookies-and-security/).

* SESSION_COOKIE_HTTPONLY = True

  ```javascript
  > document.cookie
  < ""
  ```

* SESSION_COOKIE_HTTPONLY = False

  ```javascript
  > document.cookie
  < "sessionid=bsq81sh73ttp1bag3perofkyvsmrh29j"
  ```

## Auth

```python
from django.contrib.auth import authenticate, login, logout
```

* To log a user in, from a view, use [`login()`](https://docs.djangoproject.com/en/1.9/topics/auth/default/#django.contrib.auth.login). It takes an [`HttpRequest`](https://docs.djangoproject.com/en/1.9/ref/request-response/#django.http.HttpRequest) object and a [`User`](https://docs.djangoproject.com/en/1.9/ref/contrib/auth/#django.contrib.auth.models.User) object. [`login()`](https://docs.djangoproject.com/en/1.9/topics/auth/default/#django.contrib.auth.login) saves the user’s ID in the session, using Django’s session framework.
* When you call [`logout()`](https://docs.djangoproject.com/en/1.9/topics/auth/default/#django.contrib.auth.logout), the session data for the current request is completely cleaned out. 

## Global Settings

```python
############
# SESSIONS #
############

# Cache to store session data if using the cache session backend.
SESSION_CACHE_ALIAS = 'default'
# Cookie name. This can be whatever you want.
SESSION_COOKIE_NAME = 'sessionid'
# Age of cookie, in seconds (default: 2 weeks).
SESSION_COOKIE_AGE = 60 * 60 * 24 * 7 * 2
# A string like ".example.com", or None for standard domain cookie.
SESSION_COOKIE_DOMAIN = None
# Whether the session cookie should be secure (https:// only).
SESSION_COOKIE_SECURE = False
# The path of the session cookie.
SESSION_COOKIE_PATH = '/'
# Whether to use the non-RFC standard httpOnly flag (IE, FF3+, others)
SESSION_COOKIE_HTTPONLY = True
# Whether to save the session data on every request.
SESSION_SAVE_EVERY_REQUEST = False
# Whether a user's session cookie expires when the Web browser is closed.
SESSION_EXPIRE_AT_BROWSER_CLOSE = False
# The module to store session data
SESSION_ENGINE = 'django.contrib.sessions.backends.db'
# Directory to store session files if using the file session module. If None,
# the backend will use a sensible default.
SESSION_FILE_PATH = None
# class to serialize session data
SESSION_SERIALIZER = 'django.contrib.sessions.serializers.JSONSerializer'
```

## References

[1] Docs@DjangoProject, [How to use sessions](https://docs.djangoproject.com/en/dev/topics/http/sessions/)

[2] Django@Github, [Django Global Settings](https://github.com/django/django/blob/354acd04af524ad82002b903df1189581c51cabe/django/conf/global_settings.py#L446)

[3] MDN, [Document.cookie](https://developer.mozilla.org/en-US/docs/Web/API/Document/cookie)