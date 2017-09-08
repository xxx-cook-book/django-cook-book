# Summary

* [Introduction](README.md)
* [Admins](Admins/README.md)
  * [Admin Actions](Admins/admin-actions.md)
    * Override Delete_selected()
  * [Admin Methods](Admins/admin-methods.md)
  * [Admin Options](Admins/admin-options.md)
  * [Copy An Object](Admins/copy-an-object.md)
  * [Custom Change_list Filter](Admins/custom-change_list-filter.md)
  * [Custom Site_header](Admins/custom-site_header.md)
  * [Display Extra Field](Admins/display-extra-field.md)
  * [Exec After Click Delete](Admins/exec-after-click-delete.md)
  * [Exec After Click Save](Admins/exec-after-click-save.md)
  * [Field Readonly After Save](Admins/field-readonly-after-save.md)
  * [Filter User Owned Objects](Admins/filter-user-owned-objects.md)
* [Asyns](Asyns/README.md)
  * [Beanstalkd](Asyns/Beanstalkd.md)
  * Celery
  * [Django Q](Asyns/django-q.md)
* [Auths](Auths/README.md)
  * [Password](Auths/Password.md)
  * User
* [Caches](Caches/README.md)
  * [Memcached](Caches/Memcached.md)
  * [Redis](Caches/Redis/README.md)
    * Django-redis
* [Cookies](Cookies/README.md)
  * Cookie
  * [Session](Cookies/Session.md)
* [Databases](Databases/README.md)
  * [Atomic](Databases/Atomic/README.md)
    * [F Expression Update](Databases/Atomic/f-expression-update.md)
    * [Select For Update](Databases/Atomic/select-for-update.md)
  * Connection Pool
    * DButils
  * [Emoji Support](Databases/emoji-support.md)
  * [Migrations](Databases/Migrations.md)
  * [Multiple Databases](Databases/multiple-databases.md)
  * [Persistent Connections](Databases/persistent-connections.md)
  * [Raw SQL](Databases/raw-sql.md)
* [Decorators](Decorators/README.md)
  * [Logging Request Params](Logs/logging-request-params.md)
* [Domains](Domains/README.md)
  * [Multi Domain](Domains/multi-domain.md)
* [Emails](Emails/README.md)
* [Exceptions](Exceptions/README.md)
  * [``ObjectDoesNotExist`` vs. ``DoesNotExist``](Exceptions/ObjectDoesNotExist-DoesNotExist.md)
* [Files](Files/README.md)
  - [Base64 File Upload](Files/base64-image-upload.md)
  - [Huge File Upload](Files/huge-file-upload.md)
* Forms
* [Logs](Logs/README.md)
  * [Handlers](Logs/Handlers/README.md)
    * [FileHandler](Logs/Handlers/FileHandler/file-handler.md)
      * [ConcurrentLogHandler](Logs/Handlers/FileHandler/concurrent-log-handler.md)
      * [RotatingFileHandler](Logs/Handlers/FileHandler/rotating-file-handler.md)
      * [TimedRotatingFileHandler](Logs/Handlers/FileHandler/timed-rotating-file-handler.md)
      * [WatchedFileHandler](Logs/Handlers/FileHandler/watched-file-handler.md)
    * RedisHandler
      * [RedisHandler](Logs/Handlers/RedisHandler/redis-handler.md)
      * [RedisListHandler](Logs/Handlers/RedisHandler/redis-list-handler.md)
    * [SocketHandler](Logs/Handlers/SocketHandler/socket-handler.md)
    * [StreamHandler](Logs/Handlers/StreamHandler/stream-handler.md)
* [Middlewares](Middlewares/README.md)
  * [Cross-Origin Resource Sharing (CORS)](Middlewares/CORS.md)
  * [URL Parse](Middlewares/url-parse.md)
  * [User Agent Detect](Middlewares/user-agent-detect.md)
* [Models](Models/README.md)
  * [Abstract Base Classes](Models/abstract-base-classes.md)
  * PrimaryKey/ForeignKey/Index/Unique
    * db_index/index_together
    * unique/unique_together
  * [Fields](Models/Fields/README.md)
    * [JSONField](Models/Fields/JSONField.md)
    * [ShortUUIDField](Models/Fields/ShortUUIDField.md)
  * [Options](Models/Options/README.md)
    * abstract
    * db_table
    * [managed](Models/Options/managed.md)
* [Performances](Performances/README.md)
  * [Memory](Performances/Memory/README.md)
    * [Leaking Memory](Performances/Memory/leaking-memory.md)
* [Queries](Queries/README.md)
  * [Extra/Aggregation](Queries/extra-aggregation.md)
  * [F Expression](Queries/FExpression.md)
  * [Group By](Queries/GroupBy.md)
  * Last/Latest
  * [Q Expression](Queries/QExpression.md)
  * [Variable Field](Queries/variable-field.md)
  * Year/Month/Day
* [Real-times](Real-times/README.md)
  * [Long Pooling](Real-times/LongPooling/README.md)
    * [gevent](Real-times/LongPooling/gevent.md)
  * [HTTP Streaming](Real-times/HTTPStreaming/README.md)
    * [gevent](Real-times/HTTPStreaming/gevent.md)
  * WebSocket
    * dwebsocket
    * Node.js — Socket.io
  * Channals
  * Push
* [Requests](Requests/README.md)
* [Responses](Responses/README.md)
  * [ExcelResponse](Responses/ExcelResponse.md)
  * [FileResponse](Responses/FileResponse.md)
  * [Http404](Responses/Http404.md)
  * [HttpResponse](Responses/HttpResponse.md)
  * [JsonResponse](Responses/JsonResponse.md)
    * [DjangoJSONEncoder](Responses/DjangoJSONEncoder.md)
    * [Encapsulation](Responses/Encapsulation.md)
  * StreamingHttpResponse
* [Settings](Settings/README.md)
  * [ALLOWED_HOSTS](Settings/allowed-hosts.md)
* [Signals](Signals/README.md)
* StaticFiles
* [Subcommands](Subcommands/README.md)
  * [BaseCommand](Subcommands/BaseCommand.md)
* [Tags and Filters](TagsAndFilters/README.md)
  * [3rd](TagsAndFilters/3rd/README.md)
    * [django-mathfilters — provides a set of simple math filters for Django](TagsAndFilters/3rd/django-mathfilters.md)
  * [Built-in](TagsAndFilters/Built-in.md)
  * [DIY](TagsAndFilters/DIY.md)
* Templates
  * cached.Loader
* Translations
* [URLs](URLs/README.md)
  * [Pass Extra Params to Views](URLs/pass-extra-params-to-views.md)
  * [Regexes](URLs/regexes.md)

## Solution

* [Pagination](Solutions/Pagination.md)
* [Verification Code](Solutions/verification-code.md)

## Appendix I

* [Process Managers](ProcessManagers/README.md)
  * [Supervisor](ProcessManagers/Supervisor.md)
* [Setting Ups](SettingUps/README.md)
  * WSGI
    * Nginx & mod_wsig
    * [Nginx & uWSGI](SettingUps/nginx-uwsgi.md)
    * [Nginx & Gunicorn](SettingUps/nginx-gunicorn.md)
  * ~~FastCGI - Deprecated since version 1.7 & Remove in version 1.9~~
* [VS.](VS./README.md)
  * [``exists()`` vs. ``get() + try/except``](VS./exists-or-get-try-except.md)
  * [``exists()`` vs. ``count()``](VS./exists-or-count.md)

## Appendix II

* [Design](Design/README.md)
    * [Design](Design/lastrowid.md)

