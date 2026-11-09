---
title: "Ejercicio 2 - Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios sencillos de una aplicación suelen requerir modificar varios archivos y realizar tareas como ejecutar pruebas. Un desarrollador localiza los archivos necesarios, hace cambios, ejecuta las pruebas, depura y averigua qué archivo falta; el proceso continúa hasta que todo está listo.

Para eso sirve el modo agente de Copilot.

El modo agente puede trabajar de forma más autónoma en el entorno de desarrollo integrado (IDE). Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. En este ejercicio lo usarás para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE y permite iterar rápidamente. El agente en la nube trabaja de forma asíncrona como un compañero del equipo e interactúa contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio, aprenderás a:

- Usar el modo agente de Copilot para explorar el proyecto, identificar archivos y coordinar cambios.
- Implementar funcionalidades en la interfaz y la capa de datos.
- Revisar los cambios y las pruebas generados por Copilot antes de combinarlos.

## Escenario

El catálogo de juegos crece y quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior añadiste un helper de editoriales. Ahora completarás con el modo agente el trabajo restante en la capa de datos, la interfaz y las pruebas.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio para conocer sus funciones actuales.

Tailspin Toys es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. El sitio es una única aplicación Astro que genera páginas estáticas de lenguaje de marcado de hipertexto (HTML) durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle, un mapeador objeto-relacional (ORM), en su frontmatter; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz del lado del cliente. Los helpers reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API estándar del modelo de objetos del documento (DOM).

### Inicia el sitio

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro. Ejecútala desde el codespace:

1. Vuelve al codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de Astro esté listo, debería aparecer un aviso parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en un navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro: `http://localhost:4321`.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost del terminal, se redirige a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al servidor que se ejecuta en tu codespace.

### Explora el sitio

Las funciones principales del sitio son:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, muestra más información, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; se indican en cada paso.

El sitio funciona, pero queremos añadir capacidades. La plantilla creó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. En el selector de agentes de la vista Chat, selecciona **Agent**. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos de terminal y usar herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Usamos Claude Sonnet 4.5 al crear el taller para intentar obtener sugerencias relativamente uniformes. Como los modelos de lenguaje grandes (LLM) son probabilísticos, es normal que las sugerencias varíen.

> [!NOTE]
> Copilot puede usar un comando diferente del Model Context Protocol (MCP), pero debería poder completar la tarea.

3. Envía este prompt a Copilot para consultar el backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado los elementos según el valor que considera que aportan a los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, examina el archivo de instrucciones que proporciona contexto. Usarás el archivo de [instrucciones de interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices, que incluyen:
   - Una descripción de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a instrucciones específicas para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones para facilitar la reutilización y permitir dividir instrucciones complejas en partes pequeñas con ejemplos.

## Implementa el filtrado

Para completar el filtrado, la aplicación necesita al menos tres cambios:

- Añadir o mejorar la lógica de filtrado en la capa de datos (`src/lib/`).
- Añadir o actualizar pruebas para el comportamiento del filtrado.
- Actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Antes de combinar los cambios, las pruebas deben ejecutarse y pasar. El modo agente puede realizar estas tareas.

1. Continúa en la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto, localiza los archivos relacionados con la funcionalidad y modifica la capa de datos, la interfaz y las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos. Mientras trabaja, pueden aparecer los controles **Keep** y **Undo** en el editor. No tienes que seleccionarlos hasta que Copilot termine y puedas revisar todos los cambios.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Sesión completa de Copilot Agent Mode en Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se ejecuta el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y pasos siguientes

Has usado el modo agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. En concreto, has aprendido a:

- Usar el modo agente de Copilot para implementar funcionalidades en la interfaz y la capa de datos.
- Hacer que Copilot explore el proyecto, identifique archivos y coordine los cambios.
- Revisar los cambios y las pruebas generados por Copilot antes de combinarlos.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar paginación

A medida que crezca la lista de juegos, será necesario añadir paginación.

1. Usa lo aprendido en este ejercicio para pedir a Copilot que implemente la paginación.
2. Decide cómo quieres que funcione, por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
3. Proporciona a Copilot las instrucciones necesarias e itera con prompts y contexto adicionales cuando haga falta.

## Recursos

- [Copilot: modos ask, edit y agent; qué hacen y cuándo usarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
