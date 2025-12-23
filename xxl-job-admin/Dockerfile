# base image

# ===============================
# 方案1：在容器内构建（推荐）
# ===============================
FROM maven:3.9-eclipse-temurin-21 AS builder

# 设置工作目录 - 使用项目根目录
WORKDIR /app

# 复制源码到容器（构建上下文是项目根目录）
COPY . .

# 构建 JAR（跳过测试，加快速度）
RUN mvn clean package -DskipTests

# ===============================
# 方案2：直接使用预构建的JAR（更简单）
# ===============================
# 如果你已经在本地根目录执行过：mvn clean package -DskipTests
# 可以注释掉上面的构建阶段，取消下面的注释：
#
# FROM eclipse-temurin:21-jdk AS builder
# WORKDIR /app
# COPY ./xxl-job-admin/target/xxl-job-admin-*.jar ./app.jar

FROM eclipse-temurin:21-jdk

# maintainer
MAINTAINER xuxueli

# set params
ENV PARAMS=""

# set timezone
ENV TZ=PRC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# copy jar
COPY --from=builder /app/xxl-job-admin/target/xxl-job-admin-*.jar /app.jar

# command
# log home: -e LOG_HOME=/data/applogs
# jvm options: -e JAVA_OPTS="-Xms128m -Xmx128m"
# app params: -e PARAMS="--server.port=8080"
ENTRYPOINT ["sh","-c","java ${LOG_HOME:+-DLOG_HOME=$LOG_HOME} -jar $JAVA_OPTS /app.jar $PARAMS"]