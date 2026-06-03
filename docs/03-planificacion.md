# 03. Planificación del Proyecto

Este documento detalla el cronograma y la asignación de tiempos para el despliegue del entorno y su posterior ampliación con HAProxy.

## 📅 Calendario del Proyecto (Gantt)

A continuación se muestra la secuencia temporal de las actividades planificadas para la implantación de la infraestructura:

```mermaid
gantt
    title Plan de Despliegue de Infraestructura LAMP + HAProxy
    dateFormat  YYYY-MM-DD
    section Fase 1: Análisis y Diseño
    Análisis de Requisitos       :done,    des1, 2026-06-01, 1d
    Diseño de Red y Componentes   :done,    des2, after des1, 1d
    section Fase 2: Plataforma Base
    Despliegue de Apache y PHP    :active,  plat1, 2026-06-03, 1d
    Despliegue de MySQL Server    :active,  plat2, after plat1, 1d
    Bastionado de SSH y UFW       :active,  plat3, after plat2, 1d
    section Fase 3: Operaciones
    Configuración de Netdata      :         oper1, 2026-06-06, 1d
    Automatización de Backups     :         oper2, after oper1, 1d
    section Fase 4: Ampliación (HAProxy)
    Instalación de HAProxy        :         hap1, 2026-06-08, 1d
    Integración y Pruebas Web     :         hap2, after hap1, 1d
    section Fase 5: Cierre
    Revisión Cruzada y Entrega   :         cier1, 2026-06-10, 1d
```

---

## 📋 Resumen de Hitos y Tareas

| Fase | Tarea Principal | Duración | Responsable Principal |
| :--- | :--- | :--- | :--- |
| **Fase 1** | Definición de especificaciones y arquitectura lógica | 2 días | Ambos miembros |
| **Fase 2** | Instalación de Apache, PHP, MySQL, seguridad SSH y UFW | 3 días | Documentalista de Plataforma |
| **Fase 3** | Instalación de Netdata y script de backup en cron | 2 días | Documentalista de Operaciones |
| **Fase 4** | Integración del balanceador HAProxy (Cambio de Alcance) | 2 días | Ambos miembros |
| **Fase 5** | Auditoría final de seguridad, enlaces y entrega | 1 día | Ambos miembros |
