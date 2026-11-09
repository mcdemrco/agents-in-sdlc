---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot son asistentes de IA especializados en tareas o ámbitos concretos del desarrollo. Sus perfiles son archivos Markdown de la carpeta `.github/agents` del repositorio. Pueden proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimiento del dominio. Así, los equipos pueden convertir sus conocimientos en agentes reutilizables, como un agente de accesibilidad que aplique las [pautas de accesibilidad para el contenido web (WCAG)][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] comparten algunos aspectos: ambos se definen principalmente con archivos Markdown e indican a una IA cómo realizar tareas. La distinción más clara es que el **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; para hacerlo, podría usar habilidades de flujo de solicitudes de incorporación de cambios o de ejecución y gestión de pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar lo que funciona en cada entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio, aprenderás a:

- Revisar la definición de un agente personalizado.
- Asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todos, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que el tema oscuro puede resultar difícil de leer porque el contraste entre el texto y el fondo es insuficiente. El equipo de diseño ha solicitado un modo de alto contraste que los usuarios puedan activar y desactivar.

Como la accesibilidad es importante, usarás un agente personalizado para generar esta funcionalidad cuanto antes.

## Revisa el agente personalizado de accesibilidad

Ya hay un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

1. Vuelve al codespace, abre un terminal y crea una rama nueva desde `main` para el trabajo de accesibilidad. Así mantendrás separada la PR de filtrado del Ejercicio 3:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md`.
3. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

4. Revisa las secciones siguientes, que describen:
   - Las responsabilidades principales al generar código para un sitio accesible.
   - Las prácticas recomendadas de accesibilidad.
   - Ejemplos de código HTML, CSS y JavaScript.
   - Errores habituales que se deben evitar.

## Usa el agente personalizado en Copilot Chat

VS Code muestra en el selector de agentes de Copilot Chat todos los agentes personalizados definidos en `.github/agents`. Al seleccionar uno, la conversación usa sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; se indican en cada paso.

1. Si no está seleccionado, elige **Agent** en el selector de agentes de la vista Chat.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el selector de agentes de la parte inferior de la vista Chat; muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes.
4. Envía este prompt al agente de accesibilidad:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarla.
6. A medida que se apliquen cambios, el indicador **Files changed** se actualizará para mostrar los archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar varios minutos. Copilot modificará archivos reales del repositorio, como componentes Astro, estilos y pruebas relacionadas.

En el ejercicio siguiente revisarás y orientarás el trabajo mientras se realiza.

## Resumen y pasos siguientes

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten codificar los conocimientos y estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

En este ejercicio:

- Revisaste cómo se definen los agentes personalizados.
- Usaste un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
