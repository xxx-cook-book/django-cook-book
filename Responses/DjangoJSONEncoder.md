# DjangoJSONEncoder

Be aware that not all Django output can be passed unmodified to [`json`](https://docs.python.org/3/library/json.html#module-json). For example, if you have some custom type in an object to be serialized, you’ll have to write a custom [`json`](https://docs.python.org/3/library/json.html#module-json) encoder for it. Something like this will work:

```python
from django.utils.encoding import force_text
from django.core.serializers.json import DjangoJSONEncoder

class LazyEncoder(DjangoJSONEncoder):
    def default(self, obj):
        if isinstance(obj, YourCustomType):
            return force_text(obj)
        return super(LazyEncoder, self).default(obj)
```

## References

[1] Docs@DjangoProject, [DjangoJSONEncoder](https://docs.djangoproject.com/en/dev/topics/serialization/#djangojsonencoder)