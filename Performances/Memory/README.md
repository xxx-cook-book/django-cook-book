# MEM

## Top

* Docs

  ```shell
  man top
  ```

* Sorting

  ```
  For compatibility, this top supports most of the former top sort keys. Since this is primarily a service to former top users, these commands do not appear on any help screen.
      command   sorted field                  supported
        A         start time (non-display)      No
        M         %MEM                          Yes
        N         PID                           Yes
        P         %CPU                          Yes
        T         TIME+                         Yes
  ```

## pmap

```shell
pmap -d pid
```

## References