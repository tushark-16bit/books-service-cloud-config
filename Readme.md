
# Book Ecommerce Microservices

this project implements backend of a book ecommerce platform as microservices


## Documentation

These microservices were developed using **Hexagonal Architecture**(Ports and Adapters) using **DDD** principles. The microservice created around a domain implements business rules which are independent of techinal implementations.

The project structure implemented as:
- domain
- application
- infrastructure

Where Domain is responsible for the representation of domain objects, e.g Book in Book-Service, Cart in Cart-service and Purchase-Order in Purchase-service.

Application layer handles the business logic implemented with interfaces for the logic that may interact with outside. Also called Ports layer.
Application layer is further divided into **in** and **out**.
where in represents the input/ entrypoint into the service/ or driving port and out represents the connections to database or another microservices. Also called driven ports.

Infrastructure layer represents the implementation of application layer ports, where a web controller as driving port implementation or a database adapter as driven port implementation may be used.

The Incoming Request into application are seggregated as command and queries using CQRS principles, where the command(update) responsibilty is separated from Query(read) responsibilty.

There are three microservices implemented around domain:
- [Book-Service](https://github.com/tushark-16bit/books-microservice)
- [Cart-Service](https://github.com/tushark-16bit/cart-service)
- [Book-Purchase-Order-Service](https://github.com/tushark-16bit/book-purchase-service)

and [Api-Gateway](https://github.com/tushark-16bit/books-api-gateway) is used as security endpoint and loadbalancer for services


## Tech Stack

**Server:**
- Spring Boot
    - Web
    - Actuator
- Spring Cloud
    - Eureka Service Registry
    - Resilience4j
    - Tracing(Zipkin/Micrometer)
    - Api Gateway
- Spring Security
    - OAuth2 (Client / Resource Server)
    - Okta
- Spring Data
    - JPA
- Messaging
    - Kafka


## Deployment

To deploy this clone to local and run

```bash
  docker-compose up
```

Your Docker Daemon needs to be running for this command. This command will automatically fetch the packages deployed in different docker container registries and put up the application.

Initially, the user will have to login from browser using
```bash
    http:localhost:8080/auth/login
```
A sample Okta user has been created for running with security:

```bash
    username: sample.tester@gmail.com
    password: Okta@1User
```
The access token retrieved with this can then be used to authenticate requests to gateway exposed at port ```localhost:8080```

Please authenticate requests with oAuth2.0 and access token as retrieved above.

The endpoints exposed can be viewed from respective repository readme.md's.

**Tracing of requests** through the microservices exposed at
```http://localhost:9411/zipkin```


## Routing

Api gateway routes requests as following:

- ```http://localhost:8080/cart/**``` -> Load Balanced Instance of ```Cart-Service``` registered with Service Registry

- ```http://localhost:8080/book/**``` -> Load Balanced Instance of ```Book-Service``` registered with Service Registry

- ```http://localhost:8080/purchase/**``` -> Load Balanced Instance of ```Book-Purchase-Service``` registered with Service Registry


## Author

- [@tushark-16bit](https://github.com/tushark-16bit)


