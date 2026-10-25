---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Probablemente haya muy pocas organizaciones que no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan mucho tiempo en el backlog porque nunca ha habido tiempo para implementarlas. El agente de Copilot en la nube está diseñado para realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una pull request en borrador para que la revise un desarrollador. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en asuntos de mayor envergadura.

Con el agente de Copilot en la nube explorarás:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realizan de forma segura.
- por qué es importante delimitar claramente el alcance de las incidencias.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver algunos problemas de deuda técnica. Los contratistas que desarrollaron la primera versión del sitio dejaron la documentación en un estado poco satisfactorio: comprobarás que falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. Quiere mostrar juegos relacionados —otros títulos de la misma categoría— en la página de detalles de cada juego para que la gente pueda seguir explorando. Por ahora no necesita una implementación pulida; solo quiere algo que le permita probar la experiencia de usuario para su aceptación. En este momento, la tarea está bloqueada, pero hay otras incidencias de mayor prioridad.

Estas tareas suelen quedar relegadas y son buenas candidatas para asignarlas al agente de Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas; después, podrás revisar su trabajo y confirmar que todo está como debería.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano, de forma parecida a un desarrollador. Al igual que con el trabajo de otra persona, puedes asignarle tareas de varias formas, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea una pull request en borrador para realizar el seguimiento del trabajo, configura un entorno y empieza a trabajar. Puedes examinar la sesión de Copilot mientras está en curso o una vez haya terminado. Cuando la solución esté lista para que la revises, Copilot te mencionará en la pull request.

## Por qué es importante delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar. Incluso las operaciones que parecen sencillas tienen una cierta complejidad cuando se examinan en detalle. Por eso, [delimita cuidadosamente las tareas que asignas al agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. La llegada de la IA generativa no cambia los fundamentos del desarrollo de software.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has aprendido cómo guían a Copilot las instrucciones personalizadas. Si ya has completado el ejercicio práctico de instrucciones personalizadas, habrás visto cómo funciona `.github/copilot-instructions.md`; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente de Copilot en la nube, examina en modo de solo lectura los archivos de instrucciones que ya hay en este repositorio, para observar más adelante su efecto.

Abre los siguientes archivos en la interfaz web de GitHub para tu repositorio o en un codespace si ya tienes uno en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas sobre las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita estas instrucciones a los archivos `**/*.test.ts`.

Cuando en la siguiente sección asignes al agente en la nube la incidencia `Code lacks documentation`, observa en la pull request resultante los comentarios de documentación TSDoc, las convenciones de TypeScript y las cabeceras de comentario. Estos elementos proceden de los archivos de instrucciones que acabas de revisar. Volveremos a señalarlos al revisar las PR en un ejercicio posterior.

## Configura el entorno de desarrollo del agente de Copilot en la nube

El desarrollo de código suele requerir un entorno específico y la ejecución de algunos scripts de configuración para dejarlo todo listo, independientemente de quién realice el trabajo. Lo mismo se aplica a las tareas que asignas a Copilot, que trabaja de forma similar a un ingeniero de software (SWE).

El agente en la nube utiliza [GitHub Actions][github-actions] para crear su entorno de trabajo. Puedes personalizarlo mediante un [flujo de configuración especial][setup-workflow], definido en el archivo `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece a trabajar. Así puede acceder a las herramientas de desarrollo y dependencias necesarias. Este flujo ya está configurado para facilitar el desarrollo del laboratorio y el aprendizaje. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Define los permisos mínimos necesarios para estos pasos. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso "contents: read". Si no clonas el repositorio en los pasos de configuración, Copilot lo hará automáticamente cuando estos terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de frontend/aplicación: Node.js (toda la aplicación utiliza ahora Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra la base de datos SQLite local y añade datos iniciales para que haya datos en las compilaciones y las pruebas.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro archivo de flujo de trabajo de GitHub, pero tiene algunos detalles importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. Este trabajo se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la pull request.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Es útil para comprobar que el flujo se ejecuta correctamente sin tener que esperar a que Copilot lo inicie.

## Añade documentación

Aunque todo el mundo entiende la importancia de la documentación, la mayoría de los proyectos tienen información desactualizada o carecen de documentación. Esta deuda técnica suele quedar sin resolver, lo que reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot destaca en la creación de documentación, por lo que esta es una tarea perfecta para asignar al agente en la nube. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás a revisar el resultado.

1. En una pestaña nueva del navegador, ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear una incidencia nueva.
5. En el campo **Title**, escribe `Code lacks documentation`.
6. En el campo **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En la parte derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la pull request (PR) recién generada. Su título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la PR. Copilot está creando su entorno (ejecutando el flujo de trabajo que se ha explicado antes), analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a realizar.
14. Desplázate por la cronología de la pull request. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la página para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en curso y seguir cómo Copilot resuelve el problema. Esto incluye explorar el código y comprender su estado, hacer pausas para pensar y decidir el plan adecuado y, por último, crear código.

El proceso puede tardar varios minutos. Uno de los principales objetivos del agente de Copilot en la nube es permitirle trabajar de forma asíncrona y dejarte tiempo para otras tareas. Aprovecharemos esa función asignándole otra tarea y, a continuación, nos pondremos a escribir código para añadir funciones a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Como ya se ha indicado, una de las principales ventajas del agente de Copilot en la nube es que permite dividir el trabajo para que tú puedas ocuparte de unas tareas mientras el agente se ocupa de otras. Puede que añadir una sección de juegos relacionados para el equipo de diseño no lleve mucho tiempo, pero aun así podrías dedicar ese tiempo a otras tareas. ¡Asignemos el trabajo al agente de Copilot en la nube!

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. En el campo **Title**, escribe `Show related games on the game details page`.
6. En el campo **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En la parte derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

    ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás una serie de 👀 en el primer comentario de la incidencia. Indica que Copilot se ha puesto manos a la obra.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando con diligencia en tu segunda solicitud. El agente de Copilot en la nube funciona de forma parecida a un ingeniero de software (SWE), así que no es necesario supervisarlo continuamente. Podrás revisar el resultado cuando termine. Mientras tanto, pasemos a crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias a Copilot para que trabaje en ellas de forma asíncrona. Puedes usarlo para abordar deuda técnica, crear funcionalidades nuevas o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realizan de forma segura.
- por qué es importante delimitar claramente el alcance de las incidencias.
- cómo asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, podemos centrarnos en crear y utilizar agentes personalizados. [El agente de Copilot en la nube también puede usar servidores MCP][cloud-agent-mcp] y dispone de instrucciones personalizadas, que ya hemos explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente de Copilot en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
