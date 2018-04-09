---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Adicionando um modelo (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e9a271c64347b4004d5cc5d9d91085c4e642e95d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-model-vb"></a>Adicionando um modelo (VB)
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
> Um projeto do Visual Web Developer com VB.NET código-fonte está disponível para acompanhar este tópico. [Baixe a versão VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir c#, alterne para o [versão c#](../cs/adding-a-model.md) deste tutorial.


## <a name="adding-a-model"></a>Adicionando um modelo

Nesta seção, você adicionará algumas classes de gerenciamento de filmes em um banco de dados. Essas classes será a parte "modelo" do aplicativo ASP.NET MVC.

Você usará uma tecnologia de acesso a dados do .NET Framework conhecida como o Entity Framework para definir e trabalhar com essas classes de modelo. O Entity Framework (também conhecido como EF) oferece suporte a um paradigma de desenvolvimento chamado *Code First*. Código primeiro permite que você crie objetos de modelo, escrevendo classes simples. (Esses são também conhecidos como classes POCO, de "objetos CLR plain old".) Em seguida, você pode ter o banco de dados criado dinamicamente de suas classes, que permite que um fluxo de trabalho de desenvolvimento muito claro e rápida.

## <a name="adding-model-classes"></a>Adicionando Classes de modelo

Em **Solution Explorer**, clique com botão direito do *modelos* pasta, selecione **adicionar**e, em seguida, selecione **classe**.

![](adding-a-model/_static/image1.png)

Nomeie a classe "Filme".

Adicione as seguintes cinco propriedades para o `Movie` classe:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Usaremos a `Movie` classe para representar filmes em um banco de dados. Cada instância de um `Movie` correspondam a uma linha em uma tabela de banco de dados e cada propriedade do objeto de `Movie` classe será mapeado para uma coluna na tabela.

No mesmo arquivo, adicione o seguinte `MovieDBContext` classe:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

O `MovieDBContext` classe representa o contexto de banco de dados do filme do Entity Framework, que manipula a busca, armazenar e atualizar `Movie` instâncias em um banco de dados de classe. O `MovieDBContext` deriva o `DbContext` fornecido pelo Entity Framework de classe base. Para obter mais informações sobre `DbContext` e `DbSet`, consulte [melhorias de produtividade para Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Para poder referenciar `DbContext` e `DbSet`, você precisa adicionar o seguinte `imports` instrução na parte superior do arquivo:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

Completo *Movie.vb* arquivo é mostrado abaixo.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Criando uma cadeia de Conexão e trabalhar com o SQL Server Compact

O `MovieDBContext` classe criada lida com a tarefa de conectar-se ao banco de dados e o mapeamento `Movie` objetos registros do banco de dados. Uma pergunta, que você pode perguntar, porém, é como especificar o banco de dados que ele se conectará. Você fará isso adicionando informações de conexão no *Web. config* arquivo do aplicativo.

Abra a raiz do aplicativo *Web. config* arquivo. (Não o *Web. config* arquivo o *exibições* pasta.) A imagem a seguir mostrar ambos *Web. config* arquivos; abrir o *Web. config* arquivo dentro de um círculo em vermelho.

![](adding-a-model/_static/image2.png)

## 

Adicione a seguinte cadeia de conexão para o `<connectionStrings>` elemento o *Web. config* arquivo.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

O exemplo a seguir mostra uma parte do *Web. config* arquivo com a cadeia de caracteres de conexão nova adicionada:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Essa quantidade pequena de código e o XML é tudo o que você precisa gravar para representar e armazenar os dados do filme em um banco de dados.

Em seguida, você criará um novo `MoviesController` classe que você pode usar para exibir os dados do filme e permitir que os usuários criem novas listagens de filme.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)
