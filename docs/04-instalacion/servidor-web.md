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

---

## 🔀 Integración del Balanceador de Carga (HAProxy)

Para implementar el balanceador de carga HAProxy en el puerto `80` (y `443` en producción), reconfiguraremos Apache para escuchar en el puerto local `8080`.

### 1. Cambiar el Puerto de Apache a 8080
1.  Editar `/etc/apache2/ports.conf` y cambiar `Listen 80` por `Listen 8080`:
    ```bash
    sudo sed -i 's/Listen 80/Listen 8080/' /etc/apache2/ports.conf
    ```
2.  Editar el archivo del VirtualHost `/etc/apache2/sites-available/pyme.conf` y cambiar `<VirtualHost *:80>` a `<VirtualHost *:8080>`:
    ```bash
    sudo sed -i 's/<VirtualHost \*:80>/<VirtualHost \*:8080>/' /etc/apache2/sites-available/pyme.conf
    ```
3.  Reiniciar Apache para aplicar los cambios:
    ```bash
    sudo systemctl restart apache2
    ```

### 2. Instalar y Configurar HAProxy
1.  Instalar HAProxy:
    ```bash
    sudo apt install -y haproxy
    ```
2.  Editar la configuración principal `/etc/haproxy/haproxy.cfg`:
    ```bash
    sudo nano /etc/haproxy/haproxy.cfg
    ```
3.  Añadir al final del archivo la definición del backend y frontend:
    ```haproxy
    frontend http_front
        bind *:80
        stats uri /haproxy?stats
        default_backend apache_back

    backend apache_back
        balance roundrobin
        server apache_node1 127.0.0.1:8080 check
    ```
4.  Reiniciar HAProxy:
    ```bash
    sudo systemctl restart haproxy
    ```

