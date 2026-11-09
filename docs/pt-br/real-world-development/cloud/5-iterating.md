---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Neste laboratório, você pediu ao GitHub Copilot que documentasse o código, implementasse uma seção de jogos relacionados para a equipe de design e adicionasse recursos de acessibilidade, incluindo controles de alto contraste e modo claro. Revise as mudanças e, se necessário, dê feedback ao Copilot.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a inteligência artificial (IA) generativa. Revise o código gerado e siga o processo normal de DevOps. Agora, examine as sugestões do GitHub Copilot para documentação, jogos relacionados e acessibilidade antes de habilitar a revisão pelo restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão, há restrições de segurança:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação somente à branch usada para o trabalho.
- O agente de nuvem é executado no GitHub Actions em um ambiente separado e temporário.
- Uma pessoa precisa aprovar a execução de cada fluxo de trabalho do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão][agent-firewall], inclusive a servidores do Model Context Protocol (MCP).

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem: o PR que adiciona documentação ao código.

> [!NOTE]
> O PR pode mostrar um aviso de que o Copilot foi bloqueado por um firewall. Isso é esperado: por padrão, o acesso do agente de nuvem a recursos externos, inclusive servidores MCP, é limitado. Se necessário, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests**.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as mudanças.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que pode incluir comentários TSDoc e outra documentação. As mudanças exatas variam. Procure comentários TSDoc, convenções de TypeScript e comentários de cabeçalho, que seguem os arquivos de instruções revisados no início do Exercício 2.
6. Quando terminar a revisão, volte à guia **Conversation** e role a página para baixo.
7. Verifique se há um indicador de fluxos de trabalho aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Acompanhe os fluxos de trabalho na seção de verificações do PR. Quando todos terminarem, confirme se as verificações do projeto para o aplicativo Astro foram aprovadas. Isso pode levar alguns minutos.

## Pedir mudanças ao GitHub Copilot

Você também pode mencionar o Copilot em comentários do PR ou em comentários em linha no código. Ele pode iniciar outra sessão para atender ao pedido. Como os resultados não são determinísticos, não há um único prompt prescrito. Você pode pedir ao Copilot que:

- adicione comentários de cabeçalho aos arquivos de código.
- adicione comentários TSDoc aos arquivos TypeScript e Astro.
- crie um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário mencionando **@copilot** e solicite uma mudança na documentação gerada. Use uma das sugestões acima ou indique outra melhoria desejada.
2. Selecione **View Session** para acompanhar a nova sessão do Copilot.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Botão Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as mudanças, confirme se há um novo commit no PR.
5. Selecione **Files changed** e revise as mudanças.

Continue iterando até ficar satisfeito. Depois, você poderá marcar o PR como pronto para revisão e integrá-lo à branch principal.

![Opção para marcar um PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, revise o PR criado pelo Copilot para mostrar jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** e revise o código gerado.
5. Quando terminar, volte à guia **Conversation** e role a página para baixo.
6. Verifique se há um indicador de fluxos de trabalho aguardando aprovação.
7. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Acompanhe os fluxos de trabalho na seção de verificações do PR e confirme se as verificações do aplicativo Astro foram aprovadas.
9. **Opcional:** teste manualmente o recurso no Codespace. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados. Você pode pedir mudanças adicionando comentários na guia **Conversation**. Por exemplo, solicite que ajuste a quantidade de jogos exibidos ou adicione comentários de cabeçalho e TSDoc. Lembre-se de que essa issue foi atribuída antes de atualizar as instruções personalizadas.

## Revisar os recursos de acessibilidade

Por fim, revise o PR do agente personalizado de acessibilidade. Ele deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no painel de agentes do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Selecione **Files changed** e revise o código.
5. Examine a implementação, especialmente:
   - os componentes de interface dos controles de alternância entre modos.
   - o armazenamento local usado para manter as preferências.
   - as mudanças de folhas de estilo em cascata (CSS) para os modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos Accessible Rich Internet Applications (ARIA) e navegação por teclado.
   - o código JavaScript/TypeScript que controla a alternância.
6. Quando terminar, volte à guia **Conversation** e role a página para baixo.
7. Verifique se há um indicador de fluxos de trabalho aguardando aprovação.
8. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Acompanhe os fluxos de trabalho e confirme se as verificações do aplicativo Astro foram aprovadas.
10. **Opcional:** teste os recursos de acessibilidade no Codespace. No terminal, execute os comandos a seguir e substitua `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Inicie o aplicativo e teste os controles de alto contraste e modo claro no navegador. Confirme se as preferências persistem após recarregar a página.

O agente personalizado de acessibilidade ajudou a orientar a implementação segundo práticas recomendadas. Se encontrar problemas de acessibilidade, mencione **@copilot** em um comentário e peça as mudanças necessárias.

## Exercício opcional: continuar delegando

O agente de nuvem é útil para trabalhar em itens reais do backlog enquanto você se concentra em outras tarefas. Abra mais algumas issues no seu repositório e atribua-as ao Copilot. Por exemplo:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página de lista de jogos.
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Você:

- inspecionou os arquivos de instruções incluídos no repositório para reconhecer sua influência nos resultados do agente.
- atribuiu issues ao agente de nuvem e observou a preparação do ambiente, o planejamento e a execução assíncrona.
- criou e usou um agente personalizado de acessibilidade para adicionar modos de alto contraste e claro.
- usou a página de agentes do Copilot para monitorar e orientar a sessão de acessibilidade.
- revisou e aprimorou os pull requests do agente de nuvem, mencionando `@copilot` para solicitar mudanças e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Para continuar explorando, veja os outros percursos:

- **[Percurso do VS Code](../../vscode/)**: explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- **[Percurso da CLI](../../cli/)**: entregue duas mudanças revisadas pelo terminal com os modos Plan e Autopilot, skills, um agente de QA personalizado, MCP do Playwright, Agent Merge e uma ponte opcional `/delegate` para o agente de nuvem.

No seu repositório, experimente também:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha de tecnologias.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o ambiente do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../4-managing-agents/
[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
