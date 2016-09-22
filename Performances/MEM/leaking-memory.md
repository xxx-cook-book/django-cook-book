# Leaking Memory

## connection.queries
|                      |           Before *Django 1.8*            |            Since *Django 1.8*            |
| -------------------- | :--------------------------------------: | :--------------------------------------: |
| Web Request          | signals.request_started.connect(reset_queries) | signals.request_started.connect(reset_queries) |
| Python manage.py xxx |                unlimited                 |           queries_limit = 9000           |

* `connection.queries` is only available if [`DEBUG`](https://docs.djangoproject.com/en/dev/ref/settings/#std:setting-DEBUG) is `True`. 
* `connection.queries` includes all SQL statements – INSERTs, UPDATEs, SELECTs, etc. Each time your app hits the database, the query will be recorded.
* If ``DEBUG is True`` & ``before Django 1.8`` & ``python manage.py xxx``, You may find your Django processes are allocating more and more memory, with no sign of releasing it.

## Queries

```python
In [21]: from django.db import connection

In [22]: connection.queries
Out[22]: []

In [23]: from account.models import Profile

In [24]: Profile.objects.count()
Out[24]: 3

In [25]: connection.queries
Out[25]: [{u'sql': u'SELECT COUNT(*) FROM `account_profile`', u'time': u'0.000'}]

In [26]: from django.db import connections

In [27]: connections['default'].queries
Out[27]: [{u'sql': u'SELECT COUNT(*) FROM `account_profile`', u'time': u'0.000'}]
    
In [28]:
```

## Delete/Clear

* Web Request

  ```python
  # Register an event to reset saved queries when a Django request is started.
  def reset_queries(**kwargs):
      for conn in connections.all():
          conn.queries_log.clear()
  signals.request_started.connect(reset_queries)
  ```

* Python manage.py xxx

  ```python
  from django.db import reset_queries
  reset_queries()
  ```

## References

[1] Paul Tarjan@StackOverflow, [Python: Memory leak debugging](http://stackoverflow.com/questions/1339293/python-memory-leak-debugging)

[2] Ticket@DjangoProject, [Ceiling limit to connection.queries](https://code.djangoproject.com/ticket/6734)

[3] django/django@Github, [Fixed #3711, #6734, #12581 -- Bounded connection.queries.](https://github.com/django/django/commit/cfcca7ccce3dc527d16757ff6dc978e50c4a2e61)

[4] Releases@DjangoProject, [Django 1.8 release notes](https://docs.djangoproject.com/en/dev/releases/1.8/#models)

[5] Docs@DjangoProject, [How can I see the raw SQL queries Django is running?](https://docs.djangoproject.com/en/dev/faq/models/#how-can-i-see-the-raw-sql-queries-django-is-running)