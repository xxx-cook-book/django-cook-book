# django-mathfilters

## Introduction

django-mathfilters is a pip-installable Python 2/3 module that provides different simple math filters for Django.

Django provides an `add` template filter, but no corresponding subtracting, multiplying or dividing filters.

Django ticket [#361](https://code.djangoproject.com/ticket/361) has been closed as *wontfix*, so I had to create an alternative that is easy to install in a new Django project.

## Installation

```shell
pip install django-mathfilters
```

## Usage

* Add ``mathfilters`` to ``INSTALLED_APPS``

  ```python
  INSTALLED_APPS += ('mathfilters', )
  ```

* You need to load `mathfilters` at the top of your template. The script provides the following filters:

  - `sub` – subtraction
  - `mul` – multiplication
  - `div` – division
  - `intdiv` – integer (floor) division
  - `abs` – absolute value
  - `mod` – modulo
  - `addition` – replacement for the `add` filter with support for float / decimal types

## References

[1] django-mathfilters@Github, [django-mathfilters — provides a set of simple math filters for Django](https://github.com/dbrgn/django-mathfilters)