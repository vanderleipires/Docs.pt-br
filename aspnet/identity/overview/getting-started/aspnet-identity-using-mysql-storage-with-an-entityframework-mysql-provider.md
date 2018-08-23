---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'Identidade do ASP.NET: Usar o armazenamento do MySQL com um provedor de MySQL do EntityFramework (c#) | Microsoft Docs'
author: maumar
description: Este tutorial mostra como substituir o mecanismo de armazenamento de dados padrão para a identidade do ASP.NET com EntityFramework (provedor de cliente SQL) por uma ou se contrair MySQL...
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 3856b40b31a3deb6ad1c6c5d2cd678e183f012d7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825224"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="e15b1-103">Identidade do ASP.NET: Usar o armazenamento do MySQL com um provedor de MySQL do EntityFramework (c#)</span><span class="sxs-lookup"><span data-stu-id="e15b1-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="e15b1-104">por [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="e15b1-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="e15b1-105">Este tutorial mostra como substituir o mecanismo de armazenamento de dados padrão para [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) com EntityFramework (provedor de cliente SQL) com um provedor de MySQL.</span><span class="sxs-lookup"><span data-stu-id="e15b1-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="e15b1-106">Os seguintes tópicos serão abordados neste tutorial:</span><span class="sxs-lookup"><span data-stu-id="e15b1-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="e15b1-107">Criação de um banco de dados MySQL no Azure</span><span class="sxs-lookup"><span data-stu-id="e15b1-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="e15b1-108">Criando um aplicativo MVC usando o modelo MVC do Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="e15b1-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="e15b1-109">Configurando o EntityFramework para trabalhar com um provedor de banco de dados MySQL</span><span class="sxs-lookup"><span data-stu-id="e15b1-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="e15b1-110">Executando o aplicativo para verificar os resultados</span><span class="sxs-lookup"><span data-stu-id="e15b1-110">Running the application to verify the results</span></span>

<span data-ttu-id="e15b1-111">No final deste tutorial, você terá um aplicativo MVC com a identidade do ASP.NET armazene que está usando um banco de dados MySQL que está hospedado no Azure.</span><span class="sxs-lookup"><span data-stu-id="e15b1-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="e15b1-112">Criação de uma instância do banco de dados MySQL no Azure</span><span class="sxs-lookup"><span data-stu-id="e15b1-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="e15b1-113">Faça logon no [portal do Azure](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="e15b1-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="e15b1-114">Clique em **NEW** na parte inferior da página e, em seguida, selecione **repositório**:</span><span class="sxs-lookup"><span data-stu-id="e15b1-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="e15b1-115">No **escolher e o complemento** assistente, selecione **banco de dados ClearDB MySQL**e, em seguida, clique no **próxima** seta na parte inferior do quadro:</span><span class="sxs-lookup"><span data-stu-id="e15b1-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="e15b1-116">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-116">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-117">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="e15b1-118">Mantenha o padrão **livre** planejar, altere o **nome** para **IdentityMySQLDatabase**, selecione a região mais próxima de você e, em seguida, clique no **Avançar** seta na parte inferior do quadro:</span><span class="sxs-lookup"><span data-stu-id="e15b1-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="e15b1-119">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-119">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-120">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="e15b1-121">Clique o **compra** marca de seleção para concluir a criação do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e15b1-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
   <span data-ttu-id="e15b1-122">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-122">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-123">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="e15b1-124">Depois que seu banco de dados tiver sido criado, você pode gerenciá-lo na **complementos** guia no portal de gerenciamento.</span><span class="sxs-lookup"><span data-stu-id="e15b1-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="e15b1-125">Para recuperar as informações de conexão do banco de dados, clique em **informações de conexão** na parte inferior da página:</span><span class="sxs-lookup"><span data-stu-id="e15b1-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
   <span data-ttu-id="e15b1-126">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-126">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-127">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="e15b1-128">Copie a cadeia de conexão clicando no botão de cópia, o **CONNECTIONSTRING** campo e salvá-lo; você usará essas informações posteriormente no tutorial para seu aplicativo MVC:</span><span class="sxs-lookup"><span data-stu-id="e15b1-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
   <span data-ttu-id="e15b1-129">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-129">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-130">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="e15b1-131">Criando um projeto de aplicativo do MVC</span><span class="sxs-lookup"><span data-stu-id="e15b1-131">Creating an MVC application project</span></span>

<span data-ttu-id="e15b1-132">Para concluir as etapas nesta seção do tutorial, você primeiro precisará instalar [Visual Studio Express 2013 para Web](https://go.microsoft.com/fwlink/?LinkId=299058) ou [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="e15b1-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="e15b1-133">Depois que o Visual Studio tiver sido instalado, use as seguintes etapas para criar um novo projeto de aplicativo do MVC:</span><span class="sxs-lookup"><span data-stu-id="e15b1-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="e15b1-134">Abra o Visual Studio 2103.</span><span class="sxs-lookup"><span data-stu-id="e15b1-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="e15b1-135">Clique em **novo projeto** da **inicie** página, ou você pode clicar na **arquivo** menu e, em seguida, **novo projeto**:</span><span class="sxs-lookup"><span data-stu-id="e15b1-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
   <span data-ttu-id="e15b1-136">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-136">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-137">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="e15b1-138">Quando o **novo projeto** caixa de diálogo for exibida, expanda **Visual c#** na lista de modelos, em seguida, clique em **Web**e selecione **ASP.NET Web Application**.</span><span class="sxs-lookup"><span data-stu-id="e15b1-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="e15b1-139">Nomeie o projeto **IdentityMySQLDemo** e, em seguida, clique em **Okey**:</span><span class="sxs-lookup"><span data-stu-id="e15b1-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
   <span data-ttu-id="e15b1-140">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-140">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-141">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="e15b1-142">No **novo projeto ASP.NET** caixa de diálogo, selecione o **MVC** templatewith as opções padrão; esse será configurar **contas de usuário individuais** como método de autenticação.</span><span class="sxs-lookup"><span data-stu-id="e15b1-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="e15b1-143">Clique em **OK**:</span><span class="sxs-lookup"><span data-stu-id="e15b1-143">Click **OK**:</span></span>  
  
   <span data-ttu-id="e15b1-144">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-144">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-145">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="e15b1-146">Configurar o EntityFramework para trabalhar com um banco de dados MySQL</span><span class="sxs-lookup"><span data-stu-id="e15b1-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="e15b1-147">Atualizar o assembly do Entity Framework para o seu projeto</span><span class="sxs-lookup"><span data-stu-id="e15b1-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="e15b1-148">O aplicativo de MVC que foi criado a partir do modelo do Visual Studio 2013 contém uma referência para o [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) do pacote, mas houve foi atualizações a esse assembly desde seu lançamento, que contêm significativa melhorias de desempenho.</span><span class="sxs-lookup"><span data-stu-id="e15b1-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="e15b1-149">Para usar essas atualizações mais recentes em seu aplicativo, use as etapas a seguir.</span><span class="sxs-lookup"><span data-stu-id="e15b1-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="e15b1-150">Abra seu projeto do MVC no Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e15b1-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="e15b1-151">Clique em **ferramentas**, em seguida, clique em **Gerenciador de pacotes de biblioteca**e, em seguida, clique em **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="e15b1-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
   <span data-ttu-id="e15b1-152">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-152">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-153">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="e15b1-154">O **Package Manager Console** aparecerá na seção inferior do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e15b1-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="e15b1-155">Tipo de &quot; **Update-Package EntityFramework** &quot; e pressione Enter:</span><span class="sxs-lookup"><span data-stu-id="e15b1-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
   <span data-ttu-id="e15b1-156">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-156">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-157">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="e15b1-158">Instalar o provedor de MySQL para EntityFramework</span><span class="sxs-lookup"><span data-stu-id="e15b1-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="e15b1-159">Para o EntityFramework para se conectar ao banco de dados MySQL, você precisa instalar um provedor de MySQL.</span><span class="sxs-lookup"><span data-stu-id="e15b1-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="e15b1-160">Para fazer isso, abra o **Package Manager Console** e digite &quot; **MySql.Data.Entity Install-Package - Pre**&quot;, e pressione Enter.</span><span class="sxs-lookup"><span data-stu-id="e15b1-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="e15b1-161">Esta é uma versão de pré-lançamento do assembly e como tal, ele pode conter bugs.</span><span class="sxs-lookup"><span data-stu-id="e15b1-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="e15b1-162">Você não deve usar uma versão de pré-lançamento do provedor em produção.</span><span class="sxs-lookup"><span data-stu-id="e15b1-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="e15b1-163">[Clique a imagem a seguir para expandi-la.]</span><span class="sxs-lookup"><span data-stu-id="e15b1-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="e15b1-164">Fazer alterações de configuração de projeto para o arquivo Web. config para seu aplicativo</span><span class="sxs-lookup"><span data-stu-id="e15b1-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="e15b1-165">Nesta seção que você irá configurar o Entity Framework para usar o provedor de MySQL que acabou de instalar, registrar o alocador de provedores de MySQL e adicione sua cadeia de caracteres de conexão do Azure.</span><span class="sxs-lookup"><span data-stu-id="e15b1-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="e15b1-166">Os exemplos a seguir contém uma versão do assembly específico para MySql.Data.dll.</span><span class="sxs-lookup"><span data-stu-id="e15b1-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="e15b1-167">Se a versão do assembly for alterado, você precisará modificar as definições de configuração apropriado com a versão correta.</span><span class="sxs-lookup"><span data-stu-id="e15b1-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="e15b1-168">Abra o arquivo Web. config para seu projeto no Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="e15b1-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="e15b1-169">Localize as seguintes definições de configuração, que definem o provedor de banco de dados padrão e a fábrica para o Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="e15b1-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="e15b1-170">Substitua os parâmetros de configuração a seguir, que irá configurar o Entity Framework para usar o provedor de MySQL:</span><span class="sxs-lookup"><span data-stu-id="e15b1-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="e15b1-171">Localize o &lt;connectionStrings&gt; seção e substitua-o pelo código a seguir, que definirá a cadeia de caracteres de conexão do banco de dados MySQL que está hospedado no Azure (Observe que o valor de providerName também foi alterado da original):</span><span class="sxs-lookup"><span data-stu-id="e15b1-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="e15b1-172">Adicionar contexto de MigrationHistory personalizado</span><span class="sxs-lookup"><span data-stu-id="e15b1-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="e15b1-173">Entity Framework Code First usa um **MigrationHistory** tabela para manter controle das alterações de modelo e para garantir a consistência entre o esquema de banco de dados e esquema conceitual.</span><span class="sxs-lookup"><span data-stu-id="e15b1-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="e15b1-174">No entanto, essa tabela não funciona para MySQL por padrão porque a chave primária é muito grande.</span><span class="sxs-lookup"><span data-stu-id="e15b1-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="e15b1-175">Para corrigir essa situação, você precisa reduzir o tamanho da chave para a tabela.</span><span class="sxs-lookup"><span data-stu-id="e15b1-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="e15b1-176">Para fazer isso, use as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="e15b1-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="e15b1-177">As informações de esquema para essa tabela são capturadas em uma **HistoryContext**, que pode ser modificado como qualquer outro **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="e15b1-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="e15b1-178">Para fazer isso, adicione um novo arquivo de classe chamado **MySqlHistoryContext.cs** ao projeto e substitua seu conteúdo pelo seguinte código:</span><span class="sxs-lookup"><span data-stu-id="e15b1-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="e15b1-179">Em seguida, será necessário configurar o Entity Framework para usar o modificada **HistoryContext**, em vez do padrão.</span><span class="sxs-lookup"><span data-stu-id="e15b1-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="e15b1-180">Isso pode ser feito, aproveitando os recursos de configuração com base em código.</span><span class="sxs-lookup"><span data-stu-id="e15b1-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="e15b1-181">Para fazer isso, adicione o novo arquivo de classe chamado **MySqlConfiguration.cs** ao seu projeto e substitua seu conteúdo com:</span><span class="sxs-lookup"><span data-stu-id="e15b1-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="e15b1-182">Criação de um inicializador personalizado do EntityFramework para ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="e15b1-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="e15b1-183">O provedor de MySQL é mostrado neste tutorial não suporta no momento, as migrações do Entity Framework, portanto, será necessário usar os inicializadores de modelo para se conectar ao banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e15b1-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="e15b1-184">Como este tutorial está usando uma instância do MySQL no Azure, você precisará criar um inicializador personalizado do Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e15b1-184">Because this tutorial is using a MySQL instance on Azure, you will need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="e15b1-185">Essa etapa não será necessária se você estiver se conectando a uma instância do SQL Server no Azure ou se você estiver usando um banco de dados estiver hospedado localmente.</span><span class="sxs-lookup"><span data-stu-id="e15b1-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="e15b1-186">Para criar um inicializador personalizado do Entity Framework para MySQL, use as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="e15b1-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="e15b1-187">Adicionar um novo arquivo de classe chamado **MySqlInitializer.cs** para o projeto e substitua é conteúdo com o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="e15b1-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="e15b1-188">Abra o **IdentityModels.cs** arquivo do seu projeto, que está localizado na **modelos** diretório e substitua seu conteúdo pelo seguinte:</span><span class="sxs-lookup"><span data-stu-id="e15b1-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="e15b1-189">Executando o aplicativo e verificar o banco de dados</span><span class="sxs-lookup"><span data-stu-id="e15b1-189">Running the application and verifying the database</span></span>

<span data-ttu-id="e15b1-190">Depois de concluir as etapas nas seções anteriores, você deve testar seu banco de dados.</span><span class="sxs-lookup"><span data-stu-id="e15b1-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="e15b1-191">Para fazer isso, use as seguintes etapas:</span><span class="sxs-lookup"><span data-stu-id="e15b1-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="e15b1-192">Pressione **Ctrl + F5** para compilar e executar o aplicativo web.</span><span class="sxs-lookup"><span data-stu-id="e15b1-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="e15b1-193">Clique o **registrar** guia na parte superior da página:</span><span class="sxs-lookup"><span data-stu-id="e15b1-193">Click the **Register** tab on the top of the page:</span></span>  
  
   <span data-ttu-id="e15b1-194">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-194">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-195">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="e15b1-196">Insira um novo nome de usuário e senha e, em seguida, clique em **registrar**:</span><span class="sxs-lookup"><span data-stu-id="e15b1-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
   <span data-ttu-id="e15b1-197">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-197">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-198">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="e15b1-199">Neste ponto, as tabelas de identidade do ASP.NET são criadas no banco de dados MySQL e o usuário está registrado e conectado ao aplicativo:</span><span class="sxs-lookup"><span data-stu-id="e15b1-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
   <span data-ttu-id="e15b1-200">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-200">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-201">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="e15b1-202">Instalando a ferramenta MySQL Workbench para verificar os dados</span><span class="sxs-lookup"><span data-stu-id="e15b1-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="e15b1-203">Instalar o **MySQL Workbench** ferramenta do [página de downloads do MySQL](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="e15b1-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="e15b1-204">No Assistente de instalação: **seleção de recursos** guia, selecione **MySQL Workbench** sob **aplicativos** seção.</span><span class="sxs-lookup"><span data-stu-id="e15b1-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="e15b1-205">Inicie o aplicativo e adicionar uma nova conexão usando os dados de cadeia de caracteres de conexão do banco de dados MySQL do Azure criada no implorando deste tutorial.</span><span class="sxs-lookup"><span data-stu-id="e15b1-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="e15b1-206">Depois de estabelecer a conexão, inspecione a **ASP.NET Identity** tabelas criadas no **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="e15b1-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="e15b1-207">Você verá que todos os ASP.NET requeridos para identidade tabelas são criadas conforme mostrado na imagem abaixo:</span><span class="sxs-lookup"><span data-stu-id="e15b1-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
   <span data-ttu-id="e15b1-208">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-208">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-209">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="e15b1-210">Inspecione o **aspnetusers** por instância de tabela para verificar as entradas como registrar novos usuários.</span><span class="sxs-lookup"><span data-stu-id="e15b1-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
   <span data-ttu-id="e15b1-211">[Clique a imagem a seguir para expandi-la.</span><span class="sxs-lookup"><span data-stu-id="e15b1-211">[Click the following image to expand it.</span></span> <span data-ttu-id="e15b1-212">]</span><span class="sxs-lookup"><span data-stu-id="e15b1-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
