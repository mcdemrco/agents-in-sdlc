---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezaste por añadir una funcionalidad pequeña al proyecto, pero los cambios más amplios requieren un proceso más sólido. La aplicación GitHub Copilot está diseñada para adaptarse al flujo existente de una organización y ayudar a crear lo adecuado de la forma correcta. En esta lección iniciarás un proceso de desarrollo habitual dirigido por agentes: partirás de una incidencia, planificarás e implementarás una funcionalidad, comprobarás el código y su comportamiento y, finalmente, la combinarás en el proyecto.

> [!NOTE]
> Usarás la misma sesión durante todo el flujo de trabajo de la funcionalidad. Normalmente se usarían sesiones o solicitudes de incorporación de cambios (PR) distintas para los diferentes tipos de archivo, pero agruparemos el trabajo para centrarnos en los conceptos principales.

En esta lección, harás lo siguiente:

- Iniciar una sesión de agente desde una incidencia de GitHub.
- Definir los requisitos en modo **Plan**.
- Implementar la funcionalidad en modo **Autopilot**.
- Revisar el código.
- Validar manualmente la funcionalidad en un canvas de navegador.

Al continuar con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks` existente, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan acotar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles antes de programar, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La IA no cambia los fundamentos del desarrollo. La mayoría de los desarrolladores siguen un flujo parecido:

1. Abrir una incidencia que detalle lo que debe hacerse.
2. Crear un plan de lo que debe desarrollarse.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código después de que se revise y se complete correctamente el proceso de integración continua.

> [!NOTE]
> Los detalles varían según el equipo y la organización, pero la mayoría de los procesos siguen un flujo parecido.

Este enfoque ayuda a comprobar que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla la autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo del prompt y cambiarlo en cualquier momento:

- **Interactive**: trabajas con el agente. Este sugiere cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que el agente lo ejecute.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de continuar, confirma que la PR de valoraciones por estrellas está combinada y que tu `main` local está actualizado.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys en la barra lateral y, después, **Issues** en la navegación superior del repositorio. Abre **Allow users to filter games by category and publisher**. Es una incidencia existente creada por la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal pero no en la sesión, revisa la configuración de `PATH` o del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones que fallaron. No reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto.

1. Envía este prompt:

   ```plaintext
   Build this feature.
   ```

2. Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios en el acceso a datos, lint, pruebas unitarias, pruebas de extremo a extremo (E2E) y comprobaciones de tipos con las herramientas existentes del proyecto.
3. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando no hay juegos coincidentes.
4. Pide los cambios necesarios antes de aprobar el plan y conserva la URL de la incidencia y las aclaraciones acordadas para la validación posterior. Céntrate en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad.

## Aprueba Autopilot explícitamente

Cuando estés conforme con el plan, autoriza la implementación en modo Autopilot.

1. Selecciona **Approve and implement this plan** o la opción equivalente de tu versión.
2. Confirma que el indicador del modo muestra **Autopilot**; no des por hecho que la aprobación cambia el modo.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente; revísalo antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por completadas. Detén solo los servidores que hayas iniciado.

## Revisa y verifica la implementación

Revisa el código generado antes de combinarlo, como harías con cualquier otro código. Comprueba también el sitio para confirmar que funciona.

1. Abre **Changes** y examina la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Examina los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; confirma que el servidor probado pertenece a este árbol de trabajo, no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y ábrela en un canvas de navegador.

1. Envía este prompt para pedir a Copilot que inicie la aplicación y abra la página en el canvas:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, la aplicación se iniciará y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y confirma que los resultados coinciden con ella.
4. Combina los filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, verifica el comportamiento acordado.
5. Prueba una combinación sin juegos coincidentes y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo de esta sesión y cierre el canvas:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR del filtrado. Conserva esta sesión, el árbol de trabajo y la rama para las Lecciones 4 a 8. En la siguiente lección aplicarás instrucciones personalizadas al código de filtrado; después añadirás mejoras a la habilidad y el agente de QA, antes de crear y combinar la PR conjunta en la Lección 8. Cuando detengas el servidor, puedes dejar la sesión inactiva y volver a ella en la siguiente lección.

## Resumen y pasos siguientes

Has usado distintos modos de agente para desarrollar y revisar una funcionalidad. En esta lección:

- Iniciaste una sesión de agente desde una incidencia de GitHub.
- Definiste los requisitos en modo **Plan**.
- Implementaste la funcionalidad en modo **Autopilot**.
- Revisaste el código.
- Validaste manualmente la funcionalidad en un canvas de navegador.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que siga las prácticas documentadas mediante el [uso de instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
