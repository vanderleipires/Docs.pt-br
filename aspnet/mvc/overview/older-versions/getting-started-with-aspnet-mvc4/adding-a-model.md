---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Adicionando um modelo | Microsoft Docs
author: Rick-Anderson
description: "Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples de seguir e demonstração..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 1d066e4bab866a2195647f43aa886279fee941db
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="adding-a-model"></a>Adicionando um modelo
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É muito mais simples a seguir, mais segura e demonstra mais recursos.


Nesta seção, você adicionará algumas classes de gerenciamento de filmes em um banco de dados. Essas classes será o &quot;modelo&quot; parte do aplicativo ASP.NET MVC.

Você usará uma tecnologia de acesso a dados do .NET Framework conhecida como o [do Entity Framework](https://msdn.microsoft.com/en-us/library/bb399572(VS.110).aspx) para definir e trabalhar com essas classes de modelo. O Entity Framework (também conhecido como EF) oferece suporte a um paradigma de desenvolvimento chamado *Code First*. Código primeiro permite que você crie objetos de modelo, escrevendo classes simples. (Eles também são conhecidas como POCO classes, de &quot;objetos CLR simples antigo.&quot;) Em seguida, você pode ter o banco de dados criado dinamicamente de suas classes, que permite que um fluxo de trabalho de desenvolvimento muito claro e rápida.

## <a name="adding-model-classes"></a>Adicionando Classes de modelo

Em **Solution Explorer**, clique com botão direito do *modelos* pasta, selecione **adicionar**e, em seguida, selecione **classe**.

![](adding-a-model/_static/image1.png)

Insira o *classe* nome &quot;filme&quot;.

Adicione as seguintes cinco propriedades para o `Movie` classe:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Usaremos a `Movie` classe para representar filmes em um banco de dados. Cada instância de um `Movie` correspondam a uma linha em uma tabela de banco de dados e cada propriedade do objeto de `Movie` classe será mapeado para uma coluna na tabela.

No mesmo arquivo, adicione o seguinte `MovieDBContext` classe:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

O `MovieDBContext` classe representa o contexto de banco de dados do filme do Entity Framework, que manipula a busca, armazenar e atualizar `Movie` instâncias em um banco de dados de classe. O `MovieDBContext` deriva o `DbContext` fornecido pelo Entity Framework de classe base.

Para poder referenciar `DbContext` e `DbSet`, você precisa adicionar o seguinte `using` instrução na parte superior do arquivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Completo *Movie.cs* arquivo é mostrado abaixo. (Várias instruções using que não são necessários foram removido.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Criando uma cadeia de Conexão e trabalhar com LocalDB do SQL Server

O `MovieDBContext` classe criada lida com a tarefa de conectar-se ao banco de dados e o mapeamento `Movie` objetos registros do banco de dados. Uma pergunta, que você pode perguntar, porém, é como especificar o banco de dados que ele se conectará. Você fará isso adicionando informações de conexão no *Web. config* arquivo do aplicativo.

Abra a raiz do aplicativo *Web. config* arquivo. (Não o *Web. config* arquivo o *exibições* pasta.) Abra o *Web. config* arquivo realçado em vermelho.

![](adding-a-model/_static/image2.png)

Adicione a seguinte cadeia de conexão para o `<connectionStrings>` elemento o *Web. config* arquivo.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

O exemplo a seguir mostra uma parte do *Web. config* arquivo com a cadeia de caracteres de conexão nova adicionada:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Essa quantidade pequena de código e o XML é tudo o que você precisa gravar para representar e armazenar os dados do filme em um banco de dados.

Em seguida, você criará um novo `MoviesController` classe que você pode usar para exibir os dados do filme e permitir que os usuários criem novas listagens de filme.

>[!div class="step-by-step"]
[Anterior](adding-a-view.md)
[Próximo](accessing-your-models-data-from-a-controller.md)
