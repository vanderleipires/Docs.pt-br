---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Personalizando as implantações de banco de dados para vários ambientes | Microsoft Docs
author: jrjlee
description: 'Este tópico descreve como personalizar as propriedades de um banco de dados para ambientes de destino específico como parte do processo de implantação. Observação: O tópico supõe th...'
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 2ecd73e4eb3cb00545b5ba090ac5f8428586941b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804287"
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Personalizando as implantações de banco de dados para vários ambientes
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como personalizar as propriedades de um banco de dados para ambientes de destino específico como parte do processo de implantação.
> 
> > [!NOTE]
> > O tópico supõe que você está implantando um projeto de banco de dados do Visual Studio 2010 usando MSBuild.exe e VSDBCMD.exe. Para obter mais informações sobre por que você pode escolher essa abordagem, consulte [implantação da Web da empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) e [Implantando projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Quando você implanta um projeto de banco de dados para vários destinos, muitas vezes você desejará personalizar as propriedades de implantação de banco de dados para cada ambiente de destino. Por exemplo, em ambientes de teste seria normalmente recriar o banco de dados em todas as implantações, ao passo que em ambientes de preparo ou produção, você seria muito mais provável que faça atualizações incrementais para preservar seus dados.
> 
> Em um projeto de banco de dados do Visual Studio 2010, as configurações de implantação estão contidas em um arquivo de configuração (.sqldeployment) de implantação. Este tópico mostra como criar arquivos de configuração específicas do ambiente de implantação e especificar a que você deseja usar como um parâmetro VSDBCMD.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Este tópico pressupõe que:

- Você usa a abordagem de arquivo de projeto de divisão para a implantação da solução, conforme descrito em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Chamar VSDBCMD do arquivo de projeto para implantar seu projeto de banco de dados, conforme descrito em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Para criar um sistema de implantação que dá suporte a variando as propriedades de implantação de banco de dados entre ambientes de destino, você precisará:

- Crie um arquivo de configuração (.sqldeployment) de implantação para cada ambiente de destino.
- Crie um comando VSDBCMD que especifica o arquivo de configuração de implantação como uma opção de linha de comando.
- Parametrize o comando VSDBCMD em um arquivo de projeto do Microsoft Build Engine (MSBuild), para que as opções de VSDBCMD são apropriadas para o ambiente de destino.

Este tópico mostra como executar cada um desses procedimentos.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Criando arquivos de configuração específicas do ambiente de implantação

Por padrão, um projeto de banco de dados contém um arquivo de configuração de implantação único chamado *Database.sqldeployment*. Se você abrir esse arquivo no Visual Studio 2010, você pode ver as opções de implantação diferentes que estão disponíveis para você:

- **Agrupamento de comparação de implantação**. Isso permite que você escolha se deseja usar o agrupamento de banco de dados do seu projeto (o *fonte* agrupamento) ou o agrupamento de banco de dados do seu servidor de destino (o *destino* agrupamento). Na maioria dos casos, você desejará usar o agrupamento de origem quando você implantar em um desenvolvimento ou ambiente de teste. Quando você implanta em um ambiente de preparo ou produção, normalmente você desejará mantenha o agrupamento de destino inalterado para evitar problemas de interoperabilidade.
- **Implantar propriedades do banco de dados**. Isso permite que você escolha se deseja aplicar as propriedades de banco de dados, conforme definido na *Database.sqlsettings* arquivo. Quando você implanta um banco de dados pela primeira vez, você deve implantar as propriedades de banco de dados. Se você estiver atualizando um banco de dados existente, as propriedades já devem estar em vigor e você não deve precisar implantá-las novamente.
- **Sempre recriar banco de dados**. Isso permite que você escolha se deseja recriar o banco de dados de destino sempre que você implanta ou fazer alterações incrementais para trazer o banco de dados de destino atualizado com seu esquema. Se você recriar o banco de dados, você perderá todos os dados no banco de dados existente. Como tal, você geralmente defina **falsos** para implantações em ambientes de preparo ou produção.
- **Bloquear implantação incremental se puder ocorrer perda de dados**. Isso permite que você escolha se a implantação será interrompida se uma alteração no esquema de banco de dados causará a perda de dados. Você normalmente defina isso como **verdadeira** para uma implantação em um ambiente de produção, para lhe dar a oportunidade de intervir e proteger dados importantes. Se você tiver definido **sempre recriar banco de dados** à **falso**, essa configuração não terá efeito.
- **Executar a implantação no modo de usuário único**. Isso geralmente não é um problema em ambientes de desenvolvimento ou teste. No entanto, você normalmente deve definir isso **verdadeira** para implantações em ambientes de preparo ou produção. Isso impede que os usuários façam alterações no banco de dados, enquanto a implantação está em andamento.
- **Fazer backup de banco de dados antes da implantação**. Você normalmente defina isso como **verdadeira** quando você implanta em um ambiente de produção, como precaução contra perda de dados. Você também deseja defini-lo como **verdadeira** quando você implanta em um ambiente de preparo, se seu banco de dados de preparo contiver muitos dados.
- **Gerar instruções DROP para objetos que estão no banco de dados de destino, mas que não estão no projeto de banco de dados**. Na maioria dos casos, isso é uma parte integrante e essencial de fazer as alterações incrementais em um banco de dados. Se você tiver definido **sempre recriar banco de dados** à **falso**, essa configuração não terá efeito.
- **Não usar instruções ALTER ASSEMBLY para atualizar tipos CLR**. Essa configuração determina como o SQL Server deve atualizar tipos common language runtime (CLR) para versões mais recentes do assembly. Isso deve ser definido como **falsos** na maioria dos cenários.

Esta tabela mostra as configurações comuns de implantação para ambientes de destino diferente. No entanto, as configurações podem ser diferentes dependendo de suas necessidades exatas.

|  | Desenvolvedor/teste | Preparo/integração | Produção |
| --- | --- | --- | --- |
| **Agrupamento de comparação de implantação** | Origem | Destino | Destino |
| **Implantar propriedades do banco de dados** | verdadeiro | Apenas na primeira vez | Apenas na primeira vez |
| **Sempre recriar banco de dados** | verdadeiro | False | False |
| **Bloquear implantação incremental se puder ocorrer perda de dados** | False | Talvez | verdadeiro |
| **Executar o script de implantação no modo de usuário único** | False | verdadeiro | verdadeiro |
| **Fazer backup de banco de dados antes da implantação** | False | Talvez | verdadeiro |
| **Gerar instruções DROP para objetos que estão no banco de dados de destino, mas que não estão no projeto de banco de dados** | False | verdadeiro | verdadeiro |
| **Não usar instruções ALTER ASSEMBLY para atualizar tipos CLR** | False | False | False |
  

> [!NOTE]
> Para obter mais informações sobre propriedades de implantação de banco de dados e considerações sobre o ambiente, consulte [uma visão geral do banco de dados de configurações do projeto](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [como: configurar as propriedades para obter detalhes de implantação](https://msdn.microsoft.com/library/dd172125.aspx), [ Compilar e implantar o banco de dados em um ambiente de desenvolvimento isolado](https://msdn.microsoft.com/library/dd193409.aspx), e [compilar e implantar bancos de dados para um ambiente de produção ou preparo](https://msdn.microsoft.com/library/dd193413.aspx).


Para dar suporte a implantação de um projeto de banco de dados para vários destinos, você deve criar um arquivo de configuração de implantação para cada ambiente de destino.

**Para criar um arquivo de configuração específicas do ambiente**

1. No Visual Studio 2010, nos **Gerenciador de soluções** , clique em seu projeto de banco de dados e, em seguida, clique em **propriedades**.
2. Na página de propriedades do projeto de banco de dados, sobre o **implantar** guia da **arquivo de configuração de implantação** de linha, clique em **New**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. No **novo arquivo de configuração de implantação** diálogo caixa, dê um nome significativo ao arquivo (por exemplo, **TestEnvironment.sqldeployment**) e, em seguida, clique em **salvar**.
4. Sobre o *[Filename]***.sqldeployment** página, defina as propriedades de implantação para atender às necessidades do seu ambiente de destino e, em seguida, salve o arquivo.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Observe que o novo arquivo é adicionado à pasta propriedades em seu projeto de banco de dados.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Especificando o arquivo de configuração de implantação em VSDBCMD

Quando você usa as configurações da solução (como depuração e versão) dentro do Visual Studio 2010, você pode associar um arquivo de configuração de implantação a cada configuração. Quando você cria uma configuração específica, o processo de compilação gera um arquivo de manifesto de implantação específicas de configuração que aponta para o arquivo de configuração de implantação específicas de configuração. No entanto, um dos principais objetivos da abordagem de implantação descrita nesses tutoriais é conceder a capacidade de controlar o processo de implantação sem usar o Visual Studio 2010 e configurações da solução. Nessa abordagem, a configuração da solução é o mesmo, independentemente do ambiente de implantação de destino. Para personalizar sua implantação de banco de dados em um ambiente de destino específico, você pode usar as opções de linha de comando VSDBCMD para especificar o arquivo de configuração de implantação.

Para especificar um arquivo de configuração de implantação em seu VSDBCMD, use o **p/DeploymentConfigurationFile** alternar e forneça o caminho completo para o arquivo. Isso substituirá o arquivo de configuração de implantação que identifica o manifesto de implantação. Por exemplo, você pode usar esse comando VSDBCMD para implantar o **ContactManager** banco de dados para um ambiente de teste:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Observe que o processo de compilação pode renomear seu arquivo .sqldeployment quando ele copia o arquivo para o diretório de saída.


Se você usar variáveis de comando do SQL em seus scripts de pré-implantação ou pós-implantação SQL, você pode usar uma abordagem semelhante para associar um arquivo de .sqlcmdvars específicas do ambiente com sua implantação. Nesse caso, você usa o **p/SqlCommandVariablesFile** switch para identificar seu arquivo .sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Executando o comando VSDBCMD de um arquivo de projeto do MSBuild

Você pode invocar um comando VSDBCMD de um arquivo de projeto do MSBuild usando um **Exec** tarefa dentro de um destino do MSBuild. Em sua forma mais simples, ela teria esta aparência:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- Na prática, para tornar seus arquivos de projeto fácil de ler e reutilizar, você desejará criar propriedades para armazenar os vários parâmetros de linha de comando. Isso torna mais fácil para os usuários para fornecer valores de propriedade em um arquivo de projeto de ambiente específicas ou para substituir os valores padrão da linha de comando MSBuild. Se você usar a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), você deve dividir suas propriedades entre os dois arquivos e as instruções de build adequadamente:
- Configurações de ambiente específicas, como o nome de arquivo de configuração de implantação, a cadeia de caracteres de conexão de banco de dados e o nome do banco de dados de destino, devem ir no arquivo de projeto específicas do ambiente.
- O destino do MSBuild que executa o comando VSDBCMD, junto com quaisquer propriedades universais, como o local do executável VSDBCMD, deve ir no arquivo de projeto universal.

Você também deve garantir que você compilar o projeto de banco de dados antes de invocar VSDBCMD para que o arquivo .deploymanifest é criado e pronto para uso. Você pode ver um exemplo completo dessa abordagem no tópico [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md), que orienta os arquivos de projeto na [solução de exemplo do Gerenciador de contatos](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Conclusão

Este tópico descreveu como você pode personalizar as propriedades de banco de dados para ambientes de destino diferente quando você implanta os projetos de banco de dados usando o MSBuild e VSDBCMD. Essa abordagem é útil quando você precisa implantar projetos de banco de dados como parte das soluções maiores, de escala empresarial. Geralmente, essas soluções são implantadas para vários destinos, como área restrita e ambientes de desenvolvimento ou teste, preparo ou integração de plataformas e produção ou ambientes em tempo real. Normalmente, cada um desses ambientes de destino requer um conjunto exclusivo de propriedades de implantação de banco de dados.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como implantar projetos de banco de dados usando VSDBCMD.exe, consulte [implantar projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obter mais informações sobre como usar arquivos de projeto personalizados do MSBuild para controlar o processo de implantação, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Estes artigos no MSDN fornecem orientação geral sobre implantação de banco de dados:

- [Uma visão geral das configurações de projeto de banco de dados](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Como: configurar propriedades para obter detalhes de implantação](https://msdn.microsoft.com/library/dd172125.aspx)
- [Compilar e implantar bancos de dados em um ambiente de desenvolvimento isolado](https://msdn.microsoft.com/library/dd193409.aspx)
- [Criar e implantar bancos de dados para um ambiente de produção ou preparo](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Anterior](performing-a-what-if-deployment.md)
> [Próximo](deploying-database-role-memberships-to-test-environments.md)
