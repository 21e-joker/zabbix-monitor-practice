

## 01 安装mstmp包

```BASH
apt install msmtp msmtp-mta -y
```



## 02 msmtp文件配置

```BASH
vi /etc/msmtprc

# 默认设置
defaults
auth           on
tls            on
tls_trust_file /etc/ssl/certs/ca-certificates.crt
logfile        /var/log/msmtp.log

# QQ邮箱账号配置
account        qq
host           smtp.qq.com
port           587
from           <your-email>
user           <your-email>
password       <your-QQ授权码>

# 设置默认账户
account default : qq
```

## 03 设置权限

```bash
usermod -a -G zabbix root
chown zabbix:zabbix /tmp/mailx.log
chmod 664 /tmp/mailx.log
usermod -a -G zabbix root
```

## 04 验证测试

```BASH
# 测试命令
(
  echo "To: <your-email>"
  echo "Subject: 测试主题"
  echo ""
  echo "测试内容"
) | msmtp -f <your-email> -t

# 等待几秒就会收到
```

