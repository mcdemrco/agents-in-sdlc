---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente al agente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye numerosos comandos de barra. Estos permiten acceder a funciones avanzadas, información interna y opciones de configuración. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros útiles.

En esta lección, harás lo siguiente:

- Usar `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- Usar `/model` para explorar los modelos disponibles.
- Aprender cómo `/share` puede exportar o compartir una sesión.
- Explorar comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar al agente en la nube.

## Escenario

Has terminado el flujo de trabajo principal de CLI. Ahora explorarás otras posibilidades: gestionar el contexto, cambiar de modelo, compartir sesiones y, de forma opcional, delegar tareas al [agente de GitHub Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas amplias o complejas puedes alcanzar el límite de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario, y los comandos de barra te permiten inspeccionar o compactar el contexto manualmente.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio si todavía no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es pequeña, quizá apenas haya cambiado.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` si vas a cambiar a una tarea no relacionada y quieres iniciar una conversación nueva.

## Elige un modelo

Los modelos tienen distintas capacidades y cada desarrollador puede tener sus preferencias. Copilot CLI permite mostrar los modelos disponibles y elegir uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir el trabajo y lo aprendido ayuda a todo el equipo a mejorar el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` de la versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado; por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no tenga previsto compartir.

## Opcional: ampliar o delegar el trabajo

Has completado el taller principal. Copilot CLI también tiene comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de GitHub Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Antes de probarlos, inicia una tarea nueva y bien delimitada, y revisa los resultados con el flujo de trabajo habitual. Para profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y pasos siguientes

Los comandos de barra de Copilot CLI permiten configurar la herramienta, compartir sesiones y consultar lo que ocurre internamente. En esta lección:

- Usaste `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- Usaste `/model` para explorar los modelos disponibles.
- Aprendiste cómo `/share` puede exportar o compartir una sesión.
- Exploraste comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar al agente en la nube.

Copilot CLI ofrece más comandos de barra y funciones por explorar. Para terminar el recorrido, [repasarás lo aprendido][next-lesson] y verás los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de GitHub Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
