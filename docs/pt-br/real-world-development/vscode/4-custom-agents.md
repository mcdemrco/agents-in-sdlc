---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de inteligência artificial (IA) especializados em tarefas ou domínios do fluxo de desenvolvimento. Você os define em arquivos Markdown na pasta `.github/agents` do repositório e fornece instruções, práticas recomendadas, padrões de código e conhecimentos específicos. Assim, as equipes podem transformar sua experiência em agentes reutilizáveis, como um agente de acessibilidade que aplica as [Diretrizes de Acessibilidade para Conteúdo Web (WCAG)][wcag], um agente de segurança ou um agente de testes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem metadados em YAML (YAML Ain't Markup Language) com pelo menos os campos `name` e `description`, seguidos de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA a executar tarefas. Uma distinção útil é que o **agente personalizado** é quem executa o trabalho, enquanto as **skills** são ferramentas.

Os agentes personalizados têm uma janela de contexto própria e podem coordenar skills e outros agentes. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site de acordo com diretrizes de acessibilidade. Ele poderia usar skills para executar testes ou seguir um fluxo de trabalho de pull request.

> [!NOTE]
> Não há uma única maneira certa de criar um agente personalizado. Teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Você explorará como os agentes personalizados são definidos e como atribuir tarefas a eles.

## Cenário

A Tailspin Toys quer tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente da visão ou das preferências visuais. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro por causa do contraste insuficiente entre o texto e o fundo. A equipe de design pediu um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, use um agente personalizado para implementar o recurso.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado para tarefas de acessibilidade. Revise o perfil para entender como ele orienta o Copilot.

1. Volte ao Codespace e crie uma branch a partir de `main` para manter o trabalho de acessibilidade separado do pull request de filtragem do Exercício 3:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abra `.github/agents/accessibility.md`.
3. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

4. Examine as seções seguintes, que descrevem:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código em HTML (HyperText Markup Language), CSS (Cascading Style Sheets) e JavaScript.
   - problemas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra no seletor de agentes do Copilot Chat todos os agentes personalizados definidos em `.github/agents`. Selecione um agente para usar as instruções e ferramentas correspondentes na conversa.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de continuar, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas indicarão as opções necessárias.

1. No seletor de agentes da exibição Chat, selecione **Agent**, caso ainda não esteja selecionado.

   ![Seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Abra o seletor de agentes na parte inferior da exibição Chat; ele mostra o agente ativo, que por padrão é **default**.
3. Na lista, selecione **Accessibility agent**.
4. Envie este prompt ao agente:

    ```plaintext
    Adicione um modo de alto contraste ao site. Deve haver um controle que as pessoas usuárias possam ativar, e a preferência deve persistir entre recarregamentos da página usando o armazenamento local do navegador.
    ```

5. O Copilot Chat explorará o código, proporá mudanças e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho e um diff para revisão.
6. Observe o indicador **Files changed**, que atualiza a lista de arquivos modificados pelo agente.

> [!NOTE]
> Esse processo pode levar alguns minutos. O Copilot está fazendo mudanças reais no repositório, como editar componentes Astro, CSS e testes relacionados.

Você acompanhará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Este exercício apresentou os [agentes personalizados][custom-agents], assistentes especializados que ajudam a aplicar conhecimentos e padrões da equipe. Você:

- revisou como os agentes personalizados são definidos.
- usou um agente personalizado no modo de agente do Copilot Chat.

Em seguida, [monitore e oriente o trabalho do agente][next-lesson], revise as mudanças enquanto são feitas e peça que ele adicione um controle de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
