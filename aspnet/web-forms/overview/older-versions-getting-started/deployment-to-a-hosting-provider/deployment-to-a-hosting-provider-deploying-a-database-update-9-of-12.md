---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização do banco de dados - 9 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 0a00f9d3ed284ebbc1d83c1b5696436e5ba00f4b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30884880"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização do banco de dados - 9 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010. Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão Geral

Neste tutorial, você fazer uma alteração de banco de dados e alterações de código relacionadas, teste as alterações no Visual Studio, em seguida, implante a atualização para ambientes de teste e de produção.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Adicionar uma nova coluna a uma tabela

Nesta seção, você adiciona uma coluna de data de nascimento para o `Person` a classe base para o `Student` e `Instructor` entidades. Em seguida, você pode atualizar a página que exibe dados de instrutor para que ele exibe a nova coluna.

No *ContosoUniversity.DAL* projeto, abra *Person.cs* e adicione a propriedade a seguir no final o `Person` classe (deve haver duas chaves após ele):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Em seguida, atualize o método de propagação para que ele forneça um valor para a nova coluna. Abra *Migrations\Configuration.cs* e substitua o bloco de código que começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui informações de data de nascimento:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

No projeto ContosoUniversity, abra *Instructors.aspx* e adicionar um novo campo de modelo para exibir a data de nascimento. Adicione-o entre os para atribuição de contratação data e do office:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Se o recuo de código obtém fora de sincronia, você pode pressionar CTRL-K e CTRL-D para reformatar automaticamente o arquivo.)

Compile a solução e, em seguida, abra o **Package Manager Console** janela. Certifique-se de que ContosoUniversity.DAL ainda está selecionado como o **projeto padrão**.

No **Package Manager Console** janela, selecione **ContosoUniversity.DAL** como o **projeto padrão**e, em seguida, digite o seguinte comando:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define o novo `DbMIgration` classe e no `Up` método, você pode ver o código que cria a nova coluna.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Compile a solução e, em seguida, digite o seguinte comando no **Package Manager Console** janela (Verifique se o projeto ContosoUniversity.DAL ainda está selecionado):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Quando o comando for concluído, execute o aplicativo e selecione a página de professores. Quando a página for carregada, você verá que ele tem o novo campo de data de nascimento.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implantação da atualização do banco de dados para o ambiente de teste

Em **Solution Explorer** selecione o projeto ContosoUniversity.

No **Web um clique em publicar** barra de ferramentas, selecione o **teste** perfil de publicação e, em seguida, clique em **Publicar Web**. (Se a barra de ferramentas estiver desabilitada, selecione o projeto de ContosoUniversity na **Solution Explorer**.)

O Visual Studio implanta o aplicativo atualizado e o navegador é aberto para a home page. Execute a página instrutores para verificar se a atualização foi implantada com êxito. Quando o aplicativo tentar acessar o banco de dados para essa página, Code First para atualizar o esquema de banco de dados e executa o `Seed` método. Quando a página é exibida, você verá o esperado **data de nascimento** coluna com datas nele.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implantação da atualização do banco de dados para o ambiente de produção

Agora, você pode implantar na produção. A única diferença é que você usará *aplicativo\_offline.htm* para impedir que os usuários acessam o site e, portanto, atualizar o banco de dados enquanto você estiver implantando as alterações. Para implantação de produção, execute as seguintes etapas:

- Carregar o *aplicativo\_offline.htm* arquivo para o site de produção.
- No Visual Studio, escolha o perfil de produção no **Web um clique em publicar** barra de ferramentas e clique em **Publicar Web**.
- Excluir o *aplicativo\_offline.htm* arquivo do site de produção.

> [!NOTE]
> Enquanto seu aplicativo estiver em uso no ambiente de produção deve implementar um plano de backup. Ou seja, você deve ser periodicamente copiando o *escola-Prod.sdf* e *Prod.sdf aspnet* arquivos da produção do site para um local de armazenamento seguro e você deve manter várias gerações de tais backups. Quando você atualizar o banco de dados, você deve fazer uma cópia de backup de imediatamente antes da alteração. Em seguida, se você comete um erro e não Descubra até depois de implantá-lo em produção, você ainda poderá recuperar o banco de dados para o estado em que estava antes que ele se tornou corrompido.


Quando o Visual Studio abrirá o URL da home page no navegador, o *aplicativo\_offline.htm* página é exibida. Depois de excluir o *aplicativo\_offline.htm* arquivo, você pode navegar para a home page novamente para verificar se a atualização foi implantada com êxito.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Agora que você implantou uma atualização de aplicativo que incluía uma alteração de banco de dados para teste e produção. O seguinte tutorial mostra como migrar seu banco de dados do SQL Server Compact para o SQL Server Express e o SQL Server.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
