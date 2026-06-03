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
