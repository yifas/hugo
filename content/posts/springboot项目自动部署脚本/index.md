+++
date = '2025-08-11T14:19:44+08:00'
draft = false
title = 'Springboot项目自动部署脚本'
categories = ['SpringBoot']
tags =  ["SpringBoot", "Linux"]
+++

一、使用方法

> 将脚本上传至jar包同级目录，运行shell脚本，自动查找同名jar包，并且停止，自动运行新jar包。

二、shell脚本

```shell
#!/bin/bash
# spring-boot-deploy.sh - 自动化部署Spring Boot应用
# 使用说明：将此脚本与Jar包放在同一目录，运行 ./spring-boot-deploy.sh 或者 sh spring-boot-deploy.sh

# ===== 用户配置区 =====
APP_NAME="tendersmi-main-2.0.0-SNAPSHOT"      # 应用名称（必须修改）
JAVA_OPTS="-Xmx512m"         # JVM参数
SPRING_OPTS=""               # Spring参数（如：--spring.profiles.active=prod）
# =====================

# 获取当前目录
DEPLOY_DIR=$(pwd)

# 检查是否传入新Jar文件名
if [ -n "$1" ]; then
    NEW_JAR="$1"
else
    # 自动查找目录中最新的Jar文件
    NEW_JAR=$(ls -t *.jar | head -1)
fi

# 检查Jar文件是否存在
if [ ! -f "$NEW_JAR" ]; then
    echo "错误：未找到Jar文件！"
    exit 1
fi

echo "================================================"
echo "开始部署 ${APP_NAME}"
echo "部署目录: ${DEPLOY_DIR}"
echo "新Jar文件: ${NEW_JAR}"
echo "================================================"

# 1. 查找旧进程并停止
echo ">> 停止旧服务..."
PID=$(pgrep -f "java.*${APP_NAME}")
if [ -n "$PID" ]; then
    echo "发现运行中的进程: $PID"
    kill $PID
    # 等待最多10秒直到进程停止
    for i in {1..10}; do
        if ! ps -p $PID > /dev/null; then
            echo "进程 $PID 已停止"
            break
        fi
        sleep 1
        echo "等待进程停止...(${i}/10)"
    done
    # 强制停止如果仍未退出
    if ps -p $PID > /dev/null; then
        echo "强制停止进程 $PID"
        kill -9 $PID
    fi
else
    echo "未找到运行中的进程"
fi

# 2. 备份旧版本（按日期时间戳） 不生效移除了

# 3. 重命名新Jar为应用名
APP_JAR="${APP_NAME}.jar"
if [ ! "$NEW_JAR" = "$APP_JAR" ]; then
    echo "重命名: ${NEW_JAR} -> ${APP_JAR}"
    mv "$NEW_JAR" "$APP_JAR"
fi

# 4. 启动新服务
echo ">> 启动新服务..."
nohup java $JAVA_OPTS -jar $APP_JAR $SPRING_OPTS > console.log 2>&1 &

# 5. 检查启动状态
echo ">> 检查启动状态..."
sleep 3  # 等待进程启动

NEW_PID=$(pgrep -f "java.*${APP_NAME}")
if [ -n "$NEW_PID" ]; then
    echo "================================================"
    echo "✅ 部署成功! 应用已启动"
    echo "PID: $NEW_PID"
    echo "日志: tail -f ${DEPLOY_DIR}/console.log"
    echo "控制台: nohup.out"
    echo "================================================"
else
    echo "================================================"
    echo "❌ 启动失败! 请检查日志"
    echo "查看日志: tail -f ${DEPLOY_DIR}/console.log"
    echo "================================================"
    exit 2
fi
```

