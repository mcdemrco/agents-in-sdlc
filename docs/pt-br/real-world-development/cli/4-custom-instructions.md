---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com inteligência artificial (IA) generativa. Se uma tarefa precisar ser feita de determinada maneira, forneça essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *o que* você quer, mas também *como* o código deve ser estruturado. Agora que você implementou a filtragem, explore as instruções usadas pelo Copilot, adicione um padrão de documentação e aplique-o ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções aplicados a caminhos específicos chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- Os comentários devem explicar a intenção e decisões não óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, seus parâmetros e valores de retorno com documentação de TypeScript (TSDoc) ou de JavaScript (JSDoc), incluindo um argumento `db` injetável quando houver.
- Os componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para seguir essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo e os requisitos do projeto. Você pode especificar convenções de código, bibliotecas e tipos de comentários preferidos. Crie instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em todas as solicitações do repositório. Inclua nele informações gerais relevantes para a maioria das solicitações.
- `.github/instructions/*.instructions.md`: arquivos com diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções personalizadas do projeto

O projeto inicial já inclui arquivos de instruções. Explore o conteúdo existente antes de fazer mudanças para entender o efeito que eles têm.

1. Volte ao Codespace.
2. No editor do Codespaces, não no terminal, abra `.github/copilot-instructions.md`.
3. Leia a descrição breve do projeto e as orientações de código. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início do arquivo. Ele define um padrão glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos do projeto. Assim, você pode dividir conjuntos grandes de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos.

## Atualizar as instruções para refletir as diretrizes da equipe

Os arquivos existentes são um bom começo, mas ainda falta uma orientação. Atualize `copilot-instructions.md` para exigir comentários TSDoc no TypeScript criado.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar perto da linha 35.
2. Adicione o item a seguir como último tópico da lista:

   ```markdown
   - Todo novo código TypeScript deve incluir comentários TSDoc para fins de documentação.
   ```

O arquivo será salvo automaticamente.

## Usar as instruções atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções, saia e retome a conversa de filtragem para que o Copilot carregue as novas orientações sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para fechá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme se a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   Atualizamos as instruções e as diretrizes de código. Atualize o código que você gerou para seguir essas orientações.
   ```

4. Digite `/diff` e revise os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme se explicam o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI obtém contexto dos arquivos de instruções e aplicou um novo padrão ao recurso. Você:

- explorou como as instruções do repositório e os arquivos de instruções aplicados a caminhos específicos chegam ao agente.
- atualizou o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Em seguida, [personalize e execute a skill reutilizável `quality-checks`][next-lesson] para executar lint e testes de maneira consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot: coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
