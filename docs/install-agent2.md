### 安装agent2

- 下载安装源

```BASH
# 使用清华镜像加速（国内推荐）
rpm -Uvh https://mirrors.tuna.tsinghua.edu.cn/zabbix/zabbix/7.0/rhel/7/x86_64/zabbix-release-7.0-1.el7.noarch.rpm

yum clean all
yum install -y zabbix-agent2
```

- 配置agent2

```BASH
# 编辑配置文件 /etc/zabbix/zabbix_agent2.conf
vi /etc/zabbix/zabbix_agent2.conf

# 修改一下关键参数：
# Zabbix Server/Proxy 的 IP 地址（多个用逗号分隔）
Server=192.168.1.100

# 主动模式：Server/Proxy 的 IP 和端口（通常与 Server 相同）
ServerActive=192.168.1.100:10051

# 本机主机名（必须与 Zabbix Server 上配置的主机名一致）
Hostname=CentOS7-Agent

# 可选：启用远程命令（根据需要）
EnableRemoteCommands=1

```

- 启动并设置开机自启

```BASH
systemctl start zabbix-agent2
systemctl enable zabbix-agent2
systemctl status zabbix-agen
```
