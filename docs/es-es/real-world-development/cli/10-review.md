---
title: "Lección 10 - Repaso y pasos siguientes"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo de Tailspin Toys. En concreto:

- Preparaste un Codespace, instalaste Copilot CLI, exploraste el proyecto y localizaste la incidencia de filtrado del backlog.
- Añadiste las valoraciones por estrellas, revisaste el resultado en un navegador reenviado y combinaste manualmente tu primera solicitud de incorporación de cambios (PR).
- Partiste de la incidencia de filtrado, definiste el enfoque con Plan, lo implementaste con Autopilot y lo revisaste con Interactive.
- Orientaste al agente con instrucciones personalizadas, personalizaste la habilidad `quality-checks` y la usaste para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- Añadiste el servidor Model Context Protocol (MCP) de Playwright y lo usaste para explorar el filtrado en un navegador real.
- Creaste y seleccionaste un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador.
- Revisaste todo el cambio de filtrado y autorizaste Agent Merge para la PR.
- Exploraste comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama creada desde `main` actualizado:

1. **Valoraciones por estrellas:** mostrar el valor existente de `starRating` y un estado explícito para las tarjetas sin valoración.
2. **Flujo de filtrado y calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la creación de su PR, usaste la misma conversación y rama. Agrupamos ese trabajo en una PR para agilizar el taller.

## Distintos tipos de verificación

Comprobaste el código de varias maneras: con pruebas automatizadas, mediante tu propia inspección en el navegador y con la exploración del navegador de Copilot a través de MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, e incluyó los resultados en el nuevo formato. QA reunió esos resultados con la revisión de los requisitos y de la cobertura de pruebas antes de la PR.

Las pruebas nuevas deben cubrir carencias reales; si una ejecución de QA determina que no hacen falta pruebas, puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y vuelve a generar las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller actualizaste instrucciones, personalizaste una habilidad, creaste un perfil de QA y configuraste un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir un enfoque antes de empezar, **Interactive** para mantener el control de cambios específicos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para ediciones rutinarias y uno más capaz para tareas complejas.

El contexto sigue siendo tan importante como la infraestructura. Explicar claramente *qué* quieres crear, *por qué* y *cómo* influye de forma significativa en el resultado.

## Más opciones para explorar

Ya has recorrido el flujo principal. También puedes explorar estas funciones de CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente de GitHub Copilot en la nube.

## Pasos siguientes

La mejor forma de mejorar con cualquier herramienta es seguir usándola: en código de producción, en proyectos personales o en esa pequeña aplicación que tienes en mente desde hace años. Comparte lo que aprendas con el equipo y aprovecha lo que aprendan los demás. Y, como siempre, consulta la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la [serie opcional Backer Concierge con Foundry][foundry]. Para comparar otros entornos, explora los talleres de [VS Code][vscode], la [aplicación GitHub Copilot][app] o el [agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
