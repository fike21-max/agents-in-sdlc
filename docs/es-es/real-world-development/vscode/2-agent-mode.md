---
title: "Ejercicio 2: Añadir funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. El flujo de trabajo habitual de un desarrollador consiste en localizar todos los archivos necesarios, realizar los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, volver a actualizarlo... Y así sucesivamente.

Aquí es donde entra en juego el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo utilizar el modo agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres son parecidos, el modo agente y el agente en la nube están diseñados para experiencias distintas. El modo agente realiza tareas en el IDE y permite recibir comentarios e interactuar rápidamente. El agente en la nube está pensado para trabajar como un programador del equipo de forma asíncrona, interactuar contigo mediante incidencias y solicitudes de incorporación de cambios y colaborar como un compañero más.

En este ejercicio aprenderás cómo:

- el modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar los cambios;
- GitHub Copilot en modo agente puede implementar nuevas funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas que genera el modo agente de Copilot antes de fusionarlos en el código base.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y por editorial. En el ejercicio anterior ya añadiste una función auxiliar para editoriales; ahora completarás el resto del trabajo en la capa de datos, la interfaz de usuario y las pruebas con el modo agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para entender su funcionalidad actual.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación de Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde el frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz ejecutado en el cliente. Los asistentes de acceso a datos reutilizables están en `src/lib/`, y la interactividad se implementa con un pequeño elemento `<script>` de Astro con alcance limitado y las API estándar del DOM.

### Iniciar el sitio web

Para simplificar la ejecución del sitio, se ha preparado una tarea `npm run dev` que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en GitHub Codespace siguiendo estos pasos:

1. Vuelve al codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre una ventana de terminal nueva en el codespace pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio web manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor de Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> Si utilizas un codespace, al seleccionar el enlace a la URL de localhost desde el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio esté en funcionamiento, explora sus funciones. Entre las principales se incluyen:

- **Página principal:** muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego:** al seleccionar un juego, accederás a una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el trabajo pendiente con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; se indicarán en cada paso cuando corresponda.

La implementación inicial del sitio web funciona, pero queremos ampliarla con nuevas capacidades. Empecemos por revisar el trabajo pendiente. Al configurar el proyecto desde la plantilla, se creó automáticamente una lista de incidencias; pide a GitHub Copilot que te las muestre.

1. En la vista Chat, selecciona **Agent** en la lista desplegable de agentes. El agente **Agent** planifica e implementa cambios en distintos archivos de forma autónoma, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan preferencia por un modelo frente a otro. Durante la creación del taller utilizamos Claude Sonnet 4.5, por lo que lo hemos incluido en las instrucciones. Queremos que las sugerencias de código sean relativamente coherentes y que la experiencia sea buena. Sin embargo, como los LLM son probabilísticos, es posible que las sugerencias que recibas difieran de las que se muestran en el taller. Es completamente normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot puede utilizar un comando MCP distinto, pero debería poder completar la tarea.

3. Pide a Copilot que muestre las incidencias del trabajo pendiente con este prompt:

   ```plaintext
   Muéstrame el trabajo pendiente de mi repositorio de GitHub. Ayúdame a priorizar los elementos según cuáles serán más útiles para el usuario.
   ```
4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Fíjate en que Copilot también ha priorizado los elementos según los que considera más útiles para el usuario.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere el código, conviene revisar el archivo de instrucciones que utilizarás para proporcionar contexto a Copilot. Aprovecharás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar la incorporación de funcionalidad al sitio web.

1. En el codespace, ve a `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidad. Incluyen:
   - Una descripción general de la arquitectura.
   - Principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - Enlaces a archivos de instrucciones específicos para distintos tipos de archivos, incluidos:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar contenido, dividir conjuntos de instrucciones complejos en otros más sencillos y manejables, y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, habrá que realizar al menos tres cambios distintos en la aplicación:

- Añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- Añadir o actualizar las pruebas del comportamiento de filtrado.
- Actualizar la página del listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de fusionar los cambios en el código base. ¡El modo agente de Copilot puede realizar estas tareas! Añadamos la funcionalidad.

1. Puedes continuar en la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en la lista desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia correspondiente del trabajo pendiente, utilizando este prompt:

   ```plaintext
   Actualiza el sitio para que permita filtrar por editorial y categoría según los requisitos de la incidencia correspondiente del trabajo pendiente de GitHub. Ya existe una función auxiliar para editoriales del ejercicio anterior, así que consérvala y perfecciónala según sea necesario mientras completas el resto de la capa de datos, la interfaz y las pruebas. Asegúrate de que todas las pruebas se superen antes de terminar. El servidor ya está en ejecución, así que no es necesario iniciarlo.
   ```

6. Observa cómo Copilot empieza explorando el proyecto y localizando los archivos relacionados con la funcionalidad deseada. Debería encontrar tanto los asistentes de la capa de datos como la interfaz y las pruebas. A continuación, modificará los archivos y ejecutará las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Observarás que Copilot realiza varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, el proceso puede tardar unos minutos. Mientras tanto, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, aparecerá una opción **Keep** o **Undo** para cada cambio; no hace falta que selecciones ninguna mientras siga trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo en el panel Copilot Chat que pide confirmar si se pueden ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada. Copilot alterna entre la generación de código y las pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Chat con el modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o por cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio web en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a utilizar el modo agente de GitHub Copilot para añadir capacidades al sitio web de Tailspin Toys. Has visto cómo:

- el modo agente de GitHub Copilot puede implementar nuevas funcionalidades en la interfaz y en la capa de datos;
- el modo agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar los cambios;
- revisar los cambios y las pruebas que genera el modo agente de Copilot antes de fusionarlos en el código base.

Ahora vamos a [probar la funcionalidad con el servidor MCP de Playwright][next-lesson] y a crear una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que aumente la lista de juegos, será necesario habilitar la paginación. Utiliza lo aprendido en este ejercicio y pide a Copilot que actualice el sitio para implementar la paginación. Al preparar el código, ten en cuenta lo siguiente:

- sigue las prácticas recomendadas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación: si quieres que el usuario pueda elegir el tamaño de página o prefieres fijarlo;
- al crear el prompt, proporciona a Copilot las directrices necesarias para que la implementación se ajuste a tus preferencias;
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Este es el flujo de trabajo normal al utilizar Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
