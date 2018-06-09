---
uid: single-page-application/overview/templates/breezeangular-template
title: Modelo muito fácil/Angular | Microsoft Docs
author: madskristensen
description: Modelo de aplicativo de página única muito fácil/Angular
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506685"
---
<a name="breezeangular-template"></a>Modelo muito fácil/Angular
====================
por [Mads Kristensen](https://github.com/madskristensen)

> O modelo MVC muito fácil/Angular foi gravado pelo Ward Bell
> 
> [Baixe o modelo MVC muito fácil/Angular](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) é uma biblioteca de código aberto do Google para a criação de aplicativos de página única (SPAs). Ele oferece gerenciamento de tela, injeção de dependência e associação de dados. Combinar com [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), outra biblioteca de software livre para modelagem de dados e gerenciamento de dados e você tiver os componentes essenciais de um grande aplicativo cliente HTML/JavaScript.

O modelo muito fácil/Angular SPA é uma variação no [modelo SPA KnockoutJS](../introduction/knockoutjs-template.md) incluído no ASP.NET e Web Tools 2012.2 atualização. Se você tiver o Visual Studio, você terá um exemplo SPA em execução em menos de 60 segundos.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Uso externo, o aplicativo parece muito semelhante ao modelo KnockoutJS SPA. Mas é bastante diferente nos bastidores. O modelo de KnockoutJS usa Knockout para associação de dados e AJAX bruto para acesso a dados. O modelo muito fácil/Angular usa Angular para associação de dados e muito fácil para acesso a dados. Essas bibliotecas de habilitar recursos adicionais, incluindo o histórico e navegação de página.

Aqui está a página do aplicativo sobre:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Esta página exibe um log de eventos durante a sessão atual do usuário, incluindo:

- Paginação. Observe a criação do controlador de tarefas em 2 de # e #7.
- Consultas remotas (3) e consultas de cache local (7).
- Salvar novas (n º 5, 6 #) e modificação de entidades (4).
- Alterações validadas no cliente (n º 9), para que o usuário pode corrigir os erros antes de confirmar as alterações no banco de dados.

Há mais para explorar neste modelo, incluindo:

- Carregamento dinâmico de modelos de exibição HTML.
- Associação de dados personalizados por meio de Angular "diretivas".
- Injeção de dependência e modularidade.
- Filtros de consulta, classificações, paginação, projeções e inclusão de entidades relacionadas.
- Compartilhamento de dados em várias telas.
- Salvar várias alterações como uma única transação.
- Regras de validação propagadas automaticamente do servidor ao cliente JavaScript.

Vamos começar.

## <a name="create-a-breezeangular-template-project"></a>Criar um projeto de modelo muito fácil/Angular

Baixe e instale o modelo, clique no botão de Download acima. O modelo é empacotado como um arquivo de extensão de Visual Studio (VSIX). Talvez seja necessário reiniciar o Visual Studio.

No painel **Modelos**, selecione **Modelos Instalados** e expanda o nó **Visual C#**. Em **Visual C#**, selecione **Web**. Na lista de modelos de projeto, selecione **aplicativo Web do ASP.NET MVC 4**. Nomeie o projeto e clique em **Okey**.

No **novo projeto** assistente, selecione **SPA Angular muito fácil**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Pressione Ctrl-F5 para compilar e executar o aplicativo sem depuração, ou pressione F5 para executar com depuração.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

Quando o aplicativo é executado pela primeira vez, ele exibe uma tela de logon. Clique no link "Inscrição" e uma nova página glides no modo de exibição, onde você pode inserir um nome de usuário e senha. (As páginas de logon e registro são criadas usando o ASP.NET MVC). Quando você envia o formulário de registro, o servidor gera uma lista de tarefas com dois itens para sua conta. Em seguida, apresenta-los para você em uma nota amarela.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Agora você está na Terra da controladora. Tudo o que você vê e experiência enquanto manipular Todos é renderizado e gerenciado no cliente com a Ajuda do Knockout e muito fácil. Explore o aplicativo como um usuário... mas com olho do desenvolvedor. Use as ferramentas de desenvolvedor em seu navegador para capturar o tráfego de rede. (No Internet Explorer: pressione F12, selecione o **rede** guia e, em seguida, clique em **iniciar a captura**.) Agora, tente o seguinte:

- Adicione um novo item de tarefas.
- Clique no rótulo e editar o título do item de tarefas
- Marque a caixa de seleção para marcar o item concluído. Observe que a caixa de texto está desabilitada, portanto o título não é mais editável.
- Clique em 'x' à direita do rótulo. O item desaparece e é excluído do banco de dados.
- Escolha outro item e limpar seu título. Você obterá um erro de validação que o título é necessário. Após uma pequena pausa, o título do anterior é restaurado.
- Digite um título ridiculamente longo. Você obterá um erro de validação diferente que o título é muito longo.
- Clique no botão "Adicionar a lista de tarefas". Uma nova lista aparece à esquerda da lista anterior.
- Executar com o título da lista de tarefas, acionando necessário e validações de comprimento.
- Clique na caixa de texto título para limpar a mensagem de erro.
- Clique no círculo no canto superior direito para excluir a lista de tarefas e seu todos "x".
- Clique no link "Sobre" no canto superior direito para ver um log dessas atividades.

A lógica de validação é executada no lado do cliente por muito fácil. Atributos de validação em classes de modelo do servidor são propagados para o cliente e executados automaticamente antes do cliente contata o servidor.

Analise o tráfego de rede. Observe que não houve nenhuma chamada para o servidor quando muito fácil detectou um erro. Cada alteração válida resultou em uma solicitação POST para "/ tarefas/api/SaveChanges". Muito fácil agrupa as alterações e as envia juntos como uma única solicitação para o controlador de API da Web `SaveChanges` método. Isso é diferente do modelo do SPA KockoutJS, o que torna PUT, POST e excluir solicitações de cada item individualmente.

Além disso, observe que não há nenhum tráfego de rede quando você alternar entre a lista de tarefas e sobre as páginas. Isso ocorre porque a consulta tiver sido restrito ao cache local muito fácil.

## <a name="peek-inside"></a>Inspecionar dentro

Este aplicativo tem um cliente e no lado do servidor. A pilha do lado do cliente consiste em HTML um pouco e uma combinação de módulos de JavaScript do aplicativo (na pasta "aplicativo") e bibliotecas JavaScript de terceiros (na pasta "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

A arquitetura de interface de usuário separa os widgets HTML dos modos de exibição do código de apresentação suporte nos controladores. O sistema de associação de dados Angular coordena controladores e exibições para que cada uma pode fazer seu trabalho sem conhecimento profundo do outro.

O controlador solicita o contexto de dados para adquirir e salvar as entidades de modelo. O contexto de dados delega a maior parte do trabalho para muito fácil, que cria objetos de modelo de rastreamento automático JSON de resultados da consulta.

A pilha do lado do servidor consiste em um código de desenvolvedor e três bibliotecas de entidade de segurança do .NET: API da Web, Entity Framework e Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

A arquitetura básica é o mesmo que o modelo KockoutJS SPA. No entanto, a implementação é muito mais simples: O DTOs foram excluídos e a maioria dos detalhes do Entity Framework foram delegada a Breeze.NET.

## <a name="next-steps"></a>Próximas etapas

Sugerimos que você explore o código, seguindo o [discussão abrangente](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) do cliente e as pilhas de servidor no site muito fácil.

Você pode tentar brincando com consultas de cliente muito fácil; Adicione filtros e classificações. Você pode adicionar mais propriedades do modelo e mais entidades para entender melhor para o desenvolvimento de SPA de ponta a ponta. Quando você tiver certeza do design, você pode desfazer os recursos de Todo e substituí-los com seus próprios.

Boa codificação!
