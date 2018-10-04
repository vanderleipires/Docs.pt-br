---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Aprimorando os métodos Details e Delete (c#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 255374eb21568d05569f8af6727ad4b558acfc2f
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576432"
---
<a name="improving-the-details-and-delete-methods-c"></a>Aprimorando os métodos Details e Delete (c#)
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples a seguir e apresenta mais recursos.
> 
> 
> Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução de ferramentas de suporte +)
> 
> Se você estiver usando o Visual Studio 2010, em vez do Visual Web Developer 2010, instale os pré-requisitos, clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte c# está disponível para acompanhar este tópico. [Baixe a versão c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.


Nesta parte do tutorial, você vai fazer algumas melhorias gerados automaticamente `Details` e `Delete` métodos. Essas alterações não são necessárias, mas com apenas alguns pequenos pedaços de código, você pode facilmente aprimorar o aplicativo.

## <a name="improving-the-details-and-delete-methods"></a>Aprimorando os métodos Details e Delete

Quando você geradas por scaffolding o `Movie` controlador, o ASP.NET MVC gerou o código que funcionava bem, mas que pode se tornar mais robusto com apenas algumas pequenas alterações.

Abra o `Movie` controlador e modificar os `Details` método por meio do retorno `HttpNotFound` quando um filme não for encontrado. Você também deve modificar o `Details` método para definir um valor padrão para a ID que é passada para ele. (Você fez alterações semelhantes para o `Edit` método no [parte 6](examining-the-edit-methods-and-edit-view.md) deste tutorial.) No entanto, você deve alterar o tipo de retorno a `Details` método de `ViewResult` para `ActionResult`, porque o `HttpNotFound` método não retorna um `ViewResult` objeto. O exemplo a seguir mostra o modificado `Details` método.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Código primeiro facilita a pesquisa de dados usando o `Find` método. Um recurso de segurança importante que criamos para o método é que o código verifica se o `Find` método encontrou um filme antes do código tenta fazer algo com ele. Por exemplo, um hacker pode introduzir erros no site alterando a URL criada pelos links de `http://localhost:xxxx/Movies/Details/1` para algo como `http://localhost:xxxx/Movies/Details/12345` (ou algum outro valor que não representa um filme real). Se você não a verificação de um filme nulo, isso pode resultar em um erro de banco de dados.

Da mesma forma, altere o `Delete` e `DeleteConfirmed` métodos para especificar um valor padrão para o parâmetro de ID e retornar `HttpNotFound` quando um filme não for encontrado. Atualizada `Delete` métodos de `Movie` controlador são mostrados abaixo.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Observe que o `Delete` método não exclui os dados. A execução de uma operação de exclusão em resposta a uma solicitação GET (ou, de fato, a execução de uma operação de edição, criação ou qualquer outra operação que altera dados) abre uma falha de segurança. Para obter mais informações sobre isso, consulte a entrada de blog de Stephen Walther [ASP.NET MVC dica n º 46 — não use Excluir Links porque eles criaram buracos na segurança](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

O método `HttpPost` que exclui os dados é chamado `DeleteConfirmed` para fornecer ao método HTTP POST um nome ou uma assinatura exclusiva. As duas assinaturas de método são mostradas abaixo:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

O common language runtime (CLR) requer métodos sobrecarregados tenham uma assinatura exclusiva (mesmo nome, uma lista diferente de parâmetros). No entanto, aqui você precisa de dois métodos de exclusão – um para GET - e outro para POST que ambos exigem a mesma assinatura. (Ambos precisam aceitar um único inteiro como parâmetro.)

Para classificá-la, você pode fazer algumas coisas. Uma é fornecer aos métodos nomes diferentes. Esse é o que fizemos em que ele precede o exemplo. No entanto, isso apresenta um pequeno problema: o ASP.NET mapeia os segmentos de URL para os métodos de ação por nome e se você renomear um método, o roteamento normalmente não conseguirá encontrar esse método. A solução é o que você vê no exemplo, que é adicionar o atributo `ActionName("Delete")` ao método `DeleteConfirmed`. Isso executa efetivamente o mapeamento para o sistema de roteamento para que uma URL que inclua <em>/Delete/</em>para ver uma POSTAGEM solicitação encontrará o `DeleteConfirmed` método.

Outra maneira de evitar um problema com métodos que têm assinaturas e nomes idênticos é alterar artificialmente a assinatura do método POST para incluir um parâmetro não utilizado. Por exemplo, alguns desenvolvedores adicionar um tipo de parâmetro `FormCollection` que é passado para o método POST e, em seguida, simplesmente não usar o parâmetro:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Resumindo

Agora você tem um aplicativo ASP.NET MVC completo que armazena dados em um banco de dados do SQL Server Compact. Você pode criar, ler, atualizar, excluir e procurar filmes.

![](improving-the-details-and-delete-methods/_static/image1.png)

Este tutorial básico te a você a começar a transformar controladores, associá-los de modos de exibição e a transferência de dados embutidos. Em seguida, você criou e criou um modelo de dados. Entity Framework Code First criou um banco de dados do modelo de dados em tempo real, e o sistema de scaffolding do ASP.NET MVC gerado automaticamente os métodos de ação e os modos de exibição para operações CRUD básicas. Você, em seguida, adicionado a um formulário de pesquisa que permitem aos usuários a pesquisar o banco de dados. Você alterou o banco de dados para incluir uma nova coluna de dados e, em seguida, duas páginas atualizadas para criar e exibir esses novos dados. Você adicionou a validação, marcando o modelo de dados com atributos do `DataAnnotations` namespace. A validação resultante é executado no cliente e no servidor.

Se você deseja implantar seu aplicativo, é útil primeiro teste o aplicativo em seu servidor local do IIS 7. Você pode usá-lo [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) link para habilitar a configuração do IIS para aplicativos ASP.NET. Consulte os seguintes links de implantação:

- [Mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Permitir que o IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Implantação de projetos de aplicativos Web](https://msdn.microsoft.com/library/dd394698.aspx)

Agora recomendo que você para passar para o nosso nível intermediário [criando um modelo de dados do Entity Framework para um aplicativo ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) e [Store de música do MVC](../../mvc-music-store/mvc-music-store-part-1.md) tutoriais para explorar o [ASP.NET artigos no MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)e fazer check-out dos vídeos e a recursos em muitos [ https://asp.net/mvc ](https://asp.net/mvc) para saber mais sobre o ASP.NET MVC! O [fóruns do ASP.NET MVC](https://forums.asp.net/1146.aspx) são um ótimo lugar para fazer perguntas.

Aproveite!

— Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) e [ @shanselman ](http://twitter.com/shanselman) no Twitter) e Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Anterior](adding-validation-to-the-model.md)
