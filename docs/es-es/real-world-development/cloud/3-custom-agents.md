---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del desarrollo. Sus archivos Markdown, guardados en `.github/agents`, proporcionan instrucciones, prácticas recomendadas, patrones de programación y conocimiento del dominio. Así, los equipos pueden convertir sus conocimientos en agentes reutilizables: por ejemplo, uno de accesibilidad que aplique las [pautas de accesibilidad para el contenido web (WCAG)][wcag], uno de seguridad o uno de pruebas.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo tiene frontmatter YAML con, al menos, los campos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] comparten algunos aspectos: ambos se definen principalmente con archivos Markdown e indican a una IA cómo realizar tareas. La distinción más clara es que el **agente personalizado** es quien realiza el trabajo, mientras que las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio según las directrices correspondientes; para hacerlo, podría usar habilidades de flujo de solicitudes de incorporación de cambios o de ejecución y gestión de pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar lo que funciona en cada entorno y escenario.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio, aprenderás a:

- Revisar cómo se definen los agentes personalizados.
- Asignar una tarea a un agente personalizado.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todos, independientemente de sus capacidades o preferencias visuales. Los comentarios recientes indican que el tema oscuro puede resultar difícil de leer porque el contraste entre el texto y el fondo es insuficiente. El equipo de diseño ha solicitado un modo de alto contraste que los usuarios puedan activar y desactivar.

Como la accesibilidad es importante, usarás un agente personalizado para generar esta funcionalidad cuanto antes.

## Revisa el agente personalizado de accesibilidad

Ya hay un agente personalizado para accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

1. Vuelve al codespace y abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - Las responsabilidades principales al generar código para un sitio accesible.
   - Las prácticas recomendadas de accesibilidad.
   - Ejemplos de código HTML, CSS y JavaScript.
   - Errores habituales que se deben evitar.

## Crea y asigna una incidencia

Mission control es el centro para gestionar agentes del entorno. Puedes asignarles tareas, supervisar su trabajo y proporcionar orientación adicional. Asignarás una tarea para crear el modo de alto contraste.

1. Abre el repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. Escribe esta descripción en **Description**:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Asignación al agente en la nube con el agente Accessibility resaltado](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y pasos siguientes

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes especializados en tareas y ámbitos concretos. Permiten codificar los conocimientos y estándares del equipo en agentes reutilizables.

En este ejercicio:

- Revisaste cómo se definen los agentes personalizados.
- Asignaste una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, [supervisarás y orientarás la sesión del agente][next-lesson] desde mission control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Preparar agentes personalizados en una organización][org-custom-agents]
- [Preparar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
