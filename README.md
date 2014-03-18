webserver-setup
===============

The basic idea
--------------
Serve out webapps (Django, Flask, what-have-you) using nginx and uwsgi. Apps are differentiated by using subfolders (could have used subdomains; didn't).

Nginx handles incoming requests, and hands them off to uwsgi. uWSGI then feeds the requests and returns responses from Django/Flask/etc. My goal has been to minimize the setup required for each new webapp.

The workflow
------------
1. Install the webapp (Django or Flask project)
2. Using the nginx location block template, customize it for a subdirectory and a socket, and incorporate using an include directive in /etc/nginx/sites-enabled/apps.conf
3. Using the uwsgi.ini template, customzie for the webapp, and add to ../projects/<project>/conf/
4. Reload uwsgi


