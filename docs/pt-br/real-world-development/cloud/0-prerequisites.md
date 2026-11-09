---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios sobre o agente de nuvem do Copilot, prepare o ambiente. Você criará sua própria cópia do repositório Tailspin Toys e iniciará um [Codespace][codespaces] para editar arquivos de instruções e revisar o trabalho do agente de nuvem.

## Configurar o repositório do laboratório

Para criar uma cópia do repositório onde trabalhará, crie uma instância a partir do [modelo][template-repository]. A nova instância conterá todos os arquivos necessários para o laboratório.

1. Em uma nova janela do navegador, acesse o [modelo da Tailspin Toys][tailspin-template].
2. Crie sua cópia do repositório selecionando o botão **Use this template** na página do repositório do laboratório. Em seguida, selecione **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando de um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Configurações do modelo do repositório](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você o usará mais adiante no laboratório.

## Criar um Codespace

Em seguida, use um Codespace para concluir os exercícios do laboratório.

[GitHub Codespaces][codespaces] é um ambiente de desenvolvimento na nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um ambiente de desenvolvimento integrado (IDE) completo, com suporte a várias linguagens, extensões e ferramentas.

1. Acesse o repositório recém-criado.
2. Selecione o botão verde **Code**.

    ![Botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e, em seguida, o botão **+** para criar um Codespace.

    ![Criar um Codespace](../../../_images/ex0-create-codespace.png)

A criação do Codespace levará alguns minutos, mas ainda será mais rápida do que instalar todos os serviços manualmente. Você pode aproveitar esse tempo para explorar outros recursos do GitHub Copilot que serão usados a seguir.

> [!CAUTION]
> Você voltará ao Codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop pode ser executado em um Codespace ou em um [contêiner de desenvolvimento][dev-containers] local. Ambos oferecem os pré-requisitos necessários. Para trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado. O VS Code criará o mesmo contêiner de desenvolvimento usado pelo Codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Resumo

Você criou uma cópia do repositório do laboratório e iniciou a criação do Codespace que usará enquanto trabalha com o agente de nuvem do Copilot.

## Próxima etapa

Adicione instruções personalizadas para orientar o agente de nuvem. Continue para [Exercício 1: Instruções personalizadas][next-lesson].

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um modelo][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
