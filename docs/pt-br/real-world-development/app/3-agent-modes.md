---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para implementar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando um pequeno recurso ao projeto. Mudanças maiores exigem um processo mais robusto. O aplicativo GitHub Copilot foi desenvolvido para trabalhar com o fluxo existente de uma organização e ajudar a criar as soluções certas da maneira adequada. Nesta lição, você iniciará um processo comum de desenvolvimento orientado por agentes: usará uma issue para criar um recurso, verificará se o código e o comportamento estão corretos e, por fim, fará o merge no projeto.

> [!NOTE]
> Você continuará o fluxo de trabalho do recurso na mesma sessão. Em geral, você usaria sessões ou pull requests diferentes para cada tipo de arquivo, mas vamos simplificar para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma nova sessão de agente a partir de uma issue do GitHub.
- definir os requisitos no modo **Plan**.
- implementar o recurso no modo **Autopilot**.
- revisar o código.
- validar manualmente o recurso em um canvas de navegador.

Ao continuar esse trabalho, você atualizará as instruções do repositório, personalizará a skill `quality-checks` existente, adicionará validação com o Model Context Protocol (MCP), criará um agente de garantia de qualidade (QA) e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como combinar categorias precisam ser definidos antes de escrever o código. Você usará o modo Plan para decidir esses detalhes e, depois, autorizará uma implementação com escopo definido no modo Autopilot.

## Contexto

A introdução de agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, torna-os ainda mais importantes. A maioria das pessoas desenvolvedoras segue um fluxo semelhante:

1. Abra uma issue registrada com os detalhes do trabalho necessário.
2. Crie um plano do que precisa ser desenvolvido.
3. Desenvolva e revise o código.
4. Execute testes para validar o código.
5. Valide manualmente o novo recurso.
6. Crie um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, faça o merge.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos fluxos segue uma abordagem semelhante.

Seguir esse processo ajuda a garantir que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode selecioná-lo no menu suspenso abaixo do campo de prompt e alterá-lo quando quiser:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere mudanças e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro. Você o revisa e aprova antes de permitir que o agente o execute.
- **Autopilot**: o agente trabalha de forma autônoma, escreve código, executa testes e itera sem aguardar sua confirmação.

Comece no modo Plan, revise o plano e depois use o Autopilot para implementá-lo.

## Iniciar uma sessão a partir da issue

Confirme se o PR das avaliações por estrelas foi integrado e se a branch local `main` está atualizada antes de começar.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já existe no backlog criado pelo modelo; não crie outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme se a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente que execute `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique o PATH ou a configuração do gerenciador de versões do aplicativo e feche-o completamente antes de reabri-lo. Tente novamente a operação e as verificações com falha; não reinstale o npm apenas porque o agente não o encontrou.

## Planejar o recurso de filtragem

O planejamento permite revisar a abordagem antes de o Copilot escrever código. Como você começou pela issue, o Copilot já tem a solicitação do recurso como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme se ele abrange a filtragem por categoria e editora, controles acessíveis, mudanças na camada de acesso a dados e testes. Discuta comportamentos que não estejam claros, como combinar várias categorias ou o que acontece quando nenhum jogo corresponde aos filtros.

O plano deve incluir lint, testes unitários, testes de ponta a ponta (E2E) e verificação de tipos usando as ferramentas existentes do projeto. Mantenha o foco em implementar e testar a filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e mantenha à mão a URL da issue e os esclarecimentos acordados para validá-los depois.

## Aprovar explicitamente o Autopilot

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Não presuma que a aprovação muda o modo: confirme se o indicador mostra **Autopilot**.

O Copilot começará a trabalhar na implementação, seguindo o plano, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto, revise o plano primeiro. Se o Copilot informar dependências ausentes ou conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas. Interrompa somente os servidores que você iniciou.

## Revisar e verificar a implementação

Revise o código e execute o site para confirmar se tudo funciona antes de aceitar a implementação.

1. Abra **Changes** e examine a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo combinações de categorias e editoras. Verifique se as mudanças seguem as instruções do repositório.
3. Examine os resultados de lint, testes unitários, testes E2E e verificação de tipos. Uma verificação ignorada não conta como aprovação.
4. Corrija as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright compila e serve uma versão de pré-visualização e pode reutilizar um servidor local. Confirme se o servidor testado pertence a esta worktree, não a uma lição anterior.

## Explorar a nova funcionalidade

Com o código revisado, inicie o aplicativo e abra o site em um canvas de navegador.

1. Use o prompt a seguir para pedir ao Copilot que inicie o aplicativo e abra a página no canvas:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Aguarde o início do aplicativo e a abertura do navegador dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Limpe o filtro, selecione uma editora e confirme os resultados.
4. Combine filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado definido no plano.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme se a lista completa volta a aparecer.
6. Quando terminar, use este prompt para pedir ao Copilot que interrompa o servidor de desenvolvimento iniciado para a sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Manter esta sessão para as próximas lições

Ainda não crie nem integre o PR de filtragem. Mantenha esta sessão, worktree e branch para as próximas lições. Você aplicará instruções personalizadas, adicionará melhorias à skill e ao agente de QA e, depois, criará e integrará o PR combinado. Após interromper o servidor de desenvolvimento, deixe a sessão inativa e retome-a na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma nova sessão de agente a partir de uma issue do GitHub.
- definiu os requisitos no modo **Plan**.
- implementou o recurso no modo **Autopilot**.
- revisou o código.
- validou manualmente o recurso em um canvas de navegador.

Em seguida, explore como o código é gerado e siga as práticas documentadas [usando instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
