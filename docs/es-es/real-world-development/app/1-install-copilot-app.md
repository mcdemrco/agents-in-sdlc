---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio que has creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de integración continua funcionan desde el principio. Está diseñada para dirigir varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, y automatizar tareas repetitivas. Con Node.js instalado y tu copia del proyecto preparada, ahora instalarás la aplicación y conectarás el repositorio.

En esta lección, harás lo siguiente:

- Instalar la aplicación GitHub Copilot e iniciar sesión.
- Añadir a la aplicación tu proyecto desde su repositorio de GitHub.
- Familiarizarte con el espacio de trabajo y el backlog que creó la plantilla.
- Probar un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar un backlog cada vez mayor. La aplicación Copilot ofrece un único lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación, y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Necesitas un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

La aplicación está disponible para Windows, macOS y Linux. Instálala, autentícate y añade el repositorio de Tailspin Toys.

1. En un navegador, abre la [página de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación.
4. Selecciona **Sign in to GitHub** y sigue las instrucciones para autenticarte. Si usas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se te solicite. GitHub Enterprise Server no es compatible.
5. Si el proceso de configuración inicial te ofrece seleccionar repositorios, selecciona el repositorio Tailspin Toys que creaste a partir de la plantilla. También puedes omitir este paso y añadirlo después.
6. Cuando se te solicite, elige un tema y termina el proceso de configuración inicial.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece, no tienes que volver a añadirlo.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Usa el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En este diseño, las carpetas de repositorio, como `tailspin-toys`, aparecen agrupadas en **Sessions**, con las sesiones individuales debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, conoce las áreas de la barra lateral:

- **New**: inicia una sesión de chat con Copilot.
- **Pull requests**: explora y filtra solicitudes de incorporación de cambios, consulta el estado de CI y revisa PR mediante la integración nativa con GitHub, sin salir de la aplicación.
- **Issues**: explora y filtra incidencias e inicia sesiones a partir de ellas.
- **Automations**: consulta tareas de agente guardadas que se ejecutan según un horario o bajo demanda. Sirven, por ejemplo, para gestionar listas de tareas y realizar tareas periódicas de mantenimiento. El repaso las presenta como un paso siguiente, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación mediante servidores MCP, plugins, habilidades y otros componentes. La usarás para configurar Playwright MCP.
- **Chats**: mantén conversaciones ligeras para preguntas o lluvias de ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: consulta las carpetas de repositorio y las sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o una sesión para volver a esa conversación. Cada sesión se ejecuta en un espacio de trabajo aislado, así que puedes ejecutar varias a la vez sin que sus cambios entren en conflicto. Iniciarás tu primera sesión al añadir valoraciones por estrellas.

Explorarás el espacio de trabajo durante el taller.

> [!TIP]
> Si tienes dudas sobre cómo hacer algo o sobre si es posible, pregunta a Copilot. Puede orientarte.

### Localiza el backlog inicial

La plantilla creó incidencias de GitHub para Tailspin Toys. Localiza las siguientes por el título, sin dar por hecho su número:

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por el título:
   - Allow users to filter games by category and publisher
   - Update our repository coding standards
3. Selecciona una incidencia y lee los detalles. Cada incidencia también permite iniciar una sesión de agente. Después de un primer cambio rápido, partirás de la incidencia de filtrado.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** solo muestran elementos de los repositorios añadidos a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Un chat rápido permite hacer una pregunta o plantear ideas sin crear una rama ni un árbol de trabajo. Es adecuado para consultas breves que no necesitan una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat. Si **Chats** todavía no está visible, selecciona **+** junto a **Sessions** para iniciar tu primer chat; después aparecerá **Chats**.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar otra cuando quieras.

## Resumen y pasos siguientes

Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- Instalar la aplicación e iniciar sesión en GitHub.
- Añadir un proyecto desde su repositorio de GitHub.
- Familiarizarte con el espacio de trabajo y localizar el backlog inicial en **Issues**.
- Usar un chat rápido para hacer una pregunta breve.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y la usarás para mostrar valoraciones por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
