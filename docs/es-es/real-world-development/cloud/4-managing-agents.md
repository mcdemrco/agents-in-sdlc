---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios pediste al agente de GitHub Copilot en la nube que realizara tres tareas para mejorar la experiencia y añadir funcionalidades. Aunque trabaja de forma autónoma y asíncrona, es importante supervisar su trabajo.

GitHub ofrece varias herramientas para gestionar las tareas asignadas al agente, como la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes consultar las tareas con solicitudes de incorporación de cambios abiertas, revisar las operaciones realizadas y orientar una sesión en curso.

En este ejercicio, harás lo siguiente:

- Explorar la página de agentes para supervisar las tareas del agente en la nube.
- Orientar una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación del modo de alto contraste, el equipo decidió que sería útil añadir también un modo claro. Como ya se están actualizando los estilos y se está añadiendo un control de alternancia, pedirás al agente que incluya el modo claro.

## Revisa las tareas del agente en la nube

Consulta el estado de las tareas que has asignado al agente de GitHub Copilot en la nube.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa las tareas que aparecen en el panel principal y en el panel izquierdo. Deberías ver las tareas asignadas a Copilot, como:
   - Actualizar la documentación del código.
   - Generar API para modificar juegos.
   - Añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - Obtener el código del repositorio.
   - Crear el entorno de trabajo.
   - Configurar servidores MCP.
   - Realizar los pasos necesarios para completar la tarea.

> [!NOTE]
> Los pasos exactos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Revisa también el panel de solicitudes de incorporación de cambios de la derecha, donde puedes consultar la PR y los archivos modificados.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el control de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que le haya asignado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea del agente en la nube con el cuadro de diálogo para orientar la sesión resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al flujo cuando termine el paso que estaba realizando al recibir el mensaje. El proceso tardará varios minutos; puedes aprovechar para repasar lo aprendido.

## Resumen y pasos siguientes

Has explorado la página de agentes de Copilot, el centro de control para supervisar y orientar las tareas del agente en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso.

En este ejercicio:

- Exploraste la página de agentes para supervisar las tareas del agente en la nube.
- Orientaste una sesión en curso para solicitar funcionalidad adicional.

A continuación, [iterarás sobre las solicitudes de incorporación de cambios creadas por Copilot][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
