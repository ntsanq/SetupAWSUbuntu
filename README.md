# Set up AWS instance

## Install Nginx


Install Nginx: https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-22-04
```
sudo apt update
sudo apt install nginx -y
sudo ufw allow 'Nginx HTTP'
```

## Install PHP : https://tecadmin.net/how-to-install-php-on-ubuntu-22-04/
```
sudo apt install software-properties-common ca-certificates lsb-release apt-transport-https -y
LC_ALL=C.UTF-8 sudo add-apt-repository ppa:ondrej/php 
sudo apt update 
sudo apt install php8.1 
```

## Install MySQL: https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-22-04
```
sudo apt install mysql-server -y
sudo systemctl start mysql.service
```

```
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
```

## Add host config:
```
   server {
    listen 80;
    server_name cinema cinema.sangnguyen.me;
    root /var/www/CinemaLaravel;

    index index.html index.htm index.php;

    location / {
        try_files $uri /index.php$is_args$args;
        #try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
     }

    location ~ /\.ht {
        deny all;
    }

}
```

## Install composer
```
sudo apt install php-cli unzip -y
```

```
cd ~
curl -sS https://getcomposer.org/installer -o /tmp/composer-setup.php
```
                         
```
HASH=`curl -sS https://composer.github.io/installer.sig`
```

```
php -r "if (hash_file('SHA384', '/tmp/composer-setup.php') === '$HASH') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
```
   
```
sudo php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer
```

```
sudo apt-get install -y php8.2-fpm php8.2-cli php8.2-common php8.2-mysql php8.2-zip php8.2-gd php8.2-mbstring php8.2-curl php8.2-xml php8.2-bcmath
```
or
```
sudo apt-get install -y php8.1-fpm php8.1-cli php8.1-common php8.1-mysql php8.1-zip php8.1-gd php8.1-mbstring php8.1-curl php8.1-xml php8.1-bcmath  php8.1-fpm.sock
```

## Install NPM
```
sudo apt install nodejs -y
sudo apt install npm -y
```
                                                           



#Error you may have:

- laravel.log could not be opened?

sudo chown -R $USER:www-data storage
sudo chown -R $USER:www-data bootstrap/cache

chmod -R 775 storage
chmod -R 775 bootstrap/cache

ps aux|grep nginx|grep -v grep

- php lacks of libraries:
sudo apt install php8.1
sudo apt install --no-install-recommends php8.1
sudo apt upgrade
sudo apt install  ca-certificates apt-transport-https software-properties-common
sudo add-apt-repository ppa:ondrej/php
sudo update-alternatives --config php
sudo apt-get install -y php8.1-cli php8.1-common php8.1-mysql php8.1-zip php8.1-gd php8.1-mbstring php8.1-curl php8.1-xml php8.1-bcmath
sudo apt-get install -y php8.1-fpm
                                 

- fpm version: check your config in /etc/nginx/conf/...conf
    maybe not work: 
        $ dpkg -l | grep php-fpm
        $ sudo apt-get update
        $ sudo apt-get install php-fpm
        $ systemctl list-unit-files | grep php-fpm
        $ sudo php-fpm
        $ which php-fpm
        $ sudo systemctl restart php8.x-fpm
        

- npm conflict:
  - remove npm:
  https://stackoverflow.com/questions/32426601/how-can-i-completely-uninstall-nodejs-npm-and-node-in-ubuntu#comment82333418_33947181

https://stackoverflow.com/questions/55763428/react-native-error-enospc-system-limit-for-number-of-file-watchers-reached

$ curl --silent --location https://deb.nodesource.com/setup_19.x  | sudo bash -  <<= [CHANGE THE 19.X VERSION, ex: “20.x”]

$ sudo apt install --assume-yes nodejs

if debugger shows ‘’dpkg: error processing archive /var/cache/apt/archives/nodejs_19.6.0-deb-1nodesource1_amd64.deb (--unpack):
trying to overwrite '/usr/include/node/common.gypi', which is also in package libnode-dev 12.22.9~dfsg-1ubuntu3
dpkg-deb: error: paste subprocess was killed by signal (Broken pipe)
Errors were encountered while processing:
/var/cache/apt/archives/nodejs_19.6.0-deb-1nodesource1_amd64.deb”

=> run: $ sudo dpkg -i --force-overwrite /var/cache/apt/archives/nodejs_19.6.0-deb-1nodesource1_amd64.deb.<<=[CHECK IF THERE BE]
https://askubuntu.com/questions/1062171/dpkg-deb-error-paste-subprocess-was-killed-by-signal-broken-pipe

THE SOLUTION FROM: https://www.simplified.guide/nodejs/install-in-ubuntu-latest
                                                    

                                       

-file:///var/www/CinemaLaravel/node_modules/vite/bin/vite.js:7
await import('source-map-support').then((r) => r.default.install())
    check vite.config.js file

