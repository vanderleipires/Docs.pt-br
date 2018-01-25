---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: "Implantação de Web do ASP.NET usando o Visual Studio: propriedades do projeto | Microsoft Docs"
author: tdykstra
description: "Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, por usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: 85b6dbcc8d40c168a49513ef6b549f9ec7fa5097
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Implantação de Web do ASP.NET usando o Visual Studio: propriedades do projeto
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Algumas opções de implantação são configuradas nas propriedades do projeto que são armazenadas no arquivo de projeto (o *. csproj* ou *. vbproj* arquivo). Na maioria dos casos, os valores padrão dessas configurações são as que você deseja, mas você pode usar o **propriedades do projeto** interface de usuário criado no Visual Studio para trabalhar com essas configurações se você precisa alterá-los. Neste tutorial, você examine as configurações de implantação no **propriedades do projeto**. Você também pode criar um arquivo de espaço reservado que faz com que uma pasta vazia ser implantado.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Definir configurações de implantação na janela de propriedades do projeto

A maioria das configurações que afetam o que acontece durante a implantação são incluídas no perfil de publicação, como você verá nos tutoriais a seguir. Algumas configurações que você deve estar atento estão localizadas no **pacote/publicar** guias do **propriedades do projeto** janela. Essas configurações são especificadas para cada configuração de build — ou seja, você pode ter configurações diferentes para um build de versão que você tem para uma compilação de depuração.

Em **Solution Explorer**, com o botão direito do **ContosoUniversity** projeto, selecione **propriedades**e, em seguida, selecione o **pacote/Publicar Web**guia.

![Guia de pacote/publicar na Web](project-properties/_static/image1.png)

Quando a janela é exibida, o padrão é mostrando as configurações para qualquer configuração de compilação está ativa no momento para a solução. Se o **configuração** caixa indica **ativo (versão)**, selecione **versão** para exibir as configurações para a configuração de compilação de versão. Você implantará compilações de versão para ambientes de teste e de produção.

![Selecionar configuração de build de versão](project-properties/_static/image2.png)

Com **ativo (versão)** ou **versão** selecionada, você vê os valores que entram em vigor quando você implanta usando a configuração de compilação de versão:

- No **itens para implantar** caixa, **apenas os arquivos necessários para executar o aplicativo** está selecionado. Outras opções são **todos os arquivos neste projeto** ou **todos os arquivos nesta pasta**. Mantendo a seleção padrão inalterada, evite implantar arquivos de código fonte, por exemplo. Essa configuração é a razão por que as pastas que contêm os arquivos binários do SQL Server Compact tinham que ser incluído no projeto. Para obter mais informações sobre essa configuração, consulte **por que não todos os arquivos na pasta de projeto implantados?** na [perguntas Frequentes de implantação de projeto de aplicativo Web ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx).
- **Símbolos de depuração de exclusão gerado** está selecionado. Você não depuração quando você usar essa configuração de compilação.
- **Incluir todos os bancos de dados configurados na guia empacotar/publicar SQL** está selecionado. Especifica se o Visual Studio irá implantar bancos de dados, bem como arquivos. Embora a caixa de seleção rótulo menções somente o **pacote/publicar SQL** guia, desmarcar essa caixa de seleção também desabilitará implantação de banco de dados que está configurada no perfil de publicação. Você fará que mais tarde, para que a caixa de seleção deve permanecer selecionada. O **pacote/publicar SQL** guia é usada para um banco de dados herdado método que você não irá usar esses tutoriais de publicação.
- O **configurações de pacote de implantação da Web** seção não se aplica porque você está usando um clique publicar esses tutoriais.

Alterar o **configuração** caixa suspensa para depuração para ver as configurações padrão para compilações de depuração. Os valores são os mesmos, exceto **exclusão gerado símbolos de depuração** é desmarcada para que você pode depurar quando você implanta uma compilação de depuração.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Certifique-se de que a pasta Elmah é implantada

Como você viu no tutorial anterior, o [pacote Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornece funcionalidade para o log de erros e emissão de relatórios. O aplicativo Contoso University Elmah foi configurado para armazenar os detalhes do erro em uma pasta chamada *Elmah*:

![Pasta ELMAH](project-properties/_static/image3.png)

Excluindo arquivos ou pastas específicas de implantação é um requisito comum; outro exemplo seria uma pasta que os usuários podem carregar arquivos. Você não deseja que os arquivos de log ou de carregar arquivos que foram criados no seu ambiente de desenvolvimento para ser implantada para produção. E se você estiver implantando uma atualização para a produção, que você não deseja que o processo de implantação para excluir arquivos que existem em produção. (Dependendo de como você define uma opção de implantação, se existir um arquivo no site de destino, mas não no site de origem quando você implanta, Web Deploy excluirá de destino.)

Conforme mencionado anteriormente neste tutorial, o **itens para implantar** opção o **pacote/publicar na Web** for definido como **apenas os arquivos necessários para executar este aplicativo**. Como resultado, os arquivos de log são criados pelo Elmah no desenvolvimento de não serão implantados, que é o que deve acontecer. (Para ser implantado, precisaria ser incluído no projeto e seus **ação de compilação** propriedade precisa ser definido como **conteúdo**. Para obter mais informações, consulte **por que não todos os arquivos na pasta de projeto implantados?** na [perguntas Frequentes de implantação de projeto de aplicativo Web ASP.NET](https://msdn.microsoft.com/library/ee942158.aspx)). No entanto, Web Deploy não criará uma pasta no site de destino a menos que haja pelo menos um arquivo a ser copiado para ele. Portanto, você adicionará um *. txt* arquivo para a pasta para atuar como um espaço reservado para que a pasta será copiada.

Em **Solution Explorer**, com o botão direito do *Elmah* pasta, selecione **Adicionar Novo Item**e crie um arquivo de texto chamado *Espaço_reservado*. Coloque o seguinte texto nele: "Este é um arquivo de espaço reservado para garantir que a pasta é implantada." E salve o arquivo. Isso é tudo o que você precisa fazer para certificar-se de que o Visual Studio implanta esse arquivo e a pasta, pois o **ação de compilação** propriedade *. txt* arquivos é definido como **conteúdo**por padrão.

## <a name="summary"></a>Resumo

Agora você concluiu todas as tarefas de configuração de implantação. O seguinte tutorial, você implantar o site da Universidade de Contoso para o ambiente de teste e testá-lo lá.

>[!div class="step-by-step"]
[Anterior](web-config-transformations.md)
[Próximo](deploying-to-iis.md)
