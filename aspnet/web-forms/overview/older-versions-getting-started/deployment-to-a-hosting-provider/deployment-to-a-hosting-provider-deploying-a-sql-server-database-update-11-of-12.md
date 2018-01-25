---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: "Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização de banco de dados do SQL Server - 11 12 | Microsoft Docs"
author: tdykstra
description: "Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: aeec69c7373a111d30e8f32a374a9f02fb4c080a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização de banco de dados do SQL Server - 11 12
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010. Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar o Windows Azure Web Sites, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como implantar uma atualização de banco de dados em um banco de dados completo do SQL Server. Como migrações do Code First faz todo o trabalho de atualização de banco de dados, o processo é quase idêntico ao que você fez para o SQL Server Compact no [implantar uma atualização de banco de dados](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Adicionar uma nova coluna a uma tabela

Esta seção do tutorial, você fará um banco de dados de alteração e alterações de código correspondente, testá-las no Visual Studio em preparação para implantá-los nos ambientes de teste e produção. A alteração envolve a adição de um `OfficeHours` coluna para o `Instructor` entidade e exibir as novas informações no **instrutores** página da web.

No projeto ContosoUniversity.DAL, abra *Instructor.cs* e adicione a seguinte propriedade entre o `HireDate` e `Courses` propriedades:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Atualize a classe de inicializador de modo que ele propaga a nova coluna com dados de teste. Abra *Migrations\Configuration.cs* e substitua o bloco de código que começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui a nova coluna:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

No projeto ContosoUniversity, abra *Instructors.aspx* e adicionar um novo campo de modelo para o horário de expediente antes do fechamento `</Columns>` marca na primeira `GridView` controle:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Compile a solução.

Abra o **Package Manager Console** janela e selecione ContosoUniversity.DAL como o **projeto padrão**.

Digite os seguintes comandos:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Execute o aplicativo e selecione o **instrutores** página. A página demora um pouco mais do que o normal para carregar, porque o Entity Framework recria o banco de dados e propaga-lo com dados de teste.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implantação da atualização do banco de dados para o ambiente de teste

Quando você usa as migrações do Code First, o método de implantação de uma alteração de banco de dados para o SQL Server é igual do SQL Server Compact. No entanto, você precisa alterar o teste de perfil de publicação, porque ele ainda está configurado para migrar do SQL Server Compact para o SQL Server.

A primeira etapa é remover as transformações de cadeia de caracteres de conexão que você criou no tutorial anterior. Eles não são mais necessários porque você irá especificar transformações de cadeia de caracteres de conexão no perfil de publicação, como você fez antes que você configurou o **pacote/publicar SQL** guia de migração para o SQL Server.

Abra o *Web.Test.config* de arquivo e remover o `connectionStrings` elemento. A transformação somente restante no *Web.Test.config* arquivo é para o `Environment` valor o `appSettings` elemento.

Agora você pode atualizar o perfil de publicação e publicar para o ambiente de teste.

Abra o **Publicar Web** assistente e, em seguida, alternar para o **perfil** guia.

Selecione o **teste** perfil de publicação.

Selecione o **configurações** guia.

Clique em **habilitar o banco de dados novas melhorias de publicação**.

Na caixa de cadeia de caracteres de conexão do **SchoolContext**, inserir o mesmo valor que você usou o *Web.Test.config* arquivo de transformação no tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Selecione **executar migrações do Code First (executado na inicialização do aplicativo)**. (Na sua versão do Visual Studio, a caixa de seleção deve ser rotulada **aplicar migrações do Code First**.)

Na caixa de cadeia de caracteres de conexão do **DefaultConnection**, inserir o mesmo valor que você usou o *Web.Test.config* arquivo de transformação no tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Deixe **Atualizar banco de dados** desmarcada.

Clique em **Publicar**.

Visual Studio implanta as alterações de código para o ambiente de teste e abre o navegador para a home page da Contoso University.

Selecione a página de professores.

Quando o aplicativo é executado nessa página, ele tenta acessar o banco de dados. Migrações do Code First verifica se o banco de dados atual e localiza a que a migração mais recente não foi aplicada ainda. Migrações do Code First aplica-se a migração mais recente, é executado o `Seed` método e, em seguida, a página é executada normalmente. Você ver a nova coluna de horário comercial com os dados de propagação.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implantação da atualização do banco de dados para o ambiente de produção

Você precisa alterar o perfil de publicação para o ambiente de produção também. Nesse caso, você será remova o perfil atual e crie um novo com a importação de um arquivo. publishsettings atualizado. O arquivo atualizado incluirá a cadeia de caracteres de conexão para o banco de dados do SQL Server em Cytanium.

Como você viu quando é implantado no ambiente de teste, você não precisa mais transformações de cadeia de caracteres de conexão no *Web.Production.config* arquivo de transformação. Abrir arquivo e remover o `connectionStrings` elemento. As transformações restantes são para o `Environment` valor o `appSettings` elemento e o `location` elemento que restringe o acesso a relatórios de erros do Elmah.

Antes de criar um novo perfil de publicação para produção, baixar um arquivo. publishsettings atualizada da mesma forma que você fez anteriormente no [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial. (No painel de controle Cytanium, clique em **Sites da Web**e, em seguida, clique no **contosouniversity.com** site. Selecione o **Web Publishing** guia e, em seguida, clique em **baixar perfil de publicação para este site**.) O motivo pelo qual que você está fazendo isso é obter a cadeia de caracteres de conexão de banco de dados no arquivo. publishsettings. A cadeia de caracteres de conexão não disponível na primeira vez em que você baixou o arquivo, porque você estava usando o SQL Server Compact e não tivesse criado o banco de dados do SQL Server no Cytanium ainda.

Agora você pode atualizar o perfil de publicação e publicar no ambiente de produção.

Abra o **Publicar Web** assistente e, em seguida, alternar para o **perfil** guia.

Clique em **gerenciar perfis**e, em seguida, exclua o perfil de produção.

Fechar o **Publicar Web** Assistente para salvar essa alteração.

Abra o **Publicar Web** novamente o assistente e depois clique em **importação**.

Sobre o **Conexão** , altere **URL de destino** para o valor apropriado se você estiver usando uma URL temporária.

Clique em **Avançar**.

Sobre o **configurações** , clique em **habilitar o banco de dados novas melhorias de publicação**.

Na lista suspensa de cadeia de caracteres de conexão para **SchoolContext**, selecione a cadeia de caracteres de conexão Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Selecione **migrações executar Code First (executado na inicialização do aplicativo)**.

Na lista suspensa de cadeia de caracteres de conexão para **DefaultConnection**, selecione a cadeia de caracteres de conexão Cytanium.

Selecione o **perfil** , clique em **gerenciar perfis**e renomeie o perfil de "contosouniversity.com - a implantação da Web" para "Produção".

Feche o perfil de publicação para salvar a alteração e abra-o novamente.

Clique em **Publicar**. (Para um site de produção real, copie *aplicativo\_offline.htm* para a produção e put na pasta do seu projeto antes de publicar, em seguida, removê-lo quando a implantação for concluída.)

Visual Studio implanta as alterações de código para o ambiente de teste e abre o navegador para a home page da Contoso University.

Selecione a página de professores.

Migrações do Code First atualiza o banco de dados da mesma maneira que no ambiente de teste. Você ver a nova coluna de horário comercial com os dados de propagação.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Você agora implantou com êxito uma atualização de aplicativo que incluía uma alteração de banco de dados, usando um banco de dados do SQL Server.

## <a name="more-information"></a>Mais informações

Isso conclui esta série de tutoriais sobre como implantar um aplicativo da web ASP.NET em um provedor de hospedagem de terceiros. Para obter mais informações sobre qualquer um dos tópicos abordados nesses tutoriais, consulte o [mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) no site do MSDN.

## <a name="acknowledgements"></a>Confirmações

Gostaria de agradecer seguintes pessoas que fizeram contribuições significativas para o conteúdo desta série tutorial:

- [Alberto Poblacion, MVP &amp; MCT, Espanha](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, MVP de desenvolvimento de plataforma de dados, EUA
- Adversos Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Pope, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Itália](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Hashimi sayed, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Sérvia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

>[!div class="step-by-step"]
[Anterior](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Próximo](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
