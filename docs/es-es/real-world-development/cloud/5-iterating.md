---
title: "Ejercicio 5: Iterar en el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

A lo largo de este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que añada documentación al código, desarrolle una funcionalidad de juegos relacionados para que el equipo de diseño pueda probarla y añada funciones de accesibilidad, como los modos de alto contraste y claro. Revisemos los cambios de código y, si es necesario, enviemos comentarios a Copilot para mejorar el resultado.

### Escenario

Como se ha repetido varias veces, los fundamentos del diseño de software y DevOps no cambian con la incorporación de la IA generativa. Siempre debemos revisar el código generado y seguir nuestro proceso habitual de DevOps. Con esto en mente, revisaremos las sugerencias de GitHub Copilot para añadir documentación, crear la funcionalidad de juegos relacionados y mejorar la accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube realiza tareas de forma asíncrona y sin supervisión, se han establecido determinadas restricciones de seguridad para mantener la seguridad del proceso. Entre ellas:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **solo** a la rama que utilizará para el código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno de trabajo separado y efímero.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [De forma predeterminada, el acceso a recursos externos está limitado](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por explorar la primera pull request (PR) que ha generado el agente de GitHub Copilot en la nube: la que añade documentación al código. Para ello, utilizaremos la interfaz habitual de las PR de GitHub.com.

> [!NOTE]
> Al explorar la PR, puede que veas un aviso de que un firewall está bloqueando GitHub Copilot. **Es lo esperado**, ya que Copilot tiene acceso limitado a los recursos externos de forma predeterminada, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el firewall del agente de Copilot en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de pull requests.
3. Abre la pull request cuyo título sea parecido a **Add missing documentation** o algo más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la pull request incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes hacer una pausa o reflexionar sobre lo que has aprendido hasta ahora.

4. Cuando la pull request esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluirá los comentarios de documentación TSDoc que se han creado y otra documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y cabeceras de comentario. Estos cambios proceden de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.

## Solicita cambios a GitHub Copilot

La colaboración con Copilot en una pull request no tiene por qué ser unidireccional. Puedes mencionarlo en comentarios de la pull request —igual que mencionarías a otra persona del equipo— o en comentarios sobre líneas concretas de código. Copilot verá esos comentarios e iniciará otra sesión para responder a ellos. Como los resultados no son deterministas, no podemos darte un texto exacto que debas enviar. Por ejemplo, podrías pedir a Copilot que:

- añada al principio de cada archivo de código una cabecera de comentario que describa brevemente su función.
- añada comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Puedes usar una de las ideas anteriores u otra sugerencia de documentación que quieras ver en el código base.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la PR.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer una confirmación nueva en la pull request.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la PR en borrador como lista para revisión y combinarla en la rama principal.

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Volvamos a la PR que ha generado Copilot para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page** o algo más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** Puedes cambiar a esta rama en tu Codespace para probar manualmente la funcionalidad de juegos relacionados. Ve a tu Codespace, abre el terminal y ejecuta los siguientes comandos (sustituye `<branch-name>` por el nombre de la rama que haya creado Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

¡Copilot ya ha creado la funcionalidad de juegos relacionados! Como antes, puedes iterar con el agente de Copilot en la nube y pedirle que actualice el resultado. Por ejemplo, puedes pedir a Copilot que cambie la cantidad de juegos relacionados que se muestran o que añada cabeceras de comentario y comentarios de documentación TSDoc (recuerda: esta incidencia se asignó **antes** de que actualizaras las instrucciones personalizadas). Como en el caso anterior, puedes hacerlo con un comentario nuevo en la pestaña **Conversation**. Copilot lo verá e iniciará una nueva sesión.

## Revisa las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir tanto el modo de alto contraste que asignaste en el Ejercicio 3 como el modo claro que solicitaste desde Mission control en el Ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website** o algo más descriptivo.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la pull request incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz de usuario que permiten cambiar de modo.
   - el uso del almacenamiento local para guardar las preferencias del usuario.
   - los cambios de CSS o estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.).
   - el código JavaScript/TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que algunos flujos de trabajo están pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto deberían superarse para la aplicación Astro. El proceso puede tardar unos minutos.
10. **Opcional:** Puedes cambiar a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Ve a tu Codespace, abre el terminal y ejecuta los siguientes comandos (sustituye `<branch-name>` por el nombre de la rama que haya creado Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba en el navegador los modos de alto contraste y claro. Comprueba que funcionen como esperas y que los cambios se conserven al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a guiar a Copilot para que implemente estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si encuentras problemas de accesibilidad o crees que se puede mejorar algo, puedes mencionar a **@copilot** en un comentario y pedirle que haga cambios, como en las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta especialmente útil cuando puedes asignarle tareas reales del backlog y centrarte en otras cosas. Para acostumbrarte a trabajar así, crea más incidencias en tu repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para indicar interés por un juego en su página de detalles.
- implementar la paginación en la lista de juegos.
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. A lo largo de estas lecciones:

- **Has revisado los archivos de instrucciones personalizadas que incluye el repositorio** para comprobar más adelante su efecto en el trabajo del agente en la nube.
- **Has asignado incidencias al agente de Copilot en la nube** y has observado cómo configura su entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir los modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado las pull requests del agente en la nube e iterado en ellas**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Repaso y siguientes pasos

Has completado el recorrido del agente en la nube. Si quieres seguir explorando, los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente QA personalizado, MCP de Playwright, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente de Copilot en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
