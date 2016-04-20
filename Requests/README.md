# Requests

* HttpRequest.REQUEST

  Deprecated since Django 1.7

  Removed since Django 1.9

  * For convenience, a dictionary-like object that searches `POST` first, then `GET`. Inspired by PHP’s `$_REQUEST`


* For example, if `GET = {"name": "john"}` and `POST = {"age": '34'}`, `REQUEST["name"]` would be`"john"`, and `REQUEST["age"]` would be `"34"`


* It’s strongly suggested that you use `GET` and `POST` instead of `REQUEST`, because the former are more explicit

  Support both ``GET`` and ``POST``

```python
  param = request.REQUEST.get('param', '')  # Before Django 1.9
  param = request.GET.get('param') or request.POST.get('param', '')  # All Django Version
```

  ​

