# Monitorización del Sistema (Netdata)

Este documento detalla la instalación, configuración y acceso a la herramienta de monitorización Netdata en nuestro servidor Ubuntu 22.04 LTS.

## 📥 Instalación de Netdata

Netdata recopila métricas del sistema en tiempo real con un impacto mínimo en el rendimiento. Para su instalación rápida y segura utilizaremos el script oficial:

```bash
# Ejecutar el script de instalación oficial de Netdata
wget -O /tmp/netdata-kickstart.sh https://get.netdata.cloud/kickstart.sh && sh /tmp/netdata-kickstart.sh --non-interactive
```

Tras completar el script, el servicio de Netdata se iniciará y se habilitará automáticamente para arrancar con el sistema.

### Verificar Estado del Servicio
```bash
sudo systemctl status netdata
```

---

## ⚙️ Configuración y Acceso

Por defecto, Netdata escucha en el puerto `19999` en todas las interfaces de red (`0.0.0.0`). Por motivos de seguridad, configuraremos Netdata para que solo sea accesible localmente o mediante la IP de administración.

El archivo de configuración principal se ubica en `/etc/netdata/netdata.conf`.

### Modificación del Puerto y Enlace (Bind)
1. Abrir el archivo de configuración:
   ```bash
   sudo nano /etc/netdata/netdata.conf
   ```
2. En la sección `[web]`, verificar o establecer:
   ```ini
   [web]
       bind to = 127.0.0.1
       default port = 19999
   ```

*Nota: Al enlazarlo a `127.0.0.1`, requerirá un túnel SSH o un proxy inverso (como Apache/HAProxy) para acceder al panel desde el exterior de forma segura.*

---

## 📊 Métricas Monitorizadas

Una vez accedido al panel en `http://localhost:19999` (a través de túnel SSH), se pueden observar las siguientes métricas:
*   **CPU:** Uso global, por núcleo y tipo de proceso.
*   **Memoria RAM:** Uso, buffers, cache y swap.
*   **Discos:** Lectura/escritura (I/O) y espacio disponible.
*   **Red:** Ancho de banda y paquetes transmitidos/recibidos.
*   **Procesos:** Servicios activos (Apache, MySQL) y consumo por aplicación.
