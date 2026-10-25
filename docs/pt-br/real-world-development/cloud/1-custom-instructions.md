---
title: "Exercício 1 - Instruções personalizadas (agente na nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente na nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou se há informações contextuais que o Copilot deve conhecer —, você deve garantir que esse contexto esteja acessível. Os [arquivos de instrução][instruction-files] fornecem essas orientações para que o Copilot entenda não apenas *o que* você quer que ele faça, mas também *como* deve fazê-lo.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instrução associados a caminhos específicos.
- adicionar um novo padrão global do repositório a `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos ambientes do VS Code e do CLI, aqui você não executará um prompt *antes e depois*. O agente na nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado e em tempo real. Você verá a influência do arquivo de instruções mais adiante neste fluxo, ao revisar os pull requests produzidos pelo agente na nuvem.

## Arquivos de instrução

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter um visual moderno.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instrução, você garantirá que o Copilot tenha as informações necessárias para realizar tarefas de acordo com as práticas destacadas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso ajuda a orientar o Copilot para gerar sugestões mais relevantes. Você pode especificar convenções de código, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos de arquivo específicos.

Há dois tipos de arquivos de instrução:

- `.github/copilot-instructions.md`, um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações sobre o projeto — contexto relevante para a maioria das solicitações enviadas ao Copilot pelo chat ou pelo CLI. Isso pode incluir a pilha tecnológica, uma visão geral do projeto, práticas recomendadas e orientações gerais.
- `.github/instructions/*.instructions.md`, arquivos que podem ser criados para tarefas ou tipos de arquivo específicos. Eles podem fornecer diretrizes para linguagens específicas, como TypeScript ou Astro, ou tarefas como criar componentes de interface ou conjuntos de testes unitários.

> [!NOTE]
> No IDE, os arquivos de instrução só são usados para gerar código no Copilot Chat — não para completar código nem para sugerir a próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente na nuvem do Copilot usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` que têm frontmatter `applyTo`.
>
> O Copilot também [oferece suporte a arquivos de instrução em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instrução

Uma conversa completa sobre a criação de arquivos de instrução está fora do escopo deste workshop. No entanto, os exemplos do projeto ilustram uma abordagem representativa. Em linhas gerais:

- Mantenha as instruções em `copilot-instructions.md` focadas em orientações para todo o projeto, como a descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões gerais de código.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo (testes unitários, componentes Astro ou a camada de dados) ou tarefas específicas.
- Use linguagem natural. Mantenha as orientações claras e mostre exemplos de como o código deve (e não deve) ser escrito.

Não há uma única maneira certa de criar arquivos de instrução, assim como não há uma única maneira certa de usar IA. A experimentação ajudará você a descobrir o que funciona melhor para seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto robusto de arquivos de instrução. Ao explorar os arquivos deste projeto, talvez você encontre instruções para vários tipos de tarefas, incluindo [atualizações da interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instrução. A forma de fazer isso varia conforme a interface (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI) — a lição para a interface usada indicará quando isso for relevante.
>
> Quer encontrar templates ou um ponto de partida? Explore o [awesome-copilot][awesome-copilot], um repositório repleto de arquivos de instrução, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instrução personalizados deste projeto

Reserve um momento para ler os arquivos de instrução que acompanham este repositório: há um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Na seção **Code standards**, observe as orientações em **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros.
4. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo — ele define um glob (relativo à raiz do repositório) que determina quais arquivos recebem as instruções. Aqui, qualquer arquivo de teste TypeScript (por exemplo, um que corresponda a `**/*.test.ts`) será incluído.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Por fim, abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instrução, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. Os caminhos ali são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicione um novo padrão ao repositório

Agora você fará a única edição deste exercício: adicionará uma regra global do projeto para que a documentação seja incluída no código em comentários TSDoc e em um cabeçalho de comentário no arquivo. O agente na nuvem usará essa orientação ao trabalhar nas issues atribuídas a ele mais adiante neste fluxo.

Antes de editar, configure um branch de trabalho. O agente na nuvem lerá as instruções do branch de destino da issue, mas você fará o commit das mudanças de maneira organizada:

1. No terminal do codespace, crie um branch e selecione-o:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como mencionado anteriormente, `.github/copilot-instructions.md` foi criado para fornecer informações sobre o projeto ao Copilot. Vamos registrar os padrões de código do repositório para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe como ela documenta os padrões de código do projeto — ainda não há uma regra para documentação no código, por isso o helper gerado não tinha comentários de documentação.
3. Adicione as linhas Markdown a seguir logo abaixo dos padrões existentes para instruir o Copilot a adicionar cabeçalhos de comentário e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instrução podem ser criados na raiz do repositório (`.github/copilot-instructions.md`) para oferecer orientações gerais ou como arquivos `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicas. O arquivo na raiz do repositório é o lugar certo para padrões globais, como a regra de comentários de documentação que você acabou de adicionar.

## Faça commit, envie e mescle a atualização das instruções

O agente na nuvem lê os arquivos de instrução do branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará um branch a partir de `main`. Portanto, suas mudanças nas instruções precisam chegar a `main` para que o agente na nuvem possa usá-las.

1. Prepare e faça commit da mudança:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie o branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request do branch `custom-instructions-cloud` para `main` e faça a mesclagem. Assim, o agente na nuvem poderá ler essas instruções ao trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar a etapa do branch e fazer commit diretamente em `main`. A etapa do branch está aqui para que o workshop reflita a abordagem usada em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot usa as instruções do repositório e adicionou um novo padrão global a `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o agente na nuvem produzir no restante deste fluxo.

Em seguida, você vai [atribuir sua primeira issue ao agente na nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instrução para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instrução e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
