---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Executando Scripts do Windows PowerShell de arquivos de projeto do MSBuild | Microsoft Docs
author: jrjlee
description: Este tópico descreve como executar um script do Windows PowerShell como parte de um processo de compilação e implantação. Você pode executar um script localmente (em outras palavras, no b...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: 018a962c3bac774a770b83b2fd1f44f72b6f5b09
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830789"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Executando Scripts do Windows PowerShell de arquivos de projeto do MSBuild
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como executar um script do Windows PowerShell como parte de um processo de compilação e implantação. Você pode executar um script localmente (em outras palavras, no servidor de compilação) ou remotamente, como em um servidor web de destino ou o servidor de banco de dados.
> 
> Há muitas razões por que você talvez queira executar um script de pós-implantação do Windows PowerShell. Por exemplo, é possível:
> 
> - Adicione uma fonte de evento personalizado para o registro.
> - Gere um diretório de sistema de arquivos para carregamentos.
> - Limpe os diretórios de compilação.
> - Gravar entradas em um arquivo de log personalizado.
> - Envie emails para convidar usuários para um aplicativo web recentemente provisionado.
> - Crie contas de usuário com as permissões apropriadas.
> - Configure a replicação entre instâncias do SQL Server.
> 
> Este tópico mostra como executar scripts do Windows PowerShell local e remotamente de um destino personalizado em um arquivo de projeto do Microsoft Build Engine (MSBuild).


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Para executar um script do Windows PowerShell como parte de um processo de implantação automatizada ou passo a passo, você precisará concluir essas tarefas de alto nível:

- Adicione o script do Windows PowerShell à sua solução e ao controle de origem.
- Crie um comando que invoca o script do Windows PowerShell.
- Todos os caracteres XML reservados em seu comando de escape.
- Criar um destino no arquivo de projeto personalizado do MSBuild e usar o **Exec** tarefa para executar o comando.

Este tópico mostra como executar esses procedimentos. As tarefas e instruções passo a passo neste tópico pressupõem que você já esteja familiarizado com as propriedades e destinos do MSBuild e que você compreenda como usar um arquivo de projeto personalizado do MSBuild para orientar um processo de compilação e implantação. Para obter mais informações, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Criando e adicionando Scripts do Windows PowerShell

As tarefas neste tópico usam um exemplo de script do Windows PowerShell nomeado **LogDeploy.ps1** para ilustrar como executar scripts do MSBuild. O **LogDeploy.ps1** script contém uma função simples que grava uma entrada de linha única para um arquivo de log:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]


O **LogDeploy.ps1** script aceita dois parâmetros. O primeiro parâmetro representa o caminho completo para o arquivo de log para o qual você deseja adicionar uma entrada e o segundo parâmetro representa o destino de implantação que você deseja gravar no arquivo de log. Quando você executa o script, ele adiciona uma linha ao arquivo de log neste formato:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Para tornar o **LogDeploy.ps1** script disponível para o MSBuild, você precisa:

- Adicione o script ao controle de origem.
- Adicione o script para sua solução no Visual Studio 2010.

Você não precisa implantar o script com o conteúdo de solução, independentemente se você planeja executar o script no servidor de compilação ou em um computador remoto. É uma opção Adicionar o script para uma pasta de solução. O exemplo Contact Manager, como você deseja usar o script do Windows PowerShell como parte do processo de implantação, faz sentido para adicionar o script para a pasta de solução de publicação.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

O conteúdo das pastas de solução é copiado para os servidores de compilação como material de origem. No entanto, eles formam nenhuma parte de qualquer saída do projeto.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Executar um Script do Windows PowerShell no servidor de compilação

Em alguns cenários, você talvez queira executar scripts do Windows PowerShell no computador que compila seus projetos. Por exemplo, você pode usar um script do Windows PowerShell para limpeza de pastas de compilação ou gravar entradas em um arquivo de log personalizado.

Em termos de sintaxe, executar um script do Windows PowerShell de um arquivo de projeto do MSBuild é o mesmo que executar um script do Windows PowerShell em um prompt de comando regular. Você precisa invocar o powershell.exe executável e usar o **– comando** switch para fornecer os comandos que você deseja que o Windows PowerShell para executar. (No Windows PowerShell v2, você também pode usar o **– arquivo** alternar). O comando deve levar este formato:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Por exemplo:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Se o caminho para o seu script contiver espaços, você precisa colocar o caminho do arquivo entre aspas, precedidas por um e comercial. É possível usar aspas duplas, porque você já tiver usado-los para incluir o comando:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


Há algumas considerações adicionais quando você invoca este comando do MSBuild. Primeiro, você deve incluir a **– NonInteractive** sinalizador para garantir que o script é executado no modo silencioso. Em seguida, você deve incluir a **ExecutionPolicy –** sinalizador com um valor de argumento apropriado. Isso especifica a política de execução que o Windows PowerShell será aplicada ao seu script e permite que você substitua a política de execução padrão, que pode impedir a execução do script. Você pode escolher entre esses valores de argumento:

- Um valor de **irrestrito** permitirá que o Windows PowerShell executar o script, independentemente se o script está assinado.
- Um valor de **RemoteSigned** permitirá que o Windows PowerShell executar scripts não assinados que foram criados no computador local. No entanto, os scripts que foram criados em outro lugar devem ser assinados. (Na prática, é muito improvável que tenha criado um script do PowerShell do Windows localmente em um servidor de compilação).
- Um valor de **AllSigned** permitirá que o Windows PowerShell executar somente scripts assinados.

A política de execução padrão é **Restricted**, que impede que o Windows PowerShell executando qualquer arquivo de script.

Por fim, você precisará escapar qualquer caracteres XML reservados que ocorrem em seu comando do Windows PowerShell:

- Substituir aspas com  **&amp;apos;**
- Substitua as aspas duplas com  **&amp;quot;**
- Substitua "e" comercial com  **&amp;amp;**

- Quando você fizer essas alterações, o comando será parecida com isto:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


Dentro de seu arquivo de projeto personalizado do MSBuild, você pode criar um novo destino e usar o **Exec** tarefa para executar este comando:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


Neste exemplo, observe que:

- Todas as variáveis, como valores de parâmetro e o local do executável do Windows PowerShell, são declaradas como propriedades do MSBuild.
- As condições são incluídas para permitir que os usuários substituir esses valores da linha de comando.
- O **MSDeployComputerName** propriedade é declarada em outro lugar no arquivo de projeto.

Quando você executar esse destino como parte do processo de compilação, o Windows PowerShell execute o comando e gravar uma entrada de log para o arquivo especificado.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Executar um Script do Windows PowerShell em um computador remoto

Windows PowerShell é capaz de executar scripts em computadores remotos por meio [gerenciamento remoto do Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Para fazer isso, você precisa usar o [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet. Isso permite que você executar o script em um ou mais computadores remotos sem copiar o script para os computadores remotos. Todos os resultados são retornados ao computador local do qual você executou o script.

> [!NOTE]
> Antes de usar o **Invoke-Command** scripts de cmdlet para executar o Windows PowerShell em um computador remoto, você precisará configurar um ouvinte de WinRM para aceitar mensagens remotas. Você pode fazer isso executando o comando **winrm quickconfig** no computador remoto. Para obter mais informações, consulte [instalação e configuração para gerenciamento remoto do Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


Em uma janela do Windows PowerShell, você usaria essa sintaxe para executar o **LogDeploy.ps1** script em um computador remoto:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Há várias outras maneiras de usar **Invoke-Command** para executar um script do arquivo, mas essa abordagem é mais simples quando você precisa fornecer valores de parâmetro e gerenciar caminhos com espaços.


Quando você executar isso em um prompt de comando, você precisará chamar o executável de PowerShell do Windows e usar o **– comando** parâmetro para fornecer suas instruções:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Como antes, você precisará fornecer algumas opções adicionais e todos os caracteres XML reservados de escape quando você executa o comando do MSBuild:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Por fim, assim como antes, você pode usar o **Exec** tarefa dentro de um destino MSBuild personalizado para executar o comando:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Quando você executar esse destino como parte do processo de compilação, o Windows PowerShell executará seu script no computador especificado na **– computername** argumento.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como executar um script do Windows PowerShell de um arquivo de projeto MSBuild. Você pode usar essa abordagem para executar um script do Windows PowerShell, localmente ou em um computador remoto, como parte de um processo de compilação e implantação automatizado ou passo a passo.

## <a name="further-reading"></a>Leitura adicional

Para obter orientação sobre como assinar scripts do Windows PowerShell e gerenciar políticas de execução, consulte [executando Scripts do Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Para obter orientação sobre como executar comandos do Windows PowerShell de um computador remoto, consulte [executando comandos remotos](https://technet.microsoft.com/library/dd819505.aspx).

Para obter mais informações sobre como usar arquivos de projeto personalizados do MSBuild para controlar o processo de implantação, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Anterior](taking-web-applications-offline-with-web-deploy.md)
> [Próximo](troubleshooting-the-packaging-process.md)
