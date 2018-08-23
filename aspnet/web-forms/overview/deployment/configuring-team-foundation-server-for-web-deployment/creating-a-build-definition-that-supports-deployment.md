---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Criando uma definição de compilação que dá suporte à implantação | Microsoft Docs
author: jrjlee
description: Se você quiser executar qualquer tipo de compilação no Team Foundation Server (TFS) 2010, você precisará criar uma definição de compilação dentro do seu projeto de equipe. Des neste tópico...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: 33ebde3074603801945c676ace64b26ca5bbf44a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834879"
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Criando uma definição de compilação que dá suporte à implantação
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Se você quiser executar qualquer tipo de compilação no Team Foundation Server (TFS) 2010, você precisará criar uma definição de compilação dentro do seu projeto de equipe. Este tópico descreve como criar uma nova definição de compilação no TFS e como controlar a implantação da web como parte do processo de compilação no Team Build.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação e implantação é controlado por dois arquivos de projeto&#x2014;um que contém instruções de compilação que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Uma definição de compilação é o mecanismo que controla como e quando as compilações ocorrem para projetos de equipe no TFS. Especifica cada definição de compilação:

- As coisas que você deseja criar, como arquivos de solução do Visual Studio ou arquivos de projeto personalizados do Microsoft Build Engine (MSBuild).
- Os critérios que determinam quando um build deve levar colocar, como gatilhos manuais, CI (integração contínua), ou check-ins restritos.
- O local ao qual o Team Build devem enviar saídas de compilação, incluindo os artefatos de implantação, como pacotes da web e scripts de banco de dados.
- A quantidade de tempo que cada compilação deve ser mantida.
- Vários outros parâmetros do processo de compilação.

> [!NOTE]
> Para obter mais informações sobre as definições de compilação, consulte [definem seu processo de criação](https://msdn.microsoft.com/library/ms181715.aspx).


Este tópico mostra como criar uma definição de compilação que usa o CI, para que uma compilação é disparada quando um desenvolvedor faz check-in novo conteúdo. Se a compilação for bem-sucedida, o serviço de compilação é executado em um arquivo de projeto personalizado para implantar a solução em um ambiente de teste.

Quando você disparar uma compilação, essas ações precisam ocorrer:

- Primeiro, o Team Build deve compilar a solução. Como parte desse processo, o Team Build irá chamar o Pipeline de publicação de Web (WPP) para gerar pacotes de implantação da web para cada um dos projetos de aplicativo web na solução. O Team Build também executará os testes de unidade associados à solução.
- Se houver falha na criação de soluções, o Team Build deve executar nenhuma ação adicional. Falhas de teste de unidade devem ser tratadas como uma falha de compilação.
- Se a solução de compilação for bem-sucedida, Team Build execute o arquivo de projeto personalizado que controla a implantação da solução. Como parte desse processo, Team Build irá chamar a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web) para instalar os aplicativos empacotados web nos servidores de web de destino e ele invocará o utilitário VSDBCMD.exe para executar a criação do banco de dados scripts nos servidores de banco de dados de destino.

Isso ilustra o processo:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

O [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solução de exemplo inclui um arquivo de projeto personalizado do MSBuild, *Publish.proj*, que podem ser executados do MSBuild ou o Team Build. Conforme descrito em [Noções básicas sobre o processo de Build](../web-deployment-in-the-enterprise/understanding-the-build-process.md), esse arquivo de projeto define a lógica que implanta os pacotes de web e os bancos de dados em um ambiente de destino. O arquivo inclui a lógica que omite a compilação e o processo de empacotamento se ele estiver em execução no Team Build, deixando apenas as tarefas de implantação para ser executado. Isso ocorre porque quando você automatiza a implantação dessa forma, você geralmente desejará garantir que a solução é compilada com êxito e passa os testes de unidade antes de começa o processo de implantação.

A próxima seção explica como implementar esse processo, criando uma nova definição de compilação.

> [!NOTE]
> Este procedimento&#x2014;no qual um único automatizada processo cria, testa e implanta uma solução&#x2014;provavelmente será mais adequado para implantação em ambientes de teste. Para ambientes de preparo e produção, você está muito mais provável que queira implantar o conteúdo de um build anterior que você já tiver verificado e validado em um ambiente de teste. Essa abordagem é descrita no próximo tópico [Implantando um Build específico](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>Quem executa este procedimento?

Normalmente, um administrador do TFS executa este procedimento. Em alguns casos, um líder de equipe do desenvolvedor pode assumir a responsabilidade para a coleção de projeto de equipe no TFS. Para criar uma nova definição de compilação, você precisa ser um membro do **administradores de compilação de coleção de projeto** grupo para a coleção de projeto de equipe que contém sua solução.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Criar uma definição de compilação para implantação e CI

O procedimento a seguir descreve como criar uma definição de compilação CI dispara. Se a compilação for bem-sucedida, a solução é implantada usando a lógica em um arquivo de projeto personalizado do MSBuild.

**Para criar uma definição de compilação para implantação e CI**

1. No Visual Studio 2010, nos **Team Explorer** janela, expanda o nó do seu projeto de equipe, clique com botão direito **compilações**e, em seguida, clique em **nova definição de compilação**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. Sobre o **gerais** guia, dê um nome de definição de compilação (por exemplo, **DeployToTest**) e uma descrição opcional.
3. Sobre o **disparador** guia, selecione os critérios no qual você deseja disparar uma nova compilação. Por exemplo, se você quiser compilar a solução e implante no ambiente de teste sempre que um desenvolvedor faz check-in do novo código, selecione **integração contínua**.
4. No **padrões de compilação** guia, o **copiar saída de compilação para a seguinte pasta-depósito** , digite o caminho de convenção de nomenclatura Universal (UNC) da sua pasta-depósito (por exemplo,  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Esse local de destino armazena várias compilações, dependendo de você configurar a política de retenção. Quando você deseja publicar os artefatos de implantação de uma compilação específica em um ambiente de preparo ou produção, isso é onde você as encontrará.
5. Sobre o **processo** guia da **arquivo do processo de compilação** lista suspensa, deixe **DefaultTemplate. XAML** selecionado. Esse é um dos modelos de processo compilação padrão que seja adicionados a todos os novos projetos de equipe.
6. No **parâmetros do processo de compilação** da tabela, clique no **itens para compilar** de linha e, em seguida, clique no **reticências** botão.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. No **itens para compilar** caixa de diálogo, clique em **Add**.
8. Navegue até o local do arquivo de solução e, em seguida, clique em **Okey**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. No **itens para compilar** caixa de diálogo, clique em **Add**.
10. No **itens do tipo** lista suspensa, selecione **arquivos de projeto MSBuild**.
11. Navegue até o local do arquivo de projeto personalizado com o qual você controlar o processo de implantação, selecione o arquivo e, em seguida, clique em **Okey**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. O **itens para compilar** caixa de diálogo agora deve mostrar dois itens. Clique em **OK**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. Sobre o **processo** guia da **parâmetros do processo de compilação** da tabela, expanda o **avançado** seção.
14. No **argumentos de MSBuild** linha, adicione quaisquer argumentos de linha de comando do MSBuild que *qualquer* de seus itens para compilar requer. No cenário de solução do Contact Manager, esses argumentos são necessários:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. Neste exemplo:

    1. O **DeployOnBuild = true** e **DeployTarget 2=pacote** argumentos são necessários quando você compila a solução de Gerenciador de contatos. Isso instrui o MSBuild para criar web pacotes de implantação depois de criar cada projeto de aplicativo da web, conforme descrito em [compilação e empacotamento Web Application Projects](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. O **TargetEnvPropsFile** argumento é necessário quando você compila a *Publish.proj* arquivo. Essa propriedade indica o local do arquivo de configuração específicas do ambiente, conforme descrito em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. Sobre o **política de retenção** guia, configure a quantas compilações de cada tipo que você deseja manter conforme necessário.
17. Clique em **Salvar**.

## <a name="queue-a-build"></a>Enfileirar uma compilação

Neste ponto, você criou pelo menos uma nova definição de compilação. O processo de compilação que você definiu agora serão executados de acordo com os gatilhos que você especificou na definição de compilação.

Se você tiver configurado sua definição de compilação para usar o CI, você pode testar sua definição de compilação de duas maneiras:

- Fazer check-in algum conteúdo ao projeto de equipe para disparar uma compilação automática.
- Enfileire manualmente uma compilação.

**Para enfileirar uma compilação manualmente**

1. No **Team Explorer** , clique com botão direito a definição de compilação e, em seguida, clique em **enfileirar nova compilação**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. No **Enfileirar compilação** caixa de diálogo, examine as propriedades de build e, em seguida, clique em **fila**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Para revisar o progresso e o resultado de uma compilação&#x2014;, independentemente se ele foi disparado manualmente ou automaticamente&#x2014;clique duas vezes a definição de compilação em de **Team Explorer** janela. Isso abrirá uma **Build Explorer** guia.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

A partir daqui, você pode solucionar problemas de compilações com falha. Se você clicar duas vezes uma compilação individual, você pode exibir informações de resumo e clique para arquivos de log detalhados.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Você pode usar essas informações para solucionar problemas de compilações com falha e resolver quaisquer problemas antes de tentar outro build.

> [!NOTE]
> As compilações que executar a lógica de implantação são provavelmente falhará até que você tiver concedido o servidor de compilação as permissões necessárias no ambiente de destino. Para obter mais informações, consulte [Configurando permissões para a implantação do Team Build](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Monitorar o processo de compilação

O TFS oferece uma ampla gama de funcionalidades para ajudá-lo a monitorar o processo de compilação. Por exemplo, o TFS pode enviar um email ou exibir alertas na área de notificação da barra de tarefas quando uma compilação for concluída. Para obter mais informações, consulte [executar e monitorar compilações](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como criar uma definição de compilação no TFS. A definição de compilação está configurada para CI, portanto, o processo de compilação é executado sempre que um desenvolvedor faz check-in de conteúdo para o projeto de equipe. A definição de compilação executa um arquivo de projeto personalizado do MSBuild para implantar pacotes da web e scripts de banco de dados em um ambiente de servidor de destino.

Em ordem para uma implantação automatizada ter sucesso como parte de um processo de compilação, você precisará conceder permissões apropriadas para a conta de serviço de compilação nos servidores de web de destino e o servidor de banco de dados de destino. O tópico final neste tutorial [Configurando permissões para a implantação do Team Build](configuring-permissions-for-team-build-deployment.md), descreve como identificar e configurar as permissões necessárias para a implantação automática de um servidor do Team Build.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como criar definições de compilação, consulte [criar uma definição de compilação básica](https://msdn.microsoft.com/library/ms181716.aspx) e [definir seu processo de criação](https://msdn.microsoft.com/library/ms181715.aspx). Para obter mais informações sobre compilações de enfileiramento de mensagens, consulte [enfileirar uma compilação](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-a-tfs-build-server-for-web-deployment.md)
> [Próximo](deploying-a-specific-build.md)
