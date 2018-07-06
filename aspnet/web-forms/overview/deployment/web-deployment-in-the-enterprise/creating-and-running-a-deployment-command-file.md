---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Criando e executando uma implantação de arquivo de comando | Microsoft Docs
author: jrjlee
description: Este tópico descreve como criar um arquivo de comando que lhe permitirá executar uma implantação usando os arquivos de projeto do Microsoft Build Engine (MSBuild) como uma única etapa, re...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: 6b2a75e0740f648a3a6e4f39c00bd30609325728
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830296"
---
<a name="creating-and-running-a-deployment-command-file"></a>Criando e executando um arquivo de comando de implantação
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como criar um arquivo de comando que permitirá que você execute uma implantação usando arquivos de projeto do Microsoft Build Engine (MSBuild) como um processo passo a passo e reproduzível.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [Contact Manager](the-contact-manager-solution.md) solução&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o processo de compilação](understanding-the-build-process.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="process-overview"></a>Visão geral do processo

Neste tópico, você aprenderá como criar e executar um arquivo de comando que usa esses arquivos de projeto para executar uma implantação repetível em seu ambiente de destino. Essencialmente, o arquivo de comando simplesmente precisa conter um comando do MSBuild que:

- Informa ao MSBuild para executar o ambiente independente *Publish.proj* arquivo.
- Informa o *Publish.proj* arquivo qual arquivo contém as configurações de projeto específicas do ambiente e onde encontrá-lo.

## <a name="create-an-msbuild-command"></a>Criar um comando do MSBuild

Conforme descrito em [Noções básicas sobre o processo de compilação](understanding-the-build-process.md), o arquivo de projeto específicas do ambiente&#x2014;por exemplo, *Env Dev.proj*&#x2014;é projetado para ser importado para o ambiente independente *Publish.proj* arquivo no momento da compilação. Juntos, esses dois arquivos fornecem um conjunto completo de instruções que dizem ao MSBuild como compilar e implantar sua solução.

O *Publish.proj* arquivo usa uma **importar** elemento para importar o arquivo de projeto específicas do ambiente.


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


Dessa forma, quando você usa MSBuild.exe para compilar e implantar a solução do Contact Manager, você precisa:

- Executar MSBuild.exe na *Publish.proj* arquivo.
- Especifique o local do arquivo de projeto específicas do ambiente, fornecendo um parâmetro de linha de comando chamado **TargetEnvPropsFile**.

Para fazer isso, seu comando de MSBuild deve ter esta aparência:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


A partir daqui, é uma etapa simple para mover para uma implantação repetível, passo a passo. Tudo que você precisa fazer é adicionar o comando de MSBuild em um arquivo. cmd. Na solução do Contact Manager, a pasta de publicação inclui um arquivo chamado *Dev.cmd publicar* que faz exatamente isso.


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> O **/fl** opção instrui o MSBuild para criar um arquivo de log chamado *MSBuild* no diretório de trabalho em que MSBuild.exe foi invocado.


Para implantar ou reimplantar a solução de Gerenciador de contatos, tudo o que você precisa fazer é executar o *Dev.cmd publicar* arquivo. Quando você executar o arquivo, o MSBuild irá:

- Compile todos os projetos na solução.
- Gere pacotes de web implantável para projetos de aplicativo web.
- Gere arquivos .dbschema e .deploymanifest para os projetos de banco de dados.
- Implante os pacotes da web para o servidor web.
- Implante o banco de dados no servidor de banco de dados.

## <a name="run-the-deployment"></a>Executar a implantação

Quando você tiver criado um arquivo de comando para o seu ambiente de destino, você poderá concluir a implantação inteira, simplesmente executando o arquivo.

**Para implantar a solução de Gerenciador de contatos em seu ambiente de teste**

1. Em sua estação de trabalho do desenvolvedor, abra o Windows Explorer e, em seguida, navegue até o local do *Dev.cmd publicar* arquivo.
2. Clique duas vezes no arquivo para executá-lo.
3. Se um **abrir arquivo – Aviso de segurança** caixa de diálogo for exibida, clique em **executar**.
4. Se suas definições de configuração e servidores de teste estão configurados corretamente, a janela de Prompt de comando mostrará um **Build bem-sucedido** mensagem ao MSBuild terminou de processar os arquivos de projeto.

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. Se essa for a primeira vez em que você implantou a solução para esse ambiente, você precisará adicionar a conta de máquina de servidor do teste da web para o **db\_datawriter** e **db\_datareader**funções do **ContactManager** banco de dados. Esse procedimento é descrito na [configurar um servidor de banco de dados para publicação de implantação na Web](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).

    > [!NOTE]
    > Você só precisa atribuir essas permissões ao criar o banco de dados. Por padrão, o processo de compilação não recriará o banco de dados em todas as implantações&#x2014;em vez disso, ele comparará o banco de dados existente para o esquema mais recente e fazer somente as alterações necessárias. Como resultado, você só será necessário mapear essas funções de banco de dados na primeira vez que você implantar a solução.
6. Abra o Internet Explorer e navegue até a URL do aplicativo Gerenciador de contatos (por exemplo, `http://testweb1:85/ContactManager/`).
7. Verifique se o aplicativo funciona conforme o esperado e você é capaz de adicionar contatos.

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a>Conclusão

Criar um arquivo de comando que contém as instruções do MSBuild fornece uma maneira rápida e fácil de criar e implantar uma solução com vários projetos em um ambiente de destino específico. Se você precisar implantar repetidamente sua solução em vários ambientes de destino, você pode criar vários arquivos de comando. Em cada arquivo de comando, o comando MSBuild criará o mesmo arquivo de projeto universal, mas ele especificará um arquivo de projeto diferente de específicas do ambiente. Por exemplo, um arquivo de comando para publicar para um desenvolvedor ou ambiente de teste pode conter este comando do MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


Um arquivo de comando para publicar em um ambiente de preparo pode conter este comando do MSBuild:


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> Para obter orientação sobre como personalizar os arquivos de projeto de ambiente específicas para seus próprios ambientes de servidor, consulte [configurar propriedades de implantação para um ambiente de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Você também pode personalizar o processo de compilação para cada ambiente substituindo propriedades ou definindo várias outras opções em seu comando de MSBuild. Para obter mais informações, consulte [referência de linha de comando do MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).

> [!div class="step-by-step"]
> [Anterior](deploying-database-projects.md)
> [Próximo](manually-installing-web-packages.md)
