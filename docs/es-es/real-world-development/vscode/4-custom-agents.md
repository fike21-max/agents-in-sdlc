---
title: "Ejercicio 4: Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados y adaptados a tareas o ámbitos concretos del flujo de trabajo de desarrollo. Al definir agentes mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones específicas, prácticas recomendadas, patrones de programación y conocimientos del ámbito que le ayuden a realizar determinados tipos de trabajo con mayor eficacia. Los equipos pueden plasmar sus conocimientos en agentes reutilizables: un agente de accesibilidad que comprueba el cumplimiento de las directrices de [WCAG][wcag], uno de seguridad que sigue prácticas de programación segura o uno de pruebas que mantiene patrones coherentes.

Los agentes personalizados se definen mediante archivos Markdown en la carpeta `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo incluye metadatos YAML con, como mínimo, los campos `name` y `description`, seguidos de un prompt Markdown que define el comportamiento, los conocimientos y las instrucciones del agente.

### Comparación entre los agentes personalizados y las habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] comparten algunos aspectos. Una forma sencilla de distinguirlos es pensar que el **agente personalizado** es quien realiza el trabajo y las **habilidades** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y están diseñados para coordinar habilidades e incluso otros agentes como parte de su trabajo. En este laboratorio, el agente personalizado de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad. Como parte de ese trabajo, podría utilizar habilidades como una para gestionar el flujo de una solicitud de incorporación de cambios o una que ejecute y administre pruebas.

> [!NOTE]
> No existe una única forma correcta de crear un agente personalizado. Como con cualquier tecnología de IA, prueba distintas opciones y repite el proceso hasta encontrar las que mejor se adapten a tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a hacer que su plataforma de financiación colectiva sea accesible para todos los usuarios, independientemente de sus capacidades visuales o preferencias. Los comentarios recientes de los usuarios indican que a algunos les cuesta leer el tema oscuro actual porque el contraste entre el texto y los colores de fondo es insuficiente. Para solucionar este problema de accesibilidad, el equipo de diseño ha solicitado implementar un modo de alto contraste que los usuarios puedan activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta funcionalidad lo antes posible. Utilizarás un agente personalizado para generarla.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisemos su contenido para entender cómo orientará a Copilot.

Vuelve al codespace, abre un terminal y crea una rama nueva a partir de `main` para trabajar en la accesibilidad. Así mantendrás independiente la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los metadatos YAML con los campos `name` y `description`.

> [!CAUTION]
> Los campos `name` y `description` en los metadatos son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, donde se destacan:
   - Las responsabilidades principales al generar código para un sitio web accesible.
   - Las prácticas recomendadas de accesibilidad.
   - Ejemplos de código HTML, CSS y JavaScript.
   - Una lista de errores y problemas habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en la lista desplegable de agentes, situada en la parte inferior de la vista Copilot Chat. Puedes seleccionar un agente personalizado para limitar una conversación a sus instrucciones y herramientas.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; se indicarán en cada paso cuando corresponda.

1. Si aún no está seleccionado, elige **Agent** en la lista desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona la lista desplegable de agentes en la parte inferior de la vista Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía el siguiente prompt al agente de accesibilidad:

    ```plaintext
    Añade al sitio un modo de alto contraste. Debe haber un control de alternancia que el usuario pueda activar para el alto contraste, y la selección debe conservarse al volver a cargar la página mediante el almacenamiento local del navegador.
    ```

5. Copilot Chat se pondrá manos a la obra: explorará el código base, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat junto con la ruta del archivo y un diff que podrás revisar.
6. A medida que se apliquen los cambios, el indicador **Files changed** se actualizará para mostrar los archivos modificados por el agente.

> [!NOTE]
> Es probable que el proceso tarde unos minutos. Copilot está realizando cambios reales en el repositorio; editará archivos existentes, como los componentes de Astro, los estilos CSS y las pruebas relacionadas.

Revisarás y orientarás el trabajo mientras se realiza en el siguiente ejercicio.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y ámbitos concretos. Con ellos puedes plasmar los conocimientos y estándares de tu equipo en agentes reutilizables que orientan a Copilot para que realice determinados tipos de trabajo con mayor eficacia.

Has aprendido:

- cómo se definen los agentes personalizados;
- cómo utilizar un agente personalizado en el modo agente de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se realizan y añadirás un selector de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y administrar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
