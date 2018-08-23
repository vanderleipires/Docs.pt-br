---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Adicionando um modelo | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. Ele é mais seguro e muito mais simples a seguir e demonstração...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a7e732da3b056980c87660f6b80366438b5823c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825008"
---
<a name="adding-a-model"></a>Adicionando um modelo
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples a seguir e apresenta mais recursos.


Nesta seção, você adicionará algumas classes para o gerenciamento de filmes em um banco de dados. Essas classes serão a &quot;modelo&quot; faz parte do aplicativo ASP.NET MVC.

Você usará uma tecnologia de acesso a dados do .NET Framework conhecida como o [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) para definir e trabalhar com essas classes de modelo. A Entity Framework (também conhecido como EF) oferece suporte a um paradigma de desenvolvimento chamado *Code First*. Código primeiro permite que você crie objetos de modelo ao escrever classes simples. (Esses também são conhecidas como classes POCO, do &quot;objetos CLR de BOM e velho.&quot;) Em seguida, você pode ter o banco de dados criado em tempo real de suas classes, que permite que um fluxo de trabalho de desenvolvimento muito claro e rápido.

## <a name="adding-model-classes"></a>Adicionando Classes de modelo

Na **Gerenciador de soluções**, clique com botão direito do *modelos* pasta, selecione **Add**e, em seguida, selecione **classe**.

![](adding-a-model/_static/image1.png)

Insira o *classe* nome &quot;filme&quot;.

Adicione as seguintes propriedades de cinco para o `Movie` classe:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Vamos usar o `Movie` classe para representar os filmes em um banco de dados. Cada instância de um `Movie` objeto corresponderá a uma linha em uma tabela de banco de dados e cada propriedade do `Movie` classe será mapeado para uma coluna na tabela.

No mesmo arquivo, adicione o seguinte `MovieDBContext` classe:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

O `MovieDBContext` classe representa o contexto de banco de dados de filme do Entity Framework, que lida com a busca, armazenar e atualizar `Movie` instâncias em um banco de dados de classe. O `MovieDBContext` deriva o `DbContext` fornecido pela estrutura de entidades de classe base.

Para poder referenciar `DbContext` e `DbSet`, você precisará adicionar o seguinte `using` instrução na parte superior do arquivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

A conclusão *Movie.cs* arquivo é mostrado abaixo. (Várias instruções que não são necessários foram removidos.)

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Criando uma cadeia de Conexão e trabalhando com LocalDB do SQL Server

O `MovieDBContext` classe criada por você lida com a tarefa de conectar-se ao banco de dados e mapeamento `Movie` objetos para os registros de banco de dados. Uma pergunta que você pode fazer, no entanto, é como especificar qual banco de dados que ele se conectará. Você terá de fazer isso adicionando as informações de conexão na *Web. config* arquivo do aplicativo.

Abra a raiz do aplicativo *Web. config* arquivo. (Não o *Web. config* arquivo na *modos de exibição* pasta.) Abra o *Web. config* arquivo descrito em vermelho.

![](adding-a-model/_static/image2.png)

Adicione a seguinte cadeia de conexão para o `<connectionStrings>` elemento na *Web. config* arquivo.

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

O exemplo a seguir mostra uma parte do *Web. config* arquivo com a cadeia de caracteres de conexão nova adicionada:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Essa pequena quantidade de código e o XML é tudo o que precisa ser escrita para representar e armazenar os dados de filmes em um banco de dados.

Em seguida, você criará um novo `MoviesController` classe que você pode usar para exibir os dados de filme e permitir que usuários criem novas listagens de filme.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)
