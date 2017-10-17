# Field Readonly After Save

## get_readonly_fields

If you want to make a field ``editable before save`` and ``readonly after save``. You can override ``get_readonly_fields`` in ``admin.py`` to realize it

* Definition

  ```python
  get_readonly_fields(self, request, obj=None)
  ```

* Usage
  * Make Appointed Fields Readonly
    ```python
    from django.contrib import admin

    class MyModelAdmin(admin.ModelAdmin):
        def get_readonly_fields(self, request, obj=None):
            if obj:  # editing an existing object
                return self.readonly_fields + ('field1', 'field2')
            return self.readonly_fields
    ```

    * ``field1`` and ``field2`` will be  ``editable before save`` and ``readonly after save``

  * Make All Fields Readonly

    ```python
    from django.contrib import admin

    class MyModelAdmin(admin.ModelAdmin):
        def get_readonly_fields(self, request, obj=None):
            if obj:  # editing an existing object
                return self.readonly_fields + tuple(obj._meta.get_all_field_names())
                # return self.readonly_fields + tuple(f.name for f in self.model._meta.fields)
                # return self.readonly_fields + tuple(f.name for f in obj._meta.fields)
            return self.readonly_fields
    ```
    * [get_all_field_names() removed in Django 1.10](https://docs.djangoproject.com/en/1.10/releases/1.10/#features-removed-in-1-10)

## References

[1] yprez@StackOverflow, [Django admin - make all fields readonly](https://stackoverflow.com/questions/13817525/django-admin-make-all-fields-readonly)

[2] django-xxx@Github, [django-admin — Django Admin Extensions](https://github.com/django-xxx/django-admin)
​
