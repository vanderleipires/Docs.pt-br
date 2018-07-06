---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização de banco de dados do SQL Server - 11 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: c744bc54c8ce12820d1e1388ac7ab2db814ff031
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816620"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização de banco de dados do SQL Server - 11 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se você instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server que não seja o SQL Server Compact e mostra como implantar o Windows Azure Web Sites, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como implantar uma atualização de banco de dados em um banco de dados completo do SQL Server. Como as migrações Code First faz todo o trabalho de atualização do banco de dados, o processo é quase idêntico ao que você fez para o SQL Server Compact na [Implantando uma atualização de banco de dados](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="adding-a-new-column-to-a-table"></a>Adicionando uma nova coluna a uma tabela

Nesta seção do tutorial, você fará um banco de dados altere e alterações de código correspondente, em seguida, testá-los no Visual Studio em preparação para implantá-los para ambientes de teste e produção. A alteração envolve a adição de um `OfficeHours` coluna para o `Instructor` entidade e exibindo as novas informações na **instrutores** página da web.

No projeto ContosoUniversity.DAL, abra *Instructor.cs* e adicione a seguinte propriedade entre o `HireDate` e `Courses` propriedades:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

Atualize a classe de inicializador para que ele propaga a nova coluna com dados de teste. Abra *Migrations\Configuration.cs* e substitua o bloco de código começa `var instructors = new List<Instructor>` com o seguinte bloco de código que inclui a nova coluna:

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

No projeto ContosoUniversity, abra *Instructors.aspx* e adicione um novo campo de modelo para o horário de expediente antes do fechamento `</Columns>` marca no primeiro `GridView` controle:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

Compile a solução.

Abra o **Package Manager Console** janela e selecione ContosoUniversity.DAL como o **projeto padrão**.

Insira os seguintes comandos:

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

Execute o aplicativo e selecione o **instrutores** página. A página demora um pouco mais do que o normal para carregar, porque o Entity Framework recria o banco de dados e propagará com os dados de teste.

[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)

## <a name="deploying-the-database-update-to-the-test-environment"></a>Implantar a atualização de banco de dados para o ambiente de teste

Quando você usar migrações do Code First, o método para implantar uma alteração de banco de dados para o SQL Server é a mesma do SQL Server Compact. No entanto, você precisará alterar o teste de perfil de publicação porque ele ainda está configurado para migrar do SQL Server Compact para o SQL Server.

A primeira etapa é remover as transformações de cadeia de caracteres de conexão que você criou no tutorial anterior. Eles não são mais necessários porque você especificará as transformações de cadeia de caracteres de conexão no perfil de publicação, como você fez antes que você configurou a **empacotar/publicar SQL** guia para a migração para o SQL Server.

Abra o *Web.Test.config* do arquivo e remover o `connectionStrings` elemento. A única transformação restante no *Web.Test.config* arquivo destina-se a `Environment` o valor o `appSettings` elemento.

Agora você pode atualizar o perfil de publicação e publicar no ambiente de teste.

Abra o **Publicar Web** assistente e, em seguida, alterne para o **perfil** guia.

Selecione o **teste** perfil de publicação.

Selecione o **configurações** guia.

Clique em **habilitar o novo banco de dados de publicação melhorias**.

Na caixa de cadeia de caracteres de conexão para **SchoolContext**, insira o mesmo valor que você usou na *Web.Test.config* arquivo de transformação no tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

Selecione **executar migrações do Code First (executado na inicialização do aplicativo)**. (Na sua versão do Visual Studio, a caixa de seleção deve ser rotulada **aplicar migrações do Code First**.)

Na caixa de cadeia de caracteres de conexão para **DefaultConnection**, insira o mesmo valor que você usou na *Web.Test.config* arquivo de transformação no tutorial anterior:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

Deixe **Atualizar banco de dados** desmarcada.

Clique em **Publicar**.

Visual Studio implanta as alterações de código para o ambiente de teste e abre o navegador para a home page do Contoso University.

Selecione a página instrutores.

Quando o aplicativo é executado nessa página, ele tenta acessar o banco de dados. Migrações do Code First verifica se o banco de dados atual e localiza a que a migração mais recente ainda não foi aplicada. Migrações do Code First aplica-se a migração mais recente, será executado o `Seed` método e, em seguida, a página é executada normalmente. Você ver a nova coluna de horas de trabalho com os dados propagados.

[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)

## <a name="deploying-the-database-update-to-the-production-environment"></a>Implantar a atualização de banco de dados para o ambiente de produção

Você precisará alterar o perfil de publicação para o ambiente de produção também. Nesse caso, você remover o perfil existente e criar um novo importando um arquivo. publishsettings atualizado. O arquivo atualizado incluirá a cadeia de caracteres de conexão para o banco de dados do SQL Server em Cytanium.

Como você viu quando você implantou no ambiente de teste, você não precisa mais transformações de cadeia de caracteres de conexão na *Web.Production.config* arquivo de transformação. Abrir arquivo e remover o `connectionStrings` elemento. As transformações restantes são para o `Environment` o valor a `appSettings` elemento e o `location` elemento que restringe o acesso a relatórios de erros do Elmah.

Antes de criar um novo perfil de publicação para produção, baixar um arquivo. publishsettings atualizada da mesma forma que você fez anteriormente na [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial. (No painel de controle Cytanium, clique em **Sites da Web**e, em seguida, clique no **contosouniversity.com** site. Selecione o **Web Publishing** guia e, em seguida, clique em **baixar perfil de publicação para este site**.) É o motivo pelo qual que você estiver fazendo isso acompanhar a cadeia de caracteres de conexão de banco de dados no arquivo. publishsettings. A cadeia de caracteres de conexão não disponível na primeira vez em que você baixou o arquivo, porque você estava usando o SQL Server Compact e ainda não tenha criado o banco de dados do SQL Server em Cytanium ainda.

Agora você pode atualizar o perfil de publicação e publicar no ambiente de produção.

Abra o **Publicar Web** assistente e, em seguida, alterne para o **perfil** guia.

Clique em **gerenciar perfis**e, em seguida, exclua o perfil de produção.

Fechar o **publicar na Web** Assistente para salvar essa alteração.

Abra o **Publicar Web** novamente o assistente e clique **importação**.

Sobre o **Conexão** , altere **URL de destino** para o valor apropriado, se você estiver usando uma URL temporária.

Clique em **Avançar**.

Sobre o **as configurações** , clique em **habilitar novo melhorias de publicação do banco de dados**.

Na lista suspensa de cadeia de caracteres de conexão para **SchoolContext**, selecione a cadeia de caracteres de conexão Cytanium.

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

Selecione **migrações Execute Code First (executado na inicialização do aplicativo)**.

Na lista suspensa de cadeia de caracteres de conexão para **DefaultConnection**, selecione a cadeia de caracteres de conexão Cytanium.

Selecione o **perfil** , clique em **gerenciar perfis**e renomeie o perfil de "contosouniversity.com - a implantação da Web" para "Produção".

Feche o perfil de publicação para salvar a alteração, em seguida, abra-o novamente.

Clique em **Publicar**. (Para um site de produção real, copie *app\_offline.htm* para produção e put-lo na pasta do projeto antes da publicação, em seguida, removê-lo quando a implantação for concluída.)

Visual Studio implanta as alterações de código para o ambiente de teste e abre o navegador para a home page do Contoso University.

Selecione a página instrutores.

Migrações do Code First atualiza o banco de dados da mesma maneira que no ambiente de teste. Você ver a nova coluna de horas de trabalho com os dados propagados.

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

Você tem agora implantado com êxito uma atualização de aplicativo que incluía uma alteração de banco de dados, usando um banco de dados do SQL Server.

## <a name="more-information"></a>Mais informações

Isso conclui esta série de tutoriais sobre como implantar um aplicativo da web ASP.NET em um provedor de hospedagem de terceiros. Para obter mais informações sobre qualquer um dos tópicos abordados nesses tutoriais, consulte o [mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) no site do MSDN.

## <a name="acknowledgements"></a>Confirmações

Eu gostaria de agradecer a pessoas que fizeram contribuições significativas para o conteúdo desta série de tutoriais:

- [Alberto Poblacion, MVP &amp; MCT, Espanha](https://mvp.support.microsoft.com/profile/Alberto)
- Jarod Ferguson, MVP de desenvolvimento de plataforma de dados, dos Estados Unidos
- Harsh Mittal, Microsoft
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Papa, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Itália](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Sérvia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)
