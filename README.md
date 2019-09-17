# java_inda_cloud
Project to show transition from Monolith application to clustered microservice app

# Getting started

`git checkout monolith` to start.

## Monolith

`git checkout monolith` to return.

## Standalone

`git checkout standalone` to return.

## Microservices

### Theory

Microservices - set of *loosely* coupled services.

### Develop

Split your application into several:

```xml
    <modules>
        <module>pc</module>
        <module>po</module>
    </modules>
```

Catalog in our case would be the same: `cat pc/src/main/java/org/nipu/pc/catalog/ProductSpecificationRepository.java`.

Ordering would have a new configuration: `cat po/src/main/java/org/nipu/po/FeignConfiguration.java`.
Feign clients make it easier to communicate with different services, as it simply another Java object.
Let's look into ` po/src/main/java/org/nipu/po/order/clients/ProductSpecificationRepository.java`:

```java
@FeignClient(name = "catalog", url = "localhost:8081", configuration = FeignConfiguration.class)
public interface ProductSpecificationRepository {

    @RequestMapping(method = RequestMethod.GET, path = "/catalog/{specificationId}")
    Object existsById(@PathVariable("specificationId") String specificationId);
}
```

And now we can use another services as usual ` po/src/main/java/org/nipu/po/order/OrderController.java`:

```java
@RestController
public class OrderController {
    private final ProductOrderRepository orderRepository;
    private final ProductSpecificationRepository specificationRepository;

    public OrderController(ProductOrderRepository orderRepository, ProductSpecificationRepository specificationRepository) {
        this.orderRepository = orderRepository;
        this.specificationRepository = specificationRepository;
    }

    @PutMapping("/catalog/{specificationId}/order")
    public ProductOrder orderProductBySpecificationId(@PathVariable String specificationId) {
        if (specificationRepository.existsById(specificationId) == null) {
            throw new RuntimeException("There is no product specification with Id: " + specificationId);
        }
        return orderRepository.save(new ProductOrder(null, specificationId, 1l));
    }
}
```

### Build

```bash
mvn clean package
```

### Prepare environment

```bash
docker-compose up -d
```

Update `application.properties` and set different ports for your services.

### Deploy

```bash
java -jar pc/target/pc-0.0.1-SNAPSHOT.jar

```

```bash
java -jar po/target/po-0.0.1-SNAPSHOT.jar
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
- -Infrastructure dependencies- ?

Microservices specific:

- Hard to test
- Hard to configure deployment
- Resources consumption

## Containerization

`git checkout containers` to continue.


