---
title: "Lección 6 - Valida la funcionalidad con MCP de Playwright"
description: "Inspecciona o configura MCP de Playwright en Copilot CLI y úsalo para explorar la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos destacado, desarrollar código implica mucho más que escribir código. Necesitamos trabajar con datos y servicios externos, y permitir que Copilot utilice automatizaciones adicionales. Aquí es donde entran en juego los servidores MCP. Los servidores MCP amplían las funciones de Copilot CLI y le proporcionan acceso a más herramientas y servicios.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadirás el servidor MCP de Playwright si aún no está disponible.
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar las actualizaciones de la interfaz de usuario hay que interactuar con ella. Quieres permitir que Copilot utilice el sitio web en el que estás trabajando como lo haría un usuario. Así podrás automatizar más los cambios y tener mayor confianza en que las actualizaciones se comportan como esperas.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Con MCP, los agentes de IA pueden comunicarse con esas herramientas y servicios en tiempo real, acceder a información actualizada y realizar acciones en tu nombre.

Se accede a estas herramientas y recursos mediante un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente.

Algunos de los servidores MCP más conocidos son:

- **[Servidor MCP de GitHub][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y pull requests de GitHub.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: permite automatizar el navegador con Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar que se descubran y para fomentar las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio web como lo haría un usuario.

> [!NOTE]
> Si MCP de Playwright ya está configurado en Copilot CLI, omite estos pasos de configuración y continúa con la siguiente sección.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo para añadir un servidor MCP escribiendo este comando en Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. En el campo de nombre, escribe `playwright` y pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor es **STDIO** pulsando <kbd>Enter</kbd> y, a continuación, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el nuevo servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Antes has comprobado manualmente que la funcionalidad se comporta como esperas. Ahora utilicemos el servidor MCP de Playwright disponible en Copilot para hacer lo mismo.

1. Usa este prompt para pedir a Copilot que utilice el servidor MCP de Playwright para validar la funcionalidad:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> En realidad, no hace falta indicar a Copilot que use el servidor MCP, ya que normalmente lo detectará por su cuenta. Sin embargo, si sabes qué herramienta debería usar, no está de más señalarlo. Así obtendrás resultados más coherentes y ahorrarás algunos tokens.

2. Observa cómo Copilot enumera los pasos que realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y asegúrate de que todo se comporta como esperas.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio web, detendrá el servidor y te proporcionará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el servidor MCP de Playwright para explorar tu funcionalidad en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- has añadido el servidor MCP de Playwright si aún no estaba disponible.
- has pedido al agente que controle un navegador y explore la funcionalidad de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en un rol especializado.

## Recursos

- [¿Qué es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
