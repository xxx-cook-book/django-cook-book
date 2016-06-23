# gevent

## gevent/Event

* Source Code

  ```python
  import gevent
  from gevent.event import Event

  '''
  Illustrates the use of events
  '''

  evt = Event()

  def setter():
      '''After 10 seconds, wake all threads waiting on the value of evt'''
      print('A: Hey wait for me, I have to do something')
      gevent.sleep(10)
      print("A: Ok, I'm done")
      evt.set()

  def waiter():
      '''After 10 seconds the get call will unblock'''
      print("B: I'll wait for you")
      evt.wait()  # blocking
      print("B: It's about time")
      
  def main():
      gevent.joinall([
          gevent.spawn(setter),
          gevent.spawn(waiter),
          gevent.spawn(waiter),
          gevent.spawn(waiter),
          gevent.spawn(waiter),
          gevent.spawn(waiter),
      ])

  if __name__ == '__main__':
      main()
  ```

* Exec

  ```
  $ python gevent_envet.py
  A: Hey wait for me, I have to do something
  B: I'll wait for you
  B: I'll wait for you
  B: I'll wait for you
  B: I'll wait for you
  B: I'll wait for you
  # 10 seconds later
  A: Ok, I'm done
  B: It's about time
  B: It's about time
  B: It's about time
  B: It's about time
  B: It's about time
  ```

## gevent + Django

* Javacript: Re Ajax Request in ``onSuccess``/``onError``
* Django: evt.wait()  # blocking
* See https://github.com/gevent/gevent/tree/master/examples/webchat

## References

[1] TIMEX@StackOverflow, [Does Django have a way to open a HTTP long poll connection?](http://stackoverflow.com/questions/4787530/does-django-have-a-way-to-open-a-http-long-poll-connection)

[2] DeveloperWorks@IBM, [Reverse Ajax, Part 1: Introduction to Comet](https://www.ibm.com/developerworks/web/library/wa-reverseajax1/)

