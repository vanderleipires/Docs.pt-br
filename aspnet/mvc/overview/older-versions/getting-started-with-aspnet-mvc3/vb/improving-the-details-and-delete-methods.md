---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: Melhorando os detalhes e métodos de exclusão (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 0c662510ce9a80e0e808af0eec2561ecdaa12c01
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="improving-the-details-and-delete-methods-vb"></a>Melhorando os detalhes e métodos de exclusão (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com VB.NET código-fonte está disponível para acompanhar este tópico. [Baixe a versão VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir c#, alterne para o [versão c#](../cs/improving-the-details-and-delete-methods.md) deste tutorial.


Nesta parte do tutorial, você fará algumas melhorias gerado automaticamente `Details` e `Delete` métodos. Essas alterações não são necessárias, mas com apenas algumas pequenas unidades de código, você pode facilmente aprimorar o aplicativo.

## <a name="improving-the-details-and-delete-methods"></a>Melhorando os detalhes e métodos de exclusão

Quando você Scaffold o `Movie` controlador, o ASP.NET MVC gerado que funcionou muito bem, mas que podem ser feitas mais robusto com apenas algumas pequenas alterações de código.

Abra o `Movie` controlador e modificar o `Details` método retornando `HttpNotFound` quando um filme não foi encontrado. Você também deve modificar o `Details` método para definir um valor padrão para a ID que é passada para ele. (Você fez alterações semelhantes para o `Edit` método [parte 6](examining-the-edit-methods-and-edit-view.md) deste tutorial.) No entanto, você deve alterar o tipo de retorno de `Details` método de `ViewResult` para `ActionResult`, pois o `HttpNotFound` método não retorna um `ViewResult` objeto. O exemplo a seguir mostra o `Details` método.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

Código primeiro torna mais fácil procurar dados usando o `Find` método. Um recurso de segurança importante que criamos no método é que o código verifica se o `Find` método foi encontrado um filme antes do código tenta fazer com ele. Por exemplo, um hacker pode introduzir erros no site, alterando a URL criada por links de `http://localhost:xxxx/Movies/Details/1` para algo como `http://localhost:xxxx/Movies/Details/12345` (ou algum outro valor que não representa um filme real). Se você não tiver a seleção de um filme nulo, isso pode resultar em um erro de banco de dados.

Da mesma forma, alterar o `Delete` e `DeleteConfirmed` métodos para especificar um valor padrão para o parâmetro de ID e retornar `HttpNotFound` quando um filme não foi encontrado. A atualização `Delete` métodos o `Movie` controlador são mostrados abaixo.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

Observe que o `Delete` método não exclui os dados. A execução de uma operação de exclusão em resposta a uma solicitação GET (ou, de fato, a execução de uma operação de edição, criação ou qualquer outra operação que altera dados) abre uma falha de segurança. Para obter mais informações sobre isso, consulte a entrada de blog de Stephen Walther [ASP.NET MVC dica 46 — não use Excluir Links porque eles criar vulnerabilidades de segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

O método `HttpPost` que exclui os dados é chamado `DeleteConfirmed` para fornecer ao método HTTP POST um nome ou uma assinatura exclusiva. As duas assinaturas de método são mostradas abaixo:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

O common language runtime (CLR) requer métodos sobrecarregados para ter uma assinatura exclusiva (mesmo nome, uma lista diferente de parâmetros). No entanto, aqui você precisa de dois métodos de exclusão – um para GET - e um POST que ambas as exigem a mesma assinatura. (Ambos precisam aceitar um único inteiro como parâmetro.)

Para classificar este limite, você pode fazer algumas coisas. Uma é dar os métodos nomes diferentes. É o que fizemos em ele anterior de exemplo. No entanto, isso apresenta um pequeno problema: o ASP.NET mapeia os segmentos de URL para os métodos de ação por nome e se você renomear um método, o roteamento normalmente não conseguirá encontrar esse método. A solução é o que você vê no exemplo, que é adicionar o atributo `ActionName("Delete")` ao método `DeleteConfirmed`. Isso executa efetivamente o mapeamento para o sistema de roteamento para que uma URL que inclui <em>/Delete/</em>um POST solicitação encontrará o `DeleteConfirmed` método.

Outra maneira de evitar um problema com métodos que têm nomes idênticos e assinaturas é artificialmente alterar a assinatura do método POST para incluir um parâmetro não utilizado. Por exemplo, alguns desenvolvedores adicionar um tipo de parâmetro `FormCollection` que é passado para o método POST e, em seguida, simplesmente não usar o parâmetro:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>Resumindo

Agora você tem um aplicativo ASP.NET MVC completo que armazena dados em um banco de dados do SQL Server Compact. Você pode criar, ler, atualizar, excluir e procurar filmes.

![](improving-the-details-and-delete-methods/_static/image1.png)

Este tutorial básico temos começar a fazer controladores, associá-los a modos de exibição e a passagem de dados embutidos. Em seguida, você criou e criou um modelo de dados. Entity Framework Code First criou um banco de dados do modelo de dados em tempo real, e o ASP.NET MVC scaffolding automaticamente gerada pelo sistema os métodos de ação e modos de exibição para operações CRUD básicas. Você adicionou, em seguida, um formulário de pesquisa que permitem aos usuários a pesquisar o banco de dados. Você alterou o banco de dados para incluir uma nova coluna de dados e, em seguida, atualizado duas páginas para criar e exibir esses dados novos. Você adicionou validação marcando o modelo de dados com atributos do `DataAnnotations` namespace. A validação resultante é executado no cliente e no servidor.

Se você gostaria de implantar seu aplicativo, é útil para teste primeiro o aplicativo em seu servidor local do IIS 7. Você pode usar isso [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) link para habilitar a configuração do IIS para aplicativos ASP.NET. Consulte os seguintes links de implantação:

- [Mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Habilitar o IIS 7. x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Implantação de projetos de aplicativo da Web](https://msdn.microsoft.com/library/dd394698.aspx)

Agora recomendo que você para passar para o nível de intermediário [criando um modelo de dados do Entity Framework para um aplicativo ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) e [repositório de música MVC](../../mvc-music-store/mvc-music-store-part-1.md) tutoriais, para explorar o [ASP.NET artigos no MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)e fazer check-out dos muitos vídeos e recursos em [ https://asp.net/mvc ](https://asp.net/mvc) para saber mais sobre o ASP.NET MVC! O [fóruns do ASP.NET MVC](https://forums.asp.net/1146.aspx) são um ótimo lugar para fazer perguntas.

Aproveite!

— Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) no Twitter) e Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Anterior](adding-validation-to-the-model.md)
