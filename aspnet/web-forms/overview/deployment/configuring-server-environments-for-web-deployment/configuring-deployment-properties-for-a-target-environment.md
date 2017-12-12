---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
title: "Configurando propriedades de implantação para um ambiente de destino | Microsoft Docs"
author: jrjlee
description: "Este tópico descreve como configurar propriedades específicas do ambiente para implantar a solução de exemplo Contact Manager em um ambiente de destino específico..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b5b86e03-b8ed-46e6-90fa-e1da88ef34e9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment
msc.type: authoredcontent
ms.openlocfilehash: f27b8376b332ff21185be0fd5c00ced7d40a20bd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="configuring-deployment-properties-for-a-target-environment"></a>Configurando propriedades de implantação para um ambiente de destino
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como configurar propriedades específicas do ambiente para implantar a solução de exemplo Contact Manager em um ambiente de destino específico.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo & #x 2014; o [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) #x 2014; & solução para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, Windows Serviço do Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md), em que o processo de compilação é controlado por dois arquivos & #x 2014; projeto contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="process-overview"></a>Visão geral do processo

O arquivo de projeto que você usará para criar e implantar a solução de Gerenciador de contato é dividido em dois arquivos físicos:

- Um que contém universal configurações e instruções de compilação (o *Publish.proj* arquivo).
- Configurações de compilação que contenha específico do ambiente (*Env-Dev.proj*, *Stage.proj Env*, e assim por diante).

No momento da compilação, o arquivo de projeto específico do ambiente apropriado é mesclado no universal *Publish.proj* arquivo para formar um conjunto completo de instruções de compilação. Você pode configurar a implantação em ambientes de destino específico ao criar ou personalizar os arquivos de projeto específico de ambiente com configurações que descrevem seu próprio cenário de implantação.

Muitos desses valores são determinados pelo como seu ambiente de destino está configurado e #x 2014; em particular, se seu servidor web de destino está configurado para usar o serviço de agente de implantação da Web (o agente remoto) ou o manipulador de implantação da Web. Para obter mais informações sobre essas abordagens e orientação sobre como escolher a abordagem certa para seu ambiente, consulte [optar pela abordagem da direita para a implantação da Web](choosing-the-right-approach-to-web-deployment.md).

O [cenário do gerente do contato](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md) requer dois arquivos de projeto específico do ambiente:

- Implantação em um ambiente de teste do desenvolvedor (*Dev.proj Env*). O ambiente de teste do desenvolvedor está configurado para aceitar a implantações remotas usando o agente remoto, conforme descrito em [cenário: configurar um ambiente de teste para implantação de Web](scenario-configuring-a-test-environment-for-web-deployment.md). Esse arquivo precisa fornecer o agente remoto endereço de ponto de extremidade, bem como configurações de local específico como cadeias de caracteres de conexão e pontos de extremidade de serviço.
- Implantação em um ambiente de preparo (*Stage.proj Env*). O ambiente de preparo está configurado para aceitar a implantações remotas usando o manipulador de implantação da Web, conforme descrito em [cenário: configurar um ambiente de preparo para a implantação da Web](scenario-configuring-a-staging-environment-for-web-deployment.md). Este arquivo deve fornecer o endereço de ponto de extremidade do manipulador de implantação da Web, bem como as configurações específicas do local como cadeias de caracteres de conexão e os pontos de extremidade de serviço.

É importante observar que as configurações definidas no arquivo de projeto específico de ambiente não afetam o conteúdo da web do pacote em si & #x 2014; em vez disso, eles controlam como o pacote é implantado e quais valores de parâmetro são fornecidos quando o pacote é extraído. Você está importando o pacote da web para o ambiente de produção manualmente, conforme descrito em [cenário: configurar um ambiente de produção para implantação de Web](scenario-configuring-a-production-environment-for-web-deployment.md) e [instalar pacotes de Web manualmente](../web-deployment-in-the-enterprise/manually-installing-web-packages.md), Portanto, não importa quais configurações usado no arquivo de projeto específico do ambiente ao gerar o pacote. Serviços de informações da Internet (IIS) Manager solicitará quaisquer valores com parâmetros, como cadeias de caracteres de conexão e pontos de extremidade de serviço, quando você importar o pacote.

Para implantar a solução de Gerenciador de contato a seu próprio ambiente de destino, você pode personalizar esse arquivo ou usá-lo como um modelo e criar seu próprio arquivo.

**Para definir configurações específicas do ambiente de implantação para a solução de Gerenciador de contato**

1. Abra a solução de ContactManager WCF no Visual Studio 2010.
2. No **Solution Explorer** janela, expanda o **publicar** pasta, expanda o **EnvConfig** pasta e, em seguida, clique duas vezes em **Env-Dev.proj**.

    ![](configuring-deployment-properties-for-a-target-environment/_static/image1.png)
3. Substitua os valores de propriedade no *Dev.proj Env* arquivo com os valores corretos para seu próprio ambiente de teste.

    > [!NOTE]
    > A tabela a seguir esse procedimento fornece mais informações sobre cada uma dessas propriedades.
4. Salve seu trabalho e, em seguida, feche o *Dev.proj Env* arquivo.

## <a name="choosing-the-right-deployment-properties"></a>Escolha as propriedades de implantação certa

Esta tabela descreve a finalidade de cada propriedade no arquivo de projeto específico do ambiente de exemplo, *Dev.proj Env*e fornece algumas diretrizes sobre os valores que você deve fornecer.

| Nome da Propriedade | Detalhes |
| --- | --- |
| **MSDeployComputerName** o nome do destino da web server ou o serviço de ponto de extremidade. | Se você estiver implantando o serviço de agente remoto no servidor web de destino, você pode especificar o nome do computador de destino (por exemplo, **TESTWEB1** ou **TESTWEB1.fabrikam.net**), ou você pode especificar o controle remoto ponto de extremidade do agente (por exemplo, `http://TESTWEB1/MSDEPLOYAGENTSERVICE`). A implantação funciona da mesma maneira em cada caso. Se você estiver implantando o manipulador de implantação da Web no servidor web de destino, você deve especificar o ponto de extremidade de serviço e incluir o nome do site do IIS como um parâmetro de cadeia de caracteres de consulta (por exemplo, `https://STAGEWEB1:8172/MSDeploy.axd?site=DemoSite`). |
| **MSDeployAuth** o método de implantação da Web deve usar para autenticar o computador remoto. | Isso deve ser definido como **NTLM** ou **básica**. Normalmente, você usará **NTLM** se você estiver implantando o serviço de agente remoto e **básica** se você estiver implantando para o manipulador de implantação da Web. Se você usar a autenticação básica, você também precisa especificar o nome de usuário e senha que a ferramenta de implantação da Web de IIS (implantação da Web) deve representar a fim de executar a implantação. Neste exemplo, esses valores são fornecidos por meio de **MSDeployUsername** e **MSDeployPassword** propriedades. Se você usar a autenticação NTLM, você pode omitir essas propriedades ou deixá-los em branco. |
| **MSDeployUsername** se você usar a autenticação básica, a implantação da Web usar essa conta no computador remoto. | Isso deve ter o formato *domínio*\*nome de usuário * (por exemplo, **FABRIKAM\matt**). Esse valor é usado somente se você especificar a autenticação básica. Se você usar a autenticação NTLM, a propriedade pode ser omitida. Se um valor for fornecido, ele será ignorado. |
| **MSDeployPassword** se você usar a autenticação básica, a implantação da Web usar essa senha no computador remoto. | Esta é a senha da conta de usuário especificada no **MSDeployUsername** propriedade. Esse valor é usado somente se você especificar a autenticação básica. Se você usar a autenticação NTLM, a propriedade pode ser omitida. Se um valor for fornecido, ele será ignorado. |
| **ContactManagerIisPath** caminho o IIS no qual você deseja implantar o aplicativo MVC gerente do contato. | Isso deve ser o caminho como ele aparece no Gerenciador do IIS, no formato [*nome do site IIS*] / [*web**nome do aplicativo*]. Lembre-se de que o site do IIS precisa existir antes de implantar seu aplicativo. Por exemplo, se você tiver criado um site do IIS denominado DemoSite, você pode especificar o caminho do IIS para o aplicativo MVC como DemoSite/ContactManager. |
| **ContactManagerServiceIisPath** caminho o IIS no qual você deseja implantar o serviço WCF do gerente do contato. | Por exemplo, se você tiver criado um site do IIS denominado DemoSite, você pode especificar o caminho do IIS para o serviço do WCF como **DemoSite/ContactManagerService**. |
| **ContactManagerTargetUrl** a URL na qual o serviço WCF pode ser acessado. | Isso terá o formato [*URL de raiz do site IIS*] / [*nome do aplicativo de serviço*] / [*ponto de extremidade de serviço*]. Por exemplo, se você tiver criado um site do IIS na porta 85, a URL seria assumem a forma `http://localhost:85/ContactManagerService/ContactService.svc`. Lembre-se de que o aplicativo MVC e o serviço WCF são implantados no mesmo servidor. Como resultado, esta URL só é acessado do computador no qual ele está instalado. Por isso, é melhor usar localhost ou o endereço IP, em vez do nome do computador ou um cabeçalho de host na URL. Se você usar o nome do computador ou um cabeçalho de host, o [verificação de loopback](https://go.microsoft.com/?linkid=9805131) recurso de segurança do IIS pode bloquear a URL e retornar um **HTTP 401.1 - não autorizado** erro. |
| **CmDatabaseConnectionString** a cadeia de caracteres de conexão para o servidor de banco de dados. | A cadeia de caracteres de conexão determina as credenciais que VSDBCMD será usado para contatar o servidor de banco de dados e criar o banco de dados e as credenciais que o pool de aplicativos de servidor web usará para entrar em contato com o servidor de banco de dados e interagir com o banco de dados. Essencialmente você tem duas opções. Você pode especificar **segurança integrada = true**, caso em que a autenticação integrada do Windows é usada: **fonte de dados = TESTDB1; Integrated Security = true** nesse caso, o banco de dados será criado usando as credenciais do usuário que executa o VSDBCMD executável e o aplicativo acessará o banco de dados usando a identidade da conta de computador do servidor web. Como alternativa, você pode especificar o nome de usuário e a senha de uma conta do SQL Server. Nesse caso, as credenciais do SQL Server são usadas por VSDBCMD para criar o banco de dados e pelo pool de aplicativos para interagir com o banco de dados: **Data Source = TESTDB1; Id de usuário = ASqlUser; Senha = Pa$ $w0rd** explicações passo a passo neste tópico supõem que você usará a autenticação integrada do Windows. |
| **CmTargetDatabase** o nome que você deseja atribuir o banco de dados será criada no servidor de banco de dados. | O valor que você fornecer aqui é adicionado ao comando VSDBCMD como um parâmetro. Ele também é usado para criar uma cadeia de conexão completa o pool de aplicativos no servidor web pode usar para interagir com o banco de dados. |
  

Estes exemplos mostram como você pode configurar essas propriedades para cenários de implantação específico.

### <a name="example-1x2014deployment-to-the-remote-agent-service"></a>Exemplo 1 & #x 2014; implantação para o serviço de agente remoto

Neste exemplo:

- Você está implantando para o serviço de agente remoto no TESTWEB1.
- Você está instruindo a implantação da Web para usar a autenticação NTLM. A implantação da Web será executado usando as credenciais usadas para invocar o Microsoft Build Engine (MSBuild).
- Você está usando a autenticação integrada para implantar o **ContactManager** banco de dados para TESTDB1. O banco de dados será implantado usando as credenciais usadas para chamar o MSBuild.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample1.xml)]


### <a name="example-2x2014deployment-to-the-web-deploy-handler-endpoint"></a>Exemplo 2 & #x 2014; implantação na Web que implantar o ponto de extremidade de manipulador

Neste exemplo:

- Você está implantando para o ponto de extremidade de serviço do manipulador de implantação da Web em STAGEWEB1.
- Você está instruindo a implantação da Web para usar a autenticação básica.
- Você está especificando que a implantação da Web deve representar a conta FABRIKAM\stagingdeployer no computador remoto.
- Você está usando a autenticação do SQL Server para implantar o **ContactManager** banco de dados para STAGEDB1.


[!code-xml[Main](configuring-deployment-properties-for-a-target-environment/samples/sample2.xml)]


## <a name="conclusion"></a>Conclusão

Neste ponto, os arquivos de projeto são totalmente configurados para compilar e implantar a solução de Gerenciador de contato para um ou mais ambientes de destino.

Para usar esses arquivos de projeto como parte de um processo de implantação única etapa, repetíveis, você precisa executar o *Publish.proj* arquivo usando MSBuild e passar o local do arquivo de projeto específico do ambiente como um parâmetro. Você pode fazer isso de várias maneiras:

- Para obter uma visão geral de MSBuild e uma introdução aos arquivos de projeto personalizados, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Para obter informações sobre como formular um comando de MSBuild que executa os arquivos de projeto personalizados, consulte [Implantando pacotes de Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).
- Para obter informações sobre como incorporar seus comandos de MSBuild em um arquivo de comando para implantações única etapa, reproduzíveis, consulte [criando e executando um arquivo de comando de implantação](../web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file.md).
- Para obter informações sobre como executar os arquivos de projeto personalizados do Team Build, consulte [criando uma definição de compilação que dá suporte à implantação](../configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment.md).

>[!div class="step-by-step"]
[Anterior](creating-a-server-farm-with-the-web-farm-framework.md)
