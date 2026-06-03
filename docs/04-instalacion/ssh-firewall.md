# Acceso Remoto Seguro (SSH) y Firewall (UFW)

Este documento detalla el bastionado de seguridad para el acceso remoto por SSH al servidor de la PYME y la configuración del cortafuegos.

## 🔑 Configuración del Servidor SSH

El servicio OpenSSH permite la administración del sistema. Para asegurar el acceso remoto, modificaremos su configuración en `/etc/ssh/sshd_config`.

### Parámetros de Bastionado Recomendados
1.  Abrir el archivo de configuración del daemon SSH:
    ```bash
    sudo nano /etc/ssh/sshd_config
    ```
2.  Aplicar los siguientes cambios en las directivas:
    ```ini
    # Cambiar el puerto por defecto (opcional, seguridad por oscuridad)
    Port 2222

    # Desactivar acceso root directo
    PermitRootLogin no

    # Desactivar autenticación por contraseña tradicional
    PasswordAuthentication no

    # Permitir únicamente autenticación mediante claves públicas SSH
    PubkeyAuthentication yes

    # Tiempo de gracia de login y máximo de intentos
    LoginGraceTime 1m
    MaxAuthTries 3
    ```

3.  Verificar la sintaxis de la configuración antes de reiniciar:
    ```bash
    sudo sshd -t
    ```
4.  Reiniciar el servicio SSH:
    ```bash
    sudo systemctl restart ssh
    ```

---

## Configuración del Firewall con UFW

Para asegurar el servidor, estableceremos una política por defecto restrictiva y permitiremos únicamente los servicios esenciales.

1.  **Establecer políticas por defecto:**
    ```bash
    sudo ufw default deny incoming
    sudo ufw default allow outgoing
    ```
2.  **Permitir tráfico web (puertos 80 y 443):**
    ```bash
    sudo ufw allow 80/tcp
    sudo ufw allow 443/tcp
    ```
3.  **Permitir SSH seguro (restringido a la IP de la oficina):**
    ```bash
    # Permitir SSH (puerto alternativo 2222) solo desde la subred de la oficina
    sudo ufw allow from 192.168.1.0/24 to any port 2222 proto tcp
    ```
4.  **Habilitar el cortafuegos:**
    ```bash
    sudo ufw enable
    ```
5.  **Verificar el estado de las reglas:**
    ```bash
    sudo ufw status verbose
    ```
