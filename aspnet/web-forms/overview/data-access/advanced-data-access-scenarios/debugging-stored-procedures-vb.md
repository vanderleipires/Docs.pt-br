---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: "Depuração de procedimentos armazenados (VB) | Microsoft Docs"
author: rick-anderson
description: "As edições do Visual Studio Professional e Team System permitem que você definir pontos de interrupção e a procedimentos armazenados do SQL Server, a etapa fazer depuração armazenados..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: e74d368b1f9eec2177a528a6b09c599d6a307b74
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="debugging-stored-procedures-vb"></a>Depuração de procedimentos armazenados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip) ou [baixar PDF](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> As edições do Visual Studio Professional e Team System permitem que você definir pontos de interrupção e a procedimentos armazenados do SQL Server, a etapa fazer a depuração de procedimentos armazenados mais fácil depurar o código do aplicativo. Este tutorial demonstra a depuração de banco de dados direto e depuração de procedimentos armazenados do aplicativo.


## <a name="introduction"></a>Introdução

Visual Studio fornece uma experiência de depuração. Com alguns pressionamentos de tecla ou cliques do mouse, ele s possível usar pontos de interrupção para parar a execução de um programa e examine seu fluxo de controle e estado. Junto com a depuração de código do aplicativo, o Visual Studio oferece suporte para depuração de procedimentos armazenados a partir do SQL Server. Assim como pontos de interrupção podem ser definidos dentro de uma classe de code-behind do ASP.NET ou a classe da camada de lógica de negócios, o código de forma muito eles ser colocados dentro de procedimentos armazenados.

Neste tutorial, examinaremos entrando em procedimentos armazenados do Gerenciador de servidores no Visual Studio, bem como definir pontos de interrupção são atingidos quando o procedimento armazenado é chamado do aplicativo ASP.NET em execução.

> [!NOTE]
> Infelizmente, os procedimentos armazenados só podem ser entrados em e depurados por meio de versões Professional e sistemas de equipe do Visual Studio. Se você estiver usando o Visual Web Developer ou a versão padrão do Visual Studio, você está bem-vindo ao longo de leitura percorremos as etapas necessárias para depurar procedimentos armazenados, mas você não poderá replicar essas etapas em seu computador.


## <a name="sql-server-debugging-concepts"></a>Conceitos de depuração do SQL Server

Microsoft SQL Server 2005 foi projetado para fornecer integração com o [Common Language Runtime (CLR)](https://msdn.microsoft.com/en-us/netframework/aa497266.aspx), que é o tempo de execução usado por todos os assemblies do .NET. Consequentemente, o SQL Server 2005 dá suporte a objetos de banco de dados gerenciados. Ou seja, você pode criar objetos de banco de dados como procedimentos armazenados e funções definidas pelo usuário (UDFs) como métodos em uma classe do Visual Basic. Isso permite que esses procedimentos armazenados e UDFs para utilizar a funcionalidade do .NET Framework e de suas próprias classes personalizadas. Naturalmente, o SQL Server 2005 também fornece suporte para objetos de banco de dados do T-SQL.

SQL Server 2005 oferece suporte à depuração de T-SQL e objetos de banco de dados gerenciados. No entanto, esses objetos podem ser depurados somente por meio de edições do Visual Studio 2005 Professional e sistemas de equipe. Neste tutorial, examinaremos depuração objetos de banco de dados do T-SQL. O tutorial subsequente olha para depurar objetos de banco de dados gerenciados.

O [visão geral do T-SQL e a depuração CLR no SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) entrada de blog do [equipe de integração de CLR do SQL Server 2005](https://blogs.msdn.com/sqlclr/default.aspx) destaca as três maneiras de depurar objetos do SQL Server 2005 do Visual Studio:

- **Direcionar o banco de dados de depuração (DDD)** - do Gerenciador de servidores que podem entrar em qualquer objeto de banco de dados do T-SQL, como procedimentos armazenados e UDFs. Vamos examinar DDD na etapa 1.
- **A depuração de aplicativo** -podemos definir pontos de interrupção dentro de um objeto de banco de dados e, em seguida, execute o aplicativo ASP.NET. Quando o objeto de banco de dados é executado, o ponto de interrupção será atingido e controle transformada para o depurador. Observe que a depuração de aplicativo não é possível passar em um objeto de banco de dados do código do aplicativo. Nós deve definir explicitamente os pontos de interrupção nesses procedimentos armazenados ou UDFs onde desejamos que o depurador para interromper. Depuração de aplicativo é examinado começando na etapa 2.
- **Depuração de um projeto do SQL Server** -edições do Visual Studio Professional e sistemas de equipe incluem um tipo de projeto do SQL Server que é normalmente usado para criar objetos de banco de dados gerenciados. Vamos examinar usando projetos do SQL Server e a depuração de seu conteúdo no tutorial Avançar.

O Visual Studio pode depurar procedimentos armazenados em instâncias do SQL Server locais e remotos. Uma instância local do SQL Server é aquele que está instalado no mesmo computador que o Visual Studio. Se o banco de dados do SQL Server que você está usando não estiver localizado no computador de desenvolvimento, ele será considerado uma instância remota. Para esses tutoriais estamos usando as instâncias locais do SQL Server. A depuração de procedimentos armazenados em uma instância remota do SQL server requer mais etapas de configuração que ao depurar procedimentos armazenados em uma instância local.

Se você estiver usando uma instância local do SQL Server, você pode começar com a etapa 1 e trabalhar com este tutorial até o final. Se você estiver usando uma instância remota do SQL Server, no entanto, você será necessário primeiro certifique-se de que durante a depuração, você está conectado à sua máquina de desenvolvimento com uma conta de usuário do Windows que tem um logon do SQL Server na instância remota. Moveover, esse logon de banco de dados e o logon de banco de dados usado para se conectar ao banco de dados do aplicativo ASP.NET em execução deve ser membros do `sysadmin` função. Consulte os objetos de banco de dados de depuração T-SQL na seção de instâncias remotas no final deste tutorial para obter mais informações sobre como configurar o Visual Studio e o SQL Server para uma instância remota de depuração.

Por fim, entenda que suporte à depuração para objetos de banco de dados do T-SQL não é como um recurso avançado como o suporte à depuração para aplicativos .NET. Por exemplo, condições de ponto de interrupção e filtros não são suportados, somente um subconjunto das janelas de depuração estão disponíveis, você não pode usar Editar e continuar, a janela imediata é renderizada inútil e assim por diante. Consulte [limitações sobre os recursos e comandos do depurador](https://msdn.microsoft.com/en-us/library/ms165035(VS.80).aspx) para obter mais informações.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Etapa 1: Depuração diretamente em um procedimento armazenado

O Visual Studio facilita a depurar diretamente um objeto de banco de dados. Permitem s examinar como usar o recurso de depuração de banco de dados direta (DDD) para passar para o `Products_SelectByCategoryID` procedimento armazenado no banco de dados Northwind. Como o nome sugere, `Products_SelectByCategoryID` retorna informações de produto para uma determinada categoria; ele foi criado o [usando existente procedimentos armazenados para o conjunto de dados tipado de s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial. Iniciar, vá para o Gerenciador de servidores e expanda o nó do banco de dados Northwind. Em seguida, fazer uma busca detalhada na pasta Stored Procedures, com o botão direito no `Products_SelectByCategoryID` procedimento armazenado e escolha a opção de etapa no procedimento armazenado do menu de contexto. Isso irá iniciar o depurador.

Como o `Products_SelectByCategoryID` procedimento armazenado espera um `@CategoryID` parâmetro de entrada, seremos solicitados a fornecer esse valor. Insira 1, que retorna informações sobre as bebidas.


![Use o valor 1 para o @CategoryID parâmetro](debugging-stored-procedures-vb/_static/image1.png)

**Figura 1**: usar o valor 1 para o `@CategoryID` parâmetro


Depois de fornecer o valor para o `@CategoryID` , o procedimento armazenado é executado. Em vez de em execução até a conclusão, no entanto, o depurador interromperá a execução na primeira instrução. Observe a seta amarela na margem, que indica o local atual no procedimento armazenado. Você pode exibir e editar valores de parâmetro através da janela de observação ou passando o mouse sobre o nome do parâmetro no procedimento armazenado.


[![O depurador foi interrompida na primeira instrução do procedimento armazenado](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**Figura 2**: O depurador foi interrompida na primeira instrução do procedimento armazenado ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image4.png))


Para depurar a declaração de um procedimento armazenado por vez, clique no botão Avançar na barra de ferramentas ou pressione a tecla F10. O `Products_SelectByCategoryID` procedimento armazenado contém uma única `SELECT` instrução, para atingir F10 será passe pela instrução única e concluir a execução do procedimento armazenado. Depois que o procedimento armazenado é concluído, a saída será exibida na janela de saída e o depurador será encerrado.

> [!NOTE]
> A depuração T-SQL ocorre no nível de instrução; Você não pode entrar em um `SELECT` instrução.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Etapa 2: Configurando o site para a depuração de aplicativo

Durante a depuração de um procedimento armazenado diretamente do Gerenciador de servidores úteis, em muitos cenários estão mais interessados em depuração o procedimento armazenado quando ele é chamado de nosso aplicativo ASP.NET. Podemos adicionar pontos de interrupção para um procedimento armazenado de dentro do Visual Studio e, em seguida, iniciar a depuração do aplicativo ASP.NET. Quando um procedimento armazenado com os pontos de interrupção é invocado do aplicativo, a execução será interrompida no ponto de interrupção e podemos exibir e alterar os valores de parâmetro do procedimento armazenado s e percorrer suas instruções, como na etapa 1.

Para que possamos começar a depuração de procedimentos armazenados chamados do aplicativo, é preciso instruir o aplicativo da web ASP.NET para integrar com o depurador do SQL Server. Iniciar clicando no nome do site no Gerenciador de soluções (`ASPNET_Data_Tutorial_74_VB`). Escolha a opção de páginas de propriedades no menu de contexto, selecione o item de opções de inicialização à esquerda e marque a caixa de seleção do SQL Server na seção depuradores (consulte a Figura 3).


[![Marque a caixa de seleção de servidor SQL nas páginas de propriedade de aplicativo s](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**Figura 3**: marque a caixa de seleção de servidor SQL em aplicativo s páginas de propriedades ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image7.png))


Além disso, é necessário atualizar a cadeia de caracteres de conexão de banco de dados usada pelo aplicativo para que o pool de conexão está desabilitado. Quando uma conexão para um banco de dados for fechada, correspondente `SqlConnection` objeto é colocado em um pool de conexões disponíveis. Ao estabelecer uma conexão para um banco de dados, um objeto de conexão disponíveis podem ser recuperados desse pool em vez de ter de criar e estabelecer uma nova conexão. Este pool de objetos de conexão é um aprimoramento de desempenho e é habilitado por padrão. No entanto, ao depurar você deseja desativar o pool de conexão porque a infraestrutura de depuração não é restabelecida corretamente ao trabalhar com uma conexão que foi tirada do pool.

Para o pool de conexão desabilitado, atualizar o `NORTHWNDConnectionString` na `Web.config` para que ele inclui a configuração `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> Depois de terminar de depuração do SQL Server por meio do aplicativo ASP.NET Certifique-se restabelecer o pooling de conexão, removendo o `Pooling` configuração da cadeia de conexão (ou definindo-a como `Pooling=true` ).


Neste ponto o aplicativo do ASP.NET foi configurado para permitir que o Visual Studio para depurar objetos de banco de dados do SQL Server quando invocado por meio do aplicativo web. Tudo o que resta agora é adicionar um ponto de interrupção para um procedimento armazenado e iniciar a depuração!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Etapa 3: Adicionando um ponto de interrupção e depuração

Abra o `Products_SelectByCategoryID` procedimento armazenado e defina um ponto de interrupção no início do `SELECT` instrução clicando na margem no local apropriado ou colocando o cursor no início do `SELECT` instrução e pressionando F9. Como ilustra a Figura 4, o ponto de interrupção aparece como um círculo vermelho na margem.


[![Definir um ponto de interrupção no Products_SelectByCategoryID procedimento armazenado](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**Figura 4**: definir um ponto de interrupção no `Products_SelectByCategoryID` procedimento armazenado ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image10.png))


Para um objeto de banco de dados SQL a ser depurado por meio de um aplicativo cliente, é fundamental que o banco de dados ser configurado para oferecer suporte à depuração de aplicativo. Quando você primeiro definir um ponto de interrupção, esta configuração automaticamente deve ser ativada, mas é recomendável verificar novamente. Clique com botão direito no `NORTHWND.MDF` nó no Gerenciador de servidores. O menu de contexto deve incluir um menu marcados item denominado depuração de aplicativo.


![Verifique se a opção de depuração de aplicativo está habilitada](debugging-stored-procedures-vb/_static/image11.png)

**Figura 5**: Verifique se a opção de depuração de aplicativo está habilitada


Com o conjunto de ponto de interrupção e a opção de depuração de aplicativo habilitada, você está pronto para depurar o procedimento armazenado quando chamado a partir do aplicativo ASP.NET. Iniciar o depurador indo até o menu de depuração e escolhendo iniciar depuração, pressionando F5 ou clicando o verde reproduzir o ícone na barra de ferramentas. Isso inicia o depurador e iniciar o site.

O `Products_SelectByCategoryID` procedimento armazenado foi criado na [usando existente procedimentos armazenados para o conjunto de dados tipado de s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial. A página da web correspondente (`~/AdvancedDAL/ExistingSprocs.aspx`) contém um GridView que exibe os resultados retornados por esse procedimento armazenado. Visite essa página por meio do navegador. Ao atingir a página, o ponto de interrupção a `Products_SelectByCategoryID` procedimento armazenado será atingido e o controle é retornado para o Visual Studio. Assim como na etapa 1, você pode percorrer as instruções do procedimento armazenado s e exibir e modificar os valores de parâmetro.


[![A página de ExistingSprocs.aspx exibe inicialmente as bebidas](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**Figura 6**: O `ExistingSprocs.aspx` página exibe inicialmente as bebidas ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image14.png))


[![O procedimento armazenado s ponto de interrupção foi atingido](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**Figura 7**: O procedimento armazenado s ponto de interrupção for atingido ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image17.png))


Como a janela de inspeção na Figura 7 mostra, o valor de `@CategoryID` parâmetro é 1. Isso ocorre porque o `ExistingSprocs.aspx` página inicialmente exibe os produtos na categoria de bebidas, que tem um `CategoryID` valor de 1. Escolha uma categoria diferente na lista suspensa. Isso causa um postback e executa novamente o `Products_SelectByCategoryID` procedimento armazenado. O ponto de interrupção é atingido novamente, mas desta vez o `@CategoryID` o valor do parâmetro s reflete o item selecionado na lista suspensa s `CategoryID`.


[![Escolha uma categoria diferente na lista suspensa](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**Figura 8**: escolha uma categoria diferente na lista suspensa ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image20.png))


[![O @CategoryID parâmetro reflete a categoria selecionada da página da Web](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**Figura 9**: O `@CategoryID` parâmetro reflete a categoria selecionada da página da Web ([clique para exibir a imagem em tamanho normal](debugging-stored-procedures-vb/_static/image23.png))


> [!NOTE]
> Se o ponto de interrupção a `Products_SelectByCategoryID` procedimento armazenado não é atingido quando visitar o `ExistingSprocs.aspx` página, certifique-se de que a caixa de seleção do SQL Server foi verificada na seção depuradores do aplicativo ASP.NET s página de propriedades, o pooling de conexão tiver sido desabilitado e que o banco de dados s opção de depuração de aplicativo está habilitado. Se você ainda tiver problemas, reinicie o Visual Studio e tente novamente.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Objetos de banco de dados do T-SQL em instâncias remotas de depuração

Depuração de objetos de banco de dados por meio do Visual Studio é bastante simples quando a instância de banco de dados do SQL Server estiver no mesmo computador que o Visual Studio. No entanto, se o SQL Server e Visual Studio residem em computadores diferentes alguma configuração cuidadosa é necessário para obter tudo estiver funcionando corretamente. Há duas tarefas principais que existem:

- Certifique-se de que o logon usado para se conectar ao banco de dados por meio do ADO.NET pertence a `sysadmin` função.
- Certifique-se de que a conta de usuário do Windows usada pelo Visual Studio no computador de desenvolvimento é uma conta de logon válida do SQL Server que pertence a `sysadmin` função.

A primeira etapa é relativamente simples. Primeiro, identifique a conta de usuário usada para se conectar ao banco de dados do aplicativo ASP.NET e, em seguida, no SQL Server Management Studio, adicione essa conta de logon para o `sysadmin` função.

A segunda tarefa requer que a conta de usuário do Windows que usam para depurar o aplicativo ser um logon válido no banco de dados remoto. No entanto, a probabilidade é registrado sua estação de trabalho com a conta do Windows não é um logon válido no SQL Server. Em vez de adicionar sua conta de logon específica para o SQL Server, uma opção melhor seria designar alguma conta de usuário do Windows como a conta de depuração de SQL Server. Em seguida, para depurar os objetos de banco de dados de uma instância remota do SQL Server, você executaria Visual Studio usando que credenciais de conta s de logon do Windows.

Um exemplo pode ajudar a esclarecer coisas. Imagine que há uma conta do Windows chamada `SQLDebug` dentro do domínio do Windows. Essa conta precisa ser adicionado à instância remota do SQL Server como um logon válido e um membro do `sysadmin` função. Em seguida, para depurar a instância remota do SQL Server no Visual Studio, temos que executar o Visual Studio como o `SQLDebug` usuário. Isso pode ser feito pelo log fora de nosso estação de trabalho, fazer logon novamente como `SQLDebug`, e, em seguida, iniciar o Visual Studio, mas uma abordagem mais simples seria nossa estação de trabalho usando nossas próprias credenciais de logon e, em seguida, usar `runas.exe` para iniciar o Visual Studio como o `SQLDebug` usuário. `runas.exe`permite que um aplicativo específico a ser executada na forma de uma conta de usuário diferente. Para iniciar o Visual Studio como `SQLDebug`, você poderia inserir a instrução a seguir na linha de comando:


[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

Para obter uma explicação mais detalhada sobre esse processo, consulte [William R. Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker guia para Visual Studio e SQL Server, edição sétimo* , bem como [como: definir permissões do SQL Server para depuração](https://msdn.microsoft.com/en-us/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Se sua máquina de desenvolvimento estiver executando o Windows XP Service Pack 2, você precisará configurar o Firewall de Conexão de Internet para permitir a depuração remota. [Como para: habilitar a depuração do SQL Server 2005](https://msdn.microsoft.com/en-us/library/s0fk6z6e(VS.80).aspx) artigo notas que isso envolve duas etapas: (a) no computador de host do Visual Studio, você deverá adicionar `Devenv.exe` à lista de exceções e abrir a porta TCP 135; e (b) no computador remoto (SQL), você deve abrir o TCP 135 porta e adicionar `sqlservr.exe` à lista de exceções. Se a diretiva de domínio requer comunicação de rede sejam feitas via IPSec, você deve abrir as portas UDP 4500 e UDP 500.


## <a name="summary"></a>Resumo

Além de fornecer suporte à depuração de código do aplicativo .NET, o Visual Studio também fornece uma variedade de opções de depuração para o SQL Server 2005. Neste tutorial vimos duas dessas opções: depuração direta do banco de dados e a depuração de aplicativo. Para depurar diretamente um objeto de banco de dados do T-SQL, localize o objeto por meio do Gerenciador de servidores, em seguida, clique nele e escolha entrar. Isso inicia o depurador e é interrompida na primeira instrução no objeto de banco de dados, no ponto em que você pode percorrer as instruções do objeto s e exibir e modificar valores de parâmetro. Na etapa 1, usamos essa abordagem para passar o `Products_SelectByCategoryID` procedimento armazenado.

Depuração de aplicativo permite que os pontos de interrupção a ser definida diretamente dentro dos objetos de banco de dados. Quando um objeto de banco de dados com pontos de interrupção é chamado de um aplicativo cliente (como um aplicativo web do ASP.NET), o programa é interrompida, como o depurador assume. Depuração de aplicativo é útil porque mostra mais claramente que ação de aplicativo faz com que um objeto de banco de dados específico a ser invocado. No entanto, ele exige um pouco mais a instalação e configuração de depuração direta do banco de dados.

Objetos de banco de dados também podem ser depurados por meio de projetos do SQL Server. Vamos examinar usando projetos do SQL Server e como usá-las para criar e depurar gerenciados objetos de banco de dados no tutorial Avançar.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

>[!div class="step-by-step"]
[Anterior](protecting-connection-strings-and-other-configuration-information-vb.md)
[Próximo](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)
