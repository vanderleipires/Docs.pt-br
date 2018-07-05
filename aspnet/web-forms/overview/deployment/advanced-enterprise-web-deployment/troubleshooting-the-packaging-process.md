---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
title: O processo de empacotamento da solução de problemas | Microsoft Docs
author: jrjlee
description: Este tópico descreve como você pode coletar informações detalhadas sobre o processo de empacotamento, usando a propriedade EnablePackageProcessLoggingAndAssert no M...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 794bd819-00fc-47e2-876d-fc5d15e0de1c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/troubleshooting-the-packaging-process
msc.type: authoredcontent
ms.openlocfilehash: 22086d3c154214457fe35794998accdf6109471c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37384138"
---
<a name="troubleshooting-the-packaging-process"></a>O processo de empacotamento da solução de problemas
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como você pode coletar informações detalhadas sobre o processo de empacotamento usando o **EnablePackageProcessLoggingAndAssert** propriedade no Microsoft Build Engine (MSBuild).
> 
> Quando você define o **EnablePackageProcessLoggingAndAssert** propriedade **verdadeiro**, MSBuild será:
> 
> - Adicione informações adicionais sobre o processo de empacotamento para os logs de build.
> - Registrar erros em determinadas condições, por exemplo, se os arquivos duplicados são encontrados na lista de empacotamento.
> - Crie um diretório de Log na *NomeDoProjeto*\_pasta do pacote e usá-la para registrar informações sobre os arquivos que você está Empacotando.
> 
> Se o processo de empacotamento está falhando, ou os pacotes de implantação da web não contêm os arquivos que você espera, você pode usar essas informações para solucionar problemas de pinpoint e processo em que as coisas estão indo erradas.
> 
> > [!NOTE]
> > O **EnablePackageProcessLoggingAndAssert** propriedade só funciona se você compilar seu projeto usando o **depurar** configuração. A propriedade é ignorada em outras configurações.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="understanding-the-enablepackageprocessloggingandassert-property"></a>Compreendendo a propriedade EnablePackageProcessLoggingAndAssert

[Compilação e empacotamento Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md) descrito como o Pipeline de publicação de Web (WPP) fornece um conjunto de destinos do MSBuild que estendem a funcionalidade do MSBuild e habilitá-lo para integrar com a Web de serviços de informações da Internet (IIS) Ferramenta de implantação (implantação da Web). Ao empacotar um projeto de aplicativo web, você invoca os destinos WPP.

Muitos desses destinos WPP incluem lógica condicional que registra em log informações adicionais quando o **EnablePackageProcessLoggingAndAssert** estiver definida como **verdadeiro**. Por exemplo, se você examinar a **pacote** destino, você pode ver que ele cria um diretório de log adicionais e grava uma lista de arquivos em um arquivo de texto se **EnablePackageProcessLoggingAndAssert** é igual a **verdadeira**.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample1.xml)]


> [!NOTE]
> Os destinos WPP são definidos na *Microsoft.Web.Publishing.targets* arquivo na pasta % PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web. Você pode abrir esse arquivo e examine os destinos no Visual Studio 2010 ou em qualquer editor de XML. Tome cuidado para não modificar o conteúdo do arquivo.


## <a name="enabling-the-additional-logging"></a>Habilitando o registro em log adicional

Você pode fornecer um valor para o **EnablePackageProcessLoggingAndAssert** propriedade de várias maneiras, dependendo de como você compila seu projeto.

Se você compilar seu projeto a partir da linha de comando, você pode fornecer um valor para o **EnablePackageProcessLoggingAndAssert** a propriedade como um argumento de linha de comando:


[!code-console[Main](troubleshooting-the-packaging-process/samples/sample2.cmd)]


Se você estiver usando um arquivo de projeto personalizados para compilar seus projetos, você pode incluir a **EnablePackageProcessLoggingAndAssert** o valor a **propriedades** atributo do **MSBuild**tarefa:


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample3.xml)]


Se você estiver usando uma definição de compilação do Team Foundation Server (TFS) para compilar seus projetos, você pode fornecer um valor para o **EnablePackageProcessLoggingAndAssert** propriedade no **argumentos do MSBuild** linha:![](troubleshooting-the-packaging-process/_static/image1.png)

> [!NOTE]
> Para obter mais informações sobre como criar e configurar as definições de compilação, consulte [criação de uma implantação de dá suporte a que definição de compilação](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).


Como alternativa, se você quiser incluir o pacote em cada compilação, você pode modificar o arquivo de projeto do projeto de aplicativo da web definir a **EnablePackageProcessLoggingAndAssert** propriedade **verdadeiro**. Você deve adicionar a propriedade para a primeira **PropertyGroup** elemento dentro do seu arquivo. csproj ou. vbproj.


[!code-xml[Main](troubleshooting-the-packaging-process/samples/sample4.xml)]


## <a name="reviewing-the-log-files"></a>Revisando os arquivos de Log

Quando você compilar e empacotar um projeto de aplicativo web com **EnablePackageProcessLoggingAndAssert** definido como **verdadeiro**, MSBuild cria uma pasta adicional chamada Login o *ProjectName* \_Pasta do pacote. A pasta de Log contém vários arquivos:

![](troubleshooting-the-packaging-process/_static/image2.png)

A lista de arquivos que você vê variam de acordo com as coisas em seu projeto e o processo de compilação. No entanto, esses arquivos normalmente são usados para gravar a lista de arquivos que o WPP está coletando para empacotamento em vários estágios do processo:

- O *PreExcludePipelineCollectFilesPhaseFileList.txt* arquivo lista os arquivos que a coleta de MSBuild para criação de pacotes antes de todos os arquivos que são especificados para exclusão são removidos.
- O *AfterExcludeFilesFilesList.txt* arquivo contém a lista de arquivos modificados depois que todos os arquivos que são especificados para exclusão são removidos.

    > [!NOTE]
    > Para obter mais informações sobre como excluir arquivos e pastas do processo de empacotamento, consulte [excluindo arquivos e pastas de implantação](excluding-files-and-folders-from-deployment.md).
- O *AfterTransformWebConfig.txt* arquivo de lista os arquivos coletados para empacotamento após qualquer *Web. config* transformações foram realizadas. Nessa lista, qualquer configuração específica *Web. config* transformar arquivos, como *Debug* e *Release*, são excluídos da lista de arquivos para empacotamento. Um único transformado *Web. config* está incluído em seu lugar.
- O *PostAutoParameterizationWebConfigConnectionStrings.txt* arquivo contém a lista de arquivos após as cadeias de caracteres de conexão na *Web. config* arquivo foram parametrizados. Este é o processo que permite que você substitua as cadeias de conexão com as configurações corretas para o seu ambiente de destino quando você implanta o pacote.
- O *Prepackage.txt* arquivo contém a lista de pré-build finalizada de arquivos a serem incluídos no pacote.

> [!NOTE]
> Normalmente, os nomes dos arquivos de log adicionais correspondem aos destinos WPP. Você pode examinar esses destinos examinando os *Microsoft.Web.Publishing.targets* arquivo na pasta % PROGRAMFILES (x86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web.


Se o conteúdo do pacote da web não forem os esperados, examinar esses arquivos pode ser uma maneira útil para identificar em que ponto em que as coisas de processo não deu certo.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como você pode usar o **EnablePackageProcessLoggingAndAssert** propriedade do MSBuild para solucionar problemas do processo de empacotamento. Ele explicou as diferentes maneiras em que você pode fornecer o valor da propriedade para o processo de compilação e descreveu as informações adicionais que são registradas quando você definir a propriedade como **verdadeira**.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como usar arquivos de projeto personalizados do MSBuild para controlar o processo de implantação, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obter mais informações sobre o WPP e como ele gerencia o processo de empacotamento, consulte [compilação e empacotamento Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md). Para obter orientação sobre como excluir arquivos e pastas específicos de pacotes de implantação da web, consulte [excluindo arquivos e pastas de implantação](excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Anterior](running-windows-powershell-scripts-from-msbuild-project-files.md)
