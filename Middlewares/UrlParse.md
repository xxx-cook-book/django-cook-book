# URL Parse

## [Components of an URI](https://medialize.github.io/URI.js/about-uris.html)

![](http://ww4.sinaimg.cn/large/c05783a7gw1f2h380qt9jj21500i8q5o.jpg)

## Django Request

* uri

  ```python
  request.build_absolute_uri()
  ```

  * [Request and response objects —— django.http.HttpRequest.build_absolute_uri](https://docs.djangoproject.com/en/1.9/ref/request-response/#django.http.HttpRequest.build_absolute_uri)

* origin

  * Method One

    ```python
    origin = '{scheme}://{host}'.format(
          scheme='https' if request.is_secure() else 'http',
          host=request.get_host()
    )
    ```

  * Method Two

    ```python
    parts = urlparse(request.build_absolute_uri())
    parts = parts._replace(path='', params='', query='', fragment='')
    # origin = urlunparse(parts)
    origin = parts.geturl()
    ```

  * Method Three, Wrapper As A Middleware

    ```python
    class UrlParseMiddleware(object):

    	def process_request(self, request):
            parts = urlparse(request.build_absolute_uri())
            parts = parts._replace(path='', params='', query='', fragment='')
            request.origin = parts.geturl()
            return None
    ```

* protocol/scheme

  * Method One

    ```python
    protocol = 'https' if request.is_secure() else 'http'
    ```

    * [How to use Django to get the name for the host server?](http://stackoverflow.com/questions/4093999/how-to-use-django-to-get-the-name-for-the-host-server)

  * Method Two

    ```python
    request.scheme
    ```

    * _New In Django 1.7_
    * [Request and response objects —— django.http.HttpRequest.scheme](https://docs.djangoproject.com/en/1.7/ref/request-response/#django.http.HttpRequest.scheme)

* host

  ```python
  request.get_host()
  ```

  * [Request and response objects —— django.http.HttpRequest.get_host](https://docs.djangoproject.com/en/1.9/ref/request-response/#django.http.HttpRequest.get_host)

* port

  ```python
  request.get_port()
  ```

  * _New In Django 1.9_
  * [Request and response objects —— django.http.HttpRequest.get_port](https://docs.djangoproject.com/en/1.9/ref/request-response/#django.http.HttpRequest.get_port)

* path

  ```python
  request.get_full_path()
  ```

  * [Request and response objects —— django.http.HttpRequest.get_full_path](https://docs.djangoproject.com/en/1.9/ref/request-response/#django.http.HttpRequest.get_full_path)

## django-uri

* Installation

  ```shell
  pip install django-uri
  ```

* Settings.py

  ```python
  MIDDLEWARE_CLASSES = (
      ...
      'uri.middleware.URIMiddleware',
      ...
  )
  ```

* Usage

  ```python
  request.uri.origin
  request.uri.scheme
  ```

## References

[1] Brightcells@Github, [django-uri — Django URI](https://github.com/Brightcells/django-uri)