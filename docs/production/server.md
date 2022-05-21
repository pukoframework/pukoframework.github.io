---
layout: default
title: Server
parent: Production
nav_order: 1
---

## Apache

You can set *clean url* with apache with the following `.htaccess` configuration:

```text
<IfModule mod_rewrite.c>

RewriteEngine On

RewriteCond %{REQUEST_URI} !(public|css)
RewriteCond %{REQUEST_URI} !(\.css|\.js|\.png|\.jpg|\.gif|robots\.txt)$ [NC]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d

RewriteRule  ^(.+)?$ index.php?request=$1&lang=$2 [L,QSA]

</IfModule>
```

## Docker


Configuration template for ```Dockerfile```


```bash
FROM alpine:3.11

# Install packages
RUN apk --no-cache add php7 php7-session php7-fpm php7-pdo php-pdo_mysql \
    php7-mysqli php7-json php7-openssl php7-curl php7-zlib php7-xml \
    php7-phar php7-intl php7-dom php7-xmlreader php7-ctype \
    php7-mbstring php7-gd php7-soap php7-ldap nginx supervisor curl git

# Configure nginx
COPY bootstrap/nginx.conf /etc/nginx/nginx.conf

# Configure PHP-FPM
COPY bootstrap/fpm-pool.conf /etc/php7/php-fpm.d/zzz_custom.conf
COPY bootstrap/php.ini /etc/php7/conf.d/zzz_custom.ini

# Configure supervisord
COPY bootstrap/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Add application
RUN mkdir -p /var/www/html
WORKDIR /var/www/html
COPY . /var/www/html/

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer
RUN composer install

EXPOSE 80 443
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
```

Template ```supervisord.conf```

```bash
[supervisord]
nodaemon=true

[program:php-fpm]
command=php-fpm7 -F
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0
autorestart=false
startretries=0

[program:nginx]
command=nginx -g 'daemon off;'
stdout_logfile=/dev/stdout
stdout_logfile_maxbytes=0
stderr_logfile=/dev/stderr
stderr_logfile_maxbytes=0
autorestart=false
startretries=0
```

Template ```php.ini```

```bash
[Date]
date.timezone="Asia/Jakarta"

[Memory]
memory_limit=512M

[Upload]
upload_max_filesize=50M
```

Template ```nginx.conf```

```bash
worker_processes  1;
pid /run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    log_format  main_timed  '$remote_addr - $remote_user [$time_local] "$request" '
                            '$status $body_bytes_sent "$http_referer" '
                            '"$http_user_agent" "$http_x_forwarded_for" '
                            '$request_time $upstream_response_time $pipe $upstream_cache_status';

    access_log /dev/stdout main_timed;
    error_log /dev/stderr notice;

    keepalive_timeout  65;

    server {
        listen [::]:80 default_server;
        listen 80 default_server;
        server_name _;

        client_max_body_size 50M;
        sendfile on;

        root /var/www/html;
        index index.php index.html;

        location / {
            # First attempt to serve request as file, then
            # as directory, then fall back to index.php
            rewrite ^/(.*)$ /index.php?request=$1 last;
            try_files $uri $uri/ =404;
        }

        location = /favicon.ico {
            access_log off;
            log_not_found off;
        }

        location = /robots.txt {
            access_log off;
            log_not_found off;
        }

        # redirect server error pages to the static page /50x.html
        #
        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
            root /var/lib/nginx/html;
        }

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        location ~ \.php$ {
            try_files $uri =404;
            fastcgi_split_path_info ^(.+\.php)(/.+)$;
            fastcgi_pass  127.0.0.1:9000;
            fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
            fastcgi_param SCRIPT_NAME $fastcgi_script_name;
            fastcgi_index index.php;
            include fastcgi_params;
        }

        location ~* \.(jpg|jpeg|gif|png|css|js|ico|xml|xls|xlsx|svg|doc|docx|txt|ttf|eot|woff)$ {
            add_header Access-Control-Allow-Origin *;
            expires 5d;
        }

        # deny access to . files, for security
        #
        location ~ /\. {
            log_not_found off;
            deny all;
        }
    }
}
```

Template ```fpm-pool.conf```


```bash
[global]
; Log to stderr
error_log = /dev/stderr

[www]
; Enable status page
pm.status_path = /fpm-status

; Ondemand process manager
pm = ondemand

; The number of child processes to be created when pm is set to 'static' and the
; maximum number of child processes when pm is set to 'dynamic' or 'ondemand'.
; This value sets the limit on the number of simultaneous requests that will be
; served. Equivalent to the ApacheMaxClients directive with mpm_prefork.
; Equivalent to the PHP_FCGI_CHILDREN environment variable in the original PHP
; CGI. The below defaults are based on a server without much resources. Don't
; forget to tweak pm.* to fit your needs.
; Note: Used when pm is set to 'static', 'dynamic' or 'ondemand'
; Note: This value is mandatory.
pm.max_children = 50

; The number of seconds after which an idle process will be killed.
; Note: Used only when pm is set to 'ondemand'
; Default Value: 10s
pm.process_idle_timeout = 10s;

; The number of requests each child process should execute before respawning.
; This can be useful to work around memory leaks in 3rd party libraries. For
; endless request processing specify '0'. Equivalent to PHP_FCGI_MAX_REQUESTS.
; Default Value: 0
pm.max_requests = 500

; Make sure the FPM workers can reach the environment variables for configuration
clear_env = no

; Catch output from PHP
catch_workers_output = yes
```

## Nginx

You can set *clean url* with nginx with the following `nginx.conf` configuration:

```text
server {
    listen   8000;
    server_name  localhost;

    client_max_body_size  100m;

    root   /home/www_puko;
    index index.php index.html index.htm;

    location = /favicon.ico {
        access_log off;
        log_not_found off;
    }

    location = /robots.txt {
        access_log off;
        log_not_found off;
    }

    location ~* (\.css|\.js|\.png|\.jpg|\.gif|robots\.txt|\.eot|\.ttf|\.woff)$ { 
        add_header Access-Control-Allow-Origin *;
    }
  
    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php-fpm/php-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location / {
        rewrite ^/(.*)$ /index.php?request=$1 last;
        try_files $uri $uri/ =404;
    }

}
```

If you want to set *reverse proxy* you can use the following sample:

```text
server
{
  server_name anywhere.com;
  location /
  {    
    proxy_pass  http://localhost:8000;
    include conf.d/proxy_header;
  }
}
```

Template *proxy_header*

```text
### force timeouts if one of backend is died ##
proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;

set	$scheme_to_sn	"://";
set	$after_sn	"/";
 
### Set headers ####
proxy_set_header	Accept-Encoding	    "";
proxy_set_header	Host			    $host;
proxy_set_header	X-Proxy-Target	    $proxy_host;
proxy_set_header	X-Proxy-Port	    $proxy_port;
proxy_set_header	X-Real-IP		    $remote_addr;
proxy_set_header	X-Forwarded-For	    $proxy_add_x_forwarded_for;
proxy_set_header	X-Forwarded-Proto	$scheme;
proxy_set_header	X-Gateway		    $server_addr;
proxy_set_header	X-Development-Server	"true";
proxy_set_header	X-Server-Name		$server_name;
proxy_set_header	X-Site-For		    "";
proxy_set_header	X-Source-Access	    "internal";
proxy_set_header	App-Base-URI		$scheme$scheme_to_sn$server_name$after_sn;
		
add_header		    Front-End-Https on;

proxy_redirect		off;

proxy_no_cache 		$cookie_PHPSESSID;
proxy_cache_bypass 	$cookie_PHPSESSID;
```

## Puko (Development)

You can run *Puko* without using a web server. Use *command* below to run it directly:

```text
php puko serve ...
```

You can change ... with the desired port.

```text
php puko serve 4000
```

Then, you can open it on a web browser:

```text
localhost:4000
```

## Vagrant (Development)


Configuration file for `Vagrantfile`

```bash
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # Box Settings
  config.vm.box = "ubuntu/trusty64"
  config.vm.boot_timeout = 600

  # Provider Settings
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 700
    vb.cpus = 2
    vb.name = "puko"
  end

  # Network Settings
  config.vm.network "private_network", ip: "192.168.33.10", auto_correct: true

  # Database Settings
  config.vm.network "forwarded_port", guest: 3306, host: 3306, auto_correct: true

  # Folder Settings
  config.vm.synced_folder ".", "/home/www_app", :nfs => { :mount_options => ["dmode=777", "fmode=666"] }

  # Hosts Settings
  if defined?(VagrantPlugins::HostsUpdater)
    config.vm.hostname = "puko.com"
    config.hostsupdater.aliases = [
      "www.puko.com"
    ]
  end

  # Provision Settings
  config.vm.provision "shell", path: "bootstrap.sh"
end
```

Provisioning with ```bootstrap.sh```

```bash
# Update Packages
apt-get update
# Upgrade Packages
apt-get upgrade

# Basic Linux Stuff
apt-get install -y git

# Apache
apt-get install -y nginx

#Add Onrej PPA Repo
apt-add-repository ppa:ondrej/php
apt-get update

# Install Memcached
apt-get install memcached

# Install PHP
apt-get install -y php7.2

# PHP Mods
apt-get install -y php7.2-common
apt-get install -y php7.2-cli
apt-get install -y php7.2-cgi
apt-get install -y php7.2-soap
apt-get install -y php7.2-curl
apt-get install -y php7.2-fpm
apt-get install -y php7.2-zip
apt-get install -y php7.2-gd
apt-get install -y php7.2-json
apt-get install -y php7.2-ldap
apt-get install -y php7.2-mcrypt
apt-get install -y php7.2-mbstring
apt-get install -y php7.2-xml
apt-get install -y php7.2-xmlrpc
apt-get install -y php7.2-memcached
apt-get install -y php7.2-zip

# Install Composer
curl -Ss https://getcomposer.org/installer | php
sudo mv composer.phar /usr/bin/composer

# Set MySQL Pass
debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'
debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'

# Install MySQL
apt-get install -y mysql-server

# PHP-MYSQL lib
apt-get install -y php7.2-mysql

# Configure host
cat << 'EOF' > /etc/nginx/sites-available/default
server {
	# Port that the web server will listen on.
	listen 80;

	# Host that will serve this project.
	server_name localhost;

    # Client body size
	client_max_body_size  100m;

	# The location of our projects public directory.
	root /home/www_app;

	# Point index to the Laravel front controller.
	index index.php index.html;

	location = /favicon.ico {
        access_log off;
        log_not_found off;
    }

    location = /robots.txt {
        access_log off;
        log_not_found off;
    }

    location ~* (\.css|\.js|\.png|\.jpg|\.gif|robots\.txt|\.eot|\.ttf|\.woff|\.xlsx)$ {
        add_header Access-Control-Allow-Origin *;
    }

    location ~ \.php$ {
        try_files $uri =404;
        include /etc/nginx/fastcgi_params;
        fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    }

    location / {
        rewrite ^/(.*)$ /index.php?request=$1 last;
        try_files $uri $uri/ =404;
    }

}
EOF

# Restart Service
sudo service nginx restart
```

Additional steps for make MySQL can have *remote access*

```bash
vagrant ssh

vim /etc/mysql/my.cnf

bind-address = 127.0.0.1

:w !sudo tee %

mysql -u root -p

GRANT ALL PRIVILEGES ON *.* TO `root`@`%` IDENTIFIED BY 'root' WITH GRANT OPTION;

FLUSH PRIVILEGES;

exit;

sudo service mysql restart
```
