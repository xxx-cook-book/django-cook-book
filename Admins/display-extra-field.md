# Display Extra Field

* See [django admin - add custom form fields that are not part of the model](http://stackoverflow.com/questions/17948018/django-admin-add-custom-form-fields-that-are-not-part-of-the-model)

  ```python
  from django.utils.html import format_html

  class MyModelAdmin(models.ModelAdmin):
        list_display = ('field1', 'field2', 'combined_fields')
        readonly_fields = ('combined_fields', )

        def combined_fields(self, obj):
            return '{} {}'.format(obj.field1, obj.field2)

        def show_href(self, obj):
            href = '{0}/admin/app/model/?q={1}'.format(settings.DOMAIN, obj.xxx_id)
            return format_html(u"""<a href='{0}'>{1}</a>""", href, u'Verbose_name')
  ```

  * ``combined_fields`` is a ``extra field`` and can use as defined field

