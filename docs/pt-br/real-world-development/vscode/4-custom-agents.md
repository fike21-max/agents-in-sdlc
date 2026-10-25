---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados, adaptados a tarefas ou domínios específicos do seu fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você pode fornecer ao Copilot instruções, práticas recomendadas, padrões de programação e conhecimento específico do domínio para orientá-lo a realizar determinados tipos de trabalho com mais eficiência. As equipes podem registrar seus conhecimentos em agentes reutilizáveis — por exemplo, um agente de acessibilidade que impõe a conformidade com as diretrizes da [WCAG][wcag], um agente de segurança que segue práticas de programação segura ou um agente de testes que mantém padrões de teste consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo contém frontmatter YAML com pelo menos os campos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Comparação entre agentes personalizados e habilidades de agente

Há alguma sobreposição conceitual entre agentes personalizados e [habilidades de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como realizar operações. A maneira mais simples de distingui-los é: um **agente personalizado** é quem executa o trabalho; as **habilidades** são as ferramentas.

Os agentes personalizados têm sua própria janela de contexto e foram criados para orquestrar habilidades (e até outros agentes) durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade; como parte desse trabalho, ele pode invocar habilidades como uma habilidade de fluxo de trabalho para pull requests ou outra para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como acontece com qualquer recurso de IA, teste e faça iterações para encontrar o que funciona melhor para seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys está comprometida em tornar sua plataforma de financiamento coletivo acessível a todos os usuários, independentemente de suas capacidades ou preferências visuais. Comentários recentes de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver essa questão de acessibilidade, a equipe de design solicitou a implementação de um modo de alto contraste que os usuários possam ativar e desativar.

Como a acessibilidade é essencial, você quer implementar esse recurso o mais rápido possível. Para isso, usará um agente personalizado.
Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente;
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Já foi criado um agente personalizado de acessibilidade. Vamos revisar o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace, abra um terminal e alterne para uma nova ramificação baseada em `main` para o trabalho de acessibilidade (assim, o PR de filtragem do Exercício 3 ficará separado):

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível;
   - práticas recomendadas de acessibilidade;
   - exemplos de código HTML, CSS e JavaScript;
   - uma lista de erros e problemas comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` na lista suspensa de agentes, na parte inferior da exibição Copilot Chat. Você pode selecionar um agente personalizado para limitar a sessão de chat às instruções e ferramentas desse agente.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam de um exercício para outro — cada etapa indicará o que selecionar quando isso for relevante.

1. Selecione **Agent** na lista suspensa de agentes da exibição Chat, se essa opção ainda não estiver selecionada.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione a lista suspensa de agentes na parte inferior da exibição Chat (ela mostra o agente ativo — por padrão, **default**).
3. Selecione **Accessibility agent** na lista de agentes disponíveis.
4. Envie o seguinte prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat começará o trabalho — explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá em linha no chat, com o caminho do arquivo e um diff para você revisar.
6. À medida que as edições forem aplicadas, o indicador **Files changed** será atualizado para mostrar o conjunto de arquivos que o agente modificou.

> [!NOTE]
> Esse processo provavelmente levará alguns minutos. O Copilot está fazendo alterações reais no repositório — editará arquivos existentes, como componentes Astro, CSS e testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximas etapas

Nesta lição, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados para tarefas e domínios específicos. Com agentes personalizados, você pode registrar os conhecimentos e padrões da equipe em agentes reutilizáveis que orientam o Copilot a realizar determinados tipos de trabalho com mais eficiência.

Você explorou estes conceitos:

- como os agentes personalizados são definidos;
- como usar um agente personalizado no modo de agente do Copilot Chat.

A seguir, você [acompanhará e orientará o trabalho do agente][next-lesson] — revisando as alterações à medida que forem feitas e adicionando uma opção para ativar o modo claro na mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no seu IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar sua funcionalidade com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
