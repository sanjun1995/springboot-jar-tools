# springboot-jar-tools
 * 该工具主要用来辅助springboot项目打成Jar包后，部署到服务器使用。

## config外置配置文件
 * 启动时带上参数
```
java -jar xmltool-0.0.1-SNAPSHOT.jar --spring.config.location=./config/application.properties
```

## springboot.sh 一键启动、停止、重启springboot项目脚本
```
#!/bin/bash
APP_NAME=xmltool-0.0.1-SNAPSHOT.jar
APP_CONFIG_PATH="--spring.config.location=./conf/application.properties"
# 使用说明，用来提示输入参数
usage() {
    echo "Usage: sh 执行脚本.sh [start|stop|restart|status]"
    exit 1
}
# 检查程序是否在运行
is_exist() {
    pid=`ps -ef | grep $APP_NAME | grep -v grep | awk '{print $2}'`
    # 如果不存在返回1，存在返回0
    if [ -z "${pid}" ]
    then
        return 1
    else
        return 0
    fi
}
# 启动方法
start() {
    is_exist
    if [ $? -eq "0" ]
    then
        echo "${APP_NAME} is already running. pid=${pid} ."
    else
        nohup java -jar $APP_NAME $APP_CONFIG_PATH > /dev/null 2>&1 &
    fi
}
# 停止方法
stop() {
    is_exist
    if [ $? -eq "0" ]
    then
        kill -9 $pid
    else
        echo "${APP_NAME} is not running"
    fi
}
# 输出运行状态
status() {
    is_exist
    if [ $? -eq "0" ]
    then
        echo "${APP_NAME} is running, pid is ${pid}."
    else
        echo "${APP_NAME} is not runing."
    fi
}
# 重启
restart() {
    stop
    start
}
# 根据输入参数，选择执行对应方法，不输入则执行使用说明
case "$1" in
    "start")
        start
     ;;
    "stop")
        stop
     ;;
    "status")
        status
     ;;
    "restart")
        restart
     ;;
    *)
        usage
     ;;
esac
```
