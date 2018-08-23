---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
title: Excluindo arquivos e pastas de implantação | Microsoft Docs
author: jrjlee
description: Este tópico descreve como você pode excluir arquivos e pastas de um pacote de implantação da web quando você compilar e empacotar um projeto de aplicativo web.
ms.author: riande
ms.date: 05/04/2012
ms.assetid: f4cc2d40-6a78-429b-b06f-07d000d4caad
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/excluding-files-and-folders-from-deployment
msc.type: authoredcontent
ms.openlocfilehash: fd7357a94ab09effcec86f3725a37cfb2ef4746a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833157"
---
<a name="excluding-files-and-folders-from-deployment"></a>Excluindo arquivos e pastas de implantação
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como você pode excluir arquivos e pastas de um pacote de implantação da web quando você compilar e empacotar um projeto de aplicativo web.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="overview"></a>Visão geral

Quando você compila um projeto de aplicativo web no Visual Studio 2010, o Pipeline de publicação de Web (WPP) permite que você estenda esse processo de compilação ao empacotar seu aplicativo web compilados em um pacote implantável de web. Em seguida, você pode usar a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web) para implantar este pacote da web para um servidor web IIS remoto ou importar o pacote da web manualmente por meio do Gerenciador do IIS. Esse processo de empacotamento é explicado [compilação e empacotamento Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).

Como você controla o que é incluído em sua web do pacote? As configurações do projeto no Visual Studio, por meio do arquivo de projeto subjacente, fornecem controle suficiente para muitos cenários. No entanto, em alguns casos você talvez queira personalizar o conteúdo do pacote da web para ambientes de destino específico. Por exemplo, você talvez queira incluir uma pasta para arquivos de log quando você implanta seu aplicativo em um ambiente de teste, mas excluir a pasta quando você implanta o aplicativo em um ambiente de preparo ou produção. Este tópico mostrará como fazer isso.

## <a name="what-gets-included-by-default"></a>O que é incluído por padrão?

Quando você configura as propriedades do projeto de aplicativo web no Visual Studio, o **itens para implantar** lista os **pacote/Publicar Web** página permite que você especifique o que você deseja incluir em sua implantação da web pacote. Por padrão, isso é definido como **somente os arquivos necessários para executar este aplicativo**.

![](excluding-files-and-folders-from-deployment/_static/image1.png)

Quando você escolhe **somente os arquivos necessários para executar este aplicativo**, o WPP tentará determinar quais arquivos devem ser adicionados ao pacote da web. Isso inclui:

- Todas as saídas de compilação para o projeto.
- Todos os arquivos marcados com uma ação de build **conteúdo**.

> [!NOTE]
> A lógica que determina quais arquivos serão incluídos está contida neste arquivo:   
> *%PROGRAMFILES%\MSBuild\Microsoft\VisualStudio\v10.0\Web\ Microsoft.Web.Publishing.OnlyFilesToRunTheApp.targets*


## <a name="excluding-specific-files-and-folders"></a>Excluindo arquivos e pastas específicas

Em alguns casos, você desejará um controle mais refinado sobre quais arquivos e pastas são implantadas. Se você sabe quais arquivos você deseja excluir à frente de tempo e a exclusão se aplica a todos os ambientes de destino, você pode simplesmente definir a **ação de compilação** de cada arquivo para **None**.

**Para excluir arquivos específicos da implantação**

1. No **Gerenciador de soluções** , clique no arquivo e, em seguida, clique em **propriedades**.
2. No **propriedades** janela, no **Build Action** linha, selecione **None**.

No entanto, essa abordagem nem sempre é conveniente. Por exemplo, talvez você queira variam de quais arquivos e pastas estão incluídas de acordo com seu ambiente de destino e de fora do Visual Studio. Por exemplo, na solução de exemplo do Gerenciador de contatos, vejamos o conteúdo do projeto ContactManager.Mvc:

![](excluding-files-and-folders-from-deployment/_static/image2.png)

- A pasta interna contém alguns scripts SQL que o desenvolvedor usa para criar, descartar e popular os bancos de dados locais para fins de desenvolvimento. Nada nessa pasta deve ser implantado em um ambiente de preparo ou produção.
- A pasta de Scripts contém vários arquivos de JavaScript. Muitos desses arquivos são incluídos puramente para dar suporte à depuração ou fornecer o IntelliSense no Visual Studio. Alguns desses arquivos não devem ser implantados em ambientes de preparo ou produção. No entanto, você talvez queira implantá-los em um ambiente de teste do desenvolvedor para facilitar a solução.

Embora você pode manipular seus arquivos de projeto para excluir arquivos e pastas específicas, há uma maneira mais fácil. O WPP inclui um mecanismo para excluir arquivos e pastas, criando listas de item chamadas **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles**. Você pode estender esse mecanismo, adicionando seus próprios itens para essas listas. Para fazer isso, você precisa concluir estas etapas de alto nível:

1. Crie um arquivo de projeto personalizado chamado *[nome do projeto].wpp.targets* na mesma pasta do arquivo de projeto.

    > [!NOTE]
    > O *. wpp.targets* arquivo precisa estar na mesma pasta que o arquivo de projeto de aplicativo web&#x2014;por exemplo, *ContactManager.Mvc.csproj*&#x2014;, em vez de na mesma pasta como qualquer personalizado Você pode usar para controlar o processo de compilação e implantação de arquivos de projeto.
2. No *. wpp.targets* do arquivo, adicione uma **ItemGroup** elemento.
3. No **ItemGroup** elemento, adicione **ExcludeFromPackageFolders** e **ExcludeFromPackageFiles** itens para excluir arquivos específicos e pastas conforme necessário.

Esta é a estrutura básica desse *. wpp.targets* arquivo:


[!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample1.xml)]


Observe que cada item inclui um elemento de metadados de item nomeado **FromTarget**. Esse é um valor opcional que não afeta o processo de compilação. ele simplesmente serve para indicar por que determinados arquivos ou pastas foram omitidas se alguém analisa os logs de build.

## <a name="excluding-files-and-folders-from-a-web-package"></a>Excluindo arquivos e pastas de um pacote da Web

O procedimento a seguir mostra como adicionar um *. wpp.targets* arquivo para um projeto de aplicativo web e como usar o arquivo para excluir arquivos e pastas específicos do pacote da web quando você compila seu projeto.

**Para excluir arquivos e pastas de um pacote de implantação da web**

1. Abra sua solução no Visual Studio 2010.
2. No **Gerenciador de soluções** janela, com o botão direito nó do projeto de aplicativo web (por exemplo, **ContactManager.Mvc**), aponte para **adicionar**e, em seguida, clique em **Novo Item**.
3. No **Adicionar Novo Item** caixa de diálogo, selecione o **arquivo XML** modelo.
4. No **nome** , digite *[nome do projeto] * * *.wpp.targets** (por exemplo, **ContactManager.Mvc.wpp.targets**) e, em seguida, clique em **Add**.

    ![](excluding-files-and-folders-from-deployment/_static/image3.png)

    > [!NOTE]
    > Se você adicionar um novo item para o nó raiz de um projeto, o arquivo é criado na mesma pasta que o arquivo de projeto. Você pode verificar isso abrindo a pasta no Windows Explorer.
5. No arquivo, adicione uma **Project** elemento e um **ItemGroup** elemento:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample2.xml)]
6. Se você quiser excluir pastas do pacote da web, adicione uma **ExcludeFromPackageFolders** elemento para o **ItemGroup** elemento:

   1. No **Include** de atributo, forneça uma lista separada por vírgulas das pastas que deseja excluir.
   2. No **FromTarget** elemento de metadados, forneça um valor significativo para indicar por que as pastas estão sendo excluídas, como o nome da *. wpp.targets* arquivo.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample3.xml)]
7. Se você quiser excluir arquivos do pacote da web, adicione uma **ExcludeFromPackageFiles** elemento para o **ItemGroup** elemento:

   1. No **Include** de atributo, forneça uma lista separada por ponto e vírgula dos arquivos que você deseja excluir.
   2. No **FromTarget** elemento de metadados, forneça um valor significativo para indicar por que os arquivos estão sendo excluídos, como o nome da *. wpp.targets* arquivo.

      [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample4.xml)]
8. O *[nome do projeto].wpp.targets* arquivo agora deve ser semelhante isso:

    [!code-xml[Main](excluding-files-and-folders-from-deployment/samples/sample5.xml)]
9. Salve e feche o *[nome do projeto].wpp.targets* arquivo.

Na próxima vez que você compilar e empacotar seu projeto de aplicativo web, o WPP detectará automaticamente a *. wpp.targets* arquivo. Todos os arquivos e pastas que você especificou não serão incluídas no pacote da web.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como excluir arquivos e pastas específicos quando você compila um pacote da web, criando um personalizado *. wpp.targets* arquivo na mesma pasta que o arquivo de projeto de aplicativo web.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como usar arquivos de projeto personalizados do Microsoft Build Engine (MSBuild) para controlar o processo de implantação, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obter mais informações sobre o processo de implantação e empacotamento, consulte [compilação e empacotamento Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md), [Configurando parâmetros para a implantação de pacote da Web](../web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment.md), e [ Implantando pacotes da Web](../web-deployment-in-the-enterprise/deploying-web-packages.md).

> [!div class="step-by-step"]
> [Anterior](deploying-membership-databases-to-enterprise-environments.md)
> [Próximo](taking-web-applications-offline-with-web-deploy.md)
