# 🏨 Sistema de Gestión Hotelera - Arquitectura de Microservicios

## 📋 Descripción del Proyecto

Sistema distribuido basado en microservicios para la gestión integral de un hotel,
que incluye administración de hoteles, habitaciones, clientes y usuarios.
Cada microservicio es independiente, con su propia base de datos y lógica de negocio,
comunicándose entre sí mediante REST y centralizando el tráfico a través de un API Gateway.

---

## 👥 Integrantes del Equipo

| Nombre | Rol |
|--------|-----|
| [Tomás Octavio Acuña Maldonado] | Microservicios: ms-hoteles, ms-habitaciones |
| [Maxence Andre Jean Travers Bellais] | Microservicios: ms-usuarios, ms-clientes |

---

## 🧩 Microservicios Implementados

| Microservicio | Puerto | Base de Datos | Descripción |
|---------------|--------|---------------|-------------|
| ms-gateway | 8080 | - | API Gateway central |
| ms-hoteles | 8081 | hoteles_db | Gestión de hoteles |
| ms-usuarios | 8082 | usuarios_db | Gestión de usuarios del sistema |
| ms-clientes | 8083 | clientes_db | Gestión de clientes |
| ms-habitaciones | 8084 | habitaciones_db | Gestión de habitaciones por hotel |

---

## 🔀 Rutas principales del Gateway

Todas las peticiones se realizan a través del Gateway en `http://localhost:8080`

| Método | Ruta | Microservicio destino |
|--------|------|-----------------------|
| GET | `/api/hoteles` | ms-hoteles |
| GET | `/api/hoteles/{id}` | ms-hoteles |
| POST | `/api/hoteles` | ms-hoteles |
| PUT | `/api/hoteles/{id}` | ms-hoteles |
| DELETE | `/api/hoteles/{id}` | ms-hoteles |
| GET | `/api/hoteles/{id}/habitaciones` | ms-hoteles → ms-habitaciones |
| GET | `/api/habitaciones` | ms-habitaciones |
| GET | `/api/habitaciones/hotel/{hotelId}` | ms-habitaciones |
| POST | `/api/habitaciones` | ms-habitaciones |
| GET | `/api/usuarios` | ms-usuarios |
| POST | `/api/usuarios` | ms-usuarios |
| GET | `/api/clientes` | ms-clientes |
| POST | `/api/clientes` | ms-clientes |

---

## 📚 Documentación Swagger

| Microservicio | URL Swagger |
|---------------|-------------|
| ms-hoteles | http://localhost:8081/swagger-ui/index.html |
| ms-usuarios | http://localhost:8082/swagger-ui/index.html |
| ms-clientes | http://localhost:8083/swagger-ui/index.html |
| ms-habitaciones | http://localhost:8084/swagger-ui/index.html |

---

## ▶️ Instrucciones de Ejecución Local

### Prerrequisitos
- Java 21
- Maven
- MySQL corriendo en localhost:3306
- Crear las siguientes bases de datos vacías en MySQL:

```sql
CREATE DATABASE hoteles_db;
CREATE DATABASE usuarios_db;
CREATE DATABASE clientes_db;
CREATE DATABASE habitaciones_db;
```

### Orden de arranque

Levanta cada microservicio en este orden desde IntelliJ o con Maven:

```bash
# 1. Servidor de Descubrimiento (DEBE ir primero para que los demás puedan registrarse)
cd ms-eureka-server && mvn spring-boot:run

# 2. Microservicios de negocio (esperar a que Eureka esté arriba)
cd ms-usuarios     && mvn spring-boot:run
cd ms-clientes     && mvn spring-boot:run
cd ms-hoteles      && mvn spring-boot:run
cd ms-habitaciones && mvn spring-boot:run

# 3. Gateway (último, para que descubra las rutas registradas en Eureka)
cd ms-gateway      && mvn spring-boot:run

### Verificar que todo funciona

```bash
# Test rápido desde el Gateway
curl http://localhost:8080/api/hoteles
curl http://localhost:8080/api/usuarios
curl http://localhost:8080/api/clientes
curl http://localhost:8080/api/habitaciones
```

---

## 🧪 Pruebas Unitarias

Cada microservicio incluye pruebas unitarias con JUnit 5 y Mockito.

Para ejecutar los tests:

```bash
mvn test
```

Cobertura mínima alcanzada: **80%** sobre la capa de servicio y controlador.
