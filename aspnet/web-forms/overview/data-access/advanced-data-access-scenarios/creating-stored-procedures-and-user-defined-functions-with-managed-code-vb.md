---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: "Criando procedimentos armazenados e funções definidas pelo usuário com gerenciado código (VB) | Microsoft Docs"
author: rick-anderson
description: "Microsoft SQL Server 2005 integra-se com o .NET Common Language Runtime para permitir que os desenvolvedores criem objetos de banco de dados por meio de código gerenciado. Este tutorial..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: efec52c4085c24b1d6227a86f7c435ca657e493c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>Criando procedimentos armazenados e funções definidas pelo usuário com código gerenciado (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip) ou [baixar PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> Microsoft SQL Server 2005 integra-se com o .NET Common Language Runtime para permitir que os desenvolvedores criem objetos de banco de dados por meio de código gerenciado. Este tutorial mostra como criar procedimentos armazenados gerenciados e funções definidas pelo usuário com o seu código Visual Basic ou c# gerenciadas. Também, vemos como essas edições do Visual Studio permitem que você depurar esses objetos de banco de dados gerenciados.


## <a name="introduction"></a>Introdução

Bancos de dados como s do Microsoft SQL Server 2005 usam o [Transact-Structured (linguagem T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL) para inserir, modificar e recuperar dados. A maioria dos sistemas de banco de dados incluem construções para agrupamento de uma série de instruções SQL que pode ser executado como uma unidade única e reutilizável. Procedimentos armazenados são um exemplo. Outra vantagem é *funções definidas pelo usuário*(UDFs), uma construção que vamos examinar mais detalhadamente na etapa 9.

Essencialmente, o SQL foi projetado para trabalhar com conjuntos de dados. O `SELECT`, `UPDATE`, e `DELETE` instruções inerentemente se aplicam a todos os registros na tabela correspondente e são limitadas somente por seus `WHERE` cláusulas. Ainda há muitos recursos de linguagem projetados para trabalhar com um registro de cada vez e para a manipulação de dados escalares. [`CURSOR`s](http://www.sqlteam.com/item.asp?ItemID=553) permitem que um conjunto de registros em loop por meio de um de cada vez. Funções de manipulação de como cadeia de caracteres `LEFT`, `CHARINDEX`, e `PATINDEX` funcionam com dados escalares. SQL também inclui instruções de fluxo de controle como `IF` e `WHILE`.

Antes do Microsoft SQL Server 2005, procedimentos armazenados e UDFs só podem ser definidos como uma coleção de instruções T-SQL. No entanto, o SQL Server 2005, foi projetado para fornecer integração com o [Common Language Runtime (CLR)](https://msdn.microsoft.com/en-us/netframework/aa497266.aspx), que é o tempo de execução usado por todos os assemblies do .NET. Consequentemente, os procedimentos armazenados e UDFs em um banco de dados do SQL Server 2005 podem ser criados usando código gerenciado. Ou seja, você pode criar um procedimento armazenado ou UDF como um método em uma classe do Visual Basic. Isso permite que esses procedimentos armazenados e UDFs para utilizar a funcionalidade do .NET Framework e de suas próprias classes personalizadas.

Neste tutorial, examinaremos como criar gerenciado procedimentos armazenados e funções definidas pelo usuário e como integrá-las em nosso banco de dados Northwind. Permitir que o s começar!

> [!NOTE]
> Objetos de banco de dados gerenciado oferecem algumas vantagens sobre suas contrapartes SQL. Riqueza de linguagem e familiaridade e a capacidade de reutilizar a lógica e o código existente são as principais vantagens. Mas os objetos de banco de dados gerenciados poderão ser menos eficiente, ao trabalhar com conjuntos de dados que não envolvem muita lógica de procedimento. Para obter uma discussão mais completa sobre as vantagens de usar código gerenciado em vez de T-SQL, confira o [vantagens de usar código gerenciado para criar objetos de banco de dados](https://msdn.microsoft.com/en-us/library/k2e1fb36(VS.80).aspx).


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>Etapa 1: Mover o banco de dados Northwind de`App_Data`

Todos os nossos tutoriais até o momento tem usado um arquivo de banco de dados do Microsoft SQL Server 2005 Express Edition em aplicativo s web `App_Data` pasta. Colocar o banco de dados em `App_Data` simplificado de distribuição e executar esses tutoriais conforme todos os arquivos foram localizados em um diretório e não necessário nenhum etapas de configuração adicionais para testar o tutorial.

Para este tutorial, no entanto, permitem s mover o banco de dados Northwind de `App_Data` e registre-o explicitamente com a instância de banco de dados do SQL Server 2005 Express Edition. Enquanto estamos pode executar as etapas neste tutorial com o banco de dados no `App_Data` pasta, um número das etapas é feito muito mais simples registrando explicitamente o banco de dados com a instância de banco de dados do SQL Server 2005 Express Edition.

O download para este tutorial tem os arquivos de banco de dados de duas - `NORTHWND.MDF` e `NORTHWND_log.LDF` - colocada em uma pasta chamada `DataFiles`. Se você estiver seguindo junto com sua própria implementação dos tutoriais, feche o Visual Studio e mova o `NORTHWND.MDF` e `NORTHWND_log.LDF` arquivos do site s `App_Data` para uma pasta fora do site. Depois que os arquivos de banco de dados foram movidos para outra pasta, é preciso registrar o banco de dados Northwind com a instância de banco de dados do SQL Server 2005 Express Edition. Isso pode ser feito no SQL Server Management Studio. Se você tiver uma não - Express Edition do SQL Server 2005 instalado em seu computador, em seguida, você provavelmente já terão Management Studio instalado. Se você tiver o SQL Server 2005 Express Edition no computador, reserve um tempo para baixar e instalar somente [Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Inicie o SQL Server Management Studio. Como mostra a Figura 1, o Management Studio começa perguntando qual servidor para se conectar ao. Digite localhost\SQLExpress para o nome do servidor, escolha a autenticação do Windows na lista suspensa de autenticação e clique em conectar.


![Conecte-se à instância do banco de dados apropriado](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**Figura 1**: conectar-se à instância do banco de dados apropriado


Quando você tiver conectado, a janela Pesquisador de objetos lista informações sobre a instância de banco de dados do SQL Server 2005 Express Edition, incluindo seus bancos de dados, informações de segurança, opções de gerenciamento e assim por diante.

É necessário anexar o banco de dados Northwind no `DataFiles` pasta (ou onde quer que você talvez tenha movido) para a instância de banco de dados do SQL Server 2005 Express Edition. Com o botão direito na pasta de bancos de dados e escolha a opção de anexar o menu de contexto. Isso abrirá a caixa de diálogo anexar bancos de dados. Clique no botão Adicionar, fazer drill down até apropriada `NORTHWND.MDF` de arquivo e, em seguida, clique em Okey. Neste ponto, sua tela deve ser semelhante a Figura 2.


[![Conecte-se à instância do banco de dados apropriado](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**Figura 2**: conectar-se à instância apropriada do banco de dados ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))


> [!NOTE]
> Ao conectar-se à instância do SQL Server 2005 Express Edition com o Management Studio a caixa de diálogo anexar bancos de dados não permitem detalhar os diretórios de perfil de usuário, como Meus documentos. Portanto, certifique-se de colocar o `NORTHWND.MDF` e `NORTHWND_log.LDF` arquivos em um diretório de perfil de usuário não.


Clique no botão Okey para anexar o banco de dados. A caixa de diálogo anexar bancos de dados será fechado e o Pesquisador de objetos agora deve listar o banco de dados anexado apenas. A Northwind banco de dados tem um nome como a probabilidade é `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`. Renomear o banco de dados Northwind clicando duas vezes no banco de dados e escolha Renomear.


![Renomear o banco de dados Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**Figura 3**: renomear o banco de dados Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>Etapa 2: Criar uma nova solução e projeto do SQL Server no Visual Studio

Para criar procedimentos armazenados gerenciados ou UDFs no SQL Server 2005 é gravará o procedimento armazenado e a lógica UDF como o código do Visual Basic em uma classe. Depois que o código foi escrito, será necessário compilar essa classe em um assembly (um `.dll` arquivo), registre o assembly com o banco de dados do SQL Server e, em seguida, criar um procedimento armazenado ou UDF no banco de dados que aponta para o método correspondente em o assembly. Essas etapas podem ser executadas manualmente. Isso pode criar o código em qualquer texto editor, compilá-lo na linha de comando usando o compilador do Visual Basic (`vbc.exe`), registre-o com o banco de dados usando o [ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/en-us/library/ms189524.aspx) comando ou no Management Studio e adicione o armazenado procedimento ou objeto UDF através de meios semelhantes. Felizmente, as versões Professional e sistemas de equipe do Visual Studio incluem um tipo de projeto do SQL Server que automatiza a essas tarefas. Neste tutorial percorreremos usando o tipo de projeto do SQL Server para criar um procedimento armazenado gerenciado e o UDF.

> [!NOTE]
> Se você estiver usando o Visual Web Developer ou Standard edition do Visual Studio, você precisará usar a abordagem manual em vez disso. Etapa 13 fornece instruções detalhadas para executar essas etapas manualmente. Recomendo que você leia as etapas 2 a 12 antes de ler a etapa 13 desde que essas etapas incluem instruções de configuração do SQL Server importantes que devem ser aplicadas, independentemente de qual versão do Visual Studio que você está usando.


Comece abrindo o Visual Studio. No menu Arquivo, escolha o novo projeto para exibir a caixa de diálogo Novo projeto caixa (consulte a Figura 4). Fazer drill down até o tipo de projeto de banco de dados e, em seguida, nos modelos de listados à direita, optar por criar um novo projeto do SQL Server. Escolheu para este projeto `ManagedDatabaseConstructs` e colocou em uma solução nomeada `Tutorial75`.


[![Criar um novo projeto do SQL Server](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**Figura 4**: criar um novo projeto do SQL Server ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))


Clique no botão Okey na caixa de diálogo Novo projeto para criar a solução e projeto do SQL Server.

Um projeto do SQL Server estiver associado a um determinado banco de dados. Consequentemente, depois de criar o novo projeto do SQL Server são imediatamente solicitado para especificar essas informações. Figura 5 mostra a caixa de diálogo Nova referência de banco de dados que foram preenchida para apontar para o banco de dados Northwind que são registrados na instância de banco de dados do SQL Server 2005 Express Edition novamente na etapa 1.


![Associar o projeto do SQL Server com o banco de dados Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**Figura 5**: associar o projeto do SQL Server com o banco de dados Northwind


Para depurar os procedimentos armazenados gerenciados e UDFs criaremos dentro desse projeto, é preciso habilitar a depuração de suporte para a conexão de SQL/CLR. Sempre que a associação de um projeto do SQL Server com um novo banco de dados (como fizemos na Figura 5), o Visual Studio perguntará se queremos habilitar a depuração SQL/CLR a conexão (consulte a Figura 6). Clique em Sim.


![Habilitar a depuração SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**Figura 6**: habilitar a depuração SQL/CLR


Neste ponto, o novo projeto do SQL Server foi adicionado à solução. Ele contém uma pasta chamada `Test Scripts` com um arquivo chamado `Test.sql`, que é usado para depurar objetos de banco de dados gerenciados criados no projeto. Examinaremos depuração na etapa 12.

Estamos agora pode adicionar novos procedimentos armazenados gerenciados e UDFs para este projeto, mas antes que permitem s incluem primeiro nosso aplicativo web existente na solução. No menu Arquivo, selecione a opção de adicionar e escolha Site existente. Navegue até a pasta do site apropriado e clique em Okey. Como mostra a Figura 7, isso atualizará a solução para incluir os dois projetos: o site e o `ManagedDatabaseConstructs` projeto do SQL Server.


![O Gerenciador de soluções agora inclui dois projetos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**Figura 7**: O Gerenciador de soluções agora inclui dois projetos


O `NORTHWNDConnectionString` valor em `Web.config` atualmente faz referência a `NORTHWND.MDF` arquivo o `App_Data` pasta. Já que removemos esse banco de dados `App_Data` e registrá-lo explicitamente na instância de banco de dados do SQL Server 2005 Express Edition, precisamos atualizar correspondente a `NORTHWNDConnectionString` valor. Abra o `Web.config` arquivo no site e alterar o `NORTHWNDConnectionString` valor para que lê a cadeia de caracteres de conexão: `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`. Após essa alteração, o `<connectionStrings>` seção `Web.config` deve ser semelhante à seguinte:


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> Como discutido o [tutorial anterior](debugging-stored-procedures-vb.md), ao depurar um objeto do SQL Server de um aplicativo cliente, como um site ASP.NET, é preciso desabilitar o pooling de conexão. A cadeia de caracteres de conexão mostrada acima desabilita pool de conexão ( `Pooling=false` ). Se você não planeja depuração de procedimentos armazenados gerenciados e UDFs do site do ASP.NET, habilite o pool de conexão.


## <a name="step-3-creating-a-managed-stored-procedure"></a>Etapa 3: Criar um gerenciado de procedimento armazenado

Para adicionar um procedimento armazenado gerenciado para o banco de dados Northwind é preciso primeiro criar o procedimento armazenado como um método no projeto do SQL Server. No Gerenciador de soluções, clique com botão direito no `ManagedDatabaseConstructs` nome do projeto e escolha Adicionar um novo item. Isso exibirá a caixa de diálogo Adicionar Novo Item, que lista os tipos de objetos de banco de dados gerenciado que podem ser adicionados ao projeto. Como mostra a Figura 8, isso inclui procedimentos armazenados e funções definidas pelo usuário, entre outros.

Permitir que o s comece adicionando um procedimento armazenado que simplesmente retorna todos os produtos que foram descontinuados. Nomeie o novo arquivo de procedimento armazenado `GetDiscontinuedProducts.vb`.


[![Adicionar um novo procedimento armazenado chamado GetDiscontinuedProducts.vb](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**Figura 8**: adicionar um novo chamado procedimento de armazenado `GetDiscontinuedProducts.vb` ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))


Isso criará um novo arquivo de classe do Visual Basic com o seguinte conteúdo:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

Observe que o procedimento armazenado é implementado como um `Shared` método dentro de um `Partial` arquivo de classe chamado `StoredProcedures`. Além disso, o `GetDiscontinuedProducts` método é decorado com o [ `SqlProcedure` atributo](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx), que marca o método como um procedimento armazenado.

O código a seguir cria um `SqlCommand` objeto e define seu `CommandText` para um `SELECT` consulta que retorna todas as colunas da `Products` tabela de produtos cujos `Discontinued` campo é igual a 1. Em seguida, ele executa o comando e envia os resultados para o aplicativo cliente. Adicione este código para o `GetDiscontinuedProducts` método.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

Todos os objetos de banco de dados gerenciado tem acesso a um [ `SqlContext` objeto](https://msdn.microsoft.com/en-us/library/ms131108.aspx) que representa o contexto do chamador. O `SqlContext` fornece acesso a um [ `SqlPipe` objeto](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlpipe.aspx) por meio de seu [ `Pipe` propriedade](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx). Isso `SqlPipe` objeto é usado para ferry informações entre o banco de dados do SQL Server e o aplicativo de chamada. Como o nome sugere, o [ `ExecuteAndSend` método](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx) executa um passado-in `SqlCommand` objeto e enviam os resultados de volta para o aplicativo cliente.

> [!NOTE]
> Objetos de banco de dados gerenciados são mais adequados para procedimentos armazenados e UDFs que usam a lógica de procedimento em vez de lógica baseada em conjunto. Lógica de procedimento envolve trabalhar com conjuntos de dados em uma base linha por linha ou trabalhando com dados escalares. O `GetDiscontinuedProducts` não envolve o método que acabamos de criar, no entanto, nenhuma lógica de procedimento. Portanto, ele idealmente poderia ser implementado como um procedimento armazenado T-SQL. Ele é implementado como um procedimento armazenado gerenciado para demonstrar as etapas necessárias para criar e implantar gerenciado procedimentos armazenados.


## <a name="step-4-deploying-the-managed-stored-procedure"></a>Etapa 4: Implantação gerenciada de procedimento armazenado

Com esse código completo, você está pronto para implantá-lo no banco de dados Northwind. Implantar um projeto SQL Server compila o código em um assembly, registra o assembly com o banco de dados e cria os objetos correspondentes no banco de dados e vinculá-los para os métodos apropriados no assembly. Mais precisamente o conjunto exato das tarefas executadas pela opção de implantação é informado na etapa 13. Clique com botão direito no `ManagedDatabaseConstructs` nome no Gerenciador de soluções do projeto e escolha a opção de implantar. No entanto, a implantação falhará com o seguinte erro: sintaxe incorreta próxima a 'Externo'. Talvez seja necessário definir o nível de compatibilidade do banco de dados atual para um valor mais alto para habilitar este recurso. Consulte a Ajuda para o procedimento armazenado `sp_dbcmptlevel`.

Essa mensagem de erro ocorre ao tentar registrar o assembly com o banco de dados Northwind. Para registrar um assembly com um banco de dados do SQL Server 2005, o nível de compatibilidade do banco de dados s deve ser definido como 90. Por padrão, os novos bancos de dados do SQL Server 2005 têm um nível de compatibilidade 90. No entanto, os bancos de dados criados usando o Microsoft SQL Server 2000 tem um nível de compatibilidade padrão de 80. Como o banco de dados Northwind foi inicialmente um banco de dados do Microsoft SQL Server 2000, seu nível de compatibilidade está definido para 80 e, portanto, precisa ser aumentada para 90 para registrar os objetos de banco de dados gerenciados.

Para atualizar o nível de compatibilidade do banco de dados s, abra uma janela nova consulta no Management Studio e digite:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

Clique no ícone executar na barra de ferramentas para executar a consulta anterior.


[![Atualizar o nível de compatibilidade do banco de dados Northwind s](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**Figura 9**: atualizar o banco de dados Northwind s nível de compatibilidade ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))


Depois de atualizar o nível de compatibilidade, reimplante o projeto do SQL Server. Neste momento a implantação deve ser concluída sem erros.

Retornar ao SQL Server Management Studio, clique com botão direito no banco de dados Northwind no Pesquisador de objetos e escolha atualizar. Em seguida, fazer uma busca detalhada na pasta programação e, em seguida, expanda a pasta de módulos (assemblies). Como mostra a Figura 10, o banco de dados Northwind agora inclui o assembly gerado pelo `ManagedDatabaseConstructs` projeto.


![O Assembly de ManagedDatabaseConstructs está agora registrado com o banco de dados Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**Figura 10**: O `ManagedDatabaseConstructs` Assembly é registrado com o banco de dados Northwind


Além disso, expanda a pasta de procedimentos armazenados. Lá, você verá um procedimento armazenado denominado `GetDiscontinuedProducts`. Esse procedimento armazenado foi criado pelo processo de implantação e aponta para o `GetDiscontinuedProducts` método o `ManagedDatabaseConstructs` assembly. Quando o `GetDiscontinuedProducts` procedimento armazenado é executado, ele, por sua vez, executa o `GetDiscontinuedProducts` método. Como esse é um procedimento armazenado gerenciado não pode ser editado por meio do Management Studio (portanto, o ícone de cadeado ao lado do nome do procedimento armazenado).


![O procedimento armazenado do GetDiscontinuedProducts está listado na pasta de procedimentos armazenados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**Figura 11**: O `GetDiscontinuedProducts` procedimento armazenado está listado na pasta de procedimentos armazenados


Ainda há mais um obstáculo temos superar antes, podemos chamar o procedimento armazenado gerenciado: o banco de dados está configurado para impedir a execução de código gerenciado. Verificar isso abrindo uma nova janela de consulta e executar o `GetDiscontinuedProducts` procedimento armazenado. Você receberá a seguinte mensagem de erro: a execução do código do usuário no .NET Framework está desabilitada. Habilite a opção de configuração de clr habilitado.

Para examinar as informações de configuração de banco de dados s Northwind, digite e execute o comando `exec sp_configure` na janela de consulta. Isso mostra que o clr habilitado configuração atualmente é definido como 0.


[![O clr habilitado configuração é definido como 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**Figura 12**: clr habilitado configuração é definido como 0 ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))


Observe que cada configuração na Figura 12 tem quatro valores listados com ele: o mínimo e valores máximos e a configuração e valores de execução. Para atualizar o valor de configuração para a configuração de clr habilitado, execute o seguinte comando:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

Se você executar novamente o `exec sp_configure` você verá que a instrução acima atualizado o valor de configuração de configuração do clr habilitado s como 1, mas o valor de execução ainda está definido como 0. Para que essa alteração de configuração tenha efeito, é preciso executar o [ `RECONFIGURE` comando](https://msdn.microsoft.com/en-us/library/ms176069.aspx), que definirá o valor de execução para o valor da configuração atual. Basta digitar `RECONFIGURE` na janela de consulta e clique no ícone executar na barra de ferramentas. Se você executar `exec sp_configure` agora você deve ver um valor de 1 para a configuração de configuração do clr habilitado s e executar os valores.

Com a configuração de clr habilitado concluída, você está pronto para executar o gerenciado `GetDiscontinuedProducts` procedimento armazenado. Na janela de consulta, digite e execute o comando `exec` `GetDiscontinuedProducts`. Chamar o procedimento armazenado faz com que o código gerenciado correspondente no `GetDiscontinuedProducts` método a ser executado. Esse código emite um `SELECT` consulta para retornar todos os produtos que são interrompidos e retorna dados para o aplicativo de chamada, que é o SQL Server Management Studio nesta instância. Management Studio recebe esses resultados e os exibe na janela de resultados.


[![O GetDiscontinuedProducts procedimento armazenado retorna todos os produtos de descontinuados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**Figura 13**: O `GetDiscontinuedProducts` armazenado procedimento retorna todos os produtos descontinuados ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>Etapa 5: Criando gerenciado procedimentos armazenados que aceitam parâmetros de entrada

Muitas das consultas e procedimentos armazenados que criamos durante esses tutoriais usaram *parâmetros*. Por exemplo, no [criando novos procedimentos armazenados para o conjunto de dados tipado de s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial criamos um procedimento armazenado denominado `GetProductsByCategoryID` que aceita um parâmetro de entrada denominado `@CategoryID`. O procedimento armazenado, em seguida, retornado todos os produtos cujo `CategoryID` campo correspondeu ao valor de fornecido `@CategoryID` parâmetro.

Para criar um procedimento armazenado gerenciado que aceita parâmetros de entrada, basta especifica esses parâmetros na definição de método s. Para ilustrar isso, vamos s adicionar outro procedimento armazenado gerenciado para o `ManagedDatabaseConstructs` projeto chamado `GetProductsWithPriceLessThan`. Esse procedimento armazenado gerenciado aceita um parâmetro de entrada especificando um preço e retornará todos os produtos cujo `UnitPrice` campo for menor que o valor do parâmetro s.

Para adicionar um novo procedimento armazenado ao projeto, clique duas vezes no `ManagedDatabaseConstructs` nome do projeto e escolha Adicionar um novo procedimento armazenado. Dê o nome `GetProductsWithPriceLessThan.vb` para o arquivo. Como vimos na etapa 3, isso criará um novo arquivo de classe do Visual Basic com um método denominado `GetProductsWithPriceLessThan` colocado dentro de `Partial` classe `StoredProcedures`.

Atualização de `GetProductsWithPriceLessThan` definição de método s para que ele aceite um [ `SqlMoney` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlmoney.aspx) parâmetro de entrada denominado `price` e escrever o código para executar e retorna os resultados da consulta:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

O `GetProductsWithPriceLessThan` definição de método s e código parecida com a definição e o código do `GetDiscontinuedProducts` método criado na etapa 3. As únicas diferenças são que o `GetProductsWithPriceLessThan` método o aceita como parâmetro de entrada (`price`), o `SqlCommand` s consulta inclui um parâmetro (`@MaxPrice`), e é adicionado um parâmetro para o `SqlCommand` s `Parameters` coleção é e recebe o valor de `price` variável.

Depois de adicionar esse código, reimplante o projeto do SQL Server. Em seguida, volte para o SQL Server Management Studio e atualize a pasta de procedimentos armazenados. Você deve ver uma nova entrada, `GetProductsWithPriceLessThan`. Em uma janela de consulta, digite e execute o comando `exec GetProductsWithPriceLessThan 25`, que será a lista de todos os produtos de menor que US $25, como mostra a Figura 14.


[![Produtos em US $25 são exibidos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**Figura 14**: produtos em US $25 são exibidas ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>Etapa 6: Chamar o procedimento armazenado gerenciado da camada de acesso a dados

Neste ponto, adicionamos a `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` gerenciados procedimentos armazenados para o `ManagedDatabaseConstructs` do projeto e registrou com o banco de dados Northwind SQL Server. Chamamos esses procedimentos armazenados gerenciados do SQL Server Management Studio também (consulte figuras 13 e 14). Para que nossos ASP.NET aplicativo usar esses gerenciado procedimentos armazenados e, em seguida, no entanto, é preciso adicioná-los para o acesso a dados e camadas de lógica de negócios na arquitetura. Nesta etapa, adicionaremos dois novos métodos para o `ProductsTableAdapter` no `NorthwindWithSprocs` conjunto de dados tipado, que foi inicialmente criado no [criando novos procedimentos armazenados para o conjunto de dados tipado de s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial. Na etapa 7 adicionaremos métodos correspondentes ao BLL.

Abra o `NorthwindWithSprocs` conjunto de dados tipado no Visual Studio e comece a adicionar um novo método para o `ProductsTableAdapter` chamado `GetDiscontinuedProducts`. Para adicionar um novo método a um TableAdapter, clique no nome do TableAdapter no Designer e escolha a opção de consulta adicionar no menu de contexto.

> [!NOTE]
> Desde que passamos o banco de dados Northwind a `App_Data` pasta para a instância de banco de dados do SQL Server 2005 Express Edition, é fundamental que a cadeia de caracteres de conexão correspondente em Web. config atualizado para refletir essa alteração. Na etapa 2 discutimos atualizando o `NORTHWNDConnectionString` valor em `Web.config`. Se você esqueceu de fazer esta atualização, você verá a mensagem de erro Falha ao adicionar consulta. Não é possível localizar a conexão `NORTHWNDConnectionString` para objeto `Web.config` em uma caixa de diálogo ao tentar adicionar um novo método para o TableAdapter. Para resolver esse erro, clique em Okey e, em seguida, vá para `Web.config` e atualize o `NORTHWNDConnectionString` valor conforme descrito na etapa 2. Em seguida, tente adicionar novamente o método no TableAdapter. Neste momento, ele deve funcionar sem erros.


Adicionando um novo método inicia o Assistente de configuração de consulta do TableAdapter que usamos muitas vezes nos últimos tutoriais. A primeira etapa nos pede para especificar como o TableAdapter deve acessar o banco de dados: por meio de uma instrução de SQL ad hoc ou um procedimento armazenado existente ou novo. Desde que já tenha criado e registrado o `GetDiscontinuedProducts` procedimento armazenado gerenciado com o banco de dados, escolha usar existente armazenado opção procedimento e pressione próximo.


[![Escolha o uso existente de opção de procedimento armazenado](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**Figura 15**: escolher usar existente armazenado procedimento opção ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))


A próxima tela solicita nos para o procedimento armazenado, que invoque o método. Escolha o `GetDiscontinuedProducts` procedimento armazenado gerenciado na lista suspensa e pressione próximo.


[![Selecione o GetDiscontinuedProducts gerenciada de procedimento armazenado](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**Figura 16**: selecione o `GetDiscontinuedProducts` gerenciados Stored Procedure ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))


Em seguida, seremos solicitados para especificar se o procedimento armazenado retorna linhas, um único valor ou nada. Como `GetDiscontinuedProducts` retorna o conjunto de linhas de produtos fora de linha, escolher a primeira opção (tabela de dados) e clique em Avançar.


[![Selecione a opção de dados de tabela](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**Figura 17**: selecione a opção de dados tabulares ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))


A tela final do assistente permite especificar os padrões de acesso de dados usados e os nomes dos métodos resultantes. Deixe as caixas de seleção marcadas e o nome de métodos `FillByDiscontinued` e `GetDiscontinuedProducts`. Clique em Concluir para concluir o assistente.


[![Nome de métodos FillByDiscontinued e GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**Figura 18**: nomear os métodos `FillByDiscontinued` e `GetDiscontinuedProducts` ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))


Repita essas etapas para criar métodos chamados `FillByPriceLessThan` e `GetProductsWithPriceLessThan` no `ProductsTableAdapter` para o `GetProductsWithPriceLessThan` procedimento armazenado gerenciado.

A Figura 19 mostra uma captura de tela do Designer de conjunto de dados após a adição de métodos para o `ProductsTableAdapter` para o `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` gerenciados procedimentos armazenados.


[![O ProductsTableAdapter inclui novos métodos adicionados nesta etapa](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**Figura 19**: O `ProductsTableAdapter` inclui novos métodos adicionados nesta etapa ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>Etapa 7: Adicionando métodos correspondentes para a camada de lógica de negócios

Agora que nós atualizamos a camada de acesso a dados para incluir os métodos para chamar os procedimentos armazenados gerenciados adicionados nas etapas 4 e 5, precisamos adicionar métodos correspondentes para a camada de lógica de negócios. Adicione os dois métodos a seguir para o `ProductsBLLWithSprocs` classe:


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

Ambos os métodos simplesmente chama o método DAL correspondente e retornar o `ProductsDataTable` instância. O `DataObjectMethodAttribute` marcação acima cada método faz com que esses métodos devem ser incluídos na lista suspensa na guia selecione do assistente ObjectDataSource s configurar fonte de dados.

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>Etapa 8: Invocar gerenciado procedimentos armazenados de camada de apresentação

Com as camadas de acesso de dados e lógica de negócios aumentados para incluir suporte para chamar o `GetDiscontinuedProducts` e `GetProductsWithPriceLessThan` gerenciados procedimentos armazenados, agora podemos exibir esses armazenados resultados de procedimentos por meio de uma página ASP.NET.

Abra o `ManagedFunctionsAndSprocs.aspx` página o `AdvancedDAL` pasta e, na caixa de ferramentas, arraste um controle GridView para o Designer. Definir o GridView s `ID` propriedade `DiscontinuedProducts` e, na marca inteligente, de associá-lo a um novo ObjectDataSource denominado `DiscontinuedProductsDataSource`. Configurar ObjectDataSource para efetuar o pull de seus dados a partir de `ProductsBLLWithSprocs` classe s `GetDiscontinuedProducts` método.


[![Configurar o ObjectDataSource para usar a classe ProductsBLLWithSprocs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**Figura 20**: configurar o ObjectDataSource para usar o `ProductsBLLWithSprocs` classe ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))


[![Escolha o método GetDiscontinuedProducts da lista suspensa na guia SELECT](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**Figura 21**: escolha o `GetDiscontinuedProducts` método na lista suspensa na guia SELECT ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))


Como essa grade será usado para exibir informações de produto apenas, definir as listas suspensas na atualização, inserção e excluir guias como (nenhum) e, em seguida, clique em Concluir.

Após concluir o assistente, o Visual Studio adiciona automaticamente um BoundField ou CheckBoxField para cada campo de dados de `ProductsDataTable`. Reserve um tempo para remover todos os campos, exceto para `ProductName` e `Discontinued`, em que ponto o GridView e marcação declarativa de s ObjectDataSource deve ser semelhante à seguinte:


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

Dedicar um tempo para exibir esta página por meio de um navegador. Quando a página for visitada, as chamadas de ObjectDataSource o `ProductsBLLWithSprocs` classe s `GetDiscontinuedProducts` método. Como vimos na etapa 7, este método chama para baixo para o s DAL `ProductsDataTable` classe s `GetDiscontinuedProducts` método, que invoca o `GetDiscontinuedProducts` procedimento armazenado. Esse procedimento armazenado é gerenciada de procedimento armazenado e executa o código criado na etapa 3, retornando os produtos descontinuados.

Os resultados retornados pelo procedimento armazenado gerenciado são empacotados em um `ProductsDataTable` pela DAL e, em seguida, retornado ao BLL, que retorna-os para a camada de apresentação onde estão associados a GridView e exibidos. Conforme o esperado, a grade lista os produtos que foram descontinuados.


[![Os produtos descontinuados estão listados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**A Figura 22**: são listados os produtos descontinuados ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))


Para obter mais prática, adicione uma caixa de texto e GridView outro para a página. Ter esse GridView exibe os produtos de menor que o valor inserido na caixa de texto chamando o `ProductsBLLWithSprocs` classe s `GetProductsWithPriceLessThan` método.

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>Etapa 9: Criar e chamar UDFs T-SQL

Funções definidas pelo usuário ou UDFs, são objetos estreitamente imitar a semântica de funções em linguagens de programação. Como uma função no Visual Basic, UDFs podem incluir um número variável de parâmetros de entrada e retornar um valor de um tipo específico. Um UDF pode retornar dados escalares - uma cadeia de caracteres, um inteiro e assim por diante - ou dados de tabela. Permitir que o s dar uma olhada rápida em ambos os tipos de UDFs, começando com um UDF que retorna um tipo de dados escalares.

O UDF a seguir calcula o valor estimado do estoque de um produto específico. Ele faz isso considerando em três parâmetros de entrada - o `UnitPrice`, `UnitsInStock`, e `Discontinued` valores para um determinado produto - e retorna um valor do tipo `money`. Ela calcula o valor estimado do estoque multiplicando o `UnitPrice` pelo `UnitsInStock`. Para itens descontinuados, esse valor é reduzido à metade.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

Depois que essa UDF foi adicionado ao banco de dados, ele pode ser encontrado com o Management Studio expandindo a pasta de programação, em seguida, funções e, em seguida, funções com valor escalar. Ele pode ser usado em um `SELECT` consulta da seguinte forma:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

Adicionei o `udf_ComputeInventoryValue` UDF no banco de dados Northwind; Figura 23 mostra a saída dos itens acima `SELECT` consultar quando visualizado no Management Studio. Observe também que o UDF é listado sob a pasta de funções com valor escalar no Pesquisador de objetos.


[![Cada produto s valores de estoque é listado](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**Figura 23**: cada produto s valores de estoque está listado ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))


UDFs também podem retornar dados tabulares. Por exemplo, podemos criar uma UDF que retorna os produtos que pertencem a uma determinada categoria:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

O `udf_GetProductsByCategoryID` UDF aceita um `@CategoryID` parâmetro de entrada e retorna os resultados do `SELECT` consulta. Depois de criado, essa UDF pode ser referenciada no `FROM` (ou `JOIN`) cláusula de um `SELECT` consulta. O exemplo a seguir retorna o `ProductID`, `ProductName`, e `CategoryID` valores para cada uma das bebidas.


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

Adicionei o `udf_GetProductsByCategoryID` UDF no banco de dados Northwind; Figura 24 mostra a saída dos itens acima `SELECT` consultar quando visualizado no Management Studio. UDFs que retornam dados tabulares podem ser encontrados na pasta de funções com valor de tabela s Pesquisador de objetos.


[![O ProductID, ProductName e CategoryID são listadas para cada bebidas](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**Figura 24**: O `ProductID`, `ProductName`, e `CategoryID` são listadas para cada Bebidas ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))


> [!NOTE]
> Para obter mais informações sobre como criar e usar UDFs, check-out [gratuito para funções definidas pelo usuário](http://www.sqlteam.com/item.asp?ItemID=1955). Confira também [vantagens e as funções de Drawbacks of User-Defined](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1).


## <a name="step-10-creating-a-managed-udf"></a>Etapa 10: Criar uma UDF gerenciada

O `udf_ComputeInventoryValue` e `udf_GetProductsByCategoryID` UDFs criados nos exemplos acima são objetos de banco de dados do T-SQL. UDFs gerenciados, que podem ser adicionados ao SQL Server 2005 também oferece suporte a `ManagedDatabaseConstructs` projeto como gerenciado procedimentos armazenados das etapas 3 e 5. Nesta etapa, permitir que s implementar o `udf_ComputeInventoryValue` UDF em código gerenciado.

Para adicionar uma UDF gerenciada para o `ManagedDatabaseConstructs` de projeto, com o botão direito no nome do projeto no Gerenciador de soluções e escolha Adicionar um novo Item. Selecione o modelo definido pelo usuário na caixa de diálogo Adicionar Novo Item e nomeie o novo arquivo UDF `udf_ComputeInventoryValue_Managed.vb`.


[![Adicionar uma UDF gerenciada novo ao projeto ManagedDatabaseConstructs](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**Figura 25**: adicionar um novo UDF gerenciados para o `ManagedDatabaseConstructs` projeto ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))


O modelo de função definida pelo usuário cria um `Partial` classe denominada `UserDefinedFunctions` com um método cujo nome é igual ao nome de arquivo s de classe (`udf_ComputeInventoryValue_Managed`, nesta instância). Esse método é decorado usando o [ `SqlFunction` atributo](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx), que indica o método como uma UDF gerenciada.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

O `udf_ComputeInventoryValue` método atualmente retorna um [ `SqlString` objeto](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlstring.aspx) e não aceita parâmetros de entrada. É necessário atualizar a definição de método para que ela aceita três parâmetros de entrada de - `UnitPrice`, `UnitsInStock`, e `Discontinued` - e retorna um `SqlMoney` objeto. A lógica para calcular o valor do estoque é idêntica de T-SQL `udf_ComputeInventoryValue` UDF.


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

Observe que os parâmetros de entrada do método s UDF dos seus tipos de SQL correspondente: `SqlMoney` para o `UnitPrice` campo, [ `SqlInt16` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlint16.aspx) para `UnitsInStock`, e [ `SqlBoolean` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlboolean.aspx) para `Discontinued`. Esses tipos de dados refletem os tipos definidos no `Products` tabela: o `UnitPrice` coluna é do tipo `money`, o `UnitsInStock` coluna do tipo `smallint`e o `Discontinued` coluna do tipo `bit`.

O código começa criando um `SqlMoney` instância denominada `inventoryValue` que é atribuído um valor de 0. O `Products` permite a tabela de banco de dados `NULL` valores no `UnitsInPrice` e `UnitsInStock` colunas. Portanto, é preciso primeiro verificar para ver se esses valores contêm `NULL` s, o que fazemos por meio de `SqlMoney` objeto s [ `IsNull` propriedade](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlmoney.isnull.aspx). Se ambos os `UnitPrice` e `UnitsInStock` contêm não`NULL` valores, em seguida, nós de computação de `inventoryValue` para ser o produto dos dois. Em seguida, se `Discontinued` for true, é dividir o valor.

> [!NOTE]
> O `SqlMoney` objeto só permite que dois `SqlMoney` instâncias para ser multiplicados juntos. Ele não permite uma `SqlMoney` instância seja multiplicado pelo número de ponto flutuante literal. Portanto, para dividir `inventoryValue` , multiplique-o por um novo `SqlMoney` instância que tem o valor 0,5.


## <a name="step-11-deploying-the-managed-udf"></a>Etapa 11: Implantando o UDF gerenciado

Agora que que o UDF gerenciada foi criado, você está pronto para implantá-lo no banco de dados Northwind. Como vimos na etapa 4, os objetos gerenciados em um projeto do SQL Server são implantados clicando duas vezes no nome do projeto no Gerenciador de soluções e escolhendo a opção de implantar no menu de contexto.

Depois de ter implantado o projeto, retorne ao SQL Server Management Studio e atualize a pasta de funções com valor escalar. Agora você deve ver duas entradas:

- `dbo.udf_ComputeInventoryValue`-T-SQL UDF criado na etapa 9, e
- `dbo.udf ComputeInventoryValue_Managed`-gerenciado UDF criado na etapa 10 que acabou de ser implantado.

Para testar este UDF gerenciado, execute a seguinte consulta no Management Studio:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

Esse comando usa o gerenciado `udf ComputeInventoryValue_Managed` UDF em vez do T-SQL `udf_ComputeInventoryValue` UDF, mas a saída é o mesmo. Consulte a Figura 23 para ver uma captura de tela da saída s UDF.

## <a name="step-12-debugging-the-managed-database-objects"></a>Etapa 12: Depuração de objetos de banco de dados gerenciados

No [depuração de procedimentos armazenados](debugging-stored-procedures-vb.md) tutorial abordamos as três opções para depuração SQL Server por meio do Visual Studio: depuração direta do banco de dados, depuração de aplicativo e depuração de um projeto do SQL Server. Gerenciado banco de dados de objetos não podem ser depurados por meio de depuração direta do banco de dados, mas podem ser depurados em um aplicativo cliente e diretamente do projeto do SQL Server. Em ordem para a depuração de trabalho, no entanto, o banco de dados do SQL Server 2005 deve permitir depuração SQL/CLR. Lembre-se de que quando é criado pela primeira vez o `ManagedDatabaseConstructs` projeto Visual Studio solicitado se quiséssemos habilitar a depuração (veja a Figura 6 na etapa 2) do SQL/CLR. Essa configuração pode ser modificada clicando no banco de dados da janela do Gerenciador de servidores.


![Verifique se o banco de dados permite a depuração SQL/CLR](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**Figura 26**: Verifique se o banco de dados permite a depuração SQL/CLR


Imagine que queremos depurar o `GetProductsWithPriceLessThan` procedimento armazenado gerenciado. Começamos definindo um ponto de interrupção no código do `GetProductsWithPriceLessThan` método.


[![Definir um ponto de interrupção no método GetProductsWithPriceLessThan](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**Figura 27**: definir um ponto de interrupção no `GetProductsWithPriceLessThan` método ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))


Permitir que o s primeiro examinar os objetos de banco de dados gerenciados do projeto do SQL Server de depuração. Como nossa solução inclui dois projetos - o `ManagedDatabaseConstructs` projeto do SQL Server junto com nosso site - para depurar a partir do projeto do SQL Server é necessário para instruir o Visual Studio para iniciar o `ManagedDatabaseConstructs` quando iniciar a depuração de projeto do SQL Server. Clique com botão direito do `ManagedDatabaseConstructs` do projeto no Gerenciador de soluções e escolha o conjunto como opção de projeto de inicialização no menu de contexto.

Quando o `ManagedDatabaseConstructs` projeto é iniciado a partir do depurador, ele executa as instruções SQL no `Test.sql` arquivo, que está localizado no `Test Scripts` pasta. Por exemplo, para testar o `GetProductsWithPriceLessThan` gerenciados do procedimento armazenado, substituir o `Test.sql` arquivo de conteúdo com a instrução a seguir, que chama o `GetProductsWithPriceLessThan` passando de procedimento armazenado gerenciado a `@CategoryID` valor 14,95:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

Depois que você tiver inserido o script acima no `Test.sql`, iniciar a depuração no menu Depurar e escolhendo iniciar depuração ou pressionando F5 ou verde reproduzir o ícone na barra de ferramentas. Isso será compilar projetos na solução, implantar os objetos de banco de dados gerenciados no banco de dados Northwind e, em seguida, execute o `Test.sql` script. Neste ponto, o ponto de interrupção será atingido e é possível percorrer o `GetProductsWithPriceLessThan` método, examine os valores dos parâmetros de entrada e assim por diante.


[![O ponto de interrupção no método GetProductsWithPriceLessThan foi atingido](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**Figura 28**: O ponto de interrupção no `GetProductsWithPriceLessThan` método foi atingido ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))


Para um objeto de banco de dados SQL a ser depurado por meio de um aplicativo cliente, é fundamental que o banco de dados ser configurado para oferecer suporte à depuração de aplicativo. Clique com botão direito no banco de dados no Gerenciador de servidores e certifique-se de que a opção de depuração de aplicativo está marcada. Além disso, é preciso configurar o aplicativo ASP.NET para integrar com o depurador do SQL e para desabilitar o pooling de conexão. Essas etapas foram discutidas em detalhes na etapa 2 do [depuração de procedimentos armazenados](debugging-stored-procedures-vb.md) tutorial.

Depois de configurar o aplicativo ASP.NET e o banco de dados, definir o site do ASP.NET como o projeto de inicialização e iniciar a depuração. Se você visita uma página que chama um dos objetos gerenciados que tem um ponto de interrupção, o aplicativo será interrompido e o controle será ativado o depurador, onde você pode percorrer o código como mostrado na Figura 28.

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>Etapa 13: Compilar e implantar gerenciados manualmente os objetos de banco de dados

Projetos do SQL Server torna mais fácil criar, compilar e implantar objetos de banco de dados gerenciados. Infelizmente, os projetos do SQL Server somente estão disponíveis nas edições Professional e sistemas de equipe do Visual Studio. Se você estiver usando o Visual Web Developer ou Standard Edition do Visual Studio e para usar objetos de banco de dados gerenciados, você precisará criar e implantá-los manualmente. Isso envolve quatro etapas:

1. Criar um arquivo que contém o código-fonte para o objeto de banco de dados gerenciados,
2. Compila o objeto em um assembly
3. Registre o assembly com o banco de dados do SQL Server 2005, e
4. Crie um objeto de banco de dados do SQL Server que aponta para o método apropriado no assembly.

Para ilustrar essas tarefas, permitem s a criar um novo gerenciado por um procedimento armazenado que retorna os produtos cujos `UnitPrice` é maior do que um valor especificado. Criar um novo arquivo em seu computador denominado `GetProductsWithPriceGreaterThan.vb` e insira o código a seguir para o arquivo (você pode usar o Visual Studio, o bloco de notas ou qualquer editor de texto para fazer isso):


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

Esse código é quase idêntico do `GetProductsWithPriceLessThan` método criado na etapa 5. As únicas diferenças são os nomes de método, o `WHERE` cláusula e o nome do parâmetro usado na consulta. Volta o `GetProductsWithPriceLessThan` método, o `WHERE` cláusula ler: `WHERE UnitPrice < @MaxPrice`. Aqui, em `GetProductsWithPriceGreaterThan`, usamos: `WHERE UnitPrice > @MinPrice` .

Agora precisamos compile essa classe em um assembly. Na linha de comando, navegue até o diretório onde você salvou o `GetProductsWithPriceGreaterThan.vb` de arquivo e usa o compilador c# (`csc.exe`) para compilar o arquivo de classe em um assembly:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

Se a pasta que contém o v `bc.exe` em não no sistema s `PATH`, você precisará referenciar totalmente seu caminho, `%WINDOWS%\Microsoft.NET\Framework\version\`, da seguinte forma:


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]


[![Compilar GetProductsWithPriceGreaterThan.vb em um Assembly](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**Figura 29**: compilar `GetProductsWithPriceGreaterThan.vb` em um Assembly ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))


O `/t` sinalizador Especifica que o arquivo de classe do Visual Basic deve ser compilado em uma DLL (em vez de um arquivo executável). O `/out` sinalizador Especifica o nome do assembly resultante.

> [!NOTE]
> Em vez de compilar o `GetProductsWithPriceGreaterThan.vb` arquivo de classe da linha de comando como alternativa, você pode usar [Visual Basic Express Edition](https://msdn.microsoft.com/vstudio/express/vb/) ou criar um projeto de biblioteca de classes separado no Visual Studio Standard Edition. S ren Jacob Lauritsen, forneceu um projeto de Visual Basic Express Edition com o código para o `GetProductsWithPriceGreaterThan` procedimento armazenado e os dois procedimentos armazenados de gerenciados e UDF criado nas etapas 3, 5 e 10. Projeto de s ren S também inclui os comandos T-SQL necessários para adicionar os objetos de banco de dados correspondente.


Com o código compilado em um assembly, você está pronto para registrar o assembly no banco de dados do SQL Server 2005. Isso pode ser realizado por meio do T-SQL, usando o comando `CREATE ASSEMBLY`, ou por meio do SQL Server Management Studio. Solte foco usando o Management Studio.

No Management Studio, expanda a pasta de programabilidade no banco de dados Northwind. Uma das subpastas é Assemblies. Para adicionar manualmente um novo Assembly no banco de dados, clique com botão direito na pasta Assemblies e escolha o novo Assembly no menu de contexto. Este modo exibe a caixa de diálogo Novo Assembly caixa (consulte a Figura 30). Clique no botão de navegação, selecione o `ManuallyCreatedDBObjects.dll` assembly é compilado apenas e, em seguida, clique em Okey para adicionar o Assembly no banco de dados. Você não verá o `ManuallyCreatedDBObjects.dll` assembly no Pesquisador de objetos.


[![Adicione o Assembly de ManuallyCreatedDBObjects.dll no banco de dados](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**Figura 30**: adicionar o `ManuallyCreatedDBObjects.dll` Assembly no banco de dados ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))


![O ManuallyCreatedDBObjects.dll está listado no Pesquisador de objetos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**Figura 31**: O `ManuallyCreatedDBObjects.dll` está listado no Pesquisador de objetos


Enquanto adicionamos o assembly no banco de dados Northwind, ainda precisamos associar um procedimento armazenado com o `GetProductsWithPriceGreaterThan` método no assembly. Para fazer isso, abra uma nova janela de consulta e execute o script a seguir:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

Isso cria um novo procedimento armazenado no banco de dados Northwind denominado `GetProductsWithPriceGreaterThan` e associa o método gerenciado `GetProductsWithPriceGreaterThan` (que é a classe `StoredProcedures`, que está no assembly `ManuallyCreatedDBObjects`).

Depois de executar o script acima, atualize a pasta de procedimentos armazenados no Pesquisador de objetos. Você deve ver uma nova entrada de procedimento armazenado - `GetProductsWithPriceGreaterThan` -que tem um ícone de cadeado ao lado dele. Para testar esse procedimento armazenado, digite e execute o script a seguir na janela de consulta:


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

Como mostra a Figura 32, o comando acima exibe informações para os produtos com um `UnitPrice` maior que r $24.95.


[![O ManuallyCreatedDBObjects.dll está listado no Pesquisador de objetos](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**A Figura 32**: O `ManuallyCreatedDBObjects.dll` está listado no Pesquisador de objetos ([clique para exibir a imagem em tamanho normal](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))


## <a name="summary"></a>Resumo

Microsoft SQL Server 2005 fornece integração com o tempo de execução do CLR (Common Language), que permite que os objetos de banco de dados a ser criado usando código gerenciado. Anteriormente, esses objetos de banco de dados só podem ser criados usando o T-SQL, mas agora, podemos criar esses objetos usando a programação de linguagens como Visual Basic .NET. Neste tutorial, criamos dois gerenciados procedimentos armazenados e uma função definida pelo usuário gerenciada.

O Visual Studio s tipo de projeto do SQL Server facilita criar, compilar e implantar objetos de banco de dados gerenciados. Além disso, ele oferece suporte avançado a depuração. No entanto, os tipos de projeto do SQL Server somente estão disponíveis nas edições Professional e sistemas de equipe do Visual Studio. Para aqueles usar o Visual Web Developer ou Standard Edition do Visual Studio, a criação, compilação e etapas de implantação deve ser executada manualmente, conforme vimos na etapa 13.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Vantagens e desvantagens de funções definidas pelo usuário](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [Criando objetos do SQL Server 2005 em código gerenciado](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Criando gatilhos usando código gerenciado no SQL Server 2005](http://www.15seconds.com/issue/041006.htm)
- [Como: Criar e executar um SQL CLR procedimento armazenado de servidor](https://msdn.microsoft.com/en-us/library/5czye81z(VS.80).aspx)
- [Como: Criar e executar uma função definida pelo usuário do CLR SQL Server](https://msdn.microsoft.com/en-us/library/w2kae45k(VS.80).aspx)
- [Como: Editar o `Test.sql` Script para executar objetos SQL](https://msdn.microsoft.com/en-us/library/ms233682(VS.80).aspx)
- [Funções definidas pelo gratuito para usuário](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Código gerenciado e o SQL Server 2005 (vídeo)](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [Referência do Transact-SQL](https://msdn.microsoft.com/en-us/library/aa299742(SQL.80).aspx)
- [Passo a passo: Criando um procedimento armazenado no código gerenciado](https://msdn.microsoft.com/en-us/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi S ren Jacob Lauritsen. Além de analisar este artigo, S ren também criou o projeto de Visual c# Express Edition incluído neste download do artigo s para compilar manualmente os objetos de banco de dados gerenciados. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](debugging-stored-procedures-vb.md)
