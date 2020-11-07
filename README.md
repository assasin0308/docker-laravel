# docker-compose-laravel
用于 laravel 开发的 docker-compose 运行环境

**提供的 Docker 容器服务：**
 - mysql 5.7.32
  >- 端口：33060 -> 转发到 3306
  >- 用户名：root
  >- 密码：nosmoking
 - postgres 12.4
  >- 端口：54320 -> 转发到 5432
  >- 用户名：postgres
  >- 密码：nosmoking
 - redis 5.0.9
  >- 端口：63790 -> 转发到 6379
 - php-fpm 7.3.23
 - php-cli 7.3.23
 - nginx 1.18.0
  >- 端口：8000 -> 转发到 80
  >- 端口：44300 -> 转发到 443
 - composer latest-stable
 - laravel 6.x
 - node 15.0.1

**使用 Docker 导出/导入镜像：**
```
# 导出
docker save docker-compose-laravel_mysql:latest -o docker-compose-laravel_mysql-latest.tar
docker save docker-compose-laravel_postgres:latest -o docker-compose-laravel_postgres-latest.tar
docker save docker-compose-laravel_redis:latest -o docker-compose-laravel_redis-latest.tar
docker save docker-compose-laravel_php-fpm:latest -o docker-compose-laravel_php-fpm-latest.tar
docker save docker-compose-laravel_php-cli:latest -o docker-compose-laravel_php-cli-latest.tar
docker save docker-compose-laravel_nginx:latest -o docker-compose-laravel_nginx-latest.tar
docker save docker-compose-laravel_composer:latest -o docker-compose-laravel_composer-latest.tar
docker save docker-compose-laravel_laravel:latest -o docker-compose-laravel_laravel-latest.tar
docker save docker-compose-laravel_node:latest -o docker-compose-laravel_node-latest.tar
#
# 导入
docker load -i docker-compose-laravel_mysql-latest.tar
docker load -i docker-compose-laravel_postgres-latest.tar
docker load -i docker-compose-laravel_redis-latest.tar
docker load -i docker-compose-laravel_php-fpm-latest.tar
docker load -i docker-compose-laravel_php-cli-latest.tar
docker load -i docker-compose-laravel_nginx-latest.tar
docker load -i docker-compose-laravel_composer-latest.tar
docker load -i docker-compose-laravel_laravel-latest.tar
docker load -i docker-compose-laravel_node-latest.tar
```

**使用 Composer 命令：**
 - 语法：
```
docker-compose run --rm composer [options] [--] [<command_name>]
```
 - 示例：
```
# 自我更新
docker-compose run --rm composer self-update
#
# 安装项目依赖
docker-compose run --rm composer install -d /var/www/code/laravel/blog
#
# 更新项目依赖
docker-compose run --rm composer update -d /var/www/code/laravel/blog
```

**使用 Laravel 安装器：**
 - 首先，通过使用 Composer 安装 Laravel 安装器：
```
docker-compose run --rm composer require laravel/installer -d /var/www/code/laravel
```
 - 语法：
```
docker-compose run --rm laravel command [options] [arguments]
```
 - 示例：
```
# 通过 Laravel 安装器创建 Laravel 项目
#   创建一个名为 blog 的项目，并已安装好 Laravel 所有的依赖项
docker-compose run --rm laravel new blog
#
# 也可以通过 Composer 命令创建 Laravel 项目
#   以下命令生成的 blog 项目与上面一致
docker-compose run --rm composer -d /var/www/code/laravel create-project --prefer-dist laravel/laravel blog
#
# 也可以指定 Laravel 版本
docker-compose run --rm composer -d /var/www/code/laravel create-project --prefer-dist laravel/laravel blog "6.*"
#
# Web 服务器配置
#     复制文件 conf\nginx\conf.d\default.conf 为 conf\nginx\conf.d\blog.conf
#     修改 blog.conf 文件内容“server_name default.www;”为“server_name blog.www;”
#     修改 blog.conf 文件内容“root "/var/www/code/default";” 为“root "/var/www/code/laravel/blog/public";”
#     修改 blog.conf 文件内容“www.default.access.log";”为“www.blog.access.log”
#     修改 blog.conf 文件内容“www.default.error.log";”为“www.blog.error.log”
#     修改 hosts 增加“127.0.0.1 blog.www”（格式：Docker容器可访问IP 域名）
#     重启 docker 后即可访问项目网址“http://blog.www:8000”
```

**使用 Artisan 命令行：**
 - 语法：
```
docker-compose run --rm php-cli php /path/to/artisan <command> [options] [arguments]
```
 - 示例：
```
# 查看所有可用的 Artisan 命令的列表
docker-compose run --rm php-cli php /var/www/code/laravel/blog/artisan list
#
# 运行迁移
docker-compose run --rm php-cli php /var/www/code/laravel/blog/artisan migrate
```

**使用 Node 包管理器命令：**
 - 语法：
```
docker-compose run --rm node npm <command>
docker-compose run --rm node cnpm [option] <command>
docker-compose run --rm node yarn [command] [flags]
```