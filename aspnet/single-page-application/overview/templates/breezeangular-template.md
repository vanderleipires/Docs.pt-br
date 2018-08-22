---
uid: single-page-application/overview/templates/breezeangular-template
title: Modelo Breeze/Angular | Microsoft Docs
author: madskristensen
description: Modelo de aplicativo de página única do Breeze/Angular
ms.author: riande
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: a3e8b42cdadf99df6971a278834b1429e129ce72
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834501"
---
<a name="breezeangular-template"></a>Modelo Breeze/Angular
====================
por [Mads Kristensen](https://github.com/madskristensen)

> Modelo Breeze/Angular MVC foi escrito por Ward Bell
> 
> [Baixe o modelo MVC Breeze/Angular](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) é uma biblioteca de software livre do Google para a criação de aplicativos de página única (SPAs). Ele oferece gerenciamento de tela, injeção de dependência e vinculação de dados. Combine-o com [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), outra biblioteca de software livre para modelagem de dados e gerenciamento de dados e você tiver os ingredientes essenciais para um ótimo aplicativo cliente HTML/JavaScript.

Modelo Breeze/Angular SPA é uma variação sobre o [modelo KnockoutJS SPA](../introduction/knockoutjs-template.md) incluídas no ASP.NET e Web Tools 2012.2 atualização. Se você tiver o Visual Studio, você terá um SPA de exemplo em funcionamento em menos de 60 segundos.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Externamente, o aplicativo se parece muito semelhante ao modelo KnockoutJS SPA. Mas é bastante diferente nos bastidores. O modelo KnockoutJS usa o Knockout para vinculação de dados e AJAX bruto para acesso a dados. Modelo Breeze/Angular usa Angular para vinculação de dados e a Breeze para acesso a dados. Essas bibliotecas de habilitar recursos adicionais, incluindo o histórico e navegação de página.

Aqui está a página sobre do aplicativo:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Esta página exibe um log de eventos durante a sessão atual do usuário, incluindo:

- Paginação. Observe a criação do controlador de tarefas pendentes em 2 de # e #7.
- Consultas remotas (3) e consultas do cache local (#7).
- Salvar novo (n º 5, 6 de #) e modificação de entidades (4).
- Validado no cliente (#9), para que o usuário pode corrigir erros antes de confirmar as alterações no banco de dados de alterações.

Não há mais para explorar neste modelo, incluindo:

- Carregamento dinâmico de modelos de exibição HTML.
- Ligação de dados personalizados por meio do Angular "diretivas".
- A modularidade e injeção de dependência.
- Filtros de consulta, classificações, paginação, projeções e inclusão de entidades relacionadas.
- Compartilhando dados entre várias telas.
- Salvando várias alterações como uma única transação.
- Regras de validação propagadas automaticamente do servidor para o cliente JavaScript.

Vamos começar.

## <a name="create-a-breezeangular-template-project"></a>Criar um projeto de modelo Breeze/Angular

Baixe e instale o modelo clicando no botão de Download acima. O modelo é empacotado como um arquivo de extensão VSIX (Visual Studio). Talvez você precise reiniciar o Visual Studio.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto e clique em **Okey**.

No **novo projeto** assistente, selecione **SPA do Angular Breeze**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Pressione Ctrl-F5 para compilar e executar o aplicativo sem depuração ou pressione F5 para executar com depuração.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- Clique no link "Sobre" no canto superior direito para ver um log dessas atividades.

A lógica de validação é executada no lado do cliente pelo Breeze. Atributos de validação nas classes de modelo de servidor são propagados para o cliente e executados automaticamente antes do cliente contata o servidor.

Analise o tráfego de rede. Observe que não havia nenhuma chamada para o servidor quando a Breeze detectou um erro. Cada alteração válida resultou em uma solicitação POST para "/ api/Todo/SaveChanges". Breeze agrupa as alterações e envia-os juntos como uma única solicitação para o controlador de API da Web `SaveChanges` método. Isso é diferente do modelo KockoutJS SPA, o que torna o PUT, POST e excluir solicitações para cada item individualmente.

Além disso, observe que não há nenhum tráfego de rede quando você alterna entre a lista de tarefas pendentes e sobre as páginas. Isso ocorre porque a consulta tiver sido restrito para o cache local da Breeze.

## <a name="peek-inside"></a>Olhar dentro

Este aplicativo tem um cliente e um servidor. A pilha do lado do cliente consiste em HTML uma pequena e uma combinação de módulos de JavaScript do aplicativo (na pasta "aplicativo") além de bibliotecas JavaScript de terceiros (na pasta "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

A arquitetura de interface do usuário separa os widgets HTML dos modos de exibição do código de apresentação suporte nos controladores. O sistema de associação de dados Angular coordena as exibições e controladores para que cada um pode fazer seu trabalho sem conhecimento profundo das outras.

O controlador solicita que o contexto de dados para adquirir e salvar as entidades de modelo. O contexto de dados delega a maior parte do trabalho a Breeze, que constrói objetos de modelo de rastreamento automático dos resultados de consulta JSON.

A pilha do lado do servidor consiste em um código de desenvolvedor e três bibliotecas .NET de princípio: Breeze.NET, Entity Framework e API da Web:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

A arquitetura básica é o mesmo que o modelo do SPA KockoutJS. No entanto, a implementação é muito mais simples: O DTOs foram excluídos, e a maioria dos detalhes do Entity Framework foram delegada a Breeze.NET.

## <a name="next-steps"></a>Próximas etapas

Sugerimos que você explore o código, orientado pela [discussão abrangente](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) do cliente e as pilhas de servidor no site da Breeze.

Você pode experimentar, brincar com consulta do lado do cliente Breeze; Adicione alguns filtros e classificações. Você pode adicionar mais propriedades de modelo e entidades mais para obter uma ideia melhor para desenvolvimento de ponta a ponta do SPA. Quando estiver certo do design, você pode desfazer os recursos de Todo e substitua-os pelos seus próprios.

Boa codificação!
