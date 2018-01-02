---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
title: "Criando o esquema de associação no SQL Server (c#) | Microsoft Docs"
author: rick-anderson
description: "Este tutorial inicia examinando técnicas para adicionar o esquema necessário para o banco de dados para usar o SqlMembershipProvider. Depois disso, podemos wi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: b4ac129d-1b8e-41ca-a38f-9b19d7c7bb0e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs
msc.type: authoredcontent
ms.openlocfilehash: 2155f9b0893a0b1d3cf60bc63d80df4417649beb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-the-membership-schema-in-sql-server-c"></a>Criando o esquema de associação no SQL Server (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_cs.pdf)

> Este tutorial inicia examinando técnicas para adicionar o esquema necessário para o banco de dados para usar o SqlMembershipProvider. Depois disso, vamos examinar as principais tabelas no esquema e discutir o seu objetivo e a importância. Este tutorial termina com uma olhada em como saber qual provedor deve usar a estrutura de associação de um aplicativo ASP.NET.


## <a name="introduction"></a>Introdução

Dois tutoriais anteriores examinados usando a autenticação de formulários para identificar os visitantes do site. A estrutura de autenticação de formulários facilita para os desenvolvedores do logon de um usuário em um site e lembre-se em visitas de página com o uso de tíquetes de autenticação. O `FormsAuthentication` classe inclui métodos para gerar o tíquete e adicioná-lo para cookies do visitante. O `FormsAuthenticationModule` examina todas as solicitações de entrada e, para aqueles com um tíquete de autenticação válido, cria e associa um `GenericPrincipal` e um `FormsIdentity` objeto com a solicitação atual. Autenticação de formulários é simplesmente um mecanismo para conceder um tíquete de autenticação para um visitante ao fazer logon no e, em solicitações subsequentes, analisando esse tíquete para determinar a identidade do usuário. Para um aplicativo web dar suporte a contas de usuário, ainda é preciso implementar um repositório de usuário e adiciona a funcionalidade para validar as credenciais, registrar novos usuários e a variedade de outras tarefas relacionadas à conta de usuário.

Antes do ASP.NET 2.0, os desenvolvedores estavam em gancho para implementar todas as tarefas relacionadas à conta de usuário. Felizmente, a equipe do ASP.NET reconhecido este empecilho e introduziu a estrutura de associação com o ASP.NET 2.0. A estrutura de associação é um conjunto de classes do .NET Framework que fornecem uma interface programática para realizar tarefas relacionadas à conta de usuário principal. Essa estrutura é construída sobre o [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que permite aos desenvolvedores conecte uma implementação personalizada de uma API padronizada.

Como discutido o <a id="Tutorial1"> </a> [ *Noções básicas sobre segurança e suporte ao ASP.NET* ](../introduction/security-basics-and-asp-net-support-cs.md) tutorial, o .NET Framework vem com dois provedores de associação internas: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/en-us/library/system.web.security.activedirectorymembershipprovider.aspx) e [ `SqlMembershipProvider` ](https://msdn.microsoft.com/en-us/library/system.web.security.sqlmembershipprovider.aspx). Como o nome sugere, o `SqlMembershipProvider` usa um banco de dados do Microsoft SQL Server como o repositório do usuário. Para usar esse provedor em um aplicativo, é preciso instruir o provedor qual banco de dados para usar como o repositório. Como você pode imaginar, a `SqlMembershipProvider` espera que o banco de dados de repositório de usuário para determinadas tabelas de banco de dados, exibições e procedimentos armazenados. É preciso adicionar esse esquema esperado para o banco de dados selecionado.

Este tutorial inicia examinando técnicas para adicionar o esquema necessário para o banco de dados para usar o `SqlMembershipProvider`. Depois disso, vamos examinar as principais tabelas no esquema e discutir o seu objetivo e a importância. Este tutorial termina com uma olhada em como saber qual provedor deve usar a estrutura de associação de um aplicativo ASP.NET.

Vamos começar!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Etapa 1: Decidir onde colocar o armazenamento do usuário

Dados de um aplicativo ASP.NET geralmente são armazenados em um número de tabelas em um banco de dados. Ao implementar o `SqlMembershipProvider` esquema de banco de dados, deverá decidir se deseja colocar o esquema de associação no mesmo banco de dados como os dados do aplicativo ou em um banco de dados alternativo.

É recomendável localizar o esquema de associação no mesmo banco de dados como os dados do aplicativo pelos seguintes motivos:

- **Facilidade de manutenção** um aplicativo cujos dados são encapsulados em um banco de dados é mais fácil de entender, manter e implantar do que um aplicativo que tem dois bancos de dados separados.
- **Integridade relacional** Localizando as tabelas de associação no mesmo banco de dados, como o aplicativo tabelas-é possível estabelecer [restrições de chave estrangeira](http://en.wikipedia.org/wiki/Foreign_key) entre as chaves primárias no Tabelas relacionadas a associação e tabelas de aplicativos relacionados.

Separando os dados de armazenamento e o aplicativo do usuário em bancos de dados separados só faz sentido, se você tiver vários aplicativos que cada usa bancos de dados separados, mas precisa compartilhar um repositório de usuário comum.

### <a name="creating-a-database"></a>Criando um banco de dados

O aplicativo que temos criado desde o segundo tutorial ainda não tem necessária para um banco de dados. É necessário um agora, no entanto, para o repositório do usuário. Vamos criar um e, em seguida, adicione a ele o esquema necessário para o `SqlMembershipProvider` provedor (consulte a etapa 2).

> [!NOTE]
> Em toda esta série de tutoriais usaremos um [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/en-us/sql/Aa336346.aspx) banco de dados para armazenar nossas tabelas de aplicativo e o `SqlMembershipProvider` esquema. Essa decisão foi feita por dois motivos: primeiro, devido a seu custo - livre - a edição Express é a versão mais legibilidade acessível do SQL Server 2005; em segundo lugar, os bancos de dados do SQL Server 2005 Express Edition podem ser colocados diretamente no aplicativo da web do `App_Data` pasta, tornando muito fácil para o banco de dados do pacote e aplicativo web juntos em um arquivo ZIP e reimplantá-lo sem quaisquer instruções de instalação especial ou as opções de configuração. Se você preferir acompanhar usando uma versão de não - Express Edition do SQL Server, fique à vontade. As etapas são praticamente idênticas. O `SqlMembershipProvider` esquema funcionarão com qualquer versão do Microsoft SQL Server 2000 e backup.


No Gerenciador de soluções, clique com botão direito no `App_Data` pasta e optar por adicionar novo Item. (Se você não vir um `App_Data` pasta em seu projeto, clique com botão direito no projeto no Gerenciador de soluções, selecione Adicionar pasta ASP.NET e escolha `App_Data`.) Na caixa de diálogo Adicionar Novo Item, escolha Adicionar um novo banco de dados SQL denominado `SecurityTutorials.mdf`. Neste tutorial adicionaremos o `SqlMembershipProvider` esquema para este banco de dados; em tutoriais subsequentes, vamos criar mais tabelas para capturar os dados de aplicativo.


[![Adicionar um novo banco de dados SQL denominado SecurityTutorials.mdf banco de dados para a pasta App_Data](creating-the-membership-schema-in-sql-server-cs/_static/image2.png)](creating-the-membership-schema-in-sql-server-cs/_static/image1.png)

**Figura 1**: adicionar um novo chamado banco de dados do SQL `SecurityTutorials.mdf` de banco de dados para o `App_Data` pasta ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image3.png))


Adicionar um banco de dados para o `App_Data` pasta inclui automaticamente no modo de exibição do Pesquisador de objetos de banco de dados. (A versão de não - Express Edition do Visual Studio, o Gerenciador de banco de dados é chamado o Gerenciador de servidores.) Vá para o Gerenciador de banco de dados e expanda a apenas adicionados `SecurityTutorials` banco de dados. Se você não vir o Gerenciador de banco de dados na tela, vá para o menu de exibição e escolha o Pesquisador de objetos de banco de dados ou ocorrências Ctrl + Alt + S. Como mostra a Figura 2, o `SecurityTutorials` banco de dados está vazio - contém nenhuma tabela, nenhum modo de exibição e não há procedimentos armazenados.


[![O banco de dados SecurityTutorials está vazia](creating-the-membership-schema-in-sql-server-cs/_static/image5.png)](creating-the-membership-schema-in-sql-server-cs/_static/image4.png)

**Figura 2**: O `SecurityTutorials` banco de dados está vazia ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Etapa 2: Adicionando a`SqlMembershipProvider`esquema no banco de dados

O `SqlMembershipProvider` requer um conjunto específico de tabelas, exibições e procedimentos armazenados a serem instalados no banco de dados de repositório do usuário. Esses objetos de banco de dados necessários podem ser adicionados usando o [ `aspnet_regsql.exe` ferramenta](https://msdn.microsoft.com/en-us/library/ms229862.aspx). Esse arquivo está localizado no `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` pasta.

> [!NOTE]
> O `aspnet_regsql.exe` ferramenta oferece funcionalidade de linha de comando e uma interface gráfica do usuário. A interface gráfica é mais amigável e o que veremos neste tutorial. A interface de linha de comando é útil quando a adição do `SqlMembershipProvider` esquema precisa ser automatizadas, como a compilação de scripts ou automatizado cenários de teste.


O `aspnet_regsql.exe` ferramenta é usada para adicionar ou remover *serviços de aplicativos ASP.NET* para um banco de dados do SQL Server especificado. Os serviços do aplicativo ASP.NET englobam os esquemas para o `SqlMembershipProvider` e `SqlRoleProvider`, junto com os esquemas para os provedores baseados em SQL para outras estruturas de ASP.NET 2.0. É preciso fornecer dois bits de informações para o `aspnet_regsql.exe` ferramenta:

- Nós queremos adicionar ou remover serviços de aplicativos, e
- O banco de dados do qual adicionar ou remover o esquema de serviços de aplicativo

No aviso para o banco de dados a ser usado, o `aspnet_regsql.exe` ferramenta pede para fornecer o nome do servidor de banco de dados reside, as credenciais de segurança para se conectar ao banco de dados e o nome do banco de dados. Se você estiver usando o não - Express Edition do SQL Server, você já deve saber essas informações, como as mesmas informações que você deve fornecer por meio de uma cadeia de caracteres de conexão ao trabalhar com o banco de dados por meio de uma página da web. Determinando o nome de servidor e banco de dados ao usar um banco de dados do SQL Server 2005 Express Edition o `App_Data` pasta, no entanto, é um pouco mais envolvida.

A seção a seguir examina uma maneira simples para especificar o nome do servidor e banco de dados para um banco de dados do SQL Server 2005 Express Edition o `App_Data` pasta. Se você não estiver usando o SQL Server 2005 Express Edition à vontade pular para a instalação a seção Serviços de aplicativo.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Determinando o servidor e o nome do banco de dados para um banco de dados do SQL Server 2005 Express Edition no`App_Data`pasta

Para usar o `aspnet_regsql.exe` ferramenta precisamos saber os nomes de servidor e banco de dados. O nome do servidor é `localhost\InstanceName`. Provavelmente, o *InstanceName* é `SQLExpress`. No entanto, se você instalou manualmente o SQL Server 2005 Express Edition (ou seja, você não o instalou automaticamente durante a instalação do Visual Studio), em seguida, é possível que você selecionou um nome de instância diferente.

O nome do banco de dados é um pouco mais complicado determinar. Bancos de dados no `App_Data` pasta normalmente têm um nome de banco de dados que inclui um [identificador globalmente exclusivo](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) juntamente com o caminho para o arquivo de banco de dados. É necessário determinar esse nome de banco de dados para adicionar o esquema do serviços de aplicativo por meio de `aspnet_regsql.exe`.

A maneira mais fácil de determinar o nome do banco de dados é examiná-lo por meio do SQL Server Management Studio. SQL Server Management Studio fornece uma interface gráfica para gerenciar bancos de dados do SQL Server 2005, mas ele não é fornecido com o Express Edition do SQL Server 2005. A boa notícia é que [você pode baixar](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) o livre Express Edition do SQL Server Management Studio.

> [!NOTE]
> Se você também tem uma versão de não - Express Edition do SQL Server 2005 está instalado em sua área de trabalho, em seguida, a versão completa do Management Studio provavelmente está instalada. Você pode usar a versão completa para determinar o nome do banco de dados, seguindo as mesmas etapas, conforme descrito a seguir para a edição Express.


Iniciar ao fechar o Visual Studio para garantir que todos os bloqueios de imposto pelo Visual Studio no arquivo de banco de dados sejam fechados. Em seguida, inicie o SQL Server Management Studio e conecte-se para o `localhost\InstanceName` banco de dados do SQL Server 2005 Express Edition. Conforme observado anteriormente, a probabilidade é o nome da instância é `SQLExpress`. Para a opção de autenticação, selecione autenticação do Windows.


[![Conecte-se à instância do SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-cs/_static/image8.png)](creating-the-membership-schema-in-sql-server-cs/_static/image7.png)

**Figura 3**: conectar-se à instância do SQL Server 2005 Express Edition ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image9.png))


Depois de se conectar à instância do SQL Server 2005 Express Edition, Management Studio exibe as pastas para os bancos de dados, as configurações de segurança, os objetos de servidor e assim por diante. Se você expandir a guia de bancos de dados você verá que o `SecurityTutorials.mdf` banco de dados é *não* registrado na instância do banco de dados - é preciso anexar o banco de dados pela primeira vez.

Com o botão direito na pasta de bancos de dados e escolha anexar o menu de contexto. Isso exibirá a caixa de diálogo anexar bancos de dados. A partir daqui, clique no botão Adicionar, navegue até o `SecurityTutorials.mdf` de banco de dados e, em seguida, clique em Okey. A Figura 4 mostra a caixa de diálogo anexar bancos de dados após o `SecurityTutorials.mdf` banco de dados foi selecionado. A Figura 5 mostra o Pesquisador de objetos do Management Studio depois que o banco de dados foi anexado com êxito.


[![Anexar o banco de dados SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-cs/_static/image11.png)](creating-the-membership-schema-in-sql-server-cs/_static/image10.png)

**Figura 4**: anexar o `SecurityTutorials.mdf` banco de dados ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image12.png))


[![O banco de dados SecurityTutorials.mdf aparece na pasta de bancos de dados](creating-the-membership-schema-in-sql-server-cs/_static/image14.png)](creating-the-membership-schema-in-sql-server-cs/_static/image13.png)

**Figura 5**: O `SecurityTutorials.mdf` banco de dados aparece na pasta de bancos de dados ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image15.png))


Como mostra a Figura 5, o `SecurityTutorials.mdf` banco de dados tem um nome em vez disso, confusas. Vamos alterá-lo para um mais fácil de lembrar (e mais fácil de digitar) nome. Clique com botão direito no banco de dados, escolha Renomear no menu de contexto e renomeá-la `SecurityTutorialsDatabase`. Isso não altera o nome do arquivo, apenas o nome do banco de dados usa para identificar-se ao SQL Server.


[![Renomeie o banco de dados para SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-cs/_static/image17.png)](creating-the-membership-schema-in-sql-server-cs/_static/image16.png)

**Figura 6**: renomear o banco de dados `SecurityTutorialsDatabase`([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image18.png))


Neste ponto sabemos que os nomes de servidor e banco de dados para o `SecurityTutorials.mdf` arquivo de banco de dados: `localhost\InstanceName` e `SecurityTutorialsDatabase`, respectivamente. Agora estamos prontos para instalar os serviços de aplicativo por meio de `aspnet_regsql.exe` ferramenta.

### <a name="installing-the-application-services"></a>Instalando os serviços de aplicativo

Para iniciar o `aspnet_regsql.exe` ferramenta, vá ao menu Iniciar e escolha Executar. Digite `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` na caixa de texto e clique em Okey. Como alternativa, você pode usar o Windows Explorer para fazer drill down até a pasta apropriada e clique duas vezes o `aspnet_regsql.exe` arquivo. Qualquer abordagem será net os mesmos resultados.

Executando o `aspnet_regsql.exe` ferramenta sem quaisquer argumentos de linha de comando inicia a interface gráfica do usuário ASP.NET Assistente de instalação do SQL Server. O assistente facilita a adicionar ou remover os serviços do aplicativo ASP.NET em um banco de dados especificado. A primeira tela do assistente, mostrado na Figura 7, descreva a finalidade da ferramenta.


[![Usar o ASP.NET SQL Server instalação assistente faz para adicionar o esquema de associação](creating-the-membership-schema-in-sql-server-cs/_static/image20.png)](creating-the-membership-schema-in-sql-server-cs/_static/image19.png)

**Figura 7**: usar o ASP.NET SQL Server Setup assistente torna para adicionar o esquema de associação ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image21.png))


A segunda etapa no Assistente de pergunta deseja adicionar os serviços de aplicativo ou removê-los. Como queremos adicionar as tabelas, exibições e procedimentos armazenados necessários para o `SqlMembershipProvider`, escolher configurar o SQL Server para a opção de serviços de aplicativo. Posteriormente, se você deseja remover este esquema do banco de dados, execute novamente este assistente, mas em vez disso, escolha as informações de serviços do aplicativo de remoção de uma opção de banco de dados existente.


[![Escolha a configurar o SQL Server para a opção de serviços de aplicativo](creating-the-membership-schema-in-sql-server-cs/_static/image23.png)](creating-the-membership-schema-in-sql-server-cs/_static/image22.png)

**Figura 8**: escolher configurar o SQL Server para a opção de serviços de aplicativo ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image24.png))


A terceira etapa solicita as informações de banco de dados: o nome do servidor, informações de autenticação e o nome do banco de dados. Se você tem sido a seguir junto com este tutorial e adicionou o `SecurityTutorials.mdf` banco de dados `App_Data`, anexado a `localhost\InstanceName`e renomeado para `SecurityTutorialsDatabase`, em seguida, use os seguintes valores:

- Servidor:`localhost\InstanceName`
- Autenticação do Windows
- Banco de dados:`SecurityTutorialsDatabase`


[![Insira as informações do banco de dados](creating-the-membership-schema-in-sql-server-cs/_static/image26.png)](creating-the-membership-schema-in-sql-server-cs/_static/image25.png)

**Figura 9**: insira as informações do banco de dados ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image27.png))


Depois de inserir as informações do banco de dados, clique em Avançar. A etapa final resume as etapas que serão tomadas. Clique em Avançar para instalar os serviços de aplicativo e em seguida Concluir para concluir o assistente.

> [!NOTE]
> Se você usou o Management Studio para anexar o banco de dados e renomeie o arquivo de banco de dados, certifique-se de desanexar o banco de dados e feche o Management Studio antes de reabrir o Visual Studio. Para desanexar o `SecurityTutorialsDatabase` de banco de dados, clique no nome do banco de dados e, no menu tarefas, escolha desanexar.


Após a conclusão do assistente, retorne ao Visual Studio e navegue até o Gerenciador de banco de dados. Expanda a pasta de tabelas. Você deve ver uma série de tabelas cujos nomes começam com o prefixo `aspnet_`. Da mesma forma, uma variedade de exibições e procedimentos armazenados pode ser encontrada em pastas de exibições e procedimentos armazenados. Esses objetos de banco de dados compõem o esquema do serviços de aplicativo. Vamos examinar os objetos de banco de dados específicas de função e associação na etapa 3.


[![Uma variedade de tabelas, exibições e procedimentos armazenados foram adicionados ao banco de dados](creating-the-membership-schema-in-sql-server-cs/_static/image29.png)](creating-the-membership-schema-in-sql-server-cs/_static/image28.png)

**Figura 10**: uma variedade de tabelas, exibições e armazenados procedimentos foram adicionados ao banco de dados ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image30.png))


> [!NOTE]
> O `aspnet_regsql.exe` interface de usuário gráfica da ferramenta instala o esquema do serviços de aplicativo inteiro. Mas ao executar `aspnet_regsql.exe` na linha de comando, você pode especificar qual aplicativo específico dos serviços de componentes para instalar (ou remover). Portanto, se você quiser adicionar apenas as tabelas, exibições e armazenados procedimentos necessários para o `SqlMembershipProvider` e `SqlRoleProvider` provedores, executar `aspnet_regsql.exe` na linha de comando. Como alternativa, você pode executar manualmente o subconjunto de T-SQL criar scripts usados por `aspnet_regsql.exe`. Esses scripts estão localizados no `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` pasta com nomes como `InstallCommon.sql`,`InstallMembership.sql`,`InstallRoles.sql`, `InstallProfile.sql`,`InstallSqlState.sql`e assim por diante.


Neste ponto, criamos os objetos de banco de dados necessários para o `SqlMembershipProvider`. No entanto, ainda precisamos instruir a estrutura de associação que ele deve usar o `SqlMembershipProvider` (versus, digamos, o `ActiveDirectoryMembershipProvider`) e que o `SqlMembershipProvider` devem usar o `SecurityTutorials` banco de dados. Vamos examinar como especificar qual provedor será usado e como personalizar as configurações do provedor selecionado na etapa 4. Mas primeiro, vamos dar uma análise mais profunda sobre os objetos de banco de dados que acabaram de ser criadas.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Etapa 3: Examinar tabelas de núcleo do esquema

Ao trabalhar com as estruturas de associação e funções em um aplicativo ASP.NET, os detalhes de implementação são encapsulados pelo provedor. Em tutoriais futuros que interagem com essas estruturas por meio do .NET Framework `Membership` e `Roles` classes. Ao usar essas APIs de alto nível não precisamos nos estão relacionados com os detalhes de baixo nível, como quais consultas são executadas ou quais tabelas são modificadas pelo `SqlMembershipProvider` e `SqlRoleProvider`.

Dito isto, podemos com confiança usar as estruturas de associação e funções sem ter explorado o esquema de banco de dados criado na etapa 2. No entanto, ao criar as tabelas para armazenar dados de aplicativo é necessário criar entidades relacionadas a usuários ou funções. Isso ajuda a familiaridade com o `SqlMembershipProvider` e `SqlRoleProvider` esquemas ao estabelecer estrangeira chave restrições entre as tabelas de dados de aplicativo e as tabelas criadas na etapa 2. Além disso, em determinadas circunstâncias raras talvez seja necessário fazer a interface com o usuário e a função armazena diretamente no nível do banco de dados (em vez de por meio de `Membership` ou `Roles` classes).

### <a name="partitioning-the-user-store-into-applications"></a>Particionando o repositório do usuário em aplicativos

As estruturas de associação e funções são projetadas de modo um único repositório de usuário e a função pode ser compartilhado entre vários aplicativos diferentes. Um aplicativo ASP.NET que usa as estruturas de associação ou funções deve especificar qual partição de aplicativo a ser usado. Em resumo, vários aplicativos da web podem usar os mesmos armazenamentos de usuário e a função. Figura 11 mostra a repositórios de usuário e a função que são particionados em três aplicativos: HRSite, CustomerSite e SalesSite. Esses aplicativos três web tem suas próprias funções e usuários exclusivos, mesmo que todos eles fisicamente armazenam suas informações de conta e a função de usuário nas mesmas tabelas de banco de dados.


[![Contas de usuário podem ser particionadas em vários aplicativos](creating-the-membership-schema-in-sql-server-cs/_static/image32.png)](creating-the-membership-schema-in-sql-server-cs/_static/image31.png)

**Figura 11**: contas podem ser particionados em vários aplicativos de usuário ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-cs/_static/image33.png))


O `aspnet_Applications` tabela é o que define essas partições. Cada aplicativo que usa o banco de dados para armazenar informações de conta de usuário é representado por uma linha nesta tabela. O `aspnet_Applications` tabela tem quatro colunas: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, e `Description`. `ApplicationId`é do tipo [ `uniqueidentifier` ](https://msdn.microsoft.com/en-us/library/ms187942.aspx) e é a chave da tabela primária; `ApplicationName` fornece um nome amigável a humanos exclusivo para cada aplicativo.

Vincular as outras tabelas relacionadas a associação e funções para o `ApplicationId` campo `aspnet_Applications`. Por exemplo, o `aspnet_Users` tabela, que contém um registro para cada conta de usuário, tem um `ApplicationId` campo de chave estrangeira; tais para o `aspnet_Roles` tabela. O `ApplicationId` campo nessas tabelas Especifica a partição de aplicativo de conta de usuário ou a função pertence.

### <a name="storing-user-account-information"></a>Armazenando informações de conta de usuário

Informações de conta de usuário estão hospedadas em duas tabelas: `aspnet_Users` e `aspnet_Membership`. O `aspnet_Users` tabela contém campos que contêm as informações de conta de usuário essenciais. As três colunas mais pertinentes são:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId`é a chave primária (e do tipo `uniqueidentifier`). `UserName`é do tipo `nvarchar(256)` e, juntamente com a senha, faz com que as credenciais do usuário. (A senha do usuário é armazenada no `aspnet_Membership` tabela.) `ApplicationId` vincula a conta de usuário para um aplicativo específico em `aspnet_Applications`. Há uma composição [ `UNIQUE` restrição](https://msdn.microsoft.com/en-us/library/ms191166.aspx) no `UserName` e `ApplicationId` colunas. Isso garante que o de um determinado aplicativo cada nome de usuário é exclusivo, mas permite o mesmo `UserName` a ser usado em diferentes aplicativos.

O `aspnet_Membership` tabela inclui informações de conta de usuário adicionais, como a senha do usuário, endereço de email, o último logon data e hora e assim por diante. Há uma correspondência entre registros da `aspnet_Users` e `aspnet_Membership` tabelas. Essa relação é garantida pelo `UserId` campo `aspnet_Membership`, que serve como chave primária da tabela. Como o `aspnet_Users` tabela, `aspnet_Membership` inclui um `ApplicationId` campo que está associado a essas informações para uma determinada partição de aplicativo.

### <a name="securing-passwords"></a>Proteção de senhas

Informações de senha são armazenadas no `aspnet_Membership` tabela. O `SqlMembershipProvider` permite que as senhas sejam armazenadas no banco de dados usando uma das três seguintes técnicas:

- **Limpar** -a senha é armazenada no banco de dados como texto sem formatação. Eu não recomendam o uso dessa opção. Se o banco de dados estiver comprometido - seja por um hacker que localiza uma porta dos fundos ou um funcionário insatisfeito com acesso de banco de dados - cada único as credenciais de usuário existem para a captura.
- **Hash** -senhas são transformadas em hash usando um algoritmo de hash unidirecional e um valor de sal gerado aleatoriamente. Esse valor de hash (junto com o ' sal ') é armazenado no banco de dados.
- **Criptografado** -uma versão criptografada da senha é armazenada no banco de dados.

A técnica de armazenamento de senha usada depende de `SqlMembershipProvider` as configurações especificadas no `Web.config`. Vamos examinar como personalizar o `SqlMembershipProvider` configurações na etapa 4. O comportamento padrão é armazenar o hash da senha.

As colunas responsáveis por armazenar a senha são `Password`, `PasswordFormat`, e `PasswordSalt`. `PasswordFormat`é um campo do tipo `int` cujo valor indica a técnica usada para armazenar a senha: 2 para criptografado, 1 para Hashed; 0 para limpar. `PasswordSalt`recebe uma cadeia de caracteres gerada aleatoriamente, independentemente da técnica de armazenamento de senha usada; o valor de `PasswordSalt` só é usado ao calcular o hash da senha. Por fim, o `Password` coluna contém os dados reais de senha, seja a senha de texto sem formatação, o hash de senha ou a senha criptografada.

Tabela 1 ilustra essas três colunas aparência para as várias técnicas de armazenamento ao armazenar a senha MySecret! .

| **Técnica de armazenamento&lt;\_o3a\_p /&gt;** | **Senha&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Clear | MySecret! | 0 | tTnkPlesqissc2y2SMEygA = = |
| Hash | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM = | 1 | wFgjUfhdUFOCKQiI61vtiQ = = |
| Criptografado | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | Aa/LSRzhGS/oqAXGLHJNBw = = |

**Tabela 1**: valores de exemplo para os campos relacionados a senha ao armazenar a senha MySecret!

> [!NOTE]
> A criptografia específica ou o algoritmo de hash usado pelo `SqlMembershipProvider` é determinado pelas configurações no `<machineKey>` elemento. Discutimos este elemento de configuração na etapa 3 do <a id="Tutorial3"> </a> [ *configuração de autenticação de formulários e tópicos avançados* ](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md) tutorial.


### <a name="storing-roles-and-role-associations"></a>Armazenamento de funções e associações de função

A estrutura de funções permite que os desenvolvedores definir um conjunto de funções e especificar que os usuários pertencem a quais funções. Essas informações são capturadas no banco de dados por meio de duas tabelas: `aspnet_Roles` e `aspnet_UsersInRoles`. Cada registro de `aspnet_Roles` tabela representa uma função para um aplicativo específico. Assim como o `aspnet_Users` tabela, o `aspnet_Roles` tabela tem três colunas pertinentes à nossa discussão:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId`é a chave primária (e do tipo `uniqueidentifier`). `RoleName` é do tipo `nvarchar(256)`. E `ApplicationId` vincula a conta de usuário para um aplicativo específico em `aspnet_Applications`. Há uma composição `UNIQUE` restrição a `RoleName` e `ApplicationId` colunas, garantindo que, em um determinado aplicativo cada nome de função é exclusivo.

O `aspnet_UsersInRoles` tabela serve como um mapeamento entre usuários e funções. Há apenas duas colunas - `UserId` e `RoleId` - e juntos eles formam uma chave primária composta.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Etapa 4: Especifica o provedor e personalizar suas configurações

Todas as estruturas que dão suporte o modelo de provedor, como as estruturas de associação e funções - falta próprios detalhes de implementação e, em vez disso, delegar essa responsabilidade a uma classe de provedor. No caso da estrutura de associação, o `Membership` classe define a API de gerenciamento de contas de usuário, mas ele não interagem diretamente com qualquer repositório de usuário. Em vez disso, o `Membership` mão de métodos da classe a solicitação para o provedor configurado - utilizaremos o `SqlMembershipProvider`. Quando podemos chamar um dos métodos no `Membership` classe, como a estrutura de associação sabe para delegar a chamada para o `SqlMembershipProvider`?

O `Membership` classe tiver um [ `Providers` propriedade](https://msdn.microsoft.com/en-us/library/system.web.security.membership.providers.aspx) que contém uma referência a todas as classes de provedor registrado disponíveis para uso pela estrutura de associação. Cada provedor registrado tem um nome de associado e um tipo. O nome oferece uma maneira de humanos amigável para fazer referência a um determinado provedor no `Providers` coleção, enquanto o tipo identifica a classe de provedor. Além disso, cada provedor registrado pode incluir parâmetros de configuração. Definições de configuração para a estrutura de associação incluem `passwordFormat` e `requiresUniqueEmail`, entre outros. Consulte a tabela 2 para obter uma lista de definições de configuração usado pelo `SqlMembershipProvider`.

O `Providers` conteúdo da propriedade é especificado por meio de configurações do aplicativo da web. Por padrão, todos os aplicativos web têm um provedor chamado `AspNetSqlMembershipProvider` do tipo `SqlMembershipProvider`. Esse provedor de associação padrão é registrado no `machine.config` (localizado em `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG)`:

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample1.xml)]

Como a marcação acima mostra, o [ `<membership>` elemento](https://msdn.microsoft.com/en-us/library/1b9hw62f.aspx) define as configurações para a estrutura de associação durante a [ `<providers>` elemento filho](https://msdn.microsoft.com/en-us/library/6d4936ht.aspx) Especifica registrado provedores. Provedores podem ser adicionados ou removidos usando o [ `<add>` ](https://msdn.microsoft.com/en-us/library/whae3t94.aspx) ou [ `<remove>` ](https://msdn.microsoft.com/en-us/library/aykw9a6d.aspx) elementos; use o [ `<clear>` ](https://msdn.microsoft.com/en-us/library/t062y6yc.aspx) elemento para remover todos os atualmente provedores registrados. Como a marcação acima mostra, `machine.config` adiciona um provedor chamado `AspNetSqlMembershipProvider` do tipo `SqlMembershipProvider`.

Além de `name` e `type` atributos, o `<add>` elemento contém atributos que definem os valores para várias configurações. A tabela 2 lista disponíveis `SqlMembershipProvider`-definições de configuração específicas, junto com uma descrição de cada.

> [!NOTE]
> Valores padrão indicados na tabela 2 consultem os valores padrão definidos no `SqlMembershipProvider` classe. Observe que nem todas as definições de configuração em `AspNetSqlMembershipProvider` correspondem aos valores padrão da `SqlMembershipProvider` classe. Por exemplo, se não for especificado em um provedor de associação, o `requiresUniqueEmail` definir padrões para verdadeiro. No entanto, o `AspNetSqlMembershipProvider` substitui o valor padrão especificando explicitamente um valor de `false`.


| **Configuração&lt;\_o3a\_p /&gt;** | **Descrição&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Lembre-se de que a estrutura de associação permite um repositório de usuário único a ser particionados em vários aplicativos. Essa configuração indica o nome da partição de aplicativo usado pelo provedor de associação. Se esse valor não é explicitamente especificado, ele é definido em tempo de execução, como o valor do caminho de raiz virtual do aplicativo. |
| `commandTimeout` | Especifica o valor de tempo limite do comando SQL (em segundos). O valor padrão é 30. |
| `connectionStringName` | O nome da cadeia de caracteres de conexão no `<connectionStrings>` elemento a ser usado para se conectar ao banco de dados de repositório do usuário. Esse valor é necessário. |
| `description` | Fornece uma descrição amigável humanos do provedor registrado. |
| `enablePasswordRetrieval` | Especifica se os usuários podem recuperar sua senha. O valor padrão é `false`. |
| `enablePasswordReset` | Indica se os usuários têm permissão para redefinir sua senha. Assume o padrão de `true`. |
| `maxInvalidPasswordAttempts` | O número máximo de tentativas de logon sem êxito que podem ocorrer para um determinado usuário durante especificado `passwordAttemptWindow` antes que o usuário seja bloqueado. O valor padrão é 5. |
| `minRequiredNonalphanumericCharacters` | O número mínimo de caracteres não alfanuméricos deve aparecer em uma senha de usuário. Esse valor deve estar entre 0 e 128; o padrão é 1. |
| `minRequiredPasswordLength` | O número mínimo de caracteres necessários na senha. Esse valor deve estar entre 0 e 128; o padrão é 7. |
| `name` | O nome do provedor registrado. Esse valor é necessário. |
| `passwordAttemptWindow` | O número de minutos durante os quais falha são rastreadas tentativas de logon. Se um usuário fornece credenciais de logon inválidas `maxInvalidPasswordAttempts` especificado de vezes dentro dessa janela, eles estão bloqueados. O valor padrão é 10. |
| `PasswordFormat` | O formato de armazenamento de senha: `Clear`, `Hashed`, ou `Encrypted`. O padrão é `Hashed`. |
| `passwordStrengthRegularExpression` | Se fornecido, essa expressão regular é usado para avaliar a força de senha de usuário selecionada ao criar uma nova conta ou ao alterar sua senha. O valor padrão é uma cadeia de caracteres vazia. |
| `requiresQuestionAndAnswer` | Especifica se um usuário deve responder sua pergunta de segurança ao recuperar ou redefinir sua senha. O valor padrão é `true`. |
| `requiresUniqueEmail` | Indica se todas as contas de usuário em uma partição de aplicativo fornecido devem ter um endereço de email exclusivo. O valor padrão é `true`. |
| `type` | Especifica o tipo do provedor. Esse valor é necessário. |

**Tabela 2**: associação e `SqlMembershipProvider` definições de configuração

Além `AspNetSqlMembershipProvider`, outros provedores de associação podem ser registrados em uma base por aplicativo adicionando marcação semelhante para o `Web.config` arquivo.

> [!NOTE]
> A estrutura de funções funciona quase da mesma maneira: há um provedor de função registrado padrão no `machine.config` e os provedores registrados podem ser personalizados em uma base por aplicativo em `Web.config`. Vamos examinar a estrutura de funções e sua marcação de configuração em detalhes em um tutorial futuras.


### <a name="customizing-thesqlmembershipprovidersettings"></a>Personalizando o`SqlMembershipProvider`configurações

O padrão `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) tem seu `connectionStringName` atributo definido como `LocalSqlServer`. Como o `AspNetSqlMembershipProvider` provedor, o nome de cadeia de caracteres de conexão `LocalSqlServer` é definido em `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample2.xml)]

Como você pode ver, essa cadeia de caracteres de conexão define um SQL 2005 Express Edition banco de dados localizado em | DataDirectory|aspnetdb.mdf. A cadeia de caracteres | DataDirectory | é convertida em tempo de execução para apontar para o `~/App_Data/` directory, portanto, o caminho do banco de dados | DataDirectory|aspnetdb.mdf"se traduz em `~/App_Data` / `aspnet.mdf`.

Se não especificamos qualquer informação de provedor de associação em nosso aplicativo `Web.config` arquivo, o aplicativo usa o provedor de associação padrão registrado, `AspNetSqlMembershipProvider`. Se o `~/App_Data/aspnet.mdf` banco de dados não existe, o tempo de execução do ASP.NET automaticamente criá-lo e adicionar o esquema do serviços de aplicativo. No entanto, nós não desejamos usar o `aspnet.mdf` banco de dados; em vez disso, queremos usar o `SecurityTutorials.mdf` banco de dados criados na etapa 2. Essa modificação pode ser feita de duas maneiras:

- **Especifique um valor para o *`LocalSqlServer`* nome de cadeia de caracteres de conexão em *`Web.config`*.** Substituindo o `LocalSqlServer` o valor do nome da cadeia de conexão no `Web.config`, podemos usar o provedor de associação padrão registrado (`AspNetSqlMembershipProvider`) e ainda funcionar corretamente com o `SecurityTutorials.mdf` banco de dados. Essa abordagem é aceitável se tiver conteúdo com as configurações especificadas pelo `AspNetSqlMembershipProvider`. Para obter mais informações sobre essa técnica, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da postagem de blog, [Configurando serviços de aplicativos do ASP.NET 2.0 para uso do SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- **Adicionar um novo provedor registrado do tipo *`SqlMembershipProvider`* e configurar seu *`connectionStringName`* configuração para apontar para o *`SecurityTutorials.mdf`* banco de dados.** Essa abordagem é útil em situações em que você deseja personalizar outras propriedades de configuração, além de cadeia de caracteres de conexão do banco de dados. Em Meus próprio projetos sempre uso essa abordagem devido a sua flexibilidade e facilitar a leitura.

Antes de adicionar um novo provedor registrado que faz referência a `SecurityTutorials.mdf` banco de dados, primeiro precisamos adicionar um valor de cadeia de caracteres de conexão apropriado no `<connectionStrings>` seção `Web.config`. A marcação a seguir adiciona uma nova cadeia de caracteres de conexão chamada `SecurityTutorialsConnectionString` que referencia o SQL Server 2005 Express Edition `SecurityTutorials.mdf` banco de dados de `App_Data` pasta.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample3.xml)]

> [!NOTE]
> Se você estiver usando um arquivo de banco de dados alternativo, atualize a cadeia de conexão, conforme necessário. Para obter mais informações sobre formam a cadeia de caracteres de conexão correta, consulte [ConnectionStrings.com](http://www.connectionstrings.com/).

Em seguida, adicione a seguinte marcação de configuração de associação para o `Web.config` arquivo. Essa marcação registra um novo provedor denominado `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-cs/samples/sample4.xml)]

Além de registrar o `SecurityTutorialsSqlMembershipProvider` provedor, a marcação acima define o `SecurityTutorialsSqlMembershipProvider` como o provedor padrão (por meio de `defaultProvider` atributo no `<membership>` elemento). Lembre-se de que a estrutura de associação pode ter vários provedores registrados. Como `AspNetSqlMembershipProvider` é registrado como o primeiro provedor em `machine.config`, ele serve como o provedor padrão, a menos que indique o contrário.

Atualmente, o nosso aplicativo tem dois provedores registrados: `AspNetSqlMembershipProvider` e `SecurityTutorialsSqlMembershipProvider`. No entanto, antes de registrar o `SecurityTutorialsSqlMembershipProvider` provedor que pode ter sido limpo todas anteriormente provedores registrados adicionando um [ `<clear />` elemento](https://msdn.microsoft.com/en-us/library/t062y6yc.aspx) imediatamente antes do nosso `<add>` elemento. Isso deve limpar o `AspNetSqlMembershipProvider` da lista de provedores registrados, que significa que o `SecurityTutorialsSqlMembershipProvider` deve ser o único provedor de associação registrado. Se usamos essa abordagem, não seria necessário marcar o `SecurityTutorialsSqlMembershipProvider` como o provedor padrão, como seria o único provedor de associação registrado. Para obter mais informações sobre como usar `<clear />`, consulte [usando `<clear />` provedores quando adicionar](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Observe que o `SecurityTutorialsSqlMembershipProvider`do `connectionStringName` definindo referências a apenas adicionados `SecurityTutorialsConnectionString` nome de cadeia de caracteres de conexão e que seu `applicationName` configuração foi definida como um valor de SecurityTutorials. Além disso, o `requiresUniqueEmail` configuração foi definida como `true`. Todas as outras opções de configuração são idênticas aos valores no `AspNetSqlMembershipProvider`. Fique à vontade para fazer as modificações de configuração aqui, se desejar. Por exemplo, você pode aumentar a força da senha exigindo dois caracteres não alfanuméricos em vez de um ou aumentando o comprimento da senha de oito caracteres, em vez de sete.

> [!NOTE]
> Lembre-se de que a estrutura de associação permite um repositório de usuário único a ser particionados em vários aplicativos. O provedor de associação `applicationName` configuração indica que o provedor usa ao trabalhar com o repositório do usuário de aplicativo. É importante que você definir explicitamente um valor para o `applicationName` porque a definição de configuração se o `applicationName` não é definido explicitamente, ela é atribuída a caminho de raiz virtual do aplicativo da web em tempo de execução. Isso funciona bem como caminho de raiz virtual do aplicativo não é alterado, mas se você mover o aplicativo para um caminho diferente, o `applicationName` configuração mudará também. Quando isso acontece, o provedor de associação começará a funcionar com uma partição de aplicativo diferente da que foi usada anteriormente. Contas de usuário criadas antes da movimentação residem em uma partição de aplicativo diferente e os usuários não poderão fazer logon no site. Para obter uma discussão mais detalhada sobre esse assunto, consulte [sempre definir o `applicationName` propriedade quando configurar 2.0 associação do ASP.NET e outros provedores](https://weblogs.asp.net/scottgu/443634).


## <a name="summary"></a>Resumo

Neste ponto, temos um banco de dados com os serviços de aplicativo configurado (`SecurityTutorials.mdf`) e tiver configurado o nosso aplicativo web para que a estrutura de associação usa o `SecurityTutorialsSqlMembershipProvider` provedor acabou de registrar. Este provedor registrado é do tipo `SqlMembershipProvider` e tem seu `connectionStringName` definido como a cadeia de caracteres de conexão apropriado (`SecurityTutorialsConnectionString`) e sua `applicationName` valor explicitamente definido.

Agora você está pronto para usar a estrutura de associação de nosso aplicativo. O seguinte tutorial, examinaremos como criar novas contas de usuário. A seguir que iremos explorar autenticar usuários, execução de autorização baseada em usuário e armazenar informações adicionais relacionadas ao usuário no banco de dados.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Sempre defina a `applicationName` propriedade ao configurar o ASP.NET 2.0 associação e outros provedores](https://weblogs.asp.net/scottgu/443634)
- [Configurando o ASP.NET 2.0 dos serviços de aplicativos para usar o SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Baixar o SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Examinando o ASP.NET 2.0 s associação, funções e perfil](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [O `<add>` elemento para provedores de associação](https://msdn.microsoft.com/en-us/library/whae3t94.aspx)
- [O `<membership>` elemento](https://msdn.microsoft.com/en-us/library/1b9hw62f.aspx)
- [O `<providers>` elemento de associação](https://msdn.microsoft.com/en-us/library/6d4936ht.aspx)
- [Usando `<clear />` ao adicionar provedores](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Trabalhando diretamente com o`SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre tópicos contidos neste tutorial

- [Noções básicas sobre as associações do ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configurando o SQL para trabalhar com esquemas de associação](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Alterar as configurações de associação do esquema de associação padrão](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é  *[Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou em seu blog [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Alicja Maziarz. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

>[!div class="step-by-step"]
[Avançar](creating-user-accounts-cs.md)
