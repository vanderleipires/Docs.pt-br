---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatizar tudo (compilação de aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 2e30ab7831a10f215a08f74e61adf2d147e76543
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatizar tudo (compilação de aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter uma introdução ao livro eletrônico, consulte [primeiro capítulo](introduction.md).


Os três primeiros padrões, examinaremos realmente se aplicam a qualquer projeto de desenvolvimento de software, mas especialmente em projetos de nuvem. Esse padrão é sobre como automatizar tarefas de desenvolvimento. É um tópico importante porque processos manuais são lentos e sujeitos a erros; automatizando como muitos deles como possíveis ajuda a configurar um fluxo de trabalho agile rápido e confiável. É importante exclusivamente para o desenvolvimento de nuvem porque você pode facilmente automatizar muitas tarefas que são difíceis ou impossíveis de automatizar em um ambiente local. Por exemplo, você pode configurar todo teste ambientes, inclusive o novo servidor de web e VMs de back-end, bancos de dados blob (armazenamento de arquivos) de armazenamento, filas, etc.

## <a name="devops-workflow"></a>Fluxo de trabalho de DevOps

Cada vez mais ouvir o termo "DevOps." O termo desenvolvido fora de um reconhecimento de que você precisa integrar tarefas de desenvolvimento e operações para desenvolver software com eficiência. O tipo de fluxo de trabalho que você deseja habilitar é um no qual você pode desenvolver um aplicativo, implantá-lo, aprender a partir de uso de produção, alterá-la em resposta a você aprendeu e repita o ciclo de maneira rápida e confiável.

As equipes de desenvolvimento alguns nuvem bem-sucedida implantar várias vezes ao dia para um ambiente dinâmico. A equipe do Azure usada para implantar um principal atualiza todos os meses de 2 a 3, mas agora ele atualizações secundárias de versões cada 2 a 3 dias e principais libera todas as semanas de 2 a 3. Obtendo em que o ritmo realmente ajuda a responder aos comentários dos clientes.

Para fazer isso, você precisa habilitar um ciclo de desenvolvimento e implantação repetível, confiável e previsível, e o tempo de ciclo de baixa.

![Fluxo de trabalho de DevOps](automate-everything/_static/image1.png)

Em outras palavras, o período de tempo entre quando você tiver uma ideia para um recurso e quando os clientes estão usando e fornecer comentários deve ser tão curto quanto possível. Os padrões de três primeiros automatizar tudo, controle de origem, e a integração contínua e entrega – são tudo sobre as práticas recomendadas que recomendamos para habilitar esse tipo de processo.

## <a name="azure-management-scripts"></a>Scripts de gerenciamento do Azure

No [Introdução a este livro eletrônico](introduction.md), você viu o console baseado na web, o Portal de gerenciamento do Azure. O portal de gerenciamento permite que você monitore e gerencie todos os recursos que você tiver implantado no Azure. É uma maneira fácil de criar e excluir serviços, como aplicativos web e máquinas virtuais, configure esses serviços, monitorar a operação de serviço e assim por diante. É uma excelente ferramenta, mas usá-lo é um processo manual. Se você pretende desenvolver um aplicativo de produção de qualquer tamanho, e especialmente em um ambiente de equipe, é recomendável que você percorrer o portal de interface do usuário para saber mais e explorar o Azure e automatizar os processos que você realizará repetidamente.

Quase tudo o que você pode fazer manualmente no portal de gerenciamento ou do Visual Studio também pode ser feito chamando a API de gerenciamento do REST. Você pode escrever scripts usando [do Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), ou você pode usar uma estrutura de software livre como [Chef](http://www.opscode.com/chef/) ou [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Você também pode usar a ferramenta de linha de comando do Bash em um ambiente de Mac ou Linux. O Azure tem scripts de APIs para todos os ambientes diferentes, e tem um [API de gerenciamento do .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) caso você deseje gravar o código em vez de script.

Para o aplicativo corrigir criamos alguns scripts do Windows PowerShell que automatizam os processos de criação de um ambiente de teste e implantação do projeto nesse ambiente, e analisaremos parte do conteúdo dos scripts.

## <a name="environment-creation-script"></a>Script de criação do ambiente

O script primeiro vamos examinar é nomeado *AzureWebsiteEnv.ps1 novo*. Ele cria um ambiente do Azure que você pode implantar o corrigir aplicativo para teste. As tarefas principais que esse script executa são os seguintes:

- Crie um aplicativo web.
- Crie uma conta de armazenamento. (De necessária para blobs e filas, como você verá nos capítulos posteriores.)
- Criar um servidor de banco de dados SQL e dois bancos de dados: um banco de dados do aplicativo e um banco de dados de associação.
- Armazenar configurações no Azure que o aplicativo usará para acessar a conta de armazenamento e bancos de dados.
- Crie arquivos de configuração que serão usados para automatizar a implantação.

### <a name="run-the-script"></a>Execute o script


> [!NOTE]
> Esta parte do capítulo mostra exemplos de scripts e os comandos que você inserir para executá-los. Esta uma demonstração e não fornece tudo o que você precisa saber para executar os scripts. Para obter instruções passo a passo do how-a--it, consulte [Apêndice: A corrigir-aplicativo de exemplo](the-fix-it-sample-application.md#deploybase).


Para executar um script do PowerShell que gerencia serviços do Azure, que você precisa instalar o console do PowerShell do Azure e configurá-lo para trabalhar com sua assinatura do Azure. Depois que você estiver configurado, você pode executar o corrigir ambiente script de criação com um comando como este:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

O `Name` parâmetro especifica o nome a ser usado ao criar as contas de armazenamento e de banco de dados e o `SqlDatabasePassword` parâmetro especifica a senha para a conta de administrador que será criada para o banco de dados SQL. Há outros parâmetros que você pode usar o que veremos mais tarde.

![Janela do PowerShell](automate-everything/_static/image2.png)

Após a conclusão do script você pode ver no portal de gerenciamento que foi criado. Você encontrará dois bancos de dados:

![Bancos de dados](automate-everything/_static/image3.png)

Uma conta de armazenamento:

![Conta de armazenamento](automate-everything/_static/image4.png)

E um aplicativo web:

![Site](automate-everything/_static/image5.png)

Sobre o **configurar** guia para o aplicativo web, você pode ver que tem as configurações de conta de armazenamento e cadeias de conexão de banco de dados SQL configurados para a correção-aplicativo.

![appSettings e connectionStrings](automate-everything/_static/image6.png)

O *automação* pasta agora também contém um  *&lt;websitename&gt;. pubxml* arquivo. Esse arquivo armazena configurações que MSBuild usará para implantar o aplicativo no ambiente do Azure que acabou de ser criado. Por exemplo:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Como você pode ver, o script criou um ambiente de teste completo, e todo o processo é feito em cerca de 90 segundos.

Se outra pessoa da sua equipe deseja criar um ambiente de teste, assim eles podem executar o script. Não só é rápido, mas também podem ser seguras de que eles estão usando um ambiente idêntico ao que você está usando. Você não poderia ser tão seguro de que todos foi Configurando tudo manualmente usando o portal de gerenciamento da interface do usuário.

### <a name="a-look-at-the-scripts"></a>Examinar os scripts

Há, na verdade, três scripts que realizam esse trabalho. Chamar um da linha de comando e usa automaticamente os outros dois para realizar algumas das tarefas:

- *Novo AzureWebSiteEnv.ps1* é o script principal.

    - *Novo AzureStorage.ps1* cria a conta de armazenamento.
    - *Novo AzureSql.ps1* cria os bancos de dados.

### <a name="parameters-in-the-main-script"></a>Parâmetros do script principal

O script principal, *AzureWebSiteEnv.ps1 novo*, define vários parâmetros:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Dois parâmetros são necessários:

- O nome do aplicativo web que cria o script. (Isso também é usado para a URL: `<name>.azurewebsites.net`.)
- A senha para o novo usuário administrativo do servidor de banco de dados que cria o script.

Parâmetros opcionais permitem que você especifique o local de centro de dados (o padrão é "US West"), nome de administrador de servidor de banco de dados (o padrão é "dbuser") e uma regra de firewall para o servidor de banco de dados.

### <a name="create-the-web-app"></a>Criar o aplicativo web

A primeira coisa que o script faz é criar o aplicativo web chamando o `New-AzureWebsite` cmdlet, passando a ele o aplicativo web valores de parâmetro de nome e local:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Criar a conta de armazenamento

Em seguida, o script principal é executado o <em>novo AzureStorage.ps1</em> script, especificando "<em>&lt;websitename&gt;</em>armazenamento" para o nome da conta de armazenamento, e a mesma data center local como o aplicativo web.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*Novo AzureStorage.ps1* chama o `New-AzureStorageAccount` cmdlet para criar a conta de armazenamento e retorna a conta de valores de chave de acesso e do nome. O aplicativo precisará esses valores para acessar os blobs e filas na conta de armazenamento.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Você não pode ser sempre criar uma nova conta de armazenamento; Você pode aprimorar o script adicionando um parâmetro que direciona opcionalmente para usar uma conta de armazenamento existente.

### <a name="create-the-databases"></a>Criar os bancos de dados

O script principal, em seguida, executa o script de criação de banco de dados, *AzureSql.ps1 novo*, após configurar o banco de dados padrão e nomes de regras de firewall:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

O script de criação de banco de dados recupera o endereço IP do computador de desenvolvimento e define uma regra de firewall para a máquina de desenvolvimento pode se conectar e gerenciar o servidor. O script de criação de banco de dados, em seguida, passa por várias etapas para configurar os bancos de dados:

- Cria o servidor usando o `New-AzureSqlDatabaseServer` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Cria regras de firewall para habilitar a máquina de desenvolvimento para gerenciar o servidor e para habilitar o aplicativo web para se conectar a ele. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Cria um contexto de banco de dados que inclui o nome do servidor e as credenciais, usando o `New-AzureSqlDatabaseServerContext` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` é uma função no script que chama o `ConvertTo-SecureString` cmdlet para criptografar a senha e retorna um `PSCredential` do objeto, o mesmo tipo que o `Get-Credential` cmdlet retorna.
- Cria o banco de dados do aplicativo e o banco de dados de associação usando o `New-AzureSqlDatabase` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Chama uma função definida localmente tocreates uma cadeia de caracteres de conexão para cada banco de dados. O aplicativo usará essas cadeias de caracteres de conexão para acessar os bancos de dados. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString é uma função definida no script que cria a cadeia de caracteres de conexão de valores de parâmetro fornecidos a ele.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Retorna uma tabela de hash com o nome do servidor de banco de dados e as cadeias de caracteres de conexão.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

O aplicativo corrigir usa associação separado e bancos de dados do aplicativo. Também é possível colocar os dados de associação e o aplicativo em um único banco de dados.

### <a name="store-app-settings-and-connection-strings"></a>Armazenar cadeias de caracteres de conexão e as configurações do aplicativo

O Azure tem um recurso que permite que você armazene as configurações e cadeias de caracteres de conexão que substituem automaticamente o que é retornado ao aplicativo quando ele tenta ler o `appSettings` ou `connectionStrings` coleções no arquivo Web. config. Essa é uma alternativa à aplicação [transformações de Web. config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) quando você implanta. Para obter mais informações, consulte [armazenar dados confidenciais no Azure](source-control.md#appsettings) posteriormente este livro eletrônico.

O script de criação do ambiente armazena no Azure todas as `appSettings` e `connectionStrings` valores que o aplicativo precisa acessar a conta de armazenamento e bancos de dados quando ele é executado no Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) é uma estrutura de telemetria demonstramos no [monitoramento e telemetria](monitoring-and-telemetry.md) capítulo. O script de criação do ambiente também reinicia o aplicativo web para certificar-se de que ele escolhe as configurações do New Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Preparando para implantação

No final do processo, o script de criação do ambiente chama duas funções para criar arquivos que serão usados pelo script de implantação.

Uma dessas funções cria um perfil de publicação *(&lt;websitename&gt;. pubxml* arquivo). O código chama a API REST do Azure para obter as configurações de publicação e salva as informações em um *. publishsettings* arquivo. Em seguida, ele usa as informações desse arquivo junto com um arquivo de modelo (*pubxml.template*) para criar o *. pubxml* arquivo que contém o perfil de publicação. Esse processo de duas etapas simula o que fazer no Visual Studio: baixar um *. publishsettings* de arquivo e importá-lo para criar um perfil de publicação.

A outra função usa outro arquivo de modelo (site environment.template) para criar um *site environment.xml* arquivo que contém as configurações de script de implantação usará juntamente com o *. pubxml*arquivo.

### <a name="troubleshooting-and-error-handling"></a>Solução de problemas e tratamento de erros

Os scripts são como programas: eles podem falhar e ao fazer você deseja saber quanto possível sobre a falha e o que o causou. Por esse motivo, o script de criação do ambiente altera o valor da `VerbosePreference` variável do `SilentlyContinue` para `Continue` para que todas as mensagens detalhadas são exibidas. Ele também altera o valor de `ErrorActionPreference` variável do `Continue` para `Stop`, de modo que o script interrompe mesmo quando ele encontra erros de não finalização:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Antes de qualquer trabalho, o script armazena a hora de início para que ele pode calcular o tempo decorrido quando é feito:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Depois de concluir seu trabalho, o script exibe o tempo decorrido:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

E para cada operação de chave o script grava as mensagens detalhadas, por exemplo:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Script de implantação

O que o *New-AzureWebsiteEnv.ps1* script faz para a criação do ambiente, o *AzureWebsite.ps1 publicar* script faz para implantação de aplicativo.

O script de implantação obtém o nome do aplicativo da web a *site environment.xml* arquivo criado pelo script de criação do ambiente.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Obtém a senha de usuário de implantação de *. publishsettings* arquivo:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Ele executa o [MSBuild](http://msbuildbook.com/) comando que cria e implanta o projeto:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

E se você tiver especificado o `Launch` parâmetro na linha de comando, ele chama o `Show-AzureWebsite` cmdlet para abrir o navegador padrão para a URL do site.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Você pode executar o script de implantação com um comando como este:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

E quando estiver pronto, o navegador é aberto com o site em execução na nuvem no `<websitename>.azurewebsites.net` URL.

![Corrija-o aplicativo implantado no Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Resumo

Com esses scripts, você pode ter certeza de que as mesmas etapas sempre serão executadas na mesma ordem usando as mesmas opções. Isso ajuda a garantir que cada desenvolvedor da equipe não perder algo atrapalhe algo ou implantar algo personalizado em sua própria máquina que na verdade não funcionará da mesma maneira no ambiente do outro membro da equipe ou em produção.

De maneira semelhante, você pode automatizar funções de gerenciamento do Azure mais que você pode fazer no portal de gerenciamento, usando a API REST, scripts do Windows PowerShell, uma linguagem .NET API ou um utilitário de Bash que podem ser executados no Linux ou Mac.

No [próximo capítulo](source-control.md) vamos examinar o código-fonte e explicar por que é importante incluir os scripts em seu repositório de código fonte.

## <a name="resources"></a>Recursos

- [Instalar e configurar o Windows PowerShell para Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Explica como instalar os cmdlets do PowerShell do Azure e como instalar o certificado que você precisa em seu computador para gerenciar seu Azure da conta. Isso é um ótimo lugar para começar porque ele também contém links para recursos de aprendizagem do PowerShell em si.
- [Azure Script Center](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portal WindowsAzure.com aos recursos para o desenvolvimento de scripts que gerenciam serviços do Azure, com links para obter scripts de exemplo, código de origem e de documentação de referência de cmdlet e tutoriais de Introdução
- [Fim de semana Scripter: Introdução ao Azure e PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). Em um blog dedicado para o Windows PowerShell, esta postagem fornece uma excelente introdução ao uso do PowerShell para funções de gerenciamento do Azure.
- [Instalar e configurar a Interface de linha de comando de plataforma cruzada do Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Tutorial de Introdução para uma estrutura de script do Azure que funciona no Mac e Linux, bem como Windows sistemas.
- [Seção de ferramentas de linha de comando do tópico baixar SDKs do Azure e as ferramentas](https://azure.microsoft.com/downloads/). Página do portal para documentação e downloads relacionados para ferramentas de linha de comando do Azure.
- [Automatizando tudo com o .NET e bibliotecas de gerenciamento do Azure](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman apresenta a API de gerenciamento de .NET do Azure.
- [Usando Scripts do Windows PowerShell para publicar em ambientes de teste e desenvolvimento](https://msdn.microsoft.com/library/azure/dn642480.aspx). Documentação do MSDN que explica como usar scripts que o Visual Studio gera automaticamente para projetos da web de publicação.
- [Ferramentas do PowerShell para Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Extensão do Visual Studio que adiciona o suporte de idioma para o Windows PowerShell no Visual Studio.

> [!div class="step-by-step"]
> [Anterior](introduction.md)
> [Próximo](source-control.md)
