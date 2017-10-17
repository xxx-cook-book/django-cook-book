# Disable Editing

## ReadOnlyModelAdmin
```python
class ReadonlyModelAdmin(object):
    def get_readonly_fields(self, request, obj=None):
        if obj:  # editing an existing object
            return tuple(set(self.readonly_fields) | set(f.name for f in self.model._meta.fields) - set(self.readonly_fields_exclude))
        return tuple(set(self.readonly_fields) - set(self.readonly_fields_exclude))


class ReadOnlyModelAdmin(ReadonlyModelAdmin):
    """ Disables all editing capabilities. """
    change_form_template = 'admin/view.html'

    def __init__(self, *args, **kwargs):
        super(ReadOnlyModelAdmin, self).__init__(*args, **kwargs)

    def get_actions(self, request):
        actions = super(ReadOnlyModelAdmin, self).get_actions(request)
        if 'delete_selected' in actions:
            del actions['delete_selected']
        return actions

    def has_add_permission(self, request):
        return False

    def has_delete_permission(self, request, obj=None):
        return False

    def save_model(self, request, obj, form, change):
        pass

    def delete_model(self, request, obj):
        pass

    def save_related(self, request, form, formsets, change):
        pass
```

* templates/admin/view.html
  ```html
  {% extends "admin/change_form.html" %}
  {% load i18n %}

  {% block submit_buttons_bottom %}
  {% endblock %}
  ```

## References

[1] ppo@DjangoSnippets, [ReadOnlyAdminMixin](https://djangosnippets.org/snippets/10539/)

[2] django-xxx@Github, [django-admin — Django Admin Extensions](https://github.com/django-xxx/django-admin)
​
