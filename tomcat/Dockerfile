FROM alpine:3.18

LABEL maintainer="Techie_Horizon"

# Install Java 17 & tools
RUN apk update && \
    apk add --no-cache openjdk17 curl bash

ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk
ENV PATH="${JAVA_HOME}/bin:${PATH}"

# Setup Tomcat
WORKDIR /usr/local/tomcat

RUN curl -fsSL https://dlcdn.apache.org/tomcat/tomcat-11/v11.0.14/bin/apache-tomcat-11.0.14.tar.gz -o tomcat.tar.gz && \
    tar -xzf tomcat.tar.gz && \
    mv apache-tomcat-11.0.14/* . && \
    rm -rf tomcat.tar.gz apache-tomcat-11.0.14

# Download MySQL JDBC driver automatically ✅
RUN curl -fsSL https://repo1.maven.org/maven2/com/mysql/mysql-connector-j/8.3.0/mysql-connector-j-8.3.0.jar \
    -o /usr/local/tomcat/lib/mysql-connector-j.jar

# Copy JNDI config
COPY tomcat/context.xml /usr/local/tomcat/conf/context.xml

EXPOSE 8080
CMD ["bin/catalina.sh", "run"]
