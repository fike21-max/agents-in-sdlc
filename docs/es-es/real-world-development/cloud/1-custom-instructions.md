---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta, o hay información general que Copilot debería conocer, conviene asegurarse de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] permiten proporcionar esa orientación para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo* quieres que lo haga.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones acotados a rutas.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no harás una comparación entre un prompt *antes y después*. El agente de Copilot en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar su efecto en tiempo real. Más adelante, en este recorrido, verás la influencia del archivo de instrucciones cuando revises las pull requests del agente en la nube.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz de usuario debe estar en modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- La cabecera de cada archivo debe incluir un bloque de comentarios que describa su función.

Con los archivos de instrucciones, te asegurarás de que Copilot tenga la información adecuada para realizar las tareas de acuerdo con las prácticas indicadas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y requisitos de programación. Son una forma eficaz de orientar las sugerencias y los fragmentos de código. Puedes indicar las convenciones de programación, las bibliotecas y hasta el tipo de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tareas o tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot para **cada** solicitud del repositorio. Debe incluir información del proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general de lo que se está desarrollando, las prácticas recomendadas y otras directrices generales.
- `.github/instructions/*.instructions.md`, archivos que se pueden crear para tareas o tipos de archivos concretos. Permiten proporcionar directrices para determinados lenguajes, como TypeScript o Astro, o para tareas como crear un componente de interfaz de usuario o un conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerencias de edición siguiente.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que tengan frontmatter `applyTo`.
>
> Además, Copilot [admite archivos de instrucciones de otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Explicar a fondo cómo crear archivos de instrucciones queda fuera del alcance de este taller. Sin embargo, el proyecto de ejemplo muestra un enfoque representativo. A grandes rasgos:

- Mantén las instrucciones de `copilot-instructions.md` centradas en la orientación general del proyecto, como una descripción de lo que se está desarrollando, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para proporcionar indicaciones específicas sobre tipos de archivo (pruebas unitarias, componentes Astro o la capa de datos) o tareas concretas.
- Usa lenguaje natural. Formula directrices claras y proporciona ejemplos del aspecto que debe y no debe tener el código.

No hay una única forma correcta de crear archivos de instrucciones, igual que no hay una única forma de usar IA. La experimentación te permitirá descubrir qué funciona mejor en tus proyectos y escenarios.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían disponer de un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, puede que veas instrucciones para muchos tipos de tareas, como las [actualizaciones de la interfaz de usuario][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de una forma distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI). La lección correspondiente al entorno que estés usando te indicará cómo hacerlo cuando proceda.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio que contiene archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y un conjunto de archivos `*.instructions.md` para diversas tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y observa la breve descripción del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las directrices anidadas de **GitHub Actions Workflows**. Estas instrucciones se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Observa que hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior, que establece un patrón glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, coincide con cualquier archivo de pruebas TypeScript, por ejemplo, los que cumplen el patrón `**/*.test.ts`.
5. Lee las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos más pequeños y reutilizables, y señalar ejemplos para que Copilot los siga al generar código. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación en el código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y las cabeceras de los archivos.

## Añade un nuevo estándar para el repositorio

El siguiente es el único paso de este ejercicio en el que editarás un archivo: añadirás una regla para que la documentación se incorpore al código mediante comentarios TSDoc y una cabecera de comentario a nivel de archivo. El agente en la nube utilizará estas instrucciones cuando trabaje en las incidencias que le asignes más adelante en este recorrido.

Antes de editar, crea una rama (el agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero confirmarás los cambios de forma ordenada):

1. En el terminal del codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha indicado antes, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot. Asegurémonos de que los estándares de programación del repositorio incluyan la documentación en el código para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de programación del proyecto, pero aún no contiene ninguna regla sobre la documentación del código; por eso, el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown debajo de los estándares existentes para pedir a Copilot que incluya cabeceras de comentario y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para proporcionar directrices globales o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para los estándares generales, como la regla sobre los comentarios de documentación que acabas de añadir.

## Confirma, sube y combina la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que apunta la incidencia. Cuando le asignes una incidencia en el próximo ejercicio, Copilot creará una rama a partir de `main`. Por tanto, tus cambios en las instrucciones deben llegar a `main` para que el agente en la nube pueda utilizarlos.

1. Prepara los cambios y crea una confirmación:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Sube la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una pull request desde `custom-instructions-cloud` hasta `main` y combínala. El agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes en el siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama sirve para que el taller refleje cómo trabajarías en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo obtiene Copilot el contexto de los archivos de instrucciones de este proyecto y has añadido un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`. Más adelante, verás cómo se aplica ese estándar en las pull requests que genera el agente en la nube.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
