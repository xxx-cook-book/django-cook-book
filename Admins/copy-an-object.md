# Copy An Object

1、Import python's ``copy`` to make a shallow copy of the object

2、Make the copy. Note this will copy ``regular`` attributes: CharField, IntegerField, etc. In addition, it will copy ForeignKey attributes

3、Set the object id to None. This is important. When the object is saved, a new row (or rows) will be added to the database

```python
import copy  # (1) use python copy

obj_copy = copy.copy(obj)  # (2) django copy object
obj_copy.id = None  # (3) set 'id' to None to create new object
obj_copy.save()  # initial save
```

## References

[1] rprasad @ harvard.edu, [using the Django admin to copy an object](https://blogs.harvard.edu/rprasad/2012/08/24/using-django-admin-to-copy-an-object/)

