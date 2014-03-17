
uwsgi emperor
lives at /etc/init/uwsgi.conf

#######################################################
# uWSGI - manage uWSGI application server
#

description		"uWSGI Emperor"

start on runlevel [2345]
stop on runlevel [06]

respawn

env LOGTO=/var/log/uwsgi.log
env BINPATH=/usr/bin/uwsgi

exec $BINPATH --emperor /home/grant/apps/vassals/ --logto $LOGTO
#######################################################


uwsgi options
http://uwsgi-docs.readthedocs.org/en/latest/Options.html



tutorials
http://eshlox.net/en/2012/09/11/nginx-uwsgi-virtualenv-and-django-ubuntu-1204/
http://bartek.im/blog/2012/07/08/simplicity-nginx-uwsgi-deployment.html
http://uwsgi.readthedocs.org/en/latest/tutorials/Django_and_nginx.html

uWSGI
/etc/uwsgi/apps-enabled     #currently empty
/etc/uwsgi/apps-available     #currently empty
/etc/uwsgi/vassals/vassal.ini   #test ini

uWSGI upstart
/etc/init/uwsgi.conf
#######################
# uWSGI - manage uWSGI application server
# 

description		"uWSGI Emperor"

start on runlevel [2345]
stop on runlevel [06]

respawn

env LOGTO=/var/log/uwsgi.log
env BINPATH=/usr/bin/uwsgi

exec $BINPATH --emperor /home/grant/projects/*/conf/uwsgi.ini --logto $LOGTO
######################