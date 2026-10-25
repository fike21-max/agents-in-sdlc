---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, pediste al agente de Copilot en la nube que realizara tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube está diseñado para trabajar de forma autónoma y asíncrona, sigue siendo importante supervisar esas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agentes que tienen pull requests (PR) abiertas. Puedes explorar las operaciones realizadas e incluso orientar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- orientarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo la funcionalidad de cambio de modo, parecía lógico incluir esta mejora. Quieres orientar el trabajo del agente para asegurarte de que añada tanto el modo claro como el de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Veamos el estado actual de todas las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel de la izquierda. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos de la lista variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Fíjate también en el panel de pull requests (PR) de la derecha. Desde ahí puedes consultar la PR y los archivos modificados para supervisar el trabajo.

## Orienta el trabajo del agente en la nube

Ya has visto las tareas activas. Ahora pidamos a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con la incorporación de un modo de alto contraste. El título exacto variará según el nombre que utilice Copilot y el estado actual del trabajo.

    ![Sesión de accesibilidad en Mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha completado la configuración y ha empezado a trabajar. Lo sabrás cuando empieces a ver mensajes similares a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de una tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo reconoce y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, ¡Copilot empezará a trabajar en la tarea actualizada! Incorporará la nueva solicitud al flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

Como el proceso puede tardar varios minutos, aprovecha para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que se supervisan y orientan las tareas del agente de Copilot en la nube. Puedes hacer un seguimiento de todas las tareas activas y completadas, revisar el trabajo en curso e incluso redirigir las tareas activas para cambiar su alcance o proporcionar nuevas indicaciones.

Has explorado estos conceptos:

- has explorado Mission control y la página de agentes para supervisar las tareas del agente en la nube.
- has redirigido una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot completa las funcionalidades de accesibilidad, pasemos a [iterar en las pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar en el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
