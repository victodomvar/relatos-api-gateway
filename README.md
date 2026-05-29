# relatos-api-gateway

API Gateway de Relatos de Papel construido con Spring Boot, Spring Cloud Gateway, Eureka Discovery Client y Actuator.

## Requisitos

- Java 17
- Maven 3.9+
- Eureka Server disponible en `http://localhost:8761/eureka`
- `catalogue-service` registrado en Eureka como `CATALOGUE-SERVICE`
- `orders-service` registrado en Eureka como `ORDERS-SERVICE`

## Configuracion principal

- Puerto del gateway: `8080`
- Nombre de aplicacion: `api-gateway`
- Registro Eureka: `http://localhost:8761/eureka`
- Ruta configurada: `/api/books/**` hacia `lb://CATALOGUE-SERVICE`
- Ruta configurada: `/api/orders/**` hacia `lb://ORDERS-SERVICE`
- Actuator expone: `health`, `info`, `gateway`

## Ejecutar

Desde esta carpeta:

```bash
mvn spring-boot:run
```

O compilar y ejecutar el JAR:

```bash
mvn clean package
java -jar target/relatos-api-gateway-0.0.1-SNAPSHOT.jar
```

## Probar

Con Eureka Server, `catalogue-service` y `orders-service` levantados:

```bash
curl http://localhost:8080/api/books
curl http://localhost:8080/api/orders
curl -X POST http://localhost:8080/api/orders \
  -H "Content-Type: application/json" \
  -d '{"bookId":1,"customerEmail":"cliente@example.com","quantity":2}'
```

El gateway mantiene el path original y enruta por service discovery hacia `lb://CATALOGUE-SERVICE` y `lb://ORDERS-SERVICE`.

Endpoints utiles de Actuator:

```bash
curl http://localhost:8080/actuator/health
curl http://localhost:8080/actuator/gateway/routes
```
