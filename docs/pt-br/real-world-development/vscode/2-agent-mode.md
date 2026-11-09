---
title: "Exercício 2 - Adicionar funcionalidade com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma pequena mudança em um aplicativo pode exigir alterações em vários arquivos e a execução de testes. É comum localizar os arquivos necessários, fazer as mudanças, executar testes, depurar e descobrir que algum arquivo ficou faltando. O modo de agente do Copilot pode explorar a estrutura do projeto, fazer mudanças coordenadas, executar tarefas como testes e corrigir problemas encontrados. Use-o para adicionar um recurso ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, com ciclos de feedback rápidos e interação. O agente de nuvem trabalha de forma assíncrona como parte da equipe e interage com você por meio de issues e pull requests.

Neste exercício, você vai aprender como:

- o modo de agente do Copilot explora o projeto, identifica arquivos pertinentes e faz mudanças coordenadas.
- o modo de agente do GitHub Copilot implementa recursos na interface e na camada de dados.
- revisar as mudanças e os testes gerados pelo modo de agente do Copilot antes de integrá-los ao código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas usuárias filtrem por categoria e editora. Você já adicionou um auxiliar de editoras no exercício anterior; agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer mudanças, explore o site para conhecer os recursos existentes.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento de software. Ele permite listar jogos e exibir detalhes. O aplicativo Astro gera páginas da Linguagem de Marcação de Hipertexto (HTML) estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter por meio do Drizzle ORM; não há uma interface de programação de aplicações (API) de back-end separada nem uma estrutura de interface no cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com pequenos elementos `<script>` do Astro que usam as APIs padrão do modelo de objetos do documento (DOM).

### Iniciar o site

Use a tarefa `npm run dev` para iniciar o único servidor de desenvolvimento Astro no Codespace.

1. Volte ao Codespace. Depois do exercício anterior, você deverá estar na branch `main`.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, será exibido um aviso semelhante a este:

   ```text
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se um diálogo solicitar que você abra `http://localhost:4321` em um navegador, feche-o selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> No Codespace, selecionar o link `localhost` no terminal redireciona para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor em execução no Codespace.

### Explorar o site

Com o site aberto, explore os recursos existentes:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá seus detalhes, incluindo título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

O projeto criado a partir do modelo já inclui um backlog de issues. Peça ao GitHub Copilot que o apresente.

1. No seletor de agentes da exibição Chat, selecione **Agent**. O agente planeja e implementa mudanças em vários arquivos, executa comandos do terminal e chama ferramentas.

   ![Seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> A inclusão de **Claude Sonnet 4.5** não indica preferência dos autores por um modelo. Ele foi usado para desenvolver o workshop e ajudar a manter as sugestões relativamente consistentes. Como os modelos de linguagem grandes (LLMs) são probabilísticos, as sugestões podem variar; isso é normal.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem de grande porte (LLMs), o Copilot pode usar um comando MCP diferente e ainda concluir a tarefa.

3. Envie este prompt para pedir ao Copilot que mostre o backlog:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los de acordo com o que será mais útil para as pessoas usuárias.
   ```

4. Selecione **Continue** para executar o comando que lista as issues.
5. Revise a lista de issues.

O Copilot também prioriza os itens com base no que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, leia o arquivo de instruções de interface do usuário (UI), que fornece contexto sobre como adicionar funcionalidades ao site.

1. No Codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, capacidade de teste e acessibilidade.
   - links para instruções específicas de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo que contém a referência. Isso permite reutilizar orientações, dividir conjuntos complexos em partes menores e fornecer exemplos ao Copilot.

## Implementar a filtragem

Para concluir a filtragem, faça mudanças na camada de dados, nos testes e na página de listagem de jogos. O modo de agente do Copilot pode ajudar com essas tarefas.

- Adicione ou refine a lógica de filtragem na camada de dados (`src/lib/`).
- Adicione ou atualize testes para o comportamento da filtragem.
- Atualize a página de listagem de jogos com a interface de filtragem.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de interface do usuário](../../../_images/ex3-select-instructions-file.png)

3. No seletor de agentes da exibição Chat, confirme se **Agent** está selecionado.

   ![Seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade de acordo com a issue relacionada no backlog:

   ```plaintext
   Atualize o site para incluir a filtragem por editora e categoria, de acordo com os requisitos da issue relacionada no backlog. O auxiliar de editoras já existe por causa do exercício anterior; preserve e refine esse trabalho conforme necessário ao concluir as mudanças restantes na camada de dados, na interface e nos testes. Confirme se todos os testes passam antes de concluir. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Observe o Copilot explorar o projeto, localizar os arquivos da camada de dados, da interface e dos testes, fazer mudanças e executar testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot pode levar alguns minutos para explorar o projeto, editar arquivos e executar testes. Enquanto trabalha, ele pode exibir os botões **Keep** e **Undo** no editor. Aguarde a conclusão antes de selecionar uma dessas opções.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Diálogo no painel Copilot Chat que solicita confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Alguns testes podem falhar durante o processo. O Copilot alterna entre gerar código e executar testes até concluir a tarefa e não detectar erros.

   ![Sessão concluída do Copilot no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)
9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Revise sempre o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador em que o site está aberto e explore o novo recurso.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximos passos

Você usou o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- o modo de agente do GitHub Copilot explora o projeto, identifica arquivos pertinentes e faz mudanças coordenadas.
- o modo de agente do Copilot implementa recursos na interface e na camada de dados.
- revisar as mudanças e os testes gerados pelo modo de agente do Copilot antes de integrá-los ao código.

Em seguida, [teste o recurso com o servidor MCP do Playwright][next-lesson] e abra um pull request para o trabalho de filtragem.

### Exercício extra: implementar paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use o que aprendeu neste exercício para pedir ao Copilot que adicione esse recurso. Considere:

- seguir as práticas recomendadas existentes, inclusive as dos arquivos de instruções.
- decidir se as pessoas usuárias poderão selecionar o tamanho da página ou se ele será fixo.
- incluir no prompt as orientações necessárias para obter a implementação desejada.
- iterar com o GitHub Copilot, pedindo mudanças e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot.

## Recursos

- [Copilot Ask, Edit e Agent: o que fazem e quando usar cada modo][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
