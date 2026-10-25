---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es hora de revisarlos en conjunto y abrir una pull request. Tú combinaste la pull request (PR) de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> En condiciones normales, dividiríamos la funcionalidad, las actualizaciones de instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una sola conversación y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la PR del filtrado.
- habilitarás Agent Merge solo después de revisar el trabajo y confirmar que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener el control de la autorización para combinar los cambios.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para combinar una pull request. Al habilitarlo, Copilot resuelve los problemas que bloquean la PR —corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a aplicar la base cuando es necesario— y, después, habilita la combinación automática de GitHub cuando la configuración del repositorio lo permite.

Hasta ahora, seleccionabas **Merge pull request** manualmente. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y confirma que esté listo antes de habilitar Agent Merge.

## Usa Agent Merge para gestionar la PR

Ya está todo el código. Revisémoslo, creemos la PR y dejemos que Agent Merge gestione el resto del proceso.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo se haya superado correctamente, la combinará.
7. Confirma que aparece un mensaje de Copilot parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni la falta de permisos. Si no puede continuar, lee el motivo indicado y completa manualmente la combinación revisada cuando la configuración del repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación del código, las pruebas y la validación, y ahora el proceso de la pull request. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has inspeccionado el cambio completo de la funcionalidad y las pruebas de verificación.
- has creado la PR del filtrado.
- has habilitado Agent Merge solo después de revisar el trabajo y confirmar que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente en la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
