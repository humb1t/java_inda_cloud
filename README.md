# java_inda_cloud
Project to show transition from Monolith application to clustered microservice app

# Getting started

`git checkout monolith` to start.

## Monolith

### Theory

Traditional and streightforward approach - you have an application server with deployed `WAR`.
Simple to:
- develop
- test
- deploy
- scale

### Develop

- `cat src/main/java/org/nipu/po/`
- `cat src/main/java/org/nipu/po/PoApplication.java` - usual Spring Boot application
- `cat src/main/java/org/nipu/po/ServletInitializer.java` - Servlet Initializer for Application Server
- `cat src/main/java/org/nipu/po/catalog/ProductSpecificationRepository.java` - RESTfull repository
- `cat src/main/java/org/nipu/po/order/OrderController.java` - custom action needs controller

### Build

```bash
./mvnw clean package
```
### Prepare environment

```bash
docker-compose up -d
```

### Deploy

Then open `http://localhost:8080/manager/html` in browser  with `user\password` credentials and upload your build.

### Use

```bash
curl --request POST \
  --url http://localhost:8080/po-0.0.1-SNAPSHOT/catalog \
  --header 'content-type: application/json' \
  --data '{
	"name": "Internet",
	"price": 500
}'
```

```bash
curl --request GET \
  --url http://localhost:8080/po-0.0.1-SNAPSHOT/catalog/{specification_id}
```

```bash
curl --request PUT \
  --url http://localhost:8080/po-0.0.1-SNAPSHOT/catalog/{specification_id}/order
```

### Drawbacks

- Big codebase
- Risk: trespassing between modules
- Risk: problems with maintenance and evolution
- IDE load
- Web Container load
- Problems with Continuous Deployment
- Limitations of environment scalability
- Problems with development scalability
- Fixed technical stack
- Infrastructure dependencies

## Standalone

`git checkout standalone` to continue.
