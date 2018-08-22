---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Implantação de projetos de banco de dados | Microsoft Docs
author: jrjlee
description: 'Observação: Em vários cenários de implantação do enterprise, você precisa da capacidade para publicar atualizações incrementais em um banco de dados implantado. A alternativa é recriar...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 43fa197a1d5a3cf521f4d2202754ff0d121cebe3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830391"
---
<a name="deploying-database-projects"></a>Implantação de projetos de banco de dados
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> Em vários cenários de implantação do enterprise, você precisa ter a capacidade de publicar atualizações incrementais para um banco de dados implantado. A alternativa é recriar o banco de dados em cada implantação, o que significa que você perderá os dados no banco de dados existente. Quando você trabalha com o Visual Studio 2010, usar VSDBCMD é a abordagem recomendada para publicação de banco de dados incrementais. No entanto, a próxima versão do Visual Studio e o Pipeline de publicação de Web (WPP) incluirá as ferramentas que oferece suporte à publicação incremental diretamente.


Se você abrir a solução de exemplo do Gerenciador de contatos no Visual Studio 2010, você verá que o projeto de banco de dados inclui uma pasta de propriedades que contém quatro arquivos.

![](deploying-database-projects/_static/image1.png)

Junto com o arquivo de projeto (*ContactManager.Database.dbproj* neste caso), esses arquivos controlam vários aspectos do processo de compilação e implantação:

- O *Database.sqlcmdvars* arquivo fornece valores para quaisquer variáveis SQLCMD use quando você implanta o projeto. Cada configuração da solução (por exemplo, depuração e versão) pode especificar um arquivo de .sqlcmdvars diferentes.
- O *Database.sqldeployment* arquivo fornece configurações específicas de implantação, como se deve usar o agrupamento definido no seu projeto ou o agrupamento do servidor de destino, se você recriar o banco de dados de destino cada a hora ou simplesmente corrigir o banco de dados existente para colocá-lo até a data e assim por diante. Cada configuração da solução pode especificar um arquivo de .sqldeployment diferentes.
- O *Database.sqlpermissions* arquivo é um documento XML que você pode usar para definir as permissões que você deseja adicionar ao banco de dados de destino. Todas as configurações da solução compartilham o mesmo arquivo. sqlpermissions.
- O *Database.sqlsettings* arquivo Especifica as propriedades de nível de banco de dados a ser usado ao criar o banco de dados, como o agrupamento de usar, o comportamento dos operadores de comparação, e assim por diante. Todas as configurações da solução compartilham o mesmo arquivo. sqlsettings.

Vale a pena gastar algum tempo para abrir esses arquivos no Visual Studio e familiarizar-se com o conteúdo.

Quando você compila um projeto de banco de dados, o processo de compilação cria dois arquivos:

- Um *esquema de banco de dados* (.dbschema arquivo). Descreve o esquema do banco de dados que você deseja criar em formato XML.
- Um *manifesto de implantação* (.deploymanifest arquivo). Isso contém todas as informações necessárias para criar e implantar seu banco de dados. Ele faz referência ao arquivo de .dbschema juntamente com outros recursos, como as instruções de implantação (o arquivo .sqldeployment) e quaisquer scripts SQL de pré-implantação ou pós-implantação.

Isso mostra a relação entre esses recursos:

![](deploying-database-projects/_static/image2.png)

Como você pode ver, o arquivo. sqlsettings e o arquivo. sqlpermissions são entradas para o processo de compilação. Junto com o arquivo de projeto de banco de dados, esses arquivos são usados para criar o arquivo de esquema de banco de dados. O arquivo .sqldeployment e o arquivo de .sqlcmdvars passam pelo processo de compilação inalterado. O manifesto de implantação indica o local do arquivo .sqldeployment, o esquema de banco de dados, o arquivo .sqlcmdvars e quaisquer scripts de pré-implantação ou pós-implantação SQL.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Por que usar VSDBCMD para implantar um projeto de banco de dados?

Há várias abordagens diferentes para implantação de projetos de banco de dados. No entanto, nem todos eles são adequados para implantação de um projeto de banco de dados em servidores remotos em um ambiente corporativo. Considere o que você deseja de uma implantação de projeto de banco de dados. Em cenários de implantação do enterprise, você provavelmente deseja:

- A capacidade de implantar o projeto de banco de dados de um local remoto.
- A capacidade de fazer atualizações incrementais no banco de dados existente.
- A capacidade de incluir scripts de pré-implantação ou pós-implantação.
- A capacidade de personalizar a implantação em vários ambientes de destino.
- A capacidade de implantar o projeto de banco de dados como parte de uma maior, normalmente em script, implantação da solução do passo a passo.

Há três abordagens principais que você pode usar para implantar um projeto de banco de dados:

- Você pode usar a funcionalidade de implantação com o tipo de projeto de banco de dados no Visual Studio 2010. Quando você criar e implanta um projeto de banco de dados no Visual Studio 2010, o processo de implantação usa o manifesto de implantação para gerar um arquivo de implantação com base em SQL específico para a configuração de compilação. Isso criará o banco de dados se ele já não existir ou faça as alterações necessárias no banco de dados se ela já existir. Você pode usar SQLCMD.exe para executar esse arquivo no seu servidor de destino, ou você pode definir o Visual Studio para criar e executar o arquivo. A desvantagem dessa abordagem é que você tem apenas controle limitado sobre as configurações de implantação. Geralmente, eles também, você talvez precise modificar o arquivo de implantação do SQL para fornecer valores de variável de ambiente específicas. Você só pode usar essa abordagem de um computador com o Visual Studio 2010 instalado, e o desenvolvedor precisa saber e forneça cadeias de caracteres de conexão e credenciais para todos os ambientes de destino.
- Você pode usar a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web) para [implantar um banco de dados como parte de um projeto de aplicativo web](https://msdn.microsoft.com/library/dd465343.aspx). No entanto, essa abordagem é muito mais complexa, se você quiser implantar um projeto de banco de dados em vez de simplesmente replicar o banco de dados local existente em um servidor de destino. Você pode configurar a implantação da Web para executar o script de implantação do SQL que gera o projeto de banco de dados, mas para fazer isso, você precisa criar um arquivo de destinos WPP personalizado para seu projeto de aplicativo web. Isso adiciona uma quantidade significativa de complexidade ao processo de implantação. Além disso, implantação da Web não diretamente suporte a atualizações incrementais para bancos de dados existentes. Para obter mais informações sobre essa abordagem, consulte [estender o Pipeline de publicação na Web ao projeto de banco de dados do pacote implantado arquivo SQL](https://go.microsoft.com/?linkid=9805121).
- Você pode usar o utilitário VSDBCMD para implantar o banco de dados, usando o esquema de banco de dados ou o manifesto de implantação. Você pode chamar VSDBCMD.exe de um destino do MSBuild, que lhe permite publicar bancos de dados como parte de um processo de implantação maior, com script. Você pode substituir as variáveis no seu arquivo .sqlcmdvars e muitas outras propriedades de banco de dados de um comando VSDBCMD, que permite que você personalize sua implantação para ambientes diferentes, sem criar várias configurações de build. VSDBCMD fornece a funcionalidade de diferenciação, que significa que ele fará somente as alterações necessárias para alinhar um banco de dados de destino com seu esquema de banco de dados. VSDBCMD também oferece uma ampla variedade de opções de linha de comando, que lhe dá controle refinado sobre o processo de implantação.

Esta visão geral, você pode ver que usar VSDBCMD com o MSBuild é a abordagem mais adequada para um cenário de implantação corporativa típica:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Dá suporte à implantação remota? | Sim | Sim | Sim |
| Dá suporte a atualizações incrementais? | Sim | Não | Sim |
| Dá suporte a scripts de pré/pós-implantação? | Sim | Sim | Sim |
| Dá suporte à implantação de vários ambiente? | Limitado | Limitado | Sim |
| Dá suporte à implantação de scripts? | Limitado | Sim | Sim |

O restante deste tópico descreve o uso de VSDBCMD com o MSBuild para implantar projetos de banco de dados.

## <a name="understanding-the-deployment-process"></a>Noções básicas sobre o processo de implantação

O utilitário VSDBCMD permite implantar um banco de dados usando o esquema de banco de dados (o arquivo .dbschema) ou o manifesto de implantação (o arquivo .deploymanifest). Na prática, você quase sempre usará o manifesto de implantação, como o manifesto de implantação permite que você fornecer valores padrão para várias propriedades de implantação e identificar quaisquer scripts de pré-implantação ou pós-implantação de SQL, que você deseja executar. Por exemplo, este comando VSDBCMD é usado para implantar o **ContactManager** banco de dados para um servidor de banco de dados em um ambiente de teste:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


Nesse caso:

- O **/a** (ou **/Action**) opção especifica o que você deseja VSDBCMD fazer. Você pode definir isso como **importação** ou **implantar**. O **importação** opção é usada para gerar um arquivo de .dbschema do banco de dados existente e o **implantar** opção é usada para implantar um arquivo de .dbschema em um banco de dados de destino.
- O **/manifest** (ou **/ManifestFile**) comutador identifica o arquivo de .deploymanifest que você deseja implantar. Se você quiser usar o arquivo .dbschema em vez disso, você usaria o **/model** (ou **/ModelFile**) alternar.
- O **/cs** (ou **/ConnectionString**) comutador fornece a cadeia de caracteres de conexão para o servidor de banco de dados de destino. Observe que isso não inclui o nome do banco de dados&#x2014;VSDBCMD precisa para se conectar ao servidor para criar o banco de dados; ele não precisa se conectar a um banco de dados individual. Se seu arquivo .deploymanifest inclui uma cadeia de caracteres de conexão, você pode omitir essa opção. Se você usar a opção de qualquer forma, o valor da opção substituirá o valor de .deploymanifest.
- O <strong>/p:TargetDatabase</strong> propriedade fornece o nome que você deseja atribuir ao banco de dados de destino na criação. Isso substitui o valor de <strong>TargetDatabase</strong> propriedade no arquivo .deploymanifest. Você pode usar o <strong>/p:</strong> <em>[nome da propriedade]</em>sintaxe para definir uma ampla variedade de propriedades de implantação e para substituir as variáveis SQLCMD declarado em seu arquivo .sqlcmdvars.
- O **/dd+** (ou **/DeployToDatabase+**) opção indica que você deseja criar uma implantação e implantá-lo para o ambiente de destino. Se você especificar **/dd-**, ou omitir a opção, VSDBCMD irá gerar um script de implantação, mas não vão implantá-lo para o ambiente de destino. Essa opção geralmente é a fonte de confusão e é explicada mais detalhadamente na próxima seção.
- O **/script** (ou **/DeploymentScriptFile**) comutador Especifica onde você deseja gerar o script de implantação. Esse valor não afeta o processo de implantação.

Para obter mais informações sobre VSDBCMD, consulte [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx) e [como: preparar um banco de dados para a implantação em um Prompt de comando usando VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Para obter um exemplo de como você pode usar VSDBCMD de um arquivo de projeto do MSBuild, consulte [Noções básicas sobre o processo de compilação](understanding-the-build-process.md). Para obter exemplos de como definir as configurações de implantação de banco de dados para vários ambientes, consulte [Personalizando as implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Noções básicas sobre a opção DeployToDatabase

O comportamento do **/dd** ou **/DeployToDatabase** switch depende se você estiver usando VSDBCMD com um arquivo de .dbschema ou um arquivo .deploymanifest. Se você estiver usando um arquivo .dbschema, o comportamento é bem simples:

- Se você especificar **/dd+** ou **/dd**, VSDBCMD gerará um script de implantação e implantar o banco de dados.
- Se você especificar **/dd-** ou omitir a opção, VSDBCMD irá gerar um script de implantação.

Se você estiver usando um arquivo .deploymanifest, o comportamento é muito mais complicado. Isso ocorre porque o arquivo .deploymanifest contém um nome de propriedade **DeployToDatabase** que também determina se o banco de dados é implantado.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


O valor dessa propriedade é definido de acordo com as propriedades do projeto de banco de dados. Se você definir a **ação de implantação** à **criar um script de implantação (. SQL)**, o valor será **False**. Se você definir a **ação de implantação** à **criar um script de implantação (. SQL) e implantar o banco de dados**, o valor será **verdadeiro**.

> [!NOTE]
> Essas configurações são associadas com uma plataforma e configuração de compilação específica. Por exemplo, se você definir configurações para o **Debug** configuration e, em seguida, publicar usando o **versão** configuração, suas configurações não serão usadas.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> Nesse cenário, o **ação de implantação** sempre deve ser definido como **criar um script de implantação (. SQL)**, porque você não quer o Visual Studio 2010 para implantar seu banco de dados. Em outras palavras, o **DeployToDatabase** propriedade deve ser sempre **falso**.


Quando um **DeployToDatabase** propriedade for especificada, o **/dd** switch substituirá a propriedade somente se o valor da propriedade for **false**:

- Se o **DeployToDatabase** é de propriedade **falso**, e você especificar **/dd+** ou **/dd**, VSDBCMD substituirá o  **DeployToDatabase** propriedade e implantar o banco de dados.
- Se o **DeployToDatabase** é de propriedade **falso**, e você especificar **/dd-** ou omitir a opção, VSDBCMD não implantará o banco de dados.
- Se o **DeployToDatabase** é de propriedade **verdadeiro**, VSDBCMD ignorará o comutador e implantar o banco de dados.
- Um script de implantação é gerado em cada caso, independentemente se você estiver implantando o banco de dados.

## <a name="conclusion"></a>Conclusão

Este tópico forneceu uma visão geral do processo de compilação e implantação para projetos de banco de dados no Visual Studio 2010. Ele também descreveu como você pode usar VSDBCMD.exe com o MSBuild para dar suporte à implantação de banco de dados de escala empresarial.

Para obter mais informações sobre como isso funciona na prática, consulte [Personalizando as implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Leitura adicional

Para obter informações sobre como personalizar as implantações de banco de dados, criando um arquivo de configuração de implantação separado para cada ambiente, consulte [Personalizando as implantações de banco de dados para vários ambientes](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Para obter orientação sobre como configurar associações de função de banco de dados executando um script de pós-implantação, consulte [Implantando associações de função de banco de dados em ambientes de teste](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Para obter orientação sobre como gerenciar alguns dos desafios exclusivos essa associação de bancos de dados impõem, consulte [implantando bancos de dados de associação para ambientes empresariais](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Estes tópicos no MSDN fornecem orientação mais ampla e informações sobre os projetos de banco de dados do Visual Studio e o processo de implantação de banco de dados:

- [Projetos de banco de dados do Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx)
- [Gerenciamento de alterações do banco de dados](https://msdn.microsoft.com/library/aa833404.aspx)
- [Como: preparar um banco de dados para a implantação em um Prompt de comando usando VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Uma visão geral do banco de dados de compilação e implantação](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-web-packages.md)
> [Próximo](creating-and-running-a-deployment-command-file.md)
