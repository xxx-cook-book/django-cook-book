# UserAgent Detect

## django-mobi

* Installation

  ```shell
  pip install django-mobi
  ```


* Settings.py

  ```python
  MIDDLEWARE_CLASSES = (
      ...
      'mobi.middleware.MobileDetectionMiddleware',
      ...
  )
  ```


* Usage

  ```python
  def khb_home(request):
      render_tmp = 'home/wap_home.html' if request.mobile else 'home/pc_home.html'
      return render(request, render_tmp, {})
  ```


## django-detect

* Installation

  ```shell
  pip install django-detect
  ```


* Settings.py

  ```python
  MIDDLEWARE_CLASSES = (
      ...
      'detect.middleware.UserAgentDetectionMiddleware',
      ...
  )
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

## References

[1] kencochrane@Bitbucket, [django-mobi —— Django middleware and view decorator to detect phones and small-screen devices](https://bitbucket.org/kencochrane/django-mobi/)

[2] Brightcells@Github, [django-detect —— UserAgent Detect](https://github.com/Brightcells/django-detect)