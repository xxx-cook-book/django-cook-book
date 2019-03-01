# Models

* Support ``charset`` default ``utf8` for field, How?
```
nickname = models.CharField(_(u'nickname'), max_length=255, blank=True, null=True, help_text=u'用户昵称', charset='utf8mb4')
```
