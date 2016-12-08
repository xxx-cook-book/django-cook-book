# Regexes

## Regular Expression Syntax

* General

  ```python
  Choice
      Sequence | Sequence | ...
  Sequence
      Factor Quantifier ...
  Factor
      Groups
      Classes
      Characters & Escapes
  Quantifier
      * 0 or more times
      ? 0 or 1 times
      + 1 or more times
      {N} exactly N times
      {N,} N or more times
      {N,M} N through M times
  ```

* Groups

  ```
  Capturing Group
      (RegExp)
  Non-capturing Group
      (?:RegExp)
  Non-greedy Matching
      (RegExp?)
  Backreference
      \1 ... \N 
  ```

* Classes

  ```
  Basic
      [12345], [abcde]
  Ranges
      [1-5], [a-e]
  Complements "Not In"
      [^6-9], [^f-z]
  Characters Needing Escape
      - / [ \ ] ^
  ```

* Chars

  ```
  Characters
      Any Unicode character except:
      /\ [] () {} ? + * | special/ctrl- char 
  Special Characters
      ^ Start of Line
      $ End of Line
      . Any character except end of line
  Escapes
      \d	Digit 	\D	Non-digit
      \s	Whitespace 	\S	Non-whitespace
      \w	"Word" 	\W	Non-"Word"
      \n	New Line 	\f	Formfeed
      \t	Tab 	\r	Carriage Return
      \uNNNN Unicode where N is hex
  ```

## Examples

* Chinese

  ```python
  url(r'^update/(?P<uname>[^/]+)/$', 'update', name='update'),
  ```

* Any One

  ```python
  url(r'^(?P<action>(cancel|confirm|success))/$', 'order_handle', name='order_handle'),
  ```

## References

[1] GetHiFi, [HiFi RegExp Tool](http://www.gethifi.com/tools/regex)