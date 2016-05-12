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
            return self.readonly_fields
    ```

    ​