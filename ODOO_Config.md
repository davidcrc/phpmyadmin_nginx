# ## ARCHIVO BASH

    sudo mkdir -p /var/www/html/odoo13.com/


    ## sudo chown -R www-data:www-data /var/www/odoo13.com/


    ##sudo nano /var/www/html/odoo13.com/index.html
    echo "ODoo 13 " > /var/www/html/odoo13.com/index.html



    sudo nano /etc/nginx/sites-available/odoo13.com

    # Este es la opcion basica para webs::

    server {
            ## Escucha en el puerto 80 (HTTP)
            listen   80; 

            ## Raíz donde se encuentra la página Web
            root /var/www/html/odoo13.com/;

            ## Orden de prioridad de los archivos index
            index index.html index.htm;

            server_name odoo13.com;
    }




    sudo ln -s /etc/nginx/sites-available/odoo13.com /etc/nginx/sites-enabled/odoo13.com

    sudo nginx -t

    sudo service nginx restart

## ARCHIVO "odoo13.com" de nginx :
    server {
        server_name odoo13.com; ## replace ip and server name with your domain and ip
        listen 80;

        ## Raíz donde se encuentra la página Web
            root /var/www/html/odoo13.com/public_html/;

            ## Orden de prioridad de los archivos index
            index index.html index.htm;


        access_log /var/log/nginx/testing-access.log;
        error_log /var/log/nginx/testing-error.log;


        location / {
            proxy_connect_timeout   3600;
            proxy_read_timeout      3600;
            proxy_send_timeout      3600;
            send_timeout            3600;
            proxy_pass http://127.0.0.1:8069/;
            proxy_set_header Host $http_host;
            proxy_set_header X-Forwarded-Host $http_host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        }
        gzip on;
        gzip_min_length 1000;

        # error 502
        error_page 502 /502.html;
        location = /502.html {

            root  /var/www/html/odoo13.com/public_html/;
            internal;
        }	

        ## error 404
        error_page 404 /404.html;
            location = /404.html {
                    root /var/www/html/odoo13.com/public_html/;
                    internal;
            }
        
    }
    upstream odoo {
        server 127.0.0.1:8069 weight=1 fail_timeout=0;
    }
    upstream odoo-im {
        server 127.0.0.1:8072 weight=1 fail_timeout=0;
    }
