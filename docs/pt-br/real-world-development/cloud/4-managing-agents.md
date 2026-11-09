---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos exercícios anteriores, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

A [página de agentes][agents-page] no GitHub.com permite gerenciar tarefas atribuídas ao agente de nuvem. Nesse painel, você vê as tarefas com pull requests abertos, explora as operações realizadas e orienta sessões em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar outra funcionalidade.

## Cenário

Depois de atribuir ao agente a implementação do modo de alto contraste, a equipe decidiu adicionar também um modo claro. Como o agente já estava atualizando o estilo do site e adicionando controles de alternância, peça que amplie o trabalho em andamento.

## Revisar as tarefas do agente de nuvem do Copilot

Confira o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas listadas no painel principal e no painel à esquerda. Elas devem incluir tarefas como:
   - adicionar documentação ao código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações feitas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - preparar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas listadas variam conforme o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel de pull request à direita, onde você pode acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que adicione o controle de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada ao modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

   ![Sessão de acessibilidade no painel de agentes](../../../_images/ex6-accessibility-session.png)

2. Aguarde até que a configuração seja concluída e o Copilot comece a trabalhar. Você verá mensagens indicando essas etapas.
3. No diálogo **Steer active session while Copilot is working**, envie este prompt:

   ```plaintext
   Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Deve haver um controle para esse modo, que permita às pessoas usuárias selecionar a opção de exibição desejada.
   ```

   ![Tarefa do agente de nuvem no painel de agentes, com o diálogo para orientar a sessão ativa destacado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma o recebimento do pedido e o inclui no fluxo de trabalho.

## Deixar o Copilot trabalhar

O Copilot incorporará o novo pedido ao trabalho depois de concluir a etapa que estava executando quando você enviou a mensagem. Isso pode levar alguns minutos.

## Resumo e próximos passos

Você explorou a página de agentes do Copilot, o painel central para monitorar e orientar tarefas do agente de nuvem. Você:

- explorou o painel de agentes para monitorar tarefas do agente de nuvem.
- redirecionou uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot conclui o trabalho de acessibilidade, [revise e itere sobre os pull requests criados por ele][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
