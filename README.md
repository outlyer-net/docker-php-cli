# docker-php-cli

A small docker image with just PHP in it, built on Alpine and alternatively
on Debian or Ubuntu.
\
This image is intended to be used as a base for simple PHP web apps,
[PHP's builtin server][php_builtin_server] can be used to that effect.
\
This is a multiarch image, with 32 and 64 bit support on PC and ARM.

[![Image Size](https://images.microbadger.com/badges/image/outlyernet/php-cli.svg)][microbadger]

#### Information

* [Docker Hub][dockerhub]
* [Github][github]

### Usage

As stated above this image is intended as a base for other images,
however you can also use it for quick and dirty web app testing,
examples below.

The default entrypoint runs PHP's builtin server on port 80 with no other parameters, hence making the root directory (`/`) its document root.

#### Example: Single file

For this example we test a single PHP file, in particular we show the
output of the [`phpinfo()` function][phpinfo] to display the server information.

1. Create the file:
\
`$ echo '<?php phpinfo();' > phpinfo.php` 

2. Run the image:
\
`$ docker run --rm -it -p 8000:80 -v "$PWD"/phpinfo.php:/index.php outlyernet/php-cli`

Now accessing http://localhost:8000 should show the output of [`phpinfo()`][phpinfo].

#### Example: Multiple files / app

If instead we have an app that can run on the builtin server, we pass the document root as an argument easily:

Assuming the app is on the `./app` directory:
\
`$ docker run --rm -it -p 8000:80 -v "$PWD"/app:/app outlyernet/php-cli -t /app`

#### MIT License

Copyright &copy; 2020 Toni Corvera &lt;outlyer@gmail.com&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

[php_builtin_server]: https://www.php.net/manual/en/features.commandline.webserver.php
[phpinfo]: https://php.net/phpinfo
[dockerhub]: https://hub.docker.com/r/outlyernet/php-cli/
[github]: https://github.com/outlyer-net/docker-php-cli
[microbadger]: https://microbadger.com/images/outlyernet/php-cli
