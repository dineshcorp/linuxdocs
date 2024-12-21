# Nginx Default Server Configuration For SSL

This document provides the default configuration file for an Nginx server, along with detailed explanations for each directive.

---

## Default Server Configuration
### Copy and paste this nginx default file configuration in /etc/nginx/sites-available/default file
```nginx
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        # SSL configuration
        listen 443 ssl default_server;
        listen [::]:443 ssl default_server;

        ssl_certificate /etc/nginx/ssl/nginx-selfsigned.crt;
        ssl_certificate_key /etc/nginx/ssl/nginx-selfsigned.key;

        root /var/www/html;
        location /var/www/html/calls {
                # Enable directory browsing
                autoindex on;
        }

        # Add index.php to the list if you are using PHP
        index index.php index.html index.htm index.nginx-debian.html;

        server_name _;

        location / {
                try_files $uri $uri/ @php;
        }

        # pass PHP scripts to FastCGI server
        location @php {
                rewrite ^/(.*)$ /$1.php last;
        }

        location ~ \.php$ {
                include snippets/fastcgi-php.conf;
                fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                include fastcgi_params;
        }

        if ($request_uri ~ \.php($|\?)) {
                rewrite ^(.*)\.php$ $1 permanent;
        }

        location ~* \.(jpg|png|gif|js|css|php)$ {
                valid_referers 172.17.12.56;
                if ($invalid_referer) {
                        return 403;
                }
        }

        add_header X-Powered-By "Nginx";
        add_header X-Content-Type-Options nosniff;
        add_header X-Frame-Options SAMEORIGIN;
        add_header X-XSS-Protection "1; mode=block";

        server_tokens off;
        server_name_in_redirect off;
        proxy_hide_header Server;

        location /ws {
                proxy_pass http://127.0.0.1:8080;
                proxy_http_version 1.1;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection "upgrade";
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
        }

        location ~* .(jpg|jpeg|png|gif|ico|css|js)$ {
                expires 30d;
        }
}
```
### Replace the "/etc/nginx/ssl/nginx-selfsigned.crt" and "/etc/nginx/ssl/nginx-selfsigned.key"  with your actual SSL certificate paths and also update the server Local IP/Domain/Public IP in valid referers section
### Check the nginx syntax for any errors

```
nginx -t
```

### Now restart the nginx service to apply the changes

```
systemctl restart nginx.service
```
