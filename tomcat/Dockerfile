FROM alpine:3.18

RUN apk add --no-cache openjdk17 curl bash

ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk
ENV PATH=$JAVA_HOME/bin:$PATH

WORKDIR /usr/local/tomcat

RUN curl -fsSL https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.49/bin/apache-tomcat-10.1.49.tar.gz -o tomcat.tar.gz && \
    tar -xzf tomcat.tar.gz && \
    mv apache-tomcat-10.1.49/* . && \
    rm -rf apache-tomcat-10.1.49 tomcat.tar.gz

# JDBC driver (optional)
RUN curl -fsSL https://repo1.maven.org/maven2/com/mysql/mysql-connector-j/8.3.0/mysql-connector-j-8.3.0.jar \
    -o /usr/local/tomcat/lib/mysql-connector-j.jar

# ✅ COPY APP INTO IMAGE ✅
COPY app/sample/index.jsp /usr/local/tomcat/webapps/sample

COPY tomcat/context.xml /usr/local/tomcat/conf/context.xml

EXPOSE 8080
CMD ["bin/catalina.sh", "run"]
