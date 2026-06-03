# Instalación y Configuración del Servidor Web Apache + PHP

Este documento detalla la instalación, configuración e integración de la capa web de la infraestructura.

## 📥 Instalación de Paquetes

Instalamos el servidor Apache2 y PHP 8.1 junto con las extensiones requeridas para el correcto funcionamiento de las aplicaciones:

```bash
# Actualizar lista de paquetes e instalar Apache y PHP
sudo apt update
sudo apt install -y apache2 php8.1 php8.1-mysql php8.1-xml php8.1-curl php8.1-mbstring libapache2-mod-php8.1
```

### Verificar el Estado de Apache
```bash
sudo systemctl status apache2
```

---

## ⚙️ Configuración del Servidor Virtual (VirtualHost)

Configuraremos un VirtualHost para el sitio web corporativo de la PYME en `/etc/apache2/sites-available/pyme.conf`.

1.  Crear el archivo de configuración:
    ```bash
    sudo nano /etc/apache2/sites-available/pyme.conf
    ```
2.  Añadir el siguiente contenido:
    ```apache
    <VirtualHost *:80>
        ServerName pyme.local
        ServerAlias www.pyme.local
        ServerAdmin admin@pyme.local
        DocumentRoot /var/www/html/pyme

        <Directory /var/www/html/pyme>
            Options -Indexes +FollowSymLinks
            AllowOverride All
            Require all granted
        </Directory>

        ErrorLog ${APACHE_LOG_DIR}/pyme_error.log
        CustomLog ${APACHE_LOG_DIR}/pyme_access.log combined
    </VirtualHost>
    ```

3.  Crear el directorio raíz del sitio y asignar permisos:
    ```bash
    sudo mkdir -p /var/www/html/pyme
    sudo chown -R www-data:www-data /var/www/html/pyme
    ```

4.  Habilitar el nuevo sitio y el módulo rewrite, y reiniciar Apache:
    ```bash
    sudo a2ensite pyme.conf
    sudo a2dissite 000-default.conf
    sudo a2enmod rewrite
    sudo systemctl restart apache2
    ```

---

## 🧪 Pruebas de Integración PHP

Para validar el funcionamiento del intérprete PHP con Apache:

1.  Crear un archivo de prueba:
    ```bash
    echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/pyme/info.php
    ```
2.  Probar el acceso en `http://pyme.local/info.php`.
3.  **Importante:** Eliminar el archivo después de validar para evitar fugas de información sobre la configuración del servidor:
    ```bash
    sudo rm /var/www/html/pyme/info.php
    ```
