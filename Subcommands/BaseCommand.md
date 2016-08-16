# BaseCommand

## Since Django 1.8

## Before Django 1.8

## Compatibility

```python
class ProxyParser(object):
    """Faux parser object that will ferry our arguments into options."""

    def __init__(self, command):
        self.command = command

    def add_argument(self, *args, **kwargs):
        self.command.option_list += (make_option(*args, **kwargs), )


class CompatibilityBaseCommand(BaseCommand):
    """Provides a compatibility between optparse and argparse transition.
    Starting in Django 1.8, argparse is used. In Django 1.9, optparse support will be removed.
    For optparse, you append to the option_list class attribute.
    For argparse, you must define add_arguments(self, parser).
    BaseCommand uses the presence of option_list to decide what course to take.
    """

    def __init__(self, *args, **kwargs):
        if django.VERSION < (1, 8) and hasattr(self, 'add_arguments'):
            self.option_list = BaseCommand.option_list
            parser = ProxyParser(self)
            self.add_arguments(parser)
        super(CompatibilityBaseCommand, self).__init__(*args, **kwargs)


class CompatibilityAppCommand(AppCommand, CompatibilityBaseCommand):
    """AppCommand is a BaseCommand sub-class without its own __init__."""


class CompatibilityLabelCommand(LabelCommand, CompatibilityBaseCommand):
    """LabelCommand is a BaseCommand sub-class without its own __init__."""
```

## References

[1] django-extensions/django-extensions@Github, [Transition command files to argparse compatibility.](https://github.com/django-extensions/django-extensions/pull/804)

[2] django-chinese-docs-16@ReadTheDocs, [Django 1.6 documentation — Writing custom django-admin commands](http://django-chinese-docs-16.readthedocs.io/en/latest/howto/custom-management-commands.html)

[3] Docs@DjangoProject, [Django dev documentation — Writing custom django-admin commands](https://docs.djangoproject.com/en/dev/howto/custom-management-commands/)

