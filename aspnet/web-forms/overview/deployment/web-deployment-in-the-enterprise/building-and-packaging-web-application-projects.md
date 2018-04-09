---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
title: Criação e a embalagem de projetos de aplicativo Web | Microsoft Docs
author: jrjlee
description: Quando você deseja implantar um projeto de aplicativo web em um ambiente de servidor remoto, sua primeira tarefa é compilar o projeto e gerar um ote de implantação da web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 94e92f80-a7e3-4d18-9375-ff8be5d666ac
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/building-and-packaging-web-application-projects
msc.type: authoredcontent
ms.openlocfilehash: d630e1776607bd0bd7c61e1f0f7234ef58c7533b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="building-and-packaging-web-application-projects"></a>Criação e a embalagem de projetos de aplicativo Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Quando você deseja implantar um projeto de aplicativo web em um ambiente de servidor remoto, sua primeira tarefa é compilar o projeto e gerar um pacote de implantação da web. Este tópico descreve como o processo de compilação funciona para projetos de aplicativo web. Em particular, ele explica:
> 
> - Como o Pipeline de publicação de Web (WPP) estende o processo de compilação para incluir funcionalidade de implantação.
> - Como a ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web) ativa o aplicativo da web em um pacote de implantação.
> - Como a compilação e empacotamento o processo funciona e quais arquivos são criados.


No Visual Studio 2010, o processo de compilação e implantação para projetos de aplicativo web é suportado pelo WPP. O WPP fornece um conjunto de destinos Microsoft Build Engine (MSBuild) que estendem a funcionalidade do MSBuild e habilitá-lo a integrar a implantação da Web. No Visual Studio, você pode ver essa funcionalidade estendida nas páginas de propriedades para o seu projeto de aplicativo web. O **pacote/publicar na Web** página, junto com o **pacote/publicar SQL** página permite que você configure como o projeto de aplicativo web é fornecido para a implantação quando o processo de compilação for concluído.

![](building-and-packaging-web-application-projects/_static/image1.png)

## <a name="how-does-the-wpp-work"></a>Como funciona o WPP?

Se você dar uma olhada no arquivo de projeto para c#-projeto de aplicativo web com base, você pode ver que ele importa dois arquivos. targets.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample1.xml)]


A primeira **importação** instrução é comuns a todos os projetos do Visual c#. Esse arquivo, *Microsoft.CSharp.targets*, contém destinos e tarefas que são específicas para o Visual c#. Por exemplo, o compilador c# (**Csc**) tarefa é invocada aqui. O *Microsoft.CSharp.targets* arquivo sucessivamente importações o *Microsoft.Common.targets* arquivo. Define os destinos que são comuns a todos os projetos, como **criar**, **recriar**, **executar**, **compilar**, e **limpar** . A segunda **importação** instrução é específica para projetos de aplicativo web. O *WebApplication* arquivo sucessivamente importações o *Microsoft.Web.Publishing.targets* arquivo. O *Microsoft.Web.Publishing.targets* arquivo essencialmente *é* o WPP. Define como destinos, **pacote** e **MSDeployPublish**, que chama a implantação da Web para concluir várias tarefas de implantação.

Para compreender como esses destinos adicionais são usados na solução de exemplo Contact Manager, abra o *Publish.proj* de arquivo e examine o **BuildProjects** destino.


[!code-xml[Main](building-and-packaging-web-application-projects/samples/sample2.xml)]


Esse destino usa o **MSBuild** tarefas para criar vários projetos. Observe o **DeployOnBuild** e **DeployTarget** propriedades:

- O **DeployOnBuild = true** propriedade significa basicamente "Eu quero executar um destino adicional quando a compilação for concluída com êxito."
- O **DeployTarget** propriedade identifica o nome do destino que você deseja executar quando o **DeployOnBuild** propriedade é igual a **true**. Nesse caso, você está especificando que você deseja que o MSBuild para executar o **pacote** destino depois de criar o projeto.

O **pacote** destino é definido no *Microsoft.Web.Publishing.targets* arquivo. Essencialmente, esse destino usa a saída de compilação do seu projeto de aplicativo web e o transforma em um pacote de implantação da web que pode ser publicado em um servidor web IIS.

> [!NOTE]
> Para exibir um arquivo de projeto (por exemplo, <em>ContactManager.Mvc.csproj</em>) no Visual Studio 2010, primeiro é necessário descarregar o projeto de sua solução. No <strong>Solution Explorer</strong> janela, clique com botão direito no nó do projeto e, em seguida, clique em <strong>descarregar projeto</strong>. Clique com botão direito no nó do projeto novamente e, em seguida, clique em <strong>editar</strong><em>[arquivo de projeto]</em>). O arquivo de projeto será aberto na forma de XML bruta. Lembre-se de recarregar o projeto quando terminar.  
> Para obter mais informações sobre destinos do MSBuild, tarefas, e <strong>importação</strong> instruções, consulte [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md). Para obter uma introdução mais detalhada dos arquivos de projeto e o WPP, consulte [dentro do Microsoft Build Engine: usando MSBuild e o Team Foundation Build](http://amzn.com/0735645248) Sayed Hashimi de Ibrahim e William Bartholomew, ISBN: 978-0-7356-4524-0.


## <a name="what-is-a-web-deployment-package"></a>O que é um pacote de implantação da Web?

Quando você criar e implanta um projeto de aplicativo web, usando o Visual Studio 2010 ou usando o MSBuild diretamente, o resultado final é normalmente uma *pacote de implantação da web*. O pacote de implantação da web é um arquivo. zip. Contém tudo o que o IIS e a implantação da Web necessárias para recriar seu aplicativo web, incluindo:

- A saída compilada do seu aplicativo web, incluindo conteúdo, arquivos de recursos, arquivos de configuração, JavaScript e em cascata recursos do estilo CSS (folhas) e assim por diante.
- Assemblies de seu projeto de aplicativo web e para quaisquer referenciados projetos em sua solução.
- Scripts SQL para gerar qualquer banco de dados que você estiver implantando com o aplicativo web.

Depois que o pacote de implantação da web tiver sido gerado, você poderá publicá-lo em um servidor de web do IIS de várias maneiras. Por exemplo, você pode implantá-lo remotamente direcionando o serviço Web implantar o agente remoto ou o manipulador de implantação da Web no servidor web de destino, ou você pode usar o Gerenciador do IIS para importar manualmente o pacote no servidor web de destino. Para obter mais informações sobre essas abordagens de implantação, consulte [optar pela abordagem da direita para a implantação da Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md).

## <a name="how-does-the-build-process-work"></a>Como funciona o processo de compilação?

Isso mostra o que acontece quando você criar e empacotar um projeto de aplicativo web:

![](building-and-packaging-web-application-projects/_static/image2.png)

Quando você compila um projeto de aplicativo web, o processo de compilação gera um arquivo chamado *[nome do projeto]. SourceManifest.xml*. Junto com o arquivo de projeto e a saída da compilação, isso *. SourceManifest.xml* arquivo informa a implantação da Web que deve ser incluído no pacote de implantação da web. Usando essas entradas, Web Deploy gera um pacote de implantação da web denominado *[nome do projeto]. zip*.

Junto com o pacote de implantação da web, o processo de compilação gera dois arquivos que podem ajudá-lo a usar o pacote:

- O *. Deploy* arquivo inclui um conjunto de comandos com parâmetros de implantação da Web (MSDeploy.exe) que publica o pacote de implantação da web em um servidor de web IIS remoto. Executando o *. Deploy* arquivo, com os parâmetros apropriados, normalmente fornece uma rápida e mais fácil alternativo para criar manualmente o MSDeploy.exe comandos por conta própria.
- O *SetParameters.xml* arquivo fornece um conjunto de valores de parâmetro para o comando MSDeploy.exe. Estes valores incluem propriedades como o nome do aplicativo web do IIS para o qual você deseja implantar o pacote, os valores de quaisquer pontos de extremidade de serviço e as cadeias de conexão definidas no *Web. config* arquivo e qualquer propriedade de implantação valores definidos nas páginas de propriedades do projeto.

O *SetParameters.xml* arquivo é essencial para gerenciar o processo de implantação. Esse arquivo é gerado dinamicamente de acordo com o conteúdo do seu projeto de aplicativo web. Por exemplo, se você adicionar uma cadeia de caracteres de conexão para sua *Web. config* arquivo, o processo de compilação detectará automaticamente a cadeia de caracteres de conexão, parametrizar a implantação adequadamente e criar uma entrada de  *SetParameters.xml* arquivo para que você possa modificar a cadeia de caracteres de conexão como parte do processo de implantação. O próximo tópico, [parâmetros de configuração para implantação do pacote da Web](configuring-parameters-for-web-package-deployment.md), explica a função desse arquivo mais detalhadamente e descreve as diferentes maneiras em que você pode modificá-lo durante a compilação e implantação.

> [!NOTE]
> No Visual Studio 2010, o WPP não oferece suporte as páginas em um aplicativo web antes de empacotamento de pré-compilação. A próxima versão do Visual Studio e o WPP incluirá a capacidade para pré-compilar um aplicativo da web como uma opção de empacotamento.


## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma visão geral do processo de empacotamento e compilação para projetos de aplicativo web no Visual Studio 2010. Ele descrito como o WPP permite chamar comandos de implantação da Web do MSBuild, e ele explicado como a compilação e empacotamento o processo funciona.

Depois de criar um pacote de implantação da web, a próxima etapa é implantá-lo. Para obter mais informações sobre isso, consulte [parâmetros de configuração para implantação do pacote da Web](configuring-parameters-for-web-package-deployment.md) e [Implantando pacotes de Web](deploying-web-packages.md).

## <a name="further-reading"></a>Leitura adicional

Os tópicos neste tutorial, a próximos [parâmetros de configuração para implantação do pacote da Web](configuring-parameters-for-web-package-deployment.md) e [Implantando pacotes de Web](deploying-web-packages.md), fornecem orientação sobre como usar o pacote da web que você criou. O tutorial final da série, [implantação de Web Enterprise Advanced](../advanced-enterprise-web-deployment/advanced-enterprise-web-deployment.md), fornece orientação sobre como personalizar e solucionar problemas do processo de empacotamento.

Para obter uma introdução mais detalhada dos arquivos de projeto e o WPP, consulte [dentro do Microsoft Build Engine: usando MSBuild e o Team Foundation Build](http://amzn.com/0735645248) Sayed Hashimi de Ibrahim e William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Anterior](understanding-the-build-process.md)
> [Próximo](configuring-parameters-for-web-package-deployment.md)
