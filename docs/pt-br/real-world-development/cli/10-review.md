---
title: "Lição 10 - Encerramento e próximas etapas"
description: "Recapitule o fluxo de trabalho da CLI do Copilot, dois pull requests, personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou a CLI do GitHub Copilot em um fluxo contínuo de trabalho no Tailspin Toys. Você:

- preparou um Codespace, instalou a CLI do Copilot, explorou o projeto e encontrou a issue de filtragem já criada;
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e mesclou manualmente seu primeiro pull request (PR);
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou-a no modo Autopilot e revisou o resultado no modo Interactive;
- orientou o agente com instruções personalizadas, personalizou a habilidade `quality-checks` existente e a usou para executar testes de unidade, lint e verificações de tipos;
- adicionou o servidor MCP do Playwright e o usou para explorar a filtragem em um navegador real;
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da habilidade e evidências do navegador;
- revisou toda a alteração de filtragem e autorizou o Agent Merge para o PR correspondente;
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria ramificação criada a partir da `main` atualizada:

1. **Avaliações por estrelas:** exibir a classificação existente `starRating` e um estado explícito para jogos sem classificação nos cartões dos jogos.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las à funcionalidade, personalizar o relatório `quality-checks`, criar um perfil de QA e incluir os testes associados.

Desde o planejamento da filtragem até a abertura do PR, você usou a mesma conversa e a mesma ramificação. Reunimos esse trabalho em um PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias maneiras: testes automatizados, sua própria inspeção no navegador e a exploração do navegador pelo Copilot usando MCP. A habilidade `quality-checks` executou testes de unidade, lint e verificações de tipos e apresentou os resultados no novo formato. O agente de QA reuniu esses resultados com uma revisão dos requisitos e da cobertura dos testes antes do PR.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que não exige novos testes também pode estar correta. Revise o código e as evidências antes de autorizar a mesclagem e atualize as evidências afetadas depois de fazer alterações.

## Práticas recomendadas

O contexto e as ferramentas que você fornece influenciam o trabalho do Copilot. Neste workshop, você atualizou instruções, personalizou uma habilidade, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações em várias conversas e ajuste-as conforme as necessidades da equipe mudarem. As instruções definem padrões, as habilidades descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para pensar em uma abordagem antes de começar, **Interactive** para acompanhar alterações específicas e **Autopilot** para tarefas bem definidas. Escolha um modelo mais rápido para edições rotineiras e um mais capaz para trabalhos complexos.

O contexto continua sendo tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer desenvolver, *por que* e *como* altera significativamente o resultado.

## Mais para explorar

Você aprendeu o fluxo de trabalho principal. Confira também alguns outros recursos da CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações;
- `/rubber-duck` para discutir um problema e obter outra perspectiva;
- `/fleet` para orquestrar subtarefas independentes em paralelo;
- `/worktree` para isolar uma tarefa separada;
- `/delegate` para enviar uma tarefa ao agente na nuvem do Copilot.

## Próximas etapas

A melhor maneira de aprender a usar qualquer ferramenta é continuar usando-a! Use-a para código de produção, projetos pessoais ou aquele pequeno aplicativo que você imagina há anos, mas nunca chegou a desenvolver. Compartilhe o que aprendeu com a equipe e aprenda com as experiências dos colegas. E, como sempre, explore a documentação.

Se quiser ampliar o Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop de VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente na nuvem do Copilot][cloud].

## Recursos

- [Sobre a CLI do GitHub Copilot][about-cli]
- [Referência de comandos da CLI do Copilot][cli-reference]
- [Personalizar a CLI do Copilot][customize-cli]
- [Gerenciar pull requests com a CLI do Copilot][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
