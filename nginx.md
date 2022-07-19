

**Installation**
- `sudo apt install -y nginx`

**Check an status**
- `sudo systemctl status nginx`
- If active and running, navigate to http://localhost.

**Nginx Directory**
- `cd /etc/nginx`
- All the configuration file for nginx are available here.
- All the custom configuration file are available on `/etc/nginx/conf.d`

**Creating custom files**
- We have to unlink the default configuration file under `/etc/nginx/sites-enabled`.
- `unlink default`
- Navigate to `/etc/nginx/conf.d`
- Create a new configuration file. The name should be the same as the site which you are going to create.
- `vim example.com.conf`
- Inside the configuration file,
 - `server {
 	listen 80 default_server;
 	index index.html index.htm index.php;
 	server_name example.com;
 	root /var/www/example.com;
 }`
 - Test the configuration file is correct or not: `nginx -t`

 **Default location of the Nginx**
 - Whe nginx is installed, the files will be available at `/var/www/nginx`
 - Create a new directory `mkdir /var/www/example.com`
 - Navigate inside the directory and create an `index.html` file.
 - Download a github repository under `/var/www/` https://github.com/techbeast-org/nginx-basics
 - Move the files inside it to example.com

 **Reload Nginx**
 - After the configuration file has been changed, we need to reload an nginx.
 - `systemctl reload nginx`

 **Server multiple pages**
 - Navigate to the configuration directory
 - `sudo vim /etc/nginx/conf.d/example.com.conf`
 - The configuration file will be
  - `server {
        listen 80 default_server;
        index index.html index.htm index.php;
        server_name example.com;
        root /var/www/example.com;
     location / {
        try_files $uri $uri/ $uri.html =400;
     }
     location /foss {
        try_files $uri /foss.html;
     }
     }`
  
**Create a custom error pages**
- The configuration file will be
- `server {
        listen 80 default_server;
        index index.html index.htm index.php;
        server_name example.com;
        root /var/www/example.com;
location / {
        try_files $uri $uri/ $uri.html =400;
}
location /foss {
        try_files $uri /foss.html;
}
error_page 400 404 /400.html;
location = /400.html {
        internal;
}
}`
- `sudo nginx -t`
- `sudo systemctl reload nginx`

**Create a 50x error pages**
- The configuration file will be
- `server {
        listen 80 default_server;
        index index.html index.htm index.php;
        server_name example.com;
        root /var/www/example.com;
location / {
        try_files $uri $uri/ $uri.html =400;
}
location /foss {
        try_files $uri /foss.html;
}
error_page 400 404 /400.html;
location = /400.html {
        internal;
}
error_page 500 502 503 504 /50x.html;
location = /50x.html {
        internal;
}
}`
- `sudo nginx -t`
- `sudo systemctl reload nginx`


