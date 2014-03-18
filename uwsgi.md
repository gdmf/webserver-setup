Annotated uWSGI .ini file

```ini
; uwsgi.ini template (<var> changes per project)
[uwsgi]

; Enable uWSGI master process
master = true

; more processes = more concurrency but more memory
processes = 4

; try to remove generated files/sockets upon exit
vacuum = true

; specify unix socket on the filesystem
socket = /tmp/<app2>.socket
; ALT: bind on a tcp socket
; socket = 127.0.0.1:3031

;set unix socket permissions (default = 666)
chmod-socket = 666

; set uid/gid
uid = www-data
gid = www-data

; load uWSGI python plugin
plugins = python

; path to project directory
chdir = /home/grant/projects/project2

; add directory to python search path (can be specified up to 64 times)
pythonpath = /home/grant/projects/project2
; ALT: pythonpath = ..
; ALT: pythonpath = /home/grant/projects

; loads WSGI module as application. The module (sans .py) must be importable (ie in pythonpath)
module = wsgi

; set default WSGI callable name
callable = application

; set pythonhome/virtualenv to search for Python modules in a specific virtualenv
; virtualenv (ALT, venv) = 

; reload uWSGI if specified file or directory is modified/touched
touch-reload = /home/grant/projects/project2/reload

; log
logto = /home/grant/projects/project2/logs/uwsgi.log

; trigger logroatation if the specified file is modified/touched
touch-logrotate = /home/grant/projects/project2/logs/logrotate

; set environment variable
env = DJANGO_SETTINGS_MODULE=<my_project>.settings

; respawn processes after more than 20 seconds
harakiri = 20

; QUESTIONS
; what is this? old? module = django.core.handlers.wsgi:WSGIHandler()
```