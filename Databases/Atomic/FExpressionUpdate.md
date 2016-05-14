# F Expression Update

```python
from django.db.models import F

TipReceiverInfo.objects.filter(receiver_id=receiver_id).update(receiver_total_fee=F('receiver_total_fee') + amount)
```

