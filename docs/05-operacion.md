# 05. Guía de Mantenimiento y Operaciones Diarias

Este documento detalla las tareas recurrentes de administración de sistemas para garantizar el correcto rendimiento, estabilidad y seguridad de la infraestructura.

## 🔄 Actualización y Mantenimiento del Sistema

Para mantener el servidor Ubuntu 22.04 LTS seguro frente a vulnerabilidades, se deben programar actualizaciones periódicas de software.

```bash
# 1. Actualizar la base de datos de paquetes disponibles
sudo apt update

# 2. Instalar actualizaciones de seguridad y paquetes sin romper dependencias
sudo apt upgrade -y

# 3. Eliminar paquetes residuales y dependencias que ya no son necesarias
sudo apt autoremove -y
```

---

## 📈 Comprobación de Logs del Sistema

Es crucial revisar regularmente los registros para detectar fallos o intentos de acceso no autorizados:

*   **Log de accesos y errores de Apache:**
    ```bash
    tail -n 50 /var/log/apache2/pyme_access.log
    tail -n 50 /var/log/apache2/pyme_error.log
    ```
*   **Log de fallos y auditoría de MySQL:**
    ```bash
    sudo tail -n 50 /var/log/mysql/error.log
    ```
*   **Log del sistema y seguridad (accesos SSH y UFW):**
    ```bash
    sudo tail -n 50 /var/log/auth.log
    sudo tail -n 50 /var/log/syslog
    ```

---

## 💾 Monitoreo de Disco y Rendimiento de Base de Datos

*   **Uso de espacio de almacenamiento en disco:**
    ```bash
    df -h
    ```
*   **Uso de memoria RAM libre y memoria de intercambio (swap):**
    ```bash
    free -m
    ```
*   **Optimización y reparación rápida de tablas MySQL:**
    ```bash
    sudo mysqlcheck -o --defaults-extra-file=/etc/mysql/debian.cnf --all-databases
    ```
*   **Verificación manual del estado del servicio de backups:**
    ```bash
    tail -n 20 /var/log/backup-pyme.log
    ```
