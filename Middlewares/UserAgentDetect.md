# UserAgent Detect

## django-mobi

* Open Source Project: [https://bitbucket.org/kencochrane/django-mobi/](https://bitbucket.org/kencochrane/django-mobi/)

* Installation

  ```shell
  pip install django-mobi
  ```


* Usage

  ```python
  def khb_home(request):
      render_tmp = 'home/wap_home.html' if request.mobile else 'home/pc_home.html'
      return render(request, render_tmp, {})
  ```


* Settings.py

  ```python
  MIDDLEWARE_CLASSES = (
      ...
      'mobi.middleware.MobileDetectionMiddleware',
      ...
  )
  ```

### django-detect

* Open Source Project: [https://github.com/Brightcells/django-detect](https://github.com/Brightcells/django-detect)

* Installation

  ```shell
  pip install django-detect
  ```


* Usage

  ```
  # Weixin／Wechat
  request.weixin
  request.weixin.version
  request.wechat
  request.wechat.version
  # iPhone/iPad/iPod
  request.iPhone
  request.iPad
  request.iPod
  # iOS
  request.iOS = request.iPhone or request.iPad or request.iPod
  # Android
  request.Android
  request.Android.version
  ```


* Settings.py

  ```python
  MIDDLEWARE_CLASSES = (
      ...
      'detect.middleware.UserAgentDetectionMiddleware',
      ...
  )
  ```
