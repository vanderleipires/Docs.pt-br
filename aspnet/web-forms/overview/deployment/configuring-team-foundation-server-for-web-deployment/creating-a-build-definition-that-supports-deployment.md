---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Criando uma definição de compilação que dá suporte à implantação | Microsoft Docs
author: jrjlee
description: Se você quiser executar qualquer tipo de compilação no Team Foundation Server (TFS) 2010, você precisa criar uma definição de compilação em seu projeto de equipe. Des neste tópico...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: c5ea0bd9f01bb57b96abd349741f304c0093d887
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Criando uma definição de compilação que dá suporte à implantação
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Se você quiser executar qualquer tipo de compilação no Team Foundation Server (TFS) 2010, você precisa criar uma definição de compilação em seu projeto de equipe. Este tópico descreve como criar uma nova definição de compilação no TFS e como controlar a implantação da web como parte do processo de compilação no Team Build.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação e implantação é controlado por dois arquivos de projeto&#x2014;um que contém instruções de compilação que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Uma definição de compilação é o mecanismo que controla como e quando as compilações ocorrem para projetos de equipe no TFS. Cada definição de compilação especifica:

- As coisas que você deseja criar, como arquivos de solução do Visual Studio ou arquivos de projeto Microsoft Build Engine (MSBuild) personalizados.
- Os critérios que determinam quando uma compilação deve levar colocar, como gatilhos manuais, CI (integração contínua), ou check-ins restritos.
- O local para o qual o Team Build deve enviar saídas de compilação, incluindo artefatos de implantação como pacotes da web e scripts de banco de dados.
- A quantidade de tempo que deve ser mantida cada compilação.
- Vários outros parâmetros do processo de compilação.

> [!NOTE]
> Para obter mais informações sobre definições de compilação, consulte [definir seu processo de criação](https://msdn.microsoft.com/library/ms181715.aspx).


Neste tópico mostram como criar uma definição de compilação que usa CI, para que uma compilação é acionada quando um desenvolvedor verifica no novo conteúdo. Se a compilação for bem-sucedida, o serviço de compilação é executado em um arquivo de projeto personalizados para implantar a solução em um ambiente de teste.

Quando você acionar uma compilação, essas ações precisam ocorrer:

- Primeiro, o Team Build Compile a solução. Como parte desse processo, Team Build chamará o Pipeline de publicação de Web (WPP) para gerar pacotes de implantação da web para cada um dos projetos de aplicativo web na solução. Team Build também executará testes de unidade associados com a solução.
- Se houver falha na criação de solução, Team Build não deve tomar nenhuma ação adicional. Falhas de teste de unidade devem ser tratadas como uma falha de compilação.
- Se a compilação de solução for bem-sucedida, o Team Build deve executar o arquivo de projeto personalizados que controla a implantação da solução. Como parte desse processo, Team Build invocará a ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web) para instalar os aplicativos empacotados web nos servidores da web de destino e ele invocará o utilitário VSDBCMD.exe para executar a criação do banco de dados scripts nos servidores de banco de dados de destino.

Isso ilustra o processo:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

O [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solução de exemplo inclui um arquivo de projeto MSBuild personalizado, *Publish.proj*, que você pode executar do MSBuild ou Team Build. Conforme descrito em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md), esse arquivo de projeto define a lógica que implanta os pacotes de web e os bancos de dados em um ambiente de destino. O arquivo inclui a lógica que omite a construção e o processo de empacotamento, se ele estiver em execução no Team Build, deixando apenas as tarefas de implantação para ser executado. Isso ocorre porque quando você automatizar a implantação dessa forma, você geralmente fará garantir que a solução seja criada com êxito e passa o teste de unidade antes de começa o processo de implantação.

A próxima seção explica como implementar esse processo, criando uma nova definição de compilação.

> [!NOTE]
> Esse procedimento&#x2014;na qual automatizada a um único processo cria, testa e implanta uma solução&#x2014;provavelmente será mais adequado para implantação em ambientes de teste. Para ambientes de preparo e produção estiver muito provavelmente deseja implantar o conteúdo de um build anterior que você já tiver verificado e validado em um ambiente de teste. Essa abordagem é descrita no próximo tópico, [Implantando um Build específico](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Que executa este procedimento?

Normalmente, um administrador do TFS executa este procedimento. Em alguns casos, um líder de equipe do desenvolvedor pode assumir a responsabilidade para a coleção de projeto de equipe no TFS. Para criar uma nova definição de compilação, você precisa ser um membro do **administradores de compilação do projeto coleção** grupo para a coleção de projeto de equipe que contém sua solução.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Criar uma definição de compilação para implantação e CI

O procedimento a seguir descreve como criar uma definição de compilação que dispara o IC. Se a compilação for bem-sucedida, a solução é implantada usando a lógica em um arquivo de projeto MSBuild personalizado.

**Para criar uma definição de compilação de CI e implantação**

1. No Visual Studio 2010, no **Team Explorer** janela, expanda o nó do projeto de equipe, clique com botão direito **cria**e, em seguida, clique em **nova definição de compilação**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Sobre o **geral** guia, nomeie a definição de compilação (por exemplo, **DeployToTest**) e uma descrição opcional.
3. Sobre o **gatilho** guia, selecione os critérios nos quais você deseja disparar uma nova compilação. Por exemplo, se você deseja criar a solução e implantar o ambiente de teste toda vez que um desenvolvedor verifica no novo, selecione **integração contínua**.
4. No **padrões de compilação** guia o **copiar saída da compilação para a seguinte pasta-depósito** caixa, digite o caminho de convenção de nomenclatura Universal (UNC) da sua pasta de armazenamento (por exemplo,  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Esse local de destino armazena várias compilações, dependendo da política de retenção que você configurar. Quando você deseja publicar os artefatos de implantação de uma compilação específica para um ambiente de preparo ou produção, isso é onde você encontrará-los.
5. No **processo** guia o **arquivo do processo de compilação** lista suspensa, deixe **DefaultTemplate.xaml** selecionado. Este é um dos modelos de processo padrão compilação que são adicionados a todos os novos projetos de equipe.
6. No **parâmetros de processo de compilação** da tabela, clique no **itens a serem compilados** de linha e, em seguida, clique no **reticências** botão.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. No **itens a serem compilados** caixa de diálogo, clique em **adicionar**.
8. Navegue até o local do arquivo de solução e, em seguida, clique em **Okey**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. No **itens a serem compilados** caixa de diálogo, clique em **adicionar**.
10. No **itens do tipo** lista suspensa, selecione **arquivos de projeto MSBuild**.
11. Navegue até o local do arquivo de projeto personalizado com o qual você controla o processo de implantação, selecione o arquivo e, em seguida, clique em **Okey**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. O **itens a serem compilados** caixa de diálogo agora deve mostrar os dois itens. Clique em **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. No **processo** guia o **parâmetros de processo de compilação** da tabela, expanda o **avançado** seção.
14. No **argumentos de MSBuild** de linha, adicione quaisquer argumentos de linha de comando do MSBuild que *ou* os itens para criar requer. No cenário de solução do gerente do contato, esses argumentos são necessários:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. Neste exemplo:

    1. O **DeployOnBuild = true** e **DeployTarget = pacote** argumentos são necessários quando você compila a solução do gerente do contato. Isso instrui o MSBuild para criar web pacotes de implantação depois de criar cada projeto de aplicativo da web, conforme descrito em [criação e a projetos de aplicativo Web de empacotamento](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. O **TargetEnvPropsFile** argumento é necessário quando você compila o *Publish.proj* arquivo. Essa propriedade indica o local do arquivo de configuração específico do ambiente, conforme descrito em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Sobre o **política de retenção** guia, defina quantas compilações de cada tipo que você deseja manter conforme necessário.
17. Clique em **Salvar**.

## <a name="queue-a-build"></a>Enfileirar uma compilação

Neste ponto, você criou pelo menos uma nova definição de compilação. O processo de compilação que você definiu será executado de acordo com os disparadores especificado na definição de compilação.

Se você tiver configurado a sua definição de compilação para usar CI, você pode testar sua definição de compilação de duas maneiras:

- Verifique se algum conteúdo para o projeto de equipe para disparar uma compilação automática.
- Enfileire uma compilação manualmente.

**Para enfileirar uma compilação manualmente**

1. No **Team Explorer** janela, clique na definição de compilação e, em seguida, clique em **enfileirar nova compilação**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. No **Enfileirar compilação** caixa de diálogo, analise as propriedades de compilação e, em seguida, clique em **fila**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Para analisar o progresso e o resultado de uma compilação&#x2014;independentemente se ele foi acionado manual ou automaticamente&#x2014;clique duas vezes a definição de compilação no **Team Explorer** janela. Isso abrirá um **Build Explorer** guia.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

A partir daqui, você pode solucionar falhas de construção. Se você clicar duas vezes uma compilação individual, você pode exibir informações de resumo e clique para arquivos de log detalhados.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Você pode usar essas informações para solucionar problemas de compilações com falha e resolver os problemas antes de tentar outra.

> [!NOTE]
> Compilações que executar lógica de implantação são provavelmente falharão até que você tiver concedido o servidor de compilação todas as permissões necessárias no ambiente de destino. Para obter mais informações, consulte [configurar permissões para a implantação de compilação de equipe](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Monitorar o processo de compilação

TFS fornece uma ampla gama de recursos para ajudá-lo a monitorar o processo de compilação. Por exemplo, o TFS pode enviar um email ou exibir alertas na área de notificação da barra de tarefas quando uma compilação for concluída. Para obter mais informações, consulte [executar e monitorar compilações](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como criar uma definição de compilação no TFS. A definição de compilação está configurada para CI, para que o processo de compilação é executado sempre que verifica se um desenvolvedor de conteúdo para o projeto de equipe. A definição de compilação executa um arquivo de projeto MSBuild personalizado para implantar pacotes da web e scripts de banco de dados em um ambiente de servidor de destino.

A fim de uma implantação automatizada para se tornar parte de um processo de compilação, você precisará conceder as permissões apropriadas para a conta de serviço de compilação nos servidores de web de destino e o servidor de banco de dados de destino. O tópico final neste tutorial, [configurar permissões para a implantação de compilação de equipe](configuring-permissions-for-team-build-deployment.md), descreve como identificar e configurar as permissões necessárias para a implantação automática de um servidor do Team Build.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como criar definições de compilação, consulte [criar uma definição básica de compilação](https://msdn.microsoft.com/library/ms181716.aspx) e [definir seu processo de criação](https://msdn.microsoft.com/library/ms181715.aspx). Para obter mais diretrizes em compilações de enfileiramento de mensagens, consulte [enfileirar uma compilação](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-a-tfs-build-server-for-web-deployment.md)
> [Próximo](deploying-a-specific-build.md)
