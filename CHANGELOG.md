# CHANGELOG - Registro de Cambios

Todos los cambios notables en este proyecto serán documentados en este archivo.

## [v0.3.0] - 2026-06-03 (Sesión 3)

### Añadido
- **Intercambio de roles:** Miembro A asume el rol de Operaciones y Miembro B el de Plataforma.
- **Plataforma:** Nuevo archivo [servidor-web.md](docs/04-instalacion/servidor-web.md) con la instalación de Apache y PHP.
- **Plataforma:** Nuevo archivo [base-de-datos.md](docs/04-instalacion/base-de-datos.md) con la creación de bases de datos `db_web` y `db_gestion`.
- **Plataforma/Seguridad:** Archivo [ssh-firewall.md](docs/04-instalacion/ssh-firewall.md) inicializado con configuración SSH segura.
- **Operaciones:** Nueva [Guía de Operaciones Diarias](docs/05-operacion.md) con tareas de mantenimiento apt, logs y disco.
- **Operaciones:** Nuevo [Plan de Recuperación](docs/06-recuperacion.md) para restaurar bases de datos y archivos.

---

## [v0.2.0] - 2026-06-03 (Sesión 2)

### Añadido
- **Resolución de Conflictos:** Simulación y resolución de un conflicto de merge en la tabla de versiones de [02-diseno.md](docs/02-diseno.md) integrando la versión `2.4.60` de Apache y la utilidad `Certbot`.
- **Revisión Cruzada:** Incorporación de correcciones tras feedback: sección de puertos en diseño y advertencias de claves SSH en backups.

### Modificado
- [02-diseño.md](docs/02-diseno.md): Añadido listado de puertos de comunicación requeridos por UFW.
- [backups.md](docs/04-instalacion/backups.md): Añadido bloque de advertencia sobre la configuración de llaves SSH autorizadas.

---

## [v0.1.0] - 2026-06-03 (Sesión 1)

### Añadido
- **Estructura Inicial:** Esqueleto de directorios `docs/` y ficheros markdown vacíos para la documentación de la PYME.
- **README.md:** Descripción básica del proyecto, autores y enlaces a la documentación.
- **Plataforma:** Borrador inicial de [01-analisis.md](docs/01-analisis.md) (requisitos del cliente) y [02-diseno.md](docs/02-diseno.md) (diagrama Mermaid).
- **Operaciones:** Borrador inicial de [monitorizacion.md](docs/04-instalacion/monitorizacion.md) (Netdata) y [backups.md](docs/04-instalacion/backups.md) (script `mysqldump` + `rsync`).
