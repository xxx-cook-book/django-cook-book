# Verification Code

## Randint

```python
# setting.py
VERIFICATION_CODE_RANGE = [100000, 999999]

# views.py
import random
from django.conf import settings

code = random.randint(*settings.VERIFICATION_CODE_RANGE)
```

## References

