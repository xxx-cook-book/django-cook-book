# Extra/Aggregation

## output_field

* Official Example

  * https://docs.djangoproject.com/en/dev/topics/db/aggregation/#cheat-sheet

    ```python
    # Cost per page
    >>> from django.db.models import F, FloatField, Sum
    >>> Book.objects.all().aggregate(
    ...    price_per_page=Sum(F('price')/F('pages'), output_field=FloatField()))
    {'price_per_page': 0.4470664529184653}
    ```


* Django Q Example

  * https://github.com/Koed00/django-q/pull/185

    ```python
    In [1]: from django.db.models import Sum, F, FloatField

    In [2]: from django.utils import timezone
        
    In [3]: from django_q.models import Success

    In [4]: from datetime import timedelta
        
    In [5]: Success.objects.filter(stopped__gte=timezone.now() - timedelta(hours=24)).aggregate(time_taken=Sum(F('stopped') - F('started')))
    # AttributeError: 'Decimal' object has no attribute 'tzinfo'

    In [6]: Success.objects.filter(stopped__gte=timezone.now() - timedelta(hours=24)).aggregate(time_taken=Sum(F('stopped') - F('started'), output_field=FloatField()))
    Out[6]: {'time_taken': 0.004298}
    ```

## References

[1] Docs@DjangoProject, [Aggregation](https://docs.djangoproject.com/en/dev/topics/db/aggregation/)

[2] Docs@DjangoProject, [Query Expressions](https://docs.djangoproject.com/en/dev/ref/models/expressions/)

[3] Django@Github, [django/django/db/models/expressions.py/_resolve_output_field](https://github.com/django/django/blob/master/django%2Fdb%2Fmodels%2Fexpressions.py#L244)

[4] Django@Github, [django/django/db/models/sql/compiler.py/get_converters](https://github.com/django/django/blob/master/django%2Fdb%2Fmodels%2Fsql%2Fcompiler.py#L764)

[5] Django@Github, [django/django/db/backends/mysql/operations.py/convert_datetimefield_value](https://github.com/django/django/blob/master/django%2Fdb%2Fbackends%2Fmysql%2Foperations.py#L222)

[6] Tickets@DjangoProject, [Support for annotate(end=F('start') + F('duration'), output_field=DateTimeField)](https://code.djangoproject.com/ticket/24485)