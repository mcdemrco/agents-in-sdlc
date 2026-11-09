---
title: "Lição 5 - Personalizar e usar uma skill quality-checks"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desenvolver não se resume a escrever código. Você já confirmou manualmente que o código funciona e usou arquivos de instruções para garantir que ele siga os padrões. E quanto a testes, lint e outras partes da integração contínua (CI)?

Para essas tarefas, as **skills de agente** são uma boa opção: ajudam o Copilot a entender como executar operações repetíveis.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificação de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente agrupam instruções reutilizáveis, scripts executáveis e recursos de apoio que o agente carrega quando necessário. No mínimo, uma skill é uma pasta com um arquivo Markdown chamado `SKILL.md`. O arquivo tem frontmatter com `name` e `description`, uma visão geral da tarefa e orientações sobre quando usá-la. A pasta também pode conter subpastas com scripts e outros recursos.

> [!NOTE]
> Pastas e arquivos adicionais são opcionais. A skill `quality-checks` da Tailspin Toys contém somente `SKILL.md`, pois usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta pessoal `~/.copilot/skills`.

## Explorar a skill

Leia a skill `quality-checks` que a equipe da Tailspin Toys criou para executar testes unitários, lint e verificações de tipos.

1. Volte ao Codespace e, no editor, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a decidir quando usar a skill.
3. Leia as instruções e observe como orientam o processo de teste e lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Executar a skill antes de fazer mudanças

Você pode chamar skills diretamente pelo Copilot CLI ou pedir que o agente as use em linguagem natural. Peça ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Envie este prompt:

   ```plaintext
   Execute a skill quality-checks para os testes unitários, lint e verificação de tipos.
   ```

3. Leia o relatório apresentado ao final.

## Personalizar o relatório

Melhore o relatório para mostrar o que foi executado, se houve sucesso e quais resultados as ferramentas informaram.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Formato do relatório de resultados

   Ao concluir, informe cada comando executado e se ele foi aprovado, reprovado ou bloqueado. Inclua contagens de testes, durações, erros, avisos e outras métricas somente quando a ferramenta as apresentar. Indique a próxima ação para cada falha ou bloqueio e nunca descreva uma verificação ignorada ou incompleta como aprovada.
   ```

3. O arquivo será salvo automaticamente.

## Executar a skill atualizada

O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa. Execute novamente a skill e compare o relatório.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Envie o mesmo prompt usado antes:

   ```plaintext
   Execute a skill quality-checks para os testes unitários, lint e verificação de tipos.
   ```

3. Leia o relatório e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificação de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa mudança acompanhará a filtragem no PR do recurso. Em seguida, permita que o Copilot interaja diretamente com o site [usando o servidor Model Context Protocol (MCP) do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade servem como referência, não como tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
