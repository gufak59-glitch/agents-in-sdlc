---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações — se houver alguma — não enfrentam dificuldades com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que ficaram no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi desenvolvido para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request de rascunho para que uma pessoa desenvolvedora revise o trabalho. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em assuntos mais amplos.

Neste exercício, você vai explorar:

- como personalizar o ambiente de geração de código;
- como manter as operações seguras;
- por que é importante definir issues com escopo claro;
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívida técnica a resolver. As pessoas contratadas inicialmente para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, você verá que ela está completamente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas da aplicação.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer que a página de detalhes de cada jogo mostre jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar para testes de aceitação da experiência. Isso é um bloqueio no momento, mas há outras issues de prioridade mais alta.

Esses são exemplos de tarefas que podem ser rapidamente despriorizadas e que são ótimas candidatas para o agente de nuvem. Ele pode trabalhar nelas de forma assíncrona enquanto você se concentra em outras tarefas; depois, você volta para revisar o trabalho e confirmar que está tudo como esperado.

## Conhecer o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot][copilot-agents] executa tarefas em segundo plano, de forma semelhante a uma pessoa desenvolvedora. Você pode [atribuir uma issue do GitHub ao Copilot][assign-issue]. O Copilot cria um pull request de rascunho, prepara o ambiente e começa a trabalhar. Você pode acompanhar a sessão durante a execução ou depois. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções com escopo claro

Não há mágica: ainda é preciso pensar no problema. Mesmo tarefas aparentemente simples podem ter detalhes complexos. Siga as [boas práticas para definir o escopo do trabalho do agente de nuvem][cloud-agent-best-practices] e trate-o como uma parceria de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; se não, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções incluídos neste repositório para observar o efeito deles depois.

Abra os arquivos a seguir na interface Web do GitHub para seu repositório ou em um codespace, se já houver um em execução:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, especialmente as expectativas sobre convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica essas instruções a arquivos `**/*.test.ts`.

Quando você atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Eles vêm desses arquivos de instruções; voltaremos a esse ponto ao revisar os PRs em uma lição posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

O agente de nuvem usa [GitHub Actions][github-actions] para preparar o ambiente. Você pode personalizá-lo com o workflow especial `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. O projeto já inclui essa configuração: ela disponibiliza Node.js, dependências, navegadores para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido da aplicação Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias às suas etapas. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se as etapas precisarem clonar o repositório, por exemplo para instalar dependências, será necessária a permissão contents: read. Se você não clonar o repositório, o Copilot fará isso automaticamente depois que as etapas de configuração terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend e do aplicativo: Node.js (o aplicativo agora é Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse workflow contém um job chamado `copilot-setup-steps`, executado antes de o Copilot começar. O gatilho `workflow_dispatch` permite testá-lo manualmente na guia **Actions** do repositório.

## Adicionar documentação

Documentação desatualizada ou ausente é uma forma comum de dívida técnica: ela reduz a produtividade e dificulta a manutenção do código e a integração de novas pessoas à equipe. O Copilot pode gerar essa documentação em segundo plano; você revisará o trabalho na próxima lição.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, insira `Code lacks documentation`.
6. No campo **Description**, insira:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. No painel à direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o novo pull request (PR), que terá um título semelhante a `[WIP]: Code lacks documentation`. Se o PR ainda não aparecer, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente com o workflow descrito acima, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do PR para encontrar a atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Explore a sessão ao vivo e acompanhe como o Copilot examina o código e o estado do projeto, planeja o trabalho e cria código.

Isso pode levar vários minutos. Uma das principais vantagens do agente de nuvem é executar tarefas de forma assíncrona enquanto você trabalha em outra atividade. Aproveite esse recurso atribuindo outra tarefa e, em seguida, volte sua atenção para a aplicação.

## Adicionar uma seção de jogos relacionados à página de detalhes

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é dividir o trabalho: enquanto você se concentra em um conjunto de tarefas, ele cuida de outro. Talvez a seção de jogos relacionados para a equipe de design não leve muito tempo, mas ainda assim é tempo que poderia ser usado em outras atividades. Vamos atribuí-la ao agente de nuvem do Copilot.

1. Volte ao repositório no github.com.
2. Selecione **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, insira `Show related games on the game details page`.
6. No campo **Description**, insira:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. No painel à direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Pouco depois, você verá um emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar. O agente está tratando sua segunda solicitação; como ele trabalha de forma assíncrona, não é necessário monitorá-lo continuamente — revise o resultado quando estiver pronto. Agora, conheça os agentes personalizados.

## Resumo e próximos passos

Esta lição apresentou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona. O Copilot pode ajudar a resolver dívida técnica, criar funcionalidades ou migrar código entre frameworks.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código;
- garantir que as operações sejam realizadas com segurança;
- entender a importância de definir issues com escopo claro;
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, agora você pode se concentrar em criar e usar agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e instruções personalizadas, que foram abordadas nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
