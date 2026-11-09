---
title: "Lição 12 - Implantar o site com o GitHub Pages"
description: "Use o aplicativo GitHub Copilot para configurar um fluxo de trabalho do Pages, revisar o pull request e verificar o Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dê ao Space Quiz um endereço público implantando-o com o GitHub Pages. O Copilot pode preparar o fluxo de trabalho e orientar a implantação, mas cabe a você revisar as mudanças e confirmar que o site está disponível.

Nesta lição, você vai:

- pedir ao Copilot para configurar uma implantação no GitHub Pages.
- revisar o fluxo de trabalho e o pull request antes de fazer o merge.
- acompanhar a implantação e verificar o site publicado.

## Cenário

O Space Quiz já está em um repositório do GitHub, mas as pessoas ainda precisam de uma forma simples de experimentá-lo. Publicar o site de arquivo único com o GitHub Pages disponibiliza o site em uma URL pública, sem adicionar um servidor ou dependências de runtime.

## Peça ao Copilot para implantar o site

1. Na sessão do aplicativo Copilot para `space-quiz`, envie este prompt:

    ```plaintext
    Habilite o GitHub Pages para este repositório. Crie um fluxo de trabalho do GitHub Actions que compile o site, se necessário, e o implante no GitHub Pages. Adicione um README que descreva o aplicativo e explique como executá-lo localmente, caso ainda não exista. Crie um pull request com essas mudanças e faça o merge depois que as verificações obrigatórias forem aprovadas. Acompanhe a implantação até que ela seja concluída com êxito e informe o endereço (URL) final do site publicado. Se você não tiver permissão para habilitar o Pages ou fazer o merge, diga exatamente o que preciso aprovar ou fazer; não informe que a tarefa foi concluída até que o site esteja disponível.
    ```

![Diagrama em três etapas: um pull request do space-quiz integrado; uma execução bem-sucedida de Deploy website no GitHub Actions, com as etapas Build website, Upload artifact e Deploy to Pages; e o Space Quiz publicado em yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pede ao agente que crie e integre um pull request e altere as configurações do Pages do repositório. Revise as diferenças, as permissões de implantação e quaisquer solicitações de aprovação do GitHub antes de continuar.

## Revise o fluxo de trabalho e verifique a implantação

Revise as mudanças propostas e acompanhe a execução até confirmar que o site está disponível.

1. Confirme se o `README` foi adicionado somente quando ainda não havia um no repositório.
2. Revise as mudanças do pull request, incluindo as etapas de compilação e implantação do fluxo de trabalho.
3. Faça o merge somente depois que as verificações obrigatórias do pull request forem aprovadas.
4. Acompanhe a execução de implantação do GitHub Actions até que ela seja concluída com êxito.
5. Abra a URL informada e confirme se o Space Quiz carrega.

Se o agente não puder habilitar o Pages ou fazer o merge do pull request, siga as instruções específicas que ele fornecer. Não considere a tarefa concluída até que a implantação seja bem-sucedida e o site esteja disponível.

## Resumo e próximos passos

Agora o Space Quiz está disponível como um site publicado, e você verificou a implantação em vez de confiar apenas no relato do agente. Continue para [Lição 13: Revisão e próximos passos][next-lesson].

[next-lesson]: ../13-review/
