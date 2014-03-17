Updating nginx
--------------
Current version:

    $ nginx -v
    nginx version: nginx/1.1.19

Add official nginx repo

    $ sudo nano /etc/apt/sources.list
    deb http://nginx.org/packages/ubuntu/ precise nginx

Add public key for package from ubuntu server

    $ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABF5BD827BD9BF62

Remove current nginx install

    $ sudo apt-get remove nginx-full nginx-common

Install from new repo (keep current config files)

    $ sudo apt-get install nginx
    $ nginx -v
    nginx version: nginx/1.4.6

My Configuration
-------------
Configuration files are stored here by default:
* /etc/nginx/nginx.conf
* /etc/nginx/sites-available/*.conf

Create symlinks to enable sites-available

    $ cd /etc/nginx/sites-enabled
    $ ln -s ../sites-avaliable/<nginx conf file>

http context
------------
Configured in /etc/nginx/nginx.conf. Server contexts can be store as a .conf file in sites-enabled, and incorporated through an include statement.

```Nginx
user www-data;
worker_processes 4;
pid /var/run/nginx.pid;
events {
    worker_connections 768;
}
http {
    ...
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;
    ...
    include /etc/nginx/conf.d/*.conf;       # currently empty
    include /etc/nginx/sites-enabled/*;
}
```

Server context
--------------
Configured in /etc/nginx/sites-enabled/apps.conf. Additional location contexts can be stored as .conf files in project folders, and incorporated through include statements.

```Nginx
server {
    listen 80;
    server_name 50.56.184.237;
    root /home/grant/nginx/www;
    location / {
        try_files $uri.html $uri $uri/ =404;
    }
    include /path/to/project/conf/<app-x>.conf;
}
```

Location context
----------------
I want to serve multiple webapps using the same server. One solution is to use subdomains (webapp1.mysite.com, webapp2.mysite.com, etc.). Another solution is to use subfolders (mysite.com/webapp1, mysite.com/webapp2, etc.). The following location block shows how to do the latter. 

```Nginx
location /<app-x>/ {
    include uwsgi_params;
    uwsgi_param SCRIPT_NAME /<app-x>;
    uwsgi_modifier1 30;
    uwsgi_pass unix://tmp/<app-x>.sock;
}
```

From http://uwsgi-docs.readthedocs.org/en/latest/Nginx.html#dynamic-apps:
>...SCRIPT\_NAME is the variable used to select a specific application. The uwsgi\_modifer1 30 option sets the uWSGI modifier UWSGI_MODIFIER_MANAGE_PATH_INFO. This per-request modifier instructs the uWSGI server to rewrite the PATH_INFO value removing the SCRIPT_NAME from it.

Restarting the service:

    sudo service nginx stop
    sudo service nginx start

