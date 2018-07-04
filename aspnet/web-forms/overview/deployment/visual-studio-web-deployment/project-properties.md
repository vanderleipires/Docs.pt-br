---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Implantação de Web do ASP.NET usando o Visual Studio: propriedades do projeto | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web de aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: 48f2d5066986860b657d5608bd32fcfc9b6a1eda
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365880"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Implantação de Web do ASP.NET usando o Visual Studio: propriedades do projeto
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web application para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Algumas opções de implantação são configuradas nas propriedades do projeto que são armazenadas no arquivo de projeto (o *. csproj* ou *. vbproj* arquivo). Na maioria dos casos, os valores padrão dessas configurações são o que você deseja, mas você pode usar o **propriedades do projeto** internas de interface do usuário no Visual Studio para trabalhar com essas configurações se você precisar alterá-los. Neste tutorial você examine as configurações de implantação no **propriedades do projeto**. Você também pode criar um arquivo de espaço reservado que faz com que uma pasta vazia ser implantado.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Definir configurações de implantação na janela de propriedades do projeto

A maioria das configurações que afetam o que acontece durante a implantação são incluídos no perfil de publicação, como você verá nos tutoriais a seguir. Algumas configurações que você deve estar ciente de que estão localizadas na **empacotar/publicar** guias da **propriedades do projeto** janela. Essas configurações são especificadas para cada configuração de compilação — ou seja, você pode ter configurações diferentes para um build de versão que você tem para um build de depuração.

Na **Gerenciador de soluções**, com o botão direito do **ContosoUniversity** projeto, selecione **propriedades**e, em seguida, selecione o **pacote/Publicar Web**guia.

![Guia do pacote/Publicar Web](project-properties/_static/image1.png)

Quando a janela for exibida, o padrão é mostrando as configurações para qualquer configuração de build está ativa no momento para a solução. Se o **Configuration** caixa indica **Active Directory (versão)**, selecione **versão** para exibir as configurações para a configuração de build de versão. Você implantará builds de versão para ambientes de seu teste e produção.

![Selecionar configuração de build de versão](project-properties/_static/image2.png)

Com o **Active Directory (versão)** ou **versão** selecionada, você verá os valores que estão em vigor quando você implanta usando a configuração de build de versão:

- No **itens para implantar** caixa de **somente os arquivos necessários para executar o aplicativo** está selecionado. Outras opções são **todos os arquivos neste projeto** ou **todos os arquivos nesta pasta de projeto**. Ao deixar a seleção padrão inalterada você evita a implantação de arquivos de código-fonte, por exemplo. Essa configuração é o motivo por que as pastas que contêm os arquivos binários do SQL Server Compact precisava ser incluído no projeto. Para obter mais informações sobre essa configuração, consulte **por que não todos os arquivos na pasta Meu projeto são implantados?** na [perguntas frequentes sobre o ASP.NET Web Application Project implantação](https://msdn.microsoft.com/library/ee942158.aspx).
- **Símbolos de depuração de exclusão gerado** está selecionado. Você não depuração quando você usa essa configuração de compilação.
- **Incluir todos os bancos de dados configurados na guia empacotar/publicar SQL** está selecionado. Especifica se o Visual Studio irá implantar bancos de dados, bem como arquivos. Embora a caixa de seleção de rótulo único menções a **empacotar/publicar SQL** guia, desmarcando a caixa de seleção seria também desabilitar a implantação de banco de dados que está configurada no perfil de publicação. Você fará que mais tarde, portanto, a caixa de seleção deve permanecer selecionada. O **empacotar/publicar SQL** guia é usada para um banco de dados herdado, publicando o método que você não usar esses tutoriais.
- O **configurações de pacote de implantação da Web** seção não se aplica porque você está usando um único clique publicar nestes tutoriais.

Alterar o **configuração** caixa suspensa para a depuração para ver as configurações padrão para compilações de depuração. Os valores são os mesmos, exceto **Exclude gerado símbolos de depuração** está desmarcada para que você possa depurar quando você implanta uma compilação de depuração.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Certifique-se de que a pasta do Elmah obtém implantada

Como você viu no tutorial anterior, o [pacote do NuGet do Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) fornece a funcionalidade para o log de erros e emissão de relatórios. O aplicativo Contoso University Elmah foi configurado para armazenar detalhes do erro em uma pasta chamada *Elmah*:

![Pasta do ELMAH](project-properties/_static/image3.png)

Excluindo arquivos ou pastas específicas de implantação é um requisito comum; outro exemplo seria uma pasta que os usuários podem carregar arquivos. Você não deseja que os arquivos de log ou carregado de arquivos que foram criados em seu ambiente de desenvolvimento para ser implantado na produção. E se você estiver implantando uma atualização para a produção, que você não deseja que o processo de implantação para excluir arquivos que existem em produção. (Dependendo de como você define uma opção de implantação, se existir um arquivo no site de destino, mas não o site de origem quando você implanta, implantação da Web exclui-lo do destino.)

Como você viu neste tutorial, o **itens para implantar** opção a **pacote/Publicar Web** for definido como **apenas os arquivos necessários para executar este aplicativo**. Como resultado, os arquivos de log são criados pelo Elmah no desenvolvimento de não serão implantados, que é o que você quer fazer. (A serem implantados, eles precisam ser incluídos no projeto e seus **ação de compilação** propriedade precisa ser definido como **conteúdo**. Para obter mais informações, consulte **por que não todos os arquivos na pasta Meu projeto são implantados?** na [perguntas frequentes sobre o ASP.NET Web Application Project implantação](https://msdn.microsoft.com/library/ee942158.aspx)). No entanto, implantação da Web não criará uma pasta no site de destino a menos que haja pelo menos um arquivo a ser copiado para ele. Portanto, você adicionará uma *. txt* arquivo para a pasta para atuar como um espaço reservado para que a pasta será copiada.

No **Gerenciador de soluções**, clique com botão direito do *Elmah* pasta, selecione **Add New Item**e crie um arquivo de texto chamado *Espaço_reservado*. Coloque o seguinte texto nele: "Este é um arquivo de espaço reservado para garantir que a pasta é implantada." e salve o arquivo. Isso é tudo o que você precisa fazer para certificar-se de que o Visual Studio implanta esse arquivo e a pasta em que ele está, pois o **ação de compilação** propriedade de *. txt* arquivos é definido como **conteúdo**por padrão.

## <a name="summary"></a>Resumo

Agora você concluiu todas as tarefas de configuração de implantação. No próximo tutorial, você vai implantar o site Contoso University para o ambiente de teste e testá-lo lá.

> [!div class="step-by-step"]
> [Anterior](web-config-transformations.md)
> [Próximo](deploying-to-iis.md)
