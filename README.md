# Docker Image for Swoole

![Docker Pulls](https://img.shields.io/docker/pulls/phpswoole/swoole.svg)
[![License](https://img.shields.io/badge/license-apache2-blue.svg)](LICENSE)

This image is built for general-purpose. We have different examples included in this Git repository to help developers
to get familiar with the image and _Swoole_.

You may get the image from [here](https://hub.docker.com/r/phpswoole/swoole).

# List of Images

Image _phpswoole/swoole_ is built using a recent commit from the master branch of the [Swoole](https://github.com/swoole/swoole-src) project.

Besides that, for each version of Swoole (starting from version 4.4.5), we build images with major versions of PHP (7.1,
7.2, and 7.3) under different architectures (_amd64_ and _arm64v8_ only for now). For example, we have following
images built for Swoole 4.4.5:

* `phpswoole/swoole:4.4.5-php7.1`
* `phpswoole/swoole:4.4.5-php7.2`
* `phpswoole/swoole:4.4.5-php7.3`
* `phpswoole/swoole:4.4.5-php7.1-arm64v8`
* `phpswoole/swoole:4.4.5-php7.2-arm64v8`
* `phpswoole/swoole:4.4.5-php7.3-arm64v8`

Most images have following Swoole extensions installed, although only few of them are enabled by default:

* [async](https://github.com/swoole/ext-async): Enabled by default in most images.
* [orm](https://github.com/swoole/ext-orm): Disabled by default in most images.
* [postgresql](https://github.com/swoole/ext-postgresql): Disabled by default in most images.
* [serialize](https://github.com/swoole/ext-serialize): Enabled by default in most images.
* [zookeeper](https://github.com/swoole/ext-zookeeper): Disabled by default in most images.

We also build development images where extra tools are included for testing, debugging, and monitoring purpose.
Development images are tagged in the format of _&lt;image name&gt;:&lt;image tag&gt;-dev_ (a "dev" postfix added to the
original image tag). e.g.,

* `phpswoole/swoole:latest-dev`
* `phpswoole/swoole:4.4.5-php7.1-dev`
* `phpswoole/swoole:4.4.5-php7.3-arm64v8-dev`

Here is the list of commands and tools available in development images:

* [gdb](https://www.gnu.org/s/gdb)
* git
* lsof
* [strace](https://strace.io)
* [tcpdump](https://www.tcpdump.org)
* [Valgrind](http://www.valgrind.org)
* vim

# Feature List

* Built-in scripts to manage _Swoole_ extensions and _Supervisord_ programs.
* Easy to manage booting scripts in Docker.
* Allow running PHP scripts and other commands directly in different environments (including ECS).
* Use one root filesystem for simplicity (one Docker `COPY` command only in dockerfiles).
* _Composer_ included.
* Built for different architectures (for now only amd64 and arm64v8 images are built).
* Support auto-reloading for local development.
* Support code debugging for local development.

# Examples

You may use the image to serve an HTTP/WebSocket server, or run some one-off command with it. e.g.,

```bash
docker run --rm phpswoole/swoole "php -m"
docker run --rm phpswoole/swoole "php --ri swoole"
docker run --rm phpswoole/swoole "composer --version"
```

We have various examples included under folder "_examples/_" to help developers better use the image. These examples are
numerically ordered. Each example has a _docker-compose.yml_ file included, along with some other files. To run an
example, please start Docker containers using the _docker-compose.yml_ file included, then check HTTP output from URL
http://127.0.0.1 unless otherwise noted. You may use the following commands to start/stop/restart Docker containers:

```bash
./bin/example.sh start   00 # To start container(s) of the first example.
./bin/example.sh stop    00 # To stop container(s) of the first example.
./bin/example.sh restart 00 # To restart container(s) of the first example.
```

To run another example, just replace the last command line parameter _00_ with an example number (e.g., _05_).

Here is a list of the examples under folder "_examples/_":

* Basic examples:
    * **00-autoload**: Restart the Swoole web server automatically if file changes detected under web root.
    * **01-basic**: print out "Hello, World!" using Swoole as backend HTTP server.
    * **02-www**: to use some customized PHP script(s) in the Docker image built.
    * **03-nginx**: to use Swoole behind an Nginx server.
    * **04-entrypoint**: to use a self-defined entrypoint script in the Docker image built.
    * **05-boot**: to update content in the Docker container through a booting script.
    * **06-update-token**: to show how to update server configurations with built-in script _update-token.sh_.
    * **07-disable-default-server**: Please check the [docker-compose.yml](https://github.com/swoole/docker-swoole/blob/master/examples/07-disable-default-server/docker-compose.yml) file included to see show how to disable the default web server created with _Swoole_.
* Manage PHP extensions and configurations:
    * **10-install-php-extension**: how to install/enable PHP extensions.
    * **11-customize-extension-options**: how to overwrite/customize PHP extension options.
    * **12-php.ini**: how to overwrite/customize PHP options.
* Manage Supervisord programs:
    * **20-supervisord-services**: to show how to run Supervisord program(s) in Docker.
    * **21-supervisord-tasks**: to show how to run Supervisord program(s) when launching a one-off command with Docker. Please check the [README](https://github.com/swoole/docker-swoole/blob/master/examples/21-supervisord-tasks/README.md) file included to see how to run the example.
    * **22-supervisord-enable-program**: to show how to enable program(s) in Supervisord program.
    * **23-supervisord-disable-program**: to show how to disable Supervisord program(s).
* Debugging:
    * **30-debug-with-gdb**: Please check the [README](https://github.com/swoole/docker-swoole/blob/master/examples/30-debug-with-gdb/README.md) file included to see how to debug your PHP code with _gdb_.
    * **31-debug-with-valgrind**: Please check the [README](https://github.com/swoole/docker-swoole/blob/master/examples/31-debug-with-valgrind/README.md) file included to see how to debug your PHP code with _Valgrind_.
    * **32-debug-with-strace**: Please check the [README](https://github.com/swoole/docker-swoole/blob/master/examples/32-debug-with-strace/README.md) file included to see how to debug your PHP code with _strace_.

# Build Images Manually

The Docker images are built and pushed out automatically through Travis. If you want to build some image manually, please
follow these three steps.

**1**. Install Composer packages. If you have command "composer" installed already, just run `composer update -n`.

**2**. Use commands like following to create dockerfiles:

```bash
./bin/generate-dockerfiles.php master # Generate dockerfiles to build images from the master branch of Swoole.
./bin/generate-dockerfiles.php 4.4.7  # Generate dockerfiles to build images for Swoole 4.4.7.
```

**3**. Build Docker images with commands like:

```bash
docker build -t phpswoole/swoole                      -f dockerfiles/latest/amd64/Dockerfile         .
docker build -t phpswoole/swoole:4.4.7-php7.1         -f dockerfiles/4.4.7/amd64/php7.1/Dockerfile   .
docker build -t phpswoole/swoole:4.4.7-php7.3-arm64v8 -f dockerfiles/4.4.7/arm64v8/php7.3/Dockerfile .
```

To build development images (where extra tools are included), add an argument _DEV_MODE_:

```bash
docker build --build-arg DEV_MODE=true -t phpswoole/swoole:latest-dev               -f dockerfiles/latest/amd64/Dockerfile         .
docker build --build-arg DEV_MODE=true -t phpswoole/swoole:4.4.7-php7.1-dev         -f dockerfiles/4.4.7/amd64/php7.1/Dockerfile   .
docker build --build-arg DEV_MODE=true -t phpswoole/swoole:4.4.7-php7.3-arm64v8-dev -f dockerfiles/4.4.7/arm64v8/php7.3/Dockerfile .
```

# TODOs

* Allow to stop the container gracefully.
* Support more architectures.
* Add Alpine image if needed.

# Credits

Current implementation borrows ideas from [Demin](https://github.com/deminy)'s work at [Glu Mobile](https://glu.com).
