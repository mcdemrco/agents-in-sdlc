---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora o código, propõe mudanças, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina em tempo real, o Copilot Chat no VS Code mostra as chamadas de ferramentas e as edições. Você pode revisar cada diff, aceitar ou rejeitar mudanças individuais e orientar a conversa com prompts complementares sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas.
- orientar o agente durante a sessão para adicionar um controle de modo claro.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho acontece, a equipe de design pede também um controle de modo claro. Em vez de começar uma conversa nova, oriente a sessão atual para ampliar o trabalho.

## Monitorar o progresso do Copilot

A exibição Copilot Chat mostra as etapas do agente, incluindo os arquivos que explora, os comandos que executa e as edições propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa e acompanhe as chamadas de ferramentas, leituras e edições de arquivos e comandos do terminal.
3. Quando o agente propuser uma edição, revise o diff:
   - selecione **Keep** para aceitar a mudança.
   - selecione **Undo** para desfazê-la e deixar o agente tentar novamente.
4. Abra **Files changed**, na parte superior do chat, para ver todos os arquivos modificados na sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando do terminal, o agente aguardará sua resposta e exibirá **Allow**. Aprove somente os comandos com os quais você estiver de acordo.

5. Aguarde o agente concluir o modo de alto contraste. Ele deverá apresentar uma mensagem final que resuma as mudanças.

## Orientar o agente para adicionar o modo claro

Com o modo de alto contraste implementado, amplie a mesma conversa para adicionar um controle de modo claro. A sessão mantém o contexto do trabalho anterior, incluindo os componentes, os estilos e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt:

    ```plaintext
    Bom trabalho! Agora adicione um controle de modo claro ao lado do controle de alto contraste. Use o mesmo padrão: persista a preferência no armazenamento local e deixe o controle na mesma área da interface. As pessoas usuárias devem poder ativar o alto contraste e o modo claro de forma independente.
    ```

2. O agente retomará o trabalho, explorará as mudanças existentes e proporá as alterações para o modo claro.
3. Revise cada nova edição e selecione **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada, como refatorar os controles de uma maneira inadequada, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma instrução complementar para esclarecer o pedido.

> [!TIP]
> Orientar uma sessão em andamento pode ser mais rápido do que começar de novo. Peça ajustes, como simplificar as folhas de estilo em cascata (CSS) usando variáveis, corrigir o rumo para integrar o modo claro ao painel de configurações existente ou ampliar o escopo para adicionar atalhos de teclado aos controles.

## Revisar as mudanças

Antes de fazer commit, revise o conjunto completo de arquivos alterados pelo agente.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista de arquivos alterados. Ela deve incluir componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos e examine os diffs. Confirme se as práticas de acessibilidade do agente estão refletidas, incluindo atributos Accessible Rich Internet Applications (ARIA), navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das mudanças no painel Source Control. Você as publicará na próxima lição.

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento, revisando cada edição, aceitando ou rejeitando mudanças e enviando prompts complementares sem perder o contexto. Nesta lição, você:

- monitorou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou a sessão com prompts complementares.

Na [próxima lição][next-lesson], publique o trabalho de acessibilidade como um pull request e revise as mudanças feitas pelo agente personalizado local.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
