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
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;
    include /etc/nginx/sites-enabled/*;
}
```

Server context
--------------
Configured in /etc/nginx/sites-enabled/apps.conf. Additional location contexts can be stored as .conf files in project folders, and incorporated through include statements.

```Nginx
# insert include here
include /home/<user>/projects/<project>/conf/nginx.conf;

server {
    listen 80;
    server_name 50.56.184.237;
    root /home/<user>/nginx/www;
    location / {
        try_files $uri.html $uri $uri/ =404;
    }
}
```

Location context
----------------
I want to serve multiple webapps using the same server. This template assumes the use of subdomains (a.mysite.com, b.mysite.com, etc.). Use the template to create /home/[user]/projects/[project]/conf/nginx.conf: 

```Nginx
server {
    listen 80;
    server_name b.mysite.com, b.www.gdmf.me;

    # Django media/static
    location /media {
        alias /home/<user>/projects/<project>/mysite/media;
    }

    location /static {
        alias /home/<user>/projects/<project>/mysite/static;
    }

    location / {
        include uwsgi_params;
        uwsgi_pass unix://tmp/<project>.sock;
    }
}
```

Restarting the service:

    sudo service nginx stop
    sudo service nginx start

