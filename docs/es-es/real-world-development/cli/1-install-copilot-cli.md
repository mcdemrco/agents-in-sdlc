---
title: "Lección 1 - Instalar Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y localiza la incidencia de filtrado del backlog."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un asistente de programación con agentes que se ejecuta en el terminal. Permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. Primero instalarás la herramienta con una utilidad que ya conoces.

En esta lección, harás lo siguiente:

- Instalar GitHub Copilot CLI con npm.
- Autenticarte con tu cuenta de GitHub.
- Marcar como fiable el repositorio del taller y probar una conversación.
- Localizar la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

## Escenario

Tu equipo está empezando a usar agentes de IA para abordar un backlog cada vez mayor. Copilot CLI lleva esa capacidad al terminal, donde ya trabajan muchos desarrolladores. En esta lección la instalarás, te autenticarás y la prepararás para el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js, usarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debería mostrarse el número de versión.

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI muestra este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirma que la ruta corresponde al repositorio Tailspin Toys y selecciona **Yes, and remember this folder for future sessions** para responder que sí.

> [!NOTE]
> En Codespaces, quizá ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya puedes continuar.

## Familiarízate con la herramienta

Los comandos que escribes en el prompt habitual se ejecutan directamente en Codespaces. Cuando se inicia Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, usa las teclas de dirección para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que está disponible el servidor MCP integrado de GitHub.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. Volverás a ella después de realizar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama que esté seleccionada en el terminal; no crea automáticamente un árbol de trabajo. Crearás una rama de funcionalidad antes de cada cambio.

## Usa el atajo del taller

Normalmente, Copilot CLI pide permiso antes de usar herramientas fuera de los permisos establecidos. En este taller volverás a iniciarla con `--yolo`, un atajo aprobado por ti que elimina esas solicitudes de aprobación en el Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` activa los permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual; nunca lo configures como alias predeterminado para el trabajo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

Para este taller, `--enable-all-github-mcp-tools` habilita las herramientas MCP de lectura y escritura de GitHub que se usarán más adelante para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu equipo local, pero los recursos de GitHub autenticados son reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarla desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para comprobar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, usarás `copilot --resume` para volver a la misma conversación y rama de la funcionalidad.

## Resumen y pasos siguientes

En esta lección:

- Instalaste GitHub Copilot CLI con npm.
- Te autenticaste con tu cuenta de GitHub.
- Marcaste como fiable el repositorio del taller y probaste una conversación.
- Localizaste la incidencia de filtrado con el servidor MCP integrado de GitHub.

A continuación, [empezarás un cambio específico][next-lesson] y usarás Copilot CLI para mostrar valoraciones por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
