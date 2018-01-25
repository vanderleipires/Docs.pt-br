---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: "Criando e executando uma implantação de arquivo de comando | Microsoft Docs"
author: jrjlee
description: "Este tópico descreve como criar um arquivo de comando que permitirão a você executar uma implantação usando arquivos de projeto do Microsoft Build Engine (MSBuild) como uma única etapa, novamente..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: bc31bf55b29661816e0ca9a50b51b0abc3eb2c98
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="creating-and-running-a-deployment-command-file"></a>Criando e executando um arquivo de comando de implantação
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como criar um arquivo de comando que permitem a executar uma implantação usando arquivos de projeto do Microsoft Build Engine (MSBuild) como um processo repetível única etapa.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo & #x 2014; o [Contact Manager](the-contact-manager-solution.md) #x 2014; & solução para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, Windows Serviço do Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o processo de compilação](understanding-the-build-process.md), em que o processo de compilação é controlado por dois arquivos & #x 2014; projeto contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="process-overview"></a>Visão geral do processo

Neste tópico, você aprenderá como criar e executar um arquivo de comando que usa esses arquivos de projeto para executar uma implantação repetível para seu ambiente de destino. Essencialmente, o arquivo de comando simplesmente precisa conter um comando do MSBuild que:

- Informa o MSBuild para executar o ambiente independente *Publish.proj* arquivo.
- Informa o *Publish.proj* arquivo qual arquivo contém as configurações de projeto específico do ambiente e onde encontrá-lo.

## <a name="create-an-msbuild-command"></a>Criar um comando do MSBuild

Conforme descrito em [Noções básicas sobre o processo de compilação](understanding-the-build-process.md), o arquivo de projeto específico do ambiente & #x 2014; por exemplo, *Dev.proj Env*& #x 2014; foi projetado para ser importado para o independente do ambiente *Publish.proj* arquivo no momento da compilação. Juntos, esses dois arquivos fornecem um conjunto completo de instruções que dizem MSBuild como criar e implantar sua solução.

O *Publish.proj* arquivo usa uma **importar** elemento para importar o arquivo de projeto específico do ambiente.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Dessa forma, quando você usa MSBuild.exe para criar e implantar a solução de Gerenciador de contato, você precisa:

- Executar MSBuild.exe no *Publish.proj* arquivo.
- Especifique o local do arquivo de projeto específico do ambiente, fornecendo um parâmetro de linha de comando chamado **TargetEnvPropsFile**.

Para fazer isso, o comando MSBuild deve ser semelhante a esta:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


Aqui, é uma etapa simples para mover para uma implantação repetível, a única etapa. Tudo o que você precisa fazer é adicionar o comando MSBuild em um arquivo. cmd. Na solução de Gerenciador de contato, a pasta de publicação inclui um arquivo chamado *Dev.cmd publicar* que faz exatamente isso.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> O **/fl** instrui o MSBuild para criar um arquivo de log chamado *msbuild.log* no diretório de trabalho no qual o MSBuild.exe foi invocado.


Para implantar ou reimplantar a solução de Gerenciador de contato, tudo o que você precisa fazer é executar o *Dev.cmd publicar* arquivo. Quando você executa o arquivo, o MSBuild irá:

- Crie todos os projetos na solução.
- Gere pacotes de implantação web para os projetos de aplicativo da web.
- Gere arquivos .dbschema e .deploymanifest para os projetos de banco de dados.
- Implante os pacotes da web para o servidor web.
- Implante o banco de dados no servidor de banco de dados.

## <a name="run-the-deployment"></a>Executar a implantação

Quando você criou um arquivo de comando para o seu ambiente de destino, você poderá concluir toda a implantação, simplesmente executando o arquivo.

**Para implantar a solução de Gerenciador de contato para seu ambiente de teste**

1. Em sua estação de trabalho do desenvolvedor, abra o Windows Explorer e navegue até o local do *Dev.cmd publicar* arquivo.
2. Clique duas vezes no arquivo para executá-lo.
3. Se um **abrir arquivo – Aviso de segurança** caixa de diálogo for exibida, clique em **executar**.
4. Se suas definições de configuração e servidores de teste estão configurados corretamente, a janela de Prompt de comando mostrará uma **compilação bem-sucedida** mensagem ao MSBuild terminou de processar os arquivos de projeto.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Se esta for a primeira vez em que você implantou a solução para esse ambiente, você precisará adicionar a conta de computador do servidor de web teste para o **db\_datawriter** e **db\_datareader**funções a **ContactManager** banco de dados. Este procedimento é descrito em [configurar um servidor de banco de dados para publicação de implantação da Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Você somente precisa atribuir essas permissões ao criar o banco de dados. Por padrão, o processo de compilação não recriará o banco de dados em cada implantação & #x 2014; em vez disso, ele comparará o banco de dados existente para o esquema mais recente e verifique apenas as alterações necessárias. Como resultado, só será necessário mapear essas funções de banco de dados na primeira vez que você implantar a solução.
6. Abra o Internet Explorer e navegue até a URL do aplicativo Gerenciador de contato (por exemplo, `http://testweb1:85/ContactManager/`).
7. Verifique se o aplicativo funciona conforme o esperado e você pode adicionar contatos.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Conclusão

Criar um arquivo de comando que contém as instruções de MSBuild fornece uma maneira rápida e fácil de criar e implantar uma solução multiprojeto em um ambiente de destino específico. Se você precisar repetidamente implantar sua solução em vários ambientes de destino, você pode criar vários arquivos de comando. Em cada arquivo de comando, o comando MSBuild criará o mesmo arquivo de projeto universal, mas ele especificará um arquivo de projeto específico do ambiente diferente. Por exemplo, um arquivo de comando para publicar para um desenvolvedor ou ambiente de teste pode conter este comando MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Um arquivo de comando para publicar em um ambiente de preparo pode conter este comando MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Para obter orientação sobre como personalizar os arquivos de projeto específico de ambiente para seus próprios ambientes de servidor, consulte [configurar propriedades de implantação de um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Você também pode personalizar o processo de compilação para cada ambiente substituir propriedades ou definindo vários outros parâmetros no comando de MSBuild. Para obter mais informações, consulte [referência de linha de comando do MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

>[!div class="step-by-step"]
[Anterior](deploying-database-projects.md)
[Próximo](manually-installing-web-packages.md)
