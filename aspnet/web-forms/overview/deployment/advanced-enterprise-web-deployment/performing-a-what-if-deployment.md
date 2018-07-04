---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Realizar ao que se implantação | Microsoft Docs
author: jrjlee
description: Este tópico descreve como executar 'e se' (ou simulados) implantações usando a ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web) e V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: f50053c3b231a1d174df51ce7aa4c6013192bca8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378093"
---
<a name="performing-a-what-if-deployment"></a>Executando uma implantação "What If"
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como executar "what if" (ou simulados) usando a ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web) e VSDBCMD de implantações. Isso lhe permite determinar os efeitos de sua lógica de implantação em um ambiente de destino em particular antes de realmente implantar seu aplicativo.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação e implantação é controlado por dois arquivos de projeto&#x2014;um que contém instruções de compilação que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Executando uma implantação "What If" para pacotes da Web

A implantação da Web inclui a funcionalidade que permite que você realize implantações em "what if" (ou versão de avaliação) modo. Quando você implanta os artefatos no modo "what if", implantação da Web gera um arquivo de log como se você executou a implantação, mas, na verdade, ele não altera nada no servidor de destino. Revisão do arquivo de log pode ajudá-lo a entender o impacto que sua implantação terá no servidor de destino, em particular:

- O que serão adicionados ao.
- O que será atualizado.
- O que será excluído.

Como uma implantação "what if", na verdade, não altera nada no servidor de destino, o que ele nem sempre isso é prever se uma implantação será bem-sucedida.

Conforme descrito em [Implantando pacotes da Web](../web-deployment-in-the-enterprise/deploying-web-packages.md), você pode implantar os pacotes da web usando a implantação da Web de duas maneiras&#x2014;usando o utilitário de linha de comando MSDeploy.exe diretamente ou executando o *. Deploy. cmd* arquivo que gera o processo de compilação.

Se você estiver usando MSDeploy.exe diretamente, você pode executar uma implantação "what if", adicionando a **– whatif** sinalizador ao seu comando. Por exemplo, para avaliar o que aconteceria se você implantar o pacote de ContactManager.Mvc.zip em um ambiente de preparo, o comando MSDeploy deve ter esta aparência:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


Quando estiver satisfeito com os resultados da implantação "what if", você pode remover o **– whatif** sinalizador para executar uma implantação em tempo real.

> [!NOTE]
> Para obter mais informações sobre opções de linha de comando para MSDeploy.exe, consulte [Web implantar as configurações de operação](https://technet.microsoft.com/library/dd569089(WS.10).aspx).


Se você estiver usando o *. Deploy. cmd* arquivo, você pode executar uma implantação "what if", incluindo o **/t** sinalizador sinalizador (modo de avaliação), em vez da **/y** sinalizador ("Sim", ou o modo de atualização) no seu comando. Por exemplo, para avaliar o que aconteceria se você implantar o pacote de ContactManager.Mvc.zip executando o *. Deploy. cmd* arquivo, seu comando deve ter esta aparência:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


Quando estiver satisfeito com os resultados da implantação do "modo de avaliação", você pode substituir a **/t** sinalizador com um **/y** sinalizador para executar uma implantação em tempo real:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> Para obter mais informações sobre opções de linha de comando para *. Deploy. cmd* arquivos, consulte [como: instalar uma implantação de pacote usando o arquivo Deploy](https://msdn.microsoft.com/library/ff356104.aspx). Se você executar o *. Deploy. cmd* arquivo sem especificar quaisquer sinalizadores, o prompt de comando exibirá uma lista de sinalizadores disponíveis.


## <a name="performing-a-what-if-deployment-for-databases"></a>Executando uma implantação "What If" para bancos de dados

Esta seção pressupõe que você estiver usando o utilitário VSDBCMD para executar uma implantação incremental com base no esquema de banco de dados. Essa abordagem é descrita mais detalhadamente [implantar projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md). É recomendável que você se familiarizar com este tópico antes de aplicar os conceitos descritos aqui.

Quando você usa VSDBCMD na **Deploy** modo, você pode usar o **/dd** (ou **/DeployToDatabase**) sinalizar ao controle se VSDBCMD realmente implanta o banco de dados ou apenas gera uma script de implantação. Se você estiver implantando um arquivo .dbschema, esse é o comportamento:

- Se você especificar **/dd+** ou **/dd**, VSDBCMD gerará um script de implantação e implantar o banco de dados.
- Se você especificar **/dd-** ou omitir a opção, VSDBCMD irá gerar um script de implantação.

> [!NOTE]
> Se você estiver implantando um arquivo .deploymanifest em vez de um arquivo .dbschema, o comportamento do **/dd** switch é muito mais complicado. Essencialmente, VSDBCMD ignorará o valor da **/dd** mudar se o arquivo .deploymanifest inclui um **DeployToDatabase** elemento com um valor de **verdadeiro**. [Implantação de projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md) descreve esse comportamento por completo.


Por exemplo, para gerar um script de implantação para o **ContactManager** banco de dados sem realmente implantar o banco de dados, seu comando VSDBCMD deve ter esta aparência:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD é uma ferramenta de implantação de banco de dados diferencial, e como tal, o script de implantação é gerado dinamicamente para conter todos os comandos SQL necessários para atualizar o banco de dados atual, se existir, o esquema especificado. Revisar o script de implantação é uma maneira útil para determinar o que afeta sua implantação terá no banco de dados atual e os dados que ele contém. Por exemplo, talvez você queira determinar:

- Se todas as tabelas existentes serão removidas e se isso resultará em perda de dados.
- Se a ordem das operações traz um risco de perda de dados, por exemplo, se você estiver dividindo ou mesclando tabelas.

Se você estiver satisfeito com o script de implantação, você pode repetir o VSDBCMD com um **/dd+** sinalizador para fazer as alterações. Como alternativa, você pode editar o script de implantação para atender às suas necessidades e, em seguida, executá-lo manualmente no servidor de banco de dados.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrar a funcionalidade "What If" em arquivos de projeto personalizado

Em cenários de implantação mais complexos, você desejará usar um arquivo de projeto personalizado do Microsoft Build Engine (MSBuild) para encapsular sua lógica de compilação e implantação, conforme descrito em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Por exemplo, nos [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) exemplo de solução, o *Publish.proj* arquivo:

- Compila a solução.
- Usa a implantação da Web para empacotar e implantar o aplicativo ContactManager.Mvc.
- Usa a implantação da Web para empacotar e implantar o aplicativo ContactManager.Service.
- Implanta o **ContactManager** banco de dados.

Ao integrar a implantação de vários pacotes da web e/ou bancos de dados em um processo passo a passo, dessa forma, você poderá também a opção de executar toda a implantação em um modo "what if".

O *Publish.proj* arquivo demonstra como você pode fazer isso. Primeiro, você precisa criar uma propriedade para armazenar o valor "what if":


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


Nesse caso, você criou uma propriedade chamada **WhatIf** com um valor padrão de **falso**. Os usuários podem substituir esse valor definindo a propriedade como **verdadeira** em um parâmetro de linha de comando, como você verá em breve.

A próxima etapa é parametrizar a qualquer implantação da Web e VSDBCMD comandos para que os sinalizadores de refletem a **WhatIf** valor da propriedade. Por exemplo, o próximo destino (tirados o *Publish.proj* do arquivo e simplificado) executa o *. Deploy. cmd* arquivo para implantar um pacote da web. Por padrão, o comando inclui uma **/Y** switch ("Sim" ou o modo de atualização). Se **WhatIf** é definido como **verdadeiro**, isso é substituído por um **/T** switch (versão de avaliação ou modo de "what if").


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


Da mesma forma, o próximo destino usa o utilitário VSDBCMD para implantar um banco de dados. Por padrão, uma **/dd** comutador não está incluído. Isso significa que VSDBCMD irá gerar um script de implantação, mas não implantará o banco de dados&#x2014;em outras palavras, um "what if" cenário. Se o **WhatIf** não está definida **verdadeiro**, um **/dd** switch é adicionado e VSDBCMD implantará o banco de dados.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


Você pode usar a mesma abordagem para parametrizar todos os comandos relevantes no arquivo de projeto. Quando você deseja executar uma implantação "what if", você poderá simplesmente fornecer um **WhatIf** valor da propriedade da linha de comando:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


Dessa forma, você pode executar uma implantação "what if" para todos os componentes de projeto em uma única etapa.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como executar "what if" implantações usando o MSBuild, VSDBCMD e implantação da Web. Uma implantação "what if" lhe permite avaliar o impacto de uma implantação de propostas antes de realmente fazer qualquer alteração no ambiente de destino.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre a sintaxe de linha de comando de implantação da Web, consulte [Web implantar as configurações de operação](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Para obter diretrizes sobre opções de linha de comando quando você usa o *. Deploy. cmd* arquivos, consulte [como: instalar uma implantação de pacote usando o arquivo Deploy](https://msdn.microsoft.com/library/ff356104.aspx). Para obter orientação sobre a sintaxe de linha de comando VSDBCMD, consulte [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Anterior](advanced-enterprise-web-deployment.md)
> [Próximo](customizing-database-deployments-for-multiple-environments.md)
