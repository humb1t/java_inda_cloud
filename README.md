# java_inda_cloud
Project to show transition from Monolith application to clustered microservice app

# Getting started

`git checkout monolith` to start.

## Monolith

`git checkout monolith` to return.

## Standalone

### Theory

Self-executable applications. In Java world mostly `JAR` file with embedded application server.

### Develop

No need in Servlet Initializer.
```xml
    <packaging>jar</packaging>
```

instead of

```xml
    <packaging>war</packaging>
```

### Build

```bash
mvn clean package
```

### Prepare environment

Change `application.properties` and set MongoDB Host to `localhost`

### Deploy

```bash
java -jar target\po-0.0.1-SNAPSHOT.jar
```

### Use

```bash
curl --request POST \
  --url http://localhost:8080/catalog \
  --header 'content-type: application/json' \
  --data '{
	"name": "Internet",
	"price": 500
}'
```


```bash
curl --request GET \
  --url http://localhost:8080/catalog/{specification_id}
```

```bash
curl --request PUT \
  --url http://localhost:8080/catalog/{specification_id}/order
```

### Drawbacks

- Big codebase
- Risk: trespassing between modules
- -Risk: problems with maintenance and evolution- ?
- IDE load
- Web Container load
- -Problems with Continuous Deployment- ?
- Limitations of environment scalability
- -Problems with development scalability- ?
- Fixed technical stack
- -Infrastructure dependencies- ?

## Microservices

`git checkout microservices` to continue.


