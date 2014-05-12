```Nginx
location /<app-x>/ {
    include uwsgi_params;
    uwsgi_param SCRIPT_NAME /<app-x>;
    uwsgi_modifier1 30;
    uwsgi_pass unix://tmp/<app-x>.sock;
}
```


From the [uwsgi docs](http://uwsgi-docs.readthedocs.org/en/latest/Nginx.html#dynamic-apps):
>...SCRIPT\_NAME is the variable used to select a specific application. The uwsgi\_modifer1 30 option sets the uWSGI modifier UWSGI_MODIFIER_MANAGE_PATH_INFO. This per-request modifier instructs the uWSGI server to rewrite the PATH_INFO value removing the SCRIPT_NAME from it.