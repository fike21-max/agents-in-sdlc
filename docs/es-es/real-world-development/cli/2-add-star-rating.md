---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juego, revísalo en un navegador reenviado y combínalo en tu primera pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y probado una conversación, es hora de hacer el primer cambio en el proyecto. Será algo pequeño: los datos de los juegos ya incluyen una valoración por estrellas, pero las tarjetas de la página de inicio aún no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera pull request.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de funcionalidad.
- pedirás al agente que haga un pequeño cambio en el proyecto.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación para comprobar el cambio en un navegador reenviado.
- abrirás y combinarás tu primera pull request.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles del juego. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre la valoración existente en cada tarjeta: un cambio pequeño y autónomo, perfecto para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio en el que trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama que estén activos en el terminal, en lugar de crear un árbol de trabajo dedicado. Las conversaciones guardadas te permiten volver al mismo diálogo más tarde, mientras que los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente; la actividad del agente con herramientas mientras explora y edita archivos; y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Inicia una conversación nueva para empezar a implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si aún no está abierto, inicia Copilot con el siguiente comando:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de iniciar una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa este prompt para solicitar el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explora el proyecto, localiza los archivos que se usan para mostrar los detalles de los juegos y crea el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisa las diferencias

Todos los cambios generados por IA deben revisarse antes de combinarlos, aunque sean pequeños. Vamos a explorar los cambios directamente en Copilot CLI.

1. Escribe `/diff` e inspecciona cada archivo modificado.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que haya ejecutado Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la pantalla de diferencias.

> [!NOTE]
> Como Copilot, al igual que todas las herramientas de IA generativa, es probabilístico y no determinista, el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación concreta.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pidamos a Copilot que inicie el sitio para que podamos examinar la interfaz de usuario (UI) actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de juego muestran sus valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y combina tu primera pull request

¡Ya has creado la funcionalidad! Ahora toca abrir una pull request (PR) para combinar el nuevo código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI sube la confirmación existente al crear la PR y muestra su URL.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación de Copilot centrada en una rama de funcionalidad.
- has indicado al agente que hiciera un pequeño cambio en las tarjetas de juego.
- has revisado el cambio con `/diff`.
- has ejecutado la aplicación para comprobar la valoración en un navegador reenviado.
- has abierto y combinado tu primera pull request.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para desarrollar una funcionalidad mayor.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
