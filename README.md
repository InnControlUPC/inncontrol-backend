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

## 🔄 Cómo clonar correctamente este repositorio

> ⚠️ Este repositorio usa **submódulos**, asegúrate de clonar correctamente.

```bash
git clone --recurse-submodules https://github.com/InnControlUPC/microservices-root.git
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

## 🧹 Buenas prácticas

- Si modificas un submódulo (código dentro del microservicio), recuerda:
  1. Hacer `commit` y `push` dentro del submódulo.
  2. Luego hacer `git add nombre-submodulo` en el repo raíz para actualizar el puntero.
  3. Finalmente `git commit` y `push` en el repo raíz.

---

## 👥 Contacto

Este repositorio es mantenido por el equipo técnico de **InnControlUPC**. Para soporte o dudas internas, contacta a tu equipo de infraestructura o DevOps.

