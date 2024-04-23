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
mysql> create database zabbix character set utf8 collate utf8_bin;\
mysql> create user zabbix@localhost identified by 'password';\
mysql> grant all privileges on zabbix.* to zabbix@localhost;\
mysql> set global log_bin_trust_function_creators = 1;\
mysql> quit;
