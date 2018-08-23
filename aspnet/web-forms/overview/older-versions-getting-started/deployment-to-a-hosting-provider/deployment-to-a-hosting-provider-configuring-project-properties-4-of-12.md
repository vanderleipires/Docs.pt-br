---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Configurando propriedades do projeto - 4 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: ecd169f70eee162a647f6ea827ba5649b6c22ffd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832452"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Configurando propriedades do projeto - 4 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se você instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server que não seja o SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Algumas opções de implantação são configuradas nas propriedades do projeto que são armazenadas no arquivo de projeto (o *. csproj* ou *. vbproj* arquivo). Na maioria dos casos, os valores padrão dessas configurações são o que você deseja, mas você pode usar o **propriedades do projeto** internas de interface do usuário no Visual Studio para trabalhar com essas configurações se você precisar alterá-los. Neste tutorial você examine as configurações de implantação no **propriedades do projeto**. Você também pode criar um arquivo de espaço reservado que faz com que uma pasta vazia ser implantado.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Definindo as configurações de implantação na janela de propriedades do projeto

A maioria das configurações que afetam o que acontece durante a implantação são incluídos no perfil de publicação, como você verá nos tutoriais a seguir. Algumas configurações que você deve estar ciente de que estão localizadas na **empacotar/publicar** guias da **propriedades do projeto** janela. Essas configurações são especificadas para cada configuração de compilação — ou seja, você pode ter configurações diferentes para um build de versão que você tem para um build de depuração.

Na **Gerenciador de soluções**, com o botão direito do **ContosoUniversity** projeto, selecione **propriedades**e, em seguida, selecione o **pacote/Publicar Web**guia.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Quando a janela for exibida, o padrão é mostrando as configurações para qualquer configuração de build está ativa no momento para a solução. Se o **Configuration** caixa indica **Active Directory (versão)**, selecione **versão** para exibir as configurações para a configuração de build de versão. Você implantará builds de versão para ambientes de seu teste e produção.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Com o **Active Directory (versão)** ou **versão** selecionada, você verá os valores que estão em vigor quando você implanta usando a configuração de build de versão:

- No **itens para implantar** caixa de **somente os arquivos necessários para executar o aplicativo** está selecionado. Outras opções são **todos os arquivos neste projeto** ou **todos os arquivos nesta pasta de projeto**. Ao deixar a seleção padrão inalterada você evita a implantação de arquivos de código-fonte, por exemplo. Essa configuração é o motivo por que as pastas que contêm os arquivos binários do SQL Server Compact precisava ser incluído no projeto. Para obter mais informações sobre essa configuração, consulte **por que não todos os arquivos na pasta Meu projeto são implantados?** na [perguntas frequentes sobre o ASP.NET Web Application Project implantação](https://msdn.microsoft.com/library/ee942158.aspx).
- **Símbolos de depuração de exclusão gerado** está selecionado. Você não depuração quando você usa essa configuração de compilação.
- **Excluir arquivos do aplicativo\_pasta de dados** não estiver selecionada. O arquivo SQL Server Compact para o banco de dados de associação é nessa pasta e você precisará implantá-lo. Quando você implanta atualizações que não incluem alterações de banco de dados, você selecionará essa caixa de seleção.
- **Pré-compilar este aplicativo antes da publicação** não estiver selecionada. Na maioria dos cenários, não é necessário para pré-compilar a projetos de aplicativos web. Para obter mais informações sobre essa opção, consulte [guia do pacote/Publicar Web, as propriedades do projeto](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) e [Advanced pré-compilar caixa de diálogo Configurações](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Incluir todos os bancos de dados configurados na guia empacotar/publicar SQL** estiver selecionada, mas essa opção não tem nenhum efeito agora porque você não estiver configurando o **empacotar/publicar SQL** guia. Essa guia é para um método de implantação de banco de dados herdadas que costumava ser a única opção para implantar bancos de dados do SQL Server. Você usará o **empacotar/publicar SQL** guia o [migrando para o SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) tutorial.
- O **configurações de pacote de implantação da Web** seção não se aplica porque você está usando um único clique publicar nestes tutoriais.

Alterar o **configuração** caixa suspensa para a depuração para ver as configurações padrão para compilações de depuração. Os valores são os mesmos, exceto **Exclude gerado símbolos de depuração** está desmarcada para que você possa depurar quando você implanta uma compilação de depuração.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Tornando a certeza de que a pasta do Elmah obtém implantada

Como você viu no tutorial anterior, o [pacote do NuGet do Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornece a funcionalidade para o log de erros e emissão de relatórios. O aplicativo Contoso University Elmah foi configurado para armazenar detalhes do erro em uma pasta chamada *Elmah*:

![Pasta do ELMAH](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Excluindo arquivos ou pastas específicas de implantação é um requisito comum; outro exemplo seria uma pasta que os usuários podem carregar arquivos. Você não deseja que os arquivos de log ou carregado de arquivos que foram criados em seu ambiente de desenvolvimento para ser implantado na produção. E se você estiver implantando uma atualização para a produção, que você não deseja que o processo de implantação para excluir arquivos que existem em produção. (Dependendo de como você define uma opção de implantação, se existir um arquivo no site de destino, mas não o site de origem quando você implanta, implantação da Web exclui-lo do destino.)

Como você viu neste tutorial, o **itens para implantar** opção a **pacote/Publicar Web** for definido como **apenas os arquivos necessários para executar este aplicativo**. Como resultado, os arquivos de log são criados pelo Elmah no desenvolvimento de não serão implantados, que é o que você quer fazer. (A serem implantados, eles precisam ser incluídos no projeto e seus **ação de compilação** propriedade precisa ser definido como **conteúdo**. Para obter mais informações, consulte **por que não todos os arquivos na pasta Meu projeto são implantados?** na [perguntas frequentes sobre o ASP.NET Web Application Project implantação](https://msdn.microsoft.com/library/ee942158.aspx)). No entanto, implantação da Web não criará uma pasta no site de destino a menos que haja pelo menos um arquivo a ser copiado para ele. Portanto, você adicionará uma *. txt* arquivo para a pasta para atuar como um espaço reservado para que a pasta será copiada.

No **Gerenciador de soluções**, clique com botão direito do *Elmah* pasta, selecione **Add New Item**e crie um arquivo chamado *Espaço_reservado*. Coloque o seguinte texto nele: "Este é um arquivo de espaço reservado para garantir que a pasta é implantada." e salve o arquivo. Isso é tudo o que você precisa fazer para certificar-se de que o Visual Studio implanta esse arquivo e a pasta em que ele está, pois o **ação de compilação** propriedade de *. txt* arquivos é definido como **conteúdo**por padrão.

Agora você concluiu todas as tarefas de configuração de implantação. No próximo tutorial, você vai implantar o site Contoso University para o ambiente de teste e testá-lo lá.

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
