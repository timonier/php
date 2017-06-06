# README

The PHP Interpreter

## Installation

Copy `bin/php` into your executable folder (like `/usr/local/bin` or `$HOME/bin`):

```sh
sudo curl -sLo /usr/local/bin/php "https://github.com/timonier/php/raw/master/bin/php"
sudo chmod +x /usr/local/bin/php
```

Linux users can use the [installer](https://github.com/timonier/php/blob/master/bin/installer):

```sh
curl --location "https://github.com/timonier/php/raw/master/bin/installer" | sudo sh -s -- install
```

## Usage

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
* [timonier/dumb-entrypoint](https://github.com/timonier/dumb-entrypoint)
