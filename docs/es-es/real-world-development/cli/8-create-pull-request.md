---
title: "Lección 8 - Crear y combinar la solicitud de incorporación de cambios (PR) de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de control de calidad (QA) y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora revisarás el conjunto y abrirás una solicitud de incorporación de cambios. Tú combinaste la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para agilizar el taller, has mantenido el flujo completo de filtrado y calidad en una conversación y una rama.

En esta lección, harás lo siguiente:

- Aprender qué es Agent Merge y cómo automatiza el ciclo de combinación.
- Inspeccionar el cambio completo de la funcionalidad y las pruebas de verificación.
- Crear la PR de filtrado.
- Activar Agent Merge solo después de la revisión y confirmar que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al activarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reorganiza la rama de ser necesario. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora seleccionabas **Merge pull request**. Agent Merge puede encargarse de ese proceso. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

Revisa todo el código, crea la PR y deja que Agent Merge gestione el resto.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se complete correctamente, la combinará.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y, cuando el repositorio lo permita, realiza manualmente la combinación después de revisar los cambios.

## Resumen y pasos siguientes

Has automatizado varias partes del desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de solicitud de incorporación de cambios. En concreto:

- Aprendiste qué es Agent Merge y cómo automatiza el ciclo de combinación.
- Inspeccionaste el cambio completo de la funcionalidad y las pruebas de verificación.
- Creaste la PR de filtrado.
- Activaste Agent Merge solo después de la revisión y confirmaste que la PR se combinó.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
