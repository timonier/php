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

__Note__: If you do not define `INSTALL_DIRECTORY`, `installer` will use in `/usr/local/bin`.

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

Run the application via `docker run`:

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
    root /opt/test;

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

docker run --detach --net host --volume $PWD:/opt/test:ro timonier/php:fpm

docker run --detach --net host --volume $PWD:/etc/nginx/conf.d:ro --volume $PWD:/opt/test:ro nginx:stable-alpine

# Go to "http://localhost/"
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
* [melody](http://melody.sensiolabs.org)
* [php](http://www.php.net/)
* [php-cs-fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer)
* [timonier/dumb-entrypoint](https://github.com/timonier/dumb-entrypoint)
