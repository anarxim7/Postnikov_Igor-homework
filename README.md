
### Задание 1

Установите Zabbix Server с веб-интерфейсом.

![1.png](https://github.com/anarxim7/Postnikov_Igor-homework/blob/main/img/1.png)

```
Поле для вставки кода...
....
....
apt install postgresql
wget https://repo.zabbix.com/zabbix/7.4/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.4+debian13_all.deb
dpkg -i zabbix-release_latest_7.4+debian13_all.deb
apt update 
apt install zabbix-server-pgsql zabbix-frontend-php php8.4-pgsql zabbix-apache-conf zabbix-sql-scripts
sudo -u postgres createuser --pwprompt zabbix
sudo -u postgres createdb -O zabbix zabbix 
zcat /usr/share/zabbix/sql-scripts/postgresql/server.sql.gz | sudo -u zabbix psql zabbix 
systemctl restart zabbix-server zabbix-agent apache2
systemctl enable zabbix-server zabbix-agent apache2 
....
....
```

---

### Задание 2

Установите Zabbix Agent на два хоста.

```
Поле для вставки кода...
....
....
#На Zabbix server
sudo apt install zabbix-agent
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent 

#На VM 
sudo wget https://repo.zabbix.com/zabbix/7.4/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.4+debian13_all.deb
sudo dpkg -i zabbix-release_latest_7.4+debian13_all.deb
sudo apt update 
sudo apt install zabbix-agent
sudo nano /etc/zabbix/zabbix_agentd.conf
# изменил
#Server=192.168.31.167,192.168.56.0/24
#ServerActive=192.168.56.1
sudo systemctl restart zabbix-agent
sudo systemctl enable zabbix-agent 

....
....
```

![2.png](https://github.com/anarxim7/Postnikov_Igor-homework/blob/main/img/2.png)
![2.1.png](https://github.com/anarxim7/Postnikov_Igor-homework/blob/main/img/2.1.png)
![2.2.png](https://github.com/anarxim7/Postnikov_Igor-homework/blob/main/img/2.png)
![2.3.png](https://github.com/anarxim7/Postnikov_Igor-homework/blob/main/img/2.1.png)

