---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
title: Implantando pacotes da Web | Microsoft Docs
author: jrjlee
description: Este tópico descreve como você pode publicar pacotes de implantação da web para um servidor remoto usando a ferramenta de implantação do Internet Information Services (IIS) da Web (Web...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a5c5eed2-8683-40a5-a2e1-35c9f8d17c29
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages
msc.type: authoredcontent
ms.openlocfilehash: d7a17a2830ab044f6f7446edc18c394d97b09fa0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830386"
---
<a name="deploying-web-packages"></a>Implantando pacotes da Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como você pode publicar pacotes de implantação da web para um servidor remoto usando a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web) 2.0.
> 
> Há duas maneiras principais no qual você pode implantar um pacote da web a um servidor remoto:
> 
> - Você pode usar o utilitário de linha de comando MSDeploy.exe diretamente.
> - Você pode executar o *[nome do projeto].deploy.cmd* arquivo que gera o processo de compilação.
> 
> O resultado final é a mesma, independentemente de qual abordagem você usa. Essencialmente, todos os *. Deploy. cmd* arquivo faz é executar MSDeploy.exe com alguns valores predeterminados, para que você não precisa fornecer o máximo de informações para implantar o pacote. Isso simplifica o processo de implantação. Por outro lado, usando MSDeploy.exe diretamente oferece muito mais flexibilidade sobre exatamente como o pacote é implantado.
> 
> Abordagem usada dependerá de uma variedade de fatores, incluindo o nível de controle necessário durante o processo de implantação e se você estiver visando o serviço Web implantar o agente remoto ou o manipulador de implantação da Web. Este tópico explica como usar cada abordagem e identifica quando cada abordagem é apropriada.
> 
> As tarefas e instruções passo a passo neste tópico pressupõem que:
> 
> - Você criou e empacotados de seu aplicativo web, conforme descrito em [compilação e empacotamento Web Application Projects](building-and-packaging-web-application-projects.md).
> - Você modificou o *SetParameters.xml* arquivo para fornecer os valores de parâmetro certo para seu ambiente de destino, conforme descrito em [Configurando parâmetros para a implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md).


Executando o [*nome do projeto*]*. Deploy. cmd* arquivo é a maneira mais simples de implantar um pacote da web. Em particular, usando o *. Deploy. cmd* arquivo oferece estas vantagens em relação ao uso MSDeploy.exe diretamente:

- Você não precisa especificar o local do pacote de implantação da web&#x2014;o *. Deploy. cmd* arquivo já sabe onde ele está.
- Você não precisa especificar o local do *SetParameters.xml* arquivo&#x2014;o *. Deploy. cmd* arquivo já sabe onde ele está.
- Você não precisa especificar provedores de MSDeploy de origem e destino&#x2014;o *. Deploy. cmd* arquivo já sabe quais valores a serem usados.
- Você não precisa especificar as configurações de operação do MSDeploy&#x2014;o *. Deploy. cmd* arquivo adiciona automaticamente os valores comumente necessários para o comando MSDeploy.exe.

Antes de usar o *. Deploy. cmd* arquivo para implantar um pacote da web, você deve garantir que:

- O *. Deploy. cmd* arquivo, o [*nome do projeto*]. *SetParameters.xml* arquivo e o pacote da web ([*nome do projeto*]. *ZIP*) estão na mesma pasta.
- A implantação da Web (MSDeploy.exe) está instalada no computador que executa o *. Deploy. cmd* arquivo.

O *. Deploy. cmd* arquivo dá suporte a várias opções de linha de comando. Quando você executar o arquivo em um prompt de comando, esta é a sintaxe básica:


[!code-console[Main](deploying-web-packages/samples/sample1.cmd)]


Você deve especificar uma **/T** sinalizador ou uma **/Y** sinalizador para indicar se deseja executar uma execução de teste ou uma implantação em tempo real, respectivamente (não use ambos os sinalizadores no mesmo comando). Esta tabela explica a finalidade de cada um desses sinalizadores.

| Sinalizador | Descrição |
| --- | --- |
| **/T** | Chama MSDeploy.exe com o **– whatif** sinalizador que indica uma execução de teste. Em vez de implantar o pacote, ele cria um relatório do que ocorreria se você tiver implantado o pacote. |
| **/Y** | Chama MSDeploy.exe sem o **– whatif** sinalizador. Isso implanta o pacote para o computador local ou o servidor de destino especificado. |
| **/M** | Especifica o servidor de destino nome ou URL do serviço. Para obter mais informações sobre os valores que você pode fornecer aqui, consulte a **considerações sobre o ponto de extremidade** neste tópico. Se você omitir a **/M** sinalizador, o pacote será implantado no computador local. |
| **/A** | Especifica o tipo de autenticação que MSDeploy.exe deve usar para executar a implantação. Os valores possíveis são **NTLM** e **básica**. Se você omitir a **/A** sinalizador, o tipo de autenticação padrão é **NTLM** para a implantação para o serviço Web implantar o agente remoto e, ao **básica** para implantação para a implantação da Web Manipulador. |
| **/U** | Especifica o nome de usuário. Isso se aplica apenas se você estiver usando autenticação básica. |
| **/P** | Especifica a senha. Isso se aplica apenas se você estiver usando autenticação básica. |
| **/L** | Indica que o pacote deve ser implantado na instância local do IIS Express. |
| **/G** | Especifica que o pacote é implantado usando o [configuração de provedor tempAgent](https://technet.microsoft.com/library/ee517345(WS.10).aspx). Se você omitir a **/G** sinalizador, o valor padrão de **falso**. |

> [!NOTE]
> Sempre que o processo de compilação cria um pacote da web, ele também cria um arquivo chamado *[nome do projeto] Readme. txt. Deploy* que explica essas opções de implantação.


Além desses sinalizadores, você pode especificar as configurações de implantação da Web de operação como adicionais *. Deploy. cmd* parâmetros. As configurações adicionais que você especificar simplesmente são passadas para o comando MSDeploy.exe subjacente. Para obter mais informações sobre essas configurações, consulte [Web implantar as configurações de operação](https://technet.microsoft.com/library/dd569089(WS.10).aspx).

Suponha que você deseja implantar o projeto de aplicativo web ContactManager.Mvc em um ambiente de teste executando o *. Deploy. cmd* arquivo. Seu ambiente de teste está configurado para usar o serviço de agente remoto da Web implantar, conforme descrito em [configurar um servidor Web para publicação de implantação do Web (agente remoto)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-remote-agent.md). Para implantar o aplicativo web, você precisará concluir as próximas etapas.

**Para implantar um aplicativo web usando o. arquivo Deploy.**

1. Compile e empacote o projeto de aplicativo web, conforme descrito em [compilação e empacotamento Web Application Projects](building-and-packaging-web-application-projects.md).
2. Modificar a *ContactManager.Mvc.SetParameters.xml* arquivo para conter os valores de parâmetro corretos para seu ambiente de teste, conforme descrito em [Configurando parâmetros para a implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md).
3. Abra uma janela de Prompt de comando e navegue até o local do *ContactManager.Mvc.deploy.cmd* arquivo.
4. Digite o seguinte comando e pressione Enter:

    [!code-console[Main](deploying-web-packages/samples/sample2.cmd)]

Neste exemplo:

- O **/Y** sinalizador indica que você deseja implantar, na verdade, o pacote, em vez de fazer uma avaliação de execução.
- O **/M** sinalizador indica que você deseja implantar o pacote em um servidor chamado TESTWEB1. A partir desse valor, MSDeploy.exe tentar implantar o pacote para o serviço Web implantar o agente remoto no http://TESTWEB1/MSDeployAgentService.
- O **/A** sinalizador indica que você deseja usar a autenticação NTLM. Como tal, você não precisa especificar um nome de usuário e senha.

Para ilustrar como usar o *. Deploy. cmd* arquivo simplifica o processo de implantação, dê uma olhada no que obtém gerado e executado quando você executa o comando MSDeploy.exe *ContactManager.Mvc.deploy.cmd* usando as opções mostradas acima.


[!code-console[Main](deploying-web-packages/samples/sample3.cmd)]


Para obter mais informações sobre como usar o *. Deploy. cmd* arquivo para implantar um pacote da web, consulte [como: instalar uma implantação de pacote usando o arquivo Deploy](https://msdn.microsoft.com/library/ff356104.aspx).

## <a name="using-msdeployexe"></a>Usando MSDeploy.exe

Embora o uso de *. Deploy. cmd* arquivo geralmente simplifica o processo de implantação, há algumas situações, quando é preferível usar MSDeploy.exe diretamente. Por exemplo:

- Se você deseja implantar para o manipulador de implantação da Web como um usuário não administrador, você não poderá usar o *. Deploy. cmd* arquivo. Isso é devido a um bug na Web Deploy 2.0, conforme descrito em **considerações sobre o ponto de extremidade**.
- Se você quiser mudar manualmente entre diferentes *SetParameters.xml* arquivos em locais diferentes, talvez você prefira usar MSDeploy.exe diretamente.
- Se você quiser substituir vários argumentos de linha de comando MSDeploy.exe, você pode preferir usar MSDeploy.exe diretamente.

Quando você usa MSDeploy.exe, você precisa fornecer três informações cruciais:

- Um **– origem** parâmetro que indica onde seus dados estão vindo.
- Um **dest –** parâmetro que indica onde será seus dados.
- Um **– verbo** parâmetro que indica a [operação](https://technet.microsoft.com/library/dd568989(WS.10).aspx) você deseja executar.

Depende de MSDeploy.exe [provedores de implantação da Web](https://technet.microsoft.com/library/dd569040(WS.10).aspx) para processar dados de origem e de destino. A implantação da Web inclui vários provedores que representam o intervalo de fontes de dados e aplicativos que ele pode funcionar com&#x2014;por exemplo, há provedores para bancos de dados do SQL Server, servidores web IIS, certificados e assemblies do GAC (cache) de assembly global, várias arquivos de configuração diferentes e muitos outros tipos de dados. Ambos os o **– código-fonte** parâmetro e o **– dest** parâmetro deve especificar um provedor, no formulário **– origem**: [*providerName*] = [*local*]. Quando você estiver implantando um pacote da web para um site do IIS, você deve usar esses valores:

- O **– código-fonte** provedor está sempre [pacote](https://technet.microsoft.com/library/dd569019(WS.10).aspx). Por exemplo:

    [!code-console[Main](deploying-web-packages/samples/sample4.cmd)]
- O **– dest** provedor está sempre [automática](https://technet.microsoft.com/library/dd569016(WS.10).aspx). Por exemplo:

    [!code-console[Main](deploying-web-packages/samples/sample5.cmd)]
- O **– verbo** é sempre **sincronização**.

    [!code-console[Main](deploying-web-packages/samples/sample6.cmd)]

Além disso, você precisará especificar vários outros [configurações específicas do provedor](https://technet.microsoft.com/library/dd569001(WS.10).aspx) e gerais [as configurações de operação](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Por exemplo, suponha que você deseja implantar o aplicativo da web de ContactManager.Mvc em um ambiente de preparo. A implantação será o manipulador de implantação da Web de destino e deve usar a autenticação básica. Para implantar o aplicativo web, você precisará concluir as próximas etapas.

**Para implantar um aplicativo web usando MSDeploy.exe**

1. Compile e empacote o projeto de aplicativo web, conforme descrito em [compilação e empacotamento Web Application Projects](building-and-packaging-web-application-projects.md).
2. Modificar a *ContactManager.Mvc.SetParameters.xml* arquivo para conter os valores de parâmetro corretos para seu ambiente de preparo, conforme descrito em [Configurando parâmetros para a implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md).
3. Abra uma janela de Prompt de comando e navegue até o local do MSDeploy.exe. Isso é geralmente em %PROGRAMFILES%\IIS\Microsoft Web implantar V2\msdeploy.exe.
4. Digite o seguinte comando e pressione Enter (ignore as quebras de linha):

    [!code-console[Main](deploying-web-packages/samples/sample7.cmd)]

Neste exemplo:

- O **– código-fonte** parâmetro especifica o **pacote** provedor e indica o local do pacote da web.
- O **– dest** parâmetro especifica o **automático** provedor. O **computerName** configuração fornece a URL do serviço do manipulador de implantar a Web no servidor de destino. O **authtype** configuração indica que você deseja usar a autenticação básica, e como tal, você precisa fornecer um **nome de usuário** e um **senha**. Por fim, o **includeAcls = "False"** configuração indica que você não deseja copiar as listas de controle de acesso (ACLs) dos arquivos em seu aplicativo web de origem para o servidor de destino.
- O **– verbo: sincronização** argumento indica que você deseja replicar o conteúdo de origem no servidor de destino.
- O **– disableLink** argumentos indicam que você não deseja replicar os pools de aplicativos, a configuração de diretório virtual ou certificados de Secure Sockets Layer (SSL) no servidor de destino. Para obter mais informações, consulte [Web implantar extensões de Link](https://technet.microsoft.com/library/dd569028(WS.10).aspx).
- O **– setParamFile** parâmetro fornece o local do *SetParameters.xml* arquivo.
- O **– allowUntrusted** opção indica que a implantação da Web deve aceitar certificados SSL que não foram emitidos por uma autoridade de certificação confiável. Se você estiver implantando para o manipulador de implantação da Web, e você usou um certificado autoassinado para proteger a URL do serviço, você precisa incluir essa opção.

## <a name="automating-web-package-deployment"></a>Automatizando a implantação de pacote da Web

Em muitos cenários empresariais, você vai querer implantar seus pacotes de web como parte de uma única etapa maior ou implantação automatizada. Independentemente de escolher implantar seus pacotes de web executando o *. Deploy. cmd* de arquivo ou usando MSDeploy.exe diretamente, você pode parametrizar seus comandos e chamá-las de um destino em um Microsoft Build Engine (MSBuild) arquivo de projeto.

Na solução de exemplo Contact Manager, dê uma olhada a **PublishWebPackages** de destino na *Publish.proj* arquivo. Este destino será executado uma vez para cada *. Deploy. cmd* arquivo identificado por uma lista de itens chamada **PublishPackages**. O destino usa propriedades e metadados de item para criar um conjunto completo de valores de argumento para cada *. Deploy. cmd* arquivo e, em seguida, usa o **Exec** tarefa para executar o comando.


[!code-xml[Main](deploying-web-packages/samples/sample8.xml)]


> [!NOTE]
> Para obter uma visão mais ampla do modelo de arquivo de projeto na solução de exemplo e uma introdução aos arquivos de projeto personalizado em geral, consulte [Noções básicas sobre o arquivo de projeto](understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](understanding-the-build-process.md).


## <a name="endpoint-considerations"></a>Considerações sobre o ponto de extremidade

Independentemente se você implanta o pacote da web executando o *. Deploy. cmd* do arquivo ou usando MSDeploy.exe diretamente, você precisa especificar um nome de computador ou um ponto de extremidade de serviço para sua implantação.

Se o servidor web de destino está configurado para implantação usando o serviço Web implantar o agente remoto, você especifica a URL do serviço de destino como seu destino.


[!code-console[Main](deploying-web-packages/samples/sample9.cmd)]


Como alternativa, você pode especificar somente o nome de servidor como seu destino, e implantação da Web irá inferir a URL do serviço de agente remoto.


[!code-console[Main](deploying-web-packages/samples/sample10.cmd)]


Se o servidor web de destino está configurado para implantação usando o manipulador de implantação da Web, você precisa especificar o endereço do ponto de extremidade do serviço de gerenciamento do IIS da Web (WMSvc) como seu destino. Por padrão, isso assume a forma:


[!code-console[Main](deploying-web-packages/samples/sample11.cmd)]


Você pode direcionar qualquer um desses pontos de extremidade usando o *. Deploy. cmd* MSDeploy.exe diretamente ou arquivo. No entanto, se você quiser implantar o manipulador de implantação da Web como um usuário não administrador, conforme descrito em [configurar um servidor Web para publicação de implantação do Web (manipulador de implantação da Web)](../configuring-server-environments-for-web-deployment/configuring-a-web-server-for-web-deploy-publishing-web-deploy-handler.md), você precisa adicionar uma cadeia de caracteres de consulta para o endereço do ponto de extremidade de serviço.


[!code-console[Main](deploying-web-packages/samples/sample12.cmd)]


Isso ocorre porque o usuário não administrador não tem acesso de nível de servidor no IIS; ele só tem acesso a um site do IIS específico. No momento da escrita, devido a um bug no Web Publishing Pipeline (WPP), você não pode executar o *. Deploy. cmd* de arquivos usando um endereço de ponto de extremidade que inclui uma cadeia de caracteres de consulta. Nesse cenário, você precisará implantar o pacote da web usando MSDeploy.exe diretamente.

> [!NOTE]
> Para obter mais informações sobre o serviço Web implantar o agente remoto e o manipulador de implantação da Web, consulte [escolhendo a abordagem da direita para a implantação da Web](../configuring-server-environments-for-web-deployment/choosing-the-right-approach-to-web-deployment.md). Para obter orientação sobre como configurar seus arquivos de projeto específicas do ambiente para implantar esses pontos de extremidade, consulte [configurar propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="authentication-considerations"></a>Considerações sobre autenticação

Independentemente se você implanta o pacote da web executando o *. Deploy. cmd* do arquivo ou usando MSDeploy.exe diretamente, você precisa especificar um tipo de autenticação. A implantação da Web aceita dois valores possíveis: **NTLM** ou **básica**. Se você especificar a autenticação básica, você também precisará fornecer um nome de usuário e senha. Há vários fatores que você precisa estar ciente de quando você seleciona um tipo de autenticação:

- Se você estiver implantando o serviço Web implantar o agente remoto, você deve usar a autenticação NTLM. O serviço de agente remoto não aceitar credenciais de autenticação básica.
- Se você estiver implantando o manipulador de implantação da Web, você pode usar a autenticação básica ou NTLM. A configuração padrão é a autenticação básica. Embora a autenticação básica se baseia em nomes de usuário e as senhas sejam transmitidas em texto sem formatação, suas credenciais são protegidas, como o manipulador de implantação da Web sempre usa criptografia SSL.
- Se o pacote da web inclui um banco de dados e o servidor web e o servidor de banco de dados forem computadores separados, não será capaz de implantar o banco de dados usando a autenticação NTLM devido à [limitação NTLM "salto duplo"](https://go.microsoft.com/?linkid=9805120). Você precisa usar credenciais do SQL Server em sua cadeia de conexão de implantação ou fornecer credenciais de autenticação básica para a implantação da Web. Esse problema é descrito mais detalhadamente [implantando bancos de dados de associação para ambientes empresariais](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como você pode implantar um pacote da web seja executando o *. Deploy. cmd* arquivo ou usando MSDeploy.exe diretamente. Ele explicou quando cada abordagem pode ser apropriada e descreveu como você pode parametrizar e executar um comando de implantação como parte de um processo de compilação passo a passo ou automatizado maior.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como criar e parametrizar um pacote de implantação da web, consulte [compilação e empacotamento Web Application Projects](building-and-packaging-web-application-projects.md) e [Configurando parâmetros para a implantação de pacote da Web](configuring-parameters-for-web-package-deployment.md). Para obter orientação sobre como criar e implantar pacotes de web de uma instância do Team Foundation Server (TFS), consulte [Configurando o Team Foundation Server para a implantação automatizada de Web](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md). Para obter informações sobre como personalizar e solucionar problemas do processo de implantação, consulte [excluindo arquivos e pastas de implantação](../advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment.md).

> [!div class="step-by-step"]
> [Anterior](configuring-parameters-for-web-package-deployment.md)
> [Próximo](deploying-database-projects.md)
