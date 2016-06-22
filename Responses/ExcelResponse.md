# ExcelResponse

## django-excel-response2

* Inherit

  ```python
  # Since Version 2.0.2
  if 'FileResponse' in names:
      ExcelResponse = type('ExcelResponse', (http.FileResponse, ), dict(__init__=__init__))
  elif 'StreamingHttpResponse' in names:
      ExcelResponse = type('StreamingHttpResponse', (http.StreamingHttpResponse, ), dict(__init__=__init__))
  else:
      ExcelResponse = type('HttpResponse', (http.HttpResponse, ), dict(__init__=__init__))
  ```

* Installation

  ```shell
  pip install django-excel-response2
  ```

* Usage

  ```shell
  from excel_response2 import ExcelResponse

  def excelview(request):
      objs = SomeModel.objects.all()
      return ExcelResponse(objs)

  def excelview2(request):
      data = [
          ['Column 1', 'Column 2'],
          [1, 2],
          [3, 4]
      ]
      return ExcelResponse(data, 'my_data', font='name SimSum')
  ```

* Params

  * font=’name SimSum’

    ```
    Set Font as SimSum(宋体). Default is ''
    ```

  * force_csv=True

    ```
    CSV Format? True for Yes, False for No. Default is False
    ```


## References

[1] Brightcells@Github, [django-excel-response2 — A function extends of Tarken's django-excel-response](https://github.com/Brightcells/django-excel-response2)