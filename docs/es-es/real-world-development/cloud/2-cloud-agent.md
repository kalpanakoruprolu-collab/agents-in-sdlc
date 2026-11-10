---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Probablemente haya muy pocas organizaciones —si es que hay alguna— que no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog porque no ha habido tiempo para implementarlas. El agente en la nube de GitHub Copilot está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de cambios en borrador para que la revise un desarrollador. Así se pueden delegar tareas tediosas, acelerar el desarrollo y permitir que los desarrolladores se centren en cuestiones de mayor alcance.

Explorarás los siguientes aspectos del agente en la nube de Copilot:

- personalizar el entorno de generación de código;
- asegurarte de que las operaciones se realicen de forma segura;
- comprender la importancia de delimitar claramente las incidencias;
- asignar incidencias a Copilot.

## Situaciones

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco satisfactorio: como verás, no hay documentación. Como primer paso, quieren añadir comentarios de documentación de TypeScript (TSDoc) a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la búsqueda de juegos. Quiere que la página de detalles de cada juego muestre otros títulos relacionados, es decir, juegos de la misma categoría, para que los usuarios puedan seguir explorando. No necesitan una implementación pulida todavía; solo quieren algo que les permita probar la experiencia de usuario como parte de las pruebas de aceptación. Aunque esta tarea bloquea el trabajo actual, hay otras incidencias con mayor prioridad.

Estas son tareas que suelen perder prioridad y que se pueden asignar al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas, para después volver a revisar el trabajo de Copilot y comprobar que todo es correcto.
## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma similar a un desarrollador. Al igual que ocurre al trabajar con otro desarrollador, puedes hacerlo de varias maneras, por ejemplo, [asignando una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot creará una solicitud de cambios (PR) en borrador para hacer un seguimiento del progreso, configurará un entorno y empezará a trabajar en la tarea. Puedes consultar la sesión de Copilot mientras está en curso o una vez finalizada. Cuando la solución propuesta esté lista para que la revises, Copilot te mencionará en la solicitud de cambios.
## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar. Incluso las tareas que parecen sencillas pueden ser complejas al analizarlas en profundidad, así que [delimita con cuidado las tareas que asignes al agente en la nube de Copilot][cloud-agent-best-practices]. Trátalo como a un compañero de programación basado en inteligencia artificial: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por añadir inteligencia artificial generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has realizado el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube de Copilot, revisa los archivos de instrucciones que ya incluye este repositorio, sin modificarlos, para que más adelante puedas observar su efecto.

Abre los siguientes archivos en la interfaz web de GitHub de tu repositorio o en un codespace si ya tienes uno en marcha:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas sobre las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en los metadatos `applyTo`, que limitan el alcance de estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation` en la sección siguiente, observa si la solicitud de cambios contiene comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones. Volveremos a mencionarlo al revisar las solicitudes de cambios en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube de Copilot

Para crear código suele ser necesario contar con un entorno concreto y ejecutar algunos scripts de configuración para garantizar que todo esté en buen estado. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en el archivo `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas de desarrollo y dependencias necesarias. Este flujo ya está configurado para facilitar el desarrollo del laboratorio y ofrecerte esta oportunidad de aprendizaje. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales para la única aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no clonas el repositorio en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación utiliza Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

Como cualquier otro archivo de flujo de trabajo de GitHub, tiene algunos puntos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Este trabajo se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona correctamente sin tener que esperar a que Copilot lo ejecute.

## Añadir documentación

Todo el mundo entiende la importancia de la documentación, pero la mayoría de los proyectos tienen información desactualizada o carecen de documentación. Este tipo de deuda técnica suele quedar sin resolver, lo que reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores al equipo. Por suerte, Copilot es especialmente útil para crear documentación, así que esta es una incidencia idónea para asignarle al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás a revisar su trabajo.

1. Abre una pestaña nueva del navegador y ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación de incidencias.
4. Selecciona **Blank issue** para crear la incidencia.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación a Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de cambios que se acaba de generar. Su título será parecido a `[WIP]: Code lacks documentation`. Si no aparece en la lista, espera un momento y actualiza la ventana del navegador.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> Es posible que la lista de tareas de Copilot tarde varios minutos en aparecer en la solicitud de cambios. Copilot está configurando el entorno (ejecutando el flujo de trabajo descrito anteriormente), analizando el proyecto y determinando cómo abordar el problema.

13. Revisa la lista y las tareas que Copilot va a realizar.
14. Desplázate hacia abajo por la cronología de la solicitud de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema: explora el código y comprende el estado del proyecto, se detiene para pensar y decidir el plan adecuado, y crea código.

Este proceso probablemente tardará varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es permitir que trabaje de forma asíncrona y liberarte para que te ocupes de otras tareas. Aprovecharemos precisamente esa función: asignaremos otra tarea al agente en la nube de Copilot y después nos ocuparemos de escribir código para añadir funcionalidades a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles de un juego

Como se ha indicado, una de las ventajas del agente en la nube de GitHub Copilot es que permite dividir el trabajo: tú puedes centrarte en un conjunto de tareas mientras el agente se ocupa de otro. Añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, pero ese tiempo se puede dedicar a otras tareas. ¡Vamos a asignársela al agente en la nube de Copilot!

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de creación de incidencias.
4. Selecciona **Blank issue** para utilizar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo, verás un conjunto de 👀 en el primer comentario de la incidencia. Esto indica que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. El agente en la nube de Copilot trabaja de forma parecida a un ingeniero de software, así que no hace falta que lo supervises continuamente; puedes revisar su trabajo cuando lo haya terminado. Ahora vamos a crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignar incidencias a Copilot para que trabaje en ellas de forma asíncrona. Copilot puede ocuparse de la deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código;
- asegurarte de que las operaciones se realicen de forma segura;
- comprender la importancia de delimitar claramente las incidencias;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, nos centraremos en crear y utilizar agentes personalizados. [El agente en la nube de Copilot también puede utilizar servidores del Protocolo de contexto de modelo (MCP)][cloud-agent-mcp] e instrucciones personalizadas, que ya hemos visto en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
