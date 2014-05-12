Minimal uWSGI .ini file (configured for Django)
-------------------------

```ini
[uwsgi]
master = true
processes = 4
socket = /tmp/<project>.sock
uid = www-data
gid = www-data
virtualenv = /home/<user>/projects/project/env/
chdir = /home/<user>/projects/<project>/<app>
module = <app>.wsgi:application
env = DJANGO_SETTINGS_MODULE=<app>.settings
```
