### To run this "nginx-php7.4-fpm-configure-role"

#### Add Servsers host names in this path```ansible-playbook/hosts ``` In [nginx-php7.4-fpm-configure-role] section
```
[zabbix-servers]
172.x.x.x host_name=nginx
```
#### Next run the ````/playbooks/nginx-php7.4-fpm-playbook.yml ```` In this repo: ````ansible-playbooks/````
Example:
```
ansible-playbook -i hosts playbooks/nginx-php7.4-fpm-playbook.yml 
```