---
title: "Lição 10 - Recapitulação e próximos passos"
description: "Revise o fluxo do Copilot CLI, as duas pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo de trabalho no Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem já criada.
- adicionou avaliações por estrelas, revisou o resultado em um navegador encaminhado e fez merge manual da primeira pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, implementou no Autopilot e revisou no Interactive.
- orientou o agente com instruções personalizadas, personalizou a skill `quality-checks` existente e usou-a para executar testes unitários, lint e verificações de tipos.
- adicionou o servidor Playwright Model Context Protocol (MCP) e usou-o para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou toda a alteração de filtragem e autorizou o Agent Merge para a PR.
- explorou comandos de barra para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch criada a partir de `main` atualizado:

1. **Avaliações por estrelas:** exibir o `starRating` existente e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implementar a filtragem, atualizar as instruções e aplicá-las ao recurso, personalizar o relatório `quality-checks`, criar um perfil de QA e incluir os testes relacionados.

Do planejamento da filtragem até a abertura da PR, você usou a mesma conversa e branch. Para simplificar o workshop, reunimos esse trabalho em uma PR.

## Diferentes tipos de verificação

Você verificou o código de várias formas: testes automatizados, inspeção própria no navegador e exploração do navegador pelo Copilot via MCP. A skill `quality-checks` executou testes unitários, lint e verificações de tipos e relatou os resultados no novo formato. Antes da PR, o QA reuniu esses resultados com a revisão dos requisitos e da cobertura de testes.

Os testes adicionados devem preencher lacunas reais; uma execução de QA que não precisa de novos testes pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas após alterações.

## Práticas recomendadas

O contexto e as ferramentas fornecidos moldam o trabalho do Copilot. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e ajuste-as às necessidades da equipe. Instruções definem padrões, skills descrevem tarefas repetíveis, agentes personalizados definem funções especializadas e servidores MCP conectam ferramentas externas. Revise as alterações e os resultados das ferramentas, não apenas o resumo do agente.

Escolha o **modo e o modelo** de acordo com a tarefa. Use **Plan** para pensar na abordagem antes de implementar, **Interactive** para acompanhar alterações focadas e **Autopilot** para tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e um mais capaz para trabalhos complexos.

O contexto é tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer criar, *por que* e *como* altera significativamente o resultado.

## Mais recursos para explorar

Você concluiu o fluxo principal. Outros recursos da CLI que valem a pena conhecer:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para explicar um problema e obter outra perspectiva.
- `/fleet` para coordenar subtarefas independentes em paralelo.
- `/worktree` para isolar uma tarefa.
- `/delegate` para enviar uma tarefa ao Copilot cloud agent.

## Próximos passos

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a: em código de produção, projetos pessoais ou naquele pequeno app que você pensa em criar há anos. Compartilhe o que aprendeu com a equipe e aprenda com os outros. E, como sempre, explore a documentação.

Para ampliar o Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do app GitHub Copilot][app] ou o [workshop do Copilot cloud agent][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
