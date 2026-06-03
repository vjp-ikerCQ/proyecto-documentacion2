# REVISION - Reflexiones del Proyecto de Documentación Colaborativa

Este documento contiene la revisión final del proyecto, analizando el uso de Git y GitHub, la resolución de conflictos y las lecciones aprendidas durante las 4 sesiones de trabajo práctico.

---

## 💥 Conflictos vividos y su Resolución

Durante el desarrollo de la documentación, nos enfrentamos a dos tipos de conflictos provocados para simular escenarios reales de desarrollo colaborativo:

### 1. Primer Conflicto: Fusión por Merge (`docs/02-diseno.md`)
*   **Origen:** El **Miembro A** y el **Miembro B** modificaron simultáneamente la tabla de especificaciones de software desde ramas diferentes (`feature/actualizar-versiones` y `feature/nuevas-tecnologias`), proponiendo versiones incompatibles para el servidor Apache (`2.4.60` y `2.4.59` respectivamente).
*   **Detección:** GitHub bloqueó la fusión automática del segundo Pull Request indicando conflictos.
*   **Resolución:** Realizamos un merge de `main` en local hacia la rama `feature/nuevas-tecnologias`. Esto insertó marcadores de conflicto (`<<<<<<<`, `=======`, `>>>>>>>`). Resolvimos de forma manual seleccionando la versión más reciente de Apache (`2.4.60` aportada por A) y conservando la adición de la herramienta `Certbot` (aportada por B). Confirmamos el archivo resuelto con `git commit` y subimos para completar la fusión en `main`.

### 2. Segundo Conflicto: Fusión por Rebase (`docs/04-instalacion/ssh-firewall.md`)
*   **Origen:** Ambos miembros redactaron la sección de cortafuegos de UFW de manera incompatible. El **Miembro A** propuso un firewall con reglas de SSH genéricas en `feature/guia-operacion` y el **Miembro B** propuso reglas limitadas al puerto y la IP de la oficina en `feature/servidor-web-y-bd`.
*   **Resolución:** Fusionamos primero el PR de Plataforma (B) a `main`. Para integrar la rama de Operaciones (A), en lugar de merge, aplicamos un rebase:
    ```bash
    git checkout feature/guia-operacion
    git pull --rebase origin main
    ```
    Git detuvo el rebase al encontrar conflicto en `ssh-firewall.md`. Resolvimos manualmente combinando lo mejor de ambas: la política restrictiva por defecto (`deny incoming`) y la limitación estricta de IP para el puerto SSH reconfigurado (`2222`). Continuamos el rebase con `git rebase --continue` y subimos la rama utilizando `git push --force-with-lease`.

---

## 🛠️ Comandos Git más utilizados

El flujo de trabajo nos obligó a utilizar una gran variedad de comandos locales y remotos:

*   **Gestión de Ramas:** `git checkout -b <rama>` (crear y cambiar), `git branch` (listar).
*   **Confirmaciones:** `git add .` (preparar cambios), `git commit -m "..."` (confirmar con mensajes semánticos).
*   **Sincronización:** `git pull origin main` (traer cambios), `git push origin <rama>` (subir ramas).
*   **Fusión e Historial:** `git merge --no-ff` (crear merge commit para conservar topología de ramas), `git log --graph --oneline` (visualizar historial).
*   **Reorganización:** `git pull --rebase origin main` (rebasear rama local) y `git push --force-with-lease` (subir cambios rebaseados de forma segura).

---

## 👥 Intercambio de Roles

El intercambio de roles al inicio de la **Sesión 3** fue el punto clave del ejercicio:
*   **Rotación efectiva:** El **Miembro A** pasó de diseñar la plataforma inicial a encargarse de la operación, mantenimiento y recuperación. El **Miembro B** pasó de los scripts de backup y monitorización a estructurar los servidores Apache y MySQL.
*   **Beneficios:** Obligó a ambos integrantes a leer y comprender detalladamente la documentación redactada por el compañero antes de poder continuar o corregir, simulando perfectamente un ambiente ágil de desarrollo real.

---

## 💡 Lecciones Aprendidas y Qué haríamos diferente

*   **Hacer `git pull` con frecuencia:** Trabajar en paralelo sin sincronizar frecuentemente con `main` multiplica la probabilidad de toparse con conflictos complejos.
*   **Rebase vs Merge:** Aprendimos que el `rebase` mantiene un historial limpio y lineal (fácil de leer en proyectos grandes), mientras que el `merge` preserva la historia cronológica real del desarrollo. En el futuro, preferiremos rebase para ramas locales cortas y merge para integrar ramas de características principales a `main`.
*   **Documentación semántica:** Mantener los nombres de los archivos ordenados numéricamente en la carpeta `docs/` simplifica enormemente la navegación de cara al cliente final.
