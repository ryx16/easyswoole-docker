English | [中文](./README-CN.md)

# easyswoole-docker

## Modifications

This repository is forked from [XueSiLf/easyswoole-docker](https://github.com/XueSiLf/easyswoole-docker), maintained independently by a third party. It has **no affiliation with and no endorsement by** EasySwoole or the upstream author.

Major changes in this fork:

- **Security fix**: upgraded PHP to 8.5.12 / 8.4.27 / 8.3.36 (fixes CVE-2026-109618 / CVE-2026-109619)
- **Vulnerable versions removed**: PHP 7.3 / 7.4 / 8.1 / 8.2 Dockerfiles and CI workflows deleted
- **PHP 8.5 added**: base and Swoole 6.2.2 / 6.2.3 images on Alpine 3.24
- **Rebranded**: image namespace and references updated to `ryx16`

## Develop in Docker

If your native environment does not meet the EasySwoole system requirements, or if you are unfamiliar with system configuration, you can run and develop the EasySwoole project as follows using Docker.

### Run Container

In the following example the host will be mapped to the local directory `/workspace/project`:

> If `the selinux-enabled` option is enabled when docker starts, access to host resources in the container will be restricted, so you should add the `--privileged -u root` option when starting the container.

```bash
docker run --name easyswoole \
-v /workspace/project:/var/www/project \
-p 9501:9501 -it \
--privileged -u root \
--entrypoint /bin/sh \
ryx16/easyswoole:php8.5.12-alpine3.24-swoole6.2.3
```

The public images you can choose from: 

#### php 8.3

- `ryx16/easyswoole:php8.3.36-alpine3.24-swoole5.1.2`
- `ryx16/easyswoole:php8.3.36-alpine3.24-swoole5.1.6`

#### php 8.4

- `ryx16/easyswoole:php8.4.27-alpine3.24-swoole5.1.2`
- `ryx16/easyswoole:php8.4.27-alpine3.24-swoole5.1.6`

#### php 8.5

- `ryx16/easyswoole:php8.5.12-alpine3.24-swoole6.2.2`
- `ryx16/easyswoole:php8.5.12-alpine3.24-swoole6.2.3`

### Security Status (CVE-2026-109618 / CVE-2026-109619)

> Updated 2026-10-22. Two critical vulnerabilities (CVE-2026-109618, CVE-2026-109619) allow **unauthenticated remote code execution (RCE)** and affect PHP versions dating back to 5.4. Official fixed releases: **8.5.12 / 8.4.27 / 8.3.36 / 8.2.35**.
> These images are built from Alpine packages, so the actually installed PHP version depends on the packaging progress of the corresponding Alpine repository.

**✅ Fixed (no vulnerability, recommended)**

- `ryx16/easyswoole:php8.5.12-alpine3.24-swoole6.2.2/6.2.3`
- `ryx16/easyswoole:php8.4.27-alpine3.24-swoole5.1.2/5.1.6`
- `ryx16/easyswoole:php8.3.36-alpine3.24-swoole5.1.2/5.1.6`

> **Vulnerable versions removed** (2026-10-22): all vulnerable images (PHP 7.3.33 / 7.4.33 / 8.1.x / 8.2.x) have been deleted from this repository — their Dockerfiles and CI workflows are removed, and only the fixed versions above are built and published. If you previously pulled any old vulnerable tag, remove it locally and switch to the fixed images.

### Create Project

```bash
cd /var/www/project
composer require easyswoole/easyswoole
php vendor/bin/easyswoole.php install
# php vendor/bin/easyswoole install # When the EasySwoole framework version in your project is less than 3.7.1.
```

> In some environments, such as the `Docker` environment of the `Win10` system. Virtual machine shared directory cannot be used as the `Temp` directory of the `EasySwoole` framework, as it will not be able to create sockets due to insufficient permissions. This will result in an error message: `listen xxxxx.lock fail`. To do so, you can manually set `TEMP_DIR` in the `dev.php` configuration file, Change the DIR directory to a different path, such as `'/tmp'`.

### Start the project

```bash
cd /var/www/project
php easyswoole.php server start
# php easyswoole server start # When the EasySwoole framework version in your project is less than 3.7.1.
```

Next, you can see your installed project in `/var/www/project`. Since EasySwoole is a persistent CLI framework, when you have modified your code, you should terminate the running process instance with `CTRL + C` and re-execute the `php easyswoole server start` start startup command to restart your server and reload the code.

## Supported tags and respective Dockerfile links

tag format:

-   8.5.12: php version, support 8.3/8.4/8.5, Recommend 8.5.12
-   3.24: alpine version, support alpine 3.24, recommend 3.24
-   6.2.3: swoole version

support:

### php 8.3.36

-   [`php8.3.36-alpine3.24-base`](https://github.com/ryx16/easyswoole-docker/tree/main/dockerfiles/php8/8.3.36/alpine/3.24/base/Dockerfile)
-   [`php8.3.36-alpine3.24-swoole-*`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.3.36/alpine/3.24/swoole/Dockerfile), [`php8.3.36-alpine3.24-swoole5.1.6`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.3.36/alpine/3.24/swoole/5.1.6/Dockerfile)

### php 8.4.27

-   [`php8.4.27-alpine3.24-base`](https://github.com/ryx16/easyswoole-docker/tree/main/dockerfiles/php8/8.4.27/alpine/3.24/base/Dockerfile)
-   [`php8.4.27-alpine3.24-swoole-*`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.4.27/alpine/3.24/swoole/Dockerfile), [`php8.4.27-alpine3.24-swoole5.1.6`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.4.27/alpine/3.24/swoole/5.1.6/Dockerfile)

### php 8.5.12

-   [`php8.5.12-alpine3.24-base`](https://github.com/ryx16/easyswoole-docker/tree/main/dockerfiles/php8/8.5.12/alpine/3.24/base/Dockerfile)
-   [`php8.5.12-alpine3.24-swoole-*`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.5.12/alpine/3.24/swoole/Dockerfile), [`php8.5.12-alpine3.24-swoole6.2.3`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.5.12/alpine/3.24/swoole/6.2.3/Dockerfile)


## Quick reference

-   [easyswoole](https://github.com/easy-swoole)
-   [easyswoole doc](https://www.easyswoole.com/)

## How to use this image

Added  [Dockerfile](https://github.com/ryx16/easyswoole-docker/blob/main/Dockerfile)  to your project.

## Info

Base image contains extensions below:

```bash
[PHP Modules]
bcmath
Core
ctype
curl
date
dom
fileinfo
filter
gd
hash
iconv
igbinary
json
libxml
mbstring
mongodb
mysqli
mysqlnd
openssl
pcntl
pcre
PDO
pdo_mysql
pdo_sqlite
Phar
posix
readline
redis
Reflection
session
SimpleXML
sockets
sodium
SPL
standard
swoole
sysvmsg
sysvsem
sysvshm
tokenizer
xml
xmlreader
xmlwriter
Zend OPcache
zip
zlib

[Zend Modules]
Zend OPcache
```
