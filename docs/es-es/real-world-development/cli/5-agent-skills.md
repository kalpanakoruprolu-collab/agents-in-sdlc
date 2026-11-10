---
title: "Lección 5 - Personalizar y usar una skill quality-checks"
description: "Explora la skill quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código no es lo único necesario. Ya hemos validado manualmente que funciona y hemos usado archivos de instrucciones para asegurar que sigue nuestros estándares. ¿Y las pruebas, el lint y el resto de la integración continua (CI)?

Para este tipo de tareas, las **skills de agente** son la mejor opción. Ayudan a Copilot a entender cómo ejecutar correctamente estas operaciones.

En esta lección:

- explorarás la skill existente `quality-checks`.
- personalizarás el formato de sus resultados.
- volverás a cargar la skill y la ejecutarás.

## Escenario

Tailspin Toys usa la skill `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las skills de agente agrupan instrucciones reutilizables, scripts ejecutables y recursos complementarios que el agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la skill y un archivo Markdown llamado `SKILL.md`. El Markdown contiene un frontmatter con el nombre y la descripción, un resumen de sus funciones y orientación sobre cuándo debe invocarse. La carpeta también puede incluir scripts y otros recursos.

> [!NOTE]
> Una skill no necesita archivos ni carpetas adicionales. La skill `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, ya que usa los comandos existentes del proyecto.

Las skills pueden residir en la carpeta `.github/skills` del proyecto para compartirse y reutilizarse en el equipo, o en la carpeta personal `~/.copilot/skills`.

## Explorar la skill

Explora la skill `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace y abre `.github/skills/quality-checks/SKILL.md` en el editor de Codespaces.
2. Lee `name` y `description` al principio. La descripción ayuda a Copilot a decidir cuándo invocar la skill.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y el lint.
4. Comprueba que todavía no existe una sección **Results output formatting**.

## Ejecutar la skill antes de modificarla

Las skills se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Consulta el informe final.

## Personalizar el informe

Queremos un informe mejor que indique qué se ejecutó, si tuvo éxito y qué comunicaron las herramientas. Actualiza la skill para generar ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la skill actualizada

Comprueba el cambio. Copilot CLI puede volver a cargar las skills editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Consulta el informe final y compáralo con el primero.

## Resumen y próximos pasos

Has personalizado y usado una skill existente. En esta lección:

- exploraste la skill `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos.
- personalizaste el formato de los resultados.
- volviste a cargar y ejecutaste la skill.

Ese cambio acompañará al filtrado en la PR de la característica. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor Playwright MCP][next-lesson].

## Más ejemplos de skills

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación integrado: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
