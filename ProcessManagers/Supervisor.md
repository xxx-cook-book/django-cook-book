# Supervisor

## apt

* Installation

  ```shell
  sudo apt install supervisor
  ```

* Service

  ```shell
  $ sudo service supervisor start
  Starting supervisor: supervisord.
  $ sudo service supervisor restart
  Restarting supervisor: supervisord.
  $ ps -ef | grep supervisor
  root     17452     1  0 19:23 ?        00:00:00 /usr/bin/python /usr/bin/supervisord -c /etc/supervisor/supervisord.conf
  $ sudo service supervisor stop
  Stopping supervisor: supervisord.
  $ ps -ef | grep supervisor
  ```

* ``/etc/supervisor/supervisord.conf``

  ```shell
  ; supervisor config file

  [unix_http_server]
  file=/var/run/supervisor.sock   ; (the path to the socket file)
  chmod=0766                       ; sockef file mode (default 0700)

  [supervisord]
  logfile=/var/log/supervisor/supervisord.log ; (main log file;default $CWD/supervisord.log)
  pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
  childlogdir=/var/log/supervisor            ; ('AUTO' child log dir, default $TEMP)

  ; the below section must remain in the config file for RPC
  ; (supervisorctl/web interface) to work, additional interfaces may be
  ; added by defining them in separate rpcinterface: sections
  [rpcinterface:supervisor]
  supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface

  [supervisorctl]
  serverurl=unix:///var/run/supervisor.sock ; use a unix:// URL  for a unix socket

  ; The [include] section can just contain the "files" setting.  This
  ; setting can list multiple files (separated by whitespace or
  ; newlines).  It can also contain wildcards.  The filenames are
  ; interpreted as relative to this file.  Included files *cannot*
  ; include files themselves.

  [include]
  files = /etc/supervisor/conf.d/*.conf
  ```

* ``/etc/supervisor/conf.d/rlog.conf``

  ```shell
  [program:rlog]
  command=/home/diors/env/bin/python /home/diors/work/tt4it/manage.py rlistlog --key=django:logit:tt4it --filename=/tmp/tt4it.logit.log
  autostart=true
  autorestart=true
  startretries=3
  exitcodes=0,1,2
  stopsignal=KILL  # ``signal SIGQUIT/SIGINT`` can't kill as expect, Use ``signal SIGKILL``
  stopasgroup=true
  killasgroup=true
  stdout_logfile=/var/log/supervisor_rlog_access.log
  stderr_logfile=/var/log/supervisor_rlog_error.log
  user=diors
  ```

  * ``python /home/diors/work/tt4it/manage.py rlistlog --key=django:logit:tt4it --filename=/tmp/tt4it.logit.log`` cause ``ImportError: No module named django.core.management``

* Supervisorctl

  ```shell
  $ sudo supervisorctl
  supervisor> quit
  $ sudo supervisorctl reread
  rlog: available
  $ sudo supervisorctl update
  rlog: added process group
  $ sudo supervisorctl
  rlog                             FATAL      Exited too quickly (process log may have deta
  ils)
  supervisor> quit
  $ sudo supervisorctl reread
  rlog: changed
  $ sudo supervisorctl update
  rlog: stopped
  rlog: updated process group
  $ sudo supervisorctl
  rlog                             RUNNING    pid 17767, uptime 0:00:07
  supervisor> quit
  ```

* Problems

  * [Errno 13] Permission denied

    ```shell
    $ supervisorctl
    error: <class 'socket.error'>, [Errno 13] Permission denied: file: /usr/lib/python2.7/soc
    ket.py line: 224
    ```

    * See [Permession denied error when use supervisorctl #173](https://github.com/Supervisor/supervisor/issues/173)

    * Change Socket File Mode

      ```shell
      [unix_http_server]
      file=/var/run/supervisor.sock   ; (the path to the socket file)
      chmod=0766                       ; sockef file mode (default 0700)
      ```

## yum

* Installation
  ```shell
  sudo yum install supervisor
  ```

* Service

  ```shell
  $ sudo service supervisord start
  Redirecting to /bin/systemctl start  supervisord.service
  $ sudo service supervisord restart
  Redirecting to /bin/systemctl restart  supervisord.service
  $ ps -ef | grep supervisor
  root     28383     1  0 16:51 ?        00:00:00 /usr/bin/python /usr/bin/supervisord -c /etc/supervisord.conf
  $ sudo service supervisord stop
  Redirecting to /bin/systemctl stop  supervisord.service
  $ ps -ef | grep supervisor
  ```

* Configuration File
  ```shell
  $ cd /etc/supervisord.
  supervisord.conf  supervisord.d/
  ```

## PIP

* Installation
  ```shell
  pip install supervisor
  ```

* Creating Configuration File

  ```shell
  # This won’t work if you do not have root access
  # If you don’t have root access, or you’d rather not put the supervisord.conf file in /etc/supervisord.conf`
  # You can place it in a specify directory (echo_supervisord_conf > /home/diors/supervisord.conf)
  # And start supervisord with the -c flag in order to specify the configuration file location
  echo_supervisord_conf > /etc/supervisord.conf  # Need root access
  echo_supervisord_conf > /home/diors/supervisord.conf  # Specify the configuration file location
  ```

* Modify Configuration File

  ```shell
  ;[include]
  ;files = relative/directory/*.ini

  ==>

  [include]
  files = /home/diors/supervisor/*.ini
  ```

  *Tips: Supervisor will include files when started*

  *Tips: Configuration Dir: ``/home/diors/supervisor/``*

  *Tips: Log Dir: ``/home/diors/supervisorlog/``*

  *Tips: Create Above Dir if Not Exists*

* Start Supervisor

  ```shell
  supervisord  # Default /etc/supervisord.conf
  supervisord -c /home/diors/supervisord.conf  # Specify the configuration file location
  ```

* ``/home/diors/supervisor/rlog.ini``

  ```shell
  [program:rlog]
  command=/home/diors/env/bin/python /home/diors/work/tt4it/manage.py rlistlog --key=django:logit:tt4it --filename=/tmp/tt4it.logit.log
  autostart=true
  autorestart=true
  startretries=3
  exitcodes=0,1,2
  stopsignal=KILL  # ``signal SIGQUIT/SIGINT`` can't kill as expect, Use ``signal SIGKILL``
  stopasgroup=true
  killasgroup=true
  stdout_logfile=/home/diors/supervisorlog/supervisor_rlog_access.log
  stderr_logfile=/home/diors/supervisorlog/supervisor_rlog_error.log
  user=diors
  ```

* Enter Supervisorctl

  ```shell
  supervisorctl  # Default /etc/supervisord.conf
  supervisorctl -c /home/diors/supervisord.conf  # Specify the configuration file location
  ```

* Supervisorctl Commands

  ```shell
  $ supervisorctl -c /home/diors/supervisord.conf reread
  rlog: changed
  $ supervisorctl -c /home/diors/supervisord.conf update
  rlog: stopped
  rlog: updated process group
  $ supervisorctl -c /home/diors/supervisord.conf reread
  No config updates to processes
  ```

* Alias

  * ``~/.bashrc``

    ```shell
    # Supervisor Start
    alias supervisord='supervisord -c /home/diors/supervisord.conf'
    alias supervisorctl='supervisorctl -c /home/diors/supervisord.conf'
    # Supervisor End
    ```
    *Tips: If not have effect, exec ``source ~/.bashrc``*

    * ``http://localhost:9001 refused connection``

      ```shell
      $ supervisorctl restart kuaihongbao
      http://localhost:9001 refused connection
      ```

      * [Supervisorctl not respecting my configuration](http://stackoverflow.com/questions/17005404/supervisorctl-not-respecting-my-configuration)

  * Commands

    ```shell
    $ supervisorctl reread
    rlog: changed
    $ supervisorctl update
    rlog: stopped
    rlog: updated process group
    $ supervisorctl status rlog
    rlog                             RUNNING   pid 15343, uptime 0:00:15
    ```

## Configuration

- `[program:nodehook]` - Define the program to monitor. We'll call it "nodehook".
- `command` - This is the command to run that kicks off the monitored process. We use "node" and run the "http.js" file. If you needed to pass any command line arguments or other data, you could do so here.
- `directory` - Set a directory for Supervisord to "cd" into for before running the process, useful for cases where the process assumes a directory structure relative to the location of the executed script.
- `autostart` - Setting this "true" means the process will start when Supervisord starts (essentially on system boot).
- `autorestart` - If this is "true", the program will be restarted if it exits unexpectedly.
- `startretries` - The number of retries to do before the process is considered "failed"
- `stderr_logfile` - The file to write any errors output.
- `stdout_logfile` - The file to write any regular output.
- `user` - The user the process is run as.
- `environment` - Environment variables to pass to the process.
- [``more detail``](http://supervisord.org/configuration.html)

*Tips: Supervisord won't create a directory for logs if they do not exist*

## Supervisord

```shell
$ supervisord -h
supervisord -- run a set of applications as daemons.

Usage: /usr/bin/supervisord [options]

Options:
-c/--configuration FILENAME -- configuration file
-n/--nodaemon -- run in the foreground (same as 'nodaemon true' in config file)
-h/--help -- print this usage message and exit
-v/--version -- print supervisord version number and exit
-u/--user USER -- run supervisord as this user (or numeric uid)
-m/--umask UMASK -- use this umask for daemon subprocess (default is 022)
-d/--directory DIRECTORY -- directory to chdir to when daemonized
-l/--logfile FILENAME -- use FILENAME as logfile path
-y/--logfile_maxbytes BYTES -- use BYTES to limit the max size of logfile
-z/--logfile_backups NUM -- number of backups to keep when max bytes reached
-e/--loglevel LEVEL -- use LEVEL as log level (debug,info,warn,error,critical)
-j/--pidfile FILENAME -- write a pid file for the daemon process to FILENAME
-i/--identifier STR -- identifier used for this instance of supervisord
-q/--childlogdir DIRECTORY -- the log directory for child process logs
-k/--nocleanup --  prevent the process from performing cleanup (removal of
                   old automatic child log files) at startup.
-a/--minfds NUM -- the minimum number of file descriptors for start success
-t/--strip_ansi -- strip ansi escape codes from process output
--minprocs NUM  -- the minimum number of processes available for start success
--profile_options OPTIONS -- run supervisord under profiler and output
                             results based on OPTIONS, which  is a comma-sep'd
                             list of 'cumulative', 'calls', and/or 'callers',
                             e.g. 'cumulative,callers')
```

## Supervisorctl

* supervisorctl -h

  ```shell
  $ supervisorctl -h
  supervisorctl -- control applications run by supervisord from the cmd line.

  Usage: /usr/bin/supervisorctl [options] [action [arguments]]

  Options:
  -c/--configuration -- configuration file path (default /etc/supervisor.conf)
  -h/--help -- print usage message and exit
  -i/--interactive -- start an interactive shell after executing commands
  -s/--serverurl URL -- URL on which supervisord server is listening
       (default "http://localhost:9001").
  -u/--username -- username to use for authentication with server
  -p/--password -- password to use for authentication with server
  -r/--history-file -- keep a readline history (if readline is available)

  action [arguments] -- see below

  Actions are commands like "tail" or "stop".  If -i is specified or no action is
  specified on the command line, a "shell" interpreting actions typed
  interactively is started.  Use the action "help" to find out about available
  actions.
  ```

* supervisorctl help

  ```shell
  $ supervisorctl help

  default commands (type help <topic>):
  =====================================
  add    clear  fg        open  quit    remove  restart   start   stop  update
  avail  exit   maintail  pid   reload  reread  shutdown  status  tail  version
  ```

## Web Interface

```shell
[inet_http_server]
port = 9001
username = user # Basic auth username
password = pass # Basic auth password
```

## XML-RPC Interface 

## Configuration Examples

* Python manage.py

  ```shell
  # runserver
  command=/home/diors/env/bin/python /home/diors/work/tt4it/manage.py runserver 0.0.0.0:9999

  # rlog
  command=/home/diors/env/bin/python /home/diors/work/tt4it/manage.py rlistlog --key=django:logit:tt4it --filename=/tmp/tt4it.logit.log

  # django_q
  command=/home/diors/env/bin/python /home/diors/work/tt4it/manage.py qcluster

  # beanstalk_worker
  command=/home/diors/env/bin/python /home/diors/work/tt4it/manage.py beanstalk_worker -w 5 -l debug
  ```

* uWSGI

  ```shell
  command=/home/diors/env/bin/uwsgi --ini /home/diors/work/tt4it/tt4it/uwsgi/tt4it.ini
  ```

* Gunicorn

  ```
  TODO
  ```

  *Tips: ``Daemon`` of ``Gunicorn`` should set ``False``*

* Gogs

  ```shell
  directory=/home/diors/gogs/
  command=/home/diors/gogs/gogs web
  environment=HOME="/home/diors", USER="diors"
  ```
  * environment
    * [supervisor管理gogs,需要增加environment=HOME="xxx",USER="xxx" #1695](https://github.com/gogits/gogs/issues/1695)
  * directory
    * Error: `[Macaron] PANIC: session(start): mkdir data: permission denied`
    * Causes: Gogs creates `data` subdirectory at the same directory where Gogs binary is located.
    * Solution: make sure Gogs has permission to create subdirectory at that directory.
    * [Troubleshooting](https://gogs.io/docs/intro/troubleshooting)

## Logs

* Print
  * [stdout not being captured #13](https://github.com/Supervisor/supervisor/issues/13)
    * Flush each stream after writing like `sys.stdout.flush()` or run with the streams unbuffered using `python -u`

* Logging

  ```python
  logger = logging.getLogger('xxx')
  logger.addHandler(logging.StreamHandler())
  logger.info('ooo')
  ```

* Multi Process

  ```shell
  # beanstalk_worker
  command=/home/diors/env/bin/python /home/diors/work/tt4it/manage.py beanstalk_worker -w 5 -l debug

  # Process
  diors    27842 16899  0 18:17 ?        00:00:00 /home/diors/env/bin/python manage.py beanstalk_worker -w 2
  diors    27852 27842  0 18:17 ?        00:00:00 /home/diors/env/bin/python manage.py beanstalk_worker -w 2
  diors    27853 27842  0 18:17 ?        00:00:00 /home/diors/env/bin/python manage.py beanstalk_worker -w 2
  ```

  * Python logging is not process-safe, so when start ``5`` workers for beanstalk, will miss some logs.

    ```shell
    # Testit ``-w 1``/``-w 2``
    for i in range(8): client.call('beanstalkd.background_counting', str(i))
    ```

  * When ``restart``, only master process will be killed.

    ```shell
    diors    27852 1  0 18:17 ?        00:00:00 /home/diors/env/bin/python manage.py beanstalk_worker -w 2
    diors    27853 1  0 18:17 ?        00:00:00 /home/diors/env/bin/python manage.py beanstalk_worker -w 2
    ```
  * Solution
    ```shell
    # stopsignal=QUIT
    # stopsignal=INT  # ``signal SIGQUIT`` can't kill as expect, Use the "fast" shutdown ``signal SIGINT``
    stopsignal=KILL  # ``signal SIGQUIT/SIGINT`` can't kill as expect, Use ``signal SIGKILL``
    stopasgroup=true
    killasgroup=true
    ```

## Supervisord.log

* exited
  * ``exit status 0``

  * ``exit status 1; expected``

    * Raise Error In Codes

  * ``exit status 1; not expected``

  * ``terminated by SIGQUIT``

    * stop xxx

      ```
      waiting ==> stopped
      ```

    * restart xxx

      ```
      waiting ==> stopped ==> spawned ==> success
      ```

    * xxx

  * ``terminated by SIGKILL; not expected``

    * kill -9 xxx

      ```
      exited ==> spawned ==> success
      ```

    * Other Condition Lead To Process Killed

      * [Who “Killed” my process and why?](http://stackoverflow.com/questions/726690/who-killed-my-process-and-why)

      * [Ubuntu] ``sudo vim /var/log/kern.log``

        ```shell
        Sep 21 07:36:02 iZ28e73hi9aZ kernel: [37656975.398263] Out of memory: Kill process 18738 (python) score 429 or sacrifice child
        Sep 21 07:36:02 iZ28e73hi9aZ kernel: [37656975.399284] Killed process 18738 (python) total-vm:1760008kB, anon-rss:1622680kB, file-rss:1048kB
        ```
      * [References: Leaking Memory](https://xxx-cook-book.gitbooks.io/django-cook-book/content/Performances/MEM/leaking-memory.html)

## References

[1] Supervisord@TT4IT, [Supervisord — Supervisor process control system for UNIX](http://tt4it.com/resources/discuss/154/)

[2] DigitalOcean, [How To Install and Manage Supervisor on Ubuntu and Debian VPS](https://www.digitalocean.com/community/tutorials/how-to-install-and-manage-supervisor-on-ubuntu-and-debian-vps)

[3] Servers for Hackers, [Monitoring Processes with Supervisord](https://serversforhackers.com/monitoring-processes-with-supervisord)