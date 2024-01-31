# WordPress Automated Deployment with Nginx, LEMP Stack, and GitHub Actions

This repository contains the code and configurations for setting up an automated deployment process for a WordPress website using Nginx, LEMP stack (Linux, Nginx, MySQL, PHP), and GitHub Actions as the CI/CD automation tool will be added soon.

## Server Provisioning

### 1. Provision a Virtual Private Server (VPS)

- Choose a major cloud provider (e.g., AWS, Azure, Google Cloud, or DigitalOcean) to host the WordPress website.
- Configure the VPS with a secure Linux distribution (e.g., Ubuntu 22.04).
- Ensure that all necessary firewall rules are applied.

### 2. Install and Configure Software

- Install and configure Nginx as the web server.

  ```bash
  sudo apt update
  sudo apt install nginx
  ```
  
- Set up MySQL/MariaDB as the database.
  ```
  sudo apt install mariadb-server
  ```

- Set up MySQL/MariaDB as the database.

  ```
  sudo apt install mariadb-server
  sudo mysql_secure_installation
  ```

- Install PHP for processing dynamic content.
  
  ```
  sudo apt install php-curl php-gd php-intl php-mbstring php-soap php-xml php-xmlrpc php-zip
  sudo apt install php-fpm php-mysql
  ```

### 3. WordPress Installation and Security

  ```
  # Download and extract WordPress
  wget https://wordpress.org/latest.tar.gz
  tar -xvf latest.tar.gz
  sudo mv wordpress/* /var/www/html/

  # Set proper permissions
  sudo chown -R www-data:www-data /var/www/html/
  ```
- Implement security best practices (strong passwords, limited user privileges, etc.).
- Implement SSL/TLS certificate using Let's Encrypt for secure communication.

  ```
  sudo apt install certbot
  sudo certbot --nginx
  ```

### 4. Nginx Optimization

- Optimize the Nginx server configuration to enhance website performance (caching, gzip compression, etc.).

  ```
  # Edit Nginx configuration file
  sudo nano /etc/nginx/nginx.conf

  # Make Required changes as follow

  map $sent_http_content_type $expires {
    default                    off;
    text/html                  epoch;
    text/css                   max;
    application/javascript     max;
    ~image/                    max;
    ~font/                     max;

   root /var/www/html;

  # Add index.php to the list if you are using PHP
  index index.html index.htm index.nginx-debian.html index.php;

  server_name _;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                #try_files $uri $uri/ =404;
                try_files $uri $uri/ /index.php$is_args$args;
  ```

  - Add domain (here mydomain is visheshtask.ddns.net)

    ```
    ssl_certificate /etc/letsencrypt/live/visheshtask.ddns.net/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/visheshtask.ddns.net/privkey.pem; # managed by Certbot
    ```

- Make necessary changes and restart Nginx:

  ```
  sudo systemctl restart nginx
  ```
