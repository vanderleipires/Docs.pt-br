---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização Code-Only - 8 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 39075d2e979076f88200ea595c92f95f38f35911
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374007"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização Code-Only - 8 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se você instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server que não seja o SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Após a implantação inicial, continua seu trabalho de manutenção e desenvolvimento de seu site da web e pouco tempo você deseja implantar uma atualização. Este tutorial o guiará durante o processo de implantação de uma atualização para o código do aplicativo. Esta atualização não envolve uma alteração de banco de dados; Você verá o que é diferente sobre como implantar uma alteração de banco de dados no próximo tutorial.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Fazer uma alteração de código

Como um exemplo simples de uma atualização para o seu aplicativo, você adicionará a **instrutores** página uma lista de cursos ministrados pelo instrutor selecionado.

Se você executar o **instrutores** página, você observará que há **selecione** links na grade, mas eles não fazem nada diferente de marca a linha cinza de turno em segundo plano.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Agora você adicionará código que é executada quando o **selecionar** link é clicado e exibe uma lista de cursos ministrados pelo instrutor selecionado.

Na *Instructors.aspx*, adicione a seguinte marcação imediatamente após o **ErrorMessageLabel** `Label` controle:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Execute a página e selecione um instrutor. Você ver uma lista de cursos ministrados por instrutor.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Implantar a atualização de código para o ambiente de teste

A implantação no ambiente de teste é uma simples questão de execução em um único clique publique novamente. Para tornar esse processo mais rápido, você pode usar o **publicação Web com um clique** barra de ferramentas.

No **modo de exibição** menu, escolha **barras de ferramentas** e, em seguida, selecione **publicação Web com um clique**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

Na **Gerenciador de soluções**, selecione o projeto ContosoUniversity.

o **publicação Web com um clique em** barra de ferramentas, escolha o **teste** perfil de publicação e, em seguida, clique em **publicar na Web** (o ícone com setas apontando para a esquerda e direita).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

O Visual Studio implanta o aplicativo atualizado e o navegador é aberto automaticamente para a home page. Execute a página instrutores e selecione um instrutor para verificar se a atualização foi implantada com êxito.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Você faria normalmente também fazer testes de regressão (ou seja, teste o restante do site para certificar-se de que a nova alteração não quebra nenhuma funcionalidade existente). Mas para este tutorial, você vai ignorar essa etapa e continuar para implantar a atualização para a produção.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Impedindo a reimplantação do estado inicial do banco de dados para a produção

Em um aplicativo real, os usuários interagem com seu site de produção após a implantação inicial e os bancos de dados são preenchidos com dados dinâmicos. Portanto, você não deseja reimplantar o banco de dados de associação em seu estado inicial, o que seria apagar todos os dados em tempo real. Já que os bancos de dados do SQL Server Compact são arquivos no *App\_dados* pasta, você precisa evitar isso, alterando as configurações de implantação para que arquivos no *App\_dados* pasta não são implantados.

Abra o **propriedades do projeto** janela para o projeto ContosoUniversity e selecione o **pacote/Publicar Web** guia. Certifique-se de que o **Configuration** caixa suspensa foi **Active Directory (versão)** ou **versão** selecionado, selecione **excluir arquivos do aplicativo\_Pasta de dados**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

No caso de você optar por implantar uma compilação de depuração no futuro, é uma boa ideia fazer a mesma alteração para a configuração de build de depuração: altere **Configuration** para **Debug** e, em seguida, selecione **excluir arquivos do aplicativo\_pasta de dados**.

Salve e feche o **empacotar/Publicar Web** guia.

> [!NOTE] 
> 
> [!IMPORTANT]
> Certifique-se de que você não tem **remover arquivos adicionais no destino** selecionado em seus perfis de publicação. Se você selecionar essa opção, o processo de implantação excluirá os bancos de dados que você tem no aplicativo\_dados no site implantado e ele excluirá o aplicativo\_própria pasta de dados.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Impedindo o acesso de usuário ao Site de produção durante a atualização

Agora, a alteração que você está implantando é uma alteração simple em uma única página. Mas, às vezes, você pode implantar alterações maiores e, nesse caso, o site pode se comportar de forma estranha se um usuário solicita uma página antes da implantação for concluída. Para evitar isso, você pode usar um *app\_offline.htm* arquivo. Quando você colocar um arquivo chamado *app\_offline.htm* na pasta raiz do seu aplicativo, o IIS exibe automaticamente esse arquivo em vez de executar seu aplicativo. Então, para impedir o acesso durante a implantação, coloque *app\_offline.htm* na pasta raiz, execute o processo de implantação e, em seguida, remova *app\_offline.htm*.

Na **Gerenciador de soluções**, a solução (não um dos projetos) com o botão direito e selecione **nova pasta de solução**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

Nomeie a pasta *SolutionFiles*.

Na nova pasta, crie uma página HTML chamada *app\_offline.htm*. Substitua o conteúdo existente com a seguinte marcação:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Você pode copiar o *app\_offline.htm* arquivo para o site usando uma conexão de FTP ou o **Gerenciador de arquivos** utilitário no painel de controle do provedor de hospedagem. Para este tutorial, você usará o **Gerenciador de arquivos**.

Abra o painel de controle e selecione **Gerenciador de arquivos** como fez na [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial. Selecione **contosouniversity.com** e, em seguida **wwwroot** para obter a pasta raiz do seu aplicativo e, em seguida, clique em **carregar**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

No **carregar arquivo** caixa de diálogo, selecione o *app\_offline.htm* de arquivo e, em seguida, clique em **carregar**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Navegue até a URL do site. Você verá que o *app\_offline.htm* página agora é exibida em vez de sua home page.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Agora você está pronto para implantar em produção.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Implantar a atualização de código para o ambiente de produção

No **publicação Web com um clique em** barra de ferramentas, escolha o **produção** perfil de publicação e, em seguida, clique em **publicar na Web**.

Visual Studio implanta o aplicativo atualizado e abre o navegador para a home page do site. O *app\_offline.htm* arquivo é exibido. Antes de poder testar para verificar a implantação bem-sucedida, você deve remover o *app\_offline.htm* arquivo.

Volte para o **Gerenciador de arquivos** aplicativo no painel de controle. Selecione **contosouniversity.com** e **wwwroot**, selecione **aplicativo\_offline.htm**e, em seguida, clique em **excluir**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

No navegador, abra a página instrutores no site público e selecione um instrutor para verificar se a atualização foi implantada com êxito.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Agora, você implantou uma atualização de aplicativo que envolve uma alteração de banco de dados. O próximo tutorial mostra como implantar uma alteração de banco de dados.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
