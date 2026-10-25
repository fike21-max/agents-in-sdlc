---
title: "Lección 10: Repaso y siguientes pasos"
description: "Repaso del flujo de trabajo de Copilot CLI, dos solicitudes de incorporación de cambios, personalizaciones reutilizables y recursos adicionales."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI durante un flujo de trabajo continuo con Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado creada de antemano.
- añadido valoraciones con estrellas, revisado el resultado en el navegador mediante el reenvío de puertos y fusionado manualmente tu primera solicitud de incorporación de cambios (PR).
- partido de la incidencia de filtrado, definido el enfoque en el modo Plan, implementado la funcionalidad en el modo Autopilot y revisado el resultado en el modo Interactive.
- guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y la has utilizado para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y utilizado para explorar el filtrado en un navegador real.
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas realizadas en el navegador.
- revisado el cambio completo del filtrado y autorizado Agent Merge para la PR de filtrado.
- explorado los comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Qué has entregado

El taller incluye dos hitos de PR, cada uno en su propia rama basada en `main` actualizada:

1. **Valoraciones con estrellas:** mostrar el valor existente de `starRating` y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has utilizado la misma conversación y la misma rama. Hemos agrupado ese trabajo en una sola PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, revisándolo tú mismo en el navegador y explorándolo con Copilot a través de MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos, y ha generado un informe con el nuevo formato. QA ha reunido esos resultados con una revisión de los requisitos y la cobertura de las pruebas antes de la PR.

Las pruebas que añadas deben cubrir carencias reales; puede ser correcto que una ejecución de QA no requiera pruebas nuevas. Revisa el código y las evidencias antes de autorizar la fusión, y actualiza las evidencias afectadas después de cualquier cambio.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller, has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas según las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen funciones especializadas y los servidores MCP conectan herramientas externas. Revisa los cambios reales y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Utiliza **Plan** para pensar el enfoque antes de implementar, **Interactive** para mantenerte al tanto de cambios concretos y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para modificaciones rutinarias y otro más potente para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir claramente *qué* quieres crear, *por qué* y *cómo* cambia de forma significativa el resultado.

## Más ideas para explorar

Has aprendido el flujo de trabajo principal. Aquí tienes algunas funciones más de CLI que merece la pena probar:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otro punto de vista.
- `/fleet` para coordinar subtareas independientes en paralelo.
- `/worktree` para aislar una tarea aparte.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala con código de producción, con proyectos personales o con esa pequeña aplicación que llevas años imaginando y nunca has llegado a crear. Comparte lo que has aprendido con tu equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Administrar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
