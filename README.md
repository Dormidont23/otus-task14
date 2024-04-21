# Настройка мониторинга
Цель: научиться настраивать дашборд.

Настроить дашборд с 4-мя графиками:
- память;
- процессор;
- диск;
- сеть.

Настроить zabbix (использовать screen (комплексный экран).

Устанавливаем репозиторий Zabbix\
[root@otus-task14 ~]# rpm -Uvh https://repo.zabbix.com/zabbix/6.5/rhel/7/x86_64/zabbix-release-6.5-1.el7.noarch.rpm \
Устанавливаем zabbix-server, zabbix-agent, zabbix-frontend\
[root@otus-task14 ~]# yum install zabbix-server-mysql zabbix-agent\
[root@otus-task14 ~]# yum install centos-release-scl -y\
Активируем zabbix-frontend репозиторий\
[root@otus-task14 ~]# nano /etc/yum.repos.d/zabbix.repo\
[zabbix-frontend]\
...\
enabled=1\
...\
