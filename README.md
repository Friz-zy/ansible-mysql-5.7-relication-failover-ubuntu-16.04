# Ansible role for installing Mysql 5.7 master-slave with gtid and automatic failover

This Ansible role should
1) add [Oracle repo](https://dev.mysql.com/downloads/repo/apt/) into servers
2) protect hosts with [UWF firewall](https://en.wikipedia.org/wiki/Uncomplicated_Firewall)
3) install Mysql 5.7 packages
4) configure [mysql master-slave replication](https://dev.mysql.com/doc/refman/5.7/en/replication.html) (replica seed is possible)
5) install [Haproxy](https://en.wikipedia.org/wiki/HAProxy) as mysql frontend
6) install [Orchestrator](https://github.com/github/orchestrator) for automatic failover and simply mysql replication management
Note: resolving hostnames important for orchestrator
8) add mysqldump into root cron on all mysql servers that will take full backup from mysql slave daily

Tested with ubuntu 16.04, debian 8 and ansible 2.3 (and vagrant 1.9)

Minimum variables that you should know and set:

mysql.yml
```
- name: Mysql Setup
  hosts: mysql
  become: true
  vars:
    replication_user:
      name: replicator
      host: '%'
      password: '_strong_password_'
    root_user:
      name: root
      host: '%'
      password: '_strong_password_'
***
- name: Orchestrator Setup
  hosts: orchestrator
  become: true
  vars:
    mysql_root_user:
      name: root
      password: '_strong_password_'
```

group_vars/all.yml
```
mysql_port: 3306
frontend_mysql_master_port: 3310
frontend_mysql_slave_port: 3311
```

How to test:
1) start vagrant vms
```
vagrant up
```
2) run ansible playbook
```
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook --ask-pass -i hosts mysql.yml
```