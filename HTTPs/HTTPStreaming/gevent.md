# gevent

## gevent/Event

* Source Code

  ```python
  import gevent
  from gevent.pywsgi import WSGIServer
  from gevent.queue import Queue, Empty

  data_source = Queue()

  def producer():
      while True:
          data_source.put_nowait('Hello World')
          gevent.sleep(1)

  def ajax_endpoint(environ, start_response):
      status = '200 OK'
      headers = [
          ('Content-Type', 'application/json')
      ]

      start_response(status, headers)

      while True:
          try:
              datum = data_source.get(timeout=5)
              yield datum + '\n'
          except Empty:
              pass

  gevent.spawn(producer)

  WSGIServer(('', 8000), ajax_endpoint).serve_forever()
  ```
* Streaming tells clinet not close by adding ``Transfer Encoding: chunked`` in ``Response Headers``
  ![](http://ww3.sinaimg.cn/large/c05783a7gw1f557st2m0ij21kw09xq4n.jpg)

## gevent + Django

* Impossible if directly realize in Django
* Django is ``request`` and ``resposne``
* Should start a extra service

## References

[1] TIMEX@StackOverflow, [Does Django have a way to open a HTTP long poll connection?](http://stackoverflow.com/questions/4787530/does-django-have-a-way-to-open-a-http-long-poll-connection)

[2] DeveloperWorks@IBM, [Reverse Ajax, Part 1: Introduction to Comet](https://www.ibm.com/developerworks/web/library/wa-reverseajax1/)

