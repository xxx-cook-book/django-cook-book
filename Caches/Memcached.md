# Memcached

## Settings

* Sole

  ```python
  CACHES = {
      'default': {
          'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
          'LOCATION': '127.0.0.1:11211',
      }
  }
  ```


* Multi

  ```python
  CACHES = {
      'default': {
          'BACKEND': 'django.core.cache.backends.memcached.MemcachedCache',
          'LOCATION': [
              '172.19.26.240:11211',
              '172.19.26.242:11211',
          ]
      }
  }
  ```

##  BACKEND

* MemcachedCache

  * Install Binding

    ```shell
    pip install python-memcached
    ```


* PyLibMCCache

  * Install Lib

    ```shell
    sudo apt-get install libmemcached-dev  # Ubuntu
    sudo brew install libmemcached         # Mac OS X
    ```

  * Install Bingding

    ```shell
    pip install pylibmc
    ```

## Usage

* Basic

  ```python
  >>> from django.core.cache import cache

  >>> cache.set('my_key', 'hello, world!', 30)
  >>> cache.get('my_key')
  'hello, world!'
  ```

## Problems

* Using python-memcached happens multi server's data outer-sync
  * [How does django handle multiple memcached servers?](http://stackoverflow.com/questions/6876250/how-does-django-handle-multiple-memcached-servers)
  * Solved by changing to use `django.core.cache.backends.memcached.PyLibMCCache` (pylibmc)
* When ``List`` is too large, set/get is too slow
  * Use ``String`` replace of ``List``, ``json.dumps()`` before set, ``json.loads()`` after get

## References

[1] Docs@DjangoProject, [Django’s cache framework](https://docs.djangoproject.com/en/1.9/topics/cache/)

[2] python-memcached@Github, [python-memcached — A python memcached client library](https://github.com/linsomniac/python-memcached)

[3] pylibmc@Github, [pylibmc — A Python wrapper around the libmemcached interface from TangentOrg](https://github.com/lericson/pylibmc)