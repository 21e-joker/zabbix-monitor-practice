### 安装proxy

- 安装 zabbix 官方仓库

```BASH
wget https://repo.zabbix.com/zabbix/7.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_7.0-2+ubuntu$(lsb_release -rs)_all.deb
sudo dpkg -i zabbix-release_7.0-2+ubuntu$(lsb_release -rs)_all.deb
sudo apt update
```

- 安装 zabbix Proxy (含MySQL支持)

```BASH
sudo apt install -y zabbix-proxy-mysql zabbix-sql-scripts
```

[^TIPS]: 如果使用 **PostgreSQL**，将 `zabbix-proxy-mysql` 替换为 `zabbix-proxy-pgsql`

如果使用 **SQLite**（轻量级），替换为 `zabbix-proxy-sqlite3`

- 创建数据库并导入表结构

```BASH
# 登录 MySQL
sudo mysql -uroot -p

# 在 MySQL 中执行：
CREATE DATABASE zabbix_proxy CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER 'zabbix_proxy'@'localhost' IDENTIFIED BY '你的密码';
GRANT ALL PRIVILEGES ON zabbix_proxy.* TO 'zabbix_proxy'@'localhost';
SET GLOBAL log_bin_trust_function_creators = 1;
QUIT;

# 导入初始表结构
zcat /usr/share/zabbix-sql-scripts/mysql/proxy.sql.gz | mysql -u zabbix_proxy -p zabbix_proxy
```

- 配置 Proxy 连接 zabbix-server

```BASH
# 编辑配置文件 /etc/zabbix/zabbix_proxy.conf
sudo vi /etc/zabbix/zabbix_proxy.conf
# 关键配置项（按需修改）：
Server=你的Zabbix Server IP或域名
Hostname=Proxy名称（必须和 Server 前端配置一致）
DBName=zabbix_proxy
DBUser=zabbix_proxy
DBPassword=你的密码
```

- 启动并设置开机自启

```BASH
sudo systemctl restart zabbix-proxy
sudo systemctl enable zabbix-proxy
sudo systemctl status zabbix-proxy
```

[^TIPS]: 在配置完代理后，如需监控一台主机，还需将proxyIP加入到对应的被监控主机agent中，一般是：vi /etc/zabbix/zabbix_agent2.conf

```bash
Server=proxyIP     #一般可以是可以用','添加，或者直接添加一个网段
```

