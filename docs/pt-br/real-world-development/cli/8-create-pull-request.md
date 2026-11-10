---
title: "Lição 8 - Criar e fazer merge da pull request (PR) do recurso"
description: "Revise a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie uma PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Agora você os revisará juntos e abrirá uma pull request (PR). Você fez o merge da PR das avaliações por estrelas manualmente; desta vez, deixará que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos o recurso, as atualizações das instruções e da skill e o agente de QA em algumas PRs separadas. Para simplificar o workshop, todo o trabalho de filtragem e qualidade ficou em uma conversa e branch e será incluído nesta PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionar toda a alteração do recurso e as evidências de verificação.
- criar a PR de filtragem.
- habilitar o Agent Merge somente após a revisão e confirmar que a PR foi mesclada.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora a Tailspin Toys quer automatizar o trabalho restante da PR, mantendo a autorização do merge sob o controle do desenvolvedor.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante para concluir uma pull request. Quando habilitado, o Copilot resolve os bloqueios da PR: corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário. Depois, habilita o merge automático do GitHub quando o repositório permite.

Até agora, você mesmo selecionava **Merge pull request**. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme que está pronto antes de habilitá-lo.

## Usar o Agent Merge para gerenciar a PR

Com todo o código criado, vamos revisar tudo, criar a PR e deixar o Agent Merge cuidar do restante.

1. Volte ao Codespace.
2. Abra a caixa de diálogo do agente digitando `/agent`.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie uma PR com o comando `/pr create`.
5. Ative o agent merge com `/pr agentmerge`.
6. O Copilot acompanhará a integração contínua da PR. Quando tudo for aprovado, fará o merge.
7. Confirme que aparece uma mensagem do Copilot semelhante a "PR #14 was squash-merged successfully."

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e conclua manualmente o merge revisado quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias partes do desenvolvimento: geração de código, testes e validação e, agora, o processo da pull request. Nesta lição, você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionou toda a alteração do recurso e as evidências de verificação.
- criou a PR de filtragem.
- habilitou o Agent Merge após a revisão e confirmou que a PR foi mesclada.

Em seguida, [explore outros comandos de barra úteis do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional na nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
