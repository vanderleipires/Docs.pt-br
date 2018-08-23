---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: 'Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web de aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: a3d181f3ec8db74781c550720ba4331bdf6478b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834878"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a><span data-ttu-id="3c49c-103">Implantação de Web do ASP.NET usando o Visual Studio: Implantando uma atualização de código</span><span class="sxs-lookup"><span data-stu-id="3c49c-103">ASP.NET Web Deployment using Visual Studio: Deploying a Code Update</span></span>
====================
<span data-ttu-id="3c49c-104">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="3c49c-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="3c49c-105">Baixe o projeto inicial</span><span class="sxs-lookup"><span data-stu-id="3c49c-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="3c49c-106">Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web application para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="3c49c-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="3c49c-107">Para obter informações sobre a série, consulte [o primeiro tutorial na série](introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3c49c-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="3c49c-108">Visão geral</span><span class="sxs-lookup"><span data-stu-id="3c49c-108">Overview</span></span>

<span data-ttu-id="3c49c-109">Após a implantação inicial, continua seu trabalho de manutenção e desenvolvimento de seu site da web e pouco tempo você deseja implantar uma atualização.</span><span class="sxs-lookup"><span data-stu-id="3c49c-109">After the initial deployment, your work of maintaining and developing your web site continues, and before long you will want to deploy an update.</span></span> <span data-ttu-id="3c49c-110">Este tutorial o guiará durante o processo de implantação de uma atualização para o código do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-110">This tutorial takes you through the process of deploying an update to your application code.</span></span> <span data-ttu-id="3c49c-111">A atualização que você implementa e implantar este tutorial não envolve uma alteração de banco de dados; Você verá o que é diferente sobre como implantar uma alteração de banco de dados no próximo tutorial.</span><span class="sxs-lookup"><span data-stu-id="3c49c-111">The update that you implement and deploy in this tutorial does not involve a database change; you'll see what's different about deploying a database change in the next tutorial.</span></span>

<span data-ttu-id="3c49c-112">Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="3c49c-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="make-a-code-change"></a><span data-ttu-id="3c49c-113">Alterar um código</span><span class="sxs-lookup"><span data-stu-id="3c49c-113">Make a code change</span></span>

<span data-ttu-id="3c49c-114">Como um exemplo simples de uma atualização para o seu aplicativo, você adicionará a **instrutores** página uma lista de cursos ministrados pelo instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="3c49c-114">As a simple example of an update to your application, you'll add to the **Instructors** page a list of courses taught by the selected instructor.</span></span>

<span data-ttu-id="3c49c-115">Se você executar o **instrutores** página, você observará que há **selecione** links na grade, mas eles não fazem nada diferente de marca a linha cinza de turno em segundo plano.</span><span class="sxs-lookup"><span data-stu-id="3c49c-115">If you run the **Instructors** page, you'll notice that there are **Select** links in the grid, but they don't do anything other than make the row background turn gray.</span></span>

![Página instrutores com seleção](deploying-a-code-update/_static/image1.png)

<span data-ttu-id="3c49c-117">Agora você adicionará código que é executada quando o **selecionar** link é clicado e exibe uma lista de cursos ministrados pelo instrutor selecionado.</span><span class="sxs-lookup"><span data-stu-id="3c49c-117">Now you'll add code that runs when the **Select** link is clicked and displays a list of courses taught by the selected instructor .</span></span>

1. <span data-ttu-id="3c49c-118">Na *Instructors.aspx*, adicione a seguinte marcação imediatamente após o **ErrorMessageLabel** `Label` controle:</span><span class="sxs-lookup"><span data-stu-id="3c49c-118">In *Instructors.aspx*, add the following markup immediately after the **ErrorMessageLabel** `Label` control:</span></span>

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. <span data-ttu-id="3c49c-119">Execute a página e selecione um instrutor.</span><span class="sxs-lookup"><span data-stu-id="3c49c-119">Run the page and select an instructor.</span></span> <span data-ttu-id="3c49c-120">Você ver uma lista de cursos ministrados por instrutor.</span><span class="sxs-lookup"><span data-stu-id="3c49c-120">You see a list of courses taught by that instructor.</span></span>

    ![Página instrutores com cursos ministrados](deploying-a-code-update/_static/image2.png)
3. <span data-ttu-id="3c49c-122">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="3c49c-122">Close the browser.</span></span>

## <a name="deploy-the-code-update-to-the-test-environment"></a><span data-ttu-id="3c49c-123">Implante a atualização de código para o ambiente de teste</span><span class="sxs-lookup"><span data-stu-id="3c49c-123">Deploy the code update to the test environment</span></span>

<span data-ttu-id="3c49c-124">Antes de usar seus perfis de publicação para implantar em teste, preparação e produção, você precisa alterar as opções de publicação de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="3c49c-124">Before you can use your publish profiles to deploy to test, staging, and production, you need to change database publishing options.</span></span> <span data-ttu-id="3c49c-125">Você não precisa executar os scripts de implantação de concessão e dados para o banco de dados de associação.</span><span class="sxs-lookup"><span data-stu-id="3c49c-125">You no longer need to run the grant and data deployment scripts for the membership database.</span></span>

1. <span data-ttu-id="3c49c-126">Abra o **Publicar Web** assistente clicando com botão direito no projeto ContosoUniversity e clicando em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-126">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="3c49c-127">Clique o **teste** perfil na **perfil** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="3c49c-127">Click the **Test** profile in the **Profile** drop-down list.</span></span>
3. <span data-ttu-id="3c49c-128">Clique o **configurações** guia.</span><span class="sxs-lookup"><span data-stu-id="3c49c-128">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="3c49c-129">Sob **DefaultConnection** na **bancos de dados** seção, desmarque o **Atualizar banco de dados** caixa de seleção.</span><span class="sxs-lookup"><span data-stu-id="3c49c-129">Under **DefaultConnection** in the **Databases** section, clear the **Update database** check box.</span></span>
5. <span data-ttu-id="3c49c-130">Clique o **perfil** guia e, em seguida, clique no **preparo** perfil na **perfil** lista suspensa.</span><span class="sxs-lookup"><span data-stu-id="3c49c-130">Click the **Profile** tab, and then click the **Staging** profile in the **Profile** drop-down list.</span></span>
6. <span data-ttu-id="3c49c-131">Quando for perguntado se você deseja salvar as alterações feitas para o **teste** de perfil, clique em **Sim**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-131">When you are asked if you want to save the changes made to the **Test** profile, click **Yes**.</span></span>
7. <span data-ttu-id="3c49c-132">Fazer a mesma alteração no perfil de preparo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-132">Make the same change in the Staging profile.</span></span>
8. <span data-ttu-id="3c49c-133">Repita o processo para fazer a mesma alteração no perfil produção.</span><span class="sxs-lookup"><span data-stu-id="3c49c-133">Repeat the process to make the same change in the Production profile.</span></span>
9. <span data-ttu-id="3c49c-134">Fechar o **publicar na Web** assistente.</span><span class="sxs-lookup"><span data-stu-id="3c49c-134">Close the **Publish Web** wizard.</span></span>

<span data-ttu-id="3c49c-135">A implantação no ambiente de teste é agora uma simples questão de execução em um único clique publique novamente.</span><span class="sxs-lookup"><span data-stu-id="3c49c-135">Deploying to the test environment is now a simple matter of running one-click publish again.</span></span> <span data-ttu-id="3c49c-136">Para tornar esse processo mais rápido, você pode usar o **publicação Web com um clique** barra de ferramentas.</span><span class="sxs-lookup"><span data-stu-id="3c49c-136">To make this process quicker, you can use the **Web One Click Publish** toolbar.</span></span>

1. <span data-ttu-id="3c49c-137">No **modo de exibição** menu, escolha **barras de ferramentas** e, em seguida, selecione **publicação Web com um clique**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-137">In the **View** menu, choose **Toolbars** and then select **Web One Click Publish**.</span></span>

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. <span data-ttu-id="3c49c-139">Na **Gerenciador de soluções**, selecione o projeto ContosoUniversity.</span><span class="sxs-lookup"><span data-stu-id="3c49c-139">In **Solution Explorer**, select the ContosoUniversity project.</span></span>
3. <span data-ttu-id="3c49c-140">o **publicação Web com um clique em** barra de ferramentas, escolha o **teste** perfil de publicação e, em seguida, clique em **publicar na Web** (o ícone com setas apontando para a esquerda e direita).</span><span class="sxs-lookup"><span data-stu-id="3c49c-140">the **Web One Click Publish** toolbar, choose the **Test** publish profile and then click **Publish Web** (the icon with arrows pointing left and right).</span></span>

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. <span data-ttu-id="3c49c-142">O Visual Studio implanta o aplicativo atualizado e o navegador é aberto automaticamente para a home page.</span><span class="sxs-lookup"><span data-stu-id="3c49c-142">Visual Studio deploys the updated application, and the browser automatically opens to the home page.</span></span>
5. <span data-ttu-id="3c49c-143">Execute a página instrutores e selecione um instrutor para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="3c49c-143">Run the Instructors page and select an instructor to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="3c49c-144">Você faria normalmente também fazer testes de regressão (ou seja, teste o restante do site para certificar-se de que a nova alteração não quebra nenhuma funcionalidade existente).</span><span class="sxs-lookup"><span data-stu-id="3c49c-144">You would normally also do regression testing (that is, test the rest of the site to make sure that the new change didn't break any existing functionality).</span></span> <span data-ttu-id="3c49c-145">Mas para este tutorial, você vai ignorar essa etapa e continuar para implantar a atualização para o preparo e produção.</span><span class="sxs-lookup"><span data-stu-id="3c49c-145">But for this tutorial you'll skip that step and proceed to deploy the update to staging and production.</span></span>

<span data-ttu-id="3c49c-146">Quando você reimplanta, implantação da Web determina automaticamente quais arquivos foram alterados e apenas copia os arquivos alterados para o servidor.</span><span class="sxs-lookup"><span data-stu-id="3c49c-146">When you redeploy, Web Deploy automatically determines which files have changed and only copies changed files to the server.</span></span> <span data-ttu-id="3c49c-147">Por padrão, implantação da Web usa datas alteradas por último em arquivos para determinar quais delas foram alterados.</span><span class="sxs-lookup"><span data-stu-id="3c49c-147">By default, Web Deploy uses last-changed dates on files to determine which ones have changed.</span></span> <span data-ttu-id="3c49c-148">Alguns sistemas de controle do código-fonte alterar arquivo datas, mesma quando você não altere o conteúdo do arquivo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-148">Some source control systems change file dates even when you don't change the file contents.</span></span> <span data-ttu-id="3c49c-149">Nesse caso, você talvez queira configurar a implantação da Web para usar as somas de verificação para determinar quais arquivos foram alterados.</span><span class="sxs-lookup"><span data-stu-id="3c49c-149">In that case, you might want to configure Web Deploy to use file checksums to determine which files have changed.</span></span> <span data-ttu-id="3c49c-150">Para obter mais informações, consulte [por que todos os meus arquivos reimplantados embora eu não alterá-los?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) nas perguntas Frequentes de implantação de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="3c49c-150">For more information, see [Why do all of my files get redeployed although I didn't change them?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) in the ASP.NET Deployment FAQ.</span></span>

## <a name="take-the-application-offline-during-deployment"></a><span data-ttu-id="3c49c-151">Colocar o aplicativo offline durante a implantação</span><span class="sxs-lookup"><span data-stu-id="3c49c-151">Take the application offline during deployment</span></span>

<span data-ttu-id="3c49c-152">Agora, a alteração que você está implantando é uma alteração simple em uma única página.</span><span class="sxs-lookup"><span data-stu-id="3c49c-152">The change you're deploying now is a simple change to a single page.</span></span> <span data-ttu-id="3c49c-153">Mas, às vezes, você pode implantar alterações maiores, ou você implantar alterações de código e o banco de dados e o site pode se comportar incorretamente se um usuário solicita uma página antes da implantação for concluída.</span><span class="sxs-lookup"><span data-stu-id="3c49c-153">But sometimes you deploy larger changes, or you deploy both code and database changes, and the site might behave incorrectly if a user requests a page before deployment is finished.</span></span> <span data-ttu-id="3c49c-154">Para impedir que os usuários acessem o site durante a implantação está em andamento, você pode usar um *app\_offline.htm* arquivo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-154">To prevent users from accessing the site while deployment is in progress, you can use an *app\_offline.htm* file.</span></span> <span data-ttu-id="3c49c-155">Quando você colocar um arquivo chamado *app\_offline.htm* na pasta raiz do seu aplicativo, o IIS exibe automaticamente esse arquivo em vez de executar seu aplicativo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-155">When you put a file named *app\_offline.htm* in the root folder of your application, IIS automatically displays that file instead of running your application.</span></span> <span data-ttu-id="3c49c-156">Então, para impedir o acesso durante a implantação, coloque *app\_offline.htm* na pasta raiz, execute o processo de implantação e, em seguida, remova *app\_offline.htm* depois bem-sucedida implantação.</span><span class="sxs-lookup"><span data-stu-id="3c49c-156">So to prevent access during deployment, you put *app\_offline.htm* in the root folder, run the deployment process, and then remove *app\_offline.htm* after successful deployment.</span></span>

<span data-ttu-id="3c49c-157">Você pode configurar a implantação da Web para colocar automaticamente um padrão *app\_offline.htm* de arquivos no servidor quando ele começar a implantar e removê-lo quando ela for concluída.</span><span class="sxs-lookup"><span data-stu-id="3c49c-157">You can configure Web Deploy to automatically put a default *app\_offline.htm* file on the server when it starts deploying and remove it when it finishes.</span></span> <span data-ttu-id="3c49c-158">Para fazer tudo o que você precisa fazer é adicionar o seguinte elemento XML ao seu arquivo de perfil (. pubxml) de publicação:</span><span class="sxs-lookup"><span data-stu-id="3c49c-158">To do that all you have to do is add the following XML element to your publish profile (.pubxml) file:</span></span>

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

<span data-ttu-id="3c49c-159">Para este tutorial, você verá como criar e usar um personalizado *app\_offline.htm* arquivo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-159">For this tutorial you'll see how to create and use a custom *app\_offline.htm* file.</span></span>

<span data-ttu-id="3c49c-160">Usando o *app\_offline.htm* no site de preparo não é necessária, porque você não tem usuários que acessam o site de preparo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-160">Using *app\_offline.htm* in the staging site isn't required, because you don't have users accessing the staging site.</span></span> <span data-ttu-id="3c49c-161">Mas é uma boa prática usar preparo para testar tudo o que a maneira como você planeja implantar na produção.</span><span class="sxs-lookup"><span data-stu-id="3c49c-161">But it's a good practice to use staging to test everything the way you plan to deploy in production.</span></span>

### <a name="create-appofflinehtm"></a><span data-ttu-id="3c49c-162">Criar aplicativo\_offline.htm</span><span class="sxs-lookup"><span data-stu-id="3c49c-162">Create app\_offline.htm</span></span>

1. <span data-ttu-id="3c49c-163">Na **Gerenciador de soluções**, a solução com o botão direito e clique em **Add**e, em seguida, clique em **Novo Item**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-163">In **Solution Explorer**, right-click the solution and click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="3c49c-164">Criar uma **página HTML** denominado *app\_offline.htm* (excluir o último "l" no *. HTML* extensão Visual Studio cria por padrão).</span><span class="sxs-lookup"><span data-stu-id="3c49c-164">Create an **HTML Page** named *app\_offline.htm* (delete the final "l" in the *.html* extension that Visual Studio creates by default).</span></span>
3. <span data-ttu-id="3c49c-165">Substitua a marcação de modelo com a seguinte marcação:</span><span class="sxs-lookup"><span data-stu-id="3c49c-165">Replace the template markup with the following markup:</span></span>

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. <span data-ttu-id="3c49c-166">Salve e feche o arquivo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-166">Save and close the file.</span></span>

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a><span data-ttu-id="3c49c-167">Aplicativo de cópia\_offline.htm para a pasta raiz do site da web</span><span class="sxs-lookup"><span data-stu-id="3c49c-167">Copy app\_offline.htm to the root folder of the web site</span></span>

<span data-ttu-id="3c49c-168">Você pode usar qualquer ferramenta FTP para copiar arquivos para o site da web.</span><span class="sxs-lookup"><span data-stu-id="3c49c-168">You can use any FTP tool to copy files to the web site.</span></span> <span data-ttu-id="3c49c-169">[O FileZilla](http://filezilla-project.org/) é uma ferramenta popular de FTP e é mostrada nas capturas de tela.</span><span class="sxs-lookup"><span data-stu-id="3c49c-169">[FileZilla](http://filezilla-project.org/) is a popular FTP tool and is shown in the screen shots.</span></span>

<span data-ttu-id="3c49c-170">Para usar uma ferramenta FTP, você precisa de três itens: a URL de FTP, o nome de usuário e a senha.</span><span class="sxs-lookup"><span data-stu-id="3c49c-170">To use an FTP tool, you need three things: the FTP URL, the user name, and the password.</span></span>

<span data-ttu-id="3c49c-171">A URL é mostrada na página do painel do site da web no Portal de gerenciamento do Azure, e o nome de usuário e senha para FTP podem ser encontradas na *. publishsettings* arquivo que você baixou anteriormente.</span><span class="sxs-lookup"><span data-stu-id="3c49c-171">The URL is shown on the web site's dashboard page in the Azure Management Portal, and the user name and password for FTP can be found in the *.publishsettings* file that you downloaded earlier.</span></span> <span data-ttu-id="3c49c-172">As etapas a seguir mostram como obter esses valores.</span><span class="sxs-lookup"><span data-stu-id="3c49c-172">The following steps show how to get these values.</span></span>

1. <span data-ttu-id="3c49c-173">No Portal de gerenciamento do Azure, clique em **Sites da Web** guia e, em seguida, clique em site da web de preparo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-173">In the Azure Management Portal, click **Web Sites** tab and then click the staging web site.</span></span>
2. <span data-ttu-id="3c49c-174">Sobre o **Dashboard** página, role para baixo até encontrar o nome de host de FTP na **visão rápida** seção.</span><span class="sxs-lookup"><span data-stu-id="3c49c-174">On the **Dashboard** page, scroll down to find the FTP host name in the **Quick Glance** section.</span></span>

    ![Nome do host FTP](deploying-a-code-update/_static/image5.png)
3. <span data-ttu-id="3c49c-176">Abra o preparo *. publishsettings* arquivo no bloco de notas ou outro editor de texto.</span><span class="sxs-lookup"><span data-stu-id="3c49c-176">Open the staging *.publishsettings* file in Notepad or another text editor.</span></span>
4. <span data-ttu-id="3c49c-177">Encontre o `publishProfile` elemento para o perfil FTP.</span><span class="sxs-lookup"><span data-stu-id="3c49c-177">Find the `publishProfile` element for the FTP profile.</span></span>
5. <span data-ttu-id="3c49c-178">Cópia de `userName` e `userPWD` valores.</span><span class="sxs-lookup"><span data-stu-id="3c49c-178">Copy the `userName` and `userPWD` values.</span></span>

    ![Credenciais de FTP](deploying-a-code-update/_static/image6.png)
6. <span data-ttu-id="3c49c-180">Abra sua ferramenta FTP e faça logon no URL FTP.</span><span class="sxs-lookup"><span data-stu-id="3c49c-180">Open your FTP tool and log on to the FTP URL.</span></span>
7. <span data-ttu-id="3c49c-181">Cópia *app\_offline.htm* da pasta de solução para o */site/wwwroot* pasta no site de preparo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-181">Copy *app\_offline.htm* from the solution folder to the */site/wwwroot* folder in the staging site.</span></span>

    ![Copiar app_offline](deploying-a-code-update/_static/image7.png)
8. <span data-ttu-id="3c49c-183">Navegue até a URL do seu site de preparo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-183">Browse to your staging site's URL.</span></span> <span data-ttu-id="3c49c-184">Você verá que o *app\_offline.htm* página agora é exibida em vez de sua home page.</span><span class="sxs-lookup"><span data-stu-id="3c49c-184">You see that the *app\_offline.htm* page is now displayed instead of your home page.</span></span>

    ![App_offline na janela do navegador](deploying-a-code-update/_static/image8.png)

<span data-ttu-id="3c49c-186">Agora você está pronto para implantar no preparo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-186">You are now ready to deploy to staging.</span></span>

## <a name="deploy-the-code-update-to-staging-and-production"></a><span data-ttu-id="3c49c-187">Implantar a atualização de código em produção e preparo</span><span class="sxs-lookup"><span data-stu-id="3c49c-187">Deploy the code update to staging and production</span></span>

1. <span data-ttu-id="3c49c-188">No **publicação Web com um clique em** barra de ferramentas, escolha o **preparo** perfil de publicação e, em seguida, clique em **publicar na Web**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-188">In the **Web One Click Publish** toolbar, choose the **Staging** publish profile and then click **Publish Web**.</span></span>

    <span data-ttu-id="3c49c-189">Visual Studio implanta o aplicativo atualizado e abre o navegador para a home page do site.</span><span class="sxs-lookup"><span data-stu-id="3c49c-189">Visual Studio deploys the updated application and opens the browser to the site's home page.</span></span> <span data-ttu-id="3c49c-190">O *app\_offline.htm* arquivo é exibido.</span><span class="sxs-lookup"><span data-stu-id="3c49c-190">The *app\_offline.htm* file is displayed.</span></span> <span data-ttu-id="3c49c-191">Antes de poder testar para verificar a implantação bem-sucedida, você deve remover o *app\_offline.htm* arquivo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-191">Before you can test to verify successful deployment, you must remove the *app\_offline.htm* file.</span></span>
2. <span data-ttu-id="3c49c-192">Retornar à sua ferramenta FTP e exclua **app\_offline.htm** do site de preparo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-192">Return to your FTP tool, and delete **app\_offline.htm** from the staging site.</span></span>
3. <span data-ttu-id="3c49c-193">No navegador, abra a página instrutores no local de preparo e selecione um instrutor para verificar se a atualização foi implantada com êxito.</span><span class="sxs-lookup"><span data-stu-id="3c49c-193">In the browser, open the Instructors page in the staging site, and select an instructor to verify that the update was successfully deployed.</span></span>
4. <span data-ttu-id="3c49c-194">Siga o mesmo procedimento para produção, como você fez preparo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-194">Follow the same procedure for production as you did for staging.</span></span>

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a><span data-ttu-id="3c49c-195">Revisar as alterações e implantar arquivos específicos</span><span class="sxs-lookup"><span data-stu-id="3c49c-195">Reviewing Changes and Deploying Specific Files</span></span>

<span data-ttu-id="3c49c-196">Visual Studio 2012 também oferece a capacidade de implantar os arquivos individuais.</span><span class="sxs-lookup"><span data-stu-id="3c49c-196">Visual Studio 2012 also gives you the ability to deploy individual files.</span></span> <span data-ttu-id="3c49c-197">Para um arquivo selecionado você pode exibir as diferenças entre a versão local e a versão implantada, implantar o arquivo para o ambiente de destino ou copie o arquivo do ambiente de destino para o projeto local.</span><span class="sxs-lookup"><span data-stu-id="3c49c-197">For a selected file you can view differences between the local version and the deployed version, deploy the file to the destination environment, or copy the file from the destination environment to the local project.</span></span> <span data-ttu-id="3c49c-198">Nesta seção do tutorial, você verá como usar esses recursos.</span><span class="sxs-lookup"><span data-stu-id="3c49c-198">In this section of the tutorial you see how to use these features.</span></span>

### <a name="make-a-change-to-deploy"></a><span data-ttu-id="3c49c-199">Faça uma alteração para implantar</span><span class="sxs-lookup"><span data-stu-id="3c49c-199">Make a change to deploy</span></span>

1. <span data-ttu-id="3c49c-200">Abra *Content/Site.css*e localize o bloco para o `body` marca.</span><span class="sxs-lookup"><span data-stu-id="3c49c-200">Open *Content/Site.css*, and find the block for the `body` tag.</span></span>
2. <span data-ttu-id="3c49c-201">Altere o valor de `background-color` partir `#fff` para `darkblue`.</span><span class="sxs-lookup"><span data-stu-id="3c49c-201">Change the value for `background-color` from `#fff` to `darkblue`.</span></span>

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a><span data-ttu-id="3c49c-202">Exiba a alteração na janela de visualização de publicação</span><span class="sxs-lookup"><span data-stu-id="3c49c-202">View the change in the Publish Preview window</span></span>

<span data-ttu-id="3c49c-203">Quando você usa o **Publicar Web** Assistente para publicar o projeto, você pode ver quais alterações serão publicadas por duas vezes no arquivo na **visualização** janela.</span><span class="sxs-lookup"><span data-stu-id="3c49c-203">When you use the **Publish Web** wizard to publish the project, you can see what changes are going to be published by double-clicking the file in the **Preview** window.</span></span>

1. <span data-ttu-id="3c49c-204">Clique com botão direito no projeto ContosoUniversity e clique em **publicar**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-204">Right-click the ContosoUniversity project and click **Publish**.</span></span>
2. <span data-ttu-id="3c49c-205">Dos **perfil** lista suspensa, selecione o **teste** perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="3c49c-205">From the **Profile** drop-down list, select the **Test** publish profile.</span></span>
3. <span data-ttu-id="3c49c-206">Clique em **versão prévia**e, em seguida, clique em **iniciar visualização**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-206">Click **Preview**, and then click **Start Preview**.</span></span>
4. <span data-ttu-id="3c49c-207">No **versão prévia** painel, clique duas vezes em **CSS**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-207">In the **Preview** pane, double-click **Site.css**.</span></span>

    ![Clique duas vezes em CSS](deploying-a-code-update/_static/image9.png)

    <span data-ttu-id="3c49c-209">O **visualizar alterações** caixa de diálogo mostra uma visualização das alterações que serão implantados.</span><span class="sxs-lookup"><span data-stu-id="3c49c-209">The **Preview changes** dialog shows a preview of the changes that will be deployed.</span></span>

    ![Visualizar alterações de CSS](deploying-a-code-update/_static/image10.png)

    <span data-ttu-id="3c49c-211">Se você clicar duas vezes o *Web. config* arquivo, o **visualizar alterações** caixa de diálogo mostra o efeito de sua compilação transformações de configuração e transformações de perfil de publicação.</span><span class="sxs-lookup"><span data-stu-id="3c49c-211">If you double-click the *Web.config* file, the **Preview changes** dialog shows the effect of your build configuration transformations and publish profile transformations.</span></span> <span data-ttu-id="3c49c-212">Neste momento você não tiver feito tudo o que faria com que o *Web. config* arquivo no servidor para mudar, portanto, você espera não ver nenhuma alteração.</span><span class="sxs-lookup"><span data-stu-id="3c49c-212">At this point you have not done anything that would cause the *Web.config* file on the server to change, so you expect to see no changes.</span></span> <span data-ttu-id="3c49c-213">No entanto, o **visualizar alterações** janela mostra duas alterações incorretamente.</span><span class="sxs-lookup"><span data-stu-id="3c49c-213">However, the **Preview changes** window incorrectly shows two changes.</span></span> <span data-ttu-id="3c49c-214">Parece que os dois elementos XML serão removidos.</span><span class="sxs-lookup"><span data-stu-id="3c49c-214">It looks like two XML elements will be removed.</span></span> <span data-ttu-id="3c49c-215">Esses elementos são adicionados pelo processo de publicação quando você seleciona **executar migrações do Code First na inicialização do aplicativo** para uma classe de contexto de Code First.</span><span class="sxs-lookup"><span data-stu-id="3c49c-215">These elements are added by the publish process when you select **Execute Code First Migrations on application start** for a Code First context class.</span></span> <span data-ttu-id="3c49c-216">A comparação é feita antes que o processo de publicação adiciona esses elementos, portanto, parece que estão sendo removidos embora eles não serão removidos.</span><span class="sxs-lookup"><span data-stu-id="3c49c-216">The comparison is done before the publish process adds those elements, so it looks like they are being removed although they will not be removed.</span></span> <span data-ttu-id="3c49c-217">Esse erro será corrigido em uma versão futura.</span><span class="sxs-lookup"><span data-stu-id="3c49c-217">This error will be corrected in a future release.</span></span>
5. <span data-ttu-id="3c49c-218">Clique em **Fechar**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-218">Click **Close**.</span></span>
6. <span data-ttu-id="3c49c-219">Clique em **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-219">Click **Publish**.</span></span>
7. <span data-ttu-id="3c49c-220">Quando o navegador é aberto para a home page do site de teste, pressione CTRL + F5 para fazer com que uma atualização de disco rígida para ver o efeito da alteração do CSS.</span><span class="sxs-lookup"><span data-stu-id="3c49c-220">When the browser opens to the home page of the Test site, press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Efeito da alteração CSS](deploying-a-code-update/_static/image11.png)
8. <span data-ttu-id="3c49c-222">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="3c49c-222">Close the browser.</span></span>

### <a name="publish-specific-files-from-solution-explorer"></a><span data-ttu-id="3c49c-223">Publicar arquivos específicos do Gerenciador de soluções</span><span class="sxs-lookup"><span data-stu-id="3c49c-223">Publish specific files from Solution Explorer</span></span>

<span data-ttu-id="3c49c-224">Suponha que você não o plano de fundo azul e quiser reverter para a cor original.</span><span class="sxs-lookup"><span data-stu-id="3c49c-224">Suppose you don't like the blue background and want to revert to the original color.</span></span> <span data-ttu-id="3c49c-225">Nesta seção você irá restaurar as configurações originais ao publicar um arquivo específico diretamente de **Gerenciador de soluções**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-225">In this section you'll restore the original settings by publishing a specific file directly from **Solution Explorer**.</span></span>

1. <span data-ttu-id="3c49c-226">Abra *Content/Site.css* e restaurar o `background-color` definir como `#fff`.</span><span class="sxs-lookup"><span data-stu-id="3c49c-226">Open *Content/Site.css* and restore the `background-color` setting to `#fff`.</span></span>
2. <span data-ttu-id="3c49c-227">Na **Gerenciador de soluções**, clique com botão direito do *Content/Site.css* arquivo.</span><span class="sxs-lookup"><span data-stu-id="3c49c-227">In **Solution Explorer**, right-click the *Content/Site.css* file.</span></span>

    <span data-ttu-id="3c49c-228">O menu de contexto mostra que três opções de publicação.</span><span class="sxs-lookup"><span data-stu-id="3c49c-228">The context menu shows three publish options.</span></span>

    ![Publicar opções no Gerenciador de soluções](deploying-a-code-update/_static/image12.png)
3. <span data-ttu-id="3c49c-230">Clique em **visualizar alterações feitas em CSS**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-230">Click **Preview changes to Site.css**.</span></span>

    <span data-ttu-id="3c49c-231">Uma janela é aberta para mostrar as diferenças entre o arquivo local e a versão no ambiente de destino.</span><span class="sxs-lookup"><span data-stu-id="3c49c-231">A window opens to show the differences between the local file and the version of it in the destination environment.</span></span>

    ![Diff-conteúdo/CSS](deploying-a-code-update/_static/image13.png)
4. <span data-ttu-id="3c49c-233">Na **Gerenciador de soluções**, clique com botão direito **CSS** novamente e clique em **publicar CSS**.</span><span class="sxs-lookup"><span data-stu-id="3c49c-233">In **Solution Explorer**, right-click **Site.css** again and click **Publish Site.css**.</span></span>

    <span data-ttu-id="3c49c-234">O **atividade de publicação de Web** janela mostra que o arquivo foi publicado.</span><span class="sxs-lookup"><span data-stu-id="3c49c-234">The **Web Publish Activity** window shows that the file has been published.</span></span>

    ![Janela de atividade de publicação da Web](deploying-a-code-update/_static/image14.png)
5. <span data-ttu-id="3c49c-236">Abra um navegador para o `http://localhost/contosouniversity` URL e, em seguida, pressione CTRL + F5 para fazer com que um disco rígido atualizar para ver o efeito de CSS alterar.</span><span class="sxs-lookup"><span data-stu-id="3c49c-236">Open a browser to the `http://localhost/contosouniversity` URL, and then press CTRL+F5 to cause a hard refresh in order to see the effect of the CSS change.</span></span>

    ![Home page com CSS normal](deploying-a-code-update/_static/image15.png)
6. <span data-ttu-id="3c49c-238">Feche o navegador.</span><span class="sxs-lookup"><span data-stu-id="3c49c-238">Close the browser.</span></span>

## <a name="summary"></a><span data-ttu-id="3c49c-239">Resumo</span><span class="sxs-lookup"><span data-stu-id="3c49c-239">Summary</span></span>

<span data-ttu-id="3c49c-240">Agora você já viu várias maneiras de implantar uma atualização de aplicativo que não envolve uma alteração de banco de dados, e você já viu como visualizar as alterações para verificar se o que será atualizado é o que você espera.</span><span class="sxs-lookup"><span data-stu-id="3c49c-240">You've now seen several ways to deploy an application update that does not involve a database change, and you've seen how to preview the changes to verify that what will be updated is what you expect.</span></span> <span data-ttu-id="3c49c-241">A página instrutores agora tem um **cursos ministrados** seção.</span><span class="sxs-lookup"><span data-stu-id="3c49c-241">The Instructors page now has a **Courses Taught** section.</span></span>

![Página instrutores com cursos ministrados](deploying-a-code-update/_static/image16.png)

<span data-ttu-id="3c49c-243">O próximo tutorial mostra como implantar uma alteração de banco de dados: você adicionará um campo de data de nascimento para o banco de dados e para a página instrutores.</span><span class="sxs-lookup"><span data-stu-id="3c49c-243">The next tutorial shows you how to deploy a database change: you'll add a birthdate field to the database and to the Instructors page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3c49c-244">[Anterior](deploying-to-production.md)
> [Próximo](deploying-a-database-update.md)</span><span class="sxs-lookup"><span data-stu-id="3c49c-244">[Previous](deploying-to-production.md)
[Next](deploying-a-database-update.md)</span></span>
