---
title: "Lição 8 - Criar e mesclar o PR da funcionalidade"
description: "Revise a filtragem, as instruções, a skill, o perfil de QA e os testes; depois crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Sua implementação de filtragem, as instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Agora, revise tudo em conjunto e abra um pull request (PR). Desta vez, use o **Agent Merge** para gerenciar o processo.

> [!NOTE]
> Normalmente, a funcionalidade, as instruções, a skill e o agente de QA poderiam ser divididos em vários PRs. Para simplificar o workshop, todo o fluxo de filtragem e qualidade está em uma conversa e branch.

Nesta lição, você conhecerá o Agent Merge, revisará as alterações e as evidências de verificação, criará o PR e habilitará o Agent Merge somente depois da revisão.

## Cenário

Você usou o Copilot para planejar, implementar e verificar a filtragem. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização de merge sob seu controle.

## Conhecer o Agent Merge

O **Agent Merge** automatiza o trabalho necessário para integrar um pull request. Quando habilitado, o Copilot resolve bloqueios, como falhas de integração contínua (CI), comentários de revisão e necessidade de rebase, e habilita o auto-merge do GitHub quando o repositório permite.

Até agora, você selecionou **Merge pull request** manualmente. Revise o trabalho e confirme que está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar o PR

1. Volte ao codespace.
2. Execute `/agent`, selecione **Default** e pressione <kbd>Enter</kbd>.
3. Crie o PR:

   ```plaintext
   /pr create
   ```

4. Ative o Agent Merge:

   ```plaintext
   /pr agentmerge
   ```

5. O Copilot acompanhará a CI e fará o merge quando as verificações forem aprovadas. Confirme a mensagem de conclusão, por exemplo, `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteção de branch, filas de merge, configurações do repositório nem permissões ausentes. Se houver bloqueio, leia o motivo e conclua manualmente o merge revisado quando permitido.

## Resumo e próximos passos

Você revisou a alteração completa e as evidências, criou o PR de filtragem e habilitou o Agent Merge após a revisão.

Em seguida, [explore outros comandos de barra do Copilot CLI][next-lesson] para contexto, modelos, compartilhamento e delegação opcional à nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
