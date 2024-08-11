### 注意：所有操作必须使用root账户
### 测试过的系统版本 `Ubuntu 22.04.4 LTS`，低版本系统不能开机自启动，只能手动开启，需要守护外链节点请自行修改启动命令

1. 将下面脚本复制并直接在终端执行
``` BASH
cd /root
mkdir daemon
cd daemon
cat << EOF > "owner_daemon.sh"
#!/bin/bash
PRO_NAME='crust'
while true ; do
  NUM=\`ps aux | grep -i \${PRO_NAME} | grep -v grep |wc -l\`
  if [ "\${NUM}" -lt "1" ];then
    echo "进程重启"
    nohup  ./start_crust.sh >> ./output.log &
    echo "新的进程pid为：\$!"
  fi
  sleep 10
done
exit 0
EOF

cat << EOF > "start_crust.sh"
#!/bin/bash
##启动命令
crust start
EOF

cat << EOF > "/etc/init.d/owner_daemon.sh"
#!/bin/bash

### BEGIN INIT INFO
# Provides:     owner_daemon
# Required-Start:  \$all
# Required-Stop:   \$all
# Default-Start:   2 3 4 5
# Default-Stop:   0 1 6
# Short-Description: start owner_daemon.sh
# Description:    start owner_daemon.sh
### END INIT INFO

case "\$1" in
    start)
        # 在这里添加你的启动命令
        echo "Starting owner_daemon..."
        cd /root/daemon
        ./owner_daemon.sh &
        ;;
    stop)
        # 在这里添加你的停止命令
        echo "Stopping owner_daemon..."
        pkill -f "\.\/owner_daemon.sh"
        ;;
    *)
        echo "Usage: /etc/init.d/owner_daemon {start|stop}"
        exit 1
esac

exit 0
EOF


chmod +x *.sh
chmod +x /etc/init.d/owner_daemon.sh

update-rc.d owner_daemon.sh defaults

```

- 启动守护进程 `service owner_daemon.sh start` 或 `/etc/init.d/owner_daemon.sh start`
- 关闭守护进程 `service owner_daemon.sh stop` 或 `/etc/init.d/owner_daemon.sh stop`
- 永久删除守护进程 `/etc/init.d/update-rc.d -f owner_daemon.sh remove`