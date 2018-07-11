---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Criando uma cadeia de Conexão e trabalhando com LocalDB do SQL Server | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: 8c8db3fc121b2ff263ab033404211e167e19cd71
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38168370"
---
<a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Criando uma cadeia de Conexão e trabalhando com LocalDB do SQL Server
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Criando uma cadeia de Conexão e trabalhando com LocalDB do SQL Server

O `MovieDBContext` classe criada por você lida com a tarefa de conectar-se ao banco de dados e mapeamento `Movie` objetos para os registros de banco de dados. Uma pergunta que você pode fazer, no entanto, é como especificar qual banco de dados que ele se conectará. Você realmente não precisa especificar qual banco de dados usar, Entity Framework usará como padrão [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). Nesta seção vamos explicitamente adicionar uma cadeia de caracteres de conexão na *Web. config* arquivo do aplicativo.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) é uma versão leve do SQL Server Express Database Engine que é iniciado sob demanda e executado no modo de usuário. LocalDB é executado em um modo de execução especial do SQL Server Express que permite que você trabalhe com bancos de dados como *mdf* arquivos. Normalmente, os arquivos de banco de dados LocalDB são mantidos na *App\_dados* pasta de um projeto web.

SQL Server Express não é recomendado para uso em aplicativos da web de produção. LocalDB em particular não deve ser usado para produção com um aplicativo web porque ela não foi projetada para trabalhar com o IIS. No entanto, um banco de dados LocalDB pode ser migrado facilmente para SQL Server ou SQL Azure.

No Visual Studio 2017, o LocalDB é instalado por padrão com o Visual Studio.

Por padrão, o Entity Framework procura uma cadeia de caracteres de conexão que o mesmo nomeada que a classe de contexto de objeto (`MovieDBContext` para este projeto). Para obter mais informações, consulte [cadeias de Conexão do SQL Server para aplicativos Web ASP.NET](https://msdn.microsoft.com/library/jj653752.aspx).

Abra a raiz do aplicativo *Web. config* arquivo mostrado abaixo. (Não o *Web. config* arquivo na *modos de exibição* pasta.)

![](creating-a-connection-string/_static/image1.png)

Encontre o `<connectionStrings>` elemento:

![](creating-a-connection-string/_static/image2.png)

Adicione a seguinte cadeia de conexão para o `<connectionStrings>` elemento na *Web. config* arquivo.

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

O exemplo a seguir mostra uma parte do *Web. config* arquivo com a cadeia de caracteres de conexão nova adicionada:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

As cadeias de caracteres de duas conexão são muito semelhantes. A primeira cadeia de caracteres de conexão é denominada `DefaultConnection` e é usado para o banco de dados de associação para controlar quem pode acessar o aplicativo. A cadeia de caracteres de conexão que você adicionou Especifica um banco de dados LocalDB denominado *Movie.mdf* localizado na *App\_dados* pasta. Usamos não o banco de dados de associação neste tutorial, para obter mais informações sobre associação, autenticação e segurança, consulte o tutorial [criar um aplicativo ASP.NET MVC com autenticação e o banco de dados SQL e implantar no serviço de aplicativo do Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

O nome da cadeia de caracteres de conexão deve corresponder ao nome da [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe.

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

Na verdade, não precisa adicionar o `MovieDBContext` cadeia de caracteres de conexão. Se você não especificar uma cadeia de caracteres de conexão, o Entity Framework criará um banco de dados LocalDB no diretório de usuários com o nome totalmente qualificado do [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) classe (nesse caso, `MvcMovie.Models.MovieDBContext`). Você pode nomear o banco de dados que desejar, desde que ela tenha o *. Arquivos MDF* sufixo. Por exemplo, podemos pode nomear o banco de dados *MyFilms.mdf*.

Em seguida, você criará um novo `MoviesController` classe que você pode usar para exibir os dados de filme e permitir que usuários criem novas listagens de filme.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Próximo](accessing-your-models-data-from-a-controller.md)
