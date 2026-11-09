---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot são assistentes de inteligência artificial (IA) especializados em tarefas ou domínios do fluxo de desenvolvimento. Você os define em arquivos Markdown na pasta `.github/agents` do repositório ou globalmente em `~/.copilot/agents`. Com instruções, práticas recomendadas, padrões de código e conhecimentos específicos, as equipes podem transformar sua experiência em agentes reutilizáveis, como um agente de acessibilidade que aplica as [Diretrizes de Acessibilidade para Conteúdo Web (WCAG)][wcag], um agente de segurança ou um agente de testes.

Cada perfil tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA a executar tarefas. Uma distinção útil é que o **agente personalizado** é quem executa o trabalho, enquanto as **skills** são ferramentas.

Os agentes personalizados têm uma janela de contexto própria e podem coordenar skills e outros agentes. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site de acordo com diretrizes de acessibilidade. Ele poderia usar skills para executar testes ou seguir um fluxo de trabalho de pull request.

> [!NOTE]
> Não há uma única maneira certa de criar um agente personalizado. Como em outros usos de IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente da visão ou das preferências visuais. Comentários de usuários indicam que algumas pessoas têm dificuldade para ler o tema escuro por causa do contraste insuficiente entre o texto e o fundo. A equipe de design pediu um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, use um agente personalizado para implementar esse recurso rapidamente.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado para tarefas de acessibilidade. Revise o perfil para entender como ele orienta o Copilot.

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que descrevem:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código HTML, CSS e JavaScript.
   - problemas e erros comuns.

## Criar e atribuir uma issue

O painel de agentes é o local central para trabalhar com agentes no ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, monitorar o trabalho e orientar uma sessão. Crie uma issue para o modo de alto contraste e atribua-a ao agente personalizado.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Add high contrast mode to website`.
6. No campo **Description**, digite:

   ```plaintext
   Precisamos de um modo de alto contraste para o site. Deve haver um controle para ativá-lo, e a preferência deve ser armazenada no armazenamento local do navegador.
   ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot** para abrir o diálogo de atribuição.
9. Na lista de agentes personalizados, selecione **Accessibility agent**.

   ![Diálogo de atribuição do agente de nuvem, com as opções de agente personalizado e Accessibility destacadas](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. Aguarde enquanto o Copilot trabalha na tarefa em segundo plano.

## Resumo e próximos passos

Este exercício apresentou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados que ajudam a aplicar conhecimentos e padrões da equipe. Você:

- explorou como os agentes personalizados são definidos.
- atribuiu uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, [monitore e oriente a sessão do agente][next-lesson] no painel de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar o uso de agentes personalizados na organização][org-custom-agents]
- [Preparar o uso de agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
