---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Configurar parâmetros para a implantação de pacote da Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como definir valores de parâmetro, como nomes de aplicativo web de serviços de informações da Internet (IIS), cadeias de caracteres de conexão e pontos de extremidade de serviço...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: e6db7a8351e01bbbc14eb2b993248ee7d5a84f7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386539"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Configurar parâmetros para a implantação de pacote da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como definir valores de parâmetro, como nomes de aplicativo web de serviços de informações da Internet (IIS), cadeias de caracteres de conexão e pontos de extremidade de serviço, quando você implanta um pacote da web em um servidor web IIS remoto.


Quando você compila um projeto de aplicativo web, a compilação e o processo de empacotamento gera três arquivos principais:

- Um *[nome do projeto]. zip* arquivo. Isso é o pacote de implantação da web para seu projeto de aplicativo web. Este pacote contém todos os assemblies, arquivos, scripts de banco de dados e recursos necessários para recriar seu aplicativo web em um servidor de web IIS remoto.
- Um *[nome do projeto].deploy.cmd* arquivo. Contém um conjunto de comandos com parâmetros (MSDeploy.exe) de implantação da Web que publica o pacote de implantação da web em um servidor web IIS remoto.
- Um *[nome do projeto]. SetParameters.xml* arquivo. Isso fornece um conjunto de valores de parâmetro para o comando MSDeploy.exe. Você pode atualizar os valores nesse arquivo e passá-lo à implantação da Web como um parâmetro de linha de comando quando você implanta o pacote da web.

> [!NOTE]
> Para obter mais informações sobre o processo de empacotamento e a compilação, consulte [compilação e empacotamento Web Application Projects](building-and-packaging-web-application-projects.md).


O *SetParameters.xml* arquivo é gerado dinamicamente do seu arquivo de projeto de aplicativo web e quaisquer arquivos de configuração dentro de seu projeto. Quando você compila e empacotar seu projeto, o Pipeline de publicação de Web (WPP) detectará automaticamente muitas das variáveis que têm probabilidade de mudar entre ambientes de implantação, como o destino do aplicativo web do IIS e quaisquer cadeias de caracteres de conexão de banco de dados. Esses valores são parametrizados no pacote de implantação da web e adicionados automaticamente à *SetParameters.xml* arquivo. Por exemplo, se você adicionar uma cadeia de caracteres de conexão para o *Web. config* arquivo no seu projeto de aplicativo web, o processo de build detectará essa alteração e adicionará uma entrada para o *SetParameters.xml* arquivo adequadamente.

Em muitos casos, essa parametrização automática será suficiente. No entanto, se os usuários precisam para variar as outras configurações entre ambientes de implantação, como as configurações do aplicativo ou URLs de ponto de extremidade de serviço, você precisará informar o WPP parametrizar esses valores no pacote de implantação e adicionar entradas correspondentes para o *SetParameters.xml* arquivo. As seções a seguir explicam como fazer isso.

### <a name="automatic-parameterization"></a>Parametrização automática

Quando você compilar e empacota um aplicativo web, o WPP parametrizar automaticamente essas coisas:

- O destino do IIS web nome e caminho do aplicativo.
- Qualquer conexão cadeias de caracteres no seu *Web. config* arquivo.
- Cadeias de caracteres de Conexão para qualquer banco de dados que você adicionar à **empacotar/publicar SQL** guia nas páginas de propriedade do projeto.

Por exemplo, se você compilar e empacotar o [Contact Manager](the-contact-manager-solution.md) solução de exemplo sem tocar o processo de parametrização de qualquer forma, o WPP isso geraria *ContactManager.Mvc.SetParameters.xml* arquivo:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


Nesse caso:

- O **nome do aplicativo Web do IIS** parâmetro é o caminho onde você deseja implantar o aplicativo web do IIS. O valor padrão é obtido a **empacotar/Publicar Web** página nas páginas de propriedade do projeto.
- O **cadeia de caracteres de Conexão de Web. config de ApplicationServices** parâmetro foi gerado a partir um **connectionStrings/adicione** elemento no *Web. config* arquivo. Representa a cadeia de caracteres de conexão que o aplicativo deve usar, entre em contato com o banco de dados de associação. O valor que você fornece aqui serão substituídos na implantado *Web. config* arquivo. O valor padrão é obtido do Pre-deployment *Web. config* arquivo.

O WPP também parametriza essas propriedades no pacote de implantação, que ele gera. Você pode fornecer valores para essas propriedades quando você instala o pacote de implantação. Se você instalar o pacote manualmente por meio do Gerenciador do IIS, conforme descrito em [manualmente instalando os pacotes da Web](manually-installing-web-packages.md), o Assistente de instalação solicitará que você forneça valores para todos os parâmetros. Se você instalar o pacote remotamente usando o *. Deploy. cmd* do arquivo, conforme descrito em [Implantando pacotes da Web](deploying-web-packages.md), implantação da Web será a aparência a este *SetParameters.xml* de arquivos para Forneça os valores de parâmetro. Você pode editar os valores de *SetParameters.xml* arquivo manualmente, ou você pode personalizar o arquivo como parte de um processo automatizado de compilação e implantação. Esse processo é descrito em mais detalhes mais adiante neste tópico.

### <a name="custom-parameterization"></a>Parametrização personalizada

Em cenários de implantação mais complexos, você geralmente deseja parametrizar propriedades adicionais antes de implantar seu projeto. Em termos gerais, você deve parametrizar quaisquer propriedades e configurações que variam entre os ambientes de destino. Eles podem incluir:

- Pontos de extremidade de serviço a *Web. config* arquivo.
- Configurações do aplicativo na *Web. config* arquivo.
- Outras propriedades declarativas que você deseja solicitam aos usuários especificar.

A maneira mais fácil para parametrizar dessas propriedades é adicionar um *parameters.xml* arquivo na pasta raiz do seu projeto de aplicativo web. Por exemplo, na solução do Contact Manager, o projeto ContactManager.Mvc inclui um *parameters.xml* arquivo na pasta raiz.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Se você abrir esse arquivo, você verá que ele contém uma única **parâmetro** entrada. A entrada usa uma consulta XML Path Language (XPath) para localizar e parametrizar a URL de ponto de extremidade do serviço no Windows Communication Foundation (WCF) ContactService a *Web. config* arquivo.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Além de parametrizar a URL de ponto de extremidade no pacote de implantação, o WPP também adiciona uma entrada correspondente para o *SetParameters.xml* arquivo gerado junto com o pacote de implantação.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Se você instalar o pacote de implantação manualmente, o Gerenciador do IIS solicitará o endereço do ponto de extremidade de serviço junto com as propriedades que foram parametrizadas automaticamente. Se você instalar o pacote de implantação executando o *. Deploy. cmd* arquivo, você pode editar o *SetParameters.xml* arquivo para fornecer um valor para o endereço do ponto de extremidade de serviço junto com os valores para o propriedades que foram parametrizadas automaticamente.

Para obter detalhes completos sobre como criar uma *parameters.xml* arquivos, consulte [como: usar parâmetros ao configurar as configurações quando um pacote de implantação está instalado](https://msdn.microsoft.com/library/ff398068.aspx). O procedimento chamado **para usar parâmetros de implantação para configurações do arquivo Web. config** fornece instruções passo a passo.

## <a name="modifying-the-setparametersxml-file"></a>Modificando o arquivo SetParameters.xml

Se você planeja implantar o pacote de aplicativo web manualmente&#x2014;seja executando o *. Deploy. cmd* arquivo ou executando MSDeploy.exe da linha de comando&#x2014;não há nada que impeça a editando manualmente o  *SetParameters.xml* arquivo antes da implantação. No entanto, se você estiver trabalhando em uma solução de escala empresarial, você precisa implantar um pacote de aplicativo da web como parte de um processo de compilação e implantação automatizado, maior. Nesse cenário, você precisa que o Microsoft Build Engine (MSBuild) para modificar a *SetParameters.xml* arquivo para você. Você pode fazer isso usando o MSBuild **XmlPoke** tarefa.

O [solução de exemplo do Gerenciador de contatos](the-contact-manager-solution.md) ilustra esse processo. Os exemplos de código a seguir foram editados para mostrar apenas os detalhes que são relevantes para este exemplo.

> [!NOTE]
> Para obter uma visão mais ampla do modelo de arquivo de projeto na solução de exemplo e uma introdução aos arquivos de projeto personalizado em geral, consulte [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](understanding-the-build-process.md).


Primeiro, os valores de parâmetro de interesse são definidos como propriedades no arquivo de projeto específicas do ambiente (por exemplo, *Env Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Para obter orientação sobre como personalizar os arquivos de projeto de ambiente específicas para seus próprios ambientes de servidor, consulte [configurar propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Em seguida, o *Publish.proj* arquivo importa essas propriedades. Porque cada *SetParameters.xml* arquivo está associado com um *. Deploy. cmd* arquivo e podemos, por fim, deseja que o arquivo de projeto para invocar cada *. Deploy. cmd* de arquivo do projeto arquivo cria um MSBuild *item* para cada *. Deploy. cmd* de arquivo e define as propriedades de interesse como *metadados de item*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


Nesse caso:

- O **ParametersXml** metadados valor indica o local do *SetParameters.xml* arquivo.
- O **IisWebAppName** valor é o caminho para o qual você deseja implantar o aplicativo web do IIS.
- O **MembershipDBConnectionString** valor é a cadeia de caracteres de conexão para o banco de dados de associação e o **MembershipDBConnectionName** valor é o **nome** atributo do parâmetro correspondente na *SetParameters.xml* arquivo.
- O **ServiceEndpointValue** valor é o endereço do ponto de extremidade para o serviço WCF no servidor de destino e o **ServiceEndpointParamName** valor é o atributo de nome do parâmetro correspondente no o *SetParameters.xml* arquivo.

Por fim, na *Publish.proj* arquivo, o **PublishWebPackages** destino usa a **XmlPoke** tarefas para modificar esses valores no *SetParameters.xml* arquivo.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Você perceberá que cada **XmlPoke** tarefa especifica quatro valores de atributo:

- O **XmlInputPath** atributo informa à tarefa onde encontrar o arquivo que você deseja modificar.
- O **consulta** atributo é uma consulta XPath que identifica o nó XML que você deseja alterar.
- O **valor** atributo é o novo valor que você deseja inserir no nó selecionado no XML.
- O **condição** atributo é o critério no qual a tarefa deve ou não ser executado. Nesses casos, a condição garante que você não tente inserir um valor nulo ou vazio para o *SetParameters.xml* arquivo.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu a função do *SetParameters.xml* de arquivo e expliquei como ela é gerada quando você compila um projeto de aplicativo web. Ele explicou como é possível parametrizar configurações adicionais, adicionando um *parameters.xml* ao seu projeto. Também descreveu como você pode modificar os *SetParameters.xml* arquivo como parte de um processo de compilação maiores e automatizada, usando o **XmlPoke** tarefa em seus arquivos de projeto.

O próximo tópico, [Implantando pacotes da Web](deploying-web-packages.md), descreve como você pode implantar um pacote da web seja executando o *. Deploy. cmd* do arquivo ou usando MSDeploy.exe comandos diretamente. Em ambos os casos, você pode especificar sua *SetParameters.xml* arquivo como um parâmetro de implantação.

## <a name="further-reading"></a>Leitura adicional

Para obter informações sobre como criar pacotes da web, consulte [compilação e empacotamento Web Application Projects](building-and-packaging-web-application-projects.md). Para obter orientação sobre como implantar, na verdade, um pacote da web, consulte [Implantando pacotes da Web](deploying-web-packages.md). Para obter uma explicação passo a passo sobre como criar uma *parameters.xml* arquivos, consulte [como: usar parâmetros ao configurar as configurações quando um pacote de implantação está instalado](https://msdn.microsoft.com/library/ff398068.aspx).

Para obter mais informações sobre parametrização na implantação da Web, consulte [parametrização de implantação da Web em ação](https://go.microsoft.com/?linkid=9805119) (postagem de blog).

> [!div class="step-by-step"]
> [Anterior](building-and-packaging-web-application-projects.md)
> [Próximo](deploying-web-packages.md)
