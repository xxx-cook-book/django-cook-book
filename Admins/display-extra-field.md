# Display Extra Field

* See [django admin - add custom form fields that are not part of the model](http://stackoverflow.com/questions/17948018/django-admin-add-custom-form-fields-that-are-not-part-of-the-model)

  ```python
  class MyModelAdmin(models.ModelAdmin):
        list_display = ('field1', 'field2', 'combined_fields')
        readonly_fields = ('combined_fields', )

        def combined_fields(self, obj):
                return '{} {}'.format(obj.field1, obj.field2)
  ```

  * ``combined_fields`` is a ``extra field`` and can use as defined field

