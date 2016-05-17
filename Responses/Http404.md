# Http404

Based on the [HTTP standards](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html#sec10.4.5), the 404 error is supposed to be for Page Not Found, not a generic error page

## Settings.py

```python
DEBUG = False
ALLOWED_HOSTS = ['*']  # Don't Use ['*'] For Production
# ALLOWED_HOSTS = ['localhost', '127.0.0.1']
```

## 404.html

You can create an HTML template named ``404.html`` and place it in the top level of your template tree

* Parameters
  * request_path
  * exception
  * request
  * request.xxx
  * Http404 parameter
    * How?
    * [Displaying Http404 parameter in 404.html template](http://stackoverflow.com/questions/7206147/displaying-http404-parameter-in-404-html-template)

## Usage

```python
from django.http import Http404
def xxx(request):
    # Some Code
    raise Http404('xxx')
```

## References

[1] Docs@DjangoProject, [The Http404 exception](https://docs.djangoproject.com/en/dev/topics/http/views/#the-http404-exception)

[2] Docs@DjangoProject, [The 404 (page not found) view](https://docs.djangoproject.com/en/dev/ref/views/#the-404-page-not-found-view)