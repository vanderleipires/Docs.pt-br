---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Colocar aplicativos Web em Offline com Web implantar | Microsoft Docs
author: jrjlee
description: Este tópico descreve como utilizar um aplicativo da web offline para a duração de uma implantação automatizada usando o alerta de i de Web de serviços de informações da Internet (IIS)...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 1e9e5e9fef99a1179c1b830a42df6e6cb6b0e69d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382503"
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Implantar colocar aplicativos Web em Offline com a Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como utilizar um aplicativo da web offline para a duração de uma implantação automatizada usando a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web). Os usuários que navegam para o aplicativo web são redirecionados para um *App\_offline.htm* arquivo até que a implantação for concluída.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Em muitos cenários, você vai querer colocar um aplicativo da web offline quando você fizer alterações para os componentes relacionados, como bancos de dados ou serviços da web. Normalmente, no IIS e ASP.NET, você faz isso colocando um arquivo chamado *App\_offline.htm* na pasta raiz do aplicativo web ou site do IIS. O *App\_offline.htm* arquivo é um arquivo HTML padrão e geralmente conterá uma mensagem simples, avisando o usuário que o site está temporariamente indisponível devido à manutenção. Enquanto o *App\_offline.htm* arquivo existe na pasta raiz do site, o IIS será redirecionado automaticamente todas as solicitações para o arquivo. Quando você terminar de fazer atualizações, você remover o *App\_offline.htm* o site e o arquivo será retomada atendendo a solicitações como de costume.

Quando você usa a implantação da Web para realizar implantações automatizadas ou passo a passo para um ambiente de destino, talvez você queira incorporar adicionando e removendo o *App\_offline.htm* arquivo ao seu processo de implantação. Para fazer isso, você precisará concluir essas tarefas de alto nível:

- O arquivo de projeto do Microsoft Build Engine (MSBuild) que você usar para controlar o processo de implantação, crie um destino do MSBuild que copia uma *App\_offline.htm* arquivo para o servidor de destino antes de quaisquer tarefas de implantação começa.
- Adicione outro destino de MSBuild que remove os *App\_offline.htm* arquivo do servidor de destino quando todas as tarefas de implantação são concluídas.
- No projeto de aplicativo web, crie uma *. wpp.targets* arquivo que garante que um *App\_offline.htm* arquivo for adicionado ao pacote de implantação quando a implantação da Web é invocada.

Este tópico mostra como executar esses procedimentos. As tarefas e instruções passo a passo neste tópico pressupõem que você já tiver criado uma solução que contenha pelo menos um projeto de aplicativo web, e que você usa um arquivo de projeto personalizados para controlar o processo de implantação, conforme descrito em [implantação da Web no Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Como alternativa, você pode usar o [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solução a seguir os exemplos no tópico de exemplo.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Adicionar um aplicativo\_arquivo Offline para um projeto de aplicativo Web

A primeira tarefa que você precisa concluir é adicionar um *App\_offline* arquivo ao seu projeto de aplicativo web:

- Para impedir que o arquivo está interferindo com o processo de desenvolvimento (você não quer que seu aplicativo fique offline permanentemente), você deverá chamá-lo algo diferente de *App\_offline.htm*. Por exemplo, você pode nomear o arquivo *App\_offline. htm*.
- Para impedir que o arquivo que está sendo implantado como-está, você deve definir a ação de compilação para **None**.

**Adicionar um aplicativo\_arquivo offline para um projeto de aplicativo web**

1. Abra sua solução no Visual Studio 2010.
2. No **Gerenciador de soluções** janela, clique em seu projeto de aplicativo web, aponte para **Add**e, em seguida, clique em **Novo Item**.
3. No **Adicionar Novo Item** caixa de diálogo, selecione **página HTML**.
4. No **nome** , digite **App\_off-line. htm**e, em seguida, clique em **Add**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Adicionar um HTML simple para informar os usuários que o aplicativo não está disponível e, em seguida, salve o arquivo. Não inclua marcas do lado do servidor (por exemplo, todas as marcas que são prefixadas com "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. No **Gerenciador de soluções** , clique com botão direito no novo arquivo e, em seguida, clique em **propriedades**.
7. No **propriedades** janela, no **Build Action** linha, selecione **None**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Implantação e a exclusão de um aplicativo\_arquivo Offline

A próxima etapa é modificar sua lógica de implantação para copiar o arquivo para o servidor de destino no início do processo de implantação e removê-lo no final.

> [!NOTE]
> O próximo procedimento supõe que você está usando um arquivo de projeto personalizado do MSBuild para controlar o processo de implantação, conforme descrito em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Se você estiver implantando direto do Visual Studio, você precisará usar uma abordagem diferente. Sayed Ibrahim Hashimi descreve um exemplo dessa abordagem em [levar seu Offline durante a publicação de aplicativos Web como](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Para implantar um *App\_offline* arquivo para um site do IIS de destino, você precisa invocar MSDeploy.exe usando o [implantação da Web **contentPath** provedor](https://technet.microsoft.com/library/dd569034(WS.10).aspx). O **contentPath** provedor oferece suporte a caminhos de diretório físico e caminhos de site ou aplicativo do IIS, que torna a escolha ideal para a sincronização de um arquivo entre uma pasta de projeto do Visual Studio e um aplicativo web do IIS. Para implantar o arquivo, seu comando MSDeploy deve ter esta aparência:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Para remover o arquivo do site de destino no final do processo de implantação, o comando MSDeploy deve ter esta aparência:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Para automatizar esses comandos como parte de um processo de compilação e implantação, você precisará integrá-las ao seu arquivo de projeto personalizado do MSBuild. O procedimento a seguir descreve como fazer isso.

**Para implantar e excluir um aplicativo\_arquivo offline**

1. No Visual Studio 2010, abra o arquivo de projeto do MSBuild que controla seu processo de implantação. No [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solução de exemplo, isso é o *Publish.proj* arquivo.
2. Na raiz **projeto** elemento, crie uma nova **PropertyGroup** elemento para armazenar as variáveis para o *aplicativo\_offline* implantação:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. O **SourceRoot** propriedade está definida em outro lugar na *Publish.proj* arquivo. Ele indica o local da pasta raiz para o conteúdo de origem em relação ao caminho atual&#x2014;em outras palavras, relativo ao local do *Publish.proj* arquivo.
4. O **contentPath** provedor não aceitará caminhos de arquivo relativos, portanto, você precisará obter um caminho absoluto para o arquivo de origem antes de implantá-lo. Você pode usar o [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) tarefas para fazer isso.
5. Adicione um novo **alvo** elemento denominado **GetAppOfflineAbsolutePath**. Dentro desse destino, use o **ConvertToAbsolutePath** tarefa para obter um caminho absoluto para o *App\_modelo off-line* arquivo na pasta do projeto.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Esse destino usa o caminho relativo para o *App\_modelo off-line* arquivo na pasta do projeto e o salva em uma nova propriedade como um caminho de arquivo absoluto. O **BeforeTargets** atributo especifica que você deseja que esse destino seja executado antes do **DeployAppOffline** destino, que você criará na próxima etapa.
7. Adicionar um novo destino nomeado **DeployAppOffline**. Dentro desse destino, invocar o comando MSDeploy.exe que implanta seu *App\_offline* arquivo para o servidor web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. Neste exemplo, o **ContactManagerIisPath** propriedade está definida em outro lugar no arquivo de projeto. Isso é simplesmente um caminho do aplicativo IIS, no formulário *[nome do site do IIS] [nome do aplicativo] /*. Inclusão de uma condição de destino permite que os usuários alternem o *App\_offline* implantação ativado ou desativado alterando o valor de uma propriedade ou fornecendo um parâmetro de linha de comando.
9. Adicionar um novo destino nomeado **DeleteAppOffline**. Dentro desse destino, invocar o comando MSDeploy.exe que remove sua *App\_offline* arquivo do servidor web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. A tarefa final é invocar esses novos destinos nos pontos apropriados durante a execução de seu arquivo de projeto. Você pode fazer isso de várias maneiras. Por exemplo, nos *Publish.proj* arquivo, o **FullPublishDependsOn** propriedade especifica uma lista de destinos que devem ser executadas na ordem quando o **FullPublish** padrão destino é invocado.
11. Modifique o arquivo de projeto do MSBuild para invocar o **DeployAppOffline** e **DeleteAppOffline** destinos nos pontos apropriados no processo de publicação.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Quando você executa seu arquivo de projeto personalizado do MSBuild, o *App\_offline* arquivo será implantado para o servidor imediatamente após um build bem-sucedido. Ele será excluído, em seguida, do servidor depois de concluir todas as tarefas de implantação.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Adicionar um aplicativo\_arquivo Offline para pacotes de implantação

Dependendo de como você configura sua implantação, o aplicativo qualquer existente conteúdo no destino IIS web&#x2014;, como o *App\_offline.htm* arquivo&#x2014;pode ser excluído automaticamente quando você implanta uma web pacote para o destino. Para garantir que o *App\_offline.htm* arquivo permanece em vigor para a duração da implantação, você precisará incluir o arquivo dentro do próprio pacote de implantação da web Além disso, a implantação do arquivo diretamente no início do o processo de implantação.

- Se você seguiu as tarefas anteriores neste tópico, você irá adicionar o *App\_offline.htm* arquivo ao seu projeto de aplicativo web em um nome de arquivo diferente (usamos *aplicativo\_ off-line. htm*) e você tiver definirá a ação de compilação como **None**. Essas alterações são necessárias para impedir que o arquivo está interferindo com o desenvolvimento e depuração. Como resultado, você precisará personalizar o processo de empacotamento para garantir que o *App\_offline.htm* arquivo está incluído no pacote de implantação da web.

O Pipeline de publicação de Web (WPP) usa uma lista de itens chamada **FilesForPackagingFromProject** para criar uma lista de arquivos que devem ser incluídos no pacote de implantação da web. Você pode personalizar o conteúdo de seus pacotes da web, adicionando seus próprios itens a essa lista. Para fazer isso, você precisa concluir estas etapas de alto nível:

1. Crie um arquivo de projeto personalizado chamado *[nome do projeto].wpp.targets* na mesma pasta do arquivo de projeto.

    > [!NOTE]
    > O *. wpp.targets* arquivo precisa estar na mesma pasta que o arquivo de projeto de aplicativo web&#x2014;por exemplo, *ContactManager.Mvc.csproj*&#x2014;, em vez de na mesma pasta como qualquer personalizado Você pode usar para controlar o processo de compilação e implantação de arquivos de projeto.
2. No *. wpp.targets* de arquivo, crie um novo destino de MSBuild que executa *antes* o **CopyAllFilesToSingleFolderForPackage** destino. Este é o destino WPP que cria uma lista de itens a serem incluídos no pacote.
3. No novo destino, crie uma **ItemGroup** elemento.
4. No **ItemGroup** elemento, adicione uma **FilesForPackagingFromProject** item e especifique a *aplicativo\_offline.htm* arquivo.

O *. wpp.targets* arquivo deve ter esta aparência:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Estes são os principais pontos da observação neste exemplo:

- O **BeforeTargets** atributo insere esse destino para o WPP especificando que ele deve ser executado imediatamente antes do **CopyAllFilesToSingleFolderForPackage** destino.
- O **FilesForPackagingFromProject** item usa o **DestinationRelativePath** valor de metadados para renomear o arquivo de *aplicativo\_off-line. htm* para *App\_offline.htm* conforme ele é adicionado à lista.

O procedimento a seguir mostra como adicionar isso *. wpp.targets* arquivo a um projeto de aplicativo web.

**Para adicionar um. wpp.targets arquivo para um pacote de implantação da web**

1. Abra sua solução no Visual Studio 2010.
2. No **Gerenciador de soluções** janela, com o botão direito nó do projeto de aplicativo web (por exemplo, **ContactManager.Mvc**), aponte para **adicionar**e, em seguida, clique em **Novo Item**.
3. No **Adicionar Novo Item** caixa de diálogo, selecione o **arquivo XML** modelo.
4. No **nome** , digite *[nome do projeto] * * *.wpp.targets** (por exemplo, **ContactManager.Mvc.wpp.targets**) e, em seguida, clique em **Add**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Se você adicionar um novo item para o nó raiz de um projeto, o arquivo é criado na mesma pasta que o arquivo de projeto. Você pode verificar isso abrindo a pasta no Windows Explorer.
5. No arquivo, adicione a marcação de MSBuild descrita anteriormente.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Salve e feche o *[nome do projeto].wpp.targets* arquivo.

Na próxima vez que você compilar e empacotar seu projeto de aplicativo web, o WPP detectará automaticamente a *. wpp.targets* arquivo. O *App\_offline. htm* arquivo será incluído no pacote de implantação da web resultante como *App\_offline.htm*.

> [!NOTE]
> Se sua implantação falhar, o *App\_offline.htm* arquivo permanecerá em vigor e seu aplicativo permanecerá offline. Isso normalmente é o comportamento desejado. Para colocar o aplicativo novamente online, você pode excluir o *App\_offline.htm* arquivo do seu servidor web. Como alternativa, se você corrigir os erros e executar uma implantação bem-sucedida, o *App\_offline.htm* arquivo será removido.


## <a name="conclusion"></a>Conclusão

Este tópico descreveu como levar um aplicativo da web offline para a duração de uma implantação, publicando uma *App\_offline.htm* de arquivo para o servidor de destino no início do processo de implantação e removê-lo no final. Ele também abordou como incluir um *App\_offline.htm* arquivo em um pacote de implantação da web.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre o processo de implantação e empacotamento, consulte [compilação e empacotamento Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Configurando parâmetros para a implantação de pacote da Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), e [ Implantando pacotes da Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Se você publicar seus aplicativos web diretamente do Visual Studio, em vez de usar a abordagem de arquivo de projeto MSBuild personalizada descrita nesses tutoriais, você precisará usar uma abordagem ligeiramente diferente para levar seu aplicativo offline durante a publicação processo. Para obter mais informações, consulte [como se o seu aplicativo web offline durante a publicação](https://go.microsoft.com/?linkid=9805135) (postagem de blog).

> [!div class="step-by-step"]
> [Anterior](excluding-files-and-folders-from-deployment.md)
> [Próximo](running-windows-powershell-scripts-from-msbuild-project-files.md)
