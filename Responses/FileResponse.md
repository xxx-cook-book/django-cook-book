# FileResponse

## Since Django 1.7

_New In Django 1.7_

[`FileResponse`](https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.FileResponse) is a subclass of [`StreamingHttpResponse`](https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.StreamingHttpResponse) optimized for binary files. It uses [wsgi.file_wrapper](https://www.python.org/dev/peps/pep-3333/#optional-platform-specific-file-handling) if provided by the wsgi server, otherwise it streams the file out in small chunks.

## Source code

```python
class FileResponse(StreamingHttpResponse):
    """
    A streaming HTTP response class optimized for files.
    """
    block_size = 4096

    def _set_streaming_content(self, value):
        if hasattr(value, 'read'):
            self.file_to_stream = value
            filelike = value
            if hasattr(filelike, 'close'):
                self._closable_objects.append(filelike)
            value = iter(lambda: filelike.read(self.block_size), b'')
        else:
            self.file_to_stream = None
        super(FileResponse, self)._set_streaming_content(value)
```
## file_to_stream

It uses [wsgi.file_wrapper](https://www.python.org/dev/peps/pep-3333/#optional-platform-specific-file-handling) if provided by the wsgi server, otherwise it streams the file out in small chunks.

See [django/core/handlers/wsgi.py](https://github.com/django/django/blob/master/django/core/handlers/wsgi.py#L179)

```python
if getattr(response, 'file_to_stream', None) is not None and environ.get('wsgi.file_wrapper'):
            response = environ['wsgi.file_wrapper'](response.file_to_stream)
        return response
```
## Before Django 1.7

## References
[1] Docs@DjangoProject, [FileResponse objects](https://docs.djangoproject.com/en/dev/ref/request-response/#fileresponse-objects)

