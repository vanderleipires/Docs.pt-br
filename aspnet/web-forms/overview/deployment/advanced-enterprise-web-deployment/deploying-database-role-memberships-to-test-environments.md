---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: "Implantação de associações de função de banco de dados em ambientes de teste | Microsoft Docs"
author: jrjlee
description: "Este tópico descreve como adicionar contas de usuário a funções de banco de dados como parte de uma implantação de solução para um ambiente de teste. Quando você implanta uma solução que contém..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 226c28622f76e866fba1fc33cf9b9b7a01e5295b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Implantação de associações de função de banco de dados em ambientes de teste
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como adicionar contas de usuário a funções de banco de dados como parte de uma implantação de solução para um ambiente de teste.
> 
> Quando você implanta uma solução que contém um projeto de banco de dados em um ambiente de preparo ou de produção, normalmente você não deseja o desenvolvedor para automatizar a adição de contas de usuário a funções de banco de dados. Na maioria dos casos, o desenvolvedor não sabe quais contas de usuário precisam ser adicionados para as funções de banco de dados, e esses requisitos podem mudar a qualquer momento. No entanto, quando você implanta uma solução que contém um projeto de banco de dados para um desenvolvimento ou ambiente de teste, a situação geralmente é diferente:
> 
> - O desenvolvedor normalmente novamente implanta a solução regularmente, geralmente várias vezes ao dia.
> - Novamente, o banco de dados geralmente é criado em cada implantação, o que significa que os usuários de banco de dados devem ser criados e adicionados ao funções após cada implantação.
> - Normalmente, o desenvolvedor tem controle total sobre o ambiente de desenvolvimento ou teste de destino.
> 
> Nesse cenário, geralmente é útil criar usuários de banco de dados e atribuir automaticamente os membros da função de banco de dados como parte do processo de implantação.
> 
> O fator chave é que esta operação deve ser condicional com base no ambiente de destino. Se você estiver implantando em um teste ou um ambiente de produção, você deseja ignorar a operação. Se você estiver implantando em um desenvolvedor ou ambiente de teste, você deseja implantar as associações de função sem intervenção adicional. Este tópico descreve uma abordagem que você pode usar para enfrentar esse desafio.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos de implantação corporativa de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo & #x 2014; o [solução Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, Windows Serviço do Communication Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais baseia-se a abordagem de arquivo de projeto divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos & #x 2014; projeto contendo um crie instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas ao ambiente de compilação e implantação. No momento da compilação, o arquivo de projeto específico do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Este tópico pressupõe que:

- Use a abordagem de arquivo de projeto de divisão para a implantação da solução, conforme descrito em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Chamar VSDBCMD do arquivo de projeto para implantar seu projeto de banco de dados, conforme descrito em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Para criar usuários de banco de dados e atribuir as associações de função quando você implantar um projeto de banco de dados em um ambiente de teste, você precisará:

- Crie um script Transact Structured Query Language (Transact-SQL) que faz as alterações necessárias de banco de dados.
- Crie um destino de Microsoft Build Engine (MSBuild) que usa o utilitário de sqlcmd.exe para executar o script SQL.
- Configure seus arquivos de projeto para invocar o destino quando você estiver implantando sua solução em um ambiente de teste.

Neste tópico mostram como executar cada um desses procedimentos.

## <a name="scripting-the-database-role-memberships"></a>As associações de função de banco de dados de script

Você pode criar um script Transact-SQL em uma grande quantidade de maneiras diferentes, e em qualquer local que você escolher. A abordagem mais simples é criar o script em sua solução no Visual Studio 2010.

**Para criar um script SQL**

1. No **Solution Explorer** janela, expanda o nó do seu projeto de banco de dados.
2. Clique com botão direito do **Scripts** pasta, aponte para **adicionar**e, em seguida, clique em **nova pasta**.
3. Tipo **teste** como o nome da pasta e pressione Enter.
4. Clique com botão direito do **teste** pasta, aponte para **adicionar**e, em seguida, clique em **Script**.
5. No **Adicionar Novo Item** caixa de diálogo caixa, dê um nome significativo de seu script (por exemplo, **AddRoleMemberships.sql**) e, em seguida, clique em **adicionar**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. No *AddRoleMemberships.sql* de arquivo, adicione instruções Transact-SQL que:

    1. Crie um usuário de banco de dados para o logon do SQL Server que irá acessar seu banco de dados.
    2. Adicione o usuário de banco de dados a qualquer função de banco de dados necessárias.
7. O arquivo deve ser semelhante a esta:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Salve o arquivo.

## <a name="executing-the-script-on-the-target-database"></a>Executar o Script no banco de dados de destino

Idealmente, você executaria quaisquer scripts Transact-SQL necessários como parte de um script de pós-implantação quando implantar o projeto de banco de dados. No entanto, scripts de pós-implantação não permitem a execução lógica condicionalmente com base em configurações de solução ou propriedades de compilação. A alternativa é executar os scripts SQL diretamente do arquivo de projeto MSBuild, criando um **destino** elemento que executa um comando de sqlcmd.exe. Você pode usar esse comando para executar o script no banco de dados de destino:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Para obter mais informações sobre as opções de linha de comando sqlcmd, consulte [utilitário sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).


Antes de você inserir esse comando em um destino do MSBuild, você precisa considerar sob que condições você deseja que o script seja executado:

- O banco de dados de destino deve existir antes de alterar suas associações de função. Como tal, você precisa executar este script *depois* a implantação de banco de dados.
- Você precisa incluir uma condição para que o script é executado apenas para ambientes de teste.
- Se você estiver executando uma implantação "e se" & #x 2014; em outras palavras, se você estiver gerando scripts de implantação, mas não realmente em execução-los & #x 2014; você não deve executar o script SQL.

Se você estiver usando a abordagem de arquivo de projeto de divisão descrita em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), conforme demonstrado pela solução de exemplo Contact Manager, você pode dividir as instruções de compilação para o seu script SQL como esta:

- As necessárias propriedades específicas do ambiente, junto com a propriedade que determina se é necessário implantar permissões, devem ficar no arquivo de projeto específico de ambiente (por exemplo, *Dev.proj Env*).
- O destino do MSBuild, junto com as propriedades que não se altere entre ambientes de destino deve ficar no arquivo de projeto universal (por exemplo, *Publish.proj*).

No arquivo de projeto específico do ambiente, você precisa definir uma propriedade booleana que permite que o usuário especifique se deseja implantar as associações de função, o nome do banco de dados de destino e o nome do servidor de banco de dados.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


No arquivo de projeto universal, você precisa fornecer o local do executável sqlcmd e o local do script SQL que você deseja executar. Essas propriedades permanecerá a mesma, independentemente do ambiente de destino. Você também precisa criar um destino do MSBuild para executar o comando sqlcmd.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Observe que você adiciona o local do executável sqlcmd como uma propriedade estática, como isso pode ser útil para outros destinos. Por outro lado, você define o local do seu script SQL e a sintaxe do comando sqlcmd como propriedades dinâmicas no destino, porque eles não serão necessários antes que o destino seja executado. Nesse caso, o **DeployTestDBPermissions** destino só será executado se essas condições forem atendidas:

- O **DeployTestDBRoleMemberships** está definida como **true**.
- O usuário não especificou um **WhatIf = true** sinalizador.

Por fim, não se esqueça de invocar o destino. No *Publish.proj* arquivo, você pode fazer isso adicionando o destino para a lista de dependências para o padrão **FullPublish** destino. Você precisa garantir que o **DeployTestDBPermissions** destino não é executado até que o **PublishDbPackages** destino foi executado.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Conclusão

Este tópico descrito uma maneira na qual você pode adicionar usuários de banco de dados e associações de função como uma ação de pós-implantação ao implantar um projeto de banco de dados. Isso é normalmente útil quando você regularmente recriar um banco de dados em um ambiente de teste, mas geralmente deve ser evitada ao implantar bancos de dados em ambientes de teste ou produção. Como tal, você deve garantir que você use a lógica condicional necessária para que os usuários de banco de dados e associações de função são criadas apenas quando é apropriado fazer isso.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como usar VSDBCMD para implantar projetos de banco de dados, consulte [implantar projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obter orientação sobre como personalizar as implantações de banco de dados para ambientes de destino diferente, consulte [personalizando implantações de banco de dados para vários ambientes](customizing-database-deployments-for-multiple-environments.md). Para obter mais informações sobre como usar arquivos de projeto MSBuild personalizados para controlar o processo de implantação, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obter mais informações sobre as opções de linha de comando sqlcmd, consulte [utilitário sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

>[!div class="step-by-step"]
[Anterior](customizing-database-deployments-for-multiple-environments.md)
[Próximo](deploying-membership-databases-to-enterprise-environments.md)
