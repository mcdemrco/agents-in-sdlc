---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo agente de GitHub Copilot, este explora el código, propone cambios, edita archivos y ejecuta comandos de forma autónoma. Como el trabajo se realiza en tu equipo en tiempo real, Copilot Chat de VS Code muestra las llamadas a herramientas y las ediciones de archivos. Puedes revisar las diferencias, aceptar o rechazar los cambios y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio, harás lo siguiente:

- Supervisar en tiempo real el trabajo del agente de accesibilidad.
- Revisar y aceptar las ediciones propuestas.
- Orientar la sesión para añadir un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un control para el modo claro, pensado para los usuarios a quienes les resulta demasiado oscuro el tema predeterminado. En vez de empezar otra conversación, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con sus diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para observar las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos del terminal que solicita.
3. Cuando el agente proponga una edición, revisa las diferencias:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y pedir al agente que lo intente de nuevo.
4. Abre el indicador **Files changed** de la parte superior del chat para consultar todos los archivos modificados en esta sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería mostrar un mensaje final con un resumen de los cambios.

## Orienta al agente para añadir el modo claro

Ahora que el modo de alto contraste está en marcha, ampliarás la misma conversación para añadir un control de modo claro. Al continuar en la misma sesión, el agente conserva el contexto de los componentes, los estilos y el patrón de almacenamiento local que ya ha usado.

1. En la entrada de Copilot Chat, envía este prompt de seguimiento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente examinará los cambios existentes y propondrá los cambios necesarios para añadir el modo claro.
3. Revisa cada nueva edición y selecciona **Keep** o **Undo** según corresponda.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar el control de alternancia—, detenlo con el botón **Stop** junto a la entrada del chat y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Puedes pedir ajustes («simplify the CSS — use CSS variables instead of duplicating selectors»), corregir el enfoque («don't add a third toggle, integrate light mode into the existing settings panel») o ampliar el alcance («also add keyboard shortcuts for both toggles»).

## Revisa el conjunto de cambios

Antes de confirmar el trabajo, revisa todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que aplican las prácticas del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Publicarás la rama en [la lección siguiente][next-lesson].

## Resumen y pasos siguientes

Has usado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: revisaste las ediciones al aparecer, aceptaste o rechazaste cambios y enviaste prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En este ejercicio:

- Supervisaste el trabajo del agente en tiempo real.
- Aceptaste y rechazaste ediciones de archivos.
- Orientaste la sesión para añadir funcionalidad con prompts de seguimiento.

En la [lección siguiente][next-lesson] publicarás los cambios de accesibilidad como una solicitud de incorporación de cambios y revisarás el trabajo del agente personalizado local que se generó durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
