---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: "Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código | Microsoft Docs"
author: tdykstra
description: "Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, por usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: 10da2b5013ae1348b69ea4f456d81bb4c4b73df6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="708aa-103">Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código</span><span class="sxs-lookup"><span data-stu-id="708aa-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="708aa-104">Por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="708aa-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="708aa-105">Baixe o projeto Starter</span><span class="sxs-lookup"><span data-stu-id="708aa-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="708aa-106">Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="708aa-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="708aa-107">Para obter informações sobre a série, consulte [primeiro tutorial na série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="708aa-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="708aa-108">Visão Geral</span><span class="sxs-lookup"><span data-stu-id="708aa-108">Overview</span></span>

<span data-ttu-id="708aa-109">Após a implantação inicial, o trabalho de manutenção e desenvolvimento de seu site continua e pouco tempo você deseja implantar uma atualização.</span><span class="sxs-lookup"><span data-stu-id="708aa-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="708aa-110">Este tutorial leva você através do processo de implantação de uma atualização para o código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="708aa-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="708aa-111">A atualização que você implementa e implantar este tutorial não envolve uma alteração de banco de dados; Você verá qual é a diferença sobre a implantação de uma alteração de banco de dados do tutorial Avançar.</span><span class="sxs-lookup"><span data-stu-id="708aa-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="708aa-112">Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="708aa-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="708aa-113">Alterar um código</span><span class="sxs-lookup"><span data-stu-id="708aa-113">Make a code change</span></span>

<span data-ttu-id="708aa-114">Como um exemplo simples de uma atualização para o seu aplicativo, você adicionará o **instrutores** página uma lista de cursos ministrada pelo instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="708aa-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="708aa-115">Se você executar o **instrutores** página, você observará que há **selecione** links na grade, mas eles não fazem nada diferente de tornar o cinza de ativar linha em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="708aa-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Página seleção de instrutores](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="708aa-117">Agora você adicionará código que é executado quando o **selecione** link é clicado e exibe uma lista de cursos ministrada pelo instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="708aa-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="708aa-118">Em *Instructors.aspx*, adicione a seguinte marcação imediatamente após o **ErrorMessageLabel** `Label` controle:</span><span class="sxs-lookup"><span data-stu-id="708aa-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="708aa-119">Execute a página e selecione um instrutor.</span><span class="sxs-lookup"><span data-stu-id="708aa-119">Run the page and select an instructor.</span></span> <span data-ttu-id="708aa-120">Você ver uma lista de cursos ministrada por esse instrutor.</span><span class="sxs-lookup"><span data-stu-id="708aa-120">You see a list of courses taught by that instructor.</span></span>

    ![Página de instrutores cursos ministrada](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="708aa-122">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="708aa-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="708aa-123">Implante a atualização de código para o ambiente de teste</span><span class="sxs-lookup"><span data-stu-id="708aa-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="708aa-124">Antes de usar os perfis de publicação para implantar em teste, preparação e produção, você precisa alterar as opções de publicação de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="708aa-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="708aa-125">Você não precisa executar os scripts de implantação de conceder e dados para o banco de dados de associação.</span><span class="sxs-lookup"><span data-stu-id="708aa-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="708aa-126">Abra o **Publicar Web** assistente clicando duas vezes no projeto ContosoUniversity e clicando em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="708aa-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="708aa-127">Clique o **teste** perfil no **perfil** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="708aa-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="708aa-128">Clique o **configurações** guia.</span><span class="sxs-lookup"><span data-stu-id="708aa-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="708aa-129">Em **DefaultConnection** no **bancos de dados** seção, desmarque o **Atualizar banco de dados** caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="708aa-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="708aa-130">Clique o **perfil** guia e, em seguida, clique no **preparo** perfil no **perfil** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="708aa-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="708aa-131">Quando for perguntado se você deseja salvar as alterações feitas a **teste** de perfil, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="708aa-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="708aa-132">Fazer a mesma alteração no perfil de preparo.</span><span class="sxs-lookup"><span data-stu-id="708aa-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="708aa-133">Repita o processo para fazer a mesma alteração no perfil de produção.</span><span class="sxs-lookup"><span data-stu-id="708aa-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="708aa-134">Fechar o **Publicar Web** assistente.</span><span class="sxs-lookup"><span data-stu-id="708aa-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="708aa-135">A implantação no ambiente de teste é agora uma simples questão de executar um clique publicar novamente.</span><span class="sxs-lookup"><span data-stu-id="708aa-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="708aa-136">Para tornar esse processo mais rápido, você pode usar o **Web um clique em publicar** barra de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="708aa-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="708aa-137">No **exibição** menu, escolha **barras de ferramentas** e, em seguida, selecione **Web um clique em publicar**.</span><span class="sxs-lookup"><span data-stu-id="708aa-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="708aa-139">Em **Solution Explorer**, selecione o projeto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="708aa-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="708aa-140">o **Web um clique em publicar** barra de ferramentas, escolha o **teste** perfil de publicação e, em seguida, clique em **Publicar Web** (o ícone com setas que apontam para a esquerda e direita).</span><span class="sxs-lookup"><span data-stu-id="708aa-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="708aa-142">O Visual Studio implanta o aplicativo atualizado e o navegador é aberto automaticamente para a home page.</span><span class="sxs-lookup"><span data-stu-id="708aa-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="708aa-143">Execute a página de professores e selecione um instrutor para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="708aa-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="708aa-144">Normalmente também fazer o teste de regressão (ou seja, teste o restante do site para certificar-se de que a nova alteração não violam nenhuma funcionalidade existente).</span><span class="sxs-lookup"><span data-stu-id="708aa-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="708aa-145">Mas, para este tutorial você vai ignorar essa etapa e vá para implantar a atualização em preparação e produção.</span><span class="sxs-lookup"><span data-stu-id="708aa-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="708aa-146">Quando você reimplanta, Web Deploy determina automaticamente quais arquivos foram alterados e somente cópias de arquivos alterados para o servidor.</span><span class="sxs-lookup"><span data-stu-id="708aa-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="708aa-147">Por padrão, implantação da Web usa datas alterada por último em arquivos para determinar quais foram alterados.</span><span class="sxs-lookup"><span data-stu-id="708aa-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="708aa-148">Alguns sistemas de controle de origem alterar arquivo datas mesmo quando você não alterar o conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="708aa-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="708aa-149">Nesse caso, você poderá configurar a implantação da Web para usar as somas de verificação para determinar quais arquivos foram alterados.</span><span class="sxs-lookup"><span data-stu-id="708aa-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="708aa-150">Para obter mais informações, consulte [por que todos os meus arquivos obter reimplantados embora eu não alterá-los?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#use_checksum) nas perguntas Frequentes de implantação de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="708aa-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="708aa-151">Colocar o aplicativo offline durante a implantação</span><span class="sxs-lookup"><span data-stu-id="708aa-151">Take the application offline during deployment</span></span>

<span data-ttu-id="708aa-152">A alteração que você está implantando agora é uma alteração simple em uma única página.</span><span class="sxs-lookup"><span data-stu-id="708aa-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="708aa-153">Mas, às vezes, você pode implantar alterações maiores, ou implantar as alterações de código e o banco de dados e o site pode se comportar incorretamente se um usuário solicita uma página antes da conclusão da implantação.</span><span class="sxs-lookup"><span data-stu-id="708aa-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="708aa-154">Para impedir que os usuários acessem o site durante a implantação está em andamento, você pode usar um *aplicativo\_offline.htm* arquivo.</span><span class="sxs-lookup"><span data-stu-id="708aa-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="708aa-155">Quando você coloca um arquivo chamado *aplicativo\_offline.htm* na pasta raiz do seu aplicativo, o IIS exibe automaticamente esse arquivo em vez de executar seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="708aa-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="708aa-156">Portanto para impedir acesso durante a implantação, você deve colocar *aplicativo\_offline.htm* na pasta raiz, execute o processo de implantação e, em seguida, remover *aplicativo\_offline.htm* depois bem-sucedida implantação.</span><span class="sxs-lookup"><span data-stu-id="708aa-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="708aa-157">Você pode configurar a implantação da Web para colocar automaticamente um padrão *aplicativo\_offline.htm* de arquivos no servidor quando ele começar a implantar e removê-lo quando ele for concluído.</span><span class="sxs-lookup"><span data-stu-id="708aa-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="708aa-158">Para fazer o que você precisa fazer é adicionar o seguinte elemento XML para o arquivo de perfil (. pubxml) de publicação:</span><span class="sxs-lookup"><span data-stu-id="708aa-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="708aa-159">Para este tutorial, você verá como criar e usar um personalizado *aplicativo\_offline.htm* arquivo.</span><span class="sxs-lookup"><span data-stu-id="708aa-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="708aa-160">Usando *aplicativo\_offline.htm* no site de preparo não é necessária, porque você não tem usuários que acessam o site de preparo.</span><span class="sxs-lookup"><span data-stu-id="708aa-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="708aa-161">Mas é uma boa prática usar preparo para testar tudo o modo como você planeja implantar na produção.</span><span class="sxs-lookup"><span data-stu-id="708aa-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="708aa-162">Criar aplicativo\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="708aa-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="708aa-163">Em **Solution Explorer**, clique com botão direito a solução e clique em **adicionar**e, em seguida, clique em **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="708aa-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="708aa-164">Criar um **página HTML** chamado *aplicativo\_offline.htm* (excluir o último "l" no *. HTML* extensão que o Visual Studio cria por padrão).</span><span class="sxs-lookup"><span data-stu-id="708aa-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="708aa-165">Substitua a marcação de modelo com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="708aa-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="708aa-166">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="708aa-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="708aa-167">Aplicativo de cópia\_offline.htm para a pasta raiz do site da web</span><span class="sxs-lookup"><span data-stu-id="708aa-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="708aa-168">Você pode usar qualquer ferramenta FTP para copiar arquivos para o site da web.</span><span class="sxs-lookup"><span data-stu-id="708aa-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="708aa-169">[FileZilla](http://filezilla-project.org/) é uma ferramenta popular de FTP e é mostrado nas capturas de tela.</span><span class="sxs-lookup"><span data-stu-id="708aa-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="708aa-170">Para usar uma ferramenta FTP, é necessário que três coisas: a URL de FTP, o nome de usuário e a senha.</span><span class="sxs-lookup"><span data-stu-id="708aa-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="708aa-171">A URL é exibida na página do painel de controle do site da web no Portal de gerenciamento do Azure e o nome de usuário e senha de FTP podem ser encontrados na *. publishsettings* arquivo que você baixou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="708aa-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="708aa-172">As etapas a seguir mostram como obter esses valores.</span><span class="sxs-lookup"><span data-stu-id="708aa-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="708aa-173">No Portal de gerenciamento do Azure, clique em **Sites da Web** guia e, em seguida, clique no site de preparo.</span><span class="sxs-lookup"><span data-stu-id="708aa-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="708aa-174">Sobre o **painel** página, role para baixo até localizar o nome de host do FTP no **visão rápida** seção.</span><span class="sxs-lookup"><span data-stu-id="708aa-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nome de host do FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="708aa-176">Abra a preparo *. publishsettings* arquivo no bloco de notas ou outro editor de texto.</span><span class="sxs-lookup"><span data-stu-id="708aa-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="708aa-177">Localizar o `publishProfile` elemento para o perfil de FTP.</span><span class="sxs-lookup"><span data-stu-id="708aa-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="708aa-178">Copie o `userName` e `userPWD` valores.</span><span class="sxs-lookup"><span data-stu-id="708aa-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Credenciais de FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="708aa-180">Abra sua ferramenta FTP e faça logon no URL FTP.</span><span class="sxs-lookup"><span data-stu-id="708aa-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="708aa-181">Cópia *aplicativo\_offline.htm* da pasta de solução para o */site/wwwroot* pasta no site de preparo.</span><span class="sxs-lookup"><span data-stu-id="708aa-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Copiar app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="708aa-183">Navegue até a URL do site de preparo.</span><span class="sxs-lookup"><span data-stu-id="708aa-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="708aa-184">Você verá que o *aplicativo\_offline.htm* página agora é exibida em vez de sua home page.</span><span class="sxs-lookup"><span data-stu-id="708aa-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline.htm na janela do navegador](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="708aa-186">Agora você está pronto para implantar em preparo.</span><span class="sxs-lookup"><span data-stu-id="708aa-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="708aa-187">Implante a atualização de código para teste e produção</span><span class="sxs-lookup"><span data-stu-id="708aa-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="708aa-188">No **Web um clique em publicar** barra de ferramentas, escolha o **preparo** perfil de publicação e, em seguida, clique em **Publicar Web**.</span><span class="sxs-lookup"><span data-stu-id="708aa-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="708aa-189">Visual Studio implanta o aplicativo atualizado e abre o navegador para a home page do site.</span><span class="sxs-lookup"><span data-stu-id="708aa-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="708aa-190">O *aplicativo\_offline.htm* arquivo é exibido.</span><span class="sxs-lookup"><span data-stu-id="708aa-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="708aa-191">Antes de você testar para verificar a implantação bem-sucedida, você deve remover o *aplicativo\_offline.htm* arquivo.</span><span class="sxs-lookup"><span data-stu-id="708aa-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="708aa-192">Voltar para a ferramenta de FTP e excluir **aplicativo\_offline.htm** do site de preparo.</span><span class="sxs-lookup"><span data-stu-id="708aa-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="708aa-193">No navegador, abra a página de professores no site de preparo e selecione um instrutor para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="708aa-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="708aa-194">Siga o mesmo procedimento para produção quanto à preparação.</span><span class="sxs-lookup"><span data-stu-id="708aa-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="708aa-195">Revisar as alterações e implantar arquivos específicos</span><span class="sxs-lookup"><span data-stu-id="708aa-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="708aa-196">O Visual Studio 2012 também fornece a capacidade de implantar arquivos individuais.</span><span class="sxs-lookup"><span data-stu-id="708aa-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="708aa-197">Para o arquivo selecionado exibir diferenças entre a versão local e a versão implantada, implante o arquivo para o ambiente de destino ou copie o arquivo do ambiente de destino para o local do projeto.</span><span class="sxs-lookup"><span data-stu-id="708aa-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="708aa-198">Nesta seção do tutorial, você verá como usar esses recursos.</span><span class="sxs-lookup"><span data-stu-id="708aa-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="708aa-199">Fazer uma alteração para implantar</span><span class="sxs-lookup"><span data-stu-id="708aa-199">Make a change to deploy</span></span>

1. <span data-ttu-id="708aa-200">Abra *Content/Site.css*e localize o bloco para o `body` marca.</span><span class="sxs-lookup"><span data-stu-id="708aa-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="708aa-201">Altere o valor de `background-color` de `#fff` para `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="708aa-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="708aa-202">Exiba a alteração na janela de visualização de publicação</span><span class="sxs-lookup"><span data-stu-id="708aa-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="708aa-203">Quando você usa o **Publicar Web** Assistente para publicar o projeto, você pode ver quais alterações serão publicadas clicando duas vezes no **visualização** janela.</span><span class="sxs-lookup"><span data-stu-id="708aa-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="708aa-204">Clique com botão direito no projeto ContosoUniversity e clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="708aa-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="708aa-205">Do **perfil** lista suspensa, selecione o **teste** perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="708aa-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="708aa-206">Clique em **visualização**e, em seguida, clique em **visualização iniciar**.</span><span class="sxs-lookup"><span data-stu-id="708aa-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="708aa-207">No **visualização** painel, clique duas vezes em **Site.css**.</span><span class="sxs-lookup"><span data-stu-id="708aa-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Clique duas vezes no site](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="708aa-209">O **visualizar alterações** caixa de diálogo mostra uma visualização das alterações que serão implantados.</span><span class="sxs-lookup"><span data-stu-id="708aa-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Visualizar alterações para o site](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="708aa-211">Se você clicar duas vezes o *Web. config* arquivo, o **visualizar alterações** caixa de diálogo mostra o efeito de sua compilação transformações de configuração e transformações de perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="708aa-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="708aa-212">Neste momento você não o fez tudo o que faria com que o *Web. config* arquivo no servidor para mudar, portanto você espera não ver nenhuma alteração.</span><span class="sxs-lookup"><span data-stu-id="708aa-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="708aa-213">No entanto, o **visualizar alterações** janela mostra duas alterações incorretamente.</span><span class="sxs-lookup"><span data-stu-id="708aa-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="708aa-214">Parece que dois elementos XML serão removidos.</span><span class="sxs-lookup"><span data-stu-id="708aa-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="708aa-215">Esses elementos são adicionados pelo processo de publicação quando você seleciona **executar migrações do Code First no início do aplicativo** para uma classe de contexto Code First.</span><span class="sxs-lookup"><span data-stu-id="708aa-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="708aa-216">A comparação é feita antes que o processo de publicação adiciona esses elementos, portanto parece que estão sendo removidos embora eles não serão removidos.</span><span class="sxs-lookup"><span data-stu-id="708aa-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="708aa-217">Esse erro será corrigido em uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="708aa-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="708aa-218">Clique em **Fechar**.</span><span class="sxs-lookup"><span data-stu-id="708aa-218">Click **Close**.</span></span>
6. <span data-ttu-id="708aa-219">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="708aa-219">Click **Publish**.</span></span>
7. <span data-ttu-id="708aa-220">Quando o navegador abre a home page do site de teste, pressione CTRL + F5 para fazer com que uma atualização de disco rígida para ver o efeito da alteração de CSS.</span><span class="sxs-lookup"><span data-stu-id="708aa-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Efeito da alteração CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="708aa-222">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="708aa-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="708aa-223">Publicar arquivos específicos no Gerenciador de soluções</span><span class="sxs-lookup"><span data-stu-id="708aa-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="708aa-224">Suponha que você não deseja que o plano de fundo azul e deseja reverter para a cor original.</span><span class="sxs-lookup"><span data-stu-id="708aa-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="708aa-225">Nesta seção, irá restaurar as configurações originais ao publicar um arquivo específico, diretamente do **Gerenciador de soluções**.</span><span class="sxs-lookup"><span data-stu-id="708aa-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="708aa-226">Abra *Content/Site.css* e restaurar o `background-color` definindo como `#fff`.</span><span class="sxs-lookup"><span data-stu-id="708aa-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="708aa-227">Em **Solution Explorer**, com o botão direito do *Content/Site.css* arquivo.</span><span class="sxs-lookup"><span data-stu-id="708aa-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="708aa-228">O menu de contexto exibe que três opções de publicação.</span><span class="sxs-lookup"><span data-stu-id="708aa-228">The context menu shows three publish options.</span></span>

    ![Publicar opções no Gerenciador de soluções](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="708aa-230">Clique em **visualizar alterações feitas em Site.css**.</span><span class="sxs-lookup"><span data-stu-id="708aa-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="708aa-231">Uma janela é aberta para mostrar as diferenças entre o arquivo local e a versão no ambiente de destino.</span><span class="sxs-lookup"><span data-stu-id="708aa-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-conteúdo/Site.css](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="708aa-233">Em **Solution Explorer**, clique com botão direito **Site.css** novamente e clique em **Site.css publicar**.</span><span class="sxs-lookup"><span data-stu-id="708aa-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="708aa-234">O **atividade de publicar Web** janela mostra que o arquivo foi publicado.</span><span class="sxs-lookup"><span data-stu-id="708aa-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Janela de atividade de publicação da Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="708aa-236">Abra um navegador para o `http://localhost/contosouniversity` URL e, em seguida, pressione CTRL + F5 para fazer com que um disco rígido atualizar para ver o efeito de CSS alterar.</span><span class="sxs-lookup"><span data-stu-id="708aa-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Home page com CSS normal](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="708aa-238">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="708aa-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="708aa-239">Resumo</span><span class="sxs-lookup"><span data-stu-id="708aa-239">Summary</span></span>

<span data-ttu-id="708aa-240">Você viu várias maneiras de implantar uma atualização de aplicativo que não envolve uma alteração de banco de dados, e você viu como visualizar as alterações para verificar se o que será atualizado é esperada.</span><span class="sxs-lookup"><span data-stu-id="708aa-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="708aa-241">A página instrutores agora tem um **cursos ministrada** seção.</span><span class="sxs-lookup"><span data-stu-id="708aa-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Página de instrutores cursos ministrada](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="708aa-243">O seguinte tutorial mostra como implementar uma alteração de banco de dados: você adicionará um campo de data de nascimento ao banco de dados e para a página de professores.</span><span class="sxs-lookup"><span data-stu-id="708aa-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="708aa-244">[Anterior](deploying-to-production.md)
[Próximo](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="708aa-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
