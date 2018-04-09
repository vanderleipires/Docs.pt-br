---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Criando uma cadeia de Conexão e trabalhar com LocalDB do SQL Server | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: edbd46ef8a03670f0cb7527142babe9bd5846c7a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Criando uma cadeia de Conexão e trabalhar com LocalDB do SQL Server
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Criando uma cadeia de Conexão e trabalhar com LocalDB do SQL Server

O `MovieDBContext` classe criada lida com a tarefa de conectar-se ao banco de dados e o mapeamento `Movie` objetos registros do banco de dados. Uma pergunta, que você pode perguntar, porém, é como especificar o banco de dados que ele se conectará. Na verdade, não é necessário especificar qual banco de dados para usar, Entity Framework usará como padrão [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). Nesta seção adicionaremos explicitamente uma cadeia de caracteres de conexão no *Web. config* arquivo do aplicativo.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) é uma versão leve do SQL Server Express Database Engine que é iniciado sob demanda e executado no modo de usuário. LocalDB é executado em um modo especial de execução do SQL Server Express que permite que você trabalhe com bancos de dados como *. mdf* arquivos. Normalmente, os arquivos de banco de dados LocalDB são mantidos no *aplicativo\_dados* pasta de um projeto da web.

SQL Server Express não é recomendado para uso em aplicativos da web de produção. LocalDB em particular não deve ser usado para produção com um aplicativo web porque ele não foi projetado para trabalhar com o IIS. No entanto, um banco de dados LocalDB pode ser migrado facilmente para SQL Server ou SQL Azure.

No Visual Studio de 2017, o LocalDB é instalado por padrão com o Visual Studio.

Por padrão, o Entity Framework procura uma cadeia de caracteres de conexão que o mesmo nomeada que a classe de contexto de objeto (`MovieDBContext` para este projeto). Para obter mais informações, consulte [cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Abra a raiz do aplicativo *Web. config* arquivo mostrado abaixo. (Não o *Web. config* arquivo o *exibições* pasta.)

![](creating-a-connection-string/_static/image1.png)

Localizar o `<connectionStrings>` elemento:

![](creating-a-connection-string/_static/image2.png)

Adicione a seguinte cadeia de conexão para o `<connectionStrings>` elemento o *Web. config* arquivo.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

O exemplo a seguir mostra uma parte do *Web. config* arquivo com a cadeia de caracteres de conexão nova adicionada:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

As cadeias de caracteres de duas conexão são muito semelhantes. A primeira cadeia de caracteres de conexão é denominada `DefaultConnection` e é usado para o banco de dados de associação para controlar quem pode acessar o aplicativo. A cadeia de caracteres de conexão que você adicionou Especifica um banco de dados LocalDB denominado *Movie.mdf* localizado no *aplicativo\_dados* pasta. Nós não usar o banco de dados de associação neste tutorial, para obter mais informações sobre associação, autenticação e segurança, consulte o tutorial [criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar o serviço de aplicativo do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

O nome da cadeia de caracteres de conexão deve corresponder ao nome do [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Na verdade, não é necessário adicionar o `MovieDBContext` cadeia de caracteres de conexão. Se você não especificar uma cadeia de caracteres de conexão, o Entity Framework criará um banco de dados LocalDB no diretório de usuários com o nome totalmente qualificado do [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe (neste caso `MvcMovie.Models.MovieDBContext`). Você pode nomear o banco de dados que desejar, desde que ele tenha o *. MDF* sufixo. Por exemplo, podemos pode nomear o banco de dados *MyFilms.mdf*.

Em seguida, você criará um novo `MoviesController` classe que você pode usar para exibir os dados do filme e permitir que os usuários criem novas listagens de filme.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)
