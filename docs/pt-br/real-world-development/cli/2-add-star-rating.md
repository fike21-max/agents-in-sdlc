---
title: "Lição 2 - Adicione avaliações por estrelas: uma vitória rápida"
description: "Use o Copilot CLI para fazer uma pequena mudança nos cartões dos jogos, revise-a em um navegador encaminhado e mescle-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira mudança no projeto. Você manterá o escopo pequeno: os jogos já têm uma avaliação por estrelas nos dados, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que a mostre, revisará a mudança e a mesclará como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em um branch de funcionalidade.
- pedir ao agente que faça uma pequena mudança no projeto.
- revisar a mudança com `/diff`.
- executar o aplicativo para confirmar a mudança em um navegador encaminhado.
- abrir e mesclar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Os cartões de jogos na página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que exiba a avaliação existente em cada cartão — uma mudança pequena e independente, perfeita para sua primeira sessão.

## Estrutura de uma conversa

Uma **conversa** é o espaço onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e o branch que estão atualmente selecionados no terminal, em vez de criar um worktree dedicado. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e o branch continuam sendo o estado normal do Git no disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente; a atividade do agente, que explora e edita arquivos; e as mudanças que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite nossa mudança

Vamos iniciar uma nova conversa para começar a implementar nossa funcionalidade.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se ainda não estiver aberto, inicie o Copilot com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando slash `/new` e pressione <kbd>Enter</kbd>.
5. Use o seguinte prompt para solicitar a mudança:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explora o projeto, encontra os arquivos usados para exibir os detalhes dos jogos e cria o código necessário. Você acabou de adicionar uma nova funcionalidade com o Copilot CLI!

## Revise o diff

Toda mudança gerada por IA deve ser revisada antes de ser mesclada, mesmo as pequenas. Vamos explorar as mudanças diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o cartão do jogo exibe a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes abrangem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como o Copilot, assim como todas as ferramentas de IA generativa, é probabilístico e não determinístico, seu código exato pode variar. Revise o comportamento em vez de esperar uma implementação específica.

## Verifique as mudanças

Não devemos apenas ler o código e presumir que ele funciona. Vamos pedir ao Copilot que inicie o site para podermos examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cartões dos jogos exibem suas avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que ele pare o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e mescle seu primeiro pull request

Você criou a funcionalidade! Agora é hora de abrir um pull request (PR) para mesclar o novo código ao projeto.

1. Peça ao agente padrão que faça o commit da mudança:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI envia o commit existente e exibe a URL do PR.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme a mesclagem.
6. Volte ao seu Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua cópia local de `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira mudança usando o GitHub Copilot CLI! Especificamente, você:

- iniciou uma conversa focada do Copilot em um branch de funcionalidade.
- orientou o agente a fazer uma pequena mudança nos cartões dos jogos.
- revisou a mudança com `/diff`.
- executou o aplicativo para confirmar a avaliação por estrelas em um navegador encaminhado.
- abriu e mesclou seu primeiro pull request.

Em seguida, você partirá da issue de filtragem e usará os modos Plan e Autopilot para [desenvolver uma funcionalidade maior][next-lesson].

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
