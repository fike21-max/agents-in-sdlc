---
title: "Ejercicio 3: Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear la funcionalidad de filtrado con el modo agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funciona en el navegador. En lugar de recorrer la aplicación manualmente, conectarás el **servidor MCP de Playwright** y dejarás que Copilot controle un navegador real para probar la funcionalidad. Después, publicarás la rama y abrirás una PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- añadirás el servidor MCP de Playwright a tu espacio de trabajo;
- pedirás a Copilot que lo utilice para probar manualmente la funcionalidad de filtrado en un navegador;
- publicarás la rama y abrirás una solicitud de incorporación de cambios para el filtrado.

## ¿Qué es Model Context Protocol (MCP)?

El modo agente es mucho más potente cuando puede acceder a recursos fuera del editor. Copilot lo consigue mediante Model Context Protocol (MCP), un estándar que permite al agente comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo agente y su interacción con el contexto, el LLM y las herramientas, incluidas las que aportan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite a los agentes de IA comunicarse con herramientas y servicios externos. Gracias a MCP, los agentes de IA pueden comunicarse en tiempo real con herramientas y servicios externos. Así pueden acceder a información actualizada (mediante recursos) y realizar acciones en tu nombre (mediante herramientas).

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. El servidor MCP administra la comunicación entre el agente de IA y las herramientas externas, como las API existentes o las herramientas locales (por ejemplo, paquetes de NPM). Cada servidor MCP ofrece un conjunto distinto de herramientas y recursos al agente de IA.

Algunos servidores MCP conocidos son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server):** proporciona acceso a un conjunto de API para administrar repositorios de GitHub. Permite al agente de IA realizar acciones como crear repositorios, actualizar los existentes y administrar incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp):** ofrece capacidades de automatización del navegador mediante Playwright. Permite al agente de IA realizar acciones como visitar páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP que permiten acceder a distintas herramientas y recursos. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar el descubrimiento de servidores y las contribuciones al ecosistema.

> [!CAUTION]
> En materia de seguridad, trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa detenidamente su código fuente, verifica quién lo publica y evalúa las implicaciones de seguridad. Utiliza solo servidores MCP de confianza y ten cuidado al conceder acceso a recursos u operaciones confidenciales.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Deberías ver un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La entrada `github` viene incluida en la plantilla del proyecto. Por eso Copilot pudo leer las incidencias pendientes en el ejercicio anterior. Utiliza el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no tienes que instalar nada localmente. VS Code se conecta a través de HTTP y te autenticas con GitHub la primera vez que Copilot utiliza una de sus herramientas.

## Añadir el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar, justo lo que necesitas para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es un servidor **local**. VS Code lo inicia en tu máquina ejecutando `npx @playwright/mcp@latest`. La marca `--headless` indica a Playwright que ejecute el navegador sin mostrar una ventana, algo necesario en un codespace, donde no hay un escritorio en el que hacerlo.

> [!NOTE]
> El proyecto Tailspin Toys ya utiliza Playwright para las pruebas de extremo a extremo, así que normalmente el navegador necesario ya está instalado. Si Copilot indica que falta un navegador, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

## Iniciar el servidor y confiar en él

VS Code inicia los servidores MCP **a petición**: la primera vez que Copilot necesita las herramientas de un servidor, VS Code lo inicia y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pide que confirmes que confías en el servidor, revisa la configuración y confirma que confías en él para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si lo omites, VS Code lo iniciará y te pedirá que confirmes que confías en él la primera vez que Copilot necesite sus herramientas en el siguiente paso.

## Probar la funcionalidad de filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real que puede controlar. En lugar de recorrer la aplicación para comprobar tu trabajo, el agente puede abrir una página, navegar, aplicar filtros y mostrarte los resultados. Después, resumirá lo que ha observado. Es la forma más rápida de confirmar que una funcionalidad se comporta como esperas sin salir de la conversación.

Internamente, el servidor MCP de Playwright utiliza el [árbol de accesibilidad de la página][playwright-mcp-server] en lugar de capturas de pantalla. De este modo, el agente razona sobre elementos estructurados y etiquetados (botones, enlaces y elementos de listas), igual que lo hace la tecnología de asistencia. Así, una comprobación funcional rápida también sirve como verificación básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe la funcionalidad de filtrado que acabas de crear:

```text
Utiliza el servidor MCP de Playwright para abrir un navegador en la aplicación en ejecución en http://localhost:4321 y verificar la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos y anota cuántos juegos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra juegos de esa categoría.
3. Bórralo y, después, aplica un filtro de editorial y confirma que la lista solo muestra juegos de esa editorial.
4. Combina un filtro de categoría con uno de editorial y confirma que los resultados respetan ambos.

Describe lo que observas en cada paso e indica cualquier comportamiento inesperado.
```

Copilot iniciará un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te informará de lo que ha encontrado. Compara su resumen con los criterios de aceptación de la incidencia. Si algo no parece correcto, haz preguntas de seguimiento o pídele que corrija el código antes de abrir una solicitud de incorporación de cambios.

> [!NOTE]
> Para realizar esta prueba, la aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot utilice el servidor MCP de Playwright, puede que tenga que descargar un navegador. Si indica que falta uno, pídele que ejecute `npx playwright install chromium` y vuelva a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una solicitud de incorporación de cambios

Una vez que hayas confirmado que la funcionalidad funciona, ya puedes abrir una solicitud de incorporación de cambios (PR) para que el equipo la revise. El primer paso es publicar la rama `filtering-vscode`.

1. Ve al panel **Source Control** del codespace y revisa los cambios realizados por Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación mediante el botón **Sparkle**.

   ![Captura de pantalla del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para subir la rama al repositorio.

## Crear la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud de incorporación de cambios, por ejemplo, desde github.com o con la interfaz de línea de comandos (CLI) de GitHub. Pero, como ya estás trabajando con GitHub Copilot, deja que la cree. Puede localizar la incidencia pertinente y asociarla con la PR.

1. Ve al panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva.
2. Asegúrate de que **Agent** está seleccionado en la lista desplegable de agentes para que Copilot pueda utilizar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia relacionada con el filtrado por categoría y editorial. Crea una solicitud de incorporación de cambios para la rama actual y vincúlala con la incidencia correcta.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile la información y realice las operaciones necesarias. La primera vez que utilice una herramienta de GitHub, puede que se te pida que te autentiques. Sigue las instrucciones para autorizarlo.
5. Observa cómo Copilot busca entre las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace que te proporciona Copilot para revisar la solicitud de incorporación de cambios, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían Copilot con nuevas herramientas;
- has añadido el servidor MCP de Playwright a tu espacio de trabajo;
- lo has utilizado para probar manualmente la funcionalidad de filtrado en un navegador antes de publicarla;
- has publicado la rama y abierto una solicitud de incorporación de cambios para el trabajo de filtrado.

Has utilizado un servidor MCP para *probar* una funcionalidad, pero MCP es solo una de las formas de ampliar Copilot. A continuación, vamos a [crear un agente personalizado][next-lesson] para simplificar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué todo el mundo habla de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
