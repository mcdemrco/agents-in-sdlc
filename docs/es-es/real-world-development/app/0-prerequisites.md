---
title: "Lección 0 - Requisitos previos"
description: "Prepara el entorno para las lecciones de la aplicación GitHub Copilot: instala Node.js para el proyecto Tailspin Toys y crea tu propia copia del repositorio a partir de la plantilla."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La aplicación GitHub Copilot es una aplicación de escritorio que sirve de centro para Copilot y GitHub. Proporciona acceso rápido a las incidencias y las solicitudes de incorporación de cambios y permite desarrollar con GitHub Copilot. Durante este taller trabajarás en local con la aplicación Tailspin Toys, creada con Astro, y con la aplicación GitHub Copilot. Antes de empezar, comprueba que Node.js esté instalado localmente y, después, instala la aplicación Copilot.

En esta lección, harás lo siguiente:

- Instalar Node.js para ejecutar las pruebas del proyecto en tu equipo.
- Crear una copia propia del proyecto Tailspin Toys a partir de la plantilla.

## Instala Node.js

En varias lecciones pedirás a un agente que cree funcionalidades y ejecute en local las pruebas de Tailspin Toys. Para ello se necesita **[Node.js][nodejs]**, el único entorno de ejecución que requiere el proyecto. Instala la versión **LTS** actual.

La opción más sencilla en todas las plataformas es usar el instalador oficial.

1. Abre un terminal en tu sistema operativo; puedes usar Windows Terminal, Terminal de macOS o la aplicación que utilices habitualmente.
2. Ejecuta estos comandos para comprobar las versiones instaladas de Node.js y npm:

    ```shell
    node --version
    npm --version
    ```

3. Si ambos comandos se ejecutan correctamente y Node.js cumple los requisitos indicados en el README y en `package.json` del proyecto, pasa a la sección siguiente.

> [!TIP]
> Completa estos pasos solo si no tienes Node instalado o si necesitas actualizarlo.

4. Abre la [página de descarga de Node.js][node-download].
5. Descarga la versión **LTS** para tu sistema operativo.
6. Ejecuta el instalador y acepta las opciones predeterminadas. En Windows, mantén seleccionada la opción **Add to PATH**.
7. Cuando termine la instalación, abre un terminal nuevo.
8. Comprueba la instalación desde ese terminal:

    ```bash
    node --version
    npm --version
    ```

9. Deberían mostrarse las versiones instaladas de Node.js y npm.

> [!IMPORTANT]
> El agente de la aplicación Copilot también debe poder ejecutar `node --version` y `npm --version`. Que funcionen en tu terminal habitual no garantiza que estén disponibles para la aplicación. Cierra completamente la aplicación y vuelve a abrirla después de instalar Node.js o cambiar `PATH`. Si alguno de los comandos sigue fallando en una sesión, sigue los [pasos para solucionar problemas del entorno de la sesión][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada árbol de trabajo también necesita las dependencias del proyecto y Chromium de Playwright para las pruebas de extremo a extremo (E2E). Sigue el README del repositorio de Tailspin Toys al preparar un árbol de trabajo y revisa las solicitudes de instalación antes de aprobarlas.

## Configura el repositorio del taller

Trabajarás con tu propia copia del proyecto Tailspin Toys. Créala ahora a partir del [repositorio de plantilla][template-repository]. El nuevo repositorio contiene todos los archivos necesarios para el taller y lo conectarás al instalar la aplicación.

1. En una ventana nueva del navegador, abre el repositorio de GitHub del taller: `https://github.com/github-samples/tailspin-toys`.
2. Crea una copia del repositorio seleccionando **Use this template** en la página del repositorio del taller y, después, **Create a new repository**.

    ![Botón Use this template con Create a new repository seleccionado en el menú desplegable](../../../_images/app-0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. En caso contrario, crea el repositorio en una organización en la que tengas acceso a GitHub Copilot.

    ![Formulario Create a new repository con github-samples/tailspin-toys como plantilla y el nombre del repositorio completado](../../../_images/app-0-create-repository.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**); la necesitarás más adelante.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se crea automáticamente un backlog de incidencias de GitHub. Trabajarás con esas incidencias durante el taller; no tienes que crear ninguna.

Usa una copia nueva de la plantilla del taller. Incluye instrucciones del repositorio, código de la aplicación, pruebas, una habilidad `quality-checks` y una extensión de canvas existente. Durante el taller personalizarás la habilidad y crearás un agente de QA. Si usas una copia anterior, comprueba con la persona que facilita el taller que incluye los archivos necesarios.

## Resumen y pasos siguientes

Ya tienes el entorno preparado. En esta lección:

- Instalaste Node.js para compilar y probar el proyecto en tu equipo.
- Creaste una copia propia del repositorio Tailspin Toys a partir de la plantilla.

A continuación, [instalarás la aplicación GitHub Copilot][next-lesson], conectarás el repositorio que acabas de crear y te familiarizarás con el espacio de trabajo.

## Recursos

- [Descargar Node.js][node-download]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#inicia-una-sesion-desde-la-incidencia
