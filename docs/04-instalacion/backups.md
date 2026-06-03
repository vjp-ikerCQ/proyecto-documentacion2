# Política y Scripts de Copias de Seguridad (Backup)

Este documento detalla la estrategia de backups automáticos implementada para proteger el sitio web y la base de datos de gestión interna.

## 📋 Estrategia de Copias de Seguridad

1.  **Datos a respaldar:**
    *   Bases de datos de MySQL (`db_web` y `db_gestion`).
    *   Código fuente y archivos estáticos del sitio web (`/var/www/html`).
    *   Configuraciones del sistema (`/etc/apache2/`, `/etc/mysql/`).
2.  **Frecuencia:** Diaria a las 02:00 AM (hora de menor actividad).
3.  **Destino:** Copia local en `/var/backups/pyme` y replicación remota vía `rsync` a un servidor de almacenamiento externo (NAS).
4.  **Rotación:** Conservar los últimos 7 días.

---

## 🛠️ Script de Backup (`/usr/local/bin/backup-pyme.sh`)

Este script realiza el volcado de la base de datos y la sincronización de archivos:

```bash
#!/bin/bash
# Script de copias de seguridad de la infraestructura

BACKUP_DIR="/var/backups/pyme"
DATE=$(date +%Y-%m-%d)
RETENTION_DAYS=7

# Crear directorio de copias si no existe
mkdir -p "$BACKUP_DIR"

# 1. Copia de base de datos MySQL (con mysqldump)
echo "Realizando volcado de base de datos..."
mysqldump --defaults-extra-file=/etc/mysql/debian.cnf --all-databases | gzip > "$BACKUP_DIR/db-all-$DATE.sql.gz"

# 2. Copia de archivos web y configuración
echo "Comprimiendo archivos web..."
tar -czf "$BACKUP_DIR/web-$DATE.tar.gz" /var/www/html /etc/apache2 /etc/mysql 2>/dev/null

# 3. Envío al almacenamiento de respaldo remoto NAS
echo "Sincronizando backups con el servidor remoto..."
rsync -avz --delete "$BACKUP_DIR/" backup-user@192.168.1.100:/mnt/nas/backups/

# 4. Rotación de backups locales (borrar archivos de más de 7 días)
echo "Aplicando política de rotación..."
find "$BACKUP_DIR" -type f -mtime +$RETENTION_DAYS -name "*.gz" -delete

echo "Copia de seguridad completada con éxito el $DATE"
```

---

## ⏰ Automatización con Cron

Para ejecutar el script diariamente de forma automática, añadimos una tarea programada a `cron`:

1.  Editar el archivo crontab del usuario root:
    ```bash
    sudo crontab -e
    ```
2.  Añadir la siguiente línea al final del archivo:
    ```cron
    0 2 * * * /bin/bash /usr/local/bin/backup-pyme.sh >> /var/log/backup-pyme.log 2>&1
    ```
3.  Guardar y salir. Esto garantiza la ejecución del script todas las noches a las 02:00 AM.
