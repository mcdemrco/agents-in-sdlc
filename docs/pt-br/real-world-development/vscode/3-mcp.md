---
title: "Exercício 3 - Testar o recurso com o servidor Model Context Protocol (MCP) do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request (PR), confirme se ele funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que o use para testar manualmente a filtragem no navegador.
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna mais poderoso quando pode acessar ferramentas além do editor. O Model Context Protocol (MCP) oferece uma maneira padronizada para o agente se comunicar com ferramentas e serviços externos.

![Diagrama da interação do modo de agente com contexto, o modelo de linguagem e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de inteligência artificial (IA) se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes podem acessar informações atualizadas usando recursos e realizar ações em seu nome usando ferramentas.

Os agentes acessam essas ferramentas e esses recursos por meio de um servidor MCP, que faz a comunicação entre o agente de IA e as ferramentas externas, como interfaces de programação de aplicações (APIs) existentes ou ferramentas locais instaladas como pacotes npm. Cada servidor MCP oferece ao agente um conjunto específico de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp-server]**: fornece acesso a APIs para gerenciar repositórios do GitHub, criar repositórios, atualizar os existentes e gerenciar issues e pull requests.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: automatiza navegadores usando o Playwright para navegar em páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no Codespace.
2. Confirme se o servidor `github` já está configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

A entrada `github` vem do modelo do projeto. Por isso, o Copilot conseguiu ler as issues do backlog no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server]; não é necessário instalá-lo localmente. O VS Code se conecta pelo protocolo de transferência de hipertexto (HTTP), e você se autentica no GitHub na primeira vez que o Copilot usa uma ferramenta.

## Adicionar o servidor MCP do Playwright

Adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar para testar o recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, o servidor `playwright` é local: o VS Code o inicia na máquina executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, o que é necessário no Codespace, que não tem uma área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP sob demanda: quando o Copilot precisa das ferramentas pela primeira vez, o VS Code inicia o servidor e solicita confirmação de que você confia nele.

1. Em `.vscode/mcp.json`, selecione a ação **Start** que aparece acima da entrada `playwright` para iniciar o servidor e confirmar a configuração.
2. Se o VS Code pedir que você confirme se confia no servidor, revise a configuração e escolha a opção para confiar nele.

Iniciar o servidor manualmente é opcional. Se você pular essas etapas, o VS Code o iniciará e pedirá confirmação quando o Copilot precisar das ferramentas no próximo exercício.

## Testar o recurso de filtragem

O servidor MCP do Playwright permite que o Copilot use um navegador real: abra uma página, navegue, aplique filtros, leia os resultados e apresente o que observou. Assim, você pode confirmar rapidamente o comportamento do recurso sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, não capturas de tela. O agente interpreta elementos estruturados e identificados — botões, links e itens de lista — de maneira semelhante à tecnologia assistiva. Portanto, uma verificação funcional também ajuda a identificar problemas básicos de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso:

```text
Usando o servidor MCP do Playwright, abra um navegador para o aplicativo em execução em http://localhost:4321 e verifique o novo recurso de filtragem de jogos:

1. Acesse a página de jogos e informe quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme se a lista mostra somente os jogos dessa categoria.
3. Remova o filtro e aplique um filtro de editora. Confirme se a lista mostra somente jogos dessa editora.
4. Combine um filtro de categoria e um de editora e confirme se os resultados respeitam ambos.

Informe o que observar em cada etapa e destaque qualquer comportamento inesperado.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá as etapas e apresentará um relatório. Compare-o com os critérios de aceitação da issue. Se algo estiver errado, faça perguntas adicionais ou peça que o Copilot corrija o código antes de abrir o PR.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se o servidor de desenvolvimento estiver parado, inicie-o antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor MCP do Playwright, talvez seja necessário baixar um navegador. Se ele informar que o navegador está ausente, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Depois de confirmar que o recurso funciona, publique a branch `filtering-vscode` para que a equipe possa revisar o PR.

1. No Codespace, abra o painel **Source Control** e revise as mudanças feitas pelo Copilot.
2. Prepare as mudanças selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as mudanças realizadas](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Você pode criar um pull request no github.com, com a GitHub CLI ou pedir ao GitHub Copilot que localize a issue pertinente e associe-a ao PR.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão.
2. Confirme se **Agent** está selecionado no seletor de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Encontre no repositório a issue sobre a filtragem por categoria e editora. Crie um pull request para a branch atual e associe-o à issue correta.
   ```

4. Selecione **Continue** quando necessário para permitir que o Copilot obtenha informações e execute ações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja necessário autenticar-se.
5. Observe o Copilot pesquisar as issues, localizar a correta e criar o PR.
6. Abra o link criado pelo Copilot e revise o pull request, mas não faça o merge ainda.

## Resumo e próximos passos

Você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou o servidor para testar manualmente o recurso de filtragem em um navegador.
- publicou sua branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para testar um recurso, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, [crie um agente personalizado][next-lesson] para agilizar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
