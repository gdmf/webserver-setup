Django setup
------------
Create a project directory

    $ cd ~/projects
    $ mkdir myproject

Create a virtualenv

    $ cd myproject
    $ virtualenv env

Install django and create a django project

    $ source env/bin/activate
    (env)$ pip install django
    (env)$ django-admin.py startproject mysite

Add nginx/uwsgi configuration files

    $ mkdir conf
    $ sudo nano nginx.conf
    $ sudo nano uwsgi.conf

Be sure to add the include statement for nginx.conf into /etc/nginx/sites-enabled/apps.conf

Create folders for media and static

    $ cd /home/<user>/projects/<project>/<project>
    $ mkdir media
    $ mkdir static

Add STATIC_ROOT

    $ sudo nano ~/projects/<project>/<app>/<app>/settings.py
    STATIC_ROOT = '/home/<user>/projects/<project>/<app>/static/'

Run the collectstatic management command

    $ python manage.py collectstatic

Assuming you have set an alias, nginx will now serve the static files, e.g.:

  http://example.com/media/286.jpg

  http://example.com/static/test.css

Security settings (in settings.py)
    
```Python
DEBUG = False
ALLOWED_HOSTS = ['example.com', 'www.example.com']
secret_key_file = 'dir/file.txt'
with open(secret_key_file) as f:
    SECRET_KEY = f.read().strip()
```




