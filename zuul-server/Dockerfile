FROM openjdk:8-jdk-alpine
VOLUME /tmp
EXPOSE 8762
EXPOSE 5006
ENV JAVA_OPTS="-Xmx400m -Dfile.encoding=UTF-8 -agentlib:jdwp=transport=dt_socket,address=5006,server=y,suspend=n"
ADD target/zuul-server-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT exec java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app.jar
