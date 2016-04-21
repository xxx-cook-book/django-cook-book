# ExcelResponse

## django-excel-response2

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