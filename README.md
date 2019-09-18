# java_inda_cloud
Project to show transition from Monolith application to clustered microservice app

# Getting started

`git checkout monolith` to start.

## Monolith

`git checkout monolith` to return.

## Standalone

`git checkout standalone` to return.

## Microservices

`git checkout microservices` to return.

## Cloud orchestration

### Theory

Containers engines provide an ability to cut the corners of `VM` style solutions and instead of heavy Guest OS you
use layered containers.

### Develop

Create Dockerfiles `cat pc/Dockerfile`:

```
FROM openjdk:8-jdk-alpine
VOLUME /tmp
EXPOSE 8080
EXPOSE 5005
ENV JAVA_OPTS="-Xmx400m -Dfile.encoding=UTF-8 -agentlib:jdwp=transport=dt_socket,address=5005,server=y,suspend=n"
ADD target/pc-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT exec java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app.jar
```

Use Docker DNS hosts in `application.properties`:

```properties
eureka.instance.preferIpAddress=true
eureka.client.serviceUrl.defaultZone=http://eureka:8761/eureka/
spring.cloud.config.enabled=false
spring.application.name=pc
```

### Build

```bash
mvn clean package
```

### Prepare environment

Service Discovery:

```
services:
  eureka:
    image: springcloud/eureka
    ports:
      - "8761:8761"
```

### Deploy

```bash
docker-compose up -d

```

### Use

```bash
curl --request POST \
  --url http://localhost:8081/catalog \
  --header 'content-type: application/json' \
  --data '{
	"name": "Internet",
	"price": 500
}'
```

```bash
curl --request GET \
  --url http://localhost:8081/catalog/{specification_id}
```

```bash
curl --request PUT \
  --url http://localhost:8082/catalog/{specification_id}/order
```

### Drawbacks

- -Big codebase-
- -Risk: trespassing between modules-
- -Risk: problems with maintenance and evolution- ?
- -IDE load- ?
- -Web Container load-
- -Problems with Continuous Deployment- 
- -Limitations of environment scalability-
- -Problems with development scalability- 
- -Fixed technical stack-
- -Infrastructure dependencies- 
- Hard to test
- -Hard to configure deployment- 
- Resources consumption

