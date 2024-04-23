# Настройка мониторинга
Цель: научиться настраивать дашборд.

Настроить дашборд с 4-мя графиками:
- память;
- процессор;
- диск;
- сеть.

Настроить zabbix (использовать screen (комплексный экран)).

### Устанавливаем репозиторий Zabbix
[root@otus-task14 ~]# rpm -Uvh https://repo.zabbix.com/zabbix/6.5/rhel/7/x86_64/zabbix-release-6.5-1.el7.noarch.rpm
### Устанавливаем zabbix-server, zabbix-agent, zabbix-frontend
[root@otus-task14 ~]# yum install zabbix-server-mysql zabbix-agent\
[root@otus-task14 ~]# yum install centos-release-scl -y\
### Активируем zabbix-frontend репозиторий
[root@otus-task14 ~]# nano /etc/yum.repos.d/zabbix.repo\
[zabbix-frontend]\
...\
enabled=1\
...\
### Устанавливаем пакеты zabbix-frontend и mysql
[root@otus-task14 ~]# yum install zabbix-web-mysql-scl zabbix-apache-conf-scl\
[root@otus-task14 ~]# yum install mariadb-server\
[root@otus-task14 ~]# systemctl start mariadb.service
### Инициализация базы данных
[root@otus-task14 ~]# mysql -uroot\
MariaDB [(none)]> create database zabbix character set utf8 collate utf8_bin;\
MariaDB [(none)]> create user zabbix@localhost identified by 'zabbix_passwd';\
MariaDB [(none)]> grant all privileges on zabbix.* to zabbix@localhost;\
MariaDB [(none)]> set global log_bin_trust_function_creators = 1;\
MariaDB [(none)]> quit
### Импорт схемы данных Zabbix и отключение log_bin_trust_function_creators
[root@otus-task14 ~]# zcat /usr/share/doc/zabbix-server-mysql*/create.sql.gz | mysql -uzabbix -p zabbix\
[root@otus-task14 ~]# mysql -uroot\
MariaDB [(none)]> set global log_bin_trust_function_creators = 0;\
MariaDB [(none)]> quit
### Пароль для базы данных сервера Zabbix
[root@otus-task14 ~]# nano /etc/zabbix/zabbix_server.conf\
DBPassword=zabbix_passwd
# Нужно раскомментировать временную зону
[root@otus-task14 ~]# nano /etc/opt/rh/rh-php72/php-fpm.d/zabbix.conf\
php_value[date.timezone] = Europe/Moscow
