# Use the official maven/Java 8 image to create a build artifact.
# https://hub.docker.com/_/maven
FROM maven:3.5-jdk-8-alpine as builder

FROM alpine:latest as packager

RUN apk --no-cache add openjdk11-jdk openjdk11-jmods

ENV JAVA_MINIMAL="/opt/java-minimal"

# build minimal JRE
RUN /usr/lib/jvm/java-11-openjdk/bin/jlink \
   --verbose \
   --add-modules \
       java.base,java.sql,java.naming,java.desktop,java.management,java.security.jgss,java.instrument \
   --compress 2 --strip-debug --no-header-files --no-man-pages \
   --release-info="add:IMPLEMENTOR=radistao:IMPLEMENTOR_VERSION=radistao_JRE" \
   --output "$JAVA_MINIMAL"

FROM alpine:latest

ENV JAVA_HOME=/opt/java-minimal
ENV PATH="$PATH:$JAVA_HOME/bin"

COPY --from=packager "$JAVA_HOME" "$JAVA_HOME"

ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java","-jar","/app.jar"]
