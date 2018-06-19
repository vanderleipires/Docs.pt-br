---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: Examinar os detalhes e métodos de exclusão | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples de seguir e demonstração...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 00f7e5d6679f1bd8875931e601c8151049f785ac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868770"
---
<a name="examining-the-details-and-delete-methods"></a>Examinar os detalhes e métodos de exclusão
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É muito mais simples a seguir, mais segura e demonstra mais recursos.


Nesta parte do tutorial, você examinará automaticamente gerado `Details` e `Delete` métodos.

## <a name="examining-the-details-and-delete-methods"></a>Examinar os detalhes e métodos de exclusão

Abra o `Movie` controlador e examine o `Details` método.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

O mecanismo de scaffolding MVC que criou este método de ação adiciona um comentário mostrando uma solicitação HTTP que chama o método. Nesse caso é um `GET` solicitação com três segmentos de URL, o `Movies` controlador, o `Details` método e uma `ID` valor.

Código primeiro torna mais fácil procurar dados usando o `Find` método. Um recurso de segurança importante criado para o método é que o código verifica se o `Find` método foi encontrado um filme antes do código tenta fazer com ele. Por exemplo, um hacker pode introduzir erros no site, alterando a URL criada por links de `http://localhost:xxxx/Movies/Details/1` para algo como `http://localhost:xxxx/Movies/Details/12345` (ou algum outro valor que não representa um filme real). Se você não marcar um filme nulo, um filme nulo resulta em um erro de banco de dados.

Examine os métodos `Delete` e `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Observe que o `HTTP Get``Delete` método não exclui o filme especificado, ele retorna uma exibição do filme que você pode enviar (`HttpPost`) a exclusão. A execução de uma operação de exclusão em resposta a uma solicitação GET (ou, de fato, a execução de uma operação de edição, criação ou qualquer outra operação que altera dados) abre uma falha de segurança. Para obter mais informações sobre isso, consulte a entrada de blog de Stephen Walther [ASP.NET MVC dica 46 — não use Excluir Links porque eles criar vulnerabilidades de segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

O método `HttpPost` que exclui os dados é chamado `DeleteConfirmed` para fornecer ao método HTTP POST um nome ou uma assinatura exclusiva. As duas assinaturas de método são mostradas abaixo:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

O CLR (Common Language Runtime) exige que os métodos sobrecarregados tenham uma assinatura de parâmetro exclusiva (mesmo nome de método, mas uma lista diferente de parâmetros). No entanto, aqui você precisa de dois métodos de exclusão – um para GET - e um POST que têm a mesma assinatura de parâmetro. (Ambos precisam aceitar um único inteiro como parâmetro.)

Para classificar este limite, você pode fazer algumas coisas. Uma é dar os métodos nomes diferentes. Foi isso o que o mecanismo de scaffolding fez no exemplo anterior. No entanto, isso apresenta um pequeno problema: o ASP.NET mapeia os segmentos de URL para os métodos de ação por nome e se você renomear um método, o roteamento normalmente não conseguirá encontrar esse método. A solução é o que você vê no exemplo, que é adicionar o atributo `ActionName("Delete")` ao método `DeleteConfirmed`. Isso executa efetivamente o mapeamento para o sistema de roteamento para que uma URL que inclui <em>/Delete/</em>um POST solicitação encontrará o `DeleteConfirmed` método.

Outra maneira comum para evitar um problema com métodos que têm nomes idênticos e assinaturas é artificialmente alterar a assinatura do método POST para incluir um parâmetro não utilizado. Por exemplo, alguns desenvolvedores adicionar um tipo de parâmetro `FormCollection` que é passado para o método POST e, em seguida, simplesmente não usar o parâmetro:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Resumo

Agora você tem um aplicativo ASP.NET MVC completo que armazena dados em um banco de dados local do banco de dados. Você pode criar, ler, atualizar, excluir e procurar filmes.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Próximas etapas

Depois de ter criado e testado um aplicativo web, a próxima etapa é torná-lo disponível para outros usuários na Internet. Para fazer isso, você precisa implantá-lo em um provedor de hospedagem na web. A Microsoft oferece gratuito da web de hospedagem para até 10 sites da web em um [conta de avaliação do Windows Azure gratuita](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Sugerimos que você seguir siga o tutorial [implantar um aplicativo de proteger o ASP.NET MVC com associação, OAuth e o banco de dados SQL para um Site do Windows Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Um tutorial excelente é nível intermediário de Tom Dykstra [criando um modelo de dados do Entity Framework para um aplicativo ASP.NET MVC](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) e [fóruns do ASP.NET MVC](https://forums.asp.net/1146.aspx) são um ótimo coloca para fazer perguntas. Execute [me](https://twitter.com/RickAndMSFT) no twitter para que você possa obter atualizações em meu tutoriais mais recentes.

Comentários são boas-vindas.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Anterior](adding-validation-to-the-model.md)
