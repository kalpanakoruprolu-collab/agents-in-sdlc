---
title: "Lección 8 - Crear y fusionar la solicitud de incorporación de cambios (PR) de la característica"
description: "Revisa el filtrado, las instrucciones, la actualización de la skill, el perfil de QA y las pruebas; después crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones y de la skill, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios (PR). Fusionaste manualmente la PR de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la característica, las actualizaciones de instrucciones y de la skill, y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de fusión.
- inspeccionarás el cambio completo de la característica y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de la revisión y confirmarás que se ha fusionado la PR.

## Escenario

Durante el trabajo de filtrado has usado Copilot para planificar, implementar y verificar una característica. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR sin quitar al desarrollador el control para autorizar la fusión.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Al activarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y realiza un rebase cuando hace falta. Después activa la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora seleccionabas **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usar Agent Merge para gestionar la PR

Con todo el código creado, revísalo, crea la PR y deja que Agent Merge gestione el resto.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa agent merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR y, cuando todo se supere, la fusionará.
7. Confirma que aparece un mensaje de Copilot parecido a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y próximos pasos

Has automatizado varias partes del desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de solicitud de incorporación de cambios. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de fusión.
- inspeccionaste el cambio completo de la característica y las pruebas de verificación.
- creaste la PR de filtrado.
- activaste Agent Merge después de la revisión y confirmaste que se fusionó la PR.

A continuación, [explorarás otros comandos slash útiles de Copilot CLI][next-lesson] para el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Administrar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
