---
title: "Lección 4 - Guía a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es fundamental al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta, conviene proporcionar esa orientación a Copilot. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que ha usado Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones acotadas a rutas.
- actualizarás el archivo de instrucciones para asegurarte de que se siguen los estándares de programación.
- observarás cómo afectan los archivos de instrucciones al código.

## Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para las prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no limitarse a repetir el código.
- Las funciones exportadas en `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos con TSDoc/JSDoc, incluido un argumento `db` inyectable cuando corresponda.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse al día cuando cambie el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

Con los archivos de instrucciones, te asegurarás de que Copilot tenga la información adecuada para realizar las tareas de acuerdo con las prácticas indicadas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para que entienda mejor tu estilo y requisitos de programación. Son una forma eficaz de orientar las sugerencias y los fragmentos de código. Puedes indicar las convenciones de programación, las bibliotecas y hasta el tipo de comentarios que quieres incluir. Puedes crear instrucciones para todo el repositorio o para tipos de archivos concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un archivo único que se envía a Copilot para **cada** solicitud del repositorio. Debe incluir información del proyecto pertinente para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`, archivos con directrices para determinados lenguajes, tipos de archivos o tareas.

> [!NOTE]
> Otros formatos de instrucciones y su compatibilidad varían según el entorno. Consulta la [referencia sobre compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas de este proyecto

Para empezar, el proyecto incluye un conjunto de archivos de instrucciones. Explorémoslos antes de hacer cambios para ver qué efecto tienen.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora el archivo y observa la breve descripción del proyecto y las directrices de programación. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Observa que hay instrucciones para archivos Astro, la capa de datos Drizzle, las pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md`. Observa el campo `applyTo` de la parte superior: establece un patrón glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot pueda seguir.

## Actualiza los archivos de instrucciones para reflejar las directrices del equipo

Los archivos actuales son un buen comienzo, pero aún falta algo. Modificaremos el archivo principal `copilot-instructions.md` para que el nuevo TypeScript incluya comentarios TSDoc.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar aproximadamente en la línea 35.
2. Añade este elemento al final de la lista:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Usa las directrices actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones actualizadas, cierra la conversación y retómala para que Copilot aplique las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit` para cerrarla. Copilot guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los nuevos comentarios TSDoc y confirma que describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot aplica las instrucciones del repositorio y has incorporado un nuevo estándar a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones acotadas a rutas.
- has actualizado el archivo de instrucciones para asegurar el cumplimiento de los estándares de programación.
- has observado cómo afectan al código los archivos de instrucciones.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable quality-checks][next-lesson] para asegurarte de que las pruebas y el lint se ejecuten de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con las instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
