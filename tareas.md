# 📋 Panel de Tareas (Backlog)

Este archivo sirve para registrar y realizar el seguimiento de las tareas y cambios de alcance del proyecto.

## 🚀 Issues de GitHub Registrados

### 🔴 [Issue #1] Añadir balanceador HAProxy a la infraestructura
*   **Estado:** Completado ✅
*   **Asignados:** Iker Clemente Quijada (Miembro A) y Colaborador de Sistemas (Miembro B)
*   **Prioridad:** Alta (Cambio de alcance solicitado por el cliente)
*   **Descripción:** 
    El cliente ha solicitado agregar alta disponibilidad básica colocando un balanceador de carga HAProxy delante del servidor Apache actual. Se requiere documentar el diagrama actualizado y la configuración del balanceador.
*   **Subtareas:**
    *   [x] Actualizar el diagrama de arquitectura y la tabla de componentes en `docs/02-diseno.md` (Asignado a: **Miembro A**)
    *   [x] Crear cronograma y planificación de tiempos en `docs/03-planificacion.md` (Asignado a: **Miembro A**)
    *   [x] Escribir guía de instalación y configuración de HAProxy en `docs/04-instalacion/servidor-web.md` (Asignado a: **Miembro B**)
    *   [x] Añadir comandos de comprobación y mantenimiento de HAProxy en `docs/05-operacion.md` (Asignado a: **Miembro B**)

---

## ✅ Tareas Completadas (Sesiones 1 - 3)
*   [x] Estructura inicial y README.md
*   [x] docs/01-analisis.md: Requisitos de la PYME
*   [x] docs/02-diseno.md: Esquema de red inicial y versiones de software
*   [x] docs/04-instalacion/monitorizacion.md: Configuración de Netdata
*   [x] docs/04-instalacion/backups.md: Script de backup y cron
*   [x] docs/04-instalacion/servidor-web.md: Servidor Apache + PHP
*   [x] docs/04-instalacion/base-de-datos.md: Configuración de MySQL y privilegios
*   [x] docs/04-instalacion/ssh-firewall.md: Configuración SSH y cortafuegos UFW
*   [x] docs/05-operacion.md: Tareas de mantenimiento diarias
*   [x] docs/06-recuperacion.md: Plan de recuperación ante desastres
*   [x] CHANGELOG.md: Control de versiones e historial
