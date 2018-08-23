---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização de banco de dados - 9 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: b15d27a07207110187b897624814125c9e030493
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825198"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização de banco de dados - 9 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se você instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server que não seja o SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Neste tutorial, você fazer uma alteração de banco de dados e alterações de código relacionados, testar as alterações no Visual Studio e, em seguida, implante a atualização para ambientes de produção e de teste.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Adicionando uma nova coluna a uma tabela

Nesta seção, você adiciona uma coluna de data de nascimento para o `Person` a classe base para o `Student` e `Instructor` entidades. Em seguida, você pode atualizar a página que exibe dados de instrutor para que ele exibe a nova coluna.

No *ContosoUniversity.DAL* projeto, abra *Person.cs* e adicione a propriedade a seguir no final o `Person` classe (deve haver duas chaves após ele):

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

Em seguida, atualize o método de propagação para que ele forneça um valor para a nova coluna. Abra *Migrations\Configuration.cs* e substitua o bloco de código começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui informações de data de nascimento:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

No projeto ContosoUniversity, abra *Instructors.aspx* e adicione um novo campo de modelo para exibir a data de nascimento. Adicioná-la entre aqueles para a atribuição de contratação data e do office:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

(Se o recuo do código fica fora de sincronizado, você pode pressionar CTRL-K e CTRL-D para reformatar automaticamente o arquivo.)

Compile a solução e, em seguida, abra o **Package Manager Console** janela. Certifique-se de que ContosoUniversity.DAL ainda está selecionado como o **projeto padrão**.

No **Package Manager Console** janela, selecione **ContosoUniversity.DAL** como o **projeto padrão**e, em seguida, digite o seguinte comando:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

Quando esse comando for concluído, o Visual Studio abre o arquivo de classe que define o novo `DbMIgration` classe e, no `Up` método, você pode ver o código que cria a nova coluna.

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

Compile a solução e, em seguida, digite o seguinte comando na **Package Manager Console** janela (Verifique se o projeto ContosoUniversity.DAL ainda está selecionado):

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

Quando o comando for concluído, execute o aplicativo e selecione a página instrutores. Quando a página for carregada, você verá que ele tem o novo campo de data de nascimento.

[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implantar a atualização de banco de dados para o ambiente de teste

Na **Gerenciador de soluções** selecione o projeto ContosoUniversity.

No **publicação Web com um clique em** barra de ferramentas, selecione a **teste** perfil de publicação e, em seguida, clique em **publicar na Web**. (Se a barra de ferramentas estiver desabilitada, selecione o projeto de ContosoUniversity no **Gerenciador de soluções**.)

O Visual Studio implanta o aplicativo atualizado e o navegador é aberto para a home page. Execute a página instrutores para verificar se a atualização foi implantada com êxito. Quando o aplicativo tenta acessar o banco de dados para essa página, Code First para atualizar o esquema de banco de dados e executa o `Seed` método. Quando a página for exibida, você verá o esperado **data de nascimento** coluna com datas nele.

[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implantar a atualização de banco de dados para o ambiente de produção

Agora, você pode implantar para produção. A única diferença é que você usará *app\_offline.htm* para impedir que os usuários acessam o site e, portanto, atualizando o banco de dados enquanto você estiver implantando as alterações. Para implantação de produção, execute as seguintes etapas:

- Carregar o *app\_offline.htm* arquivo ao site de produção.
- No Visual Studio, escolha o perfil de produção na **publicação Web com um clique em** barra de ferramentas e clique em **publicar na Web**.
- Excluir o *app\_offline.htm* arquivo do site de produção.

> [!NOTE]
> Enquanto seu aplicativo está em uso no ambiente de produção deve implementar um plano de backup. Ou seja, você deve ser periodicamente copiando o *School-Prod.sdf* e *Prod.sdf aspnet* arquivos da produção de site para um local de armazenamento seguro e você deve manter várias gerações de tais backups. Quando você atualiza o banco de dados, você deve fazer uma cópia de backup de imediatamente antes da alteração. Em seguida, se você comete um erro e não Descubra até depois que você implantou em produção, você ainda poderá recuperar o banco de dados para o estado que estava antes de ele se tornou corrompido.


Quando o Visual Studio abre a URL da home page no navegador, o *app\_offline.htm* página é exibida. Depois de excluir o *app\_offline.htm* arquivo, você pode navegar para a home page novamente para verificar se a atualização foi implantada com êxito.

[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)

Agora, você implantou uma atualização de aplicativo que incluía uma alteração de banco de dados para teste e produção. O próximo tutorial mostra como migrar seu banco de dados do SQL Server Compact para o SQL Server Express e o SQL Server.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
