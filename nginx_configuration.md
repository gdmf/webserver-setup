## Updating nginx

    $ nginx -v
    nginx version: nginx/1.1.19

# add official nginx repo
$ sudo nano /etc/apt/sources.list
# add deb http://nginx.org/packages/ubuntu/ precise nginx

# add public key for package from ubuntu server
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABF5BD827BD9BF62

# remove current nginx install
$ sudo apt-get remove nginx-full nginx-common

# install from new repo (keep current config files)
$ sudo apt-get install nginx
$ nginx -v
nginx version: nginx/1.4.6

nginx config
/etc/nginx/sites-available

create symblinks to enable
cd /etc/nginx/sites-enabled
ln -s ../sites-avaliable/<nginx conf file>

########################################################
server {
	listen 80;
	server_name www.example.com
	access_log /var/log/nginx/<name>.log
	error_log /varl/log/nginx/<name>.log

	location / {
		uwsgi_pass	unix://tmp/www.example.com.sock;
		include		uwsgi_params;
	}

	location /media/ {
		alias /home/......./project/media/;
	}

	location /static/ {
		alias /home/......./project/static/;
	}
}
#######################################################

uwsgi config:
/etc/uwsgi/apps-available/<name>.ini

#######################################################
[uwsgi]
vhost = true
plugins = python
socket = /tmp/<name>.sock
master = true
enable-threads = true
processes = 2
wsgi-file = /home/.....project/project/wsgi.py
virtualenv = /home/....projects/<name>
chdir = /home/.......project/
touch-reload = /home/......project/reload
####################################################

enable
cd /etc/uwsgi/apps-enabled/
ls -s ../apps-availablle/<name>.ini

sudo service nginx start
sudo service uwsgi start