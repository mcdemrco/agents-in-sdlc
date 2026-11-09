---
title: "Lição 2 - Adicionar avaliações por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revisar a mudança em um navegador encaminhado e integrá-la no seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e iniciou uma conversa, faça sua primeira alteração no projeto. Mantenha-a pequena: os dados dos jogos já incluem avaliações por estrelas, mas elas ainda não aparecem nos cards da página inicial. Peça ao agente que as exiba, revise a mudança e integre-a no seu primeiro pull request (PR).

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de recurso.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo e confirmar a mudança em um navegador encaminhado.
- abrir e integrar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cards da página inicial, porém, mostram somente o título, a categoria, a editora e a descrição. Como primeiro exercício, peça ao agente que exiba em cada card a avaliação existente — uma mudança pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é onde você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal do CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a discussão depois, enquanto os arquivos e a branch continuam no estado normal do Git em disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente, a atividade das ferramentas que o agente usa para explorar e editar arquivos e as mudanças que você pode inspecionar com `/diff`.

## Iniciar uma conversa e solicitar a mudança

Inicie uma conversa nova para começar a implementar o recurso.

1. Volte ao Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver aberto, execute:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Envie o prompt a seguir para solicitar a mudança:

   ```plaintext
   Exiba em cada card de jogo da página de listagem a avaliação `starRating` de 5. Se a avaliação for null, mostre "No rating yet". Mantenha o layout dos cards, adicione testes e execute as verificações pertinentes.
   ```

O Copilot explora o projeto, localiza os arquivos que exibem os detalhes dos jogos e cria o código necessário. Você acabou de adicionar um recurso com o Copilot CLI.

## Revisar as diferenças

Revise todas as mudanças geradas por IA antes de integrá-las, mesmo quando forem pequenas.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme se o card exibe a avaliação numérica quando ela existe e `No rating yet` quando `starRating` é `null`.
3. Confirme se os testes cobrem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diferenças.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico. O código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica à de um exemplo.

## Verificar as mudanças

Não basta ler o código e presumir que ele funciona. Peça ao Copilot para iniciar o site e examine a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa verificar no navegador a mudança nas avaliações por estrelas. Informe o localizador uniforme de recursos (URL) e mantenha o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cards dos jogos exibem as avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Pare o servidor de desenvolvimento que você iniciou.
   ```

## Abrir e integrar seu primeiro pull request

Agora que você criou o recurso, abra um pull request para integrar o código ao projeto.

1. Peça ao agente padrão que faça commit da mudança:

   ```plaintext
   Faça commit das mudanças revisadas nas avaliações por estrelas com uma mensagem adequada.
   ```

2. Digite `/pr create`. Ao criar o pull request, o Copilot CLI envia o commit existente e exibe a URL do pull request.
3. Abra o pull request mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua branch local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira mudança usando o GitHub Copilot CLI. Você:

- iniciou uma conversa focada do Copilot em uma branch de recurso.
- orientou o agente a fazer uma pequena alteração nos cards de jogos.
- revisou a mudança com `/diff`.
- executou o aplicativo e confirmou a avaliação por estrelas em um navegador encaminhado.
- abriu e integrou seu primeiro pull request.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
