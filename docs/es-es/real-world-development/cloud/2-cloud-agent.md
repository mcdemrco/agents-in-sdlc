---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen deuda técnica: incidencias de seguridad pendientes, código heredado que necesita actualizarse o funcionalidades del backlog que no se han implementado por falta de tiempo. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Al terminar, crea una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así se pueden delegar tareas tediosas y dedicar más tiempo a cuestiones de mayor alcance.

En esta lección explorarás cómo usar el agente en la nube para:

- Personalizar el entorno de generación de código.
- Garantizar que las operaciones se realicen de forma segura.
- Crear incidencias con un alcance claro.
- Asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica. La documentación de la primera versión del sitio, creada por contratistas, no está en buen estado: comprobarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar el descubrimiento de juegos. Quiere mostrar en la página de detalles de cada juego otros títulos de la misma categoría para que los usuarios puedan seguir explorando. No necesita una implementación refinada; basta con una versión que permita hacer pruebas de aceptación de la experiencia. Aunque esta tarea está pendiente, hay otras incidencias de mayor prioridad.

Estas tareas pueden quedar relegadas con facilidad y son adecuadas para asignarlas al agente en la nube. El agente trabaja de forma asíncrona mientras tú te ocupas de otras tareas; después puedes volver para revisar su trabajo.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Al asignarla, Copilot crea una PR en borrador para seguir el progreso, configura un entorno e inicia el trabajo. Puedes consultar la sesión mientras se ejecuta o después. Cuando la solución esté lista para revisarla, Copilot te mencionará en la PR.

## La importancia de definir bien el alcance

GitHub Copilot no elimina la necesidad de pensar. Incluso las tareas aparentemente sencillas pueden tener detalles complejos. Ten en cuenta las [prácticas recomendadas para definir el alcance de las tareas del agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas de este repositorio

Los ejercicios anteriores presentaron las instrucciones personalizadas y cómo orientan a Copilot. Si ya completaste el ejercicio práctico, has visto `.github/copilot-instructions.md` en funcionamiento; si no, examina el archivo y las instrucciones incluidas en el repositorio antes de asignar trabajo al agente.

1. En la interfaz web de GitHub del repositorio o en el codespace, abre `.github/copilot-instructions.md`.
2. Revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
3. Abre `.github/instructions/unit-tests.instructions.md` y observa el frontmatter `applyTo`, que aplica las instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation`, fíjate en los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentarios que aparezcan en la PR. Proceden de estos archivos de instrucciones; volverás a revisarlos en otro ejercicio.

## Configura el entorno de desarrollo del agente en la nube

La creación de código suele requerir un entorno específico y la ejecución de scripts de configuración. Lo mismo se aplica al agente de Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente usa [GitHub Actions][github-actions] para preparar su entorno. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`. El flujo de trabajo de este taller ya está preparado para instalar Node.js y las dependencias del proyecto, obtener los binarios del navegador para las pruebas de extremo a extremo y configurar la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Usa los permisos mínimos necesarios para los pasos. Copilot recibe su propio token para sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitan contents: read. Si no se clona, Copilot lo hará automáticamente al terminar estos pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la aplicación: Node.js (la aplicación es Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y las pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la PR.
- El activador `workflow_dispatch` permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions**. Así puedes probarlo sin esperar a que Copilot lo ejecute.

## Añade documentación

Muchos proyectos tienen documentación desactualizada o carecen de ella. Esa deuda técnica puede reducir la productividad y dificultar el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot puede ayudar a crear documentación; por eso, esta es una buena tarea para el agente en la nube. Más adelante revisarás el resultado.

1. En una pestaña nueva del navegador, abre el repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. Escribe esta descripción en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

    ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la PR que se acaba de generar. Su título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer mientras Copilot prepara el entorno con el flujo de trabajo, analiza el proyecto y determina cómo abordar la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la PR y comprueba que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la página para ver el indicador actualizado.

16. Desplázate por la sesión en directo para observar cómo Copilot explora el código y comprende su estado, elabora un plan y crea código.

El proceso puede tardar varios minutos. El agente en la nube puede trabajar de forma asíncrona, así que aprovecharás esa capacidad para asignarle otra tarea mientras tú te ocupas de añadir una funcionalidad.

## Añade una sección de juegos relacionados a la página de detalles

Una ventaja del agente en la nube es que permite dividir el trabajo: tú puedes ocuparte de unas tareas mientras el agente trabaja en otras. Asigna al agente la sección de juegos relacionados que solicita el equipo de diseño.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. Escribe esta descripción en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot**.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás un emoji 👀 en el primer comentario de la incidencia para indicar que Copilot ha empezado a trabajar.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

El agente trabaja en tu segunda solicitud. Como trabaja de forma parecida a un ingeniero de software, no tienes que supervisarlo constantemente; podrás revisar el resultado cuando termine. Ahora crearás y usarás agentes personalizados.

## Resumen y pasos siguientes

Has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona, por ejemplo para abordar deuda técnica, crear funcionalidades o migrar código entre frameworks.

En esta lección:

- Personalizaste el entorno para generar código.
- Exploraste cómo realizar operaciones de forma segura.
- Aprendiste la importancia de delimitar claramente las incidencias.
- Asignaste incidencias a Copilot.

Mientras el agente en la nube trabaja, puedes crear y usar agentes personalizados. El [agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y las instrucciones personalizadas que exploraste en ejercicios anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
