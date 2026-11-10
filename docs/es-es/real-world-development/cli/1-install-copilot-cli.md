---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con la herramienta y encuentra la incidencia de filtrado preparada."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal. Permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas y solicitar cambios sin perder la concentración. El primer paso es instalar la herramienta, algo que puedes hacer con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- autenticarás tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación.
- encontrarás la incidencia de filtrado mediante el servidor GitHub MCP integrado.

## Escenario

Tu equipo empieza a usar agentes de IA para abordar un backlog cada vez mayor. Copilot CLI incorpora esa capacidad al terminal, donde ya trabajan muchos desarrolladores. Esta lección te permitirá instalarlo, autenticarte y dejarlo listo para el resto del taller.

## Instalar Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js, usarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js esté instalado y cumpla el requisito de versión:

   ```bash
   node --version
   ```

   Debe aparecer la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Verifica la instalación:

   ```bash
   copilot --version
   ```

   Se mostrará el número de versión.

## Autenticarte con GitHub

La primera vez que se inicia, Copilot CLI solicita la autenticación con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones del código de dispositivo para autenticarte y autorizar Copilot CLI.
3. Copilot CLI muestra este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirma que la ruta corresponde al repositorio Tailspin Toys y responde que sí seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un Codespace puede que ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya puedes continuar.

## Familiarizarte con la herramienta

Los comandos del prompt normal del shell se ejecutan directamente en Codespace. Una vez iniciado Copilot CLI, el lenguaje natural se envía al agente y los comandos slash controlan la conversación.

1. Escribe `/model`, usa las flechas para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsarlo para confirmar.
2. Escribe `/help` para ver los comandos de la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que el servidor GitHub MCP integrado está disponible.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La usarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama seleccionada en el terminal; no crea automáticamente un worktree. Crea una rama de características antes de cada cambio.

## Usar el atajo del taller

Normalmente, Copilot CLI pregunta antes de usar herramientas fuera de los permisos establecidos. En este taller lo volverás a iniciar con `--yolo`, un atajo aprobado por el usuario que elimina esas solicitudes dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual, y nunca lo conviertas en un alias predeterminado para el desarrollo diario. Consulta [Allowing and denying tool use][allow-all-warning].

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas de lectura y escritura de GitHub MCP que usarán las lecciones posteriores con incidencias y pull requests. Codespace limita el acceso a tu equipo local, pero los recursos de GitHub autenticados son reales. Revisa los cambios antes de publicarlos o fusionarlos.

1. Sal de Copilot CLI con `/exit`.
2. Reinícialo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para confirmar que la conversación funciona y sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, cuando cambies las instrucciones o añadas un agente, usa `copilot --resume` para volver a la misma conversación y rama de la característica.

## Resumen y próximos pasos

¡Enhorabuena! En esta lección:

- instalaste GitHub Copilot CLI con npm.
- autenticase tu cuenta de GitHub.
- confiaste en el repositorio del taller y probaste una conversación.
- encontraste la incidencia de filtrado mediante el servidor GitHub MCP integrado.

A continuación, [iniciarás tu primer cambio acotado][next-lesson] y usarás Copilot CLI para mostrar una valoración con estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
