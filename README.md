# Microservicios: Canchas y Reservas

## Descripción

Este proyecto implementa una arquitectura de microservicios utilizando Spring Boot, donde se desarrollan dos servicios independientes:

- Servicio de Canchas
- Servicio de Reservas

La comunicación entre microservicios se realiza mediante **Spring Cloud OpenFeign**, que permite consumir APIs REST de forma declarativa.

---

## 🔗 Comunicación entre microservicios

Se utiliza OpenFeign para que el microservicio de Reservas pueda consultar información del microservicio de Canchas.

### ¿Cómo funciona?

- El servicio de Reservas necesita validar que una cancha exista antes de crear una reserva.
- Para esto, utiliza un cliente Feign que realiza una petición HTTP al servicio de Canchas.
- Si la cancha existe, se guarda la reserva.
- Si no existe, se lanza un error.

---

## ⚙️ Cambios implementados

Para integrar OpenFeign se realizaron los siguientes cambios:

### 1. Se agregó la dependencia:
xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>

### 2. Se habilitó Feign en la aplicación:
@EnableFeignClients

### 3. Se creó un cliente Feign:
@FeignClient(name = "cancha-service", url = "http://localhost:8081")
public interface CanchaClient {

    @GetMapping("/api/canchas/{id}")
    Cancha obtenerCancha(@PathVariable Long id);
}

### 4. Se reemplazó WebClient por Feign

Antes:

webClient.get()

Ahora:

canchaClient.obtenerCancha(id);


### Pruebas realizadas

Creación de cancha ✔️

Creación de reserva con cancha válida ✔️

Validación de error cuando la cancha no existe ✔️


### Tecnologías utilizadas

Java

Spring Boot

Spring Data JPA

MySQL

Spring Cloud OpenFeign

### Ejecución

Levantar servicio de canchas (puerto 8081)

Levantar servicio de reservas (puerto 8080)

Probar endpoints con Postman
