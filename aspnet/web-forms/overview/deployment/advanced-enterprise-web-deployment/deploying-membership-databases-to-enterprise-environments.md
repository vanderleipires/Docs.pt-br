---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Implantação de bancos de dados de associação em ambientes corporativos | Microsoft Docs
author: jrjlee
description: Este tópico explica as principais considerações e os desafios que você precisará superar ao provisionar bancos de dados do serviços de aplicativo ASP.NET (mais comuns...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: b783fcf57759f2a431480eec6902105f6d683408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Implantação de bancos de dados de associação em ambientes corporativos
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico explica as principais considerações e os desafios que você precisará superar ao provisionar aplicativos ASP.NET serviços bancos de dados (mais comumente chamados de bancos de dados de associação) em ambientes de produção, teste ou preparo. Ele também descreve as abordagens que você pode usar para atender a esses desafios.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Quais são os problemas quando você implantar um banco de dados de associação?

Na maioria dos casos, quando você criar uma estratégia de implantação para um banco de dados, a primeira coisa que você precisa considerar é quais dados você deseja implantar. Em um ambiente de desenvolvimento ou teste, você talvez queira implantar dados de conta de usuário para facilitar o teste rápido e fácil. Em um ambiente de preparo ou de produção, é muito improvável que você deseja implantar os dados da conta de usuário.

Infelizmente, bancos de dados de associação ASP.NET apresentam alguns desafios específicos que tome essa decisão muito mais complexo:

- Uma implantação somente de esquema deixará o banco de dados de associação em um estado não-operacional. Isso ocorre porque o banco de dados de associação inclui alguns dados de configuração (no **aspnet\_SchemaVersions** tabela) que o banco de dados requer para a função. Dessa forma, se você executar uma implantação somente de esquema do banco de dados de associação para excluir dados da conta de usuário, você precisará executar um script de pós-implantação para adicionar os dados de configuração essenciais.
- Dependendo de como seu banco de dados de associação é configurado, o provedor de associação pode usar a chave do computador para criptografar senhas e armazená-los no banco de dados. Nesse caso, nenhum dado de conta de usuário que implantar com o banco de dados se tornará inutilizável no servidor de destino. Por esse motivo, a implantação de dados de conta de usuário não é um cenário com suporte.

## <a name="choosing-a-membership-database-strategy"></a>Escolhendo uma estratégia de banco de dados de associação

Use estas diretrizes ao escolher como provisionar um banco de dados de associação em um ambiente de servidor corporativo:

- Sempre que possível, não implante bancos de dados de associação. Em vez disso, crie o banco de dados de associação manualmente no servidor de banco de dados de destino. Se você não personalizou a esquema de banco de dados de associação, você pode simplesmente criar um novo em situ no destino, usando o [ferramenta de registro de servidor de SQL do ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Se você não tem opção mas ao implantar um banco de dados de associação&#x2014;por exemplo, se você fez grandes modificações no esquema de banco de dados&#x2014;você deve executar uma implantação somente de esquema do banco de dados de associação, para excluir dados da conta de usuário e, em seguida, Execute um script de pós-implantação para adicionar os dados de configuração necessárias. Você pode encontrar diretrizes amplas sobre abordagens de [como: implantar o ASP.NET associação de banco de dados sem incluindo contas de usuário](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

É importante lembrar que *o esquema do banco de dados de associação é provavelmente relativamente estáticos*. Mesmo se você personalizou o banco de dados de associação, é improvável que você precisará atualizar o esquema regularmente&#x2014;não será alterado com a mesma frequência como o código em um aplicativo web ou um projeto de banco de dados. Como tal, você não deve precisa incluir o banco de dados de associação em qualquer processo de implantação automatizada ou única etapa.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Usando VSDBCMD para atualizar um esquema de banco de dados de associação

Se você modificar a estrutura do seu banco de dados de associação após a primeira implantação, não convém usar a ferramenta de implantação da Web de serviços de informações da Internet (IIS) (implantação da Web) para implantar o banco de dados. A funcionalidade de implantação de banco de dados na implantação da Web não inclui a capacidade de fazer atualizações diferenciais para um banco de dados de destino&#x2014;em vez disso, o Web Deploy deve descartar e recriar o banco de dados. Isso significa que você perca quaisquer dados de conta de usuário existente, que é normalmente indesejáveis em ambientes de teste ou produção.

A alternativa é usar o utilitário VSDBCMD para atualizar o esquema de banco de dados de destino. VSDBCMD inclui dois recursos importantes. Primeiro, ele pode importar o esquema do banco de dados existente para um arquivo de .dbschema. Em segundo lugar, ele pode implantar um arquivo de .dbschema para um banco de dados existente como uma atualização diferencial, o que significa que só faz as alterações necessárias para trazer o banco de dados de destino atualizado e você não perderá os dados.

Você pode usar essas etapas de alto nível para atualizar um esquema de banco de dados de associação:

1. Use o VSDBCMD **importação** ação para gerar um arquivo de .dbschema para seu banco de dados de associação de origem. Este procedimento é descrito em [como: importar um esquema de um Prompt de comando](https://msdn.microsoft.com/library/dd172135.aspx).
2. Use o VSDBCMD **implantar** ação para implantar o arquivo .dbschema em seu banco de dados de associação de destino. Este procedimento é descrito em [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Conclusão

Este tópico descritos alguns dos desafios que você pode enfrentar quando você precisa provisionar os bancos de dados de associação ASP.NET em vários ambientes de destino. Em particular, explicado por implantações somente de esquema deixará o banco de dados de associação em um estado não-operacional e por que implantar dados de conta de usuário não tem suporte. O tópico também apresentadas diretrizes sobre como configurar, implantar e atualizar bancos de dados de associação em cenários diferentes.

## <a name="further-reading"></a>Leitura adicional

Para obter mais diretrizes e exemplos de como usar VSDBCMD, consulte [referência de linha de comando para VSDBCMD. EXE (implantação e importação de esquema)](https://msdn.microsoft.com/library/dd193283.aspx) e [como: importar um esquema de um Prompt de comando](https://msdn.microsoft.com/library/dd172135.aspx). Para obter mais informações sobre como usar aspnet\_regsql.exe para criar bancos de dados de associação, consulte [ferramenta de registro de servidor de SQL do ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Para obter orientação geral sobre a implantação de bancos de dados de associação, consulte [como: implantar o ASP.NET associação de banco de dados sem incluindo contas de usuário](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Anterior](deploying-database-role-memberships-to-test-environments.md)
> [Próximo](excluding-files-and-folders-from-deployment.md)
