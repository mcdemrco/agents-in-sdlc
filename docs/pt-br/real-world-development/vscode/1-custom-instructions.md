---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de determinada maneira ou o Copilot precisar conhecer informações adicionais, deixe esse contexto acessível. Os [arquivos de instruções][instruction-files] orientam a geração de código e explicam como ele deve ser estruturado.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e dos arquivos aplicados a caminhos específicos.
- gerar a primeira parte dos dados para a filtragem — um auxiliar de editoras — com as instruções atuais.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviar o mesmo prompt novamente e observar o código gerado seguir o novo padrão.
- fazer commit das instruções e da parte de filtragem em `main` para que o Copilot use as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo é observar a tendência de mudança após atualizar as instruções, não obter resultados idênticos caractere por caractere.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir documentação em comentários TSDoc.
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

Os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado nas orientações gerais, como uma descrição do projeto, sua estrutura e padrões globais de código.
- Use arquivos `*.instructions.md` para tipos de arquivo, como testes unitários, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras. Inclua exemplos do que o código deve ou não deve fazer.

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

Leia os arquivos de instruções que acompanham o repositório no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Leia a descrição breve do projeto e as seções **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Observe também as orientações aninhadas **GitHub Actions Workflows** em **Code standards**.
3. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`. Ele define um padrão glob relativo à raiz do repositório. Neste projeto, arquivos de teste TypeScript, como os que correspondem a `**/*.test.ts`, serão incluídos.
5. Observe as instruções específicas para criar testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes. Os caminhos são relativos ao próprio arquivo de instruções. Isso permite dividir instruções grandes em arquivos reutilizáveis e apontar o Copilot para exemplos.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Criar uma branch para as mudanças

Crie uma branch para trabalhar nas mudanças.

1. Volte ao Codespace do exercício anterior.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma nova branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar o Copilot Chat antes de atualizar as instruções

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de continuar, volte ao Codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas indicarão as opções necessárias.

Para observar o efeito das instruções personalizadas, envie um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente para comparar os resultados.

1. Feche as guias de editor abertas nos exercícios anteriores para limitar o contexto disponível ao Copilot.
2. Abra `src/lib/publishers.ts` para indicar onde o auxiliar deve ficar.
3. No seletor de agentes da exibição Chat, selecione **Agent** para que o Copilot possa alterar arquivos.

   ![Seletor de agentes na exibição Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um auxiliar de acesso a dados que retorne uma lista de todas as editoras, com o nome e a ID de cada uma. Aplique as mudanças nos arquivos.
   ```

5. O Copilot explorará o projeto e aplicará mudanças, geralmente ao arquivo do auxiliar e aos testes.
6. Observe que a função proposta é tipada, recebe um cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Essas convenções vêm de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observe se a proposta não inclui comentários TSDoc nem um comentário no início do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Tudo bem: o objetivo é observar a consistência depois da atualização das instruções.

## Adicionar um novo padrão para o repositório

O arquivo `.github/copilot-instructions.md` fornece orientações gerais ao Copilot. Adicione a ele um padrão de documentação para melhorar as sugestões de código.

1. Reabra `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27.
3. Logo abaixo dos padrões existentes, adicione estas linhas:

   ```markdown
   - Toda função exportada deve ter um comentário TSDoc que descreva sua finalidade, seus parâmetros e seu valor de retorno.
   - Antes dos imports ou de qualquer código, adicione um bloco de comentários que explique a finalidade do arquivo.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> As instruções podem ficar no nível do repositório, em `.github/copilot-instructions.md`, ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo e tarefas específicas. O arquivo do repositório é o local adequado para padrões gerais, como a regra de comentários que você acabou de adicionar.

## Enviar o mesmo prompt novamente e observar a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Volte a `src/lib/publishers.ts` para que o Copilot se concentre no arquivo correto.
3. Envie novamente o mesmo prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um auxiliar de acesso a dados que retorne uma lista de todas as editoras, com o nome e a ID de cada uma. Aplique as mudanças nos arquivos.
   ```

4. Observe se o arquivo proposto começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Auxiliares de acesso aos dados de editoras da plataforma de financiamento coletivo da Tailspin Toys.
    * Fornece funções para recuperar informações de editoras no banco de dados.
    */
   ```

5. Observe se a função proposta agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de editoras com suas IDs e seus nomes.
    *
    * @param db - O cliente do banco de dados Drizzle.
    * @returns Uma promessa que é resolvida com uma matriz de objetos de editora.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo a um código que será usado no próximo exercício.

## Fazer commit das instruções e enviar a branch

Os arquivos de instruções são controlados pelo Git como os outros arquivos do repositório. Faça commit das instruções e das mudanças do auxiliar.

1. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
2. Confirme se as atualizações estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit das instruções e de todos os arquivos alterados pelo Copilot para criar a base do auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras mudanças de apoio (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Criar e integrar um pull request

Depois de enviar a branch, crie um pull request associado à issue sobre o padrão de documentação. O Copilot pode localizar a issue e criar o pull request com essa associação.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a exibição Copilot Chat e confirme se **Agent** está selecionado no seletor de agentes.
3. Peça ao Copilot que localize a issue sobre as instruções e crie um PR a partir da branch atual, descrevendo também o auxiliar de editoras:

   ```plaintext
   Encontre a issue relacionada à atualização do arquivo de instruções. Crie um pull request a partir da branch atual, indique que ele fecha essa issue e mencione também que adicionamos a base do auxiliar de editoras para o trabalho de filtragem do próximo exercício.
   ```

4. Aguarde enquanto o Copilot localiza a issue e cria o PR.
5. Quando solicitado a permitir ações do GitHub, revise o comando e selecione **Allow** se estiver de acordo.
6. Depois que o PR for criado, peça ao Copilot que faça o merge em `main` e atualize a branch local:

   ```plaintext
   Integre o PR em main. Depois, volte para main localmente e obtenha o código mais recente.
   ```

7. Quando solicitado a permitir ações do GitHub ou comandos do shell, revise cada comando e selecione **Allow** se estiver de acordo.

Você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções existentes.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.
- enviar novamente o mesmo prompt e observar o código proposto seguir o novo padrão.
- fazer commit das instruções e da base do auxiliar em `main` para que o próximo exercício possa usá-las.

Em seguida, use as instruções no [modo de agente][next-lesson] para adicionar um recurso em vários arquivos.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot: coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
