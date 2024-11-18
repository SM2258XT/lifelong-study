---
title: GC日志详情
updated: 2022-11-22T18:26:13
created: 2022-11-22T18:22:17
---

内存问题排查什么最重要？当然是信息收集，留下一些为我们的排查提供支持的依据，强烈建议把GC日志输出的详细一些
JDK8或者以下的GC日志参数：
\#!/bin/sh
LOG_DIR="/tmp/logs"
JAVA_OPT_LOG=" -verbose:gc"
JAVA_OPT_LOG="\${JAVA_OPT_LOG} -XX:+PrintGCDetails"
JAVA_OPT_LOG="\${JAVA_OPT_LOG} -XX:+PrintGCDateStamps"
JAVA_OPT_LOG="\${JAVA_OPT_LOG} -XX:+PrintGCApplicationStoppedTime"
JAVA_OPT_LOG="\${JAVA_OPT_LOG} -XX:+PrintTenuringDistribution"
JAVA_OPT_LOG="\${JAVA_OPT_LOG} -Xloggc:\${LOG_DIR}/gc\_%p.log"

JAVA_OPT_OOM=" -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=\${LOG_DIR} -XX:ErrorFile=\${LOG_DIR}/hs_error_pid%p.log "

JAVA_OPT="\${JAVA_OPT_LOG} \${JAVA_OPT_OOM}"
JAVA_OPT="\${JAVA_OPT} -XX:-OmitStackTraceInFastThrow"

**读懂GC日志**
GC日志并不需要一行一行全看一遍，可以使用辅助工具GcEasy进行分析

