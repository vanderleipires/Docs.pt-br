---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: O processo de empacotamento de solução de problemas | Microsoft Docs
author: jrjlee
description: Este tópico descreve como você pode coletar informações detalhadas sobre o processo de empacotamento, usando a propriedade EnablePackageProcessLoggingAndAssert em M...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 816ab77c44b52c6449a139475f2ef8546bd38071
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892680"
---
<a name="troubleshooting-the-packaging-process"></a>O processo de empacotamento de solução de problemas
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como você pode coletar informações detalhadas sobre o processo de empacotamento usando o **EnablePackageProcessLoggingAndAssert** propriedade no Microsoft Build Engine (MSBuild).
> 
> Quando você define o **EnablePackageProcessLoggingAndAssert** propriedade **true**, MSBuild será:
> 
> - Adicione informações adicionais sobre o processo de empacotamento para os logs de compilação.
> - Registrar erros em determinadas condições, por exemplo, se os arquivos duplicados são encontrados na lista de empacotamento.
> - Criar um diretório de Log no *ProjectName*\_pasta do pacote e usá-lo para registrar informações sobre os arquivos que você está criando.
> 
> Se o processo de empacotamento está falhando, ou os pacotes de implantação da web não contêm os arquivos que você espera, você pode usar essas informações para solucionar problemas de processo e pinpoint futuro errado.
> 
> > [!NOTE]
> > O **EnablePackageProcessLoggingAndAssert** propriedade só funciona quando você compila seu projeto usando o **depurar** configuração. A propriedade é ignorada em outras configurações.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Compreendendo a propriedade EnablePackageProcessLoggingAndAssert

[Compilando e projetos de aplicativo Web de empacotamento](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) descrito como o Pipeline de publicação de Web (WPP) fornece um conjunto de destinos do MSBuild que estendem a funcionalidade do MSBuild e habilitá-lo a integrar a Web de serviços de informações da Internet (IIS) Ferramenta de implantação (implantação da Web). Ao empacotar um projeto de aplicativo web, você está invocando destinos WPP.

Muitos desses alvos WPP incluem lógica condicional que registra informações adicionais quando o **EnablePackageProcessLoggingAndAssert** está definida como **true**. Por exemplo, se você revisar o **pacote** destino, você pode ver que ele cria um diretório de log adicionais e grava uma lista de arquivos em um arquivo de texto se **EnablePackageProcessLoggingAndAssert** é igual a **true**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Os destinos WPP são definidos no *Microsoft.Web.Publishing.targets* arquivo na pasta % PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web. Você pode abrir este arquivo e examine os destinos no Visual Studio 2010 ou qualquer editor de XML. Tome cuidado para não modificar o conteúdo do arquivo.


## <a name="enabling-the-additional-logging"></a>Habilitar o registro em log adicional

Você pode fornecer um valor para o **EnablePackageProcessLoggingAndAssert** propriedade de várias maneiras, dependendo de como você compilar o projeto.

Se você compilar o projeto a partir da linha de comando, você pode fornecer um valor para o **EnablePackageProcessLoggingAndAssert** a propriedade como um argumento de linha de comando:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Se você estiver usando um arquivo de projeto personalizados para criar seus projetos, você pode incluir o **EnablePackageProcessLoggingAndAssert** valor o **propriedades** atributo do **MSBuild**tarefas:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Se você estiver usando uma definição de compilação do Team Foundation Server (TFS) para criar seus projetos, você pode fornecer um valor para o **EnablePackageProcessLoggingAndAssert** propriedade o **argumentos de MSBuild** linha:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Para obter mais informações sobre como criar e configurar as definições de compilação, consulte [criar uma implantação de dá suporte a que definição de compilação](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Como alternativa, se você quiser incluir o pacote em cada compilação, você pode modificar o arquivo de projeto para o projeto de aplicativo web definir o **EnablePackageProcessLoggingAndAssert** propriedade **true**. Você deve adicionar a propriedade para o primeiro **PropertyGroup** elemento dentro do seu arquivo. csproj ou. vbproj.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Revisando os arquivos de Log

Quando você criar e empacotar um projeto de aplicativo web com **EnablePackageProcessLoggingAndAssert** definida como **true**, MSBuild cria uma pasta adicional denominada login o *ProjectName* \_Pasta do pacote. A pasta de Log contém vários arquivos:

![](troubleshooting-the-packaging-process/_static/image2.png)

A lista de arquivos que você vê variam de acordo com os itens em seu projeto e o processo de compilação. No entanto, esses arquivos normalmente são usados para gravar a lista de arquivos que está coletando o WPP para empacotamento, em várias fases do processo:

- O *PreExcludePipelineCollectFilesPhaseFileList.txt* arquivo lista os arquivos que a coleta de MSBuild para empacotar antes de todos os arquivos que são especificados para exclusão são removidos.
- O *AfterExcludeFilesFilesList.txt* arquivo contém a lista de arquivos modificados depois que todos os arquivos que são especificados para exclusão são removidos.

    > [!NOTE]
    > Para obter mais informações sobre como excluir arquivos e pastas do processo de empacotamento, consulte [excluindo arquivos e pastas de implantação](excluding-files-and-folders-from-deployment.md).
- O *AfterTransformWebConfig.txt* arquivo lista os arquivos coletados para empacotamento após qualquer *Web. config* transformações foram realizadas. Nesta lista, qualquer configuração específica *Web. config* transformar arquivos, como *Web.Debug.config* e *Web.Release.config*, são excluídos da lista de arquivos para empacotamento. Um único transformado *Web. config* está incluído em seu lugar.
- O *PostAutoParameterizationWebConfigConnectionStrings.txt* arquivo contém a lista de arquivos após as cadeias de conexão a *Web. config* arquivo foram parametrizados. Este é o processo que permite que você substitua as cadeias de conexão com as configurações corretas para o seu ambiente de destino quando você implantar o pacote.
- O *Prepackage.txt* arquivo contém a lista de pré-compilação finalizada dos arquivos a serem incluídos no pacote.

> [!NOTE]
> Normalmente, os nomes dos arquivos de log adicionais correspondem aos destinos WPP. Você pode examinar esses destinos examinando o *Microsoft.Web.Publishing.targets* arquivo na pasta % PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


Se o conteúdo do pacote da web não forem os esperados, examinar esses arquivos pode ser uma maneira útil para identificar em qual ponto de coisas processo deu errado.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como você pode usar o **EnablePackageProcessLoggingAndAssert** propriedade no MSBuild para solucionar problemas do processo de empacotamento. Ele explicado as diferentes maneiras em que você pode fornecer o valor da propriedade para o processo de compilação, e ele descritas as informações adicionais que serão registradas quando você definir a propriedade como **true**.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como usar arquivos de projeto MSBuild personalizados para controlar o processo de implantação, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obter mais informações sobre o WPP e como ele gerencia o processo de empacotamento, consulte [criação e a projetos de aplicativo Web de empacotamento](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Para obter orientação sobre como excluir arquivos e pastas específicos de pacotes de implantação da web, consulte [excluindo arquivos e pastas de implantação](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Anterior](running-windows-powershell-scripts-from-msbuild-project-files.md)
