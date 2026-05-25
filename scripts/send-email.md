## 	01 脚本编写

```SHELL
vi /usr/lib/zabbix/alertscripts/mailx.sh

#!/bin/bash
LOG_FILE="/var/log/zabbix/mailx.log"

# 确保日志文件存在且可写
touch "$LOG_FILE" 2>/dev/null || {
echo "Cannot write to $LOG_FILE" >&2
exit 1
}

# 清理参数中的换行符
messages=$(echo "$3" | tr '\r\n' '\n')
subject=$(echo "$2" | tr '\r\n' '\n')
to=$1

# 构造完整邮件头（关键：必须包含 From 和 To）
(
echo "From: 2041682691@qq.com"
echo "To: $to"
echo "Subject: $subject"
echo "Content-Type: text/plain; charset=UTF-8"
echo ""                    # 空行分隔头部和正文
echo "$messages"           # 邮件正文
) | /usr/bin/msmtp -f 2041682691@qq.com -t >> "$LOG_FILE" 2>&1

result=$?

# 记录详细结果
echo "$(date '+%Y-%m-%d %H:%M:%S'): To=$to, Subject=$subject, ExitCode=$result" >> "$LOG_FILE"
```

