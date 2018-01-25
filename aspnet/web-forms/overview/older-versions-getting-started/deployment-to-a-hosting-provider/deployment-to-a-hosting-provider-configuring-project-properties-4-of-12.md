---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Configurando propriedades do projeto - 4 de 12 | Microsoft Docs'
author: tdykstra
description: "Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 5632b801586c13084f887c4c414fc8686731094c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: Configurando propriedades do projeto - 4 de 12
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010. Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Algumas opções de implantação são configuradas nas propriedades do projeto que são armazenadas no arquivo de projeto (o *. csproj* ou *. vbproj* arquivo). Na maioria dos casos, os valores padrão dessas configurações são as que você deseja, mas você pode usar o **propriedades do projeto** interface de usuário criado no Visual Studio para trabalhar com essas configurações se você precisa alterá-los. Neste tutorial, você examine as configurações de implantação no **propriedades do projeto**. Você também pode criar um arquivo de espaço reservado que faz com que uma pasta vazia ser implantado.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Definindo configurações de implantação na janela de propriedades do projeto

A maioria das configurações que afetam o que acontece durante a implantação são incluídas no perfil de publicação, como você verá nos tutoriais a seguir. Algumas configurações que você deve estar atento estão localizadas no **pacote/publicar** guias do **propriedades do projeto** janela. Essas configurações são especificadas para cada configuração de build — ou seja, você pode ter configurações diferentes para um build de versão que você tem para uma compilação de depuração.

Em **Solution Explorer**, com o botão direito do **ContosoUniversity** projeto, selecione **propriedades**e, em seguida, selecione o **pacote/Publicar Web**guia.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Quando a janela é exibida, o padrão é mostrando as configurações para qualquer configuração de compilação está ativa no momento para a solução. Se o **configuração** caixa indica **ativo (versão)**, selecione **versão** para exibir as configurações para a configuração de compilação de versão. Você implantará compilações de versão para ambientes de teste e de produção.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Com **ativo (versão)** ou **versão** selecionada, você vê os valores que entram em vigor quando você implanta usando a configuração de compilação de versão:

- No **itens para implantar** caixa, **apenas os arquivos necessários para executar o aplicativo** está selecionado. Outras opções são **todos os arquivos neste projeto** ou **todos os arquivos nesta pasta**. Mantendo a seleção padrão inalterada, evite implantar arquivos de código fonte, por exemplo. Essa configuração é a razão por que as pastas que contêm os arquivos binários do SQL Server Compact tinham que ser incluído no projeto. Para obter mais informações sobre essa configuração, consulte **por que não todos os arquivos na pasta de projeto implantados?** na [perguntas Frequentes de implantação de projeto de aplicativo Web ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx).
- **Símbolos de depuração de exclusão gerado** está selecionado. Você não depuração quando você usar essa configuração de compilação.
- **Excluir arquivos do aplicativo\_pasta dados** não estiver selecionada. O arquivo do SQL Server Compact para o banco de dados de associação é nessa pasta, e você precisa implantá-lo. Ao implantar atualizações que não incluem alterações de banco de dados, você selecionará essa caixa de seleção.
- **Pré-compilar este aplicativo antes de publicar** não estiver selecionada. Na maioria dos cenários, não é necessário para pré-compilar a projetos de aplicativo web. Para obter mais informações sobre essa opção, consulte [guia do pacote/Publicar Web, propriedades de projeto](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) e [pré-compilar as configurações de caixa de diálogo avançada](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Incluir todos os bancos de dados configurados na guia empacotar/publicar SQL** for selecionada, mas essa opção não tem nenhum efeito agora porque você não estiver configurando o **pacote/publicar SQL** guia. Essa guia destina-se um método de implantação de banco de dados herdadas que costumava ser a única opção de implantação de bancos de dados do SQL Server. Você usará o **pacote/publicar SQL** guia o [migrando para o SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) tutorial.
- O **configurações de pacote de implantação da Web** seção não se aplica porque você está usando um clique publicar esses tutoriais.

Alterar o **configuração** caixa suspensa para depuração para ver as configurações padrão para compilações de depuração. Os valores são os mesmos, exceto **exclusão gerado símbolos de depuração** é desmarcada para que você pode depurar quando você implanta uma compilação de depuração.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Fazendo a certeza de que a pasta Elmah é implantada

Como você viu no tutorial anterior, o [pacote Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornece funcionalidade para o log de erros e emissão de relatórios. O aplicativo Contoso University Elmah foi configurado para armazenar os detalhes do erro em uma pasta chamada *Elmah*:

![Pasta ELMAH](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Excluindo arquivos ou pastas específicas de implantação é um requisito comum; outro exemplo seria uma pasta que os usuários podem carregar arquivos. Você não deseja que os arquivos de log ou de carregar arquivos que foram criados no seu ambiente de desenvolvimento para ser implantada para produção. E se você estiver implantando uma atualização para a produção, que você não deseja que o processo de implantação para excluir arquivos que existem em produção. (Dependendo de como você define uma opção de implantação, se existir um arquivo no site de destino, mas não no site de origem quando você implanta, Web Deploy excluirá de destino.)

Conforme mencionado anteriormente neste tutorial, o **itens para implantar** opção o **pacote/publicar na Web** for definido como **apenas os arquivos necessários para executar este aplicativo**. Como resultado, os arquivos de log são criados pelo Elmah no desenvolvimento de não serão implantados, que é o que deve acontecer. (Para ser implantado, precisaria ser incluído no projeto e seus **ação de compilação** propriedade precisa ser definido como **conteúdo**. Para obter mais informações, consulte **por que não todos os arquivos na pasta de projeto implantados?** na [perguntas Frequentes de implantação de projeto de aplicativo Web ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx)). No entanto, Web Deploy não criará uma pasta no site de destino a menos que haja pelo menos um arquivo a ser copiado para ele. Portanto, você adicionará um *. txt* arquivo para a pasta para atuar como um espaço reservado para que a pasta será copiada.

Em **Solution Explorer**, com o botão direito do *Elmah* pasta, selecione **Adicionar Novo Item**e crie um arquivo chamado *Espaço_reservado*. Coloque o seguinte texto nele: "Este é um arquivo de espaço reservado para garantir que a pasta é implantada." E salve o arquivo. Isso é tudo o que você precisa fazer para certificar-se de que o Visual Studio implanta esse arquivo e a pasta, pois o **ação de compilação** propriedade *. txt* arquivos é definido como **conteúdo**por padrão.

Agora você concluiu todas as tarefas de configuração de implantação. O seguinte tutorial, você implantar o site da Universidade de Contoso para o ambiente de teste e testá-lo lá.

>[!div class="step-by-step"]
[Anterior](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
[Próximo](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
