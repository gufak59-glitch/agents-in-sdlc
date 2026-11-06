---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, autoriza Agent Merge para combinar la PR."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las instrucciones actualizadas, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Tú combinaste la solicitud de incorporación de cambios (PR) de las valoraciones con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionarás toda la PR de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de revisarla y confirmarás que se ha combinado.

## Escenario

A lo largo del flujo de filtrado has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar las tareas restantes de la PR, pero mantener bajo control del desarrollador la autorización para combinarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al activarlo, la sesión de la aplicación lee la PR y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y reorganiza la base cuando hace falta. Después, la combina en cuanto GitHub lo permite. El proceso se ejecuta en segundo plano, continúa aunque reinicies la aplicación y se desactiva cuando se combina la PR.

Hasta ahora seleccionabas **Merge pull request** manualmente. Agent Merge puede encargarse de esa tarea, pero todavía necesita tu autorización explícita para editar código y combinar. Revisa las acciones permitidas y el trabajo antes de concederle permiso para combinar.

## Utiliza Agent Merge para gestionar la PR

Cuando hayas creado y revisado todo el código, deja que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable situado junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable por sí sola no inicia el proceso.

Agent Merge se pondrá en marcha y:

- creará la solicitud de incorporación de cambios con un título y una descripción.
- si iniciaste la sesión desde una incidencia, la mencionará en el cuerpo de la descripción.
- reorganizará la base o resolverá posibles conflictos con la rama de destino.
- supervisará CI para comprobar que se superen todas las verificaciones.
- supervisará la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y realizará los cambios necesarios para resolverlos.
- podrá combinar automáticamente la PR cuando se haya completado todo correctamente.

Permitamos que Agent Merge combine también la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** esté marcado.
8. Pide explícitamente al agente que combine la PR:

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. Supervisa el proceso y resuelve los bloqueos que indique. Antes de continuar, confirma que el estado de la PR es **Merged**; tener habilitado el permiso de combinación o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generación de código, pruebas y validación, y ahora también el proceso de la PR. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de combinación.
- inspeccionaste toda la PR de la funcionalidad y las pruebas de verificación.
- autorizaste Agent Merge solo después de revisarla y confirmaste que se había combinado.

A continuación, [utilizarás un lienzo existente y crearás un lienzo de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Administrar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
