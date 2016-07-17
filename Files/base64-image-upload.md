# Base64 File Upload

## Core Codes

```python
# base64 encoded image - decode
format, imgstr = data.split(';base64,')  # format ~= data:image/X,
ext = format.split('/')[-1]  # guess file extension

data = ContentFile(base64.b64decode(imgstr), name='temp.' + ext)
```

## Base64ImageField

```python
import base64

from django.core.files.base import ContentFile
from rest_framework import serializers


class Base64ImageField(serializers.ImageField):
    def from_native(self, data):
        if isinstance(data, basestring) and data.startswith('data:image'):
            # base64 encoded image - decode
            format, imgstr = data.split(';base64,')  # format ~= data:image/X,
            ext = format.split('/')[-1]  # guess file extension

            data = ContentFile(base64.b64decode(imgstr), name='temp.' + ext)

        return super(Base64ImageField, self).from_native(data)
```

* [Django rest framework - Base64 image field](https://gist.github.com/yprez/7704036)

## Extension

* Javascript Get Base64

  ```javascript
  function getBase64Image(img) {
      // Create an empty canvas element
      var canvas = document.createElement("canvas");
      canvas.width = img.width;
      canvas.height = img.height;

      // Copy the image contents to the canvas
      var ctx = canvas.getContext("2d");
      ctx.drawImage(img, 0, 0);

      // Get the data-URL formatted image
      // Firefox supports PNG and JPEG. You could check img.src to
      // guess the original format, but be aware the using "image/jpg"
      // will re-encode the image.
      var dataURL = canvas.toDataURL("image/png");

      return dataURL.replace(/^data:image\/(png|jpg);base64,/, "");
  }
  ```
  * [Get image data in JavaScript?](http://stackoverflow.com/questions/934012/get-image-data-in-javascript)

## References

