# How to deploy a Flask app to Ubuntu server
In this tutorial, we will deploy an Flask website to Ubuntu 18.04. There are some options for us to do this, however, in this guide, we adopt Nginx as a web server and uWSGI as an interface between our Flask application and Nginx.

## Install Nginx
Before we can serve our Flask website, we probably need to configure our Ubuntu to accept inbound and outbound data. By seeting up basic firewall, we can allow only certain connection in order to enhace security of the server. On Ubuntu, we handle this using `ufw`. For instance, the code below enables only SSH connection which is sufficient to remotely connect to our Ubuntu server. 

```
ufw allow OpenSSH
```
Install `Nginx` by using:

```
sudo apt update
sudo apt install nginx
```

To allow traffic to our web server, we open port 80 by:

```
sudo ufw allow 'Nginx HTTP'
```
To test our installation is done properly, we enter server's IP address to a browser. Since, after we set up Ngnix, Ubuntu will start Ngnix so the web server should already be up and running. Then, we will see the defaut Ngnix page if the server is running correctly.

Some crucial `Nginx` files and directories are (1) the web content in `/var/www/html`; (2) server logs are in `/var/log/nginx/access.log` which records every request to our web server and `/var/log/nginx/error.log` for any errors occurred; (3) Configuring web server is done by modify mutiple file including `/etc/nginx/nginx.conf` which is the main configuration file, `/etc/nginx/sites-available` comprises sever block configurations and `/etc/nginx/sites-enabled` created linked file to `sites-available` directory.

In case our server hosts more than one domain, we need to set up per-site server blocks. By default, `Nginx` servers the web content found in `/var/www/html`. To host other sites, we create a directory in `/var/www`, for instance, `test.com`:
```
sudo mkdir -p /var/www/test.com/html
``` 
Next, we generate a configuration file in `/etc/nginx/sites-available/test.com` where the `root` is pointed to the new created directory and `server_name` is our domain name.

```
server {
        listen 80;
        listen [::]:80;

        root /var/www/test.com/html;
        index index.html index.htm index.nginx-debian.html;

        server_name test.com www.test.com;

        location / {
                try_files $uri $uri/ =404;
        }
}
```
And create a link to `sites-enabled` which read by `Nginx` when starts.

```
sudo ln -s /etc/nginx/sites-available/test.com /etc/nginx/sites-enabled/
```

Finally, restart `Nginx` to update changes

```
sudo systemctl restart nginx
```

## uWSGI
WSGI(Web server Gateway interface) is a standard interface assisting an interchangable communication between a web server and our application. This is implemented by a uWSGI server which is used to convert client requests to the application. As an example, `Nginx` handle actual client requests and proxy them to `uWSGI` server, then `uWSGI` will forward these to application by triggering defined 'callables'. Then, the application returns an iterable that create body of client response.

We install the `uWSGI` server into our Python environment by using `pip`:

```
pip install uwsgi
```

## Setting up `Flask`
Enhancing package installation by using `wheel`:

```
pip install wheel
```

Enable port `5000` for testing `Flask` development server

```
sudo ufw allow 5000
```

