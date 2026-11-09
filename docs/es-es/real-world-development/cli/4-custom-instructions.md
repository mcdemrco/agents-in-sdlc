---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera determinada, conviene que Copilot tenga acceso a esas indicaciones. Los [archivos de instrucciones][instruction-files] describen tanto qué código se necesita como la forma en que debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección, harás lo siguiente:

- Explorar cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- Actualizar el archivo de instrucciones para que se sigan los estándares de código.
- Observar el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones que no resulten evidentes, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar con TSDoc/JSDoc su finalidad, sus parámetros y sus valores devueltos; también deben documentar el argumento `db` inyectable cuando exista.
- Los componentes reutilizables de Astro deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Se deben conservar las directrices de formato y lint existentes.

Con los archivos de instrucciones proporcionarás a Copilot el contexto necesario para que sus tareas sigan estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda mejor el estilo y los requisitos de programación. Permiten orientar sus sugerencias con convenciones de código, bibliotecas y comentarios. Se pueden crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto que sea pertinente para la mayoría de las solicitudes de chat o CLI.
- `.github/instructions/*.instructions.md`, archivos que contienen directrices para lenguajes, tipos de archivos o tareas concretos.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones de este proyecto

El proyecto de ejemplo ya incluye varios archivos de instrucciones. Examínalos antes de hacer cambios para ver qué orientación está disponible.

1. Vuelve a tu Codespace.
2. En la raíz del repositorio, abre `.github/copilot-instructions.md`.
3. Explora el archivo y fíjate en la descripción del proyecto y las directrices de código. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Hay instrucciones para Astro, la capa de datos Drizzle, las pruebas y otros ámbitos.
5. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior contiene un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y fíjate en los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así se pueden dividir instrucciones extensas en archivos reutilizables más pequeños y señalar ejemplos para Copilot.

## Actualiza los archivos de instrucciones

Modifica el archivo principal `copilot-instructions.md` para que Copilot añada comentarios de documentación.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade como último elemento de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones actualizadas, cierra y reanuda la conversación de filtrado para que Copilot las use sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio y en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Comprueba que los nuevos comentarios TSDoc describen el código con precisión.

## Resumen y pasos siguientes

Has explorado cómo Copilot CLI recoge el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- Exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- Actualizaste el archivo de instrucciones para que se sigan los estándares de código.
- Observaste el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para que las pruebas y lint se ejecuten de forma uniforme.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
