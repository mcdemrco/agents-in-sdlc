---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor del Model Context Protocol (MCP) de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. Conectarás el **servidor MCP de Playwright** para que Copilot controle un navegador real y pruebe la funcionalidad. Después publicarás la rama y abrirás la solicitud de incorporación de cambios (PR).

En este ejercicio, harás lo siguiente:

- Aprender qué es el Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- Añadir el servidor MCP de Playwright al espacio de trabajo.
- Pedir a Copilot que lo use para probar manualmente la funcionalidad de filtrado en un navegador.
- Publicar la rama y abrir una solicitud de incorporación de cambios para los cambios de filtrado.

## ¿Qué es el Model Context Protocol (MCP)?

El modo agente gana capacidades cuando puede acceder a recursos externos. El Model Context Protocol (MCP) es un estándar que permite a Copilot comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el modelo de lenguaje de gran tamaño (LLM) y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

El [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente y los servicios externos, como las interfaces de programación de aplicaciones (API) existentes o las herramientas locales distribuidas en paquetes de npm. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios de GitHub, crear repositorios y gestionar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona automatización del navegador con Playwright, como navegar por páginas, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Por motivos de seguridad, trata los servidores MCP como cualquier otra dependencia. Antes de usar un servidor, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos o acciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu codespace.
2. Comprueba que ya hay configurado un servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla ya incluye la entrada `github`, que permitió a Copilot leer el backlog en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server]; no tienes que instalarlo localmente. VS Code se conecta mediante HTTP y, la primera vez que Copilot usa una de sus herramientas, te autenticas con GitHub.

## Añade el servidor MCP de Playwright

Añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es local: VS Code lo inicia en el equipo mediante `npx @playwright/mcp@latest`. El argumento `--headless` ejecuta el navegador sin una ventana visible, necesario en un codespace que no tiene escritorio gráfico.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para las pruebas de extremo a extremo, por lo que normalmente el navegador ya está instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confirma que es de confianza

VS Code inicia los servidores MCP cuando se necesitan. La primera vez que Copilot usa sus herramientas, VS Code inicia el servidor y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, selecciona **Start**, que aparece sobre la entrada `playwright`, para iniciar el servidor y comprobar la configuración.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para iniciarlo.

El inicio manual es opcional. Si lo omites, VS Code iniciará el servidor y te pedirá confirmación la primera vez que Copilot necesite sus herramientas.

## Prueba la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real. El agente puede abrir una página, navegar por ella, aplicar filtros, leer los resultados y resumirlos. Así puedes comprobar el comportamiento sin salir de la conversación.

El servidor MCP de Playwright usa el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. Por eso el agente trabaja con elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, igual que las tecnologías de asistencia. Una comprobación funcional sencilla también ofrece una comprobación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado en el navegador.

1. Envía este prompt:

   ```text
   Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

   1. Go to the games page and note how many games are listed.
   2. Apply a category filter and confirm the list updates to only show games in that category.
   3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
   4. Combine a category and a publisher filter and confirm the results respect both.

   Report what you observe at each step, and call out anything that does not behave as expected.
   ```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará lo que observa. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona como esperabas, pide más información o que corrija el código antes de abrir una PR.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321` para realizar la prueba. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor MCP de Playwright quizá tenga que descargar un navegador; si informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publica la rama y crea una solicitud de incorporación de cambios

Después de confirmar que la funcionalidad funciona, publica la rama `filtering-vscode` para que el equipo pueda revisar los cambios.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Puedes crear una solicitud de incorporación de cambios desde github.com, la interfaz de línea de comandos de GitHub (CLI) o Copilot. Pide a Copilot que busque la incidencia relacionada y cree una PR vinculada a ella.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el selector de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub, quizá debas autenticarte; sigue las instrucciones.
5. Comprueba que Copilot busca y encuentra la incidencia adecuada y crea la PR.
6. Selecciona el enlace de Copilot para revisar la PR, pero **no la combines todavía**.

## Resumen y pasos siguientes

En este ejercicio:

- Aprendiste qué es el Model Context Protocol (MCP) y cómo amplían los servidores MCP las herramientas de Copilot.
- Añadiste el servidor MCP de Playwright al espacio de trabajo.
- Lo usaste para probar manualmente el filtrado en un navegador antes de publicar los cambios.
- Publicaste la rama y abriste una solicitud de incorporación de cambios para el trabajo de filtrado.

Has usado un servidor MCP para probar una funcionalidad, pero MCP es solo una forma de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué se habla tanto de él][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro de MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
