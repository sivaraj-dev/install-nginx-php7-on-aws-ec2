# Cheatsheet (Install nginx server + PHP 7 on AWS EC2 Linux)

### Install linux update, followed by GCC and Make
```sh
sudo yum -y update
sudo yum install -y gcc make
```

### to see what's new (optional)
```sh
sudo yum install -y yum-plugin-changelog
sudo yum update --changelog
```

### install php 7, php-fpm and related modules
```sh
sudo rpm -Uvh https://mirror.webtatic.com/yum/el6/latest.rpm
sudo yum install --enablerepo=webtatic-testing php70w php70w-devel php70w-fpm php70w-mysqlnd php70w-mbstring php70w-pdo php70w-mcrypt php70w-xml php70w-mbcrypt php70w-pear
```

### install nginx
```sh
sudo yum install -y nginx 
```

# Config 
### Php-fpm
`/etc/php-fpm.d/www.conf`(Add or uncomment by removing `;` in the start)
```sh
[global]
emergency_restart_threshold = 10
emergency_restart_interval = 1m
process_control_timeout = 10s

[www]
listen = /var/run/php-fpm/php-fpm.sock
listen.owner = nginx
listen.group = nginx
listen.mode = 0664
user = nginx
group = nginx

pm.max_children = 20
pm.start_servers = 5
pm.min_spare_servers = 5
pm.max_spare_servers = 20
pm.max_requests = 200

php_admin_value[memory_limit] = 64M

### run at startup, and start them up
sudo chkconfig nginx on
sudo chkconfig php-fpm on
sudo service nginx start
sudo service php-fpm start
```

### reload nginx config & restart
```sh
sudo nginx -s reload && sudo service nginx restart && sudo service php-fpm restart
```

### to check php version
```sh
php -v
```

# Database & Drivers 
### install mysql
```sh
sudo yum -y install mysql-server sudo service mysqld start
```

### install mongodb driver for php7
```sh
sudo yum install -y openssl-devel 
sudo pecl install mongodb
```

Note:
  - check file exists ====> `/usr/lib64/php/modules/mongodb.so`
  - add `extension=mongodb.so` to `php.ini`

# install composer
```sh
cd ~
curl -sS https://getcomposer.org/installer | php
chmod +x composer.phar
mv composer.phar /usr/local/bin/composer
```




