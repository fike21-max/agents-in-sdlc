---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su pull request y comprobar que Space Quiz está disponible."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dale a Space Quiz un hogar público y publícalo con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiarte durante la publicación, pero tú sigues siendo responsable de revisar los cambios y confirmar que el sitio está disponible.

En esta lección:

- pedirás a Copilot que configure una publicación en GitHub Pages.
- revisarás el flujo de trabajo y la pull request antes de combinarla.
- seguirás la publicación y comprobarás que el sitio se ha publicado.

## Escenario

Space Quiz ya está en un repositorio de GitHub, pero la gente aún necesita una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él desde una URL pública, sin añadir un servidor ni dependencias de ejecución.

## Pide a Copilot que publique el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama en tres etapas: una pull request de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions, con los pasos Build website, Upload artifact y Deploy to Pages; y Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y combine una pull request y cambie la configuración de Pages del repositorio. Revisa las diferencias, los permisos de publicación y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisa el flujo de trabajo y comprueba la publicación

1. Confirma que el README se añade solo si el repositorio aún no tiene uno.
2. Revisa los cambios de la pull request, incluidos los pasos de compilación y publicación del flujo de trabajo.
3. Combina los cambios solo después de que se superen las comprobaciones obligatorias de la pull request.
4. Sigue la ejecución de publicación de GitHub Actions hasta que se complete correctamente.
5. Abre la URL indicada y confirma que Space Quiz se carga.

Si el agente no puede habilitar Pages o combinar la pull request, sigue las instrucciones específicas que te proporcione. No des la tarea por completada hasta que la publicación se haya completado correctamente y el sitio esté disponible.

## Resumen y siguientes pasos

Space Quiz ya está disponible como sitio publicado, y has comprobado la publicación en lugar de confiar únicamente en el informe del agente. Continúa con la [Lección 13: Repaso y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
