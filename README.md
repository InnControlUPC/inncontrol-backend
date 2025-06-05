# 🧩 Microservices Root Repository

Este repositorio actúa como raíz para el conjunto de microservicios del sistema. Incluye la infraestructura compartida (como Kafka) y los microservicios principales agregados como **submódulos de Git**.

---

## 📁 Estructura del repositorio

```
.
├── kafka/                        # Infraestructura compartida (Kafka, Zookeeper, etc.)
│   └── docker-compose.yml
├── notifications-service/       # Microservicio: Notificaciones (submódulo)
├── auth-service/                # Microservicio: Autenticación (submódulo)
├── user-service/                # Microservicio: Gestión de usuarios (submódulo)
├── docker-compose.yml           # Orquestación general de servicios
└── README.md
```

---

## 🚀 Propósito

- Centralizar la configuración y orquestación local de los microservicios.
- Facilitar el onboarding del equipo técnico.
- Permitir levantar toda la arquitectura con `docker-compose`.

---

---
## 📚 Documentación 

Si vas usar kafka 
```yml
spring:
  application:
    name: {tu-servicio}

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
Debes reemplazar `{tu-servicio}` por el nombre de tu microservicio. y poner eso en tu application.yml o application.properties.
para que el microservicio se conecte a Kafka correctamente.

!OJO: Asegúrate de que el puerto `9092` esté disponible y que Kafka esté corriendo.

> Uso de eureka:
```yml
eureka:
  instance:
    instance-id: ${spring.application.name}:${random.value}
    prefer-ip-address: true
  client:
    service-url:
      defaultZone: ${EUREKA_SERVER_URL:http://localhost:8761/eureka}
```

> Reemplaza `${EUREKA_SERVER_URL}` por la URL de tu servidor Eureka si es necesario.


## 🔄 Cómo clonar correctamente este repositorio

> ⚠️ Este repositorio usa **submódulos**, asegúrate de clonar correctamente.

```bash
git clone --recurse-submodules https://github.com/InnControlUPC/inncontrol-backend.git
cd microservices-root
```

Si ya lo clonaste sin el flag `--recurse-submodules`, puedes correr:

```bash
git submodule update --init --recursive
```

---

## 🛠️ Comandos útiles

### Inicializar submódulos (si no están cargados)

```bash
git submodule update --init --recursive
```

### Actualizar submódulos a sus últimas versiones remotas

```bash
git submodule foreach git pull origin main
```

---

## 🐳 Levantar toda la infraestructura local

Asumiendo que tienes Docker y Docker Compose instalados:

```bash
docker-compose up --build
```

Esto levantará Kafka, Zookeeper y todos los microservicios configurados en el `docker-compose.yml` raíz.

---

## 📦 Agregar un nuevo microservicio como submódulo

```bash
git submodule add https://github.com/InnControlUPC/mi-nuevo-servicio.git nombre-del-servicio
git commit -am "Add nuevo microservicio"
git push
```

---

## ✅ Pasos para Modificar un Submódulo

> Si modificas un submódulo (por ejemplo, código dentro de un microservicio), recuerda:

1. **Entrar al submódulo**:
   ```bash
   cd nombre-submodulo
   
## 🧹 Buenas prácticas

- Si modificas un submódulo (código dentro del microservicio), recuerda:
  si estás en detached HEAD: git checkout main  # o la rama que uses (opcional)
  1. Hacer `commit` y `push` dentro del submódulo.
  2. Luego hacer `git add nombre-submodulo` en el repo raíz para actualizar el puntero.
  3. Finalmente `git commit` y `push` en el repo raíz.

---

## 👥 Contacto

Este repositorio es mantenido por el equipo técnico de **InnControlUPC**. Para soporte o dudas internas, contacta a tu equipo de infraestructura o DevOps.

