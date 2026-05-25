## 安装步骤

### 安装 zabbix-server

- 安装 zabbix-server 需要的依赖包。

```BASH
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-1+ubuntu22.04_all.deb
sudo dpkg -i zabbix-release_7.0-1+ubuntu22.04_all.deb
sudo apt update
sudo apt install zabbix-server-mysql zabbix-frontend-php zabbix-apache-conf zabbix-sql-scripts zabbix-agent
```

各包名作用`zabbix-server-mysql`：Zabbix 服务端（MySQL 后端）`zabbix-frontend-php`：Zabbix Web 界面（PHP）`zabbix-apache-conf`：Apache Web 服务器配置`zabbix-sql-scripts`：数据库初始化脚本`zabbix-agent`：Zabbix 监控代理。

- 在MySQL数据库中创建数据库和用户。

  ```MYSQL
  create database zabbix character set utf8mb4 collate utf8mb4_bin;
  CREATE USER 'zabbix'@'localhost' IDENTIFIED BY '你的密码';
  GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
  FLUSH PRIVILEGES;
  ```

- 导入数据库结构（初始化SQL）

  ```BASH
  sudo zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -u zabbix -p Yourpassword
  ```

- 配置 zabbix-server.conf 文件

```BASH
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=你的密码
```

- 设置开机自启

```bash
systemctl start zabbix-server
systemctl enable zabbix-server
```

- 配置前端web界面

```bash
# 安装 PHP 和 apache
sudo apt install php php-mysql php-gd php-xml php-bcmath php-gettext

sudo systemctl restart apache2
```

[^E: Unable to locate package php-gettext]: 输入命令：apt-get install --reinstall php-common

- 访问地址

```BASH
http://IP地址/zabbix

# 默认账户登录
用户名：Admin
密码：zabbix
```
