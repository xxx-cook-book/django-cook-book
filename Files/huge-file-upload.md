# Huge File Upload

## File Upload

* Upload

  ```python
  photo_path = 'fly/{}{}'.format(shortuuid.uuid(), os.path.splitext(photo.name)[1] or 'jpeg')

  if default_storage.exists(photo_path):
      default_storage.delete(photo_path)
  default_storage.save(photo_path, photo)
  ```

* Problem

  * Upload huge file as above without any settings, the file's ``File Permission / Access Modes`` as below

    ```shell
    $ ll ZWMwDwN8hcheGm3ScHxkui
    -rw------- 1 django django 124203 Apr 16 18:24 ZWMwDwN8hcheGm3ScHxkui.jpeg
    ```

  * Visit this file in browser will be ``403 Forbidden``

## InMemoryUploadedFile/TemporaryUploadedFile
* 1M Size(InMemoryUploadedFile)
  ```python
  ipdb> request.FILES
  <MultiValueDict: {u'photo': [<InMemoryUploadedFile: photo.jpeg (image/jpeg)>]}>
  ```
* 6M Size(TemporaryUploadedFile)
  ```python
  ipdb> request.FILES
  <MultiValueDict: {u'photo': [<TemporaryUploadedFile: photo.jpeg (image/jpeg)>]}>
  ```

## FILE_UPLOAD_MAX_MEMORY_SIZE
* FILE_UPLOAD_MAX_MEMORY_SIZE

  ```
  Default: 2621440 (i.e. 2.5 MB).

  The maximum size (in bytes) that an upload will be before it gets streamed to the file system. See Managing files for details.
  ```


* See [std:setting-FILE_UPLOAD_MAX_MEMORY_SIZE](https://docs.djangoproject.com/en/dev/ref/settings/#std:setting-FILE_UPLOAD_MAX_MEMORY_SIZE)

## FILE_UPLOAD_PERMISSIONS
* FILE_UPLOAD_PERMISSIONS

  ```
  Default: None

  The numeric mode (i.e. 0o644) to set newly uploaded files to. For more information about what these modes mean, see the documentation for os.chmod().

  If this isn’t given or is None, you’ll get operating-system dependent behavior. On most platforms, temporary files will have a mode of 0o600, and files saved from memory will be saved using the system’s standard umask.

  For security reasons, these permissions aren’t applied to the temporary files that are stored in FILE_UPLOAD_TEMP_DIR.

  This setting also determines the default permissions for collected static files when using the collectstatic management command. See collectstatic for details on overriding it.
  ```


* See [file-upload-permissions](https://docs.djangoproject.com/en/dev/ref/settings/#file-upload-permissions)

## Settings.py
```python
# File Settings
FILE_UPLOAD_MAX_MEMORY_SIZE = 5242880  # InMemoryUploadedFile Max Size, Default 2.5 MB, Current Setting 5 MB
FILE_UPLOAD_PERMISSIONS = 0o644  # TemporaryUploadedFile File Permissions, Default 0o600(-rw-------), Current Setting 0o644(-rw-r--r--)
```

## References
[1] Ben@StackOverflow, [Django Image Upload…Prevent huge file uploads](http://stackoverflow.com/questions/5767840/django-image-upload-prevent-huge-file-uploads)

