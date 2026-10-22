[English](./README.md) | 中文

# easyswoole-docker

## 修改说明

本仓库 fork 自 [XueSiLf/easyswoole-docker](https://github.com/XueSiLf/easyswoole-docker)，由第三方独立维护，与 EasySwoole 官方及上游作者**无任何关联或背书关系**。

本 fork 的主要修改：

- **安全修复**：PHP 升级至 8.5.12 / 8.4.27 / 8.3.36（修复 CVE-2026-109618 / CVE-2026-109619）
- **移除漏洞版本**：删除 PHP 7.3 / 7.4 / 8.1 / 8.2 的 Dockerfile 与 CI 工作流
- **新增 PHP 8.5**：Alpine 3.24 上的 base 与 Swoole 6.2.2 镜像
- **Swoole 构建修复**：PHP 8.4 改用 Swoole 6.2.2（5.1.x 无法在 PHP 8.4 下编译）；PHP 8.5 改用 Swoole 6.2.2（6.2.3 存在上游编译回归）
- **账号更名**：镜像命名空间及相关引用统一改为 ryx16

## Docker 下开发

假设您的本机环境并不能达到 `EasySwoole` 的环境要求，或对于环境配置不是那么熟悉，那么您可以通过以下方法来运行及开发 `EasySwoole` 项目：

### 启动容器

可以根据实际情况，映射到宿主机对应的目录，以下以 `/workspace/project` 为例

> 如果 `docker` 启动时开启了 `selinux-enabled` 选项，容器内访问宿主机资源就会受限，所以启动容器时可以增加 `--privileged -u root` 选项

```bash
docker run --name easyswoole \
-v /workspace/project:/var/www/project \
-p 9501:9501 -it \
--privileged -u root \
--entrypoint /bin/sh \
ryx16/easyswoole:php8.5.12-alpine3.24-swoole6.2.2
```

你可以选择公共镜像： 

#### php 8.3

- `ryx16/easyswoole:php8.3.36-alpine3.24-swoole5.1.2`
- `ryx16/easyswoole:php8.3.36-alpine3.24-swoole5.1.6`

#### php 8.4

- `ryx16/easyswoole:php8.4.27-alpine3.24-swoole6.2.2`

#### php 8.5

- `ryx16/easyswoole:php8.5.12-alpine3.24-swoole6.2.2`

### 安全状态说明（CVE-2026-109618 / CVE-2026-109619）

> 更新于 2026-10-22。两个高危漏洞（CVE-2026-109618、CVE-2026-109619）可导致**未认证远程代码执行（RCE）**，影响范围可追溯至 PHP 5.4。官方修复版本为 **8.5.12 / 8.4.27 / 8.3.36 / 8.2.35**。
> 本项目镜像基于 Alpine 软件包构建，实际安装的 PHP 版本取决于对应 Alpine 仓库的打包进度。

**✅ 已修复（无漏洞，推荐使用）**

- `ryx16/easyswoole:php8.5.12-alpine3.24-swoole6.2.2`
- `ryx16/easyswoole:php8.4.27-alpine3.24-swoole6.2.2`
- `ryx16/easyswoole:php8.3.36-alpine3.24-swoole5.1.2/5.1.6`

> **漏洞版本已移除**（2026-10-22）：本仓库已删除全部存在漏洞的镜像（php 7.3.33、7.4.33、8.1.x、8.2.x），对应 Dockerfile 与 CI 工作流一并移除，仅构建并发布上方已修复版本（8.3.36 / 8.4.27 / 8.5.12）。若你本地已拉取过旧的漏洞镜像，请删除并改用上方已修复镜像。

### 创建项目

```bash
cd /var/www/project
composer require easyswoole/easyswoole
php vendor/bin/easyswoole.php install
# php vendor/bin/easyswoole install # 当你项目中的 EasySwoole 框架本低于 3.7.1 时
```

> 注意，在部分环境下，例如 `Win10` 系统的 `docker` 环境。
不可把虚拟机共享目录作为 `EasySwoole` 的 `Temp` 目录，将会因为权限不足无法创建 `socket`。这将产生报错：`listen xxxxxx.sock fail`， 为此可以手动在 `dev.php` 配置文件里把 `Temp` 目录（`TEMP_DIR`配置项）改为其他路径即可，如：`'/tmp'`。

### 启动项目

```bash
cd /var/www/project
php easyswoole.php server start
# php easyswoole server start # 当你项目中的 EasySwoole 框架本低于 3.7.1 时
```

接下来，就可以在宿主机 `/var/www/project` 中看到您安装好的代码了。 由于 `EasySwoole` 是持久化的 CLI 框架，当您修改完您的代码后，通过 `CTRL + C` 终止当前启动的进程实例，并重新执行 `php easyswoole server start` 启动命令即可。

## 支持的标签和独立的 Dockerfile 文件链接

标签格式:

-   8.5.12: php 版本，支持 8.3/8.4/8.5，推荐 8.5.12
-   3.24: alpine 系统版本，支持 alpine 3.24，推荐 3.24
-   6.2.2: php swoole 扩展版本（8.4 / 8.5）；5.1.2 / 5.1.6: php swoole 扩展版本（8.3）

支持的 `Dockerfile` 文件链接:

### php 8.3.36

-   [`php8.3.36-alpine3.24-base`](https://github.com/ryx16/easyswoole-docker/tree/main/dockerfiles/php8/8.3.36/alpine/3.24/base/Dockerfile)
-   [`php8.3.36-alpine3.24-swoole-*`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.3.36/alpine/3.24/swoole/Dockerfile)，[`php8.3.36-alpine3.24-swoole5.1.6`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.3.36/alpine/3.24/swoole/5.1.6/Dockerfile)

### php 8.4.27

-   [`php8.4.27-alpine3.24-base`](https://github.com/ryx16/easyswoole-docker/tree/main/dockerfiles/php8/8.4.27/alpine/3.24/base/Dockerfile)
-   [`php8.4.27-alpine3.24-swoole-*`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.4.27/alpine/3.24/swoole/Dockerfile)，[`php8.4.27-alpine3.24-swoole6.2.2`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.4.27/alpine/3.24/swoole/6.2.2/Dockerfile)

### php 8.5.12

-   [`php8.5.12-alpine3.24-base`](https://github.com/ryx16/easyswoole-docker/tree/main/dockerfiles/php8/8.5.12/alpine/3.24/base/Dockerfile)
-   [`php8.5.12-alpine3.24-swoole-*`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.5.12/alpine/3.24/swoole/Dockerfile)，[`php8.5.12-alpine3.24-swoole6.2.2`](https://github.com/ryx16/easyswoole-docker/blob/main/dockerfiles/php8/8.5.12/alpine/3.24/swoole/6.2.2/Dockerfile)

## 快速查看

-   [easyswoole](https://github.com/easy-swoole)
-   [easyswoole 文档](https://www.easyswoole.com/)

## 如何使用这个镜像

添加 [Dockerfile](https://github.com/ryx16/easyswoole-docker/blob/main/Dockerfile) 到你的项目中即可。

## 镜像详情

基础镜像包含如下扩展：

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
