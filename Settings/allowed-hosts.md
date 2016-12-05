# ALLOWED_HOSTS

## ALLOWED_HOSTS

```
Values in this list can be fully qualified names (e.g. 'www.example.com'), in which case they will be matched against the request’s Host header exactly (case-insensitive, not including port). A value beginning with a period can be used as a subdomain wildcard: '.example.com' will match example.com, www.example.com, and any other subdomain of example.com. A value of '*' will match anything; in this case you are responsible to provide your own validation of the Host header (perhaps in a middleware; if so this middleware must be listed first in MIDDLEWARE).
```

##  validate_host
```python
In [1]: from django.http.request import validate_host

In [2]: allowed_hosts = ['tt4it.com']

In [3]: validate_host('tt4it.com', allowed_hosts)
Out[3]: True

In [4]: validate_host('a.tt4it.com', allowed_hosts)
Out[4]: False

In [5]: allowed_hosts = ['.tt4it.com']

In [6]: validate_host('tt4it.com', allowed_hosts)
Out[6]: True

In [7]: validate_host('a.tt4it.com', allowed_hosts)
Out[7]: True

In [8]: validate_host('a.b.tt4it.com', allowed_hosts)
Out[8]: True

In [9]: allowed_hosts = ['*.tt4it.com']

In [10]: validate_host('tt4it.com', allowed_hosts)
Out[10]: False

In [11]: validate_host('a.tt4it.com', allowed_hosts)
Out[11]: False
```
* django/http/request.py — get_host

  ```python
  def get_host(self):
      """Return the HTTP host using the environment or request headers."""
      host = self._get_raw_host()

      # There is no hostname validation when DEBUG=True
      if settings.DEBUG:
          return host

      domain, port = split_domain_port(host)
      if domain and validate_host(domain, settings.ALLOWED_HOSTS):
          return host
      else:
          msg = "Invalid HTTP_HOST header: %r." % host
          if domain:
              msg += " You may need to add %r to ALLOWED_HOSTS." % domain
          else:
              msg += " The domain name provided is not valid according to RFC 1034/1035."
          raise DisallowedHost(msg)
  ```

* django/http/request.py — validate_host

  ```python
  def validate_host(host, allowed_hosts):
      """
      Validate the given host for this site.

      Check that the host looks valid and matches a host or host pattern in the
      given list of ``allowed_hosts``. Any pattern beginning with a period
      matches a domain and all its subdomains (e.g. ``.example.com`` matches
      ``example.com`` and any subdomain), ``*`` matches anything, and anything
      else must match exactly.

      Note: This function assumes that the given host is lower-cased and has
      already had the port, if any, stripped off.

      Return ``True`` for a valid host, ``False`` otherwise.
      """
      host = host[:-1] if host.endswith('.') else host

      for pattern in allowed_hosts:
          if pattern == '*' or is_same_domain(host, pattern):
              return True

      return False
  ```

* django/utils/http.py — is_same_domain

  ```python
  def is_same_domain(host, pattern):
      """
      Return ``True`` if the host is either an exact match or a match
      to the wildcard pattern.

      Any pattern beginning with a period matches a domain and all of its
      subdomains. (e.g. ``.example.com`` matches ``example.com`` and
      ``foo.example.com``). Anything else is an exact string match.
      """
      if not pattern:
          return False

      pattern = pattern.lower()
      return (
          pattern[0] == '.' and (host.endswith(pattern) or host == pattern[1:]) or
          pattern == host
      )
  ```

## ``DEBUG=True``

```latex
Changed in Django 1.10.3:
In older versions, ALLOWED_HOSTS wasn’t checked if DEBUG=True. This was also changed in Django 1.9.11 and 1.8.16 to prevent a DNS rebinding attack.
```
* [DNS rebinding vulnerability when `DEBUG=True`](https://docs.djangoproject.com/en/1.10/releases/1.10.3/#dns-rebinding-vulnerability-when-debug-true)

## ``Domain Name Mapping`` vs. ``Nginx`` vs. ``ALLOWED_HOSTS`` 

##### *Domain Name Mapping*

| Type | Origin | Record    | TTL        |
| ---- | ------ | --------- | ---------- |
| A    | *      | 127.0.0.1 | 10 minutes |
| A    | @      | 127.0.0.1 | 10 minutes |

* ``A`` for ``IP Addresses``，``CNAME`` for ``Domain Name Aliases``
* ``*`` match ``*.tt4it.com``
* ``@`` just match ``tt4it.com``

##### *Nginx*
```
server_name  .tt4it.com;
```
* Match ``*.tt4it.com`` and ``tt4it.com``

##### *ALLOWED_HOSTS*
```python
ALLOWED_HOSTS = ['.tt4it.com']
```
* Match ``*.tt4it.com`` and ``tt4it.com``，Same as ``Nginx``

## References
[1] Docs@DjangoProject, [ALLOWED_HOSTS](https://docs.djangoproject.com/en/dev/ref/settings/#allowed-hosts)

