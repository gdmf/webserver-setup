
uWSGI Emperor with Upstart
-------------

[Docs:](http://uwsgi-docs.readthedocs.org/en/latest/Upstart.html)
The configuration file lives at /etc/init/uwsgi.conf. 

    # uWSGI - manage uWSGI application server
    
    description "uWSGI Emperor"
    
    start on runlevel [2345]
    stop on runlevel [06]
    
    respawn
    
    env LOGTO=/var/log/uwsgi.log
    env BINPATH=/usr/bin/uwsgi
    
    exec $BINPATH --emperor /home/<user>/projects/*/conf/uwsgi.ini --logto $LOGTO

This will search all directories under 'projects' for a 'conf' directory containing a uwsgi.ini file.

[uWSGI options:](http://uwsgi-docs.readthedocs.org/en/latest/Options.html)

Other configuration locations:
/etc/uwsgi/apps-enabled     #currently empty
/etc/uwsgi/apps-available     #currently empty
unused: /etc/uwsgi/vassals/vassal.ini   #test ini

