# Emails

## send_mail

`send_mail`(*subject*, *message*, *from_email*, *recipient_list*, *fail_silently=False*, *auth_user=None*, *auth_password=None*,*connection=None*, *html_message=None*)

* Usage

  ```python
  from django.core.mail import send_mail

  send_mail('Subject here', 'Here is the message.', 'from@example.com',
      ['to@example.com'], fail_silently=False)
  ```


* from_email

  * The sender’s address. Both ``kimi.huang@brightcells.com`` and ``kimi.huang <kimi.huang@brightcells.com> `` forms are legal. If omitted, the [`DEFAULT_FROM_EMAIL`](https://docs.djangoproject.com/en/dev/ref/settings/#std:setting-DEFAULT_FROM_EMAIL) setting is used.

    ```python
    # Refer: https://github.com/django/django/blob/master/django/core/mail/message.py#L290
    self.from_email = from_email or settings.DEFAULT_FROM_EMAIL
    ```

  * Must be same as ``EMAIL_HOST_USER``, if not:

    ```shell
    SMTPSenderRefused: (501, 'mail from address must be same as authorization user', u'289264032@qq.com')
    ```
    * EMAIL_HOST_USER is only used for authentication on the mail server.

* recipient_list

  * Must be a list or tuple, if not:

    ```shell
    TypeError: "to" argument must be a list or tuple
    ```


* html_message
  * If `html_message` is provided, the resulting email will be a *multipart/alternative* email with`message` as the *text/plain* content type and `html_message` as the *text/html* content type.
* The return value will be the number of successfully delivered messages (which can be `0` or `1` since it can only send one message).

## send_mass_mail

`send_mass_mail`(*datatuple*, *fail_silently=False*, *auth_user=None*, *auth_password=None*, *connection=None*)

* Usage

  ```python
  from django.core.mail import send_mass_mail

  message1 = ('Subject here', 'Here is the message', 'from@example.com', ['first@example.com', 'other@example.com'])
  message2 = ('Another Subject', 'Here is another message', 'from@example.com', ['second@test.com'])

  send_mass_mail((message1, message2), fail_silently=False)
  ```


* `datatuple` is a tuple in which each element is in this format:

  ```python
  message1 = (subject, message, from_email, recipient_list)
  message2 = (subject, message, from_email, recipient_list)
  datatuple = (message1, message2)
  ```

* ``send_mass_mail()`` vs. ``send_mail()``

  The main difference between [`send_mass_mail()`](https://docs.djangoproject.com/en/dev/topics/email/#django.core.mail.send_mass_mail) and [`send_mail()`](https://docs.djangoproject.com/en/dev/topics/email/#django.core.mail.send_mail) is that [`send_mail()`](https://docs.djangoproject.com/en/dev/topics/email/#django.core.mail.send_mail) opens a connection to the mail server each time it’s executed, while [`send_mass_mail()`](https://docs.djangoproject.com/en/dev/topics/email/#django.core.mail.send_mass_mail) uses a single connection for all of its messages. This makes [`send_mass_mail()`](https://docs.djangoproject.com/en/dev/topics/email/#django.core.mail.send_mass_mail) slightly more efficient.


* The return value will be the number of successfully delivered messages.

## mail_admins

`mail_admins`(*subject*, *message*, *fail_silently=False*, *connection=None*, *html_message=None*)

## mail_managers

`mail_managers`(*subject*, *message*, *fail_silently=False*, *connection=None*, *html_message=None*)

## Attach File

* https://docs.djangoproject.com/en/dev/topics/email/#emailmessage-objects

## Multi Alternatives

* send_mail(…, *message*, ..., *html_message=None*)


* https://docs.djangoproject.com/en/dev/topics/email/#sending-alternative-content-types

PS: The ``message`` and ``html_message`` will not display at the same time. Only recipients can not handle an alternative content type. The ``message`` will display.

## Global Settings

```python
# Email address that error messages come from.
SERVER_EMAIL = 'kimi.huang@brightcells.com'

# The email backend to use. For possible shortcuts see django.core.mail.
# The default is to use the SMTP backend.
# Third-party backends can be specified by providing a Python path
# to a module that defines an EmailBackend class.
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'

# Host for sending email.
EMAIL_HOST = 'smtp.exmail.qq.com'

# Port for sending email.
EMAIL_PORT = 25

# Optional SMTP authentication information for EMAIL_HOST.
EMAIL_HOST_USER = 'kimi.huang@brightcells.com'
EMAIL_HOST_PASSWORD = '<^_^>pwd<^_^>'
EMAIL_USE_TLS = False
EMAIL_USE_SSL = False
EMAIL_SSL_CERTFILE = None
EMAIL_SSL_KEYFILE = None
EMAIL_TIMEOUT = None

# Default email address to use for various automated correspondence from
# the site managers.
DEFAULT_FROM_EMAIL = 'kimi.huang <kimi.huang@brightcells.com>'

# People who get code error notifications.
# In the format [('Full Name', 'email@example.com'), ('Full Name', 'anotheremail@example.com')]
ADMINS = [('Kimi.Huang', 'kimi.huang@brightcells.com'), ('QIMIN', '289264032@qq.com')]

# Not-necessarily-technical managers of the site. They get broken link
# notifications and other various emails.
MANAGERS = ADMINS

# Subject-line prefix for email messages send with django.core.mail.mail_admins
# or ...mail_managers.  Make sure to include the trailing space.
EMAIL_SUBJECT_PREFIX = u'[Django] '
```

## Email Reports Server Error
```
When DEBUG is False, 
Django will email the users listed in the ADMINS setting whenever your code raises an unhandled exception and results in an internal server error (HTTP status code 500). 
This gives the administrators immediate notification of any errors. 
The ADMINS will get a description of the error, a complete Python traceback, and details about the HTTP request that caused the error.
```
* [Email Reports Server Error](https://docs.djangoproject.com/en/dev/howto/error-reporting/#email-reports)

## References

[1] Docs@DjangoProject, [Sending email](https://docs.djangoproject.com/en/dev/topics/email/)

[2] Docs@DjangoProject, [Core Settings Topical Index — email](https://docs.djangoproject.com/en/dev/ref/settings/#email)

[3] Django@Github, [Django Global Settings](https://github.com/django/django/blob/354acd04af524ad82002b903df1189581c51cabe/django/conf/global_settings.py#L185)
