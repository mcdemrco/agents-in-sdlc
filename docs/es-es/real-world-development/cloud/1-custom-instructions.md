---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una manera determinada o Copilot necesita información adicional, conviene que tenga acceso a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan directrices para que Copilot entienda tanto qué se quiere como cómo hacerlo.

En este ejercicio, harás lo siguiente:

- Explorar cómo llegan al agente el contexto del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones específicos de rutas.
- Añadir un nuevo estándar general a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no compararás prompts antes y después. El agente en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil demostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones al revisar las PR que genere el agente en ejercicios posteriores.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz de usuario debe tener un tema oscuro y un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que explique su función.

Usarás archivos de instrucciones para proporcionar a Copilot el contexto necesario para seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda mejor el estilo y los requisitos de programación. Puedes especificar convenciones de código, bibliotecas y comentarios. Las instrucciones pueden aplicarse a todo el repositorio o a tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto pertinente para la mayoría de las solicitudes de chat o CLI.
- `.github/instructions/*.instructions.md`, archivos que contienen directrices para lenguajes, tipos de archivos o tareas concretos, como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones se usan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con frontmatter `applyTo`.
>
> Copilot también admite otros formatos de instrucciones, como AGENTS.md y CLAUDE.md; consulta la [referencia de compatibilidad][custom-instructions-support].

### Prácticas recomendadas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque posible:

- Limita `copilot-instructions.md` a directrices generales del proyecto, como su descripción, estructura y estándares globales de código.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo, como pruebas unitarias, componentes Astro, la capa de datos o tareas concretas.
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

## Explora los archivos de instrucciones personalizados del proyecto

El repositorio incluye un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para tareas concretas. Examínalos en la interfaz web de GitHub o en un codespace.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas para **GitHub Actions Workflows**, que se aplican a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explora los archivos. Hay instrucciones para Astro, la capa de datos Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` contiene un patrón glob relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones; por ejemplo, coincide con archivos TypeScript de prueba que cumplan `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y examina al final los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Las rutas son relativas al archivo de instrucciones y permiten reutilizar indicaciones y ejemplos.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para comentarios TSDoc y encabezados de comentarios en los archivos.

## Añade un estándar al repositorio

Añadirás al archivo `copilot-instructions.md` una regla general para que el agente en la nube la aplique a las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se asigne la incidencia, pero así también podrás confirmar los cambios de forma ordenada.

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

El archivo `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación, pero todavía no incluye documentación dentro del código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones generales del proyecto pertenecen a `.github/copilot-instructions.md`; las instrucciones para lenguajes, tipos de archivos o tareas concretos pueden ir en archivos `*.instructions.md`.

## Confirma, publica y combina la actualización de instrucciones

Cuando asignes una incidencia al agente en la nube en el siguiente ejercicio, este creará una rama desde `main`. Por eso, los cambios de instrucciones deben llegar a `main` para que el agente los use.

1. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. Así el agente en la nube podrá leer las instrucciones al trabajar en las incidencias del ejercicio siguiente.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante el taller, puedes omitir la rama y confirmar los cambios directamente en `main`. La rama sirve para practicar un flujo de trabajo habitual en proyectos reales.

## Resumen y pasos siguientes

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las PR que genere el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
