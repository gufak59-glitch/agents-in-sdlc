---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência e adicionar funcionalidades: documentar o código, criar uma seção de jogos relacionados e implementar recursos de acessibilidade, como alternâncias para os modos de alto contraste e claro. Agora, revise as alterações e, se necessário, envie comentários ao Copilot.

### Cenário

Os fundamentos de design de software e DevOps continuam valendo quando se usa IA generativa. Revise o código gerado e siga o processo normal de DevOps. Antes de habilitar a revisão para o restante da equipe, examine as sugestões de documentação, jogos relacionados e acessibilidade.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão, há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e pode gravar somente na própria branch.
- O agente trabalha no GitHub Actions em um ambiente separado e temporário.
- Uma pessoa precisa aprovar a execução de workflows do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão][agent-firewall], inclusive para servidores MCP.

## Revisar a documentação gerada

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o PR do Copilot para adicionar documentação. O título pode ser semelhante a **Add missing documentation**.

> [!NOTE]
> O PR pode mostrar um aviso de que o firewall está bloqueando o GitHub Copilot. Isso é **esperado**, pois o Copilot tem acesso limitado a recursos externos por padrão, incluindo chamadas a servidores MCP. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

4. Se o Copilot ainda estiver trabalhando, o PR estará marcado como **[WIP]**. Aguarde a conclusão.
5. Quando o PR estiver pronto, selecione **Files changed** e revise as alterações.
6. Examine os comentários TSDoc e a documentação adicionada. As alterações variam; procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário, conforme os arquivos de instruções personalizados vistos no início do Exercício 2.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

7. Volte à guia **Conversation** e role a página para baixo. Se houver indicação de workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Confirme que os workflows foram adicionados à seção de verificações do PR. As verificações do projeto para a aplicação Astro devem passar; isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencionar `@copilot` em comentários do PR ou em comentários de linha do código. O Copilot verá o comentário e iniciará outra sessão para atendê-lo. Como os resultados são não determinísticos, não há um prompt único a recomendar. Por exemplo, você pode pedir:

- um cabeçalho com uma breve descrição no início de cada arquivo de código;
- comentários TSDoc em arquivos TypeScript e Astro;
- um README que descreva a estrutura da aplicação Astro.

1. Adicione um comentário mencionando `@copilot` e solicite uma alteração na documentação gerada.
2. Selecione **View Session** para acompanhar o trabalho do Copilot e observe que ele inicia uma nova sessão para aplicar as alterações.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Retornar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, procure o novo commit no PR e selecione **Files changed** para revisar as mudanças.
5. Continue iterando até ficar satisfeito. Depois, converta o PR de rascunho em pronto para revisão e faça o merge na branch principal.

   ![Converter o PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar a funcionalidade de jogos relacionados

1. Volte ao repositório no GitHub.com.
2. Selecione **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** para revisar o código gerado.
5. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
6. Se houver indicação de workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

7. Confirme que os workflows foram adicionados à seção de verificações do PR e que as verificações do projeto para a aplicação Astro passaram. Isso pode levar alguns minutos.
8. **Opcional:** faça um teste manual no codespace. Abra um terminal e execute os comandos a seguir, substituindo `<branch-name>` pela branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot concluiu a funcionalidade de jogos relacionados. Você pode pedir alterações pelo comentário na guia **Conversation**; por exemplo, ajuste a quantidade de jogos exibidos ou solicite cabeçalhos de comentário e comentários TSDoc. Lembre-se de que essa issue foi atribuída **antes** da atualização das instruções personalizadas. O Copilot iniciará outra sessão para atender a novos pedidos.

## Revisar os recursos de acessibilidade

O PR do agente personalizado deve conter o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de agentes no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR estará marcado como **[WIP]**. Aguarde a conclusão; isso pode levar alguns minutos.

4. Selecione **Files changed** para revisar o código gerado.
5. Examine a implementação, com atenção especial a:
   - componentes de interface para alternar entre os modos;
   - armazenamento local que mantém as preferências da pessoa usuária;
   - alterações de CSS ou estilo para os modos de alto contraste e claro;
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado;
   - código JavaScript/TypeScript que controla a alternância dos modos.
6. Depois da revisão, volte à guia **Conversation** e role a página para baixo.
7. Se houver indicação de workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Confirme que os workflows foram adicionados à seção de verificações do PR e que as verificações do projeto para a aplicação Astro passaram. Isso pode levar alguns minutos.
9. **Opcional:** teste manualmente os recursos de acessibilidade no codespace, substituindo `<branch-name>` pela branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicie o aplicativo e teste os modos de alto contraste e claro no navegador. Confirme também que as preferências continuam ativas após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar esses recursos seguindo boas práticas de acessibilidade. Se identificar problemas ou oportunidades de melhoria, mencione `@copilot` em um comentário para solicitar atualizações, como nos PRs anteriores.

## Exercício opcional — continuar delegando

Para praticar, crie issues no repositório e atribua-as ao agente. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo;
- implementar paginação na lista de jogos;
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo das lições, você:

- **Examinou os arquivos de instruções personalizadas** incluídos no repositório para observar o efeito deles nos resultados do agente de nuvem.
- **Atribuiu issues ao agente de nuvem do Copilot** e acompanhou a preparação do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando alternâncias para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como central de controle** para acompanhar e orientar a sessão de acessibilidade durante a execução.
- **Revisou e iterou sobre pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Se quiser continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu próprio repositório, experimente:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes;
- criar um agente personalizado para outro domínio;
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
