---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: "Excluindo arquivos e pastas de implantação | Microsoft Docs"
author: jrjlee
description: "Este tópico descreve como você pode excluir arquivos e pastas de um pacote de implantação da web quando você criar e empacotar um projeto de aplicativo web."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: 80810415bac473a58f60110fb9d08772e0627bd5
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
<a name="excluding-files-and-folders-from-deployment"></a>Excluindo arquivos e pastas de implantação
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como você pode excluir arquivos e pastas de um pacote de implantação da web quando você criar e empacotar um projeto de aplicativo web.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo & #x 2014; o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, Windows Serviço do Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos & #x 2014; projeto contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="overview"></a>Visão geral

Quando você compila um projeto de aplicativo web no Visual Studio 2010, o Pipeline de publicação de Web (WPP) permite que você estenda esse processo de compilação ao empacotar seu aplicativo web compilado em um pacote de implantação web. Você pode usar a ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web) para implantar este pacote da web em um servidor de web IIS remoto, ou importar pacote da web manualmente pelo Gerenciador do IIS. Esse processo de empacotamento é explicado em [criação e a projetos de aplicativo Web de empacotamento](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

Como você controla o que é incluído na web do pacote? As configurações de projeto no Visual Studio, por meio do arquivo de projeto subjacente, fornecem controle suficiente para muitos cenários. No entanto, em alguns casos você talvez queira personalizar o conteúdo do pacote da web para ambientes de destino específico. Por exemplo, você talvez queira incluir uma pasta para os arquivos de log quando você implanta seu aplicativo em um ambiente de teste, mas excluir a pasta quando você implanta o aplicativo em um ambiente de preparo ou produção. Neste tópico mostram como fazer isso.

## <a name="what-gets-included-by-default"></a>O que é incluído por padrão?

Quando você configura suas propriedades de projeto de aplicativo web no Visual Studio, o **itens para implantar** lista o **pacote/publicar na Web** página permite que você especifique o que você deseja incluir em sua implantação da web pacote. Por padrão, isso é definido como **apenas os arquivos necessários para executar este aplicativo**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Quando você escolhe **apenas os arquivos necessários para executar este aplicativo**, o WPP tentará determinar quais arquivos devem ser adicionados ao pacote da web. Isso inclui:

- Todos os das saídas de compilação do projeto.
- Todos os arquivos marcados com uma ação de compilação de **conteúdo**.

> [!NOTE]
> A lógica que determina quais arquivos a serem incluídos está contida no arquivo:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Excluindo arquivos e pastas específicas

Em alguns casos, será necessário um controle mais refinado sobre quais arquivos e pastas são implantadas. Se você souber quais arquivos que você deseja excluir depois do tempo e a exclusão se aplica a todos os ambientes de destino, você pode simplesmente definir o **ação de compilação** de cada arquivo para **nenhum**.

**Para excluir arquivos específicos da implantação**

1. No **Solution Explorer** janela, clique no arquivo e, em seguida, clique em **propriedades**.
2. No **propriedades** janela, no **ação de compilação** linha, selecione **nenhum**.

No entanto, essa abordagem não é sempre conveniente. Por exemplo, você pode querer variar quais arquivos e pastas estão incluídas de acordo com seu ambiente de destino e de fora do Visual Studio. Por exemplo, na solução de exemplo de gerente do contato, examine o conteúdo do projeto ContactManager.Mvc:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- A pasta interna contém alguns scripts SQL que o desenvolvedor usa para criar, descartar e popular os bancos de dados locais para fins de desenvolvimento. Nada nessa pasta deve ser implantado em um ambiente de preparo ou produção.
- A pasta de Scripts contém vários arquivos JavaScript. Muitos desses arquivos são incluídos apenas para oferecer suporte à depuração ou forneça IntelliSense no Visual Studio. Alguns desses arquivos não devem ser implantados em ambientes de teste ou produção. No entanto, você talvez queira implantá-los em um ambiente de teste do desenvolvedor para facilitar a solução de problemas.

Embora você pode manipular os arquivos de projeto para excluir arquivos e pastas específicas, há uma maneira mais fácil. O WPP inclui um mecanismo para excluir arquivos e pastas, criando listas de item denominadas **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles**. Você pode estender esse mecanismo adicionando seus próprios itens para essas listas. Para fazer isso, você precisa concluir estas etapas de alto nível:

1. Crie um arquivo de projeto personalizado chamado *.wpp.targets [nome do projeto]* na mesma pasta que o arquivo de projeto.

    > [!NOTE]
    > O *. wpp.targets* arquivos precisam estar na mesma pasta que o arquivo de projeto de aplicativo web & #x 2014; por exemplo, *ContactManager.Mvc.csproj*& #x 2014; em vez de na mesma pasta qualquer arquivos de projeto personalizados que você usa para a compilação de controle e o processo de implantação.
2. No *. wpp.targets* de arquivo, adicione uma **ItemGroup** elemento.
3. No **ItemGroup** elemento, adicionar **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles** itens para excluir arquivos específicos e pastas conforme necessário.

Esta é a estrutura básica desse *. wpp.targets* arquivo:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Observe que cada item inclui um elemento de metadados de item denominado **FromTarget**. Este é um valor opcional que não afeta o processo de compilação. ele simplesmente serve para indicar por que determinados arquivos ou pastas foram omitidas se alguém examina os logs de compilação.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Excluindo arquivos e pastas de um pacote da Web

O procedimento a seguir mostra como adicionar um *. wpp.targets* arquivo para um projeto de aplicativo web e como usar o arquivo para excluir arquivos e pastas específicos do pacote da web quando você compilar o projeto.

**Para excluir arquivos e pastas de um pacote de implantação da web**

1. Abra sua solução no Visual Studio 2010.
2. No **Solution Explorer** janela, clique o nó do projeto de aplicativo web (por exemplo, **ContactManager.Mvc**), aponte para **adicionar**e, em seguida, clique em **Novo Item**.
3. No **Adicionar Novo Item** caixa de diálogo, selecione o **arquivo XML** modelo.
4. No **nome** , digite *[nome do projeto] *.wpp.targets** (por exemplo, **ContactManager.Mvc.wpp.targets**) e, em seguida, clique em **adicionar**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Se você adicionar um novo item para o nó raiz de um projeto, o arquivo é criado na mesma pasta que o arquivo de projeto. Você pode verificar isso abrindo a pasta no Windows Explorer.
5. No arquivo, adicione uma **projeto** elemento e um **ItemGroup** elemento:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Se você quiser excluir pastas do pacote da web, adicione um **ExcludeFromPackageFolders** elemento para o **ItemGroup** elemento:

    1. No **incluir** de atributo, forneça uma lista separada por vírgulas das pastas que você deseja excluir.
    2. No **FromTarget** elemento de metadados, forneça um valor significativo para indicar por que as pastas estão sendo excluídas, como o nome do *. wpp.targets* arquivo.

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Se você quiser excluir arquivos de pacote da web, adicione um **ExcludeFromPackageFiles** elemento para o **ItemGroup** elemento:

    1. No **incluir** de atributo, forneça uma lista separada por vírgulas dos arquivos que você deseja excluir.
    2. No **FromTarget** elemento de metadados, forneça um valor significativo para indicar por que os arquivos estão sendo excluídos, como o nome do *. wpp.targets* arquivo.

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. O *[nome do projeto].wpp.targets* arquivo deve agora ser semelhante a esta:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Salve e feche o *[nome do projeto].wpp.targets* arquivo.

Na próxima vez que você compilação e pacote de seu projeto de aplicativo web, o WPP detectará automaticamente o *. wpp.targets* arquivo. Todos os arquivos e pastas que você especificou não serão incluídas no pacote da web.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como excluir arquivos e pastas específicos quando você cria um pacote da web, criando um personalizado *. wpp.targets* arquivo na mesma pasta que o arquivo de projeto de aplicativo web.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como usar arquivos de projeto Microsoft Build Engine (MSBuild) personalizados para controlar o processo de implantação, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obter mais informações sobre o empacotamento e o processo de implantação, consulte [criação e a projetos de aplicativo Web de empacotamento](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [parâmetros de configuração para implantação do pacote da Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), e [ Implantando pacotes da Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

>[!div class="step-by-step"]
[Anterior](deploying-membership-databases-to-enterprise-environments.md)
[Próximo](taking-web-applications-offline-with-web-deploy.md)
