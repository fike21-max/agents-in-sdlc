---
title: "Exercício 2 - Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações, se houver alguma, não enfrentem dificuldades com dívida técnica. Pode ser por problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que ficaram paradas no backlog por falta de tempo para implementá-las. O agente na nuvem do GitHub Copilot foi criado para executar tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um PR de rascunho pronto para revisão humana. Assim, é possível delegar tarefas repetitivas, acelerar o desenvolvimento e deixar as pessoas desenvolvedoras livres para se concentrarem em questões mais amplas.

Com o agente na nuvem do Copilot, você vai explorar:

- a personalização do ambiente para geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de criar issues com escopo claro.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívidas técnicas que gostaria de resolver. As pessoas contratadas para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre jogos relacionados — outros títulos da mesma categoria — para que as pessoas possam continuar navegando. Ainda não é necessária uma implementação refinada; a equipe só quer algo que possa usar para testes de aceitação da experiência. Isso está bloqueando o trabalho, mas há issues de prioridade maior no momento.

Esses são exemplos de tarefas que podem acabar sendo adiadas e que são ótimas para atribuir ao agente na nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto a pessoa desenvolvedora se concentra em outras tarefas e, mais tarde, revisa o trabalho do Copilot para confirmar que está tudo certo.

## Conheça o agente na nuvem do GitHub Copilot

O [agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano, de maneira semelhante a uma pessoa desenvolvedora. Assim como acontece no trabalho com uma pessoa desenvolvedora, há várias maneiras de usar esse recurso, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que você atribuir uma issue, o Copilot criará um pull request de rascunho para acompanhar o progresso, configurará um ambiente e começará a trabalhar na tarefa. Você pode examinar a sessão do Copilot enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request!

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot: não dá para pular a etapa de pensar. Até tarefas aparentemente simples ficam complexas quando você as analisa em detalhes. Por isso, [defina cuidadosamente o escopo das tarefas destinadas ao agente na nuvem do Copilot][cloud-agent-best-practices]. Trate-o como uma pessoa parceira de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a adoção da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e mostraram como elas orientam o Copilot. Se você já concluiu a atividade prática sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente na nuvem do Copilot, examine somente os arquivos de instrução já incluídos neste repositório para poder observar seu efeito mais tarde.

Abra os seguintes arquivos na interface web do GitHub para seu repositório ou em um codespace, caso já tenha um em execução:

- `.github/copilot-instructions.md` - Revise a seção **Code standards**, especialmente as expectativas para convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` - Observe o frontmatter `applyTo`, que associa estas instruções a arquivos `**/*.test.ts`.

Ao atribuir ao agente na nuvem a issue `Code lacks documentation` na próxima seção, observe se o pull request resultante inclui comentários TSDoc, convenções TypeScript e cabeçalhos de comentário. Essas orientações vêm desses arquivos de instrução. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento para o agente na nuvem do Copilot

A criação de código, independentemente de quem está envolvido, normalmente exige um ambiente específico e a execução de alguns scripts de configuração para garantir que tudo esteja em boas condições. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow especial de configuração][setup-workflow], configurado no arquivo `.github/workflows/copilot-setup-steps.yml`, que é executado antes de o agente começar a trabalhar. Assim, ele terá acesso às ferramentas de desenvolvimento e dependências necessárias. Esse workflow já foi configurado para facilitar o andamento do laboratório e permitir essa experiência. Ele garante que o Copilot tenha acesso ao Node.js, às dependências do projeto, aos binários do navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados do aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Para clonar o repositório durante as etapas de configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente quando elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que os builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns detalhes importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se o workflow funciona corretamente, sem esperar que o Copilot o execute.

## Adicione documentação

Embora todas as pessoas reconheçam a importância da documentação, a maioria dos projetos tem informações desatualizadas ou não tem documentação alguma. Esse tipo de dívida técnica costuma ser ignorado, reduzindo a produtividade e dificultando a manutenção da base de código ou a integração de novas pessoas à equipe. Felizmente, o Copilot é muito eficiente para criar documentação, então essa é uma issue ideal para atribuir ao agente na nuvem do Copilot. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir o diálogo de nova issue.
4. Selecione **Blank issue** para criar a issue.
5. Defina **Title** como `Code lacks documentation`.
6. Defina **Description** como:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir o diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request recém-gerado (PR), que terá um título semelhante a `[WIP]: Code lacks documentation`. Se nenhum PR novo aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está criando o ambiente (executando o workflow mencionado anteriormente), analisando o projeto e determinando a melhor abordagem para resolver a tarefa.

13. Revise a lista e as tarefas que serão concluídas.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão ao vivo e acompanhar como o Copilot resolve o problema. Isso inclui explorar o código e entender seu estado, pausar para refletir e decidir a abordagem adequada e criar código.

Isso provavelmente levará alguns minutos. Um dos principais objetivos do agente na nuvem do Copilot é executar tarefas de forma assíncrona, deixando você livre para se concentrar em outras coisas. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente na nuvem do Copilot e, enquanto isso, voltando nossa atenção à criação de funcionalidades para o aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes dos jogos

Como vimos, uma das grandes vantagens do agente na nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele cuida de outro. Adicionar uma seção de jogos relacionados para a equipe de design talvez não leve muito tempo, mas esse tempo poderia ser usado em outras tarefas. Vamos atribuí-la ao agente na nuvem do Copilot!

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir o diálogo de nova issue.
4. Selecione **Blank issue** para usar o template em branco.
5. Defina **Title** como `Show related games on the game details page`.
6. Defina **Description** como:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir o diálogo de atribuição.

  ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Logo depois, você deverá ver alguns 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar!

![Emoji de olhos indicando que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o Copilot está trabalhando na segunda solicitação! Como o agente na nuvem do Copilot funciona de maneira semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo ativamente. Você pode revisar o trabalho quando ele terminar. Vamos nos concentrar agora em criar e usar agentes personalizados.

## Resumo e próximos passos

Esta lição apresentou o [agente na nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que sejam resolvidas de forma assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código de um framework para outro.

Você explorou estes conceitos:

- a personalização do ambiente para geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de criar issues com escopo claro.
- como atribuir issues ao Copilot.

Enquanto o agente na nuvem trabalha em segundo plano, vamos nos concentrar na criação e no uso de agentes personalizados. [O agente na nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso às instruções personalizadas que exploramos nos módulos anteriores.

## Recursos

- [Sobre o agente na nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente na nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
