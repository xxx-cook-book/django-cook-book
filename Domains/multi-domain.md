# Multi Domain

## Background

* Multi Domain such as ``domain-one.dev`` and ``domain-two.dev`` both resolution to a project

## Solution One

* Nginx rewrite

  ```nginx
  rewrite ^/((?!coin|api).*)/ /coin/$1/ last;
  ```

## Solution Two

* django-multidomain

  * Theory

    ```
    Django determines the root URLconf module to use. Ordinarily, this is the value of the ROOT_URLCONF setting, but if the incoming HttpRequest object has an attribute called urlconf (set by middleware request processing), its value will be used in place of the ROOT_URLCONF setting.
    ```

  * Source Code of Django

    * [django/django/core/urlresolvers.py](https://github.com/django/django/blob/37ea3cb03e80de80380009a7a7939bc48d75abe9/django/core/urlresolvers.py)

  * Django Multi Domain

    ```python
    import re
    from django.conf import settings

    class DomainMiddleware(object):
      def process_request(self, request):
          url_config = getattr(settings, 'URL_CONFIG', None)
          if url_config:
              try:
                  host = request.get_host()
                  for (regex, value) in url_config:
                      if re.search(regex, host):
                          request.urlconf = value
                          break
              except Exception, e:
                  print str(e)
                  pass
    ```

  * Installation

    ```shell
    pip install django-multidomain
    ```

  * Add ``multidomain`` to ``INSTALLED_APPS``

    ```python
    INSTALLED_APPS += ('multidomain', )
    ```

  * Add ``multidomain.middleware.DomainMiddleware`` to ``MIDDLEWARE_CLASSES``

    ```python
    MIDDLEWARE_CLASSES += ('multidomain.middleware.DomainMiddleware', )
    ```

  * Create App One and Two

    ```shell
    python manage.py startapp one
    python manage.py startapp two
    ```

  * Create a file for each domain you have (For example: ``domain-one.dev`` and ``domain-two.dev``)

    * urls.py (by default)

      ```python
      from django.conf.urls import patterns, include, url
      from django.contrib import admin

      urlpatterns = patterns('',
          url(r'^admin/', include(admin.site.urls)),
      )
      ```

    * urls_one.py

      ```python
      from django.conf.urls import patterns, include, url

      urlpatterns = patterns('',
          url(r'^', include('one.urls', namespace='one')),
      )
      ```

    * urls_two.py

      ```python
      from django.conf.urls import patterns, include, url

      urlpatterns = patterns('',
          url(r'^', include('two.urls', namespace='two')),
      )
      ```

  * Declare host/domain urlconfig tuple ``URL_CONFIG``

    ```python
    URL_CONFIG = (
        (r'^(.+\.)?domain-one\.dev', 'multidomaindemo.urls_one'),
        (r'^(.+\.)?domain-two\.dev', 'multidomaindemo.urls_two'),
    )
    ROOT_URLCONF = 'multidomaindemo.urls'
    ```

  * Local Test

    * localhost vs 127.0.0.1

      ```python
      URL_CONFIG = (
          (r'^(.+\.)?localhost', 'multidomaindemo.urls_one'),
          (r'^(.+\.)?127\.0\.0\.1', 'multidomaindemo.urls_two'),
      )
      ```

## Problems

* django-multidomain doesn't work when Nginx use proxy_pass

  ```nginx
  # the upstream component nginx needs to connect to
  upstream kuaihongbao {
      server unix:///home/uwsgi/kuaihongbao.sock; # for a file socket
      # server 127.0.0.1:8888; # for a web port socket (we'll use this first)
  }

  location / {
      uwsgi_pass kuaihongbao;
      # proxy_pass  http://kuaihongbao;
      include     /home/uwsgi/uwsgi_params; # the uwsgi_params file you installed
  }
  ```

  * Reason:  value of ``host = request.get_host()`` became ``127.0.0.1:8888``

## References

[1] cyacarinic@Github, [Django Multi Domain — Django application, implement multi domain concept. You can choose your url config according to your domain host.](https://github.com/cyacarinic/django-multidomain)