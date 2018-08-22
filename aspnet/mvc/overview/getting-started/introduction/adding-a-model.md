---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Adicionando um modelo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7326cefd52feb63fc2f602210ed560cdf28706a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832849"
---
<a name="adding-a-model"></a>Adicionando um modelo
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

Nesta seção, você adicionará algumas classes para o gerenciamento de filmes em um banco de dados. Essas classes serão a &quot;modelo&quot; faz parte do aplicativo ASP.NET MVC.

Você usará uma tecnologia de acesso a dados do .NET Framework conhecida como o [Entity Framework](https://docs.microsoft.com/ef/) para definir e trabalhar com essas classes de modelo. A Entity Framework (também conhecido como EF) oferece suporte a um paradigma de desenvolvimento chamado *Code First*. Código primeiro permite que você crie objetos de modelo ao escrever classes simples. (Esses também são conhecidas como classes POCO, do &quot;objetos CLR de BOM e velho.&quot;) Em seguida, você pode ter o banco de dados criado em tempo real de suas classes, que permite que um fluxo de trabalho de desenvolvimento muito claro e rápido. Se você precisar criar o banco de dados pela primeira vez, você ainda pode seguir este tutorial para saber mais sobre o desenvolvimento de aplicativo do MVC e ao EF. Em seguida, você pode seguir Tom Fizmakens [Scaffolding do ASP.NET](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, que abrange a primeira abordagem de banco de dados.

## <a name="adding-model-classes"></a>Adicionando Classes de modelo

Na **Gerenciador de soluções**, clique com botão direito do *modelos* pasta, selecione **Add**e, em seguida, selecione **classe**.

![](adding-a-model/_static/image1.png)

Insira o *classe* nome &quot;filme&quot;.

Adicione as seguintes propriedades de cinco para o `Movie` classe:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Vamos usar o `Movie` classe para representar os filmes em um banco de dados. Cada instância de um `Movie` objeto corresponderá a uma linha em uma tabela de banco de dados e cada propriedade do `Movie` classe será mapeado para uma coluna na tabela.

Observação: Para usar Entity e a classe relacionada, você precisa instalar o [pacote do NuGet do Entity Framework](https://www.nuget.org/packages/EntityFramework/). Siga o link para obter mais instruções.

No mesmo arquivo, adicione o seguinte `MovieDBContext` classe:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

O `MovieDBContext` classe representa o contexto de banco de dados de filme do Entity Framework, que lida com a busca, armazenar e atualizar `Movie` instâncias em um banco de dados de classe. O `MovieDBContext` deriva o `DbContext` fornecido pela estrutura de entidades de classe base.

Para poder referenciar `DbContext` e `DbSet`, você precisará adicionar o seguinte `using` instrução na parte superior do arquivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Você pode fazer isso adicionando manualmente o usando instrução, ou você pode passar o mouse sobre as linhas curvadas vermelhas, clique em `Show potential fixes` e clique em `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Observação: Várias não utilizados `using` instruções foram removidas. Visual Studio mostrará dependências não utilizadas como cinza. Você pode remover as dependências não utilizadas focalizando as dependências de cinza, clique em `Show potential fixes` e clique em **remover usos não utilizados.**

![](adding-a-model/_static/image3.png)

Por fim, adicionamos um modelo (M no MVC). Na próxima seção, você trabalhará com a cadeia de caracteres de conexão de banco de dados.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Próximo](creating-a-connection-string.md)
