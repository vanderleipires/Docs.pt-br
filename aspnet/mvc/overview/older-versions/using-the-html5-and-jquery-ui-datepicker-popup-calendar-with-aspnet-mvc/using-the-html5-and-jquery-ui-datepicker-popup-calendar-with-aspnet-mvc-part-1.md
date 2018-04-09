---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Usando HTML5 e jQuery UI Datepicker pop-up de calendário com ASP.NET MVC - parte 1 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de como trabalhar com modelos de editor, modelos de exibição e o calendário de pop-up jQuery UI datepicker em uma MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: 408b99c9ad4fbc8487e585ebed3183f9aedc9c10
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Usando HTML5 e jQuery UI Datepicker pop-up de calendário com ASP.NET MVC - parte 1
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial irá ensiná-lo os fundamentos de como trabalhar com modelos de editor, modelos de exibição e calendário pop-up jQuery UI datepicker em um aplicativo ASP.NET MVC.


Este tutorial ensina os fundamentos de como trabalhar com modelos de editor, modelos de exibição e o jQuery [calendário de pop-up UI datepicker](http://plugins.jquery.com/project/datepicker) em um aplicativo ASP.NET MVC. Para este tutorial, você pode usar o Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), que é uma versão gratuita do Microsoft Visual Studio, ou você pode usar o Visual Studio 2010 SP1 se ele já está instalado.

Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente o software necessário, usando os links a seguir:

- [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)

Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Este tutorial presume que você tenha concluído o [guia de Introdução do MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial ou que você está familiarizado com o desenvolvimento do ASP.NET MVC. Este tutorial começa com o projeto concluído no [guia de Introdução do MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial.

Este tutorial mostra o código em c#. No entanto, o [projeto inicial](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) e projeto concluído também estão disponíveis no Visual Basic.

Um projeto do Visual Studio com o código-fonte c# e Visual Basic está disponível para acompanhar este tópico: [baixar](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>O que você vai criar

Você adicionará modelos (especificamente, editar e exibir modelos) para o aplicativo de filme listagem simple que foi criado no [guia de Introdução do MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) tutorial. Você também adicionará uma [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendário pop-up para simplificar o processo de inserção de datas. Captura de tela a seguir mostra o aplicativo modificado com o calendário de pop-up jQuery UI datepicker exibido.

![jQuery concluído](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Você vai aprender as habilidades

Aqui está o que você aprenderá:

- Como usar os atributos de [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace para controlar o formato de dados quando ele for exibido e quando ela estiver no modo de edição.
- Como criar modelos (Editar e exibir modelos) para controlar a formatação de dados.
- Como adicionar o [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) como uma maneira de inserir os campos de data.

### <a name="getting-started"></a>Guia de Introdução

Se você ainda não tiver o aplicativo de filme listagem do projeto de starter, baixá-lo usando o seguinte link: [baixar](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800). No Windows Explorer, clique com o *MvcMovie.zip* de arquivo e selecione **propriedades**. No **MvcMovie.zip propriedades** caixa de diálogo, selecione **desbloquear**. (Desbloqueio impede que um aviso de segurança que ocorre quando você tentar usar um *. zip* arquivo que você baixou da web.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Com o botão direito do *MvcMovie.zip* de arquivo e selecione **extrair tudo** para descompactar o arquivo. No Visual Web Developer ou Visual Studio 2010, abra o *MvcMovieCS\_TU.sln* arquivo.

Em **Solution Explorer**, clique duas vezes o *exibições \ compartilhadas\\cshtml* para abri-lo. Alterar o `H1` cabeçalho de **aplicativo de filme MVC** para **filme jQuery**. Pressione CTRL + F5 para executar o aplicativo e clique no **início** guia, que leva para o `Index` método do controlador do filme. Para testar o aplicativo, selecione o **editar** link e o **detalhes** link para um dos filmes. Observe que nas exibições de índice, editar e detalhes de preço e data de lançamento são bem formatados:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

A formatação da data e o preço é o resultado do uso de [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo nas propriedades do `Movie` classe.

Abra o *Movie.cs* de arquivo e comente a `DisplayFormat` atributo no `ReleaseDate` e `Price` propriedades. Resultante `Movie` classe tem esta aparência:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Pressione CTRL + F5 novamente para executar o aplicativo e selecione o **início** guia para exibir a lista de filmes. Neste momento, a data de lançamento mostra a data e hora, e o campo de preço não mostra o símbolo de moeda. A alteração no `Movie` classe desfez a formatação adequado que vimos anteriormente, mas isso será corrigido em breve.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Usando o atributo de tipo de dados DataAnnotations para especificar o tipo de dados

Substitua o comentado `DisplayFormat` de atributo para o `ReleaseDate` propriedade com o [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) de atributo, usando o `Date` enumeração. Substitua o `DisplayFormat` de atributo para o `Price` propriedade com o [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo novamente, dessa vez usando o `Currency` enumeração. Isso é o código completo semelhante ao seguinte:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Execute o aplicativo. Agora a data de lançamento e as propriedades de preços estão formatadas corretamente (ou seja, usando os formatos de data e moeda apropriados). O [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo fornece metadados de tipo para o ASP.NET MVC internos modelos para que os campos de renderização no formato correto. Usando o `DataType` atributo é preferível a usar o `DisplayFormat` atributo que foi originalmente no código, pois o `DataType` atributo faz o modelo mais flexível para finalidades como internacionalização e limpeza.

Na próxima seção, você verá como criar modelos personalizados para exibir os campos de data.

> [!div class="step-by-step"]
> [Avançar](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
