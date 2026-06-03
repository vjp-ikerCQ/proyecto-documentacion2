# 01. Análisis de Requisitos de la Infraestructura

Este documento describe el análisis de necesidades técnicas y de infraestructura para la PYME de consultoría.

## 📋 Requisitos del Cliente

La empresa requiere una infraestructura local y remota básica para desplegar su presencia web y su sistema de gestión interna de forma segura y robusta.

### 1. Servicios del Servidor
*   **Servidor Web:** Apache2 con soporte para PHP (aplicación web y portal interno).
*   **Servidor de Base de Datos:** MySQL o MariaDB que albergará dos bases de datos independientes:
    1.  `db_web`: Base de datos de cara al público para el sitio web corporativo.
    2.  `db_gestion`: Base de datos para la gestión interna de proyectos y clientes.
*   **Acceso Remoto Seguro:** Servicio SSH para administración de sistemas remota restringida por IP.
*   **Seguridad y Cortafuegos:** Implementación de reglas estrictas mediante UFW (Uncomplicated Firewall) para bloquear accesos no deseados.
*   **Monitorización:** Herramienta ágil (Netdata) para analizar en tiempo real el rendimiento de la CPU, memoria, discos y tráfico de red.
*   **Copias de Seguridad (Backup):** Sistema automatizado y programado para volcar bases de datos e información estática mediante un script cron, enviándolo a un almacenamiento externo por `rsync`.

### 2. Entorno y Plataforma de Software
*   **Sistema Operativo:** Ubuntu Server 22.04 LTS. Se selecciona esta distribución por su soporte a largo plazo, estabilidad en producción y amplia compatibilidad con el software de la pila LAMP.
*   **Arquitectura:** Servidor virtualizado o físico en la red local de la empresa con acceso a Internet a través de un gateway seguro.

### 3. Restricciones Técnicas y de Red
*   La base de datos MySQL no debe ser accesible directamente desde el exterior (solo escucha en `localhost` o red privada interna).
*   El acceso SSH se limitará a la IP estática del departamento de administración/oficina.
*   El tráfico HTTP (puerto 80) y HTTPS (puerto 443) debe ser permitido para todos.
