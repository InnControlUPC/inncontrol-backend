# 🧩 InnControl Backend – Microservices Root Repository

Este repositorio actúa como punto central para el ecosistema de microservicios de **InnControl**. Aquí se encuentra la orquestación completa mediante `docker-compose`, junto con todos los servicios (algunos como submódulos) y la infraestructura común.

---

## 📁 Estructura del repositorio

```
inncontrol-backend/
├── accommodation-service/       # Microservicio: Gestión de alojamientos
├── api-gateway/                 # API Gateway central
├── communications-service/      # Microservicio: Comunicaciones internas (mensajería)
├── employee-service/            # Microservicio: Gestión de empleados
├── eureka-server/               # Eureka Server para descubrimiento de servicios
├── inventory-service/           # Microservicio: Gestión de inventario
├── kafka-service/               # Configuración Kafka/Zookeeper (opcional si separado)
├── notifications-service/       # Microservicio: Notificaciones asíncronas
├── task-service/                # Microservicio: Gestión de tareas
├── docker-compose.yml           # Orquestación de todos los servicios
└── README.md                    # Este archivo
```

---

## 🚀 Propósito

- Proveer un entorno local para desarrollo completo de la plataforma InnControl.
- Simplificar el arranque y prueba de todos los microservicios.
- Centralizar la configuración compartida como Eureka, Kafka y bases de datos.

---

## 🐳 Levantar todo el ecosistema local

Asegúrate de tener **Docker** y **Docker Compose** instalados.

```bash
docker-compose up --build
```

Este comando iniciará:
- **Zookeeper & Kafka**
- **Eureka Server**
- Todos los microservicios (empleados, inventario, tareas, etc.)
- Bases de datos MySQL para cada microservicio con persistencia

---

## 🔗 Integraciones esenciales

### Uso de Kafka en un microservicio

Agrega la siguiente configuración a tu `application.yml` o `application.properties`:

```yaml
spring:
  application:
    name: nombre-del-servicio

  kafka:
    bootstrap-servers: ${KAFKA_BOOTSTRAP_SERVERS:localhost:9092}
    producer:
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.springframework.kafka.support.serializer.JsonSerializer
    consumer:
      group-id: inncontrol-group
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.springframework.kafka.support.serializer.JsonDeserializer
      properties:
        spring.json.trusted.packages: "*"
```

📌 Reemplaza `nombre-del-servicio` por el identificador único de tu microservicio.

---

### Uso de Eureka

```yaml
eureka:
  instance:
    instance-id: ${spring.application.name}:${random.value}
    prefer-ip-address: true
  client:
    service-url:
      defaultZone: ${EUREKA_SERVER_URL:http://localhost:8761/eureka}
```

---

## 📦 Agregar un nuevo microservicio como submódulo

```bash
git submodule add https://github.com/InnControlUPC/mi-nuevo-servicio.git nombre-del-servicio
git commit -am "Add nuevo microservicio"
git push
```

---

## 🛠️ Comandos útiles

### Clonar el repositorio con submódulos

```bash
git clone --recurse-submodules https://github.com/InnControlUPC/inncontrol-backend.git
cd inncontrol-backend
```

Si ya lo habías clonado sin ese flag:

```bash
git submodule update --init --recursive
```

### Actualizar todos los submódulos a la última versión remota

```bash
git submodule foreach git pull origin main
```

---

## ✅ Buenas prácticas con submódulos

Si modificas un microservicio (submódulo):

1. Entra al submódulo:
   ```bash
   cd nombre-submodulo
   git checkout main  # o tu rama
   ```
2. Realiza los commits y push normalmente dentro del submódulo.
3. Luego vuelve al repo raíz y actualiza el puntero:
   ```bash
   cd ..
   git add nombre-submodulo
   git commit -m "Update submodule pointer"
   git push
   ```

---

## 👥 Contacto

Este repositorio es mantenido por el equipo técnico de **InnControlUPC**.  
Para dudas técnicas o soporte, contacta con tu responsable de DevOps o infraestructura.

---