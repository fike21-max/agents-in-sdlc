---
title: "Lição 4 - Oriente o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica, você quer que essa orientação esteja disponível para o Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *qual* código você quer, mas também *como* ele deve ser estruturado. Agora que você desenvolveu a filtragem, vai explorar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao seu código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instrução associados a caminhos específicos chegam ao agente.
- atualizar o arquivo de instruções para garantir o cumprimento dos padrões de código.
- observar o impacto dos arquivos de instrução no código.

## Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e as decisões que não são óbvias, em vez de apenas repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, os parâmetros e os valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Os componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem permanecer atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Com os arquivos de instrução, você garantirá que o Copilot tenha as informações necessárias para realizar tarefas de acordo com as práticas destacadas.

## Arquivos de instrução

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de programação e seus requisitos. Esse recurso ajuda a orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de código, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo.

Há dois tipos de arquivos de instrução:

- `.github/copilot-instructions.md`, um único arquivo de instruções enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações relevantes para a maioria das solicitações relacionadas ao projeto.
- `.github/instructions/*.instructions.md`, arquivos que fornecem diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de usar um formato específico.

## Explore os arquivos de instrução personalizados deste projeto

Para começar, o projeto inicial já inclui um conjunto de arquivos de instrução. Vamos explorá-los antes de fazer uma mudança e observar seu efeito.

1. Volte ao Codespace.
2. No editor do Codespaces (não no terminal), abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de programação. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo — ele define um glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instrução e arquivos existentes do projeto. Isso permite dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis, além de apontar o Copilot para exemplos.

## Atualize os arquivos de instrução para corresponder às diretrizes da equipe

Os arquivos existentes são um bom começo, mas ainda falta uma orientação. Vamos modificar o arquivo principal `copilot-instructions.md` para que o TSDoc seja incluído em novos arquivos TypeScript.

1. Em `.github/copilot-instructions.md`, localize a seção intitulada **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione o seguinte como o último item:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente!

## Use as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa é iniciada. Depois de salvar as instruções atualizadas, saia e retome a conversa de filtragem para que o Copilot comece a usar as novas orientações sem perder o contexto da funcionalidade.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para fechá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, no branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que o branch `game-filters-cli` está selecionado.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os comentários TSDoc recém-gerados e confirme que descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot usa as instruções do repositório e aplicou um novo padrão à sua funcionalidade. Especificamente, você:

- explorou como as instruções do repositório e os arquivos de instrução associados a caminhos específicos chegam ao agente.
- atualizou o arquivo de instruções para garantir o cumprimento dos padrões de código.
- observou o impacto dos arquivos de instrução no código.

Em seguida, você vai [personalizar e executar a skill reutilizável `quality-checks`][next-lesson] para garantir a execução consistente do lint e dos testes.

## Recursos

- [Adicionar instruções personalizadas para o Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instrução e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
