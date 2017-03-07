# Exec After Click Delete

## delete_model

If you want to ``modify some fields`` or ``run some code`` after click ``Delete Button`` to delete record. You can override ``delete_model`` in ``admin.py`` to realize it

* Definition

  ```python
  delete_model(request, obj)
  ```

* Usage

  ```python
  from django.contrib import admin

  class ArticleAdmin(admin.ModelAdmin):
  	def delete_model(self, request, obj):
  		# Some Code to Run
      	obj.delete()
      	# Some Code to Run
  ```

* Tips

  * ``obj.pk`` be ``None`` after ``obj.delete()`` executed


