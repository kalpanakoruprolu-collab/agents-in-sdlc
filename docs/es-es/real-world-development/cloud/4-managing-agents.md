---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube está diseñado para trabajar de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, como [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de cambios abiertas. También puedes explorar las operaciones que han realizado e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- orientarás una sesión en curso para solicitar funcionalidad adicional.

## Situación

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería conveniente añadir un modo claro. Como ya se estaba trabajando en actualizar el estilo del sitio y añadir la funcionalidad de alternancia, parecía lógico incluir también esta función. Quieres orientar el trabajo del agente para asegurarte de que añada el modo claro además del modo de alto contraste.

## Revisar las tareas del agente en la nube de Copilot

Veamos el estado actual de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación del código base;
    - generar interfaces de programación de aplicaciones (API) para modificar juegos;
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las tareas que ha realizado Copilot, que pueden incluir:
    - obtener el código del repositorio;
    - crear el entorno de trabajo de Copilot;
    - configurar servidores del Protocolo de contexto de modelo (MCP);
    - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos que aparezcan dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitud de cambios que aparece a la derecha. Desde allí puedes ver la solicitud de cambios y los archivos modificados para supervisar el trabajo.

## Dirigir el agente en la nube

Ahora que has visto las tareas activas, pidamos a Copilot que incluya el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con la incorporación del modo de alto contraste. El título exacto dependerá del nombre que Copilot haya elegido y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Sabrás que esto ha ocurrido cuando empiecen a aparecer mensajes similares a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta instrucción:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la instrucción.
5. Observa cómo Copilot la confirma y la incorpora al flujo de trabajo.

## Dejar que Copilot trabaje

Como antes, Copilot se ocupará de la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso que estaba realizando al recibir el mensaje.

Como antes, el proceso tardará varios minutos. Es un buen momento para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro donde se supervisan y dirigen las tareas del agente en la nube de GitHub Copilot. Desde este centro de control puedes seguir todas las tareas activas y completadas, revisar el trabajo que se está realizando e incluso redirigir las tareas en curso para ajustar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- explorar el centro de control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
