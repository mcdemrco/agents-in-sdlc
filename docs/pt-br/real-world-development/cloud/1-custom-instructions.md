---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O contexto é essencial ao trabalhar com inteligência artificial (IA) generativa. Se uma tarefa precisar ser feita de determinada maneira — ou houver informações que o Copilot deva conhecer —, esse contexto precisa estar disponível. Os [arquivos de instruções][instruction-files] fornecem orientações para que o Copilot entenda não apenas *o que* você quer, mas também *como* o trabalho deve ser feito.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e dos arquivos de instruções aplicados a caminhos específicos.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, aqui você não fará uma comparação antes e depois do prompt. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o efeito lado a lado. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests criados pelo agente.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir comentários de documentação de TypeScript (TSDoc).
- Cada arquivo deve começar com um bloco de comentários que descreva sua finalidade.

Com os arquivos de instruções, você fornecerá ao Copilot as informações necessárias para seguir essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor o estilo e os requisitos do projeto. Você pode especificar convenções de código, bibliotecas e tipos de comentários preferidos. Crie instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em todas as solicitações do repositório. Inclua nele informações gerais relevantes para a maioria das solicitações, como a pilha de tecnologias, uma visão geral do projeto, práticas recomendadas e outras orientações globais.
- `.github/instructions/*.instructions.md`: arquivos com diretrizes para linguagens, tipos de arquivo ou tarefas específicas, como criar um componente de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para sugestões de conclusão de código ou de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [aceita outros padrões de arquivos de instruções][custom-instructions-support], como `AGENTS.md` e `CLAUDE.md`.

### Práticas recomendadas para gerenciar arquivos de instruções

O projeto de exemplo demonstra uma abordagem representativa para organizar arquivos de instruções:

- Mantenha `copilot-instructions.md` focado nas orientações gerais do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo — como testes unitários, componentes Astro e a camada de dados — ou de tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos do que o código deve ou não deve fazer.

Não há uma única maneira certa de criar arquivos de instruções ou de usar IA. Experimente para descobrir o que funciona melhor no projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Entre os arquivos deste projeto, há orientações para [mudanças na interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa função de maneira diferente, por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. O exercício correspondente explicará a opção adequada para cada interface.
>
> Para encontrar modelos e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas do projeto

Leia os arquivos de instruções incluídos no repositório pelo GitHub ou no Codespace.

1. Abra `.github/copilot-instructions.md`.
2. Leia a descrição breve do projeto e as seções **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Observe também as orientações aninhadas **GitHub Actions Workflows** em **Code standards**. Essas instruções se aplicam às interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`. Ele define um padrão glob relativo à raiz do repositório. Neste projeto, arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`, serão incluídos.
5. Observe as instruções específicas para criar testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes. Os caminhos são relativos ao próprio arquivo de instruções. Assim, você pode dividir conjuntos grandes de instruções em arquivos menores e reutilizáveis e apontar o Copilot para exemplos.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Adicionar um novo padrão para o repositório

Adicione um padrão do projeto que exija comentários TSDoc e um comentário no início de cada arquivo. O agente de nuvem usará essa orientação nas issues atribuídas mais adiante neste percurso.

1. No terminal do Codespace, crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Reabra `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27.
4. Logo abaixo dos padrões existentes, adicione estas linhas:

   ```markdown
   - Toda função exportada deve ter um comentário TSDoc que descreva sua finalidade, seus parâmetros e seu valor de retorno.
   - Antes dos imports ou de qualquer código, adicione um bloco de comentários que explique a finalidade do arquivo.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> As instruções podem ficar no nível do repositório, em `.github/copilot-instructions.md`, ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo e tarefas específicas. O arquivo do repositório é o local adequado para padrões gerais, como a regra de comentários que você acabou de adicionar.

## Fazer commit, enviar e integrar a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`. Portanto, faça o merge das mudanças nas instruções em `main` para que o agente de nuvem possa usá-las.

1. Prepare e faça commit da mudança:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem poderá usar essas instruções nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, pule a criação da branch e faça commit diretamente em `main`. A etapa da branch demonstra como trabalhar em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e adicionou um padrão para todo o repositório em `.github/copilot-instructions.md`. Você verá a influência desse padrão nos pull requests que o agente de nuvem criar neste percurso.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
