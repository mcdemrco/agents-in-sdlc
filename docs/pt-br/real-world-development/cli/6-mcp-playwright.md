---
title: "Lição 6 - Validar a funcionalidade com o Model Context Protocol (MCP) do Playwright"
description: "Inspecione ou configure o MCP do Playwright no Copilot CLI e use-o para explorar o recurso de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já vimos, desenvolver não se resume a escrever código. Também é necessário trabalhar com dados, serviços externos e automações adicionais disponíveis para o Copilot. É aí que entram os servidores MCP, que ampliam as ferramentas e os serviços disponíveis no Copilot CLI.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionar o servidor MCP do Playwright, se ele ainda não estiver disponível.
- pedir ao agente que controle um navegador e explore o recurso de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar mudanças na interface exige interagir com ela. Você permitirá que o Copilot use o site como uma pessoa usuária faria, automatizando a verificação de que as mudanças funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de inteligência artificial (IA) se comuniquem com ferramentas e serviços externos. Com o MCP, os agentes podem acessar informações atualizadas e realizar ações em seu nome.

Essas ferramentas e esses recursos são acessados por um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos. Cada servidor MCP oferece ao agente um conjunto específico de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[Servidor MCP do GitHub][github-mcp]**: oferece acesso a interfaces de programação de aplicações (APIs) para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: oferece recursos de automação de navegador com o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use somente servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Adicionar o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária.

> [!NOTE]
> Se o MCP do Playwright já estiver configurado no Copilot CLI, pule estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra o diálogo para adicionar um servidor MCP digitando:

   ```plaintext
   /mcp add
   ```

3. Digite `playwright` como nome e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o comando a seguir no diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair do diálogo MCP.

## Pedir ao Copilot que explore o recurso com o Playwright

Você já confirmou manualmente que o recurso funciona. Agora, use o servidor MCP do Playwright disponível no Copilot para fazer essa verificação.

1. Envie o prompt a seguir para pedir ao Copilot que use o servidor MCP do Playwright:

   ```plaintext
   Inicie o aplicativo e use o MCP do Playwright para verificar a filtragem de acordo com a issue e o nosso plano. Informe o que funciona e o que não funciona, sem fazer mudanças. Pare o servidor que você iniciar ao terminar.
   ```

> [!NOTE]
> Não é necessário pedir explicitamente ao Copilot que use o servidor MCP: normalmente ele o identifica sozinho. Mesmo assim, indicar a ferramenta pode ajudar a obter resultados mais consistentes.

2. Observe as etapas que o Copilot executa no navegador para confirmar se o recurso funciona.
3. Leia o relatório e confirme se tudo funciona como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, parará o servidor e apresentará um relatório.

## Resumo e próximos passos

Você usou o servidor MCP do Playwright para explorar o recurso em um navegador real a partir do Copilot CLI. Nesta lição, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionou o servidor MCP do Playwright, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse o recurso de filtragem.

Em seguida, [crie um agente personalizado de garantia de qualidade (QA)][next-lesson] que combine a skill e as ferramentas de navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
