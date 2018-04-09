---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização Code-Only - 8 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: ddf6252f-9413-4c0c-a360-2cef8d231717
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12
msc.type: authoredcontent
ms.openlocfilehash: 6305a8cb87d9b00b6bb4c4f8fa6114bddc4eb89f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-code-only-update---8-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Implantando uma atualização Code-Only - 8 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010. Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão Geral

Após a implantação inicial, o trabalho de manutenção e desenvolvimento de seu site continua e pouco tempo você deseja implantar uma atualização. Este tutorial leva você através do processo de implantação de uma atualização para o código do aplicativo. Esta atualização não envolve uma alteração de banco de dados; Você verá qual é a diferença sobre a implantação de uma alteração de banco de dados do tutorial Avançar.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="making-a-code-change"></a>Fazer uma alteração de código

Como um exemplo simples de uma atualização para o seu aplicativo, você adicionará o **instrutores** página uma lista de cursos ministrada pelo instrutor selecionado.

Se você executar o **instrutores** página, você observará que há **selecione** links na grade, mas eles não fazem nada diferente de tornar o cinza de ativar linha em segundo plano.

[![Instructors_page](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image1.png)

Agora você adicionará código que é executado quando o **selecione** link é clicado e exibe uma lista de cursos ministrada pelo instrutor selecionado.

Em *Instructors.aspx*, adicione a seguinte marcação imediatamente após o **ErrorMessageLabel** `Label` controle:

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample1.aspx)]

Execute a página e selecione um instrutor. Você ver uma lista de cursos ministrada por esse instrutor.

[![Instructors_page_with_courses](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image3.png)

## <a name="deploying-the-code-update-to-the-test-environment"></a>Implantar a atualização de código para o ambiente de teste

A implantação no ambiente de teste é uma simples questão de executar um clique publicar novamente. Para tornar esse processo mais rápido, você pode usar o **Web um clique em publicar** barra de ferramentas.

No **exibição** menu, escolha **barras de ferramentas** e, em seguida, selecione **Web um clique em publicar**.

![Selecting_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image5.png)

Em **Solution Explorer**, selecione o projeto ContosoUniversity.

o **Web um clique em publicar** barra de ferramentas, escolha o **teste** perfil de publicação e, em seguida, clique em **Publicar Web** (o ícone com setas que apontam para a esquerda e direita).

![Web_One_Click_Publish_toolbar](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image6.png)

O Visual Studio implanta o aplicativo atualizado e o navegador é aberto automaticamente para a home page. Execute a página de professores e selecione um instrutor para verificar se a atualização foi implantada com êxito.

[![Instructors_page_with_courses_Test](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image7.png)

Normalmente também fazer o teste de regressão (ou seja, teste o restante do site para certificar-se de que a nova alteração não violam nenhuma funcionalidade existente). Mas, para este tutorial, você vai ignorar essa etapa e continuar para implantar a atualização para a produção.

## <a name="preventing-redeployment-of-the-initial-database-state-to-production"></a>Impedindo a reimplantação do estado inicial do banco de dados de produção

Em um aplicativo real, os usuários interagem com o site de produção após a implantação inicial e os bancos de dados são preenchidos com dados dinâmicos. Portanto, você não deseja reimplantar o banco de dados de associação em seu estado inicial, o que seria apagar todos os dados ao vivo. Como bancos de dados do SQL Server Compact são arquivos no *aplicativo\_dados* pasta, você precisa impedir isso alterando as configurações de implantação para que arquivos de *aplicativo\_dados* pasta não são implantados.

Abra o **propriedades do projeto** janela para o projeto ContosoUniversity e selecione o **pacote/publicar na Web** guia. Verifique se o **configuração** caixa suspensa foi **ativo (versão)** ou **versão** selecionado, selecione **excluir arquivos do aplicativo\_Pasta dados**.

![Exclude_files_from_the_App_Data_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image9.png)

No caso de você optar por implantar uma compilação de depuração no futuro, é recomendável fazer a mesma alteração para a configuração de compilação de depuração: alterar **configuração** para **depurar** e, em seguida, selecione **excluir arquivos do aplicativo\_pasta dados**.

Salve e feche o **pacote/publicar na Web** guia.

> [!NOTE] 
> 
> [!IMPORTANT]
> Certifique-se de que você não tem **remover arquivos adicionais no destino** selecionado em seus perfis de publicação. Se você selecionar essa opção, o processo de implantação excluirá os bancos de dados que você tem em aplicativo\_dados no site implantado e excluirá o aplicativo\_própria pasta de dados.


## <a name="preventing-user-access-to-the-production-site-during-update"></a>Impedindo o acesso de usuário para o Site de produção durante a atualização

A alteração que você está implantando agora é uma alteração simple em uma única página. Mas, às vezes, você pode implantar alterações maiores e nesse caso o site pode se comportar estranho se um usuário solicita uma página antes da conclusão da implantação. Para evitar isso, você pode usar um *aplicativo\_offline.htm* arquivo. Quando você coloca um arquivo chamado *aplicativo\_offline.htm* na pasta raiz do seu aplicativo, o IIS exibe automaticamente esse arquivo em vez de executar seu aplicativo. Portanto, para impedir acesso durante a implantação, você deve colocar *aplicativo\_offline.htm* na pasta raiz, execute o processo de implantação e, em seguida, remover *aplicativo\_offline.htm*.

Em **Solution Explorer**, a solução (não um dos projetos) e selecione **nova pasta de solução**.

![Creating_a_solution_folder](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image10.png)

O nome da pasta *SolutionFiles*.

Na nova pasta, crie uma página HTML chamada *aplicativo\_offline.htm*. Substitua o conteúdo existente com a seguinte marcação:

[!code-html[Main](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/samples/sample2.html)]

Você pode copiar o *aplicativo\_offline.htm* arquivo para o site usando uma conexão de FTP ou o **Gerenciador de arquivos** utilitário no painel de controle do provedor de hospedagem. Para este tutorial, você usará o **Gerenciador de arquivos**.

Abra o painel de controle e selecione **Gerenciador de arquivos** como no [implantando no ambiente de produção](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial. Selecione **contosouniversity.com** e **wwwroot** para obter a pasta raiz do aplicativo e, em seguida, clique em **carregar**.

[![Upload_button_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image11.png)

No **carregar arquivo** caixa de diálogo, selecione o *aplicativo\_offline.htm* de arquivo e, em seguida, clique em **carregar**.

[![Upload_dialog_box_in_File_Manager](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image13.png)

Navegue até a URL do site. Você verá que o *aplicativo\_offline.htm* página agora é exibida em vez de sua home page.

[![App_offline.htm_page_in_production](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image15.png)

Agora você está pronto para implantar na produção.

## <a name="deploying-the-code-update-to-the-production-environment"></a>Implantar a atualização de código para o ambiente de produção

No **Web um clique em publicar** barra de ferramentas, escolha o **produção** perfil de publicação e, em seguida, clique em **Publicar Web**.

Visual Studio implanta o aplicativo atualizado e abre o navegador para a home page do site. O *aplicativo\_offline.htm* arquivo é exibido. Antes de você testar para verificar a implantação bem-sucedida, você deve remover o *aplicativo\_offline.htm* arquivo.

Volte para o **Gerenciador de arquivos** aplicativo no painel de controle. Selecione **contosouniversity.com** e **wwwroot**, selecione **aplicativo\_offline.htm**e, em seguida, clique em **excluir**.

[![Deleting_app_offline.htm](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image17.png)

No navegador, abra a página instrutores em site público e selecione um instrutor para verificar se a atualização foi implantada com êxito.

[![Instructors_page_with_courses_Prod](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12/_static/image19.png)

Agora que você implantou uma atualização do aplicativo que envolve uma alteração de banco de dados. O seguinte tutorial mostra como implementar uma alteração de banco de dados.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
