# phpmyadmin_nginx


1. Intall nginx and run

2. Ensure php php*-fpm (* is 7.2 ) is installed.

3. Add default nginx (sudo gedit /etc/nginx/sites-available/default):

        location /phpmyadmin {
            root /usr/share/;
            index index.php index.html index.htm;
            location ~ ^/phpmyadmin/(.+\.php)$ {
                    try_files $uri =404;
                    root /usr/share/;
                    #fastcgi_pass 127.0.0.1:9000;
                    fastcgi_pass unix:/var/run/php/php7.2-fpm.sock;
                    fastcgi_index index.php;
                    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                    include /etc/nginx/fastcgi_params;
            }
            location ~* ^/phpmyadmin/(.+\.(jpg|jpeg|gif|css|png|js|ico|html|xml|txt))$ {
                    root /usr/share/;
            }
        }
        location /phpMyAdmin {
                rewrite ^/* /phpmyadmin last;
        }
	
4. Verified nginx sintaxis is Ok and restart nginx: 
  sudo nginx -t
  sudo systemctl status nginx
