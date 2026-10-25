---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos empezado por añadir una pequeña funcionalidad al proyecto. Sin embargo, los cambios más grandes requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para trabajar con el flujo de una organización y ayudarnos a crear lo adecuado de la forma correcta. Esta es la primera de varias lecciones en las que seguirás un proceso de desarrollo habitual con agentes: empezarás con una incidencia para generar una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, finalmente, la combinarás en el proyecto.

> [!NOTE]
> Continuarás el flujo de trabajo de la funcionalidad en la misma conversación y rama. Lo habitual sería usar ramas o PR distintas para los diferentes tipos de archivos, pero en este taller simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot nueva a partir de una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la nueva funcionalidad con el modo Autopilot.
- revisarás el código.
- validarás manualmente la funcionalidad en un navegador reenviado.

A medida que avances con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás la validación MCP, crearás un agente QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay detalles que deben acordarse antes de programar, como la combinación de categorías. Usarás el modo Plan para resolver esas decisiones y, a continuación, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia los fundamentos. ¡Si acaso, los hace aún más importantes! La mayoría de los desarrolladores sigue un flujo parecido a este:

1. Empezar con una incidencia registrada que describa el trabajo.
2. Crear un plan de lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una pull request (PR).
7. Cuando se haya revisado el código y se hayan superado las comprobaciones de integración continua, combinarlo.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos se basa en una variante del flujo anterior.

Si sigues este enfoque habitual, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito a mano.

## Modos de conversación

El **modo de conversación** controla cuánta autonomía tiene el agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para recorrer los modos:

- **Interactive**: tú y el agente trabajáis juntos. El agente sugiere cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, a continuación, usa Autopilot para implementarlo.

## Empieza con la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y asegúrate de que el repositorio y el terminal estén preparados.

1. Vuelve a tu Codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha combinado la PR de valoraciones por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que ahora el prompt empieza por `#7` (o un número similar). El carácter `#` permite incluir en el contexto una incidencia o pull request (PR) de GitHub.

## Planifica la funcionalidad de filtrado

La planificación te permite definir cómo implementar una funcionalidad o realizar una tarea antes de asignársela a Copilot. Para cualquier tarea compleja, conviene dedicar algo de tiempo a planificar. Cambiemos al modo Plan y pidamos a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador bajo el prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, introduce este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot se pone a elaborar el plan. Primero explorará el proyecto y luego determinará el mejor enfoque.

3. Durante el proceso, Copilot puede preguntarte cómo debería funcionar el filtrado. Responde según tus preferencias. ¡No hay respuestas incorrectas!
4. Cuando el plan esté completo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte del plan que no coincida con tus decisiones.

## Aprueba Autopilot

Una vez escrito y revisado el plan, toca implementarlo. Dejemos que Copilot se encargue con el modo Autopilot.

Autopilot permite que Copilot itere en la tarea hasta que considere que está completa.

1. Selecciona **Accept plan and build on autopilot (recommended)**, o una opción con un nombre similar en la versión que tengas instalada.
2. Confirma que el indicador bajo el prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera el código y ejecuta las pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente; revísalo antes. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisa y verifica la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, como cualquier otro cambio. Revisemos el código y ejecutemos el sitio para asegurarnos de que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones de la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva los errores.

## Explora la nueva funcionalidad

De acuerdo, el código parece correcto, pero ¿funciona? Iniciemos la aplicación como antes y abramos el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, por editorial y las combinaciones acordadas en el plan.
4. Confirma que el reinicio y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una conversación nueva de Copilot a partir de una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con Autopilot.
- has revisado el código.
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizaremos en cómo se genera el código y nos aseguraremos de que siga las prácticas documentadas [mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
