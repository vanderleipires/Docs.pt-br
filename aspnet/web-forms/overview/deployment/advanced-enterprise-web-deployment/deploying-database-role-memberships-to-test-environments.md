---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Implantando associações de função de banco de dados em ambientes de teste | Microsoft Docs
author: jrjlee
description: Este tópico descreve como adicionar contas de usuário a funções de banco de dados como parte de uma implantação de solução para um ambiente de teste. Quando você implanta uma solução que contém...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: a690d99df7a19c422fb217544ec183c311d1796f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828027"
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Implantando associações de função de banco de dados em ambientes de teste
====================
por [Jason Lee](https://github.com/jrjlee)

[Baixar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tópico descreve como adicionar contas de usuário a funções de banco de dados como parte de uma implantação de solução para um ambiente de teste.
> 
> Quando você implanta uma solução que contém um projeto de banco de dados em um ambiente de preparo ou produção, normalmente você não quer o desenvolvedor para automatizar a adição de contas de usuário às funções de banco de dados. Na maioria dos casos, o desenvolvedor não saberá quais contas de usuário precisam ser adicionados a quais funções de banco de dados, e esses requisitos podem mudar a qualquer momento. No entanto, quando você implanta uma solução que contém um projeto de banco de dados para um desenvolvimento ou ambiente de teste, a situação geralmente é bastante diferente:
> 
> - O desenvolvedor normalmente novamente implanta a solução de forma regular, com frequência várias vezes ao dia.
> - O banco de dados geralmente é novamente criado em cada implantação, o que significa que os usuários de banco de dados devem ser criados e adicionados a funções após cada implantação.
> - Normalmente, o desenvolvedor tem controle total sobre o ambiente de desenvolvimento ou teste de destino.
> 
> Nesse cenário, geralmente é útil para automaticamente criar usuários de banco de dados e atribuir as associações de função de banco de dados como parte do processo de implantação.
> 
> O principal fator é que essa operação precisa ser condicional com base no ambiente de destino. Se você estiver implantando para um preparo ou de um ambiente de produção, você deseja ignorar a operação. Se você estiver implantando para um desenvolvedor ou ambiente de teste, você deseja implantar as associações de função sem intervenção adicional. Este tópico descreve uma abordagem que você pode usar para enfrentar esse desafio.


Este tópico faz parte de uma série de tutoriais com base em torno de requisitos corporativos de implantação de uma empresa fictícia chamada Fabrikam, Inc. Esta série de tutoriais usa uma solução de exemplo&#x2014;o [entre em contato com o Gerenciador soluções](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar um aplicativo web com um nível realista de complexidade, incluindo um aplicativo ASP.NET MVC 3, uma comunicação do Windows Serviço Foundation (WCF) e um projeto de banco de dados.

O método de implantação no centro desses tutoriais se baseia a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), em que o processo de compilação é controlado por dois arquivos de projeto&#x2014;uma contendo instruções que se aplicam a todos os ambientes de destino e que contém configurações específicas do ambiente de compilação e implantação de build. No momento da compilação, o arquivo de projeto específicas do ambiente é mesclado no arquivo de projeto de ambiente independente para formar um conjunto completo de instruções de compilação.

## <a name="task-overview"></a>Visão geral da tarefa

Este tópico pressupõe que:

- Você usa a abordagem de arquivo de projeto de divisão para a implantação da solução, conforme descrito em [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Chamar VSDBCMD do arquivo de projeto para implantar seu projeto de banco de dados, conforme descrito em [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Para criar usuários de banco de dados e atribuir as associações de função quando você implanta um projeto de banco de dados em um ambiente de teste, você precisará:

- Crie um script Transact Structured Query Language (Transact-SQL) que faz as alterações de banco de dados necessários.
- Crie um destino de Microsoft Build Engine (MSBuild) que usa o utilitário de sqlcmd.exe para executar o script SQL.
- Configure seus arquivos de projeto para invocar o destino quando você estiver implantando sua solução para um ambiente de teste.

Este tópico mostra como executar cada um desses procedimentos.

## <a name="scripting-the-database-role-memberships"></a>As associações de função de banco de dados de script

Você pode criar um script Transact-SQL em uma grande quantidade de diferentes maneiras e em qualquer local que você escolher. A abordagem mais fácil é criar o script em sua solução no Visual Studio 2010.

**Para criar um script SQL**

1. No **Gerenciador de soluções** janela, expanda o nó do seu projeto de banco de dados.
2. Clique com botão direito do **Scripts** pasta, aponte para **Add**e, em seguida, clique em **nova pasta**.
3. Tipo de **teste** como o nome da pasta e pressione Enter.
4. Clique com botão direito do **teste** pasta, aponte para **Add**e, em seguida, clique em **Script**.
5. No **Adicionar Novo Item** diálogo caixa, dê um nome significativo de seu script (por exemplo, **AddRoleMemberships.sql**) e, em seguida, clique em **adicionar**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. No *AddRoleMemberships.sql* de arquivo, adicione instruções Transact-SQL que:

    1. Crie um usuário de banco de dados para o logon do SQL Server que irá acessar seu banco de dados.
    2. Adicione o usuário de banco de dados para todas as funções de banco de dados necessários.
7. O arquivo deve ter esta aparência:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Salve o arquivo.

## <a name="executing-the-script-on-the-target-database"></a>Executar o Script no banco de dados de destino

O ideal é que você executaria os scripts Transact-SQL necessários como parte de um script de pós-implantação quando você implanta seu projeto de banco de dados. No entanto, os scripts de pós-implantação não permitem que você execute lógica condicionalmente com base em configurações da solução ou as propriedades de compilação. A alternativa é executar seus scripts SQL diretamente do arquivo de projeto MSBuild, criando uma **destino** elemento que executa um comando sqlcmd.exe. Você pode usar esse comando para executar o script no banco de dados de destino:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Para obter mais informações sobre opções de linha de comando sqlcmd, consulte [utilitário sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).


Antes de você incorporar este comando em um destino do MSBuild, você precisa considerar sob quais condições você deseja que o script seja executado:

- O banco de dados de destino deve existir antes de alterar suas associações de função. Como tal, você precisa executar esse script *depois de* a implantação de banco de dados.
- Você precisa incluir uma condição para que o script é executado apenas para ambientes de teste.
- Se você estiver executando uma implantação "what if"&#x2014;em outras palavras, se você estiver gerando scripts de implantação, mas na verdade não executá-los&#x2014;você não deve executar o script SQL.

Se você estiver usando a abordagem de arquivo de projeto divisão descrita [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), conforme demonstrado pela solução de exemplo do Contact Manager, você pode dividir as instruções de compilação para o seu script SQL como esta:

- Todas as necessárias propriedades específicas do ambiente, junto com a propriedade que determina se é necessário implantar permissões, devem ir no arquivo de projeto específicas do ambiente (por exemplo, *Env Dev.proj*).
- O destino do MSBuild em si, junto com quaisquer propriedades que não se altere entre ambientes de destino deve ir no arquivo de projeto universal (por exemplo, *Publish.proj*).

No arquivo de projeto específicas do ambiente, você precisa definir uma propriedade booleana que permite que o usuário especifique se deseja implantar as associações de função, o nome do banco de dados de destino e o nome do servidor de banco de dados.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


No arquivo de projeto universal, você precisa fornecer o local do sqlcmd executável e o local do script SQL que você deseja executar. Essas propriedades permanecerá o mesmo, independentemente do ambiente de destino. Você também precisará criar um destino do MSBuild para executar o comando sqlcmd.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Observe que você adiciona o local do sqlcmd executável como uma propriedade estática, como isso pode ser útil para outros destinos. Por outro lado, você define o local do seu script SQL e a sintaxe do comando sqlcmd como propriedades dinâmicas dentro do destino, pois elas não serão necessárias antes do destino é executado. Nesse caso, o **DeployTestDBPermissions** destino será executado somente se essas condições forem atendidas:

- O **DeployTestDBRoleMemberships** estiver definida como **verdadeiro**.
- O usuário não especificou um **WhatIf = true** sinalizador.

Por fim, não se esqueça de invocar o destino. No *Publish.proj* arquivo, você pode fazer isso adicionando o destino para a lista de dependências para o padrão **FullPublish** destino. Você precisa garantir que o **DeployTestDBPermissions** destino não é executado até que o **PublishDbPackages** destino foi executado.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Conclusão

Este tópico descreveu uma maneira na qual você pode adicionar usuários de banco de dados e associações de função como uma ação pós-implantação quando você implanta um projeto de banco de dados. Isso é geralmente útil quando você regularmente recriar um banco de dados em um ambiente de teste, mas ela geralmente deve ser evitada ao implantar bancos de dados em ambientes de preparo ou produção. Como tal, você deve garantir que você use a lógica condicional necessária para que os usuários de banco de dados e associações de função são criadas apenas quando é apropriado fazer isso.

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre como usar VSDBCMD para implantar projetos de banco de dados, consulte [implantar projetos de banco de dados](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obter orientação sobre como personalizar as implantações de banco de dados para ambientes de destino diferente, consulte [Personalizando as implantações de banco de dados para vários ambientes](customizing-database-deployments-for-multiple-environments.md). Para obter mais informações sobre como usar arquivos de projeto personalizados do MSBuild para controlar o processo de implantação, consulte [Noções básicas sobre o arquivo de projeto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) e [Noções básicas sobre o processo de compilação](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obter mais informações sobre opções de linha de comando sqlcmd, consulte [utilitário sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Anterior](customizing-database-deployments-for-multiple-environments.md)
> [Próximo](deploying-membership-databases-to-enterprise-environments.md)
