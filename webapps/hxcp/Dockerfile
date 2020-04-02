FROM java:8

MAINTAINER huangyabin <76788424@qq.com>

# 创建 /tmp 目录并持久化到Docker数据文件夹
# Spring Boot 使用的内嵌 Tomcat 容器默认使用 /tmp 作为工作目录
VOLUME /tmp 

# 将 jar/war 包添加到容器中并更名为app.jar/war
ADD hxcq-1.0.0.war app.war

EXPOSE 8080

# SecureRandom 在 java 各种组件中使用广泛，可以可靠的产生随机数，在大量产生的场景下，性能会较低
# 可以使用"-Djava.security.egd=file:/dev/./urandom"，加快随机数产生
ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/app.war"]
# ENTRYPOINT ["java", "-Djava.security.egd=file:/dev/./urandom", "-jar", "/app.war", "--spring.profiles.active=prod"]
