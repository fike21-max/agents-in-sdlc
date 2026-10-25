---
title: "Ejercicio 5: Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando activas el modo agente de GitHub Copilot, este trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como este trabajo se realiza en tiempo real en tu máquina, Copilot Chat en VS Code muestra las llamadas a herramientas y las ediciones de archivos a medida que se producen. Puedes revisar cada diff en el chat, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones de archivos propuestas;
- orientarás al agente durante la sesión para añadir un selector de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has utilizado en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras se realiza el trabajo, el equipo de diseño ha solicitado otra funcionalidad: un selector de modo claro para los usuarios a quienes les resulte demasiado oscuro el tema predeterminado. En lugar de iniciar una conversación desde cero, orientarás la sesión actual para ampliar el trabajo.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra todos los pasos del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat junto con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas. Aparecerán las lecturas de archivos, sus ediciones y los comandos de terminal que solicite ejecutar.
3. Cuando el agente proponga una edición, el diff aparecerá en el chat:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed**, en la parte superior del chat, para ver todos los archivos modificados por el agente durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta solicita permiso (por ejemplo, para ejecutar un comando de terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios realizados.

## Orientar al agente para que añada el modo claro

Ahora que está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un selector de modo claro. Como continúas en la misma sesión de chat, el agente conserva el contexto del trabajo anterior, incluidos los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de entrada de Copilot Chat, envía este prompt de seguimiento:

    ```plaintext
    ¡Buen trabajo! Ahora añade un control de alternancia del modo claro junto al de alto contraste. El modo claro debe seguir el mismo patrón: conservarse mediante el almacenamiento local y estar disponible en la misma zona de la interfaz. El usuario debe poder activar el modo de alto contraste y el modo claro de forma independiente.
    ```

2. El agente continuará desde donde lo dejó: explorará los cambios existentes y propondrá las adiciones necesarias para el modo claro.
3. Revisa cada nueva edición a medida que aparezca, igual que hiciste con el modo de alto contraste. Acepta o revierte cada diff con **Keep** o **Undo**.
4. Si el agente toma una dirección que no te convence —por ejemplo, si intenta reorganizar la interfaz de los selectores—, puedes detenerlo con el botón **Stop** junto al campo de entrada y enviarle un mensaje para aclarar lo que necesitas.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Utiliza prompts de seguimiento para pedir ajustes («simplifica el CSS: utiliza variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos selectores»).

## Revisar los cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en los componentes Astro, los estilos y las pruebas relacionadas.
3. Abre varios archivos modificados y revisa los diffs. Comprueba que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y almacenamiento persistente mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición de archivo, la has aceptado o rechazado y has enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has aprendido a:

- supervisar en tiempo real el trabajo de un agente;
- aceptar y rechazar ediciones de archivos concretas;
- orientar una sesión en curso mediante prompts de seguimiento.

En la [siguiente lección][next-lesson], publicarás tu trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que el agente personalizado local haya generado durante el laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
