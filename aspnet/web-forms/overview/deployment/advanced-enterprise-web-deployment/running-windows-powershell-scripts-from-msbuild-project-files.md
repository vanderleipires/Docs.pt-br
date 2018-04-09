---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Executando Scripts do Windows PowerShell de arquivos de projeto MSBuild | Microsoft Docs
author: jrjlee
description: Este tópico descreve como executar um script do Windows PowerShell como parte de um processo de compilação e implantação. Você pode executar um script localmente (em outras palavras, no b....
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: c8ef22cfbba7b3b85944ea4c49f3183e5a6aafbb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Executando Scripts do Windows PowerShell de arquivos de projeto do MSBuild
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como executar um script do Windows PowerShell como parte de um processo de compilação e implantação. Você pode executar um script localmente (em outras palavras, no servidor de compilação) ou remotamente, como em um servidor web de destino ou o servidor de banco de dados.
> 
> Há várias razões por que você talvez queira executar um script do Windows PowerShell de pós-implantação. Por exemplo, é possível:
> 
> - Adicione uma fonte de evento personalizado para o registro.
> - Gere um diretório de sistema de arquivos para carregamentos.
> - Limpe os diretórios de compilação.
> - Grave entradas para um arquivo de log personalizado.
> - Envie emails convidar usuários para um aplicativo web recentemente provisionados.
> - Crie contas de usuário com as permissões apropriadas.
> - Configure a replicação entre instâncias do SQL Server.
> 
> Neste tópico mostram como executar scripts do Windows PowerShell local e remotamente de um destino personalizado em um arquivo de projeto do Microsoft Build Engine (MSBuild).


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Para executar um script do Windows PowerShell como parte de um processo de implantação automatizada ou única etapa, você precisará concluir essas tarefas de alto nível:

- Adicione o script do Windows PowerShell à sua solução e controle de origem.
- Crie um comando que invoca o script do Windows PowerShell.
- Escape de caracteres XML reservados em seu comando.
- Crie um destino em seu arquivo de projeto MSBuild personalizado e use o **Exec** tarefa para executar o comando.

Neste tópico mostram como executar esses procedimentos. As tarefas e instruções passo a passo neste tópico pressupõem que você já estiver familiarizado com as propriedades e os destinos do MSBuild e entender como usar um arquivo de projeto MSBuild personalizado para um processo de compilação e implantação da unidade. Para obter mais informações, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Criando e adicionando Scripts do Windows PowerShell

As tarefas neste tópico usam um script do Windows PowerShell de exemplo chamado **LogDeploy.ps1** para ilustrar como executar scripts do MSBuild. O **LogDeploy.ps1** script contém uma função simple que grava uma entrada de linha única para um arquivo de log:


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


O **LogDeploy.ps1** script aceita dois parâmetros. O primeiro parâmetro representa o caminho completo para o arquivo de log para o qual você deseja adicionar uma entrada, e o segundo parâmetro representa o destino de implantação que você deseja registrar no arquivo de log. Quando você executa o script, ele adiciona uma linha para o arquivo de log neste formato:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Para fazer o **LogDeploy.ps1** script disponíveis para o MSBuild, você precisa:

- Adicione o script para controle de origem.
- Adicione o script para sua solução no Visual Studio 2010.

Você não precisa implantar o script com o conteúdo de solução, independentemente se você planeja executar o script no servidor de compilação ou em um computador remoto. É uma opção Adicionar o script para uma pasta de solução. O exemplo Contact Manager, como você deseja usar o script do Windows PowerShell como parte do processo de implantação, faz sentido para adicionar o script para a pasta de solução de publicação.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

O conteúdo das pastas de solução é copiado para o build servidores do material de origem. No entanto, eles formam nenhuma parte de qualquer saída do projeto.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Executar um Script do Windows PowerShell no servidor de compilação

Em alguns cenários, convém executar scripts do Windows PowerShell no computador que cria seus projetos. Por exemplo, você pode usar um script do Windows PowerShell para limpar pastas ou gravar entradas de um arquivo de log personalizado.

Em termos de sintaxe, executar um script do Windows PowerShell de um arquivo de projeto MSBuild é o mesmo que a execução de um script do Windows PowerShell em um prompt de comando regular. Você precisa chamar o executável do powershell.exe e usar o **– comando** switch para fornecer os comandos que você deseja que o Windows PowerShell para executar. (No Windows PowerShell v2, você também pode usar o **– arquivo** alternar). O comando deve levar neste formato:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Por exemplo:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Se o caminho para o script incluir espaços, você precisa colocar o caminho do arquivo entre aspas, precedidas por um e comercial. Você não pode usar aspas duplas, porque você já usou-los para incluir o comando:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


Há algumas considerações adicionais quando você invoca esse comando do MSBuild. Primeiro, você deve incluir o **– NonInteractive** sinalizador para garantir que o script é executado silenciosamente. Em seguida, você deve incluir o **-ExecutionPolicy** sinalizador com um valor de argumento apropriado. Especifica a política de execução que o Windows PowerShell se aplicará ao seu script e permite substituir a política de execução padrão, que pode impedir a execução do script. Você pode escolher entre esses valores de argumento:

- Um valor de **irrestrito** permitirá que o Windows PowerShell para executar o script, independentemente se o script foi assinado.
- Um valor de **RemoteSigned** permitirá que o Windows PowerShell para executar scripts não assinados que foram criados no computador local. No entanto, os scripts criados em outro lugar devem ser assinados. (Na prática, você está muito provavelmente não criou um script do Windows PowerShell localmente em um servidor de compilação).
- Um valor de **AllSigned** permitirá que o Windows PowerShell para executar somente scripts assinados.

A política de execução padrão é **Restricted**, que impede que o Windows PowerShell executando qualquer arquivo de script.

Finalmente, você precisa de escape quaisquer caracteres XML reservados que ocorrem em seu comando do Windows PowerShell:

- Substituir aspas com  **&amp;apos;**
- Substituir aspas duplas com  **&amp;quot;**
- Substitua "e" comercial com  **&amp;amp;**

- Quando você fizer essas alterações, o comando será parecida com isto:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


Dentro de seu arquivo de projeto MSBuild personalizado, você pode criar um novo destino e usar o **Exec** tarefa para executar este comando:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


Neste exemplo, observe que:

- Todas as variáveis, como valores de parâmetro e o local do executável do Windows PowerShell, são declaradas como propriedades do MSBuild.
- Condições são incluídas para permitir que os usuários substitua esses valores da linha de comando.
- O **MSDeployComputerName** propriedade é declarada em outro lugar no arquivo de projeto.

Quando você executa esse destino como parte do processo de compilação, o Windows PowerShell execute o comando e gravar uma entrada de log para o arquivo especificado.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Executar um Script do Windows PowerShell em um computador remoto

O Windows PowerShell é capaz de executar scripts em computadores remotos por meio de [gerenciamento remoto do Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Para fazer isso, você precisa usar o [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet. Isso permite executar o script em um ou mais computadores remotos, sem copiar o script para os computadores remotos. Todos os resultados são retornados para o computador local do qual você executou o script.

> [!NOTE]
> Antes de usar o **Invoke-Command** scripts de cmdlet para executar o Windows PowerShell em um computador remoto, você precisa configurar um ouvinte de WinRM para aceitar mensagens remotas. Você pode fazer isso executando o comando **winrm quickconfig** no computador remoto. Para obter mais informações, consulte [instalação e configuração para gerenciamento remoto do Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


Em uma janela do Windows PowerShell, você usaria a seguinte sintaxe para executar o **LogDeploy.ps1** script em um computador remoto:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Há várias outras maneiras de usar **Invoke-Command** para executar um script do arquivo, mas essa abordagem é mais simples quando você precisa fornecer valores de parâmetro e gerenciar caminhos com espaços.


Ao executá-lo em um prompt de comando, você precisa chamar o executável do Windows PowerShell e usar o **– comando** parâmetro para fornecer suas instruções:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Como antes, é necessário fornecer algumas opções adicionais e escapar caracteres XML reservados quando você executar o comando do MSBuild:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Por fim, como antes, você pode usar o **Exec** tarefa dentro de um destino personalizado do MSBuild para executar o comando:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Quando você executa esse destino como parte do processo de compilação, do Windows PowerShell executará o script no computador especificado no **– computername** argumento.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como executar um script do Windows PowerShell de um arquivo de projeto do MSBuild. Você pode usar essa abordagem para executar um script do Windows PowerShell, localmente ou em um computador remoto, como parte de um processo de compilação e implantação automatizado ou única etapa.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como assinar scripts do Windows PowerShell e gerenciar políticas de execução, consulte [executando Scripts do Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Para obter orientação sobre como executar comandos do Windows PowerShell em um computador remoto, consulte [executando comandos remotos](https://technet.microsoft.com/library/dd819505.aspx).

Para obter mais informações sobre como usar arquivos de projeto MSBuild personalizados para controlar o processo de implantação, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Anterior](taking-web-applications-offline-with-web-deploy.md)
> [Próximo](troubleshooting-the-packaging-process.md)
