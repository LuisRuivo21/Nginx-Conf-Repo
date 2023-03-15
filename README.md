# Nginx-Conf-Repo
# How to create an Nginx Server Block
# Create the server block file in /etc/nginx/conf.d/ directory. The file name must end with .conf.
# sudo nano /etc/nginx/conf.d/example.com.conf
# Put the following text into the file. Replace the red texts with your own domain name. Don’t forget to create A records for your domain name in your DNS manager i.e CloudFlare


server {
  listen 80;
  listen [::]:80;
  server_name www.example.com example.com;
  root /usr/share/nginx/example.com/;
  index index.php index.html index.htm index.nginx-debian.html;

  error_log /var/log/nginx/wordpress.error;
  access_log /var/log/nginx/wordpress.access;

  location / {
    try_files $uri $uri/ /index.php;
  }

   location ~ ^/wp-json/ {
     rewrite ^/wp-json/(.*?)$ /?rest_route=/$1 last;
   }

  location ~* /wp-sitemap.*\.xml {
    try_files $uri $uri/ /index.php$is_args$args;
  }

  error_page 404 /404.html;
  error_page 500 502 503 504 /50x.html;

  client_max_body_size 20M;

  location = /50x.html {
    root /usr/share/nginx/html;
  }
