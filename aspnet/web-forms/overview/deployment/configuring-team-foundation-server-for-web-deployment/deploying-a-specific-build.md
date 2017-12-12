---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: "Implantando uma determinada compilação | Microsoft Docs"
author: jrjlee
description: "Este tópico descreve como implantar pacotes da web e scripts de banco de dados de um build anterior específico para um novo destino, como um enviro de produção ou preparo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: afac083c96c1396ad60275fcb55a0ec9c4c0bd44
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-specific-build"></a>Implantando uma compilação específica
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como implantar pacotes da web e scripts de banco de dados de um build anterior específico para um novo destino, como um ambiente de preparo ou produção.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo & #x 2014; o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, Windows Serviço do Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), no qual o processo de compilação e implantação é controlado por meio de dois arquivos de projeto & #x 2014; o ne contendo instruções de compilação que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Até agora, os tópicos neste tutorial conjunto tem foco sobre como criar, empacotar e implantar aplicativos da web e bancos de dados como parte de uma única etapa ou processo automatizado. No entanto, em alguns cenários comuns, você vai querer selecionar os recursos que você implanta em uma lista de compilações em uma pasta-depósito. Em outras palavras, a última compilação não pode ser a compilação que você deseja implantar.

Considere o cenário de integração contínua (CI) descrito no tópico anterior, [criar uma implantação de dá suporte a que definição de compilação](creating-a-build-definition-that-supports-deployment.md). Você criou uma definição de compilação no Team Foundation Server (TFS) 2010. Sempre que um desenvolvedor verifica o código no TFS, Team Build criará seu código, criar pacotes da web e scripts de banco de dados como parte do processo de compilação, executar testes de unidade e implantar seus recursos em um ambiente de teste. Dependendo da política de retenção configurado quando você criou a definição de compilação, o TFS manterá um determinado número de versões anteriores.

![](deploying-a-specific-build/_static/image1.png)

Agora, vamos supor que você executou a verificação e validação de testes em um deles cria no seu ambiente de teste e você está pronto para implantar seu aplicativo em um ambiente de preparo. Enquanto isso, os desenvolvedores podem com check-in novo código. Você não deseja recompilar a solução e implantar o ambiente de preparo, e você não deseja implantar a compilação mais recente para o ambiente de preparo. Em vez disso, você deseja implantar a compilação específica que você tiver verificado e validado em servidores de teste.

Para fazer isso, você precisa informar o Microsoft Build Engine (MSBuild) onde encontrar os pacotes da web e scripts de banco de dados gerada de uma compilação específica.

## <a name="overriding-the-outputroot-property"></a>Substituindo a propriedade OutputRoot

No [solução de exemplo](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), o *Publish.proj* arquivo declara uma propriedade chamada **OutputRoot**. Como o nome sugere, essa é a pasta raiz que contém tudo o que gera o processo de compilação. No *Publish.proj* de arquivos, você pode ver que o **OutputRoot** propriedade refere-se para o local raiz para todos os recursos de implantação.

> [!NOTE]
> **OutputRoot** é um nome de propriedade usadas com frequência. Arquivos de projeto Visual c# e Visual Basic também declarar esta propriedade para armazenar o local raiz para todas as saídas de compilação.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Se você quiser que o arquivo de projeto para implantar pacotes da web e scripts de banco de dados de um local diferente & #x 2014; como as saídas de um build do TFS de anterior & #x 2014; basta substituir a **OutputRoot** propriedade. Você deve definir o valor da propriedade para a pasta de compilação relevantes no servidor do Team Build. Se você estivesse executando o MSBuild da linha de comando, você pode especificar um valor para **OutputRoot** como um argumento de linha de comando:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


Na prática, no entanto, você também deseja ignorar o **criar** destino & #x 2014; não há nenhum ponto na criação de sua solução se você não planeja usar as saídas de compilação. Você pode fazer isso especificando os destinos que você deseja executar na linha de comando:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


No entanto, na maioria dos casos, você desejará criar sua lógica de implantação em uma definição de compilação do TFS. Isso permite que os usuários com o **fila compilações** permissão para disparar a implantação de qualquer instalação do Visual Studio com uma conexão com o servidor do TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Criar uma definição de compilação para implantar específicos compilações

O procedimento a seguir descreve como criar uma definição de compilação que permite aos usuários para implantações de gatilho em um ambiente de preparo com um único comando.

Nesse caso, você não deseja que a definição de compilação para realmente criar tudo & #x 2014; você apenas deseja executar a lógica de implantação em seu arquivo de projeto personalizados. O *Publish.proj* arquivo inclui a lógica condicional que ignora o **criar** se o arquivo estiver em execução no Team Build de destino. Ele faz isso avaliando interno **BuildingInTeamBuild** propriedade, que é definida automaticamente como **true** se você executar o arquivo de projeto no Team Build. Como resultado, você pode ignorar o processo de compilação e simplesmente executar o arquivo de projeto para implantar uma compilação.

**Para criar uma definição de compilação para disparar a implantação manualmente**

1. No Visual Studio 2010, no **Team Explorer** janela, expanda o nó do projeto de equipe, clique com botão direito **cria**e, em seguida, clique em **nova definição de compilação**.

    ![](deploying-a-specific-build/_static/image2.png)
2. Sobre o **geral** guia, nomeie a definição de compilação (por exemplo, **DeployToStaging**) e uma descrição opcional.
3. Sobre o **gatilho** guia, selecione **Manual – Check-ins não disparam uma nova compilação**.
4. No **padrões de compilação** guia o **copiar saída da compilação para a seguinte pasta-depósito** caixa, digite o caminho de convenção de nomenclatura Universal (UNC) da sua pasta de armazenamento (por exemplo,  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. No **processo** guia o **arquivo do processo de compilação** lista suspensa, deixe **DefaultTemplate.xaml** selecionado. Este é um dos modelos de processo padrão compilação que são adicionados a todos os novos projetos de equipe.
6. No **parâmetros de processo de compilação** da tabela, clique no **itens a serem compilados** de linha e, em seguida, clique no **reticências** botão.

    ![](deploying-a-specific-build/_static/image4.png)
7. No **itens a serem compilados** caixa de diálogo, clique em **adicionar**.
8. No **itens do tipo** lista suspensa, selecione **arquivos de projeto MSBuild**.
9. Navegue até o local do arquivo de projeto personalizado com o qual você controla o processo de implantação, selecione o arquivo e, em seguida, clique em **Okey**.

    ![](deploying-a-specific-build/_static/image5.png)
10. No **itens a serem compilados** caixa de diálogo, clique em **Okey**.
11. No **parâmetros de processo de compilação** da tabela, expanda o **avançado** seção.
12. No **argumentos de MSBuild** linha, especifique o local do arquivo do projeto específico do ambiente e adicione um espaço reservado para o local da pasta de compilação:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Você precisará substituir o **OutputRoot** valor sempre enfileirar uma compilação. Isso é abordado no próximo procedimento.
13. Clique em **Salvar**.

Quando você acionar uma compilação, você precisa atualizar o **OutputRoot** propriedade para apontar para a compilação que você deseja implantar.

**Para implantar uma compilação específica de uma definição de compilação**

1. No **Team Explorer** janela, clique na definição de compilação e, em seguida, clique em **enfileirar nova compilação**.

    ![](deploying-a-specific-build/_static/image7.png)
2. No **Enfileirar compilação** caixa de diálogo de **parâmetros** guia, expanda o **avançado** seção.
3. No **argumentos de MSBuild** linha, substitua o valor da **OutputRoot** propriedade com o local da pasta de compilação. Por exemplo:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > Certifique-se de incluir uma barra invertida no final do caminho para a pasta de compilação.
4. Clique em **fila**.

Ao enfileirar a compilação, o arquivo de projeto implantará os scripts de banco de dados e pacotes de web de pasta-depósito compilação especificado no **OutputRoot** propriedade.

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como publicar recursos de implantação, como pacotes da web e scripts de banco de dados, de um determinado anterior usando o modelo de implantação de arquivo de projeto de divisão de compilação. Ele explicado como substituir o **OutputRoot** como incorporar a lógica de implantação em um TFS e propriedade de definição de compilação.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como criar definições de compilação, consulte [criar uma definição básica de compilação](https://msdn.microsoft.com/en-us/library/ms181716.aspx) e [definir seu processo de criação](https://msdn.microsoft.com/en-us/library/ms181715.aspx). Para obter mais diretrizes em compilações de enfileiramento de mensagens, consulte [enfileirar uma compilação](https://msdn.microsoft.com/en-us/library/ms181722.aspx).

>[!div class="step-by-step"]
[Anterior](creating-a-build-definition-that-supports-deployment.md)
[Próximo](configuring-permissions-for-team-build-deployment.md)
