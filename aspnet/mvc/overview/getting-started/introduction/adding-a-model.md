---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Adicionando um modelo | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 13aab58e86829a8d4accd1d304420dcb34ffa472
ms.sourcegitcommit: ec9371e2fbfcb8d62e7e7cae69e7752f3f205385
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/23/2017
---
<a name="adding-a-model"></a>Adicionando um modelo
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

Nesta seção, você adicionará algumas classes de gerenciamento de filmes em um banco de dados. Essas classes será o &quot;modelo&quot; parte do aplicativo ASP.NET MVC.

Você usará uma tecnologia de acesso a dados do .NET Framework conhecida como o [do Entity Framework](https://docs.microsoft.com/ef/) para definir e trabalhar com essas classes de modelo. O Entity Framework (também conhecido como EF) oferece suporte a um paradigma de desenvolvimento chamado *Code First*. Código primeiro permite que você crie objetos de modelo, escrevendo classes simples. (Eles também são conhecidas como POCO classes, de &quot;objetos CLR simples antigo.&quot;) Em seguida, você pode ter o banco de dados criado dinamicamente de suas classes, que permite que um fluxo de trabalho de desenvolvimento muito claro e rápida. Se for necessário para criar o banco de dados pela primeira vez, você ainda pode seguir este tutorial para aprender sobre o desenvolvimento de aplicativo MVC e EF. Você pode seguir Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, que abrange a primeira abordagem de banco de dados.

## <a name="adding-model-classes"></a>Adicionando Classes de modelo

Em **Solution Explorer**, clique com botão direito do *modelos* pasta, selecione **adicionar**e, em seguida, selecione **classe**.

![](adding-a-model/_static/image1.png)

Insira o *classe* nome &quot;filme&quot;.

Adicione as seguintes cinco propriedades para o `Movie` classe:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Usaremos a `Movie` classe para representar filmes em um banco de dados. Cada instância de um `Movie` correspondam a uma linha em uma tabela de banco de dados e cada propriedade do objeto de `Movie` classe será mapeado para uma coluna na tabela.

Observação: Para usar System.Data.Entity e a classe relacionada, você precisa instalar o [pacote do NuGet do Entity Framework](https://www.nuget.org/packages/EntityFramework/). Siga o link para obter mais informações.

No mesmo arquivo, adicione o seguinte `MovieDBContext` classe:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

O `MovieDBContext` classe representa o contexto de banco de dados do filme do Entity Framework, que manipula a busca, armazenar e atualizar `Movie` instâncias em um banco de dados de classe. O `MovieDBContext` deriva o `DbContext` fornecido pelo Entity Framework de classe base.

Para poder referenciar `DbContext` e `DbSet`, você precisa adicionar o seguinte `using` instrução na parte superior do arquivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Você pode fazer isso manualmente adicionando o uso instrução ou passe o mouse sobre as linhas curvadas vermelhas, clique em `Show potential fixes` e clique em`using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Observação: Vários não utilizados `using` instruções foram removidas. O Visual Studio mostrará dependências não usadas em cinza. Você pode remover dependências unnused focalizando as dependências cinzas, clique em `Show potential fixes` e clique em **remover usos não utilizados.**

![](adding-a-model/_static/image3.png)

Por fim, adicionamos um modelo (M no MVC). Na próxima seção, você trabalhará com a cadeia de caracteres de conexão do banco de dados.

>[!div class="step-by-step"]
[Anterior](adding-a-view.md)
[Próximo](creating-a-connection-string.md)
