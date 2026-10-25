---
title: "Exercício 2 - Adicionar novas funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo as atualizações mais simples de um aplicativo costumam exigir alterações em vários arquivos e operações como executar testes. Como desenvolvedor, seu fluxo normalmente envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando, fazer outra atualização... E assim por diante.

É aqui que entra o modo de agente do Copilot.

O modo de agente do Copilot foi criado para trabalhar de forma mais autônoma no IDE. Ele age de maneira semelhante a um desenvolvedor: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os erros encontrados. Vamos explorar como você pode usar o modo de agente para adicionar funcionalidades ao seu site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente na nuvem foram criados para experiências diferentes. O modo de agente executa tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente na nuvem foi projetado para atuar como um programador da equipe, trabalhando de forma assíncrona e interagindo com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar seu projeto, identificar arquivos relevantes e coordenar alterações;
- o modo de agente do GitHub Copilot pode implementar novas funcionalidades na interface e na camada de dados;
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem os jogos por categoria e editora. Você já adicionou um helper de editoras no exercício anterior e agora concluirá o restante do trabalho na camada de dados, na interface e nos testes usando o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer qualquer alteração, vamos explorar o site Tailspin Toys para entender sua funcionalidade atual.

O site é uma plataforma de financiamento coletivo de jogos de tabuleiro com um tema para desenvolvedores. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza suas páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, por meio do Drizzle ORM — não há uma API de back-end separada nem uma estrutura de interface do lado do cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` Astro, com escopo definido e que usa as APIs padrão do DOM.

### Iniciar o site

Para facilitar a execução do site, foi disponibilizada a tarefa `npm run dev`, que inicia o servidor de desenvolvimento Astro. Você pode executá-la no GitHub Codespace seguindo estas etapas:

1. Volte ao codespace. Você deve estar na `main` depois do exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e alterne para uma nova ramificação para trabalhar na filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o comando a seguir para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um banner indicando a URL, semelhante ao exemplo abaixo:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionado e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar um link para a URL localhost no terminal do Codespace redirecionará você automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora está hospedando seu servidor Web!

### Explorar o site

Agora que o site está em execução, você pode explorar sua funcionalidade. Os principais recursos do site incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: quando você seleciona um jogo, é direcionado a uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam de um exercício para outro — cada etapa indicará o que selecionar quando isso for relevante.

A implementação inicial do site é funcional, mas queremos aprimorá-la com novos recursos. Vamos começar examinando o backlog. Quando você configurou o projeto a partir do template, um backlog de issues foi criado automaticamente — peça ao GitHub Copilot para exibir esses itens.

1. Selecione **Agent** na lista suspensa de agentes da exibição Chat. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos do terminal e invoca ferramentas.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão indicando preferência por um modelo em relação a outro. Ao criar o workshop, usamos o Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões de código sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como os LLMs são probabilísticos, talvez as sugestões recebidas sejam diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá conseguir concluir a tarefa.

3. Peça ao Copilot informações sobre o backlog de issues enviando o seguinte prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```
4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe que o Copilot até priorizou os itens com base no que considera mais útil para o usuário.

## Revisar os arquivos de instruções

Antes de iniciar o agente para gerar o código, é um bom momento para revisar o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contém orientações sobre como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais sobre como adicionar funcionalidades. Elas incluem:
   - uma visão geral da arquitetura;
   - princípios de design de componentes, testabilidade e acessibilidade;
   - links para arquivos de instruções específicos de vários tipos de arquivo, incluindo:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Os arquivos de instruções permitem fazer referência a outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Isso permite reutilizar conteúdo, dividir conjuntos maiores de instruções em partes menores e mais fáceis de gerenciar, além de indicar exemplos e templates para o Copilot.

## Implementar a funcionalidade de filtragem

Para concluir a filtragem, você precisará fazer pelo menos três atualizações distintas no aplicativo:

- adicionar ou aprimorar a lógica de filtragem restante na camada de dados (`src/lib/`);
- adicionar ou atualizar os testes do comportamento de filtragem;
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de mesclar tudo à base de código. O modo de agente do Copilot pode realizar essas tarefas por você. Vamos adicionar essa funcionalidade.

1. Você pode continuar a conversa atual com o Copilot ou iniciar uma nova selecionando **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela da seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista suspensa de agentes da exibição Chat.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue correspondente do backlog, usando o seguinte prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot começar explorando o projeto e localizando os arquivos associados à funcionalidade desejada. Ele deve encontrar os helpers da camada de dados, a interface e os testes. Em seguida, começará a modificar os arquivos e executar os testes.

   ![Captura de tela do Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Você verá o Copilot realizar várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante esse processo, talvez apareçam os controles **Keep** e **Undo** no editor. Quando o Copilot terminar, você verá os controles **Keep** ou **Undo** para todas as alterações; portanto, não é necessário selecioná-los enquanto o trabalho está em andamento.

7. Quando o Copilot solicitar, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat pedindo confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez ocorram pausas e até algumas falhas nos testes durante o processo. Tudo bem! O Copilot alterna entre a geração de código e a execução de testes até concluir a tarefa e não detectar erros.

   ![Captura de tela de uma sessão completa do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximas etapas

Parabéns! Neste exercício, exploramos como usar o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o modo de agente do GitHub Copilot pode implementar novas funcionalidades na interface e na camada de dados;
- o modo de agente do Copilot pode explorar seu projeto, identificar arquivos relevantes e coordenar alterações;
- revisar as alterações e os testes gerados pelo modo de agente do Copilot antes de mesclá-los à base de código.

Agora, vamos [testar sua funcionalidade com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — Implementar paginação

À medida que a lista de jogos cresce, será necessário implementar a paginação. Use as habilidades aprendidas neste exercício para pedir ao Copilot que atualize o site com paginação. Considere os seguintes pontos sobre o código:

- siga as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções disponíveis;
- pense em como deseja implementar a paginação: permitir que o usuário selecione o tamanho da página ou defini-lo no código;
- ao criar o prompt, forneça as orientações necessárias para que o Copilot implemente o que você deseja;
- talvez seja necessário iterar com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot!

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
