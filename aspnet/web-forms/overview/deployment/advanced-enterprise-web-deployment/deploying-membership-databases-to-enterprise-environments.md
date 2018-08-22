---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Implantando bancos de dados de associação em ambientes empresariais | Microsoft Docs
author: jrjlee
description: Este tópico explica as principais considerações e desafios que você precisará superar quando você provisiona os bancos de dados do serviços de aplicativos ASP.NET (mais comum...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 307375843c51f31d3d8ae0f2ef0a17a3e58d3a64
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834052"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Implantando bancos de dados de associação em ambientes empresariais
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico explica as principais considerações e desafios que você precisará superar quando você provisionar aplicativo ASP.NET serviços bancos de dados (mais comumente conhecidos como bancos de dados de associação) em ambientes de teste, preparo ou produção. Ele também descreve as abordagens que você pode usar para atender a esses desafios.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Quais são os problemas quando você implanta um banco de dados de associação?

Na maioria dos casos, quando você criar uma estratégia de implantação para um banco de dados, a primeira coisa que você precisa considerar é quais dados você deseja implantar. Em um ambiente de desenvolvimento ou teste, você talvez queira implantar dados da conta de usuário para facilitar o teste rápida e fácil. Em um ambiente de preparo ou produção, é muito improvável que você desejaria implantar dados de conta de usuário.

Infelizmente, os bancos de dados de associação ASP.NET apresentam alguns desafios específicos que tornam essa decisão muito mais complexo:

- Uma implantação somente de esquema deixará o banco de dados de associação em um estado não operacional. Isso ocorre porque o banco de dados de associação inclui alguns dados de configuração (na **aspnet\_SchemaVersions** tabela) que o banco de dados requer para funcionar. Dessa forma, se você executar uma implantação somente de esquema do banco de dados de associação para excluir dados da conta de usuário, você precisará executar um script de pós-implantação para adicionar os dados de configuração do essential.
- Dependendo de como seu banco de dados de associação é configurado, o provedor de associação pode usar a chave do computador para criptografar senhas e armazená-los no banco de dados. Nesse caso, quaisquer dados de conta de usuário que você implantar com o banco de dados poderá ficar inutilizáveis no servidor de destino. Por esse motivo, a implantação de dados de conta de usuário não é um cenário com suporte.

## <a name="choosing-a-membership-database-strategy"></a>Escolhendo uma estratégia de banco de dados de associação

Use estas diretrizes ao escolher como provisionar um banco de dados de associação em um ambiente de servidor corporativo:

- Sempre que possível, não implante bancos de dados de associação. Em vez disso, crie o banco de dados de associação manualmente no servidor de banco de dados de destino. Se você ainda não tiver personalizado seu esquema de banco de dados de associação, você pode simplesmente criar um novo em situ de destino usando o [ferramenta de registro do ASP.NET SQL Server (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Se você não tem opção, mas para implantar um banco de dados de associação&#x2014;por exemplo, se você tiver feito modificações abrangentes para o esquema de banco de dados&#x2014;você deve executar uma implantação somente de esquema do banco de dados de associação, para excluir dados da conta de usuário e, em seguida, Execute um script de pós-implantação para adicionar dados de qualquer configuração necessária. Você pode encontrar diretrizes amplas nessas abordagens [como: implantar o ASP.NET associação de banco de dados sem incluindo contas de usuário](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

É importante lembrar-se de que *o esquema de banco de dados de associação é provavelmente será razoavelmente estático*. Mesmo se você personalizou o banco de dados de associação, é improvável que você precisará atualizar o esquema regularmente&#x2014;ele não será alterado com a mesma frequência que o código em um aplicativo web ou um projeto de banco de dados. Como tal, não deve ser necessário incluir o banco de dados de associação em todos os processos automatizados ou passo a passo de implantação.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Usando VSDBCMD para atualizar um esquema de banco de dados de associação

Se você modificar a estrutura do seu banco de dados de associação após sua primeira implantação, não convém usar a ferramenta de implantação do Internet Information Services (IIS) da Web (implantação da Web) para reimplantar o banco de dados. A funcionalidade de implantação de banco de dados na implantação da Web não inclui a capacidade de fazer atualizações diferenciais para um banco de dados de destino&#x2014;em vez disso, o Web Deploy deve descartar e recriar o banco de dados. Isso significa que você perderá quaisquer dados de conta de usuário existente, que é normalmente indesejáveis em ambientes de preparo ou produção.

A alternativa é usar o utilitário VSDBCMD para atualizar o esquema de banco de dados de destino. VSDBCMD inclui dois recursos importantes. Em primeiro lugar, ele pode importar o esquema do banco de dados existente para um arquivo .dbschema. Em segundo lugar, ele pode implantar um arquivo de .dbschema para um banco de dados como uma atualização diferencial, o que significa que ele só faz as alterações necessárias para colocar o banco de dados de destino atualizado e você não perderá os dados.

Você pode usar essas etapas de alto nível para atualizar um esquema de banco de dados de associação:

1. Use o VSDBCMD **importação** ação para gerar um arquivo .dbschema do banco de dados de associação de origem. Esse procedimento é descrito na [como: importar um esquema de um Prompt de comando](https://msdn.microsoft.com/library/dd172135.aspx).
2. Use o VSDBCMD **Deploy** ação para implantar o arquivo .dbschema em seu banco de dados de associação de destino. Esse procedimento é descrito na [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Conclusão

Este tópico descreveu alguns dos desafios que você pode enfrentar quando você precisa provisionar bancos de dados de associação ASP.NET em vários ambientes de destino. Em particular, explicado por que as implantações de somente esquema deixará o banco de dados de associação em um estado não operacional e por que implantar dados da conta de usuário não tem suporte. O tópico também apresentadas orientações sobre como provisionar, implantar e atualizar bancos de dados de associação em cenários diferentes.

## <a name="further-reading"></a>Leitura adicional

Para obter mais diretrizes e exemplos de como usar VSDBCMD, consulte [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx) e [como: importar um esquema de um Prompt de comando](https://msdn.microsoft.com/library/dd172135.aspx). Para obter mais informações sobre como usar aspnet\_regsql.exe para criar bancos de dados de associação, consulte [ferramenta de registro do ASP.NET SQL Server (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Para obter orientação geral sobre a implantação de bancos de dados de associação, consulte [como: implantar o ASP.NET associação de banco de dados sem incluindo contas de usuário](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Anterior](deploying-database-role-memberships-to-test-environments.md)
> [Próximo](excluding-files-and-folders-from-deployment.md)
