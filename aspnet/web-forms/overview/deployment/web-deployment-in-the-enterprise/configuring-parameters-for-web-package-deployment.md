---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Configurar parâmetros para a implantação do pacote da Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como definir valores de parâmetro, como nomes de aplicativos da web de serviços de informações da Internet (IIS), cadeias de caracteres de conexão e pontos de extremidade de serviço...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7be08f1a1fb7232911a44cf64e2e784dbb95ff48
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="configuring-parameters-for-web-package-deployment"></a>Configurar parâmetros para a implantação do pacote da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como definir valores de parâmetro, como nomes de aplicativos da web de serviços de informações da Internet (IIS), cadeias de caracteres de conexão e pontos de extremidade de serviço, ao implantar um pacote da web em um servidor de web IIS remoto.


Quando você cria um projeto de aplicativo web, a compilação e o processo de empacotamento gera três arquivos de chave:

- Um *[nome do projeto]. zip* arquivo. Este é o pacote de implantação da web para o seu projeto de aplicativo web. Este pacote contém todos os assemblies, arquivos, scripts de banco de dados e recursos necessários para recriar seu aplicativo da web em um servidor de web IIS remoto.
- Um *[nome do projeto].deploy.cmd* arquivo. Contém um conjunto de comandos com parâmetros de implantação da Web (MSDeploy.exe) que publica o pacote de implantação da web em um servidor de web IIS remoto.
- Um *[nome do projeto]. SetParameters.xml* arquivo. Isso fornece um conjunto de valores de parâmetro para o comando MSDeploy.exe. Você pode atualizar os valores no arquivo e passá-lo para implantação da Web como um parâmetro de linha de comando quando você implanta o pacote da web.

> [!NOTE]
> Para obter mais informações sobre a criação e o processo de empacotamento, consulte [criação e a projetos de aplicativo Web de empacotamento](building-and-packaging-web-application-projects.md).


O *SetParameters.xml* arquivo é gerado dinamicamente de seu arquivo de projeto de aplicativo web e os arquivos de configuração dentro de seu projeto. Quando você cria e empacotar seu projeto, o Pipeline de publicação de Web (WPP) detectará automaticamente muitas das variáveis que provavelmente serão alterados entre ambientes de implantação, como o destino do aplicativo web do IIS e qualquer cadeia de caracteres de conexão do banco de dados. Esses valores são parametrizados no pacote de implantação da web e adicionados automaticamente ao *SetParameters.xml* arquivo. Por exemplo, se você adicionar uma cadeia de caracteres de conexão para o *Web. config* arquivo no seu projeto de aplicativo web, o processo de compilação detectará essa alteração e adicione uma entrada para o *SetParameters.xml* arquivo adequadamente.

Em muitos casos, essa parametrização automática será suficiente. No entanto, se os usuários precisam para variar as outras configurações entre ambientes de implantação, como configurações de aplicativo ou URLs de ponto de extremidade de serviço, você precisa saber o WPP parametrizar esses valores no pacote de implantação e adicionar entradas correspondentes para o *SetParameters.xml* arquivo. As seções a seguir explicam como fazer isso.

### <a name="automatic-parameterization"></a>Parametrização automática

Quando você criar e empacota um aplicativo web, o WPP será automaticamente parametrizar estas coisas:

- O destino do IIS da web nome e caminho do aplicativo.
- Qualquer conexão cadeias de caracteres em seu *Web. config* arquivo.
- Cadeias de caracteres de Conexão para bancos de dados você adicionar à **pacote/publicar SQL** guia nas páginas de propriedades do projeto.

Por exemplo, se você criar e empacotar o [Contact Manager](the-contact-manager-solution.md) solução de exemplo sem tocar o processo de parametrização de qualquer forma, o WPP isso geraria *ContactManager.Mvc.SetParameters.xml* arquivo:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


Nesse caso:

- O **nome do aplicativo Web do IIS** parâmetro é o caminho onde você deseja implantar o aplicativo web do IIS. O valor padrão é obtido do **pacote/publicar na Web** página nas páginas de propriedades do projeto.
- O **cadeia de caracteres de Conexão Web. config ApplicationServices** parâmetro foi gerado a partir um **connectionStrings ou adicionar** elemento o *Web. config* arquivo. Representa a cadeia de caracteres de conexão que o aplicativo deve usar para entrar em contato com o banco de dados de associação. O valor que você fornecer aqui serão substituídos em implantado *Web. config* arquivo. O valor padrão é obtido de pré-implantação *Web. config* arquivo.

O WPP também parametriza essas propriedades no pacote de implantação, que ele gera. Você pode fornecer valores para essas propriedades quando você instala o pacote de implantação. Se você instalar o pacote manualmente pelo Gerenciador do IIS, conforme descrito em [instalar pacotes de Web manualmente](manually-installing-web-packages.md), o Assistente de instalação solicitará que você forneça valores para os parâmetros. Se você instalar o pacote remotamente usando o *. Deploy* de arquivos, conforme descrito em [Implantando pacotes de Web](deploying-web-packages.md), implantação da Web será exibida para esse *SetParameters.xml* de arquivos para Forneça os valores de parâmetro. Você pode editar os valores de *SetParameters.xml* arquivo manualmente, ou você pode personalizar o arquivo como parte de um processo de compilação e implantação automatizado. Esse processo é descrito em mais detalhes posteriormente neste tópico.

### <a name="custom-parameterization"></a>Parametrização personalizada

Em cenários de implantação mais complexos, você geralmente deseja parametrizar propriedades adicionais antes de implantar seu projeto. Em geral, você deve parametrizar quaisquer propriedades e configurações que podem variar entre os ambientes de destino. Eles podem incluir:

- Em pontos de extremidade de serviço a *Web. config* arquivo.
- Configurações do aplicativo no *Web. config* arquivo.
- Outras propriedades declarativas que você deseja solicitar aos usuários especificar.

É a maneira mais fácil para parametrizar essas propriedades adicionar um *parameters.xml* arquivo para a pasta raiz do seu projeto de aplicativo web. Por exemplo, na solução Contact Manager, o projeto ContactManager.Mvc inclui um *parameters.xml* arquivo na pasta raiz.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Se você abrir esse arquivo, você verá que ele contém um único **parâmetro** entrada. A entrada usa uma consulta XML Path Language (XPath) para localizar e parametrizar a URL de ponto de extremidade do serviço no Windows Communication Foundation (WCF) ContactService o *Web. config* arquivo.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Além de parametrização de URL de ponto de extremidade no pacote de implantação, o WPP também adiciona uma entrada correspondente para o *SetParameters.xml* arquivo gerado junto com o pacote de implantação.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Se você instalar manualmente o pacote de implantação, o Gerenciador do IIS solicitará o endereço do ponto de extremidade de serviço junto com as propriedades que foram parametrizadas automaticamente. Se você instalar o pacote de implantação, executando o *. Deploy* arquivo, você pode editar o *SetParameters.xml* arquivo para fornecer um valor para o endereço do ponto de extremidade de serviço junto com valores para o propriedades que foram parametrizadas automaticamente.

Para obter detalhes completos sobre como criar um *parameters.xml* de arquivos, consulte [como: Use parâmetros para configurar as configurações quando um pacote de implantação está instalado](https://msdn.microsoft.com/library/ff398068.aspx). O procedimento chamado **usar parâmetros de implantação para configurações do arquivo Web. config** fornece instruções passo a passo.

## <a name="modifying-the-setparametersxml-file"></a>Modificando o arquivo SetParameters.xml

Se você planeja implantar o pacote de aplicativo web manualmente&#x2014;executando o *. Deploy* arquivo ou ao executar o MSDeploy.exe da linha de comando&#x2014;não há nada que impeça a edição manual de  *SetParameters.xml* arquivo antes da implantação. No entanto, se você estiver trabalhando em uma solução de grande porte, você precisará implantar um pacote de aplicativo da web como parte de um processo de compilação e implantação automatizado, maior. Nesse cenário, é necessário o Microsoft Build Engine (MSBuild) para modificar o *SetParameters.xml* arquivo para você. Você pode fazer isso usando o MSBuild **XmlPoke** tarefa.

O [solução de exemplo do Gerenciador de contato](the-contact-manager-solution.md) ilustra esse processo. Os exemplos de código a seguirem foram editados para mostrar apenas os detalhes que são relevantes para este exemplo.

> [!NOTE]
> Para obter uma visão mais ampla sobre o modelo de arquivo de projeto na solução de exemplo e uma introdução aos arquivos de projeto personalizados em geral, consulte [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](understanding-the-build-process.md).


Primeiro, os valores de parâmetro de interesse são definidos como propriedades no arquivo de projeto específico de ambiente (por exemplo, *Dev.proj Env*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Para obter orientação sobre como personalizar os arquivos de projeto específico de ambiente para seus próprios ambientes de servidor, consulte [configurar propriedades de implantação de um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Em seguida, o *Publish.proj* arquivo importa essas propriedades. Como cada *SetParameters.xml* arquivo está associado um *. Deploy* arquivo e é, por fim, desejar que o arquivo de projeto para invocar cada *. Deploy* de arquivo do projeto arquivo cria um MSBuild *item* para cada *. Deploy* de arquivo e define as propriedades de interesse como *metadados de item*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


Nesse caso:

- O **ParametersXml** metadados valor indica o local do *SetParameters.xml* arquivo.
- O **IisWebAppName** valor é o caminho do IIS para o qual você deseja implantar o aplicativo web.
- O **MembershipDBConnectionString** valor é a cadeia de caracteres de conexão para o banco de dados de associação e o **MembershipDBConnectionName** valor é o **nome** atributo o parâmetro correspondente no *SetParameters.xml* arquivo.
- O **ServiceEndpointValue** valor é o endereço de ponto de extremidade para o serviço WCF no servidor de destino e o **ServiceEndpointParamName** valor é o atributo de nome do parâmetro correspondente no o *SetParameters.xml* arquivo.

Por fim, no *Publish.proj* arquivo, o **PublishWebPackages** de destino usa o **XmlPoke** tarefas para modificar esses valores no *SetParameters.xml* arquivo.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Você observará que cada **XmlPoke** tarefa especifica quatro valores de atributo:

- O **XmlInputPath** atributo informa a tarefa onde encontrar o arquivo que você deseja modificar.
- O **consulta** atributo é uma consulta XPath que identifica o nó XML que você deseja alterar.
- O **valor** atributo é o novo valor que você deseja inserir o nó XML selecionado.
- O **condição** atributo é o critério no qual a tarefa deve executar ou não executar. Nesses casos, a condição garante que você não tente inserir um valor nulo ou vazio para o *SetParameters.xml* arquivo.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu a função da *SetParameters.xml* de arquivo e explicou como ele é gerado quando você compila um projeto de aplicativo web. Ele explicado como é possível parametrizar configurações adicionais, adicionando um *parameters.xml* ao seu projeto. Também descritos como você pode modificar o *SetParameters.xml* arquivo como parte de um processo de compilação automatizado maior, usando o **XmlPoke** tarefas nos arquivos de projeto.

O próximo tópico, [Implantando pacotes de Web](deploying-web-packages.md), descreve como você pode implantar um pacote da web executando o *. Deploy* arquivo ou usando o MSDeploy.exe comandos diretamente. Em ambos os casos, você pode especificar seu *SetParameters.xml* arquivo como um parâmetro de implantação.

## <a name="further-reading"></a>Leitura adicional

Para obter informações sobre como criar pacotes da web, consulte [criação e a projetos de aplicativo Web de empacotamento](building-and-packaging-web-application-projects.md). Para obter orientação sobre como implantar, na verdade, um pacote da web, consulte [Implantando pacotes de Web](deploying-web-packages.md). Para obter uma explicação passo a passo sobre como criar um *parameters.xml* de arquivos, consulte [como: Use parâmetros para configurar as configurações quando um pacote de implantação está instalado](https://msdn.microsoft.com/library/ff398068.aspx).

Para obter mais informações sobre parametrização na implantação da Web, consulte [Web implantar parametrização na ação](https://go.microsoft.com/?linkid=9805119) (postagem do blog).

> [!div class="step-by-step"]
> [Anterior](building-and-packaging-web-application-projects.md)
> [Próximo](deploying-web-packages.md)
