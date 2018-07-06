---
uid: web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
title: Criando o esquema de associação no SQL Server (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial começa examinando as técnicas para adicionar o esquema necessário para o banco de dados para usar o SqlMembershipProvider. Depois disso, podemos wi...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/18/2008
ms.topic: article
ms.assetid: 112a674d-716f-41a6-99b8-4074d65a54c0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 6fe5ab7e2748a7502f47cb29ecc0b93b6cb07d6b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396982"
---
<a name="creating-the-membership-schema-in-sql-server-vb"></a>Criando o esquema de associação no SQL Server (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_04_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial04_MembershipSetup_vb.pdf)

> Este tutorial começa examinando as técnicas para adicionar o esquema necessário para o banco de dados para usar o SqlMembershipProvider. Depois disso, examinaremos as principais tabelas no esquema e discutir o seu objetivo e a importância. Este tutorial termina com uma olhada em como saber qual a estrutura de associação deve usar o provedor de um aplicativo ASP.NET.


## <a name="introduction"></a>Introdução

Os tutoriais de duas anteriores examinados usando a autenticação de formulários para identificar os visitantes do site. A estrutura de autenticação de formulários torna mais fácil para que os desenvolvedores façam um usuário em um site e lembre-se em visitas de página com o uso de tíquetes de autenticação. O `FormsAuthentication` classe inclui métodos para gerar o tíquete e adicioná-lo para os cookies do visitante. O `FormsAuthenticationModule` examina todas as solicitações de entrada e, para aqueles com um tíquete de autenticação válido, cria e associa um `GenericPrincipal` e um `FormsIdentity` objeto com a solicitação atual. Autenticação de formulários é simplesmente um mecanismo para conceder um tíquete de autenticação para um visitante ao fazer logon no e, em solicitações subsequentes, analisando esse tíquete para determinar a identidade do usuário. Para um aplicativo web dar suporte a contas de usuário, ainda precisamos implementar um armazenamento de usuário e adicionar funcionalidade ao validar as credenciais, registrar novos usuários e uma infinidade de outras tarefas relacionadas a contas de usuário.

Antes do ASP.NET 2.0, os desenvolvedores eram sobre o gancho para implementar todas essas tarefas relacionadas a contas de usuário. Felizmente, a equipe do ASP.NET reconhecido essa deficiência e introduziu a estrutura de associação com o ASP.NET 2.0. A estrutura de associação é um conjunto de classes no .NET Framework que fornecem uma interface programática para a realização de tarefas relacionadas a contas de usuário principal. Essa estrutura é construída sobre a [modelo de provedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), que permite aos desenvolvedores conecte uma implementação personalizada de uma API padronizada.

Conforme discutido na <a id="Tutorial1"> </a> [ *Noções básicas sobre segurança e suporte do ASP.NET* ](../introduction/security-basics-and-asp-net-support-vb.md) tutorial, o .NET Framework vem com dois provedores internos de associação: [ `ActiveDirectoryMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.activedirectorymembershipprovider.aspx) e [ `SqlMembershipProvider` ](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx). Como o nome sugere, o `SqlMembershipProvider` usa um banco de dados do Microsoft SQL Server como o repositório do usuário. Para usar esse provedor em um aplicativo, é necessário instruir o provedor qual banco de dados a ser usado como o repositório. Como você pode imaginar, o `SqlMembershipProvider` espera que o banco de dados do repositório de usuário ter determinadas tabelas de banco de dados, exibições e procedimentos armazenados. Precisamos adicionar este esquema esperado para o banco de dados selecionado.

Este tutorial começa examinando as técnicas para adicionar o esquema necessário para o banco de dados para usar o `SqlMembershipProvider`. Depois disso, examinaremos as principais tabelas no esquema e discutir o seu objetivo e a importância. Este tutorial termina com uma olhada em como saber qual a estrutura de associação deve usar o provedor de um aplicativo ASP.NET.

Vamos começar!

## <a name="step-1-deciding-where-to-place-the-user-store"></a>Etapa 1: Decidir onde colocar o usuário Store

Dados de um aplicativo ASP.NET normalmente são armazenados em um número de tabelas em um banco de dados. Ao implementar o `SqlMembershipProvider` devemos decidir se é necessário colocar o esquema de associação no mesmo banco de dados como os dados de aplicativo ou em um banco de dados alternativo de esquema de banco de dados.

Eu recomendo a localização do esquema de associação no mesmo banco de dados como os dados do aplicativo pelos seguintes motivos:

- **Facilidade de manutenção** um aplicativo cujos dados são encapsulados em um banco de dados é mais fácil de entender, manter e implantar de um aplicativo que tem dois bancos de dados separados.
- **Integridade relacional** Localizando as tabelas relacionadas à associação no mesmo banco de dados como tabelas de aplicativo-lo, é possível estabelecer [restrições de chave estrangeira](http://en.wikipedia.org/wiki/Foreign_key) entre as chaves primárias no Tabelas relacionadas a associação e tabelas de aplicativos relacionados.

Desacoplar os dados de armazenamento e aplicativo do usuário em bancos de dados separados só faz sentido, se você tiver vários aplicativos que cada um usa bancos de dados separados, mas precisa compartilhar um repositório de usuário comum.

### <a name="creating-a-database"></a>Criando um banco de dados

O aplicativo que criarmos uma vez que o segundo tutorial ainda não necessária um banco de dados. É necessário um agora, no entanto, para o repositório do usuário. Vamos criar uma e, em seguida, adicionar a ele o esquema exigido pelo `SqlMembershipProvider` provedor (consulte a etapa 2).

> [!NOTE]
> Em toda esta série de tutoriais vamos usar um [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) banco de dados para armazenar nossas tabelas de aplicativo e o `SqlMembershipProvider` esquema. Essa decisão foi tomada por dois motivos: primeiro, devido a seu custo - gratuito - Express Edition é a versão mais legibilidade acessível do SQL Server 2005; em segundo lugar, os bancos de dados do SQL Server 2005 Express Edition podem ser colocados diretamente do aplicativo da web `App_Data` pasta, tornando muito fácil para o banco de dados do pacote e aplicativo web juntos em um arquivo ZIP e reimplantá-lo sem quaisquer instruções de instalação especial ou as opções de configuração. Se você preferir acompanhar usando uma versão de não - Express Edition do SQL Server, fique à vontade. As etapas são praticamente idênticas. O `SqlMembershipProvider` esquema trabalhará com qualquer versão do Microsoft SQL Server 2000 e backup.

Gerenciador de soluções, clique com botão direito no `App_Data` pasta e optar por adicionar novo Item. (Se você não vir um `App_Data` pasta no seu projeto, clique com botão direito no projeto no Gerenciador de soluções, selecione Adicionar pasta ASP.NET e escolher `App_Data`.) Na caixa de diálogo Add New Item, escolha Adicionar um novo banco de dados SQL denominado `SecurityTutorials.mdf`. Neste tutorial, adicionaremos o `SqlMembershipProvider` esquema para este banco de dados; nos tutoriais subsequentes, vamos criar mais tabelas para capturar dados de nosso aplicativo.


[![Adicionar um novo banco de dados SQL denominado SecurityTutorials.mdf banco de dados para a pasta App_Data](creating-the-membership-schema-in-sql-server-vb/_static/image2.png)](creating-the-membership-schema-in-sql-server-vb/_static/image1.png)

**Figura 1**: adicionar um banco de dados SQL novo denominado `SecurityTutorials.mdf` do banco de dados para o `App_Data` pasta ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-vb/_static/image3.png))


Adicionar um banco de dados para o `App_Data` pasta inclui automaticamente na exibição do Explorer do banco de dados. (Na versão não - Express Edition do Visual Studio, o Gerenciador de banco de dados é chamado o Gerenciador de servidores). Vá para o Gerenciador de banco de dados e expanda o just-adicionado `SecurityTutorials` banco de dados. Se você não vir o Gerenciador de banco de dados na tela, vá até o menu Exibir e escolha Gerenciador de banco de dados ou pressione Ctrl + Alt + S. Como mostra a Figura 2, o `SecurityTutorials` banco de dados está vazio – ele contém nenhuma tabela, não há modos de exibição e nenhum procedimento armazenado.


[![O banco de dados SecurityTutorials está vazio no momento](creating-the-membership-schema-in-sql-server-vb/_static/image5.png)](creating-the-membership-schema-in-sql-server-vb/_static/image4.png)

**Figura 2**: O `SecurityTutorials` banco de dados está vazio no momento ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-vb/_static/image6.png))


## <a name="step-2-adding-thesqlmembershipproviderschema-to-the-database"></a>Etapa 2: Adicionando o`SqlMembershipProvider`esquema no banco de dados

O `SqlMembershipProvider` requer um conjunto específico de tabelas, exibições e procedimentos armazenados a serem instalados no banco de dados de repositório de usuário. Esses objetos de banco de dados necessárias podem ser adicionados usando o [ `aspnet_regsql.exe` ferramenta](https://msdn.microsoft.com/library/ms229862.aspx). Esse arquivo está localizado no `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\` pasta.

> [!NOTE]
> O `aspnet_regsql.exe` ferramenta oferece funcionalidade de linha de comando e uma interface gráfica do usuário. A interface gráfica é mais amigável e o que examinaremos neste tutorial. A interface de linha de comando é útil quando a adição do `SqlMembershipProvider` esquema precisa ser automatizadas, como em compilação scripts ou automatizada de cenários de teste.

O `aspnet_regsql.exe` ferramenta é usada para adicionar ou remover *serviços de aplicativos ASP.NET* para um banco de dados do SQL Server especificado. Os serviços de aplicativo do ASP.NET abrangem os esquemas para o `SqlMembershipProvider` e `SqlRoleProvider`, juntamente com os esquemas para os provedores baseados em SQL para outras estruturas do ASP.NET 2.0. Precisamos fornecer dois bits de informações para o `aspnet_regsql.exe` ferramenta:

- Se queremos adicionar ou remover serviços de aplicativos, e
- O banco de dados do qual adicionar ou remover o esquema de serviços de aplicativo

Na solicitação para o banco de dados a ser usado, o `aspnet_regsql.exe` ferramenta nos pede para fornecer o nome do servidor de banco de dados reside, as credenciais de segurança para se conectar ao banco de dados e o nome do banco de dados. Se você estiver usando o não - Express Edition do SQL Server, você já deve saber essas informações, pois é as mesmas informações que você deve fornecer por meio de uma cadeia de caracteres de conexão ao trabalhar com o banco de dados por meio de uma página da web. Determinar o nome do servidor e banco de dados ao usar um banco de dados do SQL Server 2005 Express Edition a `App_Data` pasta, no entanto, é um pouco mais complicada.

A seção a seguir examina uma maneira simples para especificar o nome do servidor e banco de dados para um banco de dados do SQL Server 2005 Express Edition a `App_Data` pasta. Se você não estiver usando o SQL Server 2005 Express Edition vontade pular para a instalação a seção de serviços de aplicativos.

### <a name="determining-the-server-and-database-name-for-a-sql-server-2005-express-edition-database-in-theappdatafolder"></a>Determinando o servidor e o nome do banco de dados para um banco de dados do SQL Server 2005 Express Edition no`App_Data`pasta

Para usar o `aspnet_regsql.exe` ferramenta precisamos saber os nomes de servidor e banco de dados. O nome do servidor é `localhost\InstanceName`. Provavelmente, o *nome_da_instância* é `SQLExpress`. No entanto, se você instalou manualmente o SQL Server 2005 Express Edition (ou seja, você não o instalou automaticamente durante a instalação do Visual Studio), em seguida, é possível que você selecionou um nome de instância diferente.

O nome do banco de dados é um pouco mais complicado determinar. Bancos de dados na `App_Data` pasta normalmente têm um nome de banco de dados que inclui um [identificador global exclusivo](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) juntamente com o caminho para o arquivo de banco de dados. Precisamos determinar esse nome de banco de dados para adicionar o esquema de serviços de aplicativo por meio de `aspnet_regsql.exe`.

A maneira mais fácil para determinar o nome do banco de dados é examiná-lo por meio do SQL Server Management Studio. SQL Server Management Studio fornece uma interface gráfica para gerenciar bancos de dados do SQL Server 2005, mas ele não é fornecido com o Express Edition do SQL Server 2005. A boa notícia é que [você pode baixar](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) o livre Express Edition do SQL Server Management Studio.

> [!NOTE]
> Se você também tiver uma versão de não - Express Edition do SQL Server 2005 instalado na área de trabalho, a versão completa do Management Studio provavelmente será instalada. Você pode usar a versão completa para determinar o nome do banco de dados, seguindo as mesmas etapas conforme descrito a seguir para a edição Express.

Comece fechando o Visual Studio para garantir que todos os bloqueios de imposto pelo Visual Studio no arquivo de banco de dados sejam fechados. Em seguida, inicie o SQL Server Management Studio e conecte-se para o `localhost\InstanceName` banco de dados do SQL Server 2005 Express Edition. Conforme observado anteriormente, provavelmente é o nome da instância `SQLExpress`. Para a opção de autenticação, selecione autenticação do Windows.


[![Conectar-se à instância do SQL Server 2005 Express Edition](creating-the-membership-schema-in-sql-server-vb/_static/image8.png)](creating-the-membership-schema-in-sql-server-vb/_static/image7.png)

**Figura 3**: Conecte-se à instância do SQL Server 2005 Express Edition ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-vb/_static/image9.png))


Depois de se conectar à instância do SQL Server 2005 Express Edition, o Management Studio exibe pastas para os bancos de dados, as configurações de segurança, os objetos de servidor e assim por diante. Se você expandir a guia de bancos de dados você verá que o `SecurityTutorials.mdf` banco de dados está *não* registrado na instância do banco de dados - precisa anexar o banco de dados pela primeira vez.

Clique com botão direito na pasta de bancos de dados e escolha Anexar no menu de contexto. Isso exibirá a caixa de diálogo anexar bancos de dados. A partir daqui, clique no botão Adicionar, navegue até o `SecurityTutorials.mdf` de banco de dados e, em seguida, clique em Okey. A Figura 4 mostra a caixa de diálogo anexar bancos de dados após o `SecurityTutorials.mdf` banco de dados foi selecionado. Figura 5 mostra o Pesquisador de objetos do Management Studio depois que o banco de dados foi anexado com êxito.


[![Anexe o banco de dados SecurityTutorials.mdf](creating-the-membership-schema-in-sql-server-vb/_static/image11.png)](creating-the-membership-schema-in-sql-server-vb/_static/image10.png)

**Figura 4**: anexar a `SecurityTutorials.mdf` banco de dados ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-vb/_static/image12.png))


[![O banco de dados SecurityTutorials.mdf aparece na pasta de bancos de dados](creating-the-membership-schema-in-sql-server-vb/_static/image14.png)](creating-the-membership-schema-in-sql-server-vb/_static/image13.png)

**Figura 5**: O `SecurityTutorials.mdf` banco de dados aparece na pasta de bancos de dados ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-vb/_static/image15.png))


Como mostra a Figura 5, o `SecurityTutorials.mdf` banco de dados tem um nome em vez disso, confusas. Vamos alterá-lo para um mais fácil de lembrar (e mais fácil de digitar) nome. Clique com botão direito no banco de dados, escolha Renomear no menu de contexto e renomeie- `SecurityTutorialsDatabase`. Isso não altera o nome do arquivo, apenas o nome do banco de dados usa para se identificar para o SQL Server.


[![Renomear o banco de dados SecurityTutorialsDatabase](creating-the-membership-schema-in-sql-server-vb/_static/image17.png)](creating-the-membership-schema-in-sql-server-vb/_static/image16.png)

**Figura 6**: renomeie o banco de dados `SecurityTutorialsDatabase`([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-vb/_static/image18.png))


Neste ponto sabemos que os nomes de servidor e banco de dados para o `SecurityTutorials.mdf` arquivo de banco de dados: `localhost\InstanceName` e `SecurityTutorialsDatabase`, respectivamente. Agora estamos prontos para instalar os serviços de aplicativo por meio de `aspnet_regsql.exe` ferramenta.

### <a name="installing-the-application-services"></a>Instalando os serviços de aplicativo

Para iniciar o `aspnet_regsql.exe` ferramenta, vá para o menu Iniciar e escolha Executar. Insira `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\aspnet_regsql.exe` na caixa de texto e clique em Okey. Como alternativa, você pode usar o Windows Explorer para fazer drill down até a pasta apropriada e clique duas vezes o `aspnet_regsql.exe` arquivo. Qualquer uma das abordagens será dar os mesmos resultados.

Executando o `aspnet_regsql.exe` ferramenta sem nenhum argumento de linha de comando inicia a interface gráfica do usuário ASP.NET Assistente de instalação do SQL Server. O assistente facilita a adicionar ou remover os serviços de aplicativo do ASP.NET em um banco de dados especificado. A primeira tela do assistente, mostrado na Figura 7 descreve a finalidade da ferramenta.


[![Use o SQL do ASP.NET Server instalação assistente torna para adicionar o esquema de associação](creating-the-membership-schema-in-sql-server-vb/_static/image20.png)](creating-the-membership-schema-in-sql-server-vb/_static/image19.png)

**Figura 7**: Use o ASP.NET SQL Server Setup assistente faz para adicionar o esquema de associação ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-vb/_static/image21.png))


A segunda etapa do assistente nos pede que desejamos adicionar os serviços de aplicativo ou removê-los. Como queremos adicionar as tabelas, exibições e procedimentos armazenados necessários para o `SqlMembershipProvider`, escolher configurar o SQL Server para a opção de serviços de aplicativo. Posteriormente, se você deseja remover este esquema do banco de dados, execute novamente este assistente, mas em vez disso, escolha as informações de serviços do aplicativo remover de uma opção de banco de dados existente.


[![Escolha a configurar o SQL Server para a opção de serviços de aplicativo](creating-the-membership-schema-in-sql-server-vb/_static/image23.png)](creating-the-membership-schema-in-sql-server-vb/_static/image22.png)

**Figura 8**: escolha configurar o SQL Server para a opção de serviços de aplicativo ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-vb/_static/image24.png))


A terceira etapa solicitará as informações de banco de dados: o nome do servidor, informações de autenticação e o nome do banco de dados. Se você tiver sido seguindo junto com este tutorial e tiver adicionado a `SecurityTutorials.mdf` banco de dados `App_Data`, anexá-lo a `localhost\InstanceName`e renomeado para `SecurityTutorialsDatabase`, em seguida, use os seguintes valores:

- Servidor: `localhost\InstanceName`
- Autenticação do Windows
- Banco de dados: `SecurityTutorialsDatabase`


[![Insira as informações de banco de dados](creating-the-membership-schema-in-sql-server-vb/_static/image26.png)](creating-the-membership-schema-in-sql-server-vb/_static/image25.png)

**Figura 9**: insira as informações de banco de dados ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-vb/_static/image27.png))


Depois de inserir as informações de banco de dados, clique em Avançar. A etapa final resume as etapas que serão tomadas. Clique em Avançar para instalar os serviços de aplicativo e em seguida Concluir para finalizar o assistente.

> [!NOTE]
> Se você usou o Management Studio para anexar o banco de dados e renomeie o arquivo de banco de dados, certifique-se desanexar o banco de dados e feche o Management Studio antes de reabrir o Visual Studio. Para desanexar o `SecurityTutorialsDatabase` de banco de dados, clique com botão direito no nome do banco de dados e, no menu de tarefas, escolha desanexar.

Após a conclusão do assistente, retorne ao Visual Studio e navegue até o Gerenciador de banco de dados. Expanda a pasta de tabelas. Você deve ver uma série de tabelas cujos nomes começam com o prefixo `aspnet_`. Da mesma forma, uma variedade de exibições e procedimentos armazenados pode ser encontrada nas pastas exibições e procedimentos armazenados. Esses objetos de banco de dados compõem o esquema de serviços de aplicativo. Vamos examinar os objetos de banco de dados específicas de função e associação na etapa 3.


[![Uma variedade de tabelas, exibições e procedimentos armazenados foram adicionados ao banco de dados](creating-the-membership-schema-in-sql-server-vb/_static/image29.png)](creating-the-membership-schema-in-sql-server-vb/_static/image28.png)

**Figura 10**: uma variedade de tabelas, exibições e armazenados procedimentos foram adicionados ao banco de dados ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-vb/_static/image30.png))


> [!NOTE]
> O `aspnet_regsql.exe` interface de usuário gráfica da ferramenta instala o esquema de serviços do aplicativo inteiro. Mas ao executar `aspnet_regsql.exe` na linha de comando, você pode especificar quais determinado aplicativo Serviços de componentes para instalar (ou remover). Portanto, se você quiser adicionar apenas as tabelas, exibições e armazenados procedimentos necessários para o `SqlMembershipProvider` e `SqlRoleProvider` provedores, executar `aspnet_regsql.exe` na linha de comando. Como alternativa, você pode executar manualmente o subconjunto de T-SQL criar scripts usados por `aspnet_regsql.exe`. Esses scripts estão localizados na `WINDIR%\Microsoft.Net\Framework\v2.0.50727\` pasta com nomes como `InstallCommon.sql`, `InstallMembership.sql`, `InstallRoles.sql`, `InstallProfile.sql`, `InstallSqlState.sql`e assim por diante.

Neste ponto, criamos os objetos de banco de dados necessários para o `SqlMembershipProvider`. No entanto, ainda precisamos instruir a estrutura de associação que ele deve usar o `SqlMembershipProvider` (em vez de, digamos, o `ActiveDirectoryMembershipProvider`) e que o `SqlMembershipProvider` deve usar o `SecurityTutorials` banco de dados. Vamos examinar como especificar o provedor a ser usado e como personalizar configurações do provedor selecionado na etapa 4. Mas primeiro, vamos examinar os objetos de banco de dados que foram criados apenas mais profundo.

## <a name="step-3-a-look-at-the-schemas-core-tables"></a>Etapa 3: Examinar tabelas de principal do esquema

Ao trabalhar com as estruturas de associação e funções em um aplicativo ASP.NET, os detalhes de implementação são encapsulados pelo provedor. Em tutoriais futuros podemos interagirá com essas estruturas por meio do .NET Framework `Membership` e `Roles` classes. Ao usar essas APIs de alto nível não precisamos referem-se a mesmos com os detalhes de baixo nível, como quais consultas são executadas ou quais tabelas são modificadas com o `SqlMembershipProvider` e `SqlRoleProvider`.

Devido a isso, podemos pode usar as estruturas de associação e funções com confiança sem ter explorado o esquema de banco de dados criado na etapa 2. No entanto, ao criar as tabelas para armazenar dados de aplicativo é necessário criar entidades relacionadas a usuários ou funções. Isso ajuda a ter uma familiaridade com o `SqlMembershipProvider` e `SqlRoleProvider` esquemas ao estabelecer estrangeira restrições entre as tabelas de dados do aplicativo e as tabelas criadas na etapa 2 da chave. Além disso, em determinadas circunstâncias raras, talvez seja necessário fazer interface com o usuário e a função armazena diretamente no nível do banco de dados (em vez de através de `Membership` ou `Roles` classes).

### <a name="partitioning-the-user-store-into-applications"></a>Particionamento de Store de usuário em aplicativos

As estruturas de associação e funções são projetadas, de modo que um único repositório de usuário e a função pode ser compartilhado entre vários aplicativos diferentes. Um aplicativo ASP.NET que usa as estruturas de associação ou funções deve especificar qual partição de aplicativo para usar. Em resumo, vários aplicativos da web podem usar as mesmas lojas de usuário e a função. Figura 11 ilustra os repositórios de usuário e a função que são particionados em três aplicativos: HRSite, CustomerSite e SalesSite. Esses aplicativos web de três têm suas próprias funções e usuários exclusivos, embora todos eles fisicamente armazenam suas informações de conta e a função de usuário nas mesmas tabelas de banco de dados.


[![Contas de usuário podem ser particionadas em vários aplicativos](creating-the-membership-schema-in-sql-server-vb/_static/image32.png)](creating-the-membership-schema-in-sql-server-vb/_static/image31.png)

**Figura 11**: contas podem ser particionados em vários aplicativos de usuário ([clique para exibir a imagem em tamanho normal](creating-the-membership-schema-in-sql-server-vb/_static/image33.png))


O `aspnet_Applications` tabela é o que define essas partições. Cada aplicativo que usa o banco de dados para armazenar informações de conta de usuário é representado por uma linha nesta tabela. O `aspnet_Applications` tabela tem quatro colunas: `ApplicationId`, `ApplicationName`, `LoweredApplicationName`, e `Description`.`ApplicationId` é do tipo [ `uniqueidentifier` ](https://msdn.microsoft.com/library/ms187942.aspx) e é a chave da tabela primária; `ApplicationName` fornece um nome amigável exclusivo para cada aplicativo.

Opções estão vinculadas a outras tabelas relacionadas a associação e funções do `ApplicationId` campo `aspnet_Applications`. Por exemplo, o `aspnet_Users` tabela, que contém um registro para cada conta de usuário, tem um `ApplicationId` campo de chave estrangeira; idem para a `aspnet_Roles` tabela. O `ApplicationId` campo nessas tabelas Especifica a partição de aplicativo a conta de usuário ou a função pertence.

### <a name="storing-user-account-information"></a>Armazenando informações de conta de usuário

Informações de conta de usuário estão hospedadas em duas tabelas: `aspnet_Users` e `aspnet_Membership`. O `aspnet_Users` tabela contém campos que contêm as informações de conta de usuário essencial. As três colunas mais pertinentes são:

- `UserId`
- `UserName`
- `ApplicationId`

`UserId` é a chave primária (e do tipo `uniqueidentifier`). `UserName` é do tipo `nvarchar(256)` e, junto com a senha, torna-se as credenciais do usuário. (A senha do usuário é armazenada no `aspnet_Membership` tabela.) `ApplicationId` vincula a conta de usuário para um aplicativo específico no `aspnet_Applications`. Há uma composição [ `UNIQUE` restrição](https://msdn.microsoft.com/library/ms191166.aspx) sobre o `UserName` e `ApplicationId` colunas. Isso garante que um determinado aplicativo, cada nome de usuário é exclusivo, mesmo assim, permite a mesma `UserName` a ser usado em aplicativos diferentes.

O `aspnet_Membership` tabela inclui informações de conta de usuário adicionais, como a senha do usuário, endereço de email, o último logon data e hora e assim por diante. Há uma correspondência direta entre os registros na `aspnet_Users` e `aspnet_Membership` tabelas. Essa relação é garantida pela `UserId` campo `aspnet_Membership`, que serve como chave primária da tabela. Como o `aspnet_Users` tabela, `aspnet_Membership` inclui um `ApplicationId` campo que une essas informações para uma determinada partição de aplicativo.

### <a name="securing-passwords"></a>Proteger senhas

Informações de senha são armazenadas em do `aspnet_Membership` tabela. O `SqlMembershipProvider` permite que as senhas sejam armazenadas no banco de dados usando um dos três seguintes técnicas:

- **Limpar** -a senha é armazenada no banco de dados como texto sem formatação. Eu não recomendamos o uso dessa opção. Se o banco de dados for comprometido - seja ele por um hacker que localiza uma porta dos fundos ou um funcionário insatisfeito quem tem acesso de banco de dados - cada único as credenciais de usuário existem para a captura.
- **Hash** -as senhas são hash usando um algoritmo de hash unidirecional e um valor de sal gerado aleatoriamente. Esse valor com hash (juntamente com o salt) é armazenado no banco de dados.
- **Criptografado** -uma versão criptografada da senha é armazenada no banco de dados.

A técnica de armazenamento de senha usada depende de `SqlMembershipProvider` as configurações especificadas no `Web.config`. Vamos examinar como personalizar o `SqlMembershipProvider` configurações na etapa 4. O comportamento padrão é armazenar o hash da senha.

As colunas responsáveis por armazenar a senha são `Password`, `PasswordFormat`, e `PasswordSalt`. `PasswordFormat` é um campo do tipo `int` cujo valor indica que a técnica usada para armazenar a senha: 0 para limpar, 1 para Hashed; 2 para criptografado. `PasswordSalt` é atribuído a uma cadeia de caracteres gerada aleatoriamente a despeito da técnica de armazenamento de senha usada; o valor de `PasswordSalt` só é usado ao calcular o hash da senha. Por fim, o `Password` coluna contém os dados de senha real, seja a senha de texto sem formatação, o hash de senha ou a senha criptografada.

Tabela 1 ilustra essas três colunas aparência para as várias técnicas de armazenamento ao armazenar a senha MySecret! .

| **Técnica de armazenamento&lt;\_o3a\_p /&gt;** | **Senha&lt;\_o3a\_p /&gt;** | **PasswordFormat&lt;\_o3a\_p /&gt;** | **PasswordSalt&lt;\_o3a\_p /&gt;** |
| --- | --- | --- | --- |
| Clear | MySecret! | 0 | tTnkPlesqissc2y2SMEygA== |
| Em hash | 2oXm6sZHWbTHFgjgkGQsc2Ec9ZM= | 1 | wFgjUfhdUFOCKQiI61vtiQ== |
| Criptografado | 62RZgDvhxykkqsMchZ0Yly7HS6onhpaoCYaRxV8g0F4CW56OXUU3e7Inza9j9BKp | 2 | LSRzhGS/aa/oqAXGLHJNBw== |

**Tabela 1**: valores de exemplo para os campos relacionados à senha ao armazenar a senha MySecret!

> [!NOTE]
> A criptografia específica ou o algoritmo de hash usado pelas `SqlMembershipProvider` é determinado pelas configurações no `<machineKey>` elemento. Discutimos a este elemento de configuração na etapa 3 das <a id="Tutorial3"> </a> [ *configuração de autenticação de formulários e tópicos avançados* ](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) tutorial.

### <a name="storing-roles-and-role-associations"></a>Armazenamento de funções e associações de função

A estrutura de funções permite aos desenvolvedores definir um conjunto de funções e especifique o que os usuários pertencem a quais funções. Essas informações são capturadas no banco de dados por meio de duas tabelas: `aspnet_Roles` e `aspnet_UsersInRoles`. Cada registro no `aspnet_Roles` tabela representa uma função para um aplicativo específico. Assim como o `aspnet_Users` tabela, o `aspnet_Roles` tabela tem três colunas pertinentes à nossa discussão:

- `RoleId`
- `RoleName`
- `ApplicationId`

`RoleId` é a chave primária (e do tipo `uniqueidentifier`). `RoleName` é do tipo `nvarchar(256)`. E `ApplicationId` vincula a conta de usuário para um aplicativo específico no `aspnet_Applications`. Há uma composição `UNIQUE` restrição sobre o `RoleName` e `ApplicationId` colunas, garantindo que um determinado aplicativo cada nome de função é exclusivo.

O `aspnet_UsersInRoles` tabela atua como um mapeamento entre usuários e funções. Há apenas duas colunas - `UserId` e `RoleId` - e juntos eles formam uma chave primária composta.

## <a name="step-4-specifying-the-provider-and-customizing-its-settings"></a>Etapa 4: Especifica o provedor e personalizar suas configurações

Todas as estruturas que dão suporte ao modelo provedor - como as estruturas de associação e funções - falta próprios detalhes de implementação e, em vez disso, delegar essa responsabilidade para uma classe de provedor. No caso da estrutura de associação, o `Membership` classe define a API de gerenciamento de contas de usuário, mas ele não interage diretamente com qualquer repositório de usuário. Em vez disso, o `Membership` mão de métodos da classe a solicitação para o provedor configurado - usaremos o `SqlMembershipProvider`. Quando invocamos um dos métodos na `Membership` classe, como a estrutura de associação sabe para delegar a chamada para o `SqlMembershipProvider`?

O `Membership` classe tem um [ `Providers` propriedade](https://msdn.microsoft.com/library/system.web.security.membership.providers.aspx) que contém uma referência a todas as classes de provedor registrado disponíveis para uso pela estrutura de associação. Cada provedor registrado tem um nome associado e um tipo. O nome oferece uma maneira amigável a humanos para fazer referência a um determinado provedor no `Providers` coleção, enquanto o tipo identifica a classe de provedor. Além disso, cada provedor registrado pode incluir as definições de configuração. Definições de configuração para a estrutura de associação incluem `PasswordFormat` e `requiresUniqueEmail`, entre muitas outras. Consulte a tabela 2 para obter uma lista completa de definições de configuração usado pelo `SqlMembershipProvider`.

O `Providers` conteúdo da propriedade é especificado por meio de definições de configuração do aplicativo da web. Por padrão, todos os aplicativos web têm um provedor chamado `AspNetSqlMembershipProvider` do tipo `SqlMembershipProvider`. Este provedor de associação padrão é registrado no `machine.config` (localizado em `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample1.xml)]

Como a marcação acima mostra, o [ `<membership>` elemento](https://msdn.microsoft.com/library/1b9hw62f.aspx) define as definições de configuração para a estrutura de associação durante a [ `<providers>` elemento filho](https://msdn.microsoft.com/library/6d4936ht.aspx) Especifica registrado provedores. Provedores podem ser adicionados ou removidos usando o [ `<add>` ](https://msdn.microsoft.com/library/whae3t94.aspx) ou [ `<remove>` ](https://msdn.microsoft.com/library/aykw9a6d.aspx) elementos; use o [ `<clear>` ](https://msdn.microsoft.com/library/t062y6yc.aspx) elemento do qual remover tudo no momento provedores registrados. Como a marcação acima mostra, `machine.config` adiciona um provedor chamado `AspNetSqlMembershipProvider` do tipo `SqlMembershipProvider`.

Além de `name` e `type` atributos, o `<add>` elemento contém atributos que definem os valores para várias configurações de configuração. A tabela 2 lista disponíveis `SqlMembershipProvider`-definições de configuração específicas, junto com uma descrição de cada um.

> [!NOTE]
> Quaisquer valores de padrão indicados na tabela 2 referem-se os valores padrão definidos no `SqlMembershipProvider` classe. Observe que não todas as definições de configuração no `AspNetSqlMembershipProvider` correspondem aos valores padrão da `SqlMembershipProvider` classe. Por exemplo, se não for especificado em um provedor de associação, o `requiresUniqueEmail` definir padrões como true. No entanto, o `AspNetSqlMembershipProvider` substitui o valor padrão, explicitamente especificando um valor de `false`.

| **Definindo&lt;\_o3a\_p /&gt;** | **Descrição&lt;\_o3a\_p /&gt;** |
| --- | --- |
| `ApplicationName` | Lembre-se de que a estrutura de associação permite para um repositório de usuário único a ser particionado em vários aplicativos. Essa configuração indica o nome da partição do aplicativo usado pelo provedor de associação. Se esse valor não é especificados explicitamente, ela é definida, em tempo de execução, o valor do caminho de raiz virtual do aplicativo. |
| `commandTimeout` | Especifica o valor de tempo limite do comando SQL (em segundos). O valor padrão é 30. |
| `connectionStringName` | O nome da cadeia de caracteres de conexão no `<connectionStrings>` elemento a ser usado para se conectar ao banco de dados de repositório de usuário. Esse valor é necessário. |
| `description` | Fornece uma descrição amigável do provedor registrado. |
| `enablePasswordRetrieval` | Especifica se os usuários podem recuperar sua senha esquecida. O valor padrão é `false`. |
| `enablePasswordReset` | Indica se os usuários têm permissão para redefinir sua senha. Assume o padrão de `true`. |
| `maxInvalidPasswordAttempts` | O número máximo de tentativas de logon malsucedidas que podem ocorrer para um determinado usuário durante especificado `passwordAttemptWindow` antes que o usuário está bloqueado. O valor padrão é 5. |
| `minRequiredNonalphanumericCharacters` | O número mínimo de caracteres não alfanuméricos devem aparecer em uma senha de usuário. Esse valor deve estar entre 0 e 128; o padrão é 1. |
| `minRequiredPasswordLength` | O número mínimo de caracteres necessários na senha. Esse valor deve estar entre 0 e 128; o padrão é 7. |
| `name` | O nome do provedor registrado. Esse valor é necessário. |
| `passwordAttemptWindow` | O número de minutos durante os quais falha são rastreadas tentativas de logon. Se um usuário fornece credenciais de logon inválidas `maxInvalidPasswordAttempts` especificado de vezes dentro dessa janela, eles estão bloqueados. O valor padrão é 10. |
| `PasswordFormat` | O formato de armazenamento de senha: `Clear`, `Hashed`, ou `Encrypted`. O padrão é `Hashed`. |
| `passwordStrengthRegularExpression` | Se fornecido, essa expressão regular é usada para avaliar a força de senha de usuário selecionada ao criar uma nova conta ou ao alterar suas senhas. O valor padrão é uma cadeia de caracteres vazia. |
| `requiresQuestionAndAnswer` | Especifica se um usuário deve responder sua pergunta de segurança ao recuperar ou redefinir sua senha. O valor padrão é `true`. |
| `requiresUniqueEmail` | Indica se todas as contas de usuário em uma partição de aplicativo fornecido devem ter um endereço de email exclusivo. O valor padrão é `true`. |
| `type` | Especifica o tipo do provedor. Esse valor é necessário. |

**Tabela 2**: associação e `SqlMembershipProvider` definições de configuração

Além `AspNetSqlMembershipProvider`, outros provedores de associação podem ser registrados em uma base por aplicativo adicionando marcação semelhante para o `Web.config` arquivo.

> [!NOTE]
> A estrutura de funções funciona quase da mesma forma: há um provedor de função registrado padrão no `machine.config` e os provedores registrados podem ser personalizados em uma base por aplicativo no `Web.config`. Vamos examinar a estrutura de funções e sua marcação de configuração em detalhes em um tutorial futuro.

### <a name="customizing-thesqlmembershipprovidersettings"></a>Personalizando o`SqlMembershipProvider`configurações

O padrão `SqlMembershipProvider` (`AspNetSqlMembershipProvider`) tem seu `connectionStringName` atributo definido como `LocalSqlServer`. Como o `AspNetSqlMembershipProvider` provedor, o nome de cadeia de caracteres de conexão `LocalSqlServer` é definido em `machine.config`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample2.xml)]

Como você pode ver, essa cadeia de caracteres de conexão define uma SQL 2005 Express Edition, banco de dados localizado em | DataDirectory|aspnetdb.mdf. A cadeia de caracteres | DataDirectory | é convertido em tempo de execução para apontar para o `~/App_Data/` directory, portanto, o caminho do banco de dados | DataDirectory|aspnetdb.mdf se traduz em `~/App_Data` / `aspnet.mdf`.

Se não tiver especificado quaisquer informações de provedor de associação em nosso aplicativo `Web.config` arquivo, o aplicativo usa o provedor de associação padrão registrado, `AspNetSqlMembershipProvider`. Se o `~/App_Data/aspnet.mdf` banco de dados não existe, o tempo de execução do ASP.NET automaticamente criá-lo e adicionar o esquema de serviços de aplicativo. No entanto, não queremos usar o `aspnet.mdf` banco de dados; em vez disso, queremos usar o `SecurityTutorials.mdf` banco de dados criados na etapa 2. Essa modificação pode ser feita de duas maneiras:

- <strong>Especifique um valor para o</strong><strong>`LocalSqlServer`</strong><strong>nome de cadeia de caracteres de conexão no</strong><strong>`Web.config`</strong><strong>.</strong> Substituindo o `LocalSqlServer` o valor do nome da cadeia de conexão no `Web.config`, podemos usar o provedor de associação padrão registrado (`AspNetSqlMembershipProvider`) e ainda funcionar corretamente com o `SecurityTutorials.mdf` banco de dados. Essa abordagem é suficiente se você for conteúdo com as definições de configuração especificadas pelo `AspNetSqlMembershipProvider`. Para obter mais informações sobre essa técnica, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da postagem de blog [Configurando serviços de aplicativos do ASP.NET 2.0 para usar o SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Adicionar um novo provedor registrado do tipo</strong><strong>`SqlMembershipProvider`</strong><strong>e configurar seu</strong><strong>`connectionStringName`</strong><strong>configuração para apontar para o</strong> <strong>`SecurityTutorials.mdf`</strong> <strong>banco de dados.</strong> Essa abordagem é útil em situações em que você deseja personalizar outras propriedades de configuração, além da cadeia de caracteres de conexão de banco de dados. Nos meus próprios projetos eu sempre uso essa abordagem devido à sua flexibilidade e a legibilidade.

Antes de podermos adicionar um novo provedor registrado que faz referência a `SecurityTutorials.mdf` banco de dados, primeiro precisamos adicionar um valor de cadeia de caracteres de conexão apropriado na `<connectionStrings>` seção `Web.config`. A marcação a seguir adiciona uma nova cadeia de conexão denominada `SecurityTutorialsConnectionString` que referencia o SQL Server 2005 Express Edition `SecurityTutorials.mdf` banco de dados a `App_Data` pasta.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample3.xml)]

> [!NOTE]
> Se você estiver usando um arquivo de banco de dados alternativo, atualize a cadeia de caracteres de conexão, conforme necessário. Para obter mais informações sobre que formam a cadeia de caracteres de conexão correta, consulte [ConnectionStrings.com](http://www.connectionstrings.com/).

Em seguida, adicione a seguinte marcação de configuração de associação para o `Web.config` arquivo. Essa marcação registra um novo provedor chamado `SecurityTutorialsSqlMembershipProvider`.

[!code-xml[Main](creating-the-membership-schema-in-sql-server-vb/samples/sample4.xml)]

Além de registrar o `SecurityTutorialsSqlMembershipProvider` provedor, a marcação acima define o `SecurityTutorialsSqlMembershipProvider` como o provedor padrão (via o `defaultProvider` de atributo no `<membership>` elemento). Lembre-se de que a estrutura de associação pode ter vários provedores registrados. Uma vez que `AspNetSqlMembershipProvider` está registrado como o primeiro provedor na `machine.config`, ele serve como o provedor padrão, a menos que podemos indique o contrário.

Atualmente, o nosso aplicativo tem dois provedores registrados: `AspNetSqlMembershipProvider` e `SecurityTutorialsSqlMembershipProvider`. No entanto, antes de registrar o `SecurityTutorialsSqlMembershipProvider` provedor poderia ter compensamos todas anteriormente os provedores registrados com a adição de um [ `<clear />` elemento](https://msdn.microsoft.com/library/t062y6yc.aspx) imediatamente antes do nosso `<add>` elemento. Isso seria limpar os `AspNetSqlMembershipProvider` na lista de provedores registrados, o que significa que o `SecurityTutorialsSqlMembershipProvider` seria o único provedor de associação registrado. Se usamos essa abordagem, não seria preciso marcar o `SecurityTutorialsSqlMembershipProvider` como o provedor padrão, já que seria o único provedor de associação registrado. Para obter mais informações sobre como usar `<clear />`, consulte [Using `<clear />` ao adicionar provedores](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx).

Observe que o `SecurityTutorialsSqlMembershipProvider`do `connectionStringName` definindo referências a just-adicionado `SecurityTutorialsConnectionString` nome da cadeia de conexão e que seu `applicationName` configuração foi definida como um valor de SecurityTutorials. Além disso, o `requiresUniqueEmail` configuração foi definida `true`. Todas as outras opções de configuração são idênticas aos valores no `AspNetSqlMembershipProvider`. Fique à vontade para fazer as modificações de configuração aqui, se desejar. Por exemplo, você pode reforçar a força da senha, exigindo dois caracteres não alfanuméricos em vez de um, ou ao aumentar o comprimento da senha para oito caracteres, em vez de sete.

> [!NOTE]
> Lembre-se de que a estrutura de associação permite para um repositório de usuário único a ser particionado em vários aplicativos. O provedor de associação `applicationName` configuração indica que o provedor usa ao trabalhar com o repositório do usuário de aplicativo. É importante que você definir explicitamente um valor para o `applicationName` definição de configuração, pois se o `applicationName` não for definido explicitamente, ele é atribuído ao caminho de raiz virtual do aplicativo web em tempo de execução. Isso funciona bem desde que o caminho do aplicativo virtual raiz não é alterado, mas se você mover o aplicativo para um caminho diferente, o `applicationName` configuração mudará também. Quando isso acontece, o provedor de associação começar a trabalhar com uma partição de aplicativo diferente da que foi usada anteriormente. Contas de usuário criadas antes da movimentação serão residem em uma partição de aplicativo diferentes e esses usuários não poderão fazer logon no site do. Para obter uma discussão mais detalhada sobre essa questão, consulte [sempre defina a `applicationName` propriedade quando configurar o ASP.NET 2.0 associação e outros provedores de](https://weblogs.asp.net/scottgu/443634).

## <a name="summary"></a>Resumo

Neste ponto, temos um banco de dados com os serviços de aplicativo configurado (`SecurityTutorials.mdf`) e tiver configurado o nosso aplicativo web para que a estrutura de associação usa o `SecurityTutorialsSqlMembershipProvider` provedor acabou de registrar. Este provedor registrado é do tipo `SqlMembershipProvider` e tem seu `connectionStringName` definida como a cadeia de conexão apropriado (`SecurityTutorialsConnectionString`) e sua `applicationName` valor explicitamente definido.

Agora estamos prontos para usar a estrutura de associação de nosso aplicativo. O próximo tutorial, vamos examinar como criar novas contas de usuário. A seguir que iremos explorar autenticação de usuários, fazer a autorização baseada em usuário e armazenar informações adicionais relacionadas ao usuário no banco de dados.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Sempre defina o `applicationName` propriedade quando configurar 2.0 associação do ASP.NET e outros provedores](https://weblogs.asp.net/scottgu/443634)
- [Configurando o ASP.NET 2.0 dos serviços de aplicativos para utilizar o SQL Server 2000 ou SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx)
- [Baixar o SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)
- [Examinando o ASP.NET 2.0 s associações, funções e perfis](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [O `<add>` elemento para provedores de associação](https://msdn.microsoft.com/library/whae3t94.aspx)
- [O `<membership>` elemento](https://msdn.microsoft.com/library/1b9hw62f.aspx)
- [O `<providers>` elemento de associação](https://msdn.microsoft.com/library/6d4936ht.aspx)
- [Usando `<clear />` ao adicionar provedores](https://weblogs.asp.net/scottgu/archive/2006/11/20/common-gotcha-don-t-forget-to-clear-when-adding-providers.aspx)
- [Trabalhando diretamente com o `SqlMembershipProvider`](http://aspnet.4guysfromrolla.com/articles/091207-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre os tópicos contidos neste tutorial

- [Noções básicas sobre associações do ASP.NET](../../../videos/authentication/understanding-aspnet-memberships.md)
- [Configurando o SQL para trabalhar com esquemas de associação](../../../videos/authentication/configuring-sql-to-work-with-membership-schemas.md)
- [Alterar as configurações de associação do esquema de associação padrão](../../../videos/authentication/changing-membership-settings-in-the-default-membership-schema.md)

### <a name="about-the-author"></a>Sobre o autor

Scott Mitchell, autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é  *[Sams Teach por conta própria ASP.NET 2.0 em 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott pode ser contatado pelo [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Alicja Maziarz. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](storing-additional-user-information-cs.md)
> [Próximo](creating-user-accounts-vb.md)
