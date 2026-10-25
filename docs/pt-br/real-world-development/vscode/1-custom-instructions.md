---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma forma específica — ou se há informações de contexto que o Copilot deveria conhecer —, você precisa garantir que ele possa acessar esse contexto. Há várias maneiras de compartilhar informações específicas com o Copilot. Uma das principais são os [arquivos de instruções][instruction-files], que permitem fornecer orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de programação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos de instruções direcionados a caminhos;
- gerar a primeira parte dos dados para filtragem (um helper de editoras) com as instruções *atuais* em vigor;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- executar novamente o mesmo prompt e observar o código gerado adotar o novo padrão;
- confirmar as alterações nas instruções e na parte da funcionalidade de filtragem na `main` para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir alguns dos padrões definidos. O Copilot é não determinístico. O objetivo deste exercício é observar a *tendência* de mudança no comportamento após a atualização das instruções, não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

Como uma boa equipe de desenvolvimento, a Tailspin Toys tem um conjunto de diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes de unidade.
- A interface deve usar o modo escuro e ter um visual moderno.
- O código deve incluir documentação na forma de comentários TSDoc.
- Cada arquivo deve começar com um bloco de comentários que descreva sua função.

Com arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para realizar as tarefas de acordo com as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso pode ajudar a direcionar o Copilot para sugestões e trechos de código mais relevantes. Você pode especificar as convenções de programação e bibliotecas preferidas, além dos tipos de comentários que gostaria de incluir no código. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo, fornecendo contexto para cada tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas no repositório. Esse arquivo deve conter informações sobre o projeto — contexto relevante para a maioria das solicitações feitas ao Copilot Chat ou à CLI. Isso pode incluir a pilha de tecnologias, uma visão geral do que está sendo desenvolvido, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem ser criados para tarefas ou tipos de arquivo específicos. Você pode usá-los para fornecer diretrizes para linguagens específicas (como TypeScript ou Astro) ou para tarefas como criar um componente de interface ou um conjunto de testes de unidade.

> [!NOTE]
> No IDE, os arquivos de instruções são usados apenas para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, a CLI do Copilot e o agente na nuvem do Copilot usam tanto o arquivo de instruções do repositório quanto os arquivos `*.instructions.md` (com frontmatter `applyTo`) ao gerar código.
>
> O Copilot também aceita arquivos de instruções em outros padrões, como AGENTS.md e CLAUDE.md, conforme descrito em [suporte a arquivos de instruções personalizadas][custom-instructions-support].

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está fora do escopo deste workshop. No entanto, os exemplos do projeto demonstram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas em orientações para o projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de programação.
- Use arquivos `*.instructions.md` para instruções específicas a tipos de arquivo (testes de unidade, componentes Astro, camada de dados) ou a tarefas específicas.
- Escreva em linguagem natural. Mantenha as orientações claras e dê exemplos de como o código deve (e não deve) ser escrito.

Não há uma única maneira correta de criar arquivos de instruções, assim como não há uma única maneira de usar IA. A experimentação ajudará você a descobrir o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa GitHub Copilot deve ter um conjunto consistente de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá instruções para vários tipos de tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de uma forma diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` na CLI do Copilot) — a lição da interface que você está usando indicará quando isso for relevante.
>
> Quer encontrar templates ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Reserve um momento para ler os arquivos de instruções incluídos neste repositório — há um arquivo principal `copilot-instructions.md` e uma coleção de arquivos `*.instructions.md` para várias tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Essas orientações se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e explore seu conteúdo. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo — ele define um glob (relativo à raiz do repositório) que determina a quais arquivos as instruções se aplicam. Neste caso, qualquer arquivo de teste TypeScript (por exemplo, um que corresponda a `**/*.test.ts`) será incluído.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o fim. Observe os links para outros arquivos de instruções (como `unit-tests.instructions.md`) e para arquivos existentes no projeto. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir ao gerar código.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de programação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Criar uma ramificação para as alterações

Vamos seguir algumas práticas recomendadas e criar uma ramificação para as alterações.

1. Volte ao codespace do exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma nova ramificação e alterne para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção de modelo variam de um exercício para outro — cada etapa indicará o que selecionar quando isso for relevante.

Para observar o impacto das instruções personalizadas, comece enviando um prompt com o arquivo de instruções atual. Mais tarde, você o atualizará e enviará o mesmo prompt novamente para comparar os resultados.

1. Feche as guias do editor abertas nos exercícios anteriores para que o Copilot use apenas o contexto desejado.
2. Abra `src/lib/publishers.ts` para que o Copilot saiba onde o helper deve ficar.
3. Selecione **Agent** na lista suspensa de agentes da exibição Chat para que o Copilot possa aplicar alterações aos arquivos.

   ![Captura de tela do seletor de agente na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o seguinte prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará atualizações ao código, muitas vezes abrangendo o arquivo do helper e seus testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras — essa estrutura vem das convenções da camada de dados em `.github/instructions/drizzle.instructions.md` (aplicável a `src/lib/*.ts`).
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no nível do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários de documentação mesmo sem essa orientação. Se isso acontecer, tudo bem — o objetivo é observar a *consistência* após atualizar as instruções.

## Adicionar um novo padrão para o repositório

Como mencionado antes, `.github/copilot-instructions.md` foi criado para fornecer informações sobre o projeto ao Copilot. Vamos documentar os padrões de programação do repositório para melhorar as sugestões de código.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de programação do projeto — mas ainda não inclui uma regra para documentação no código, por isso o helper gerado não tinha comentários de documentação.
3. Adicione as linhas de Markdown a seguir logo abaixo dos padrões existentes para instruir o Copilot a adicionar cabeçalhos de comentário nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ser criados no nível do repositório (`.github/copilot-instructions.md`) para fornecer orientações globais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo do repositório é o lugar certo para padrões aplicáveis a todo o projeto, como a regra de comentários de documentação que você acabou de adicionar.

## Executar novamente o prompt e observar a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar a conversa.
2. Volte a `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie o **mesmo prompt** usado anteriormente:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que agora o arquivo proposto começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observe que agora a função proposta inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo a código real que servirá de base para o próximo exercício.

## Confirmar as instruções e enviar a ramificação

Os arquivos de instruções são como qualquer outro recurso do repositório: eles são gerenciados pelo mesmo processo de controle de versão. Portanto, vamos confirmar as alterações e enviar a ramificação ao repositório.

1. Abra uma nova janela de terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme se as alterações nas instruções e no helper estão presentes executando:

   ```bash
   git status
   ```

3. No terminal, prepare e confirme as alterações nas instruções e todos os arquivos modificados pelo Copilot para criar a estrutura básica do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar atualizações de suporte adicionais (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a ramificação ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e mesclar um pull request

Depois de enviar a ramificação, crie um pull request e associe-o à issue do backlog sobre os padrões de documentação. Você poderia fazer isso manualmente, mas pode pedir ao Copilot que encontre a issue, crie o PR para encerrá-la e faça a mesclagem.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub por você. O template do projeto já conecta as ferramentas do GitHub ao seu workspace, então não é necessário configurar nada — na primeira vez que o Copilot usar uma delas, o VS Code talvez solicite que você entre no GitHub. Siga as instruções para permitir o acesso. Em um exercício posterior, você aprenderá como essa conexão funciona (Model Context Protocol).

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme que **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que encontre a issue relacionada à atualização dos arquivos de instruções e crie um PR a partir da ramificação atual. Peça que a descrição inclua as atualizações das instruções e a nova estrutura básica do helper de editoras:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Quando o Copilot solicitar permissão para executar ações no GitHub em seu nome, revise o comando e selecione **Allow**, conforme apropriado.
5. Depois que o PR for criado, peça ao Copilot que o mescle e retorne sua ramificação para `main` usando o seguinte prompt:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. Quando o Copilot solicitar permissão para executar ações no GitHub e comandos do shell em seu nome, revise o comando e selecione **Allow**, conforme apropriado.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot!

## Resumo e próximas etapas

Você explorou como o Copilot obtém contexto dos arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*;
- adicionar um novo padrão para todo o repositório a `.github/copilot-instructions.md`;
- executar novamente o mesmo prompt e observar o código proposto adotar o novo padrão;
- confirmar as alterações nas instruções e na estrutura básica do helper na `main` para que o próximo exercício possa usá-las.

Agora, você usará essas instruções no [modo de agente][next-lesson] enquanto o Copilot adiciona uma nova funcionalidade à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas pessoais para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
