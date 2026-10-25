---
title: "Lección 1: Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, familiarízate con el entorno y localiza la incidencia de filtrado inicial."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un potente asistente de programación con agentes que se ejecuta en el terminal y te permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas, todo desde la línea de comandos. Te permite delegar tareas, solicitar cambios y mantener la concentración. Como cabe esperar, el primer paso es instalar la herramienta. Por suerte, puedes hacerlo con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm.
- te autenticarás con tu cuenta de GitHub.
- confiarás en el repositorio del taller y probarás una conversación rápida.
- localizarás la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

## Escenario

Tu equipo empieza a utilizar agentes de IA para gestionar un backlog cada vez mayor. Copilot CLI incorpora esta capacidad al terminal, donde muchos desarrolladores ya trabajan. En esta lección instalarás la CLI, te autenticarás y la dejarás lista para usar durante el resto del taller.

## Instala Copilot CLI

Puedes instalar Copilot CLI mediante [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces incluye Node.js preinstalado, utilizarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debería aparecer el número de versión.

## Autentícate con GitHub

La primera vez que inicies Copilot CLI, te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se te solicita, sigue las instrucciones para autenticarte con un código de dispositivo y autorizar Copilot CLI.
3. Copilot CLI mostrará el siguiente mensaje:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Comprueba que la ruta corresponde a tu repositorio Tailspin Toys y responde afirmativamente seleccionando **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En un Codespace, puede que ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedirte que te autentiques, ya está todo listo.

## Familiarízate con el entorno

Los comandos que escribas en el terminal se ejecutan directamente en tu Codespace. Cuando se inicia Copilot CLI, el agente recibe lenguaje natural y los comandos de barra controlan la conversación.

1. Escribe `/model`, usa las teclas de dirección para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para ver los comandos disponibles en la versión instalada y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla a Copilot para comprobar que todo funciona:

   ```plaintext
   What are the key files in this project?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de responder.
5. Escribe `/mcp list` y confirma que el servidor MCP integrado de GitHub está disponible.
6. Pide a Copilot que localice la incidencia de filtrado:

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. Abre la URL y lee la incidencia. La utilizarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama que esté seleccionada en el terminal; no crea un worktree automáticamente. Crearás una rama de funcionalidad antes de cada cambio.

## Usa el atajo del taller

Normalmente, Copilot CLI pide permiso antes de utilizar herramientas que no estén incluidas en sus permisos actuales. Para este taller, volverás a iniciarla con `--yolo`, un atajo autorizado por el usuario que elimina esas solicitudes de permiso dentro del Codespace para que puedas centrarte en los ejercicios.

> [!CAUTION]
> `--yolo` habilita todos los permisos automáticos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como un Codespace o una máquina virtual, y nunca lo configures como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning] para obtener más información.

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas MCP de GitHub de lectura y escritura que utilizarás en lecciones posteriores para trabajar con incidencias y solicitudes de incorporación de cambios. El Codespace limita el acceso a tu equipo local, pero los recursos autenticados de GitHub siguen siendo reales. Revisa los cambios antes de publicarlos o combinarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarla desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta rápida sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de cambiar las instrucciones o añadir un agente, utilizarás `copilot --resume` para volver a la misma conversación y rama de la funcionalidad.

## Resumen y pasos siguientes

Has instalado GitHub Copilot CLI y te has autenticado. En esta lección:

- has instalado GitHub Copilot CLI con npm.
- te has autenticado con tu cuenta de GitHub.
- has confiado en el repositorio del taller y probado una conversación rápida.
- has localizado la incidencia de filtrado mediante el servidor MCP integrado de GitHub.

A continuación, [iniciarás tu primer cambio específico][next-lesson] y utilizarás Copilot CLI para mostrar las valoraciones por estrellas en las tarjetas de juegos.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
