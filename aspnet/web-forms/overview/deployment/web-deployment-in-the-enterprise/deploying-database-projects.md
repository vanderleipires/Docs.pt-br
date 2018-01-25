---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: "Implantação de projetos de banco de dados | Microsoft Docs"
author: jrjlee
description: "Observação: Em muitos cenários de implantação corporativa, você precisa da capacidade para publicar atualizações incrementais para um banco de dados implantado. A alternativa é recriar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 9b1f9a19c76e33b5d996cb4d562cf0c1a3e2f83b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-database-projects"></a>Implantação de projetos de banco de dados
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> Em muitos cenários de implantação corporativa, você precisa da capacidade para publicar atualizações incrementais para um banco de dados implantado. A alternativa é recriar o banco de dados em cada implantação, o que significa que você perderá os dados no banco de dados existente. Quando você trabalha com o Visual Studio 2010, usar VSDBCMD é a abordagem recomendada para a publicação de banco de dados incrementais. No entanto, a próxima versão do Visual Studio e o Pipeline de publicação de Web (WPP) incluirá ferramentas que oferece suporte à publicação incremental diretamente.


Se você abrir a solução de exemplo do Gerenciador de contato no Visual Studio 2010, você verá que o projeto de banco de dados inclui uma pasta de propriedades que contém quatro arquivos.

![](deploying-database-projects/_static/image1.png)

Junto com o arquivo de projeto (*ContactManager.Database.dbproj* nesse caso), esses arquivos controlam vários aspectos do processo de compilação e implantação:

- O *Database.sqlcmdvars* arquivo fornece valores para quaisquer variáveis SQLCMD usa quando você implanta o projeto. Cada configuração de solução (por exemplo, debug e release) pode especificar um arquivo de .sqlcmdvars diferentes.
- O *Database.sqldeployment* arquivo fornece configurações de implantação específicas, como se deseja usar o agrupamento definido no seu projeto ou o agrupamento do servidor de destino, se você recriar o banco de dados de destino cada a hora ou simplesmente corrigir o banco de dados existente para colocá-lo até a data e assim por diante. Cada configuração da solução pode especificar um arquivo de .sqldeployment diferentes.
- O *Database.sqlpermissions* arquivo é um documento XML que você pode usar para definir as permissões que você deseja adicionar ao banco de dados de destino. Todas as configurações de solução compartilham o mesmo arquivo. sqlpermissions.
- O *Database.sqlsettings* arquivo Especifica as propriedades de nível de banco de dados para usar ao criar o banco de dados, como o agrupamento para usar o comportamento dos operadores de comparação e assim por diante. Todas as configurações de solução compartilham o mesmo arquivo. sqlsettings.

Vale a pena gastar alguns instantes para os arquivos forem abertos no Visual Studio e se familiarizar com o conteúdo.

Quando você cria um projeto de banco de dados, o processo de compilação cria dois arquivos:

- Um *esquema de banco de dados* (arquivo .dbschema). Descreve o esquema do banco de dados que você deseja criar em formato XML.
- Um *o manifesto de implantação* (arquivo .deploymanifest). Contém todas as informações necessárias para criar e implantar seu banco de dados. Ele faz referência ao arquivo de .dbschema junto com outros recursos, como as instruções de implantação (o arquivo .sqldeployment) e de quaisquer scripts SQL de pré-implantação ou pós-implantação.

Isso mostra a relação entre esses recursos:

![](deploying-database-projects/_static/image2.png)

Como você pode ver, o arquivo. sqlsettings e o arquivo. sqlpermissions são entradas para o processo de compilação. Junto com o arquivo de projeto de banco de dados, esses arquivos são usados para criar o arquivo de esquema de banco de dados. O arquivo .sqldeployment e o arquivo de .sqlcmdvars passam pelo processo de compilação inalterado. O manifesto de implantação indica o local de quaisquer scripts de pré-implantação ou pós-implantação SQL, o arquivo .sqldeployment, o arquivo .sqlcmdvars e o esquema de banco de dados.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Por que usar VSDBCMD para implantar um projeto de banco de dados?

Há várias abordagens diferentes para implantação de projetos de banco de dados. No entanto, nem todos eles são adequados para implantar um projeto de banco de dados para servidores remotos em um ambiente corporativo. Considere o que você deseja uma implantação de projeto de banco de dados. Em cenários de implantação corporativa, você provavelmente deseja:

- A capacidade de implantar o projeto de banco de dados de um local remoto.
- A capacidade de fazer atualizações incrementais para um banco de dados existente.
- A capacidade de incluir scripts de pré-implantação ou scripts de pós-implantação.
- A capacidade de personalizar a implantação em vários ambientes de destino.
- A capacidade de implantar o projeto de banco de dados como parte de uma maior, normalmente em script, implantação de uma solução única etapa.

Há três abordagens principais que você pode usar para implantar um projeto de banco de dados:

- Você pode usar a funcionalidade de implantação com o tipo de projeto de banco de dados no Visual Studio 2010. Quando você criar e implanta um projeto de banco de dados no Visual Studio 2010, o processo de implantação usa o manifesto de implantação para gerar um arquivo de implantação baseado em SQL específico para a configuração de compilação. Isso criará o banco de dados se ele já não existe ou faça as alterações necessárias para o banco de dados se ele já existe. Você pode usar SQLCMD.exe para executar esse arquivo no servidor de destino, ou você pode definir o Visual Studio para criar e executar o arquivo. A desvantagem dessa abordagem é limitada apenas controle sobre as configurações de implantação. Você também precisará modificar o arquivo de implantação de SQL para fornecer valores de variável de ambiente específicas. Você só pode usar essa abordagem de um computador com Visual Studio 2010 instalado, e o desenvolvedor precisa conhecer e forneça cadeias de caracteres de conexão e credenciais para todos os ambientes de destino.
- Você pode usar a ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web) para [implantar um banco de dados como parte de um projeto de aplicativo web](https://msdn.microsoft.com/library/dd465343.aspx). No entanto, essa abordagem é muito mais complexa, se desejar implantar um projeto de banco de dados em vez de simplesmente replicar um banco de dados local em um servidor de destino. Você pode configurar a implantação da Web para executar o script de implantação de SQL que gera o projeto de banco de dados, mas para fazer isso, você precisa criar um arquivo de destino WPP personalizado para seu projeto de aplicativo web. Isso adiciona uma quantidade significativa de complexidade ao processo de implantação. Além disso, implantação da Web não oferece suporte direto atualizações incrementais para bancos de dados existentes. Para obter mais informações sobre essa abordagem, consulte [estender o Pipeline de publicação na Web ao projeto de banco de dados do pacote implantado arquivo SQL](https://go.microsoft.com/?linkid=9805121).
- Você pode usar o utilitário VSDBCMD para implantar o banco de dados, usando o esquema de banco de dados ou o manifesto de implantação. Você pode chamar VSDBCMD.exe de um destino do MSBuild, que lhe permite publicar bancos de dados como parte de um processo de implantação de maiores e com script. Você pode substituir as variáveis no seu arquivo .sqlcmdvars e muitas outras propriedades de banco de dados de um comando VSDBCMD, que permite que você personalize sua implantação para diferentes ambientes sem criar várias configurações de compilação. VSDBCMD fornece a funcionalidade de diferenciação, o que significa fazer somente as alterações necessárias para alinhar um banco de dados de destino com o esquema de banco de dados. VSDBCMD também oferece uma ampla gama de opções de linha de comando, que oferecem um controle refinado sobre o processo de implantação.

Esta visão geral, você pode ver que usar VSDBCMD com o MSBuild é a abordagem mais adequada para um cenário de implantação corporativa típica:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Dá suporte à implantação remota? | Sim | Sim | Sim |
| Oferece suporte a atualizações incrementais? | Sim | Não | Sim |
| Oferece suporte a scripts de pré/pós-implantação? | Sim | Sim | Sim |
| Dá suporte à implantação de vários ambiente? | Limitado | Limitado | Sim |
| Oferece suporte à implantação de scripts? | Limitado | Sim | Sim |

O restante deste tópico descreve o uso de VSDBCMD com o MSBuild para implantar projetos de banco de dados.

## <a name="understanding-the-deployment-process"></a>Noções básicas sobre o processo de implantação

O utilitário VSDBCMD permite que você implante um banco de dados usando o esquema de banco de dados (o arquivo .dbschema) ou o manifesto de implantação (o arquivo .deploymanifest). Na prática, você quase sempre usará o manifesto de implantação, como o manifesto de implantação permite que você fornecer valores padrão para várias propriedades de implantação e identificar quaisquer scripts SQL pré ou pós-implantação, que você deseja executar. Por exemplo, este comando VSDBCMD é usado para implantar o **ContactManager** banco de dados em um servidor de banco de dados em um ambiente de teste:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


Nesse caso:

- O **/a** (ou **/Action**) opção especifica que você deseja VSDBCMD fazer. Você pode definir isso como **importação** ou **implantar**. O **importação** opção é usada para gerar um arquivo de .dbschema de um banco de dados e o **implantar** opção é usada para implantar um arquivo .dbschema em um banco de dados de destino.
- O **/manifesto** (ou **/ManifestFile**) comutador identifica o arquivo de .deploymanifest que você deseja implantar. Se você quiser usar o arquivo .dbschema em vez disso, você usaria o **/modelo** (ou **/ModelFile**) alternar.
- O **/cs** (ou **/ConnectionString**) proporciona a cadeia de caracteres de conexão para o servidor de banco de dados de destino. Observe que isso não inclui o nome do banco de dados & #x 2014; VSDBCMD precisa se conectar ao servidor para criar o banco de dados; ele não precisa se conectar a um banco de dados individual. Se o arquivo de .deploymanifest inclui uma cadeia de caracteres de conexão, você pode omitir essa opção. Se você usar a opção mesmo assim, o valor da opção substituirá o valor de .deploymanifest.
- O **/p:TargetDatabase** propriedade fornece o nome que você deseja atribuir ao banco de dados de destino durante a criação. Isso substitui o valor de **TargetDatabase** propriedade no arquivo .deploymanifest. Você pode usar o **/p:** *[nome da propriedade]*sintaxe para definir uma ampla variedade de propriedades de implantação e para substituir todas as variáveis SQLCMD declarado no arquivo .sqlcmdvars.
- O **/dd+** (ou **/DeployToDatabase+**) opção indica que você deseja criar uma implantação e implantá-lo para o ambiente de destino. Se você especificar **/dd-**, ou omitir a opção, VSDBCMD irá gerar um script de implantação, mas não a implantar no ambiente de destino. Essa opção geralmente é a fonte de confusão e é explicada mais detalhadamente na próxima seção.
- O **/script** (ou **/DeploymentScriptFile**) comutador Especifica onde você deseja gerar o script de implantação. Esse valor não afeta o processo de implantação.

Para obter mais informações sobre VSDBCMD, consulte [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx) e [como: preparar um banco de dados para a implantação de um Prompt de comando usando VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Para obter um exemplo de como você pode usar VSDBCMD de um arquivo de projeto do MSBuild, consulte [Noções básicas sobre o processo de compilação](understanding-the-build-process.md). Para obter exemplos de como definir as configurações de implantação de banco de dados para vários ambientes, consulte [personalizando implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Noções básicas sobre a opção DeployToDatabase

O comportamento do **/dd** ou **/DeployToDatabase** switch depende se você estiver usando VSDBCMD com um arquivo de .dbschema ou .deploymanifest. Se você estiver usando um arquivo de .dbschema, o comportamento é bastante simples:

- Se você especificar **/dd+** ou **/dd**, VSDBCMD irá gerar um script de implantação e implantar o banco de dados.
- Se você especificar **/dd-** ou omitir a opção, VSDBCMD irá gerar um script de implantação.

Se você estiver usando um arquivo de .deploymanifest, o comportamento é muito mais complicado. Isso ocorre porque o arquivo .deploymanifest contém um nome de propriedade **DeployToDatabase** que também determina se o banco de dados é implantado.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


O valor dessa propriedade é definido de acordo com as propriedades do projeto de banco de dados. Se você definir o **ação de implantação** para **criar um script de implantação (. SQL)**, o valor será **False**. Se você definir o **ação de implantação** para **criar um script de implantação (. SQL) e implantar o banco de dados**, o valor será **True**.

> [!NOTE]
> Essas configurações são associadas com uma configuração de compilação específica e plataforma. Por exemplo, se você definir configurações para o **depurar** configuração e, em seguida, publicar usando o **versão** configuração, suas configurações não serão usadas.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> Nesse cenário, o **ação de implantação** deve sempre ser definido como **criar um script de implantação (. SQL)**, porque não deseja que o Visual Studio 2010 para implantar seu banco de dados. Em outras palavras, o **DeployToDatabase** propriedade deve ser sempre **False**.


Quando um **DeployToDatabase** propriedade for especificada, o **/dd** switch substituirá a propriedade somente se o valor da propriedade é **false**:

- Se o **DeployToDatabase** é de propriedade **False**, e você especificar **/dd+** ou **/dd**, VSDBCMD substituirá o  **DeployToDatabase** propriedade e implantar o banco de dados.
- Se o **DeployToDatabase** é de propriedade **False**, e você especificar **/dd-** ou omitir a opção, VSDBCMD não implantará o banco de dados.
- Se o **DeployToDatabase** é de propriedade **True**, VSDBCMD ignorará o comutador e implantar o banco de dados.
- Um script de implantação é gerado em cada caso, independentemente se você estiver implantando o banco de dados.

## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma visão geral do processo de compilação e implantação para projetos de banco de dados no Visual Studio 2010. Ele também descritos como você pode usar VSDBCMD.exe com o MSBuild para dar suporte à implantação de banco de dados de grande porte.

Para obter mais informações sobre como isso funciona na prática, consulte [personalizando implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Leitura adicional

Para obter informações sobre como personalizar as implantações de banco de dados, criando um arquivo de configuração de implantação separado para cada ambiente, consulte [personalizando implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Para obter orientação sobre como configurar as associações de função de banco de dados executando um script pós-implantação, consulte [Implantando associações de função de banco de dados para ambientes de teste](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Para obter orientação sobre como gerenciar alguns dos desafios exclusivos essa associação bancos de dados impõe, consulte [implantando bancos de dados de associação para ambientes corporativos](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Esses tópicos no MSDN fornecem orientação mais ampla e obter informações sobre os projetos de banco de dados do Visual Studio e o processo de implantação de banco de dados:

- [Projetos de banco de dados do Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx)
- [Gerenciamento de alterações do banco de dados](https://msdn.microsoft.com/library/aa833404.aspx)
- [Como: preparar um banco de dados para a implantação de um Prompt de comando usando VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Uma visão geral do banco de dados de compilação e implantação](https://msdn.microsoft.com/library/aa833165.aspx)

>[!div class="step-by-step"]
[Anterior](deploying-web-packages.md)
[Próximo](creating-and-running-a-deployment-command-file.md)
