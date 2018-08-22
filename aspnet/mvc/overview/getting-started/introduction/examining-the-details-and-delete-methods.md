---
uid: mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
title: Examinando os métodos Details e Delete | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 03/26/2015
ms.assetid: f1d2a916-626c-4a54-8df4-77e6b9fff355
msc.legacyurl: /mvc/overview/getting-started/introduction/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 3ec5a9a91cf1e2273f91a529779936fd26182fc4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829988"
---
<a name="examining-the-details-and-delete-methods"></a>Examinando os métodos Details e Delete
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Nesta parte do tutorial, você examinará automaticamente gerado `Details` e `Delete` métodos.

## <a name="examining-the-details-and-delete-methods"></a>Examinando os métodos Details e Delete

Abra o `Movie` controlador e examine o `Details` método.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

O mecanismo de scaffolding MVC que criou este método de ação adiciona um comentário mostrando uma solicitação HTTP que invoca o método. Nesse caso, ele é um `GET` solicitação com três segmentos de URL, o `Movies` controlador, o `Details` método e um `ID` valor.

Código primeiro facilita a pesquisa de dados usando o `Find` método. Um recurso de segurança importante integrado do método é que o código verifica se o `Find` método encontrou um filme antes do código tenta fazer algo com ele. Por exemplo, um hacker pode introduzir erros no site alterando a URL criada pelos links de `http://localhost:xxxx/Movies/Details/1` para algo como `http://localhost:xxxx/Movies/Details/12345` (ou algum outro valor que não representa um filme real). Se você não marcou um filme nulo, um filme nulo resultaria em um erro de banco de dados.

Examine os métodos `Delete` e `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Observe que o HTTP GET `Delete` método não exclui o filme especificado, retorna uma exibição do filme em que você pode enviar (`HttpPost`) a exclusão. A execução de uma operação de exclusão em resposta a uma solicitação GET (ou, de fato, a execução de uma operação de edição, criação ou qualquer outra operação que altera dados) abre uma falha de segurança. Para obter mais informações sobre isso, consulte a entrada de blog de Stephen Walther [ASP.NET MVC dica n º 46 — não use Excluir Links porque eles criaram buracos na segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

O método `HttpPost` que exclui os dados é chamado `DeleteConfirmed` para fornecer ao método HTTP POST um nome ou uma assinatura exclusiva. As duas assinaturas de método são mostradas abaixo:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

O CLR (Common Language Runtime) exige que os métodos sobrecarregados tenham uma assinatura de parâmetro exclusiva (mesmo nome de método, mas uma lista diferente de parâmetros). No entanto, aqui você precisa de dois métodos de exclusão – um para GET - e outro para POST que têm a mesma assinatura do parâmetro. (Ambos precisam aceitar um único inteiro como parâmetro.)

Para classificá-la, você pode fazer algumas coisas. Uma é fornecer aos métodos nomes diferentes. Foi isso o que o mecanismo de scaffolding fez no exemplo anterior. No entanto, isso apresenta um pequeno problema: o ASP.NET mapeia os segmentos de URL para os métodos de ação por nome e se você renomear um método, o roteamento normalmente não conseguirá encontrar esse método. A solução é o que você vê no exemplo, que é adicionar o atributo `ActionName("Delete")` ao método `DeleteConfirmed`. Isso executa efetivamente o mapeamento para o sistema de roteamento para que uma URL que inclua */Delete/* para ver uma POSTAGEM solicitação encontrará o `DeleteConfirmed` método.

Outra maneira comum de evitar um problema com métodos que têm assinaturas e nomes idênticos é alterar artificialmente a assinatura do método POST para incluir um parâmetro não utilizado. Por exemplo, alguns desenvolvedores adicionar um tipo de parâmetro `FormCollection` que é passado para o método POST e, em seguida, simplesmente não usar o parâmetro:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Resumo

Agora você tem um aplicativo ASP.NET MVC completo que armazena dados em um banco de dados do banco de dados local. Você pode criar, ler, atualizar, excluir e procurar filmes.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Próximas etapas

Depois de ter criado e testado um aplicativo web, a próxima etapa é disponibilizá-lo para que outras pessoas usem a Internet. Para fazer isso, você precisará implantá-lo em um provedor de hospedagem na web. A Microsoft oferece a hospedagem de web gratuita para até 10 sites em uma [conta gratuita do Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Sugiro que você siga o meu tutorial em seguida [implantar um aplicativo MVC do ASP.NET seguro com associação, OAuth e banco de dados SQL no Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Um tutorial excelente é o nível intermediário de Tom Dykstra [criando um modelo de dados do Entity Framework para um aplicativo ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [StackOverflow](http://stackoverflow.com/help) e o [fóruns do ASP.NET MVC](https://forums.asp.net/1146.aspx) são uma ótima coloca para fazer perguntas. Siga [me](https://twitter.com/RickAndMSFT) no twitter, para que você possa obter atualizações em meu tutoriais mais recentes.

Comentários são bem-vindos.

— [Rick Anderson](https://blogs.msdn.com/rickAndy) do twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
— [Scott Hanselman](http://www.hanselman.com/blog/) do twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Anterior](adding-validation.md)
