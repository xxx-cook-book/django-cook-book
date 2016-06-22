# HttpResponse

## Passing iterators

Finally, you can pass `HttpResponse` an iterator rather than strings. `HttpResponse` will consume the iterator immediately, store its content as a string, and discard it. Objects with a `close()` method such as files and generators are immediately closed.

If you need the response to be streamed from the iterator to the client, you must use the [`StreamingHttpResponse`](https://docs.djangoproject.com/en/dev/ref/request-response/#django.http.StreamingHttpResponse)class instead.

## References

[1] Docs@DjangoProject, [HttpResponse objects](https://docs.djangoproject.com/en/dev/ref/request-response/#httpresponse-objects)

[2] Tickets@DjangoProject, [#6527 A bug in HttpResponse with iterators](https://code.djangoproject.com/ticket/6527)