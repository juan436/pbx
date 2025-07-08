# 🎙️ PBX con FreePBX en Docker

[![Docker](https://img.shields.io/badge/Docker-2CA5E0?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![FreePBX](https://img.shields.io/badge/FreePBX-17.0-0077b5?style=for-the-badge&logo=asterisk&logoColor=white)](https://www.freepbx.org/)
[![MariaDB](https://img.shields.io/badge/MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)](https://mariadb.org/)

## 📋 Descripción

Este repositorio contiene la configuración para desplegar una central telefónica IP (PBX) basada en FreePBX 17.0 utilizando Docker Compose. La solución incluye todos los componentes necesarios para un sistema de telefonía IP completo, incluyendo base de datos, servidor web y servicios de telefonía.

## 🚀 Características Principales

- **FreePBX 17.0** - Interfaz web de administración
- **MariaDB** - Base de datos para configuraciones y registros
- **Apache** - Servidor web local
- **Soporte para protocolos VoIP**: SIP, IAX2
- **Registro de llamadas detallado (CDR)**
- **Configuración persistente** mediante volúmenes Docker

## 🛠 Requisitos Previos

- Docker 20.10.0 o superior
- Docker Compose 1.29.0 o superior
- 2 GB de RAM mínimo (4 GB recomendado)
- 20 GB de espacio en disco
- Puertos 80, 443, 5060, 5160, 4569, 10000-20000/udp disponibles en localhost

## 🚀 Instalación Rápida

1. **Clonar el repositorio**
   ```bash
   git clone [URL_DEL_REPOSITORIO]
   cd pbx
   ```

2. **Configurar variables de entorno**
   Editar el archivo `.env` y modificar las credenciales por defecto:
   ```
   MYSQL_ROOT_PASSWORD=tu_contraseña_segura
   MYSQL_DATABASE=asterisk
   MYSQL_USER=asterisk
   MYSQL_PASSWORD=tu_contraseña_segura
   ```

3. **Iniciar los contenedores**
   ```bash
   docker-compose up -d
   ```

4. **Acceder a la interfaz web**
   Abrir en el navegador: `http://localhost` o `http://[tu-ip-local]`
   
   - **Usuario**: admin
   - **Contraseña**: (configurada durante la instalación)

## 🔧 Configuración de Red Local

El sistema está configurado con una red local aislada:
- Red `asterisk` para comunicación interna entre contenedores
- Acceso desde la red local mediante la IP del host

## 🔒 Seguridad Local

### Recomendaciones para uso en red local:

1. **Cambiar contraseñas por defecto**
   - Usuario de la base de datos
   - Usuario administrador de FreePBX
   - Extensiones SIP

2. **Restringir acceso**
   - Configurar el firewall del sistema operativo
   - Considerar el uso de VPN para acceso remoto

## 📊 Puertos Expuestos

| Puerto  | Protocolo | Uso                     |
|---------|-----------|-------------------------|
| 80      | TCP       | Interfaz web HTTP       |
| 443     | TCP       | Interfaz web HTTPS*     |
| 5060    | UDP/TCP   | Tráfico SIP             |
| 5160    | UDP       | Tráfico SIP adicional   |
| 4569    | UDP       | Protocolo IAX2          |
| 10000-20000 | UDP    | Flujo de medios RTP     |

> *Nota: La configuración HTTPS no está habilitada por defecto en esta instalación local.

## 📂 Estructura del Proyecto

```
.
├── datadb/             # Datos persistentes de MariaDB
├── sql/                # Scripts SQL de inicialización
│   └── 01-databases.sql
├── docker-compose.yml  # Configuración de Docker Compose
├── odbc.ini           # Configuración ODBC
└── odbcinst.ini       # Configuración de drivers ODBC
```

## 🛠 Mantenimiento

### Copias de seguridad
```bash
# Base de datos
docker exec freepbx_mariadb mysqldump -u asterisk -p'asterisk' --all-databases > backup_$(date +%Y%m%d).sql

# Configuraciones
docker cp freepbx_server:/etc/asterisk/ ./config_backup_$(date +%Y%m%d)
```

### Actualización
```bash
docker-compose pull
docker-compose down
docker-compose up -d
```

## 🤝 Contribuir

Las contribuciones son bienvenidas. Por favor, abre un issue para discutir los cambios propuestos antes de hacer un pull request.

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para más detalles.

---

<div align="center">
  Hecho con ❤️ para entornos de desarrollo y pruebas locales
</div>
