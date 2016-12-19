# README

The PHP Interpreter

## Installation

Copy the file `bin/php` into your executable folder (like `/usr/local/bin` or `$HOME/bin`):

```sh
sudo curl -sLo /usr/local/bin/php "https://github.com/timonier/php/raw/master/bin/php"
sudo chmod +x /usr/local/bin/php
```

Linux users can use the [installer](https://github.com/timonier/php/blob/master/bin/installer):

```sh
curl -sL "https://github.com/timonier/php/raw/master/bin/installer" | sudo sh -s install
```

## Usage

Run the command `php`:

```sh
php -a
#Interactive shell
#
#php >
```

If you wish to configure PHP, you can use option `-n`:

```sh
# Define date.timezone

php -d date.timezone=Europe/Paris -i | grep "date.timezone"

# Enable xdebug

php -m | grep "xdebug"
# (empty)
php -d zend_extension=xdebug.so -m | grep "xdebug"
# xdebug
```

__Note__: By default, the version `5.5.38` will be used. To change the version, define the `TAG` before the command. For example:

```sh
php -v
# PHP 5.5.38-1~dotdeb+7.1 (cli) (built: Jul 21 2016 18:33:48)
# ...

TAG="5.5.38" php -v
# PHP 5.5.38-1~dotdeb+7.1 (cli) (built: Jul 21 2016 18:33:48)
# ...
```

## Contributing

1. Fork it.
2. Create your branch: `git checkout -b my-new-feature`.
3. Commit your changes: `git commit -am 'Add some feature'`.
4. Push to the branch: `git push origin my-new-feature`.
5. Submit a pull request.

__Note__: Use the script `bin/build` to test your modifications locally.

## Links

* [image "timonier/php"](https://hub.docker.com/r/timonier/php/)
* [php](http://www.php.net/)
