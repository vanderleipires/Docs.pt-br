---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
title: Implantar aplicativos de Web do colocando Offline com o Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como executar um aplicativo da web offline para a duração de uma implantação automatizada usando o alerta de i do Internet Information Services (IIS) da Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3e9f6e7d-8967-4586-94d5-d3a122f12529
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/taking-web-applications-offline-with-web-deploy
msc.type: authoredcontent
ms.openlocfilehash: 511201dc5646340b21023430fa319417f2b53ae2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="taking-web-applications-offline-with-web-deploy"></a>Implantar aplicativos de Web do colocando Offline com o Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como executar um aplicativo da web offline para a duração de uma implantação automatizada usando a ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web). Os usuários que navegam para o aplicativo da web são redirecionados para um *aplicativo\_offline.htm* arquivo até que a implantação for concluída.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Em muitos cenários, você vai querer colocar um aplicativo da web offline quando você fizer alterações em componentes relacionados, como bancos de dados ou serviços da web. Normalmente, no IIS e ASP.NET, você faz isso colocando um arquivo chamado *aplicativo\_offline.htm* na pasta raiz do aplicativo web ou site do IIS. O *aplicativo\_offline.htm* arquivo é um arquivo HTML padrão e geralmente conterá uma simple mensagem avisando o usuário que o site está temporariamente indisponível devido à manutenção. Enquanto o *aplicativo\_offline.htm* arquivo existe na pasta raiz do site, o IIS redirecionará automaticamente todas as solicitações para o arquivo. Quando você terminar de fazer atualizações, você remover o *aplicativo\_offline.htm* o site e o arquivo continua a atender solicitações como de costume.

Quando você usar a implantação da Web para realizar implantações automatizadas ou única etapa em um ambiente de destino, talvez você queira incorporar adicionando e removendo o *aplicativo\_offline.htm* arquivo em seu processo de implantação. Para fazer isso, você precisará concluir essas tarefas de alto nível:

- No arquivo de projeto Microsoft Build Engine (MSBuild) que você usa para controlar o processo de implantação, criar um destino do MSBuild que copia um *aplicativo\_offline.htm* arquivo para o servidor de destino antes de quaisquer tarefas de implantação começa.
- Adicione outro destino de MSBuild remove o *aplicativo\_offline.htm* arquivo do servidor de destino quando todas as tarefas de implantação estiverem concluídas.
- No projeto de aplicativo web, crie um *. wpp.targets* arquivo que garante que uma *aplicativo\_offline.htm* arquivo é adicionado ao pacote de implantação quando a implantação da Web é invocada.

Neste tópico mostram como executar esses procedimentos. As tarefas e instruções passo a passo neste tópico pressupõem que você já criou uma solução que contém pelo menos um projeto de aplicativo web, e se você usar um arquivo de projeto personalizados para controlar o processo de implantação, conforme descrito em [implantação da Web do Enterprise](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md). Como alternativa, você pode usar o [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solução a seguir os exemplos do tópico de exemplo.

## <a name="adding-an-appoffline-file-to-a-web-application-project"></a>Adicionar um aplicativo\_arquivos off-line para um projeto de aplicativo Web

A primeira tarefa, você precisa concluir é adicionar um *aplicativo\_offline* ao seu projeto de aplicativo web:

- Para impedir que o arquivo de interferir com o processo de desenvolvimento (não desejar que o aplicativo seja permanentemente offline), você deve chamá-lo algo diferente de *aplicativo\_offline.htm*. Por exemplo, você pode nomear o arquivo *aplicativo\_off-line-. htm*.
- Para impedir que o arquivo que está sendo implantado como-é, você deve definir a ação de compilação para **nenhum**.

**Para adicionar um aplicativo\_arquivos off-line para um projeto de aplicativo web**

1. Abra sua solução no Visual Studio 2010.
2. No **Solution Explorer** janela, clique duas vezes o projeto de aplicativo web, aponte para **adicionar**e, em seguida, clique em **Novo Item**.
3. No **Adicionar Novo Item** caixa de diálogo, selecione **página HTML**.
4. No **nome** , digite **aplicativo\_off-line-. htm**e, em seguida, clique em **adicionar**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image1.png)
5. Adicionar HTML simples para informar aos usuários que o aplicativo não está disponível e, em seguida, salve o arquivo. Não inclua marcas do lado do servidor (por exemplo, marcas que têm o prefixo "asp:"). 

    ![](taking-web-applications-offline-with-web-deploy/_static/image2.png)
6. No **Solution Explorer** janela, clique no novo arquivo e, em seguida, clique em **propriedades**.
7. No **propriedades** janela, no **ação de compilação** linha, selecione **nenhum**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image3.png)

## <a name="deploying-and-deleting-an-appoffline-file"></a>Implantação e excluir um aplicativo\_arquivo Offline

A próxima etapa é modificar sua lógica de implantação para copiar o arquivo para o servidor de destino no início do processo de implantação e removê-lo no final.

> [!NOTE]
> O procedimento a seguir supõe que você estiver usando um arquivo de projeto MSBuild personalizado para controlar o processo de implantação, conforme descrito em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Se você estiver implantando direta do Visual Studio, você precisará usar uma abordagem diferente. Sayed Ibrahim Hashimi descreve uma dessas abordagens em [como levar sua publicação na Web aplicativo Offline durante](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx).


Para implantar um *aplicativo\_offline* arquivo para um site do IIS de destino, é necessário chamar MSDeploy.exe usando o [implantação da Web **contentPath** provedor](https://technet.microsoft.com/library/dd569034(WS.10).aspx). O **contentPath** provedor oferece suporte a caminhos de diretório físico e caminhos de site ou aplicativo do IIS, que é a opção ideal para a sincronização de um arquivo entre uma pasta de projeto do Visual Studio e um aplicativo web do IIS. Para implantar o arquivo, o comando MSDeploy deve ser semelhante a esta:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample1.cmd)]


Para remover o arquivo do site de destino no final do processo de implantação, o comando MSDeploy deve ser semelhante a esta:


[!code-console[Main](taking-web-applications-offline-with-web-deploy/samples/sample2.cmd)]


Para automatizar esses comandos como parte de um processo de compilação e implantação, você precisa integrá-las em seu arquivo de projeto MSBuild personalizado. O procedimento a seguir descreve como fazer isso.

**Para implantar e excluir um aplicativo\_arquivo offline**

1. No Visual Studio 2010, abra o arquivo de projeto MSBuild que controla o processo de implantação. No [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solução de exemplo, este é o *Publish.proj* arquivo.
2. Na raiz **projeto** elemento, crie um novo **PropertyGroup** elemento armazenar variáveis para o *aplicativo\_offline* implantação:

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample3.xml)]
3. O **SourceRoot** propriedade está definida em outro lugar no *Publish.proj* arquivo. Indica o local da pasta raiz para o conteúdo de origem em relação ao caminho atual&#x2014;em outras palavras, relativo ao local do *Publish.proj* arquivo.
4. O **contentPath** provedor não aceitará caminhos de arquivo, portanto você precisa obter um caminho absoluto para o arquivo de origem antes de implantá-lo. Você pode usar o [ConvertToAbsolutePath](https://msdn.microsoft.com/library/bb882668.aspx) tarefas para fazer isso.
5. Adicionar um novo **destino** elemento chamado **GetAppOfflineAbsolutePath**. Dentro desse destino, use o **ConvertToAbsolutePath** tarefa para obter um caminho absoluto para o *aplicativo\_modelo off-line* arquivo na pasta do projeto.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample4.xml)]
6. Esse destino usa o caminho relativo para o *aplicativo\_modelo off-line* arquivo na pasta do projeto e salva-o em uma nova propriedade como um caminho de arquivo absoluto. O **BeforeTargets** atributo especifica que você deseja que esse destino seja executado antes do **DeployAppOffline** destino, que você criará na próxima etapa.
7. Adicionar um novo destino chamado **DeployAppOffline**. Dentro desse destino, invocar o comando MSDeploy.exe que implanta o *aplicativo\_offline* arquivo para o servidor web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample5.xml)]
8. Neste exemplo, o **ContactManagerIisPath** propriedade está definida em outro lugar no arquivo de projeto. Isso é simplesmente um caminho aplicativo do IIS, no formato *[nome do site IIS] / [nome do aplicativo]*. Inclusão de uma condição no destino permite aos usuários alternar o *aplicativo\_offline* implantação ou desativar alterando o valor de uma propriedade ou fornecendo um parâmetro de linha de comando.
9. Adicionar um novo destino chamado **DeleteAppOffline**. Dentro desse destino, invocar o comando MSDeploy.exe que remove o *aplicativo\_offline* arquivo do servidor web de destino.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample6.xml)]
10. A tarefa final é invocar esses novos destinos momentos apropriados durante a execução do seu arquivo de projeto. Você pode fazer isso de várias maneiras. Por exemplo, no *Publish.proj* arquivo, o **FullPublishDependsOn** propriedade especifica uma lista de destinos que devem ser executadas na ordem quando o **FullPublish** padrão destino é invocado.
11. Modifique o arquivo de projeto MSBuild para invocar o **DeployAppOffline** e **DeleteAppOffline** destinos momentos apropriados no processo de publicação.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample7.xml)]

Quando você executar o arquivo de projeto MSBuild personalizado, o *aplicativo\_offline* arquivo será implantado para o servidor imediatamente após uma compilação bem-sucedida. Ele será excluído, em seguida, do servidor depois que todas as tarefas de implantação estiverem concluídas.

## <a name="adding-an-appoffline-file-to-deployment-packages"></a>Adicionar um aplicativo\_arquivos off-line para pacotes de implantação

Dependendo de como você configura sua implantação, o aplicativo qualquer existente conteúdo no destino IIS web&#x2014;como o *aplicativo\_offline.htm* arquivo&#x2014;podem ser excluídos automaticamente quando você implanta uma web pacote para o destino. Para garantir que o *aplicativo\_offline.htm* arquivo permanece em vigor durante a implantação, você precisa incluir o arquivo dentro do próprio pacote de implantação da web Além disso, para implantar o arquivo diretamente no início de o processo de implantação.

- Se você seguiu as tarefas anteriores neste tópico, você deverá adicionar o *aplicativo\_offline.htm* ao seu projeto de aplicativo web com um nome de arquivo diferente (usamos *aplicativo\_ offline-. htm*) e você tiver definirá a ação de compilação **nenhum**. Essas alterações são necessárias para impedir que o arquivo de interferir com o desenvolvimento e depuração. Como resultado, você precisa personalizar o processo de empacotamento para garantir que o *aplicativo\_offline.htm* arquivo está incluído no pacote de implantação da web.

O Pipeline de publicação de Web (WPP) usa uma lista de itens denominada **FilesForPackagingFromProject** para criar uma lista de arquivos que devem ser incluídos no pacote de implantação da web. Você pode personalizar o conteúdo de seus pacotes da web adicionando seus próprios itens à lista. Para fazer isso, você precisa concluir estas etapas de alto nível:

1. Crie um arquivo de projeto personalizado chamado *.wpp.targets [nome do projeto]* na mesma pasta que o arquivo de projeto.

    > [!NOTE]
    > O *. wpp.targets* arquivos precisam estar na mesma pasta que o arquivo de projeto de aplicativo web&#x2014;por exemplo, *ContactManager.Mvc.csproj*&#x2014;em vez de na mesma pasta que qualquer personalizado arquivos de projeto usados para controlar o processo de compilação e implantação.
2. No *. wpp.targets* de arquivo, crie um novo destino do MSBuild que executa *antes de* o **CopyAllFilesToSingleFolderForPackage** destino. Este é o destino WPP que cria uma lista de itens para incluir no pacote.
3. No novo destino, crie uma **ItemGroup** elemento.
4. No **ItemGroup** elemento, adicionar um **FilesForPackagingFromProject** item e especifique o *aplicativo\_offline.htm* arquivo.

O *. wpp.targets* arquivo deve ser semelhante a esta:


[!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample8.xml)]


Estes são os principais pontos de observação neste exemplo:

- O **BeforeTargets** atributo insere esse destino para o WPP especificando que ele deve ser executado imediatamente antes do **CopyAllFilesToSingleFolderForPackage** destino.
- O **FilesForPackagingFromProject** item usa o **DestinationRelativePath** valor de metadados para renomear o arquivo de *aplicativo\_off-line-. htm* para *aplicativo\_offline.htm* conforme ele é adicionado à lista.

O procedimento a seguir mostra como adicionar isso *. wpp.targets* arquivo em um projeto de aplicativo web.

**Para adicionar um. wpp.targets arquivo a um pacote de implantação da web**

1. Abra sua solução no Visual Studio 2010.
2. No **Solution Explorer** janela, clique o nó do projeto de aplicativo web (por exemplo, **ContactManager.Mvc**), aponte para **adicionar**e, em seguida, clique em **Novo Item**.
3. No **Adicionar Novo Item** caixa de diálogo, selecione o **arquivo XML** modelo.
4. No **nome** , digite *[nome do projeto] *.wpp.targets** (por exemplo, **ContactManager.Mvc.wpp.targets**) e, em seguida, clique em **adicionar**.

    ![](taking-web-applications-offline-with-web-deploy/_static/image4.png)

    > [!NOTE]
    > Se você adicionar um novo item para o nó raiz de um projeto, o arquivo é criado na mesma pasta que o arquivo de projeto. Você pode verificar isso abrindo a pasta no Windows Explorer.
5. No arquivo, adicione a marcação de MSBuild descrita anteriormente.

    [!code-xml[Main](taking-web-applications-offline-with-web-deploy/samples/sample9.xml)]
6. Salve e feche o *[nome do projeto].wpp.targets* arquivo.

Na próxima vez que você compilação e pacote de seu projeto de aplicativo web, o WPP detectará automaticamente o *. wpp.targets* arquivo. O *aplicativo\_off-line-. htm* arquivo será incluído no pacote de implantação da web resultante como *aplicativo\_offline.htm*.

> [!NOTE]
> Se sua implantação falhar, o *aplicativo\_offline.htm* arquivo permanecerá no local e o aplicativo permanecerá offline. Isso normalmente é o comportamento desejado. Para colocar o aplicativo online novamente, você pode excluir o *aplicativo\_offline.htm* arquivo do servidor web. Como alternativa, se você corrigir os erros e executar uma implantação bem-sucedida, o *aplicativo\_offline.htm* arquivo será removido.


## <a name="conclusion"></a>Conclusão

Este tópico descrita como se um aplicativo da web offline para a duração de uma implantação, publicando um *aplicativo\_offline.htm* o arquivo para o servidor de destino no início do processo de implantação e removê-lo no final. Ele também trata de como incluir um *aplicativo\_offline.htm* arquivo em um pacote de implantação da web.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre o empacotamento e o processo de implantação, consulte [criação e a projetos de aplicativo Web de empacotamento](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [parâmetros de configuração para implantação do pacote da Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), e [ Implantando pacotes da Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

Se você publicar seus aplicativos web diretamente do Visual Studio, em vez de usar a abordagem de arquivo de projeto MSBuild personalizada descrita nesses tutoriais, você precisará usar uma abordagem ligeiramente diferente para colocar o aplicativo offline durante a publicação processo. Para obter mais informações, consulte [como se o seu aplicativo web offline durante a publicação](https://go.microsoft.com/?linkid=9805135) (postagem do blog).

> [!div class="step-by-step"]
> [Anterior](excluding-files-and-folders-from-deployment.md)
> [Próximo](running-windows-powershell-scripts-from-msbuild-project-files.md)
