
# Installation for Zabbix3.2 

clone repository
```
git clone https://github.com/sios-tech/ansible-zabbix-rhel7.git
```

configuration
```
vim ./ansible-zabbix-rhel7/group_vars/zabbix-server-mysql-standalone
```

place of installation
```
vim ./ansible-zabbix-rhel7/hosts
```

execute
```
cd ansible-zabbix-rhel7
ansible-playbook -u <userName> ./zabbix-server-mysql-standalone.yml

# public key authentication
ansible-playbook --private-key=<pathToPrivatekey> -u <userName> ./zabbix-server-mysql-standalone.yml
```

mysql security setting
```
mysql_secure_installation
```

First time login
```
http://<ipAddress or DNS>/zabbix/
user: Admin
pass: zabbix
```
