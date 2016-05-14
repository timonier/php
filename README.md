# README

## Installation

Copy the file `bin/php` into your executable folder (like `/usr/local/bin` or `$HOME/bin`):

```sh
sudo curl -sLo /usr/local/bin/php "https://github.com/mauchede/php/raw/master/bin/php"
sudo chmod +x /usr/local/bin/php
```

Linux users can use the [installer](https://github.com/mauchede/php/blob/master/bin/installer):

```sh
curl -sL "https://github.com/mauchede/php/raw/master/bin/installer" | sudo sh -s install
```

## Usage

Run the command `php`:

```sh
php -a
#Interactive shell
#
#php >
```

__Note__: By default, the version `5.5.35` will be used. To change the version, define the `TAG` before the command. For example:

```sh
php -v
# PHP 5.5.35 (cli) (built: May  4 2016 03:14:01)
# ...

TAG="5.5.35" php -v
# PHP 5.5.35 (cli) (built: May  4 2016 03:14:01)
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

* [image "mauchede/php"](https://hub.docker.com/r/mauchede/php/)
* [php](http://www.php.net/)
