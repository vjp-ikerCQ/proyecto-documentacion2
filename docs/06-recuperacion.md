# 06. Plan de Recuperación ante Desastres (Disaster Recovery)

Este documento detalla el procedimiento paso a paso para restaurar los servicios en caso de pérdida total de datos, fallos graves de hardware o corrupción del sistema.

## 🏁 Requisitos Previos

Antes de iniciar la recuperación, asegúrate de tener:
*   Un servidor Ubuntu 22.04 LTS recién instalado y con la pila LAMP montada.
*   Acceso a los archivos de backup generados (`web-YYYY-MM-DD.tar.gz` y `db-all-YYYY-MM-DD.sql.gz`).

---

## 💾 Restauración de la Base de Datos (MySQL)

Para recuperar las bases de datos de la web corporativa y la gestión interna:

1.  Descomprimir e importar el volcado de base de datos desde el backup:
    ```bash
    # Descomprimir el archivo sql.gz (cambiar fecha según el backup elegido)
    gunzip -c /var/backups/pyme/db-all-2026-06-03.sql.gz > /tmp/backup.sql

    # Importar el volcado a la base de datos MySQL local como administrador
    sudo mysql -u root < /tmp/backup.sql
    ```
2.  Verificar que las bases de datos `db_web` y `db_gestion` se hayan creado correctamente:
    ```bash
    sudo mysql -u root -e "SHOW DATABASES;"
    ```

---

## 📁 Restauración de Archivos Web y Configuración

Para recuperar la estructura de archivos y ficheros de configuración de Apache y MySQL:

1.  Extraer el backup en la raíz del servidor:
    ```bash
    # Restaurar archivos web y configuraciones
    sudo tar -xzf /var/backups/pyme/web-2026-06-03.tar.gz -C /
    ```
2.  Verificar los permisos del directorio web:
    ```bash
    sudo chown -R www-data:www-data /var/www/html/pyme
    ```
3.  Habilitar el sitio en Apache y reiniciar los servicios principales:
    ```bash
    sudo a2ensite pyme.conf
    sudo systemctl restart apache2
    sudo systemctl restart mysql
    ```

---

## 🧪 Pruebas de Validación del Sistema

Una vez finalizado el proceso, comprueba que los servicios respondan correctamente:

1.  **Capa Web:** Acceder a la URL `http://pyme.local` y comprobar la carga de la página.
2.  **Base de Datos:** Comprobar la conexión con los usuarios específicos:
    ```bash
    mysql -u user_web -p'ContraseñaSeguraWeb2026!' -D db_web -e "SHOW TABLES;"
    ```
3.  **Monitorización:** Comprobar que el servicio Netdata esté levantado y responda localmente en el puerto `19999`.
