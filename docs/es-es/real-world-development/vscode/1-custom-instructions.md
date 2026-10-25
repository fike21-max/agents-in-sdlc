---
title: "Ejercicio 1: Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo agente →][next-lesson]

El contexto es fundamental cuando trabajas con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita conocer cierta información, conviene asegurarse de que pueda acceder a ese contexto. Hay varias maneras de proporcionar contexto específico a Copilot. Una de las más importantes son los [archivos de instrucciones][instruction-files], que permiten orientarlo sobre cómo generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones aplicables a rutas concretas;
- generarás la primera parte de los datos para el filtrado (una función auxiliar para editoriales) con las instrucciones actuales;
- añadirás un nuevo estándar para todo el repositorio a `.github/copilot-instructions.md`;
- volverás a enviar el mismo prompt y observarás cómo el código generado adopta el nuevo estándar;
- confirmarás los cambios en las instrucciones y la parte de filtrado en `main` para que Copilot pueda utilizar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no ajustarse a algunos de los estándares que establezcas. Copilot no es determinista. El objetivo de este ejercicio es observar la tendencia del comportamiento después de actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe tener un tema oscuro y un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Al principio de cada archivo debe añadirse un bloque de comentarios que describa su función.

Mediante archivos de instrucciones, te asegurarás de que Copilot disponga de la información necesaria para realizar tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que pueda entender mejor tu estilo de programación y tus requisitos. Son una herramienta eficaz para orientar sus sugerencias y fragmentos de código. Puedes especificar las convenciones de programación que prefieres, las bibliotecas que utilizas e incluso los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para determinados tipos de archivos, como contexto específico de una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe incluir información general del proyecto, es decir, contexto relevante para la mayoría de las solicitudes de chat o CLI enviadas a Copilot. Puede describir la pila tecnológica, explicar qué se está creando, indicar las prácticas recomendadas y proporcionar otras directrices generales.
- Los archivos `.github/instructions/*.instructions.md` se pueden crear para tareas o tipos de archivos concretos. Puedes utilizarlos para proporcionar directrices sobre lenguajes específicos, como TypeScript o Astro, o sobre tareas como crear un componente de interfaz de usuario o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, Copilot Chat solo utiliza los archivos de instrucciones para generar código; no los utiliza para completar código ni para sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también [admite archivos de instrucciones que siguen otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para administrar archivos de instrucciones

Crear archivos de instrucciones queda fuera del alcance de este taller. Sin embargo, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén las instrucciones de `copilot-instructions.md` centradas en las directrices generales del proyecto, como una descripción de lo que se está creando, la estructura del proyecto y los estándares globales de programación.
- Utiliza archivos `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes de Astro o la capa de datos) o de determinadas tareas.
- Utiliza lenguaje natural y expresa las directrices con claridad. Incluye ejemplos del aspecto que debe tener el código y del que debe evitarse.

No hay una única forma correcta de crear archivos de instrucciones, del mismo modo que no hay una sola manera de utilizar la IA. Con la práctica descubrirás qué funciona mejor para tu proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería contar con una colección completa de archivos de instrucciones. Al explorar los de este proyecto, puede que observes que hay archivos para numerosos tipos de tareas, como [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada interfaz lo ofrece de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección correspondiente a la interfaz que estés utilizando lo explicará cuando sea pertinente.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio que contiene archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un archivo `copilot-instructions.md` principal y varios archivos `*.instructions.md` para distintas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. Estas directrices se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Encontrarás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` del principio: define un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincidirá con cualquier archivo de pruebas TypeScript, por ejemplo, uno cuyo nombre siga el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. De esta forma puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que debe seguir al generar código. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` describe los estándares de programación del proyecto, pero todavía no exige documentación en el código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los encabezados de comentarios de los archivos.

## Crear una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; se indicarán en cada paso cuando corresponda.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar el mismo prompt para comparar los resultados.

1. Cierra las pestañas del editor que sigan abiertas de ejercicios anteriores para que Copilot utilice solo el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para que Copilot sepa dónde debe ubicarse la función auxiliar.
3. En la lista desplegable de agentes de la vista Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía el siguiente prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios en el archivo.
   ```

5. Copilot explorará el proyecto y aplicará actualizaciones al código. A menudo, los cambios abarcarán el archivo de la función auxiliar y sus pruebas.
6. Fíjate en que la función auxiliar propuesta está tipada, recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto se debe a las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentarios a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios de documentación incluso sin que se lo pidas. Si ocurre, no pasa nada: el objetivo sigue siendo observar la mejora de coherencia después de actualizar las instrucciones.

## Añadir un nuevo estándar para el repositorio

Como has visto, `.github/copilot-instructions.md` proporciona a Copilot información general del proyecto. Documentemos los estándares de programación del repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa que describe los estándares de programación del proyecto, pero aún no incluye reglas para documentar el código. Por eso la función auxiliar generada no incluía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo después de los estándares existentes para indicar a Copilot que añada encabezados de comentarios a los archivos y comentarios TSDoc:

   ```markdown
   - Todas las funciones exportadas deben tener un comentario TSDoc que describa su propósito, sus parámetros y su valor de retorno.
   - Antes de las importaciones o de cualquier código, añade al archivo un bloque de comentarios que explique su propósito.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, los archivos de instrucciones pueden crearse en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales, o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Volver a enviar el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contenido de la conversación.
2. Selecciona `src/lib/publishers.ts` para que Copilot se centre en el archivo correcto.
3. Envía el **mismo prompt** que antes:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con una función auxiliar de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios en el archivo.
   ```

4. Observa que el archivo propuesto ahora empieza con un bloque de comentarios similar a este:

   ```typescript
   /**
    * Funciones auxiliares de acceso a datos para la plataforma de financiación colectiva Tailspin Toys.
    * Incluye funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc similar a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y nombre.
    *
    * @param db - Cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de objetos que representan editoriales.
    */
   ```

Has conseguido que Copilot siga un nuevo estándar del proyecto y lo aplique a código real sobre el que se basará el siguiente ejercicio.

## Confirmar las instrucciones y subir la rama

Los archivos de instrucciones son recursos del repositorio, por lo que se administran igual que cualquier otro elemento mediante el control de versiones. Confirma los cambios y sube la rama al repositorio.

1. Abre una ventana de terminal nueva pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Desde el terminal, comprueba que están presentes la actualización de las instrucciones y los cambios del asistente ejecutando:

   ```bash
   git status
   ```

3. Desde el terminal, prepara y confirma la actualización de las instrucciones y todos los archivos que Copilot haya modificado para crear la base del asistente:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Sube la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y fusionar una solicitud de incorporación de cambios

Una vez subida la rama, crea una solicitud de incorporación de cambios (PR) y vincúlala con la incidencia sobre estándares de documentación de tu trabajo pendiente. Podrías hacerlo manualmente, pero Copilot puede encargarse: pídele que busque la incidencia, cree una PR para cerrarla y la fusione.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub por ti. La plantilla del proyecto ya conecta las herramientas de GitHub con tu espacio de trabajo, así que no tienes que configurar nada. La primera vez que Copilot utilice una de ellas, VS Code puede pedirte que inicies sesión en GitHub. Sigue las instrucciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y comprueba que **Agent** está seleccionado en el selector de agentes.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y que cree una PR desde la rama actual. La descripción debe incluir tanto las actualizaciones de las instrucciones como la estructura inicial de la función auxiliar para editoriales:

   ```
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una nueva PR desde la rama actual, indica que la PR cierra esa incidencia e incluye que también hemos añadido la estructura inicial de la función auxiliar de editoriales para el próximo trabajo de filtrado.
   ```

4. Copilot empezará a buscar la incidencia y a crear la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub en tu nombre, revisa el comando y selecciona **Allow** si corresponde.
6. Una vez creada la PR, pide a Copilot que la fusione y que cambie tu rama local a `main` con el siguiente prompt:

   ```
   Fusiona la PR en main. Después, vuelve localmente a main y descarga el código más reciente para tener todo actualizado.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos de shell en tu nombre, revisa el comando y selecciona **Allow** si corresponde.

¡Ya has creado y fusionado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo obtiene Copilot el contexto de los archivos de instrucciones del proyecto. Después, has utilizado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un estándar nuevo para todo el repositorio a `.github/copilot-instructions.md`;
- volver a enviar el mismo prompt y observar cómo el código propuesto adopta el nuevo estándar;
- confirmar las instrucciones actualizadas y la función auxiliar en `main` para que el siguiente ejercicio pueda utilizarla.

A continuación, pondrás en práctica esas instrucciones en el [modo agente][next-lesson] mientras Copilot añade una nueva funcionalidad a todo el código base.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
