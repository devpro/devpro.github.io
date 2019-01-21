# Symfony on Windows

## Local setup

## Problem with response time

- xdebug is realy slow = 3000ms
- without xdebug and symfony web dev server = 800ms
- without xdebug and with  nginx/fastcgi = 300ms

## Configuration

### Reference

- [PHP-FastCGI on Windows (nginx)](https://www.nginx.com/resources/wiki/start/topics/examples/phpfastcgionwindows/)
- [Configuring a Web Server (Symfony)](https://symfony.com/doc/current/setup/web_server_configuration.html)

### Configuration steps

- Unzip nginx file
- Edit conf file
- Launch the command from a command window inside PHP folder: `php-cgi -b 127.0.0.1:9000`
- Double click on `nginx.exe`

```conf
# D:\Programs\nginx-1.15.7\conf\nginx.conf
server {
    listen       8000;
    server_name  localhost;
    root D:/my/path/public;

    location / {
      # try to serve file directly, fallback to index.php
      try_files $uri /index.php$is_args$args;
    }

    location ~ ^/index\.php(/|$) {
      fastcgi_pass 127.0.0.1:9000;
      fastcgi_split_path_info ^(.+\.php)(/.*)$;
      include fastcgi_params;

      # optionally set the value of the environment variables used in the application
      # fastcgi_param APP_ENV prod;
      # fastcgi_param APP_SECRET <app-secret-id>;
      # fastcgi_param DATABASE_URL "mysql://db_user:db_pass@host:3306/db_name";

      # When you are using symlinks to link the document root to the
      # current version of your application, you should pass the real
      # application path instead of the path to the symlink to PHP
      # FPM.
      # Otherwise, PHP's OPcache may not properly detect changes to
      # your PHP files (see https://github.com/zendtech/ZendOptimizerPlus/issues/126
      # for more information).
      fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
      fastcgi_param DOCUMENT_ROOT $realpath_root;
      # Prevents URIs that include the front controller. This will 404:
      # http://domain.tld/index.php/some-path
      # Remove the internal directive to allow URIs like this
      internal;
    }

    # return 404 for all other php files not matching the front controller
    # this prevents access to other php files you don't want to be accessible.
    location ~ \.php$ {
      return 404;
    }

    #error_log /var/log/nginx/project_error.log;
    #access_log /var/log/nginx/project_access.log;
  }
```
