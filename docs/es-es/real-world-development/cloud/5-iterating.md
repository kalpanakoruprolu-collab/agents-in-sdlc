---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisar el trabajo

A lo largo de este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido a Copilot que añadiera documentación al código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera funciones de accesibilidad, incluidos controles para activar los modos de alto contraste y claro. Vamos a explorar los cambios de código y, si hace falta, a proporcionar comentarios a Copilot para mejorar su trabajo.

### Situación

Como se ha señalado en varias ocasiones, los fundamentos del diseño de software y de DevOps (desarrollo y operaciones) no cambian al incorporar inteligencia artificial generativa. Siempre queremos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisemos las sugerencias de GitHub Copilot para crear la documentación, la funcionalidad de juegos relacionados y las funciones de accesibilidad antes de activar la revisión para el resto del equipo.
## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad para que todo siga siendo seguro. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores del Protocolo de contexto de modelo (MCP).
## Revisar la documentación generada

Empecemos por explorar la primera solicitud de cambios generada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Para ello, utilizaremos la interfaz habitual de solicitudes de cambios de GitHub.com.

> [!NOTE]
> Al explorar la solicitud de cambios, puede que veas una advertencia que indica que el firewall ha bloqueado a GitHub Copilot. Es lo esperado, ya que el acceso de Copilot a los recursos externos está limitado de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes de cambios.
3. Abre la solicitud de cambios cuyo título sea parecido a **Add missing documentation** o algo más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud de cambios tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes hacer una pausa o reflexionar sobre lo que has aprendido hasta ahora.

4. Cuando la solicitud de cambios esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios de documentación de TypeScript (TSDoc) y otra documentación. Los cambios exactos pueden variar.

    Al revisar los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones personalizadas que has revisado al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)
9. Deberías ver que los flujos de trabajo se ponen en cola en la sección de comprobaciones de la solicitud de cambios. Si todo va bien, las comprobaciones del proyecto deberían superarse para la única aplicación Astro. Esto puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

Trabajar con Copilot en una solicitud de cambios no es una interacción unidireccional. También puedes mencionar a Copilot en comentarios, como harías con otros miembros del equipo, en la solicitud de cambios o en comentarios insertados en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto prescriptivo. Algunas ideas para pedirle a Copilot que actualice el código:

- Añadir encabezados de comentarios al principio de cada archivo de código con una breve descripción de su función.
- Añadir comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- Crear un archivo README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Utiliza una de las ideas anteriores u otra sugerencia para que Copilot añada la documentación que quieras al código base.
2. Selecciona **View Session** para ver el trabajo de Copilot. Observa cómo inicia una sesión nueva para realizar las actualizaciones.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud de cambios.

    ![Volver a la solicitud de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer una confirmación nueva en la solicitud de cambios.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la solicitud de cambios como lista para revisión y fusionarla en la rama principal.

![Marcar la solicitud de cambios como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Volvamos a la solicitud de cambios que Copilot ha generado para resolver la incidencia sobre los juegos relacionados en la página de detalles de un juego.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud de cambios cuyo título sea parecido a **Show related games on the game details page** o algo más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código que ha generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
7. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)
8. Deberías ver que los flujos de trabajo se ponen en cola en la sección de comprobaciones de la solicitud de cambios. Si todo va bien, las comprobaciones del proyecto deberían superarse para la única aplicación Astro. Esto puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente la funcionalidad de juegos relacionados. Ve al Codespace, abre el terminal y ejecuta los siguientes comandos (sustituye `<branch-name>` por el nombre de la rama que haya creado Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes trabajar de forma iterativa con el agente en la nube de Copilot para solicitar actualizaciones. Por ejemplo, podrías pedirle a Copilot que ajuste el número de juegos relacionados que se muestran, o que añada encabezados de comentarios y comentarios de documentación TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Como antes, puedes hacer estas solicitudes con un comentario nuevo en la pestaña **Conversation**. Copilot lo verá e iniciará una sesión nueva.

## Revisar las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta solicitud de cambios debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro solicitado desde el centro de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud de cambios cuyo título sea parecido a **Add high contrast mode to website** o algo más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud de cambios tendrá la etiqueta **[WIP]**. En ese caso, espera a que Copilot termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a lo siguiente:
   - los componentes de interfaz de usuario para alternar entre modos;
   - el uso del almacenamiento local para conservar las preferencias del usuario;
   - los cambios de hojas de estilo en cascada (CSS) para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas de aplicaciones de Internet enriquecidas y accesibles (ARIA), navegación con el teclado, etc.);
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes de aprobación, selecciona **Approve and run workflows**.

   ![Aprobación y ejecución de flujos de trabajo](../../../_images/shared-approve-workflows.png)
9. Deberías ver que los flujos de trabajo se ponen en cola en la sección de comprobaciones de la solicitud de cambios. Si todo va bien, las comprobaciones del proyecto deberían superarse para la única aplicación Astro. Esto puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve al Codespace, abre el terminal y ejecuta los siguientes comandos (sustituye `<branch-name>` por el nombre de la rama que haya creado Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador para comprobar que funcionan como se espera y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a orientar a Copilot para que implementara estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas algún problema o mejora de accesibilidad, puedes mencionar a **@copilot** en un comentario y solicitar una actualización, como hiciste con las solicitudes de cambios anteriores.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta más útil cuando puedes asignarle elementos reales del backlog y ocuparte de otras tareas. Para adquirir esa costumbre, crea algunas incidencias más en el repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario para expresar interés en apoyar un juego en la página de detalles.
- Implementar paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a las funciones auxiliares de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas incluidos en este repositorio** para observar después su efecto en los resultados del agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has visto cómo configuraba el entorno, planificaba y trabajaba de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para la accesibilidad, añadiendo controles para los modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado e iterado sobre las solicitudes de cambios del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración de MCP directamente en el IDE.
- 💻 **[Entorno de interfaz de línea de comandos (CLI)](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de control de calidad (QA) personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` con el agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de trabajo de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
