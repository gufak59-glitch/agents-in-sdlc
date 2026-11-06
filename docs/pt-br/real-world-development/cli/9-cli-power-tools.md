---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos de barra para contexto, seleção de modelos, compartilhamento de sessões e delegação opcional à nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como outras ferramentas de CLI, o GitHub Copilot CLI oferece comandos de barra para acessar recursos avançados, informações e configurações. Você já usou `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Nesta lição, explore `/context`, `/compact`, `/model` e `/share`, além de opções para trabalho paralelo e delegação.

## Cenário

O fluxo principal do workshop terminou. Agora você conhecerá recursos para gerenciar contexto, alternar modelos, compartilhar sessões e, opcionalmente, delegar trabalho ao [agente de nuvem do Copilot][about-cloud-agent].

## Explorar o contexto do Copilot CLI

Em tarefas complexas, talvez você se aproxime do limite da janela de contexto. O Copilot CLI compacta a conversa automaticamente, mas você também pode consultar e compactar o contexto manualmente.

1. No codespace, inicie o Copilot CLI na raiz do repositório, se necessário.
2. Execute `/context` e observe o modelo, o uso de tokens e a distribuição do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
3. Execute `/compact` e depois `/context` novamente para comparar.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente à medida que a janela se preenche. Use `/compact` para escolher o momento; use `/clear` ou `/new` para iniciar uma conversa nova ao mudar para outra tarefa.

## Escolher um modelo

1. Execute `/model`.
2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhar uma sessão

O comando `/share` pode exportar uma sessão como arquivo Markdown ou HTML, criar um link compartilhável ou publicar um GitHub gist.

1. Execute `/help` e confira as opções de `/share` na versão instalada.
2. Se quiser compartilhar a sessão, escolha o destino apropriado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo ou publicá-lo; ele pode incluir prompts, respostas e detalhes do projeto.

Publicar um link ou gist é opcional. Não compartilhe conteúdo do repositório ou da conversa que a equipe não queira divulgar.

## Opcional: ampliar o trabalho ou delegar

- `/fleet` divide tarefas independentes entre subagentes e pode executá-las em paralelo.
- `/worktree` cria uma worktree Git isolada para outra tarefa.
- `/delegate` envia uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um PR.

Esses comandos são opcionais, pois podem criar worktrees adicionais ou trabalho remoto. Comece uma tarefa nova com escopo claro e revise os resultados pelo fluxo normal. Para explorar o trabalho assíncrono, continue com o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Você usou `/context` e `/compact`, explorou os modelos com `/model`, conheceu as opções de `/share` e revisou comandos opcionais para trabalho paralelo, worktrees e delegação.

Em seguida, [revise o que aprendeu][next-lesson] e veja os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
