# Filter User Owned Objects

## get_queryset

The get_queryset method on a ModelAdmin returns a QuerySet of all model instances that can be edited by the admin site

* Definition

  ```python
  ModelAdmin.get_queryset(request)
  ```

* Usage
  * Show Objects Owned By The Logged-in User
    ```python
    class MyModelAdmin(admin.ModelAdmin):
        def get_queryset(self, request):
            qs = super().get_queryset(request)
            if request.user.is_superuser:
                return qs
            return qs.filter(author=request.user)
    ```
