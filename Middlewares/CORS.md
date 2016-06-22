# Cross-Origin Resource Sharing (CORS)

## django-cors-headers

* Installation

  ```shell
  pip install django-cors-headers
  ```


* Settings.py

  ```python
  INSTALLED_APPS = (
      ...
      'corsheaders',
      ...
  )

  MIDDLEWARE_CLASSES = (
      ...
      'corsheaders.middleware.CorsMiddleware',
      'django.middleware.common.CommonMiddleware',
      ...
  )
  ```

  * Note that `CorsMiddleware` needs to come before Django's `CommonMiddleware` if you are using Django's `USE_ETAGS = True` setting, otherwise the CORS headers will be lost from the 304 not-modified responses, causing errors in some browsers.

* Usage

  * Add hosts that are allowed to do cross-site requests to `CORS_ORIGIN_WHITELIST` or set `CORS_ORIGIN_ALLOW_ALL` to `True`to allow all hosts.
  * CORS_ORIGIN_WHITELIST: specify a list of origin hostnames that are authorized to make a cross-site HTTP request.

## DIY

* middleware.py

  ```python
  class AccessControlAllowOriginMiddleware(object):
      def process_response(self, request, response):
          response['Access-Control-Allow-Origin'] = '*'
          return response
  ```

* settings.py

  ```python
  MIDDLEWARE_CLASSES += ('xxx.middleware.AccessControlAllowOriginMiddleware', )
  ```

## References

[1] ottoyiu@Github, [django-cors-headers — Django app for handling the server headers required for Cross-Origin Resource Sharing (CORS)](https://github.com/ottoyiu/django-cors-headers)
