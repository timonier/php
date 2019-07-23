# README

The PHP Interpreter

## Installation

### cli

```sh
# Define installation folder

export INSTALL_DIRECTORY=/usr/bin

# Use local installation

sudo bin/installer install

# Use remote installation

curl --location "https://github.com/timonier/php/raw/master/bin/installer" | sudo sh -s -- install
```

__Note 1__: If you do not define `INSTALL_DIRECTORY`, `installer` will use in `/usr/local/bin`.

__Note 2__: `docker-for-mac` users have to configure [native NFS server](https://medium.com/@sean.handley/how-to-set-up-docker-for-mac-with-native-nfs-145151458adc).

## Usage

### cli

Run the command `php`:

```sh
# See all php options

php --help

# Run php as interactive shell

php -a
#Interactive shell
#
#php >

# Define date.timezone

php -d date.timezone=Europe/Paris -i | grep "date.timezone"

# Enable xdebug

php -m | grep "xdebug"
# (empty)

php -d zend_extension=xdebug.so -m | grep "xdebug"
# xdebug
```

__Note__: You can also define the PHP configuration in `${HOME}/.php`.

### fpm

```sh
# Create index.php

cat > index.php << "EOF"
<?php
phpinfo();
EOF

# Create nginx configuration

cat > default.conf << "EOF"
server {
    listen *:80 default_server;
    server_name _;

    index index.php;
    root /home/www-data;

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
EOF

# Start services

docker run --detach --net host --volume $PWD:/home/www-data:ro timonier/php:fpm

docker run --detach --net host --volume $PWD:/etc/nginx/conf.d:ro --volume $PWD:/home/www-data:ro timonier/nginx

# Go to "http://localhost/"
```

It is possible to change `UID` and / or `GID` of user `www-data` (user used to run `php-fpm`) with environment variables `PHP_UID` and `PHP_GID`:

```sh
docker run --env PHP_GID=1005 --env PHP_UID=1005 --interactive --rm --tty timonier/php:fpm
```

It is possible to run a container in `read-only` mode if you mount the following folders:
* `/etc` and `/home/www-data` if you want to change `UID` or `GID` of user `php-fpm`.
* `/run`, `/tmp`, `/usr/local/etc/php` and `/usr/local/etc/php-fpm.d`.

__Note__: `/run` and `/tmp` can be mount as `tmpfs`. In that case, `/run` must have flag `exec`.

```sh
# If you do not want to change "UID" or "GID" of user "www-data"

docker run --interactive --read-only --rm --tmpfs /run:exec --tmpfs /tmp --tty timonier/php:fpm

# If you want to change "UID" or "GID" of user "nginx"

docker run --interactive --read-only --rm --tmpfs /run:exec --tmpfs /tmp --tty --volume /etc --volume /home/www-data timonier/php:nginx
```

### nginx

```sh
docker run --interactive --publish 80:80 --rm --tty timonier/php:nginx
```

__Note__: By default `nginx` opens port `80`.

It is possible to change `UID` and / or `GID` of users `nginx` (user used to run `nginx`) and `www-data` (user used to run `php-fpm`) with environment variables `NGINX_UID`, `NGINX_GID`, `PHP_UID` and `PHP_GID`:

```sh
docker run --env NGINX_GID=1005 --env NGINX_UID=1005 --env PHP_GID=1006 --env PHP_UID=1006 --interactive --publish 80:80 --rm --tty timonier/php:nginx
```

It is possible to run a container in `read-only` mode if you mount the following folders:
* `/etc` if you want to change `UID` or `GID` of user `nginx`.
* `/etc` and `/home/www-data` if you want to change `UID` or `GID` of user `php-fpm`.
* `/etc/nginx`, `/run`, `/tmp`, `/usr/local/etc/php`, `/usr/local/etc/php-fpm.d` and `/var/cache/nginx`.

__Note__: `/run`, `/tmp` and `/var/cache/nginx` can be mount as `tmpfs`. In that case, `/run` must have flag `exec`.

```sh
# If you do not want to change "UID" or "GID" of user "nginx"

docker run --interactive --publish 80:80 --read-only --rm --tmpfs /run:exec --tmpfs /tmp --tmpfs /var/cache/nginx --tty timonier/php:nginx

# If you want to change "UID" or "GID" of user "nginx"

docker run --interactive --publish 80:80 --read-only --rm --tmpfs /run:exec --tmpfs /tmp --tmpfs /var/cache/nginx --tty --volume /etc --volume /home/www-data timonier/php:nginx
```

## Contributing

1. Fork it.
2. Create your branch: `git checkout -b my-new-feature`.
3. Commit your changes: `git commit -am 'Add some feature'`.
4. Push to the branch: `git push origin my-new-feature`.
5. Submit a pull request.

__Note__: Use the script `bin/build-images` to test your modifications locally.

If you like / use this project, please let me known by adding a [★](https://help.github.com/articles/about-stars/) on the [GitHub repository](https://github.com/timonier/php).

## Links

* [composer](https://getcomposer.org)
* [image "timonier/php"](https://hub.docker.com/r/timonier/php/)
* [just-containers/s6-overlay](https://github.com/just-containers/s6-overlay)
* [jwilder/dockerize](https://github.com/jwilder/dockerize)
* [melody](http://melody.sensiolabs.org)
* [mounting nfs shares inside docker container](https://stackoverflow.com/questions/39922161/mounting-nfs-shares-inside-docker-container)
* [php](http://www.php.net/)
* [php-cs-fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer)
* [set up docker for mac with native nfs](https://medium.com/@sean.handley/how-to-set-up-docker-for-mac-with-native-nfs-145151458adc)
* [timonier/dumb-entrypoint](https://github.com/timonier/dumb-entrypoint)
