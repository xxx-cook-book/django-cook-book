# Exec After Click Save

## save_model

If you want to ``modify some fields`` or ``run some code`` after click ``Save Button`` to save record. You can override ``save_model`` in ``admin.py`` to realize it

* Definition

  ```python
  save_model(request, obj, form, change)
  ```

* Usage

  * Save Record's Mender

    ```python
    from django.contrib import admin

    class ArticleAdmin(admin.ModelAdmin):
        def save_model(self, request, obj, form, change):
            obj.user = request.user
            obj.save()
    ```

  * Save Field's MD5

  * Judge Whether Field Modified Or Not

    ```python
    from django.contrib import admin

    class ArticleAdmin(admin.ModelAdmin):
    	def save_model(self, request, obj, form, change):
        	if ('onshalf' in form.changed_data) and (obj.onshalf == 'on'):
            	obj.online_at = timezone.now()
    ```

    ​