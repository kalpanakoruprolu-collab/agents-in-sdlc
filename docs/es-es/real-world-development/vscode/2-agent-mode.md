---
title: "Ejercicio 2 - Añadir funcionalidades con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones, como ejecutar pruebas. Como desarrollador, tu flujo de trabajo suele consistir en buscar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado, actualizarlo y volver a empezar. La lista continúa.

Aquí es donde entra en juego el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el entorno de desarrollo integrado (IDE). Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo puedes utilizar el modo agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque sus nombres sean parecidos, el modo agente y el agente en la nube están diseñados para ofrecer experiencias distintas. El modo agente realiza las tareas en el IDE, lo que permite ciclos de respuesta rápidos e interacción. El agente en la nube está diseñado para trabajar de forma asíncrona como un compañero del equipo, que interactúa contigo mediante incidencias y solicitudes de cambios.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y hacer cambios coordinados;
- el modo agente de GitHub Copilot puede implementar nuevas funcionalidades en la interfaz de usuario (UI) y la capa de datos;
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de fusionarlos en el código base.

## Situación

A medida que aumenta la lista de juegos, quieres que los usuarios puedan filtrarla por categoría y editorial. En el ejercicio anterior ya añadiste una función auxiliar para editoriales; ahora completarás el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para entender sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y consultar sus detalles. El sitio es una única aplicación Astro que genera páginas en lenguaje de marcado de hipertexto (HTML) estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante el mapeador objeto-relacional (ORM) Drizzle en su bloque frontmatter; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz de usuario del lado del cliente. Las funciones auxiliares reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con un elemento `<script>` de Astro de ámbito limitado y las API estándar del modelo de objetos del documento (DOM).

### Iniciar el sitio web

Se ha preparado una tarea `npm run dev` para iniciar el único servidor de desarrollo de Astro. Puedes ejecutarla en GitHub Codespaces siguiendo estos pasos:

1. Vuelve al codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal nuevo en el Codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella para trabajar en el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Abre el sitio web manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> En un codespace, al seleccionar el enlace de la URL localhost en el terminal del Codespace, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Una vez iniciado el sitio web, puedes explorar sus funciones. Entre las principales se encuentran:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea pertinente.

La implementación inicial del sitio web funciona, pero queremos mejorarla con nuevas funciones. Empecemos por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te muestre esos elementos.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos en el terminal e invoca herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no están expresando una preferencia por un modelo concreto. Durante su desarrollo utilizamos Claude Sonnet 4.5 y por eso aparece en estas instrucciones. El objetivo es que las sugerencias de código sean relativamente coherentes y ofrezcan una buena experiencia. Sin embargo, como los modelos de lenguaje grandes (LLM, por sus siglas en inglés) son probabilísticos, es posible que las sugerencias que recibas sean distintas a las que se describen en el taller. Es completamente normal y esperado.

> [!NOTE]
> Debido al carácter probabilístico de los LLM, Copilot puede utilizar un comando del Protocolo de contexto de modelo (MCP) distinto y aun así completar la tarea.

3. Pide a Copilot que muestre el backlog de incidencias con esta instrucción:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado los elementos según los que considera más útiles para el usuario.

## Revisar los archivos de instrucciones

Antes de pedirle al agente que genere código, conviene revisar el archivo de instrucciones que utilizarás para proporcionar contexto a Copilot. Utilizarás el archivo de [interfaz de usuario](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio web.

1. En el codespace, ve a `.github/instructions/ui.instructions.md`.
2. Fíjate en las pautas generales para añadir funcionalidades. Incluyen:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, pruebas y accesibilidad;
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, entre ellos:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar contenido, dividir instrucciones complejas en archivos más pequeños y manejables, y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, habrá que realizar al menos tres actualizaciones distintas en la aplicación:

- añadir o ajustar la lógica de filtrado restante en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, hay que ejecutar las pruebas y comprobar que se superan antes de fusionar los cambios en el código base. El modo agente de Copilot puede realizar estas tareas por ti. Vamos a añadir la funcionalidad.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de un ejemplo de selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** siga seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog con esta instrucción:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot empieza explorando el proyecto y localizando los archivos relacionados con la funcionalidad. Deberías verlo buscando las funciones auxiliares de la capa de datos, la interfaz de usuario y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. En función de la complejidad de la tarea y del código base, puede tardar unos minutos. Durante el proceso, es posible que aparezcan los botones **Keep** y **Undo** en el editor de código. Cuando Copilot termine, verás **Keep** o **Undo** para todos los cambios. No es necesario seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en el panel Copilot Chat que pide confirmar si se quieren ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Captura de pantalla de una sesión completa con el modo agente de Copilot](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de inteligencia artificial.

10. Vuelve al navegador donde está abierto el sitio web y explora la nueva funcionalidad.
11. Cuando hayas comprobado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

Enhorabuena. En este ejercicio has aprendido a utilizar el modo agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Has aprendido cómo:

- el modo agente de GitHub Copilot puede implementar funciones nuevas en la interfaz de usuario y la capa de datos;
- el modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y hacer cambios coordinados;
- revisar los cambios y las pruebas generados por el modo agente de Copilot antes de fusionarlos en el código base.

Ahora [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que aumenta la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que actualice el sitio para implementarla. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones actuales;
- piensa cómo quieres implementar la paginación: si quieres permitir que el usuario seleccione el tamaño de página o prefieres que sea fijo;
- al crear la instrucción, proporciona a Copilot las indicaciones necesarias para que implemente la solución que deseas;
- puede que tengas que iterar con GitHub Copilot, solicitando cambios y proporcionando contexto. Este es el flujo normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
