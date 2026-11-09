---
title: "Lição 8 - Criar e fazer merge do PR do recurso"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de QA e os testes; depois, crie um PR e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

A implementação da filtragem, as atualizações das instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma branch. Agora, revise-os em conjunto e abra um pull request. Você fez o merge do pull request (PR) das avaliações por estrelas por conta própria; desta vez, deixe o **Agent Merge** gerenciar o processo.

> [!NOTE]
> Em geral, dividiríamos o recurso, as atualizações das instruções e da skill e o agente de QA em alguns PRs. Para simplificar o workshop, mantivemos todo o fluxo de filtragem e qualidade em uma sessão e branch, e todo esse trabalho será incluído neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionar o PR completo do recurso e as evidências de verificação.
- autorizar o Agent Merge somente depois da revisão e confirmar se o PR foi integrado.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar um recurso. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle de quem desenvolve.

## Sobre o Agent Merge

O **Agent Merge** automatiza o trabalho necessário para integrar um pull request no aplicativo GitHub Copilot. Quando você o habilita, a sessão do aplicativo lê o PR e resolve o que estiver bloqueando o merge — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e faz o merge assim que o GitHub permite. O Agent Merge funciona em segundo plano, continua após reinicializações do aplicativo e é desativado quando o PR é integrado.

Até agora, você selecionou **Merge pull request** por conta própria. O Agent Merge pode assumir essa responsabilidade, mas ainda precisa de autorização explícita para editar código e fazer merge. Revise as ações permitidas e o trabalho antes de conceder a permissão de merge.

## Usar o Agent Merge para gerenciar o PR

Com todo o código criado e revisado, deixe o Agent Merge gerenciar o processo do PR.

1. No seletor de agentes, selecione **Default agent**.
2. Selecione o menu suspenso ao lado de **Create PR**.
3. Selecione **Agent Merge**. O botão mudará para **Agent Merge**.
4. Selecione o botão **Agent Merge** para iniciar o processo. Selecionar a opção no menu suspenso, por si só, não inicia o processo.

O Agent Merge começará a trabalhar. Ele poderá:

- criar o pull request com título e descrição.
- incluir a issue relacionada no corpo da descrição se você iniciou a sessão a partir de uma issue.
- fazer rebase ou lidar com conflitos de merge com a branch de destino.
- monitorar a integração contínua e confirmar se todas as verificações foram aprovadas.
- monitorar comentários de outras pessoas desenvolvedoras ou da revisão de código do Copilot e fazer mudanças para resolvê-los.
- fazer merge automático do PR quando tudo estiver concluído, se você autorizar.

Peça ao Agent Merge que faça o merge do PR depois que todas as verificações forem aprovadas.

5. Revise o diff do PR e as evidências de verificação antes de conceder permissão para fazer merge.
6. Selecione o menu suspenso ao lado de **Agent Merge**.
7. Confirme se há uma marca ao lado de **Merge pull request**.
8. Peça explicitamente ao agente que faça o merge:

    ```plaintext
    Faça o merge do PR quando todas as verificações e aprovações obrigatórias forem aprovadas. Não ignore as proteções do repositório.
    ```

9. Acompanhe o progresso e resolva os bloqueios informados. Antes de continuar, confirme se o estado do PR é **Merged**. Permitir o merge ou ter um agente em execução não confirma que o merge foi concluído.

> [!IMPORTANT]
> O Agent Merge não ignora as proteções do repositório nem a falta de permissões. Resolva esses bloqueios antes de continuar.

## Resumo e próximos passos

Você automatizou várias etapas do desenvolvimento: geração, teste e validação de código e o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de merge.
- inspecionou o PR completo do recurso e as evidências de verificação.
- autorizou o Agent Merge somente depois da revisão e confirmou se o PR foi integrado.

Em seguida, [use um canvas existente e crie um canvas de triagem][next-lesson] para explorar uma forma mais completa de inspecionar, planejar e visualizar o trabalho com o agente.

## Recursos

- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
