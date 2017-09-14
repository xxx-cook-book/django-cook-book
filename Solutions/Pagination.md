# Pagination

## Page Utils

* page_utils.py

  ```python
  from django.db.models.query import QuerySet

  def pagination(queryset, page, num=10, strict=False):
      """
      Simple Pagination Funciton
      :param queryset:
      :param page:
      :param num: number per page
      :param strict: strict left number or not
      :return: matched query, left number(default not strict)
      """
      start, end = num * (page - 1), num * page
      return queryset[start: end], max(queryset.count() if isinstance(queryset, QuerySet) else len(queryset) - end, 0) if strict else len(queryset[end: end + 1])
  ```

* django-paginator

  * Installation

    ```
    pip install django-paginator2
    ```

  * Usage

    ```
    In [1]: from paginator import pagination

    In [2]: pagination(range(100), 1)
    Out[2]: ([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], 1)

    In [3]: pagination(range(100), 1, strict=True)
    Out[3]: ([0, 1, 2, 3, 4, 5, 6, 7, 8, 9], 90)

    In [4]: pagination(range(100), 1, 20, strict=True)
    Out[4]: ([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19], 80)
    ```

## settings.py

```python
# MAX_BIGINT
# Why Not ``sys.maxint``
# n bit platform sys.maxint = 2 ** (n - 1) - 1
# 64 bit 9223372036854775807, 32 bit 2147483647
from django.db.models import BigIntegerField  # isort:skip
MAX_BIGINT = BigIntegerField.MAX_BIGINT
```

## views.py

```python
from django.conf import settings
from utils.page_utils import pagination

page = int(request.POST.get('page', 1))
num = int(request.POST.get('num', 10))
pk = int(request.POST.get('pk', settings.MAX_BIGINT))
# pk = request.POST.get('pk', settings.MAX_BIGINT)  # Not Convert Also OK

objs = ObjectsInfo.objects.filter(
    pk__lte=pk,
    status=True,
).order_by(
    '-pk'
)
objs, left = pagination(objs, page, num)
objs = [obj.data for obj in objs]
```

## References

[1] django-xxx/django-paginator@Github, [Simple Django Paginator](https://github.com/django-xxx/django-paginator)

