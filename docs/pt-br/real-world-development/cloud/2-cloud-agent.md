---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Muitas organizações enfrentam dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou recursos que permanecem no backlog por falta de tempo. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele cria um pull request (PR) de rascunho para revisão por uma pessoa desenvolvedora. Assim, é possível delegar tarefas demoradas e se concentrar em prioridades maiores.

Neste exercício, você vai explorar como:

- personalizar o ambiente usado para gerar código.
- garantir que as operações sejam realizadas com segurança.
- criar issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer lidar com dois itens do backlog. Primeiro, adicionar comentários de documentação de TypeScript (TSDoc) às funções exportadas do aplicativo, pois o código atual não tem documentação suficiente. Além disso, a equipe de design quer mostrar jogos relacionados — outros títulos da mesma categoria — na página de detalhes de cada jogo. Por enquanto, a equipe quer uma implementação que permita testar a experiência; esse trabalho tem prioridade menor.

Esses são exemplos de tarefas que podem ser adiadas. O agente de nuvem do Copilot pode executá-las de forma assíncrona enquanto você trabalha em outras atividades e, depois, revisar o resultado.

## Sobre o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Você pode, por exemplo, [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, prepara o ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto o trabalho acontece ou depois de concluído. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções com escopo claro

O GitHub Copilot não elimina a necessidade de pensar. Até tarefas aparentemente simples podem ter complexidades. [Defina com cuidado o escopo do trabalho para o agente de nuvem][cloud-agent-best-practices]. Trate-o como uma ferramenta de programação em parceria: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Antes de atribuir trabalho, leia os arquivos de instruções incluídos no repositório para reconhecer a influência deles nos resultados.

1. Abra `.github/copilot-instructions.md` na interface Web do GitHub ou no Codespace e revise a seção **Code standards**, principalmente as orientações de TypeScript e comentários TSDoc.
2. Abra `.github/instructions/unit-tests.instructions.md` e observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem, observe se o pull request inclui comentários TSDoc, segue as convenções de TypeScript e adiciona comentários de cabeçalho. Essas orientações vêm dos arquivos de instruções e serão retomadas em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem

O agente de nuvem usa o GitHub Actions para preparar o ambiente. Um fluxo de trabalho especial de configuração pode ser definido em `.github/workflows/copilot-setup-steps.yml` e executado antes do trabalho do agente. O projeto já inclui essa configuração em um arquivo YAML (YAML Ain't Markup Language) para instalar o Node.js e as dependências, baixar os navegadores do Playwright para testes de ponta a ponta e configurar o banco de dados SQLite da aplicação Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Conceda somente as permissões mínimas necessárias às etapas. O Copilot recebe um token próprio para as operações dele.
    permissions:
      # Se as etapas de configuração clonarem o repositório, por exemplo, para instalar dependências, será necessária a permissão `contents: read`. Caso contrário, o Copilot clonará o repositório automaticamente depois que as etapas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do aplicativo: Node.js (o aplicativo agora é todo Astro + Drizzle/libSQL)
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

      # Migra e preenche o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo tem a estrutura de um fluxo de trabalho comum do GitHub:

- Há um único trabalho, `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** e verificar se ele funciona antes de aguardar uma execução do Copilot.

## Adicionar documentação

Muitos projetos têm documentação desatualizada ou insuficiente. Esse tipo de dívida técnica pode reduzir a produtividade e dificultar a manutenção do código e a integração de pessoas desenvolvedoras. O Copilot pode criar documentação para a issue do backlog. Você revisará o resultado em um exercício futuro.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

    ```plaintext
    Nossa organização exige comentários TSDoc nas funções e nos métodos quando eles ajudam a explicar o código. Infelizmente, atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários quando eles ajudarem a esclarecer o comportamento.
    ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot** para abrir o diálogo de atribuição.

    ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o novo pull request, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele não aparecer, aguarde um ou dois minutos e atualize a página.
12. Aguarde alguns minutos até que o Copilot crie uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer. O Copilot está preparando o ambiente com o fluxo de trabalho descrito acima, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Percorra a linha do tempo do pull request e procure a atualização que indica que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

    ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Acompanhe a sessão: o Copilot explora o código, entende o estado do projeto, planeja a abordagem e cria código.

O trabalho pode levar alguns minutos. Enquanto o agente de nuvem executa essa tarefa de forma assíncrona, atribua outra e continue trabalhando.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma vantagem do agente de nuvem é poder dividir o trabalho: enquanto ele cuida de uma tarefa, você pode se concentrar em outra. Atribua ao agente a seção de jogos relacionados solicitada pela equipe de design.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos mostrando jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência e fazer testes de aceitação. Requisitos:

    - Adicione em `src/lib/` um auxiliar de acesso a dados que retorne outros jogos da mesma categoria e exclua o jogo atual.
    - Exiba uma seção "Related games" na página de detalhes usando o auxiliar.
    - Trate o caso em que um jogo não tenha jogos relacionados.
    - Adicione testes unitários para o novo auxiliar.
    - Confirme se todos os testes passam antes de criar o PR.
    ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot**.

    ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Em breve, um emoji de olhos poderá aparecer no primeiro comentário da issue para indicar que o Copilot está trabalhando nela.

![Emoji de olhos indicando que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O agente de nuvem trabalha de maneira assíncrona. Você não precisa acompanhá-lo continuamente; revise o resultado quando estiver pronto. Enquanto isso, conheça os agentes personalizados.

## Resumo e próximos passos

Este exercício apresentou o agente de nuvem do GitHub Copilot. Você pode atribuir issues ao agente para que ele trabalhe de forma assíncrona em dívida técnica, recursos ou migrações entre estruturas. Você:

- personalizou o ambiente para gerar código.
- considerou como garantir que as operações sejam realizadas com segurança.
- explorou a importância de definir issues com escopo claro.
- atribuiu issues ao Copilot.

Enquanto o agente trabalha, [crie e use agentes personalizados][next-lesson]. O agente de nuvem também [pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e pode acessar instruções personalizadas, exploradas em exercícios anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

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
