---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Implantar uma compilação específica | Microsoft Docs
author: jrjlee
description: Este tópico descreve como implantar pacotes da web e scripts de banco de dados de uma compilação anterior específica para um novo destino, como um ambiente de produção ou preparo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 6d55497dbc13133aa9c8b8eaecca0f6915fd9ed0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388218"
---
<a name="deploying-a-specific-build"></a>Implantando um Build específico
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como implantar pacotes da web e scripts de banco de dados de uma compilação anterior específica para um novo destino, como um ambiente de preparo ou produção.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação e implantação é controlado por dois arquivos de projeto&#x2014;um que contém instruções de compilação que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Até agora, os tópicos neste tutorial conjunto tem explicava como criar, empacotar e implantar aplicativos web e bancos de dados como parte de uma única etapa ou processo automatizado. No entanto, em alguns cenários comuns, você vai querer selecionar os recursos que você implanta em uma lista de compilações em uma pasta-depósito. Em outras palavras, a compilação mais recente não pode ser a compilação que você deseja implantar.

Considere o cenário de integração contínua (CI) descrito no tópico anterior [criação de uma implantação de dá suporte a que definição de compilação](creating-a-build-definition-that-supports-deployment.md). Você criou uma definição de compilação no Team Foundation Server (TFS) 2010. Sempre que um desenvolvedor verifica o código no TFS, Team Build irá compilar seu código, criar pacotes da web e scripts de banco de dados como parte do processo de compilação, executar os testes de unidade e implantar seus recursos em um ambiente de teste. Dependendo da política de retenção configurado quando você criou a definição de compilação, o TFS manterá um determinado número de compilações anteriores.

![](deploying-a-specific-build/_static/image1.png)

Agora, suponha que você executou a verificação de validação de testes em uma destas opções se baseia em seu ambiente de teste e você está pronto para implantar seu aplicativo em um ambiente de preparo. Enquanto isso, os desenvolvedores podem ter check-in novo código. Você não deseja recompilar a solução e implantar o ambiente de preparo, e você não deseja implantar a compilação mais recente para o ambiente de preparo. Em vez disso, você deseja implantar a compilação específica que você tiver verificado e validado em servidores de teste.

Para fazer isso, você precisa informar o Microsoft Build Engine (MSBuild) onde localizar os pacotes da web e scripts de banco de dados gerada de uma compilação específica.

## <a name="overriding-the-outputroot-property"></a>Substituindo a propriedade OutputRoot

No [solução de exemplo](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), o *Publish.proj* arquivo declara uma propriedade chamada **OutputRoot**. Como o nome sugere, essa é a pasta raiz que contém tudo o que gera o processo de compilação. No *Publish.proj* do arquivo, você pode ver que o **OutputRoot** propriedade refere-se para o local raiz para todos os recursos de implantação.

> [!NOTE]
> **OutputRoot** é um nome de propriedade comumente usadas. Arquivos de projeto Visual c# e Visual Basic também declarar esta propriedade para armazenar o local raiz para todas as saídas de compilação.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Se você quiser que o arquivo de projeto para implantar pacotes de web e scripts de um local diferente do banco de dados&#x2014;, como as saídas de um build anterior do TFS&#x2014;você precisa simplesmente substituir o **OutputRoot** propriedade. Você deve definir o valor da propriedade para a pasta de compilação relevantes no servidor do Team Build. Se você estiver executando o MSBuild na linha de comando, você pode especificar um valor para **OutputRoot** como um argumento de linha de comando:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


Na prática, no entanto, você também desejaria ignorar a **construir** destino&#x2014;não há nenhuma razão para criar sua solução se você não planeja usar as saídas de compilação. Você pode fazer isso especificando os destinos que você deseja executar na linha de comando:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


No entanto, na maioria dos casos, você desejará criar sua lógica de implantação em uma definição de compilação do TFS. Isso permite que os usuários com o **enfileirar compilações** permissão para disparar a implantação de qualquer instalação do Visual Studio com uma conexão ao servidor TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Criar uma definição de compilação para implantar específicos compilações

O procedimento a seguir descreve como criar uma definição de compilação que permite aos usuários para as implantações de gatilho para um ambiente de preparo com um único comando.

Nesse caso, você não deseja que a definição de compilação para realmente criar tudo&#x2014;você vai querer executar a lógica de implantação no arquivo de projeto personalizado. O *Publish.proj* arquivo inclui a lógica condicional que ignora a **Build** se o arquivo estiver em execução no Team Build de destino. Ele faz isso avaliando as políticas internas **BuildingInTeamBuild** propriedade, que é automaticamente definida como **verdadeiro** se você executar o arquivo de projeto no Team Build. Como resultado, você pode ignorar o processo de compilação e simplesmente execute o arquivo de projeto para implantar uma compilação existente.

**Para criar uma definição de compilação para disparar a implantação manualmente**

1. No Visual Studio 2010, nos **Team Explorer** janela, expanda o nó do seu projeto de equipe, clique com botão direito **compilações**e, em seguida, clique em **nova definição de compilação**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Sobre o **gerais** guia, dê um nome de definição de compilação (por exemplo, **DeployToStaging**) e uma descrição opcional.
3. Sobre o **disparador** guia, selecione **Manual – Check-ins não disparam uma nova compilação**.
4. No **padrões de compilação** guia, o **copiar saída de compilação para a seguinte pasta-depósito** , digite o caminho de convenção de nomenclatura Universal (UNC) da sua pasta-depósito (por exemplo,  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. Sobre o **processo** guia da **arquivo do processo de compilação** lista suspensa, deixe **DefaultTemplate. XAML** selecionado. Esse é um dos modelos de processo compilação padrão que seja adicionados a todos os novos projetos de equipe.
6. No **parâmetros do processo de compilação** da tabela, clique no **itens para compilar** de linha e, em seguida, clique no **reticências** botão.

    ![](deploying-a-specific-build/_static/image4.png)
7. No **itens para compilar** caixa de diálogo, clique em **Add**.
8. No **itens do tipo** lista suspensa, selecione **arquivos de projeto MSBuild**.
9. Navegue até o local do arquivo de projeto personalizado com o qual você controlar o processo de implantação, selecione o arquivo e, em seguida, clique em **Okey**.

    ![](deploying-a-specific-build/_static/image5.png)
10. No **itens para compilar** caixa de diálogo, clique em **Okey**.
11. No **parâmetros do processo de compilação** da tabela, expanda o **avançado** seção.
12. No **argumentos de MSBuild** de linha, especifique o local do arquivo de projeto específicas do ambiente e adicionar um espaço reservado para o local da sua pasta de compilação:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Você precisará substituir os **OutputRoot** valor toda vez que você enfileira uma compilação. Isso é abordado no próximo procedimento.
13. Clique em **Salvar**.

Quando você disparar uma compilação, você precisa atualizar o **OutputRoot** propriedade para apontar para a compilação que você deseja implantar.

**Para implantar uma compilação específica de uma definição de compilação**

1. No **Team Explorer** , clique com botão direito a definição de compilação e, em seguida, clique em **enfileirar nova compilação**.

    ![](deploying-a-specific-build/_static/image7.png)
2. No **Enfileirar compilação** caixa de diálogo do **parâmetros** guia, expanda o **avançado** seção.
3. No **argumentos de MSBuild** linha, substitua o valor da **OutputRoot** propriedade com o local da sua pasta de compilação. Por exemplo:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Certifique-se de incluir uma barra à direita no final do caminho para a pasta de compilação.
4. Clique em **fila**.

Quando você enfileira a compilação, o arquivo de projeto implantará os scripts de banco de dados e pacotes de web de pasta-depósito de compilação especificado na **OutputRoot** propriedade.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como recursos de implantação, como pacotes da web e scripts de banco de dados de publicação, de um determinado anterior usando o modelo de implantação de arquivo de projeto de divisão de compilação. Ele explicou como substituir a **OutputRoot** como incorporar a lógica de implantação em um TFS e propriedade de definição de compilação.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como criar definições de compilação, consulte [criar uma definição de compilação básica](https://msdn.microsoft.com/library/ms181716.aspx) e [definir seu processo de criação](https://msdn.microsoft.com/library/ms181715.aspx). Para obter mais informações sobre compilações de enfileiramento de mensagens, consulte [enfileirar uma compilação](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Anterior](creating-a-build-definition-that-supports-deployment.md)
> [Próximo](configuring-permissions-for-team-build-deployment.md)
