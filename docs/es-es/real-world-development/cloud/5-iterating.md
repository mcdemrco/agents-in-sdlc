---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

En este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia y añadir funcionalidades: documentar el código, crear una funcionalidad de juegos relacionados y añadir controles de accesibilidad para los modos de alto contraste y claro. Revisarás los cambios y, si hace falta, pedirás a Copilot que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian con la IA generativa. Hay que revisar el código generado y seguir el proceso habitual de desarrollo. Revisarás las propuestas de Copilot para documentar el código, añadir juegos relacionados y mejorar la accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente trabaja de forma asíncrona y sin supervisión, se han establecido restricciones para mantener la seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama en la que trabaja.
- El agente se ejecuta en GitHub Actions, dentro de un entorno separado y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente: la documentación del código. La revisarás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que veas un aviso indicando que un firewall bloquea a GitHub Copilot. Es lo esperado: el agente tiene acceso limitado de forma predeterminada a los recursos externos, incluidas las llamadas a servidores MCP. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la PR titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Examina el código actualizado. Los cambios exactos pueden variar; busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios, según los archivos de instrucciones que revisaste al principio del Ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos de trabajo se han puesto en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían pasar para la aplicación Astro. El proceso puede tardar varios minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot no es unidireccional. Puedes mencionarlo en comentarios generales o comentarios insertados en el código de la PR; Copilot leerá el comentario e iniciará otra sesión para resolverlo. Como los resultados no son deterministas, no hay un prompt único. Puedes pedirle, por ejemplo:

- Añadir encabezados que expliquen la finalidad de cada archivo de código.
- Añadir comentarios TSDoc a archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario a la PR solicitando cambios en la documentación generada y menciona a **@copilot**. Puedes usar una de las ideas anteriores o proponer otra mejora.
2. Selecciona **View Session** para observar el trabajo. Copilot iniciará una sesión nueva para realizar los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, comprueba que haya una confirmación nueva en la PR.
5. Selecciona **Files changed** y revisa los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Ahora revisarás la PR que Copilot generó para mostrar juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Comprueba que los flujos se pongan en cola en la sección de comprobaciones. Si todo va bien, las comprobaciones de la aplicación Astro deberían pasar. El proceso puede tardar varios minutos.
9. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la funcionalidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes seguir iterando con el agente en la nube, por ejemplo, para cambiar el número de juegos que se muestran o pedir que añada encabezados y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para solicitar cambios, añade otro comentario en **Conversation**; Copilot lo leerá e iniciará una sesión nueva.

## Revisa las funcionalidades de accesibilidad

Por último, revisarás las funcionalidades implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste que pediste en el Ejercicio 3 y el modo claro que solicitaste desde mission control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** y revisa el código generado.
5. Examina la implementación y presta atención a:
   - Los controles de alternancia de la interfaz para cambiar de modo.
   - El uso del almacenamiento local para conservar las preferencias.
   - Los cambios de CSS o estilos para los modos de alto contraste y claro.
   - Los atributos de accesibilidad, como etiquetas ARIA y navegación con teclado.
   - El código JavaScript/TypeScript que gestiona los cambios de modo.

6. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

    ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Comprueba que los flujos se pongan en cola en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones de la aplicación Astro deberían pasar. El proceso puede tardar varios minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente la accesibilidad. Abre un terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Confirma que funcionan y que la preferencia se conserva al volver a cargar la página.

El agente personalizado de accesibilidad orientó a Copilot para implementar estas funcionalidades siguiendo prácticas de accesibilidad. Si detectas problemas o mejoras posibles, puedes mencionar a **@copilot** en un comentario y solicitar cambios.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para asignarle tareas reales del backlog mientras te ocupas de otras cosas. Para practicar, crea más incidencias en el repositorio y asígnaselas a Copilot. Por ejemplo:

- Crear un formulario para expresar interés en un juego.
- Implementar paginación en la página de lista de juegos.
- Añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Inspeccionaste los archivos de instrucciones personalizados del repositorio** para observar su efecto en el trabajo del agente.
- **Asignaste incidencias a Copilot** y observaste cómo preparó el entorno, planificó y trabajó de forma asíncrona.
- **Creaste y usaste un agente personalizado** de accesibilidad para añadir controles de modo de alto contraste y modo claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad.
- **Revisaste e iteraste sobre las PR del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y pasos siguientes

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP desde el IDE.
- **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
