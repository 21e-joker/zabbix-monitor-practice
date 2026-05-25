核心配置：

```BASH
vi /etc/zabbix/zabbix_proxy.conf


Server=<your-Zabbix Server IP或域名>
Hostname=<Proxy名称>	#（必须和 Server 前端配置一致）
DBName=zabbix_proxy
DBUser=zabbix_proxy
DBPassword=<your-password>
```

