---
layout: default
title: Server
parent: Production
nav_order: 1
---

# Server
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Apache

To enable clean URLs on an Apache web server, use the following `.htaccess` configuration:

```apache
<IfModule mod_rewrite.c>

RewriteEngine On

RewriteCond %{REQUEST_URI} !(public|css)
RewriteCond %{REQUEST_URI} !(\.css|\.js|\.png|\.jpg|\.gif|robots\.txt)$ [NC]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d

RewriteRule ^(.+)?$ index.php?request=$1&lang=$2 [L,QSA]

</IfModule>
```

## Docker

Below is a configuration template for a `Dockerfile` based on Alpine Linux:

```dockerfile
FROM alpine:3.11

# Install required packages
RUN apk --no-cache add php7 php7-session php7-fpm php7-pdo php-pdo_mysql \
    php7-mysqli php7-json php7-openssl php7-curl php7-zlib php7-xml \
    php7-phar php7-intl php7-dom php7-xmlreader php7-ctype \
    php7-mbstring php7-gd php7-soap php7-ldap nginx supervisor curl git

# Configure Nginx
COPY bootstrap/nginx.conf /etc/nginx/nginx.conf

# Configure PHP-FPM
COPY bootstrap/fpm-pool.conf /etc/php7/php-fpm.d/zzz_custom.conf
COPY bootstrap/php.ini /etc/php7/conf.d/zzz_custom.ini

# Configure supervisord
COPY bootstrap/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Set up application directory
RUN mkdir -p /var/www/html
WORKDIR /var/www/html
COPY . /var/www/html/

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer
RUN composer install

EXPOSE 80 443
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]
```

### supervisord.conf Template

```ini
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

### php.ini Template

```ini
[Date]
date.timezone="Asia/Jakarta"

[Memory]
memory_limit=512M

[Upload]
upload_max_filesize=50M
```

### nginx.conf Template

```nginx
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
            # Attempt to serve request as a file, then as a directory, 
            # then fall back to index.php for clean URLs.
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

        error_page 500 502 503 504 /50x.html;
        location = /50x.html {
            root /var/lib/nginx/html;
        }

        # Pass PHP scripts to FastCGI server
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

        # Deny access to hidden files for security
        location ~ /\. {
            log_not_found off;
            deny all;
        }
    }
}
```

### fpm-pool.conf Template

```ini
[global]
error_log = /dev/stderr

[www]
pm.status_path = /fpm-status
pm = ondemand

; The maximum number of child processes to be created.
pm.max_children = 50

; The number of seconds after which an idle process will be killed.
pm.process_idle_timeout = 10s;

; The number of requests each child process should execute before respawning.
pm.max_requests = 500

clear_env = no
catch_workers_output = yes
```

## Nginx

To enable clean URLs on an Nginx web server, use the following `nginx.conf` configuration:

```nginx
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

### Reverse Proxy Configuration

If you want to set up a reverse proxy with Nginx, use the following example:

```nginx
server {
    server_name anywhere.com;
    
    location / {    
        proxy_pass  http://localhost:8000;
        include conf.d/proxy_header;
    }
}
```

#### proxy_header Template

```nginx
### Force timeouts if backend is down ###
proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;

set $scheme_to_sn "://";
set $after_sn "/";
 
### Set headers ###
proxy_set_header Accept-Encoding "";
proxy_set_header Host $host;
proxy_set_header X-Proxy-Target $proxy_host;
proxy_set_header X-Proxy-Port $proxy_port;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Gateway $server_addr;
proxy_set_header X-Development-Server "true";
proxy_set_header X-Server-Name $server_name;
proxy_set_header X-Site-For "";
proxy_set_header X-Source-Access "internal";
proxy_set_header App-Base-URI $scheme$scheme_to_sn$server_name$after_sn;
		
add_header Front-End-Https on;

proxy_redirect off;

proxy_no_cache $cookie_PHPSESSID;
proxy_cache_bypass $cookie_PHPSESSID;
```

## Puko (Development Server)

You can run the Puko Framework without a standalone web server using its built-in development server. Use the command below to start it:

```bash
php puko serve
```

By default, it runs on port 8080. You can specify a custom port as follows:

```bash
php puko serve 4000
```

Then, access your application in a web browser at `localhost:4000`.

## Vagrant (Development)

Below is a sample `Vagrantfile` configuration:

```ruby
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

  # Port Forwarding for Database Access
  config.vm.network "forwarded_port", guest: 3306, host: 3306, auto_correct: true

  # Folder Syncing
  config.vm.synced_folder ".", "/home/www_app", :nfs => { :mount_options => ["dmode=777", "fmode=666"] }

  # Hostname Settings
  if defined?(VagrantPlugins::HostsUpdater)
    config.vm.hostname = "puko.com"
    config.hostsupdater.aliases = ["www.puko.com"]
  end

  # Provisioning
  config.vm.provision "shell", path: "bootstrap.sh"
end
```

### Provisioning Script (bootstrap.sh)

```bash
#!/bin/bash

# Update and upgrade packages
apt-get update
apt-get upgrade -y

# Install essential tools
apt-get install -y git curl software-properties-common

# Install Nginx
apt-get install -y nginx

# Add PHP PPA
apt-add-repository ppa:ondrej/php
apt-get update

# Install Memcached
apt-get install -y memcached

# Install PHP 7.2 and extensions
apt-get install -y php7.2 php7.2-common php7.2-cli php7.2-cgi php7.2-fpm \
    php7.2-soap php7.2-curl php7.2-zip php7.2-gd php7.2-json php7.2-ldap \
    php7.2-mbstring php7.2-xml php7.2-xmlrpc php7.2-memcached php7.2-mysql

# Install Composer
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/bin/composer

# Set MySQL root password
debconf-set-selections <<< 'mysql-server mysql-server/root_password password root'
debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password root'

# Install MySQL
apt-get install -y mysql-server

# Configure Nginx site
cat << 'EOF' > /etc/nginx/sites-available/default
server {
    listen 80;
    server_name localhost;

    client_max_body_size 100m;
    root /home/www_app;
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

# Restart Nginx
service nginx restart
```

### Enabling Remote MySQL Access

To allow remote connections to the MySQL database within the Vagrant box:

1. SSH into the box: `vagrant ssh`
2. Edit `/etc/mysql/my.cnf` and change `bind-address` to `0.0.0.0`.
3. Save changes: `:w !sudo tee %`
4. Log into MySQL: `mysql -u root -p`
5. Run the following SQL commands:
   ```sql
   GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'root' WITH GRANT OPTION;
   FLUSH PRIVILEGES;
   EXIT;
   ```
6. Restart MySQL: `sudo service mysql restart`
