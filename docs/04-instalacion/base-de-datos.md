# Instalación y Configuración del Servidor de Base de Datos MySQL

Este documento describe el despliegue del sistema gestor de base de datos MySQL Server para dar servicio a la web corporativa y a la gestión interna.

## 📥 Instalación de MySQL

Instalamos el servidor de bases de datos desde los repositorios de la distribución:

```bash
# Instalar el paquete de MySQL Server
sudo apt update
sudo apt install -y mysql-server
```

### Verificar el Estado del Servicio
```bash
sudo systemctl status mysql
```

---

## 🔒 Bastionado Inicial (Seguridad)

Es fundamental aplicar buenas prácticas de seguridad inmediatamente después de la instalación:

```bash
# Ejecutar el script interactivo de seguridad
sudo mysql_secure_installation
```

Se recomienda configurar:
1.  Activar el plugin de validación de contraseñas (`VALIDATE PASSWORD COMPONENT`).
2.  Definir contraseñas seguras para la cuenta root de MySQL.
3.  Eliminar usuarios anónimos.
4.  Deshabilitar el acceso remoto para el usuario root.
5.  Eliminar la base de datos de pruebas (`test`).
6.  Recargar la tabla de privilegios.

---

## 🗄️ Creación de Bases de Datos y Usuarios

El cliente necesita dos bases de datos y dos usuarios independientes para mitigar riesgos de seguridad.

1.  Acceder a la consola de MySQL:
    ```bash
    sudo mysql -u root
    ```

2.  Ejecutar las siguientes consultas SQL:

    ```sql
    -- Creación de bases de datos independientes
    CREATE DATABASE db_web CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
    CREATE DATABASE db_gestion CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

    -- Crear usuario para la web corporativa y asignar permisos en su BD
    CREATE USER 'user_web'@'localhost' IDENTIFIED BY 'ContraseñaSeguraWeb2026!';
    GRANT ALL PRIVILEGES ON db_web.* TO 'user_web'@'localhost';

    -- Crear usuario para la gestión interna y asignar permisos en su BD
    CREATE USER 'user_gestion'@'localhost' IDENTIFIED BY 'ContraseñaSeguraGestion2026!';
    GRANT ALL PRIVILEGES ON db_gestion.* TO 'user_gestion'@'localhost';

    -- Recargar los privilegios
    FLUSH PRIVILEGES;
    EXIT;
    ```
