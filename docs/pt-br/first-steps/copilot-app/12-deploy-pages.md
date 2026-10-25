---
title: "Lição 12 - Publique o site com o GitHub Pages"
description: "Use o aplicativo do GitHub Copilot para configurar um workflow do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público ao publicá-lo com o GitHub Pages. O Copilot pode preparar o workflow e orientar a publicação, mas você continua responsável por revisar as mudanças e confirmar que o site está no ar.

Nesta lição, você vai:

- pedir ao Copilot para configurar a publicação no GitHub Pages.
- revisar o workflow e o pull request antes de mesclá-lo.
- acompanhar a publicação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma maneira simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages o disponibiliza em uma URL pública, sem adicionar um servidor ou uma dependência de runtime.

## Peça ao Copilot para publicar o site

Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

```plaintext
Enable GitHub Pages for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and merge it after required checks pass. Monitor the deployment until it succeeds, then report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![Diagrama em três etapas: pull request do space-quiz mesclado; execução bem-sucedida do Deploy website no GitHub Actions, com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede ao agente que crie e mescle um pull request e altere as configurações do Pages do repositório. Revise o diff, as permissões de publicação e quaisquer solicitações de aprovação do GitHub antes de prosseguir.

## Revise o workflow e verifique a publicação

1. Confirme que o README só foi adicionado se ainda não havia um no repositório.
2. Revise as mudanças do pull request, incluindo as etapas de compilação e publicação do workflow.
3. Mescle somente depois que as verificações obrigatórias do pull request forem aprovadas.
4. Acompanhe a execução de publicação do GitHub Actions até que ela seja bem-sucedida.
5. Abra a URL informada e confirme que o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou mesclar o pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a publicação seja bem-sucedida e o site esteja no ar.

## Resumo e próximos passos

Agora o Space Quiz está disponível como um site publicado, e você verificou a publicação em vez de confiar no relato do agente. Continue para [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
