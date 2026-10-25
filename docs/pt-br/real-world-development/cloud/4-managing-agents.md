---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente na nuvem do Copilot que executasse três tarefas distintas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente na nuvem trabalhe de forma assíncrona e autônoma, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente na nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel, você pode ver todas as tarefas de agentes que têm pull requests (PRs) abertos. Também pode explorar as operações executadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente na nuvem.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a criação de um modo de alto contraste, a equipe percebeu que também seria um bom momento para adicionar um modo claro. Como já havia trabalho em andamento para atualizar o estilo do site e adicionar funcionalidades de alternância, pareceu lógico incluir esse recurso. Você orientará o trabalho do agente para garantir que ele adicione o modo claro e o modo de alto contraste.

## Revise as tarefas do agente na nuvem do Copilot

Vamos verificar o status atual de todas as tarefas atribuídas ao agente na nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá encontrar as tarefas atribuídas ao Copilot, incluindo:
    - atualizar a documentação da base de código.
    - gerar APIs para modificar jogos.
    - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em execução. Revise as operações realizadas pelo Copilot, que podem incluir:
    - clonar o código do repositório.
    - criar o ambiente de trabalho do Copilot.
    - configurar servidores MCP.
    - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam conforme o estado do trabalho do Copilot e a abordagem usada.

4. Observe também o painel do pull request (PR), exibido à direita. Ele permite ver o PR e os arquivos alterados para acompanhar o trabalho.

## Oriente o agente na nuvem

Agora que você já viu as tarefas ativas, vamos pedir ao Copilot que inclua o seletor do modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração foi concluída e o trabalho começou. Você perceberá isso quando surgirem mensagens parecidas com os exemplos a seguir.
3. No diálogo **Steer active session while Copilot is working**, adicione o seguinte prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de tela da tarefa do agente na nuvem na página de agentes, com o diálogo para orientar a sessão ativa destacado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do prompt e o inclui no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada! Ele incluirá a nova solicitação no fluxo de trabalho depois que concluir a etapa em que estava trabalhando quando você enviou a mensagem.

Como isso também levará alguns minutos, aproveite para fazer uma pausa e refletir sobre tudo o que aprendeu e explorou até aqui.

## Resumo e próximos passos

Esta lição apresentou a página de agentes do Copilot, seu ponto central para acompanhar e orientar as tarefas do agente na nuvem do GitHub Copilot. Nesse painel, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho e até redirecionar tarefas em andamento para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- explorou o painel de agentes para acompanhar as tarefas do agente na nuvem.
- redirecionou uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos nos concentrar em [fazer iterações nos pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Fazer iterações no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
