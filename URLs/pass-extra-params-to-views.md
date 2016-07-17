# Pass Extra Params to Views

## urls.py

```python
('^foo/$', 'name_age', {'name': 'foo', 'age': 20}),
('^bar/$', 'name_age', {'name': 'bar', 'age': 18}),
```

## views.py

```python
def name_age(request, name, age):
    return HttpResponse('name: %s -- age: %s' %(name, age))
```

