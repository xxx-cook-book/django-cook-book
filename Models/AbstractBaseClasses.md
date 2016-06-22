# Abstract Base Classes

## basemodels.py

```python
# -*- coding: utf-8 -*-

from django.db import models
from django.utils.translation import ugettext_lazy as _


class CreateUpdateMixin(models.Model):
    status = models.BooleanField(_(u'status'), default=True, help_text=_(u'Status'), db_index=True)
    created_at = models.DateTimeField(_(u'created_at'), auto_now_add=True, editable=True, help_text=_(u'Created Time'))
    updated_at = models.DateTimeField(_(u'updated_at'), auto_now=True, editable=True, help_text=_(u'Updated Time'))

    class Meta:
        abstract = True
```

## Usage

```python
# -*- coding: utf-8 -*-

from xxx.basemodels import CreateUpdateMixin

class XxxInfo(CreateUpdateMixin):
    xxx
```

