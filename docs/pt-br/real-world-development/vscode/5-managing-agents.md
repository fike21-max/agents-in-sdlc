---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você coloca o GitHub Copilot no modo de agente, ele trabalha de forma autônoma — explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo à medida que ocorrem. Você pode revisar cada diff em linha, aceitar ou rejeitar alterações individuais e orientar a conversa com prompts de acompanhamento para refinar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade;
- revisar e aceitar as edições de arquivos propostas;
- orientar o agente durante a sessão para adicionar uma opção de modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado na [Lição 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design faz uma solicitação adicional — também gostaria de uma opção de modo claro para usuários que acham o tema padrão escuro demais. Em vez de iniciar uma conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Acompanhar o progresso do Copilot

A exibição Copilot Chat mostra cada etapa realizada pelo agente — os arquivos que ele explora, os comandos que executa e as edições que propõe. Cada edição de arquivo aparece em linha no chat, com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat da Lição 4.
2. Enquanto o agente trabalha, role a conversa para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos, além de comandos do terminal cuja execução ele solicitar.
3. Quando o agente propuser uma edição de arquivo, o diff aparecerá em linha:
    - Selecione **Keep** para aceitar a alteração.
    - Selecione **Undo** para desfazer a alteração e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos modificados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão (por exemplo, para executar um comando do terminal), o agente pausará e exibirá uma solicitação **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde até que o agente termine de implementar o modo de alto contraste. Você verá uma mensagem final resumindo as alterações feitas.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, você ampliará a mesma conversa para adicionar uma opção de modo claro. Como continuará na mesma sessão de chat, o agente manterá todo o contexto do trabalho anterior — inclusive os componentes alterados, a abordagem de estilo adotada e o padrão de armazenamento local configurado.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt de acompanhamento:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará de onde parou, explorará as alterações já feitas e proporá adições para o modo claro.
3. Revise cada nova edição à medida que for aplicada, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface das opções de um jeito que você não goste —, você pode interrompê-lo com o botão **Stop** ao lado do campo de entrada do chat e enviar uma mensagem de acompanhamento para esclarecer o que deseja.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para solicitar ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione uma terceira opção; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para as duas opções").

## Revisar o conjunto de alterações

Antes de confirmar o trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações em componentes Astro, estilos e testes relacionados.
3. Abra alguns dos arquivos alterados e examine os diffs. Confirme se os padrões de acessibilidade do agente personalizado foram aplicados — atributos ARIA, navegação pelo teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e confirme as alterações pela exibição Source Control. Você as publicará [na próxima lição][next-lesson].

## Resumo e próximas etapas

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento — revisou cada edição de arquivo, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como acompanhar o trabalho de um agente em tempo real;
- como aceitar e rejeitar edições individuais de arquivos;
- como orientar uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará seu trabalho de acessibilidade como um pull request e revisará as alterações feitas pelo agente personalizado local durante este laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
