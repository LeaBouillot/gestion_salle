-CREATED A DATABASE IN HOSTINGER

-CHANGES in .env file for the databse name, id, password and the site.
DATABASE_URL="mysql://DB_USER:DB_PASSWORD@IP_HOST/DB_NAME"

- GO TO HOSTINGER tableau aboard of the hall4all.fr then gestionnaire des fichier

- inside public_html, drop all the files of symfony project of local
- connection to the FTP via compte connection FTP
  -transfer all the neccessary files excluding .git, vendor, .gitignore with .env in dev mode always for the moment

-Open cmd line terminal in your computer and login to the hostinger SSH using the connection SSH provided by the hostinger

- enter the password for the db which you have set for the db_hall4all

-and then lanch the command to change the permissions to the dossier public_html
chmod -R 755 public_html

- inside the cmd line terminal
  go to public_html (cd public_html)
  and run
  php bin/console doctrine:migrations:migrate --env=prod
  // BUT THIS WAS NOT WORKING SAID PDO DRIVER NOT FOUND
  THEN use phpmyadmin of the hostinger for hall4all.fr and import the db directly using phpMyAdmin

- add the .htaccess in root folder of the symfony project with # Redirect all traffic to the public directory
  RewriteEngine On
  RewriteRule ^(.\*)$ /public/$1 [L]
  php_value display_errors 1
  php_value display_startup_errors 1

- add the .htaccess inside the public folder of the symfony project with # Enable URL rewriting
  RewriteEngine On

            # Redirect all HTTP requests to HTTPS (Optional: Uncomment if HTTPS is desired)
            # RewriteCond %{HTTPS} !=on
            # RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]

            # Redirect all requests to Symfony's front controller (index.php) if the requested file or directory doesn't exist
            RewriteCond %{REQUEST_FILENAME} !-f
            RewriteCond %{REQUEST_FILENAME} !-d
            RewriteRule ^(.*)$ index.php [QSA,L]

            # Set custom error pages for Symfony (Optional)
            ErrorDocument 404 /index.php
            ErrorDocument 500 /index.php

            # PHP settings for Symfony (adjust as needed)
            php_value memory_limit 256M
            php_value upload_max_filesize 50M
            php_value post_max_size 50M
            php_value max_execution_time 300
            php_value mysql.connect_timeout 60
            php_value mysqli.reconnect 1

            # Security headers (Optional)
            <IfModule mod_headers.c>
                Header set X-Content-Type-Options "nosniff"
                Header set X-Frame-Options "SAMEORIGIN"
                Header set X-XSS-Protection "1; mode=block"
            </IfModule>

            # Enable caching for static assets (Optional, improves loading speed)
            <IfModule mod_expires.c>
                ExpiresActive On
                ExpiresByType image/jpg "access plus 1 month"
                ExpiresByType image/jpeg "access plus 1 month"
                ExpiresByType image/gif "access plus 1 month"
                ExpiresByType image/png "access plus 1 month"
                ExpiresByType text/css "access plus 1 week"
                ExpiresByType application/javascript "access plus 1 week"
                ExpiresByType text/javascript "access plus 1 week"
            </IfModule>

  // this is generally to redirect the website to the index.php of the public folder which will do the necessary to do the routing of the routes in symfony
  //Inside the public folder, this .htaccess file will manage Symfony’s URL rewriting, handle PHP settings, set up custom error pages, and set up caching for assets.

- connect to SSH using terminus /git bash
  and go to file public_html and launch
  php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
  php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
  php composer-setup.php
  php -r "unlink('composer-setup.php');"

- php bin/console cache:clear --env=dev
  cleared the cache as the website was always in dev mode for the moment

-added this code to display the error at the PHP level which was not visible earlier with the incapatibility of the php versions
php_value display_errors 1
php_value display_startup_errors 1

- with the code above i got to know that i didnt do the do the installation of it dependencies which en error of the autoloading when i went to the site hall4all.fr( vendor was missing)

- installation of all the dependencies via git bash ( connecting using ssh)
  php composer.phar install --no-dev --optimize-autoloader

- but with the commande above i used to have a problem of the debug_bunddle so i change the .env file with app_env= prod

Bingo the website worked but the images of the logo was not visible yet at this point

-To be able to see the images the img folder should be inside the public folder not in assets so i just moved the img folder to the public and it worked

MAILER_DSN

- the contact page of the website was not working as there was no smtp de mail
  so i have added my mailtrap.io accounts smtp to be able to access the contact page
