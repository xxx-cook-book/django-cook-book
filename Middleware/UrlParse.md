# Url Parse

```python
  class UrlParseMiddleware(object):

    def process_request(self, request):
        parts = urlparse(request.build_absolute_uri())
        parts = parts._replace(path='', params='', query='', fragment='')
        request.origin = parts.geturl()
        request.protocol = 'https' if request.is_secure() else 'http'
        return None
```
  ​

  ​