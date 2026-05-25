##  问题记录

### Problem 01 - 数据库初始化失败

#### Error - 01

```bash
ERROR 1050 (42S01): Table 'role' already exists
```

#### Cause

数据库已存在旧表结构，导致初始化SQL重复执行。

#### Solution

```MYSQL
DROP DATABASE IF EXISTS zabbix;
```

#### Error - 02

```BASH
错误：遇权限错误（如 `SUPER privilege` 报错）
```

#### Cause

权限不够，导致没有权限执行初始化。

#### Solution

```BASH
# 需要在MySQL配置文件中配置：
SET GLOBAL log_bin_trust_function_creators = 1;
```

### Problem - 02 权限不够导致的 zabbix-server 启动失败

```BASH
Job for zabbix-server.service failed because the service did not take the steps required by its unit configuration.
See "systemctl status zabbix-server.service" and "journalctl -xeu zabbix-server.service" for details.

# 解决：
	
```

#### Error - 01

```BASH
Can't open PID file /run/zabbix/zabbix_server.pid (yet?) after start: Operation not permitted.
```

#### Cause

没有权限，导致无法启动 zabbix-server进程。

#### Solution

```BASH
# 修改对应没有的目录的权限，让 zabbix 用户拥有该目录
    sudo chown -R zabbix:zabbix /run/zabbix
    sudo chmod 755 /run/zabbix
```

#### Error - 02

```bash
# 修改后还是这个问题
Can't open PID file /run/zabbix/zabbix_server.pid (yet?) after start: Operation not permitted.
```

#### Cause

systemd启动配置中未指定zabbix-server启动用户为zabbix，导致无权限。

#### Solution

```BASH
# 设置后还是不成功，检查systemd服务配置
systemctl cat zabbix-server

# 解决：
	# 特别注意 User 和 Group 设置
	systemctl show zabbix-server | grep -E "User|Group|RuntimeDirectory"
	# 没有配置 User 和 Group，手动添加
	vi /lib/systemd/system/zabbix-server.service
    # 添加下面的内容
    [Service]
    User=zabbix
    Group=zabbix
    # 重新加载systemd配置
    systemctl daemon-reload
    # 修复目录权限
    sudo chown -R zabbix:zabbix /run/zabbix
    sudo chmod 755 /run/zabbix
    #  清理旧 PID 文件
    sudo rm -f /run/zabbix/zabbix_server.pid
    # 启动服务
    sudo systemctl start zabbix-server
    # 查看状态
    sudo systemctl status zabbix-server
```
