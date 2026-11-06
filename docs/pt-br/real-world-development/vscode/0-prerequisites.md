---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare tudo. Você criará uma cópia do repositório Tailspin Toys, iniciará um [codespace][codespaces] e confirmará que o Copilot Chat está funcionando no editor.

## Configurar o repositório do laboratório

Crie uma cópia do repositório a partir do [modelo][template-repository], que contém os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo da Tailspin Toys][tailspin-template].
2. Selecione **Use this template** e, em seguida, **Create a new repository**.

   ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

   ![Configurações do repositório criado a partir do modelo](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (`organization-or-user-name/repository-name`), pois você o usará mais adiante.

> [!NOTE]
> **O backlog está pronto**
>
> Ao criar o repositório a partir do modelo, um backlog de issues do GitHub é criado automaticamente. Você usará essas issues no workshop; não precisa criar nenhuma.

## Criar um codespace

Você usará um codespace para concluir os exercícios.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um IDE completo com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório criado.
2. Selecione o botão verde **Code**.

   ![Selecione o botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e, em seguida, o botão **+** para criar um codespace.

   ![Criar um codespace](../../../_images/ex0-create-codespace.png)

A criação levará alguns minutos, mas será bem mais rápida do que instalar os serviços manualmente. Enquanto isso, explore outros recursos do GitHub Copilot.

> [!CAUTION]
> Você voltará ao codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop funciona em um codespace ou em um [contêiner de desenvolvimento local][dev-containers], que fornece os pré-requisitos. Para trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Usar o GitHub Copilot Chat e o modo de agente

Para usar o modo de agente do Copilot Chat, instale a extensão GitHub Copilot Chat no IDE. Ela já deve estar disponível se você estiver usando um codespace.

> [!TIP]
> Se a extensão não estiver instalada, [instale-a pelo Marketplace do Visual Studio Code][copilot-chat-extension] ou abra **Extensions**, pesquise **GitHub Copilot Chat** e selecione **Install**.

Talvez seja necessário autenticar sua conta do GitHub para habilitar a extensão.

1. Volte ao codespace.
2. Se o Copilot Chat não aparecer à direita do editor, selecione o ícone **Copilot Chat** na parte superior do codespace.
3. Digite uma mensagem, como `Hello world`, e pressione <kbd>Enter</kbd>.
4. Se solicitado, siga as instruções para entrar na sua conta do GitHub.
5. Após a autenticação, a janela do Copilot Chat deverá aparecer.

   ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

## Resumo

Você criou uma cópia do repositório do laboratório e iniciou o codespace que usará para desenvolver.

## Próxima etapa

Vamos começar a usar o Copilot. Continue para o [Exercício 1: Instruções personalizadas][next-lesson], no qual você ensinará ao Copilot as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
