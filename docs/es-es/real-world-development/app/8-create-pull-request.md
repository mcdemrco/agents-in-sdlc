---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, autoriza Agent Merge para crear y combinar la PR."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios. Tú combinaste la PR de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama.

En esta lección, harás lo siguiente:

- Aprender qué es Agent Merge y cómo automatiza el ciclo de combinación.
- Inspeccionar la PR completa de la funcionalidad y las pruebas de verificación.
- Autorizar Agent Merge solo después de revisarla y confirmar que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión lee la PR y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reorganiza la base cuando es necesario. La combina en cuanto GitHub lo permite. Agent Merge trabaja en segundo plano, sigue funcionando si reinicias la aplicación y se desactiva cuando la PR se combina.

Hasta ahora seleccionabas **Merge pull request** personalmente. Agent Merge puede encargarse de esa tarea, pero todavía necesita tu autorización explícita para editar código y combinar. Revisa las acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Usa Agent Merge para gestionar la PR

Cuando hayas creado y revisado el código, deja que Agent Merge gestione el proceso de la PR.

1. En el selector de agentes, selecciona **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se inicia el proceso de Agent Merge. El agente:

- Creará la solicitud de incorporación de cambios con un título y una descripción.
- Si iniciaste la sesión desde una incidencia, la mencionará en el cuerpo de la descripción.
- Reorganizará la base o resolverá posibles conflictos con la rama de destino.
- Supervisará el proceso de CI para comprobar que se superen todas las comprobaciones.
- Supervisará los comentarios de otros desarrolladores y de la revisión de código de Copilot, y hará cambios para resolverlos.
- Podrá combinar automáticamente la PR cuando todo se haya completado correctamente.

Pide a Agent Merge que combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que **Merge pull request** está marcado.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Supervisa el progreso y resuelve los bloqueos que indique el agente. Confirma que el estado de la PR es **Merged** antes de continuar; habilitar el permiso de combinación o dejar el agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de la solicitud de incorporación de cambios. En concreto:

- Aprendiste qué es Agent Merge y cómo automatiza el ciclo de combinación.
- Inspeccionaste la PR completa de la funcionalidad y las pruebas de verificación.
- Autorizaste Agent Merge solo después de la revisión y confirmaste que la PR se había combinado.

A continuación, [usarás un canvas existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
