---
uid: single-page-application/overview/templates/breezeknockout-template
title: Modelo Breeze/Knockout | Microsoft Docs
author: madskristensen
description: Modelo de aplicativo de página única do Breeze/Knockout
ms.author: aspnetcontent
ms.date: 01/30/2013
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: f816563b77aaeae0ef79b31ddb4e066c9764bb51
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813478"
---
<a name="breezeknockout-template"></a>Modelo Breeze/Knockout
====================
por [Mads Kristensen](https://github.com/madskristensen)

> Modelo Breeze/Knockout MVC foi escrito por Ward Bell
> 
> [Baixe o modelo do MVC Breeze/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)


Você já ouviu falar do "aplicativo de página única" (SPA) e se perguntou o que é. Enquanto você pode ler sobre isso, você faria em vez disso, a experiência por conta própria. Mas quem tem tempo para baixar um exemplo? Se você tiver o Visual Studio, você terá um SPA de exemplo para cima e em execução em menos de 60 segundos com o ASP.NET MVC 4 modelo de "Breeze/Knockout aplicativo de página única".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>O que é o modelo do SPA Breeze/Knockout?

A maioria dos modelos de projeto geram um esqueleto do aplicativo. Colocar pele nesses ossos adicionando seu código e, eventualmente, fornecer um aplicativo de trabalho. Modelo Breeze/Knockout SPA é diferente. Ele gera um aplicativo de exemplo para que você possa estudar. Ela demonstra um design de aplicativo do SPA e muitas das técnicas para a criação de um SPA.

Modelo Breeze/Knockout é uma variação sobre o [modelo KnockoutJS SPA](../introduction/knockoutjs-template.md) incluídas no ASP.NET e Web Tools 2012.2 atualização. O modelo de SPA Breeze gera um aplicativo com a mesma experiência de usuário, mas tem uma implementação diferente, usando a Breeze para gerenciamento de dados.

O modelo do SPA KnockoutJS faz solicitações de serviço com o jQuery bruto AJAX, que é adequado para um aplicativo simples. Mas aplicativos mais sofisticados têm requisitos mais exigentes de gerenciamento de dados. Por exemplo, a maioria dos aplicativos:

- Consultar e consultar novamente o servidor durante uma sessão de usuário estendido.
- Adicione filtros de consulta, classificação e paginação.
- Compartilhe os mesmos dados em várias telas.
- Acumular alterações a muitos objetos e, em seguida, salvá-los como uma única transação.
- Valide alterações no cliente, para que o usuário pode corrigir erros antes de confirmar as alterações no banco de dados.

A biblioteca de BreezeJS trata essas tarefas para você, liberando você para desenvolver a experiência de usuário e a lógica de aplicativo mais importantes.

[**Breeze** ](http://www.breezejs.com/?utm_source=ms-spa) é uma biblioteca de software livre para criar aplicativos de dados avançados no JavaScript e HTML, os tipos de aplicativos que tenham sido historicamente entregues como aplicativos de área de trabalho autônomos.

Modelo Breeze/Knockout ajuda você a fazer a primeira etapa crucial em direção a uma infra-estrutura mais robusta de gerenciamento de dados. Ele produz um aplicativo de tarefas pendentes de exemplo externamente idêntica ao modelo KnockoutJS SPA. No interior, ela substitui a camada de dados do AJAX com a Breeze, para que você possa comparar as duas abordagens de lado a lado. É claro, mal toca o potencial de um aplicativo muito simples. Mas você verá como a Breeze funciona e como é necessário para tornar essa transição.

Vamos começar.

## <a name="create-a-breezeknockout-template-project"></a>Criar um projeto de modelo Breeze/Knockout

Baixe e instale o modelo clicando no botão de Download acima. O modelo é empacotado como um arquivo de extensão VSIX (Visual Studio). Talvez você precise reiniciar o Visual Studio.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto e clique em **Okey**.

No **novo projeto** assistente, selecione **Breeze Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Pressione Ctrl-F5 para compilar e executar o aplicativo sem depuração ou pressione F5 para executar com depuração.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Quando o aplicativo é executado pela primeira vez, ele exibe uma tela de logon. Clique no link de "Inscrição" e uma nova página glides no modo de exibição, onde você pode inserir um nome de usuário e senha. (As páginas de logon e registro são criadas usando o ASP.NET MVC). Quando você envia o formulário de registro, o servidor gera uma lista de tarefas pendentes com dois itens para sua conta. Ele apresenta a você uma nota amarelo.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Agora você está no land do SPA. Tudo o que você vê e apresentar enquanto manipular Todos é renderizado e gerenciado no cliente com a Ajuda do Knockout e Breeze. Explore o aplicativo como um usuário... mas com o olho do desenvolvedor. Use as ferramentas de desenvolvedor em seu navegador para capturar o tráfego de rede. (No Internet Explorer: pressione F12, selecione a **rede** guia e, em seguida, clique em **inicia a captura**.) Agora, tente o seguinte:

- Adicione um novo item de tarefas pendentes.
- Clique no rótulo e editar o título do item de tarefas pendentes
- Marque uma caixa de seleção para marcar um item concluído. Observe que a caixa de texto está desabilitada, portanto, o título não é mais editável.
- Clique em 'x' à direita do rótulo. O item desaparece e é excluído do banco de dados.
- Escolher outro item e limpar seu título. Você obterá um erro de validação que o título é obrigatório. Após uma pequena pausa, o título anterior é restaurado.
- Digite um título ridiculamente longo. Você obterá um erro de validação diferente que o título é muito longo.
- Clique no botão "Adicionar a lista de tarefas". Uma nova lista é exibida à esquerda da lista anterior.
- Brincar com o título de lista de tarefas pendentes, acionar seu necessária e validações de comprimento.
- Clique na caixa de texto de título para limpar a mensagem de erro.
- Clique no "x" no círculo no canto superior direito para excluir a lista de tarefas e suas tarefas.

A lógica de validação é executada no lado do cliente pelo Breeze. Atributos de validação nas classes de modelo de servidor são propagados para o cliente e executados automaticamente antes do cliente contata o servidor.

Analise o tráfego de rede. Observe que não havia nenhuma chamada para o servidor quando a Breeze detectou um erro. Cada alteração válida resultou em uma solicitação POST para "/ api/Todo/SaveChanges". Breeze agrupa as alterações e envia-os juntos como uma única solicitação para o controlador de API da Web `SaveChanges` método. Isso é diferente do modelo KockoutJS SPA, o que torna o PUT, POST e excluir solicitações para cada item individualmente.

## <a name="peek-inside"></a>Olhar dentro

Este aplicativo tem um cliente e um servidor. A pilha do lado do cliente consiste em HTML uma pequena e uma combinação de módulos de JavaScript do aplicativo (na pasta "aplicativo") além de bibliotecas JavaScript de terceiros (na pasta "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Se você investigamos o modelo KnockoutJS SPA, isso deve ser bastante familiar. Focalizar as caixas azuis. A arquitetura de interface do usuário é Model-View-ViewModel (MVVM), na qual os widgets HTML da exibição corretamente são separados do código de apresentação suporte no modelo de exibição. Um sistema de associação de dados (neste caso, o Knockout) coordena a exibição e o modelo de exibição para que cada um pode fazer seu trabalho sem conhecimento profundo das outras.

O modelo encapsula os dados de tarefas pendentes. Entidades no modelo são construídas pelo Breeze com propriedades observáveis do Knockout, portanto, eles podem ser associados diretamente a widgets no modo de exibição. O modelo de exibição solicita que o contexto de dados para adquirir e salvar as entidades de modelo. O contexto de dados delega a maior parte do trabalho a Breeze.

A pilha do lado do servidor consiste em um código de desenvolvedor e três bibliotecas .NET de princípio: Breeze.NET, Entity Framework e API da Web:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

A arquitetura básica é o mesmo que o modelo do SPA KockoutJS. No entanto, a implementação é muito mais simples: O DTOs foram excluídos, e a maioria dos detalhes do Entity Framework foram delegada a Breeze.NET.

## <a name="next-steps"></a>Próximas etapas

Sugerimos que você explore o código, orientado pela [discussão abrangente](http://www.breezejs.com/spa-template?utm_source=ms-spa) do cliente e as pilhas de servidor no site da Breeze.

Você pode experimentar, brincar com consulta do lado do cliente Breeze; Adicione alguns filtros e classificações. Você pode adicionar mais propriedades de modelo e entidades mais para obter uma ideia melhor para desenvolvimento de ponta a ponta do SPA. Quando estiver certo do design, você pode desfazer os recursos de Todo e substitua-os pelos seus próprios.

Em breve, você estará pronto para a próxima grande etapa: Adicionando telas do lado do cliente e navegar entre eles. Você deixar este modelo do SPA para trás e ativar a uma pilha SPA mais completa, como [toalha quente do John Papa](https://github.com/johnpapa/HotTowel#readme "Hot Towel"), que adiciona Durandal à combinação de Breeze e Knockout.
