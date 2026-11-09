---
title: "Lição 1 - Instalar o GitHub Copilot CLI"
description: "Instale e autentique o Copilot CLI no Codespace, conheça o ambiente e localize a issue de filtragem criada pelo modelo."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O [GitHub Copilot CLI][about-copilot-cli] é um assistente de programação baseado em agentes que funciona no terminal. Ele explora bases de código, gera código, executa comandos e interage com ferramentas externas pela linha de comando. Assim, você pode delegar tarefas, pedir mudanças e manter o foco. Instale a ferramenta usando recursos que você já conhece.

Nesta lição, você vai:

- instalar o GitHub Copilot CLI com npm.
- autenticar-se com sua conta do GitHub.
- confiar no repositório do workshop e iniciar uma conversa rápida.
- localizar a issue de filtragem pelo servidor integrado do Model Context Protocol (MCP) do GitHub.

## Cenário

Sua equipe está começando a usar agentes de IA para avançar em um backlog crescente. O Copilot CLI traz esses recursos para o terminal, onde muitas pessoas desenvolvedoras já trabalham. Nesta lição, você instalará e autenticará a ferramenta para usá-la no restante do workshop.

## Instalar o Copilot CLI

Você pode instalar o Copilot CLI com [npm][install-cli], WinGet ou Homebrew. Como o GitHub Codespaces já inclui o Node.js, use o npm.

1. Volte ao Codespace e abra um terminal.
2. Confirme se o Node.js está instalado e atende ao requisito de versão:

   ```bash
   node --version
   ```

   A versão deve ser 24 ou posterior.

3. Instale o Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Verifique a instalação:

   ```bash
   copilot --version
   ```

   O comando deve exibir o número da versão instalada.

## Autenticar-se com o GitHub

Na primeira inicialização, o Copilot CLI solicitará autenticação com sua conta do GitHub.

1. Inicie o Copilot CLI:

   ```bash
   copilot
   ```

2. Se solicitado, siga as instruções do código de dispositivo para autenticar-se e autorizar o Copilot CLI.
3. O Copilot CLI exibirá:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirme se o caminho corresponde ao repositório Tailspin Toys e selecione **Yes, and remember this folder for future sessions**.

> [!NOTE]
> Em um Codespace, talvez você já esteja autenticado pela sessão do GitHub. Se o Copilot CLI iniciar sem pedir autenticação, pode continuar.

## Conhecer o ambiente

Os comandos digitados no prompt normal do shell são executados diretamente no Codespace. Depois que o Copilot CLI inicia, a linguagem natural é enviada ao agente e os comandos de barra controlam a conversa.

1. Digite `/model`, use as teclas de seta para selecionar **Auto**, pressione <kbd>Enter</kbd> e pressione <kbd>Enter</kbd> novamente para confirmar.
2. Digite `/help` para ver os comandos disponíveis na versão instalada e pressione <kbd>Esc</kbd> para fechar a ajuda.
3. Faça uma pergunta simples para confirmar se tudo funciona:

   ```plaintext
   Quais são os principais arquivos deste projeto?
   ```

4. Leia a resposta e observe como o Copilot explora o repositório antes de responder.
5. Digite `/mcp list` e confirme se o servidor MCP integrado do GitHub está disponível.
6. Peça ao Copilot que localize a issue de filtragem:

   ```plaintext
   Usando o MCP do GitHub, encontre neste repositório a issue intitulada "Allow users to filter games by category and publisher". Informe a URL e um breve resumo. Não faça alterações.
   ```

7. Abra o endereço e leia a issue. Você começará a trabalhar nela depois de concluir uma primeira mudança rápida.

> [!TIP]
> Uma sessão normal do Copilot CLI trabalha na branch selecionada no terminal; ela não cria uma worktree automaticamente. Você criará uma branch de recurso antes de cada mudança.

## Usar o atalho do workshop

Normalmente, o Copilot CLI pede permissão antes de usar ferramentas fora das permissões já concedidas. Neste workshop, você o reiniciará com `--yolo`, um atalho aprovado para eliminar essas solicitações no Codespace e manter o foco nos exercícios.

> [!CAUTION]
> `--yolo` concede permissões automáticas completas (`--allow-all-tools`, `--allow-all-paths` e `--allow-all-urls`). Use-o somente em um ambiente isolado, como um Codespace ou uma máquina virtual (VM), e nunca o defina como padrão no desenvolvimento diário. Consulte [Permitir e negar o uso de ferramentas][allow-all-warning] para saber mais.

A opção `--enable-all-github-mcp-tools` habilita as ferramentas MCP de leitura e gravação do GitHub usadas nas próximas lições para trabalhar com issues e pull requests. O Codespace limita o acesso ao computador local, mas os recursos autenticados do GitHub continuam sendo reais. Revise as mudanças antes de publicá-las ou fazer merge.

1. Saia do Copilot CLI digitando `/exit`.
2. Reinicie-o na raiz do repositório:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Faça outra pergunta rápida sobre o projeto para confirmar se a conversa funciona e, depois, saia com `/exit`.

O Copilot salva as conversas automaticamente. Mais adiante, depois de atualizar instruções ou adicionar um agente, você usará `copilot --resume` para voltar à mesma conversa e branch.

## Resumo e próximos passos

Você:

- instalou o GitHub Copilot CLI com npm.
- autenticou-se com sua conta do GitHub.
- confiou no repositório do workshop e iniciou uma conversa rápida.
- localizou a issue de filtragem pelo servidor MCP integrado do GitHub.

Em seguida, [faça sua primeira mudança específica][next-lesson] e use o Copilot CLI para exibir uma avaliação por estrelas nos cards de jogos.

## Recursos

- [Instalar o GitHub Copilot CLI][install-cli]
- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
