---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot tenga acceso a las indicaciones pertinentes. Los [archivos de instrucciones][instruction-files] permiten proporcionar ese contexto para generar código.

En este ejercicio, harás lo siguiente:

- Explorar cómo llegan a Copilot el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones específicos de rutas.
- Generar la primera parte de la capa de datos para el filtrado (un helper de editoriales) con las instrucciones actuales.
- Añadir un nuevo estándar general al repositorio en `.github/copilot-instructions.md`.
- Repetir el mismo prompt y observar cómo el código generado sigue el nuevo estándar.
- Confirmar en `main` la actualización de instrucciones y la parte de filtrado para que Copilot pueda usar las nuevas directrices en el ejercicio siguiente.

> [!CAUTION]
> El código generado puede no seguir todos los estándares: Copilot no es determinista. El objetivo es observar la tendencia tras actualizar las instrucciones, no obtener siempre un resultado idéntico.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe tener un tema oscuro y un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su función.

Los archivos de instrucciones proporcionan a Copilot el contexto necesario para aplicar estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda mejor el estilo y los requisitos de programación. Puedes especificar convenciones de código, bibliotecas y comentarios. Las instrucciones pueden aplicarse a todo el repositorio o a tipos de archivo y tareas concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las solicitudes de chat o CLI.
- `.github/instructions/*.instructions.md`, archivos que contienen directrices para lenguajes, tipos de archivos o tareas concretos, como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el entorno de desarrollo integrado (IDE), los archivos de instrucciones se usan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md; consulta la [referencia de compatibilidad][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

El proyecto de ejemplo muestra un enfoque posible:

- Limita `copilot-instructions.md` a directrices generales del proyecto, como su descripción, estructura y estándares globales de código.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo, como pruebas unitarias, componentes Astro o la capa de datos.
- Escribe las indicaciones en lenguaje natural, con claridad y ejemplos del código que se debe usar y evitar.

No hay una única manera correcta de crear archivos de instrucciones ni de usar IA. La experimentación permitirá encontrar lo que funciona en cada proyecto.

> [!TIP]
> Los proyectos con GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. En este proyecto encontrarás, entre otros, archivos para [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudar a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. El taller del entorno que uses lo indicará cuando sea pertinente.
>
> Para buscar plantillas o ejemplos, explora [awesome-copilot][awesome-copilot], que reúne archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones del proyecto

El proyecto incluye un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas concretas. Examínalos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Examina la descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas para **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora sus archivos. Hay instrucciones para Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` contiene un patrón glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones; por ejemplo, coincide con archivos TypeScript de prueba que cumplan `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y examina al final los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones y permiten reutilizar indicaciones y ejemplos.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentarios en los archivos.

## Crea una rama para los cambios

Crea una rama antes de empezar a trabajar.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de continuar, vuelve al codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo dependen del ejercicio; se indicarán cuando corresponda.

Primero enviarás un prompt con las instrucciones actuales. Después las actualizarás y repetirás el mismo prompt para comparar el resultado.

1. Cierra las pestañas abiertas del editor para que Copilot solo reciba el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. En la vista Chat, selecciona **Agent** en el selector de agentes para que Copilot pueda modificar archivos.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explora el proyecto y aplica cambios, a menudo tanto al helper como a sus pruebas.
6. Observa que el helper propuesto es una función tipada que acepta un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Estas características proceden de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, aplicable a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un comentario de encabezado del archivo.

> [!CAUTION]
> Copilot puede añadir comentarios TSDoc incluso sin que se le pidan. No pasa nada: el objetivo es observar si las instrucciones nuevas mejoran la uniformidad.

## Añade un estándar al repositorio

El archivo `.github/copilot-instructions.md` contiene información general para Copilot. Añade un estándar de programación para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Todavía no incluye una regla de documentación dentro del código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones generales del proyecto pertenecen a `.github/copilot-instructions.md`; las instrucciones para lenguajes, tipos de archivos o tareas concretos pueden ir en archivos `*.instructions.md`.

## Repite el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Comprueba que la función incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique al código con el que trabajarás en el ejercicio siguiente.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones se gestionan con el mismo control de versiones que los demás archivos del repositorio. Confirma los cambios y publica la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirma que las instrucciones y el helper están modificados:

   ```bash
   git status
   ```

3. Prepara y confirma las instrucciones y todos los archivos modificados por Copilot para el helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Con la rama publicada, crea una solicitud de incorporación de cambios y relaciónala con la incidencia del backlog sobre la actualización de las instrucciones. Copilot puede buscar la incidencia, crear la PR y combinarla.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub; no tienes que configurarlas. Es posible que VS Code te pida iniciar sesión la primera vez que Copilot use una herramienta de GitHub. Sigue las indicaciones para permitirlo. En un ejercicio posterior conocerás la conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia y cree una PR desde la rama actual, indicando que incluye las actualizaciones de instrucciones y la base del helper de editoriales:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida **Allow** para realizar acciones de GitHub, revisa la acción y selecciona **Allow** si estás de acuerdo.
6. Cuando se cree la PR, pide a Copilot que la combine y que vuelva a `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot solicite **Allow** para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando antes de permitirlo.

Ya has creado y combinado una solicitud de incorporación de cambios con GitHub Copilot.

## Resumen y pasos siguientes

Has explorado cómo Copilot recoge el contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- Enviar un prompt de generación de código y observar el resultado con las instrucciones actuales.
- Añadir un estándar general al repositorio en `.github/copilot-instructions.md`.
- Repetir el prompt y observar cómo el código propuesto sigue el nuevo estándar.
- Confirmar en `main` la actualización de instrucciones y la base del helper para que el siguiente ejercicio pueda continuar el trabajo.

A continuación, pondrás en práctica esas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una funcionalidad al código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
