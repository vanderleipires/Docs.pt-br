---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: Criando uma camada de acesso a dados (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial vamos começar do começo e criar o Data Access DAL (camada), usando conjuntos de dados tipados, para acessar as informações em um banco de dados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 927b2490b5c539a79bb9939b88942499b23cc464
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
---
<a name="creating-a-data-access-layer-c"></a>Criando uma camada de acesso a dados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe) ou [baixar PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> Neste tutorial vamos começar do começo e criar o Data Access DAL (camada), usando conjuntos de dados tipados, para acessar as informações em um banco de dados.


## <a name="introduction"></a>Introdução

Como os desenvolvedores da web, nossa vida giram em torno de trabalhar com dados. Podemos criar bancos de dados para armazenar os dados, o código para recuperar e modificar e páginas da web para coletar e resumi-lo. Este é o primeiro tutorial em uma série extensa explorará técnicas para implementar esses padrões comuns no ASP.NET 2.0. Vamos começar com a criação de um [arquitetura de software](http://en.wikipedia.org/wiki/Software_architecture) composto de uma Data Access DAL (camada) usando DataSets tipados, uma camada de lógica de negócios (BLL) que impõe regras de negócio personalizadas, e as páginas de uma camada de apresentação composta do ASP.NET que Compartilhe um layout de página comuns. Depois que essa base de back-end foi apresentado, passaremos para emissão de relatórios, mostrando como exibir, resumir, coletar e validar os dados de um aplicativo web. Esses tutoriais são orientados para ser conciso e fornecem instruções passo a passo com capturas de tela suficiente para orientá-lo pelo processo visualmente. Cada tutorial está disponível em c# e Visual Basic versões e inclui um download do código completo usado. (Esse tutorial primeiro é muito longo, mas o restante são apresentados em partes amostras muito mais.)

Para esses tutoriais usaremos uma versão do Microsoft SQL Server 2005 Express Edition do banco de dados Northwind, colocado no **aplicativo\_dados** directory. Além do arquivo de banco de dados, o **aplicativo\_dados** pasta também contém os scripts SQL para criar o banco de dados, caso você deseje usar uma versão diferente do banco de dados. Esses scripts podem também ser [baixado diretamente da Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), se você preferir. Se você usar uma versão diferente do SQL Server do banco de dados Northwind, você precisará atualizar o **NORTHWNDConnectionString** configuração do aplicativo **Web. config** arquivo. O aplicativo web foi criado usando o Visual Studio 2005 Professional Edition como um projeto de site da Web baseado no sistema de arquivos. No entanto, todos os tutoriais funcionará igualmente bem com a versão gratuita do Visual Studio 2005, [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
Neste tutorial vamos começar do começo e criar o Data Access DAL (camada), seguido por criar os negócios lógica BLL (camada) no tutorial de segundo e trabalhar no layout de página e navegação no terceiro. Os tutoriais depois criará um terceiro com base apresentado nas três primeiras. Temos muitos abrangem neste tutorial primeiro, portanto, inicie o Visual Studio e vamos começar!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Etapa 1: Criar um projeto da Web e conectar-se ao banco de dados

Para que possa criar nossa camada de acesso de dados (DAL), é preciso primeiro criar um site da web e nosso banco de dados de configuração. Comece criando um novo arquivo com base em sistema site ASP.NET. Para fazer isso, vá para o menu Arquivo e escolha o novo Site da Web, exibindo a caixa de diálogo do novo Site da Web. Escolher o modelo de Site da Web ASP.NET, defina a lista suspensa do local do sistema de arquivos, escolha uma pasta para colocar o site da web e definir a linguagem c#.


[![Criar um novo arquivo com base em sistema Site da Web](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Figura 1**: criar um Site New File System-Based ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image3.png))


Isso criará um novo site com um **Default.aspx** página ASP.NET e um **aplicativo\_dados** pasta.

Com o site criado, a próxima etapa é adicionar uma referência ao banco de dados no Gerenciador de servidores do Visual Studio. Adicionando um banco de dados para o Gerenciador de servidores, você pode adicionar tabelas, procedimentos armazenados, exibições e assim por diante todos de dentro do Visual Studio. Você também pode exibir dados de tabelas ou criar suas próprias consultas manual ou graficamente via o construtor de consultas. Além disso, quando criamos os conjuntos de dados digitado para a DAL precisaremos ponto Visual Studio para o banco de dados do qual os conjuntos de dados tipado deve ser construídos. Enquanto estamos podem fornecer essas informações de conexão no momento, o Visual Studio preenche automaticamente uma lista suspensa dos bancos de dados já está registrado no Gerenciador de servidores.

As etapas para adicionar o banco de dados Northwind para o Gerenciador de servidores dependem se você deseja usar o banco de dados do SQL Server 2005 Express Edition no **aplicativo\_dados** pasta ou se você tiver um Microsoft SQL Server 2000 ou 2005 instalação de servidor de banco de dados que você deseja usar em vez disso.

## <a name="using-a-database-in-theappdatafolder"></a>Usando um banco de dados em theApp\_DataFolder

Se você não tem um SQL Server 2000 ou 2005 servidor de banco de dados para se conectar ao ou simplesmente desejar evitar ter que adicionar o banco de dados a um servidor de banco de dados, você pode usar a versão do SQL Server 2005 Express Edition do banco de dados Northwind está localizado no websit baixado e's **aplicativo\_dados** pasta (**NORTHWND. MDF**).

Um banco de dados é colocado no **aplicativo\_dados** pasta é adicionada automaticamente para o Gerenciador de servidores. Supondo que você tenha o SQL Server 2005 Express Edition instalado em seu computador, você deverá ver um nó denominado NORTHWND. MDF no Gerenciador de servidores, que você pode expandir e explorar suas tabelas, exibições, procedimento armazenado e assim por diante (consulte a Figura 2).

O **aplicativo\_dados** pasta também pode manter o Microsoft Access **. mdb** arquivos, que, assim como suas contrapartes do SQL Server, são adicionados automaticamente para o Gerenciador de servidores. Se você não quiser usar qualquer uma das opções do SQL Server, você pode sempre [baixar uma versão do Microsoft Access do arquivo de banco de dados Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) e solte no **aplicativo\_dados** directory. Lembre-se, no entanto, que não são bancos de dados do Access como muitos recursos que o SQL Server e não são projetados para serem usados em cenários de site da web. Além disso, alguns dos tutoriais do 35 + utilizará determinados recursos de nível de banco de dados que não são suportados pelo Access.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Conectar-se ao banco de dados em um servidor de banco de dados do Microsoft SQL Server 2000 ou 2005

Como alternativa, você pode se conectar a um banco de dados Northwind instalado em um servidor de banco de dados. Se o servidor de banco de dados ainda não tiver o banco de dados Northwind instalado, você primeiro deve adicioná-lo ao servidor de banco de dados executando o script de instalação incluído no download deste tutorial ou pelo [baixar a versão do SQL Server 2000 do Northwind script de instalação e](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) diretamente do site da Microsoft.

Uma vez que o banco de dados instalado, vá para o Gerenciador de servidores no Visual Studio, clique com botão direito no nó de conexões de dados e escolha Adicionar Conexão. Se você não vir o Gerenciador de servidores, acesse o modo de exibição / Gerenciador de servidores ou ocorrências Ctrl + Alt + S. Isso abrirá a caixa de diálogo Adicionar Conexão, onde você pode especificar o servidor para se conectar ao, as informações de autenticação e o nome do banco de dados. Depois de ter configurado as informações de conexão de banco de dados e clicar no botão Okey com êxito, o banco de dados será adicionado como um nó sob o nó de conexões de dados. Você pode expandir o nó de banco de dados para explorar suas tabelas, exibições, procedimentos armazenados e assim por diante.


![Adicionar uma Conexão ao banco de dados do seu servidor de banco de dados Northwind](creating-a-data-access-layer-cs/_static/image4.png)

**Figura 2**: adicionar uma Conexão ao banco de dados do seu servidor de banco de dados Northwind


## <a name="step-2-creating-the-data-access-layer"></a>Etapa 2: Criar a camada de acesso a dados

Ao trabalhar com dados de uma opção é inserir a lógica de específicos de dados diretamente para a camada de apresentação (em um aplicativo web, o ASP.NET páginas, compõem a camada de apresentação). Isso pode assumir a forma de escrever código ADO.NET na parte do código da página ASP.NET ou usando o controle SqlDataSource da parte de marcação. Em ambos os casos, essa abordagem acoplada rigidamente a lógica de acesso a dados com a camada de apresentação. No entanto, é a abordagem recomendada separar a lógica de acesso a dados da camada de apresentação. Essa camada separada é chamada de camada de acesso a dados, a DAL de forma abreviada e geralmente é implementada como um projeto de biblioteca de classes separado. Os benefícios dessa arquitetura em camadas são bem documentados (consulte a seção "Leituras adicionais" no final deste tutorial para obter informações sobre estas vantagens) e é a abordagem obtemos nesta série.

Todo o código que é específico à fonte de dados subjacente, como a criação de uma conexão ao banco de dados, emitir **selecione**, **inserir**, **atualização**, e  **Excluir** comandos e assim por diante devem estar localizado em DAL. A camada de apresentação não deve conter todas as referências a esse código de acesso de dados, mas em vez disso, deve fazer chamadas para a DAL para solicitações de todos os dados. Camadas de acesso de dados normalmente contêm métodos para acessar o banco de dados subjacente. Por exemplo, tem um banco de dados Northwind, **produtos** e **categorias** tabelas que registram os produtos de venda e as categorias às quais eles pertencem. Em nosso DAL teremos métodos, como:

- **GetCategories(),** que retornará informações sobre todas as categorias
- **GetProducts()**, que retorna informações sobre todos os produtos
- **GetProductsByCategoryID (*categoryID*)**, que retorna todos os produtos que pertencem a uma categoria especificada
- **GetProductByProductID (*productID*)**, que retorna informações sobre um produto específico

Esses métodos, quando chamado, conectar-se ao banco de dados, emita a consulta apropriada e retornar os resultados. É importante como retornamos esses resultados. Esses métodos podem simplesmente retornar um DataSet ou DataReader preenchida pela consulta de banco de dados, mas idealmente esses resultados devem ser retornados usando *objetos fortemente tipados*. Um objeto fortemente tipado é um cujo esquema rigidamente é definida em tempo de compilação, enquanto o oposto, um objeto tipadas vagamente, é uma cujo esquema não é conhecida até o tempo de execução.

Por exemplo, o DataReader e o conjunto de dados (por padrão) são objetos tipadas vagamente porque seu esquema é definido pelas colunas retornadas pela consulta de banco de dados usada para preenchê-los. Para acessar uma coluna específica de um DataTable tipadas vagamente que precisamos usar sintaxe semelhante: ***DataTable*.Linhas[*índice*]["*columnName*"]**. Tipagem flexível a DataTable neste exemplo é apresentado pelo fato de que precisamos para acessar o nome da coluna usando uma cadeia de caracteres ou o índice ordinal. Uma DataTable fortemente tipado, por outro lado, terá cada uma de suas colunas implementadas como propriedades, resultando em código que se parece com: ***DataTable*.Linhas[*índice*].*columnName***\*.

Para retornar objetos fortemente tipados, os desenvolvedores podem criar seus próprios objetos de negócios personalizada ou usar conjuntos de dados digitados. Um objeto de negócios é implementado pelo desenvolvedor como representa uma classe cujas propriedades normalmente refletem as colunas da tabela de banco de dados subjacentes do objeto comercial. Um conjunto de dados tipado é uma classe gerada para você pelo Visual Studio com base em um esquema de banco de dados e cujos membros são fortemente tipada de acordo com esse esquema. O conjunto de dados tipado próprio consiste em classes que estendem as classes de conjunto de dados ADO.NET, DataTable e DataRow. Além das tabelas de dados fortemente tipados, DataSets tipados agora também incluem TableAdapters, que são classes com métodos para popular tabelas de dados do conjunto de dados e propagar as modificações dentro de tabelas de dados no banco de dados.

> [!NOTE]
> Para obter mais informações sobre as vantagens e desvantagens do uso de DataSets tipados versus objetos comerciais personalizados, consulte [Criando componentes de camada de dados e passando por meio de camadas de dados](https://msdn.microsoft.com/library/ms978496.aspx).


Vamos usar conjuntos de dados fortemente tipado para arquitetura esses tutoriais. A Figura 3 ilustra o fluxo de trabalho entre as diferentes camadas de um aplicativo que usa conjuntos de dados digitados.


[![Código de acesso a todos os dados é relegados a DAL](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Figura 3**: código de acesso a todos os dados é relegados a DAL ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Criação de um conjunto de dados tipado e o adaptador de tabela

Para começar a criar nossa DAL, vamos começar com a adição de um conjunto de dados tipado ao nosso projeto. Para fazer isso, clique com botão direito no nó do projeto no Gerenciador de soluções e escolha Adicionar um novo Item. Selecione a opção de conjunto de dados na lista de modelos e nomeie- **Northwind.xsd**.


[![Adicionar um novo conjunto de dados ao seu projeto](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Figura 4**: adicionar um novo conjunto de dados ao seu projeto ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image10.png))


Depois de clicar em Adicionar, quando solicitado a adicionar o conjunto de dados para o **aplicativo\_código** pasta, escolha ' Sim '. O Designer para o conjunto de dados tipado será exibido e iniciará o Assistente de configuração do TableAdapter, permitindo que você adicione o TableAdapter primeiro para o conjunto de dados tipado.

Um conjunto de dados tipado serve como uma coleção fortemente tipada de dados. ele é composto de instâncias de DataTable fortemente tipada, cada um por sua vez é composta de instâncias de DataRow fortemente tipada. Vamos criar uma tabela de dados fortemente tipado para cada uma das tabelas de banco de dados subjacente que precisamos para trabalhar com esta série de tutoriais. Vamos começar com a criação de um DataTable para o **produtos** tabela.

Tenha em mente que DataTables fortemente tipadas não incluem todas as informações sobre como acessar dados da tabela de banco de dados subjacente. Para recuperar os dados para preencher a DataTable, usamos uma classe do TableAdapter, que funciona como nossa camada de acesso a dados. Para nosso **produtos** DataTable, o TableAdapter conterá os métodos **GetProducts()**, **GetProductByCategoryID (*categoryID*)**, e assim por diante que vai invocamos da camada de apresentação. Função da DataTable é servir como os usado para passar dados entre as camadas de objetos fortemente tipados.

O Assistente de configuração do TableAdapter começa solicitando que você selecione qual banco de dados para trabalhar com. A lista suspensa mostra esses bancos de dados no Gerenciador de servidores. Se você não adicionou o banco de dados Northwind para o Gerenciador de servidores, você pode clicar no botão Nova Conexão a nesse momento para fazer isso.


[![Escolha o banco de dados Northwind na lista suspensa](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Figura 5**: escolha o banco de dados Northwind na lista suspensa ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image13.png))


Após selecionar o banco de dados e clicar em Avançar, será perguntado se você deseja salvar a cadeia de conexão do **Web. config** arquivo. Salvando a cadeia de caracteres de conexão, você evitará tendo rígido codificados em classes de TableAdapter, que simplifica as coisas se as informações de cadeia de caracteres de conexão é alterado no futuro. Se você optar por salvar a cadeia de caracteres de conexão no arquivo de configuração é colocado no **&lt;connectionStrings&gt;** seção, que pode ser [opcionalmente criptografado](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) para aprimorado segurança ou modificados depois dela usando a nova página de propriedade do ASP.NET 2.0 dentro da ferramenta de administração de GUI IIS, que é mais ideal para administradores.


[![Salvar a cadeia de caracteres de Conexão em Web. config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Figura 6**: salvar a cadeia de Conexão para **Web. config** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image16.png))


Em seguida, precisamos definir o esquema para a primeira DataTable fortemente tipada e fornecer o primeiro método de nosso TableAdapter usar ao preencher o conjunto de dados fortemente tipados. Essas duas etapas são executadas simultaneamente, criando uma consulta que retorna as colunas da tabela que queremos refletidos em nossa tabela de dados. No final do assistente vamos dar um nome de método para esta consulta. Depois que isso for feito, esse método pode ser chamado do nosso camada de apresentação. O método irá executar a consulta definida e preencher uma DataTable fortemente tipada.

Para começar a definir a consulta SQL primeiro deve indicar que desejamos TableAdapter para emitir a consulta. Podemos pode usar uma instrução de SQL ad hoc, crie um novo procedimento armazenado ou usar um procedimento armazenado existente. Para esses tutoriais vamos usar instruções SQL ad hoc. Consulte [Brian Noyes](http://briannoyes.net/)do artigo, [criar uma camada de acesso de dados com o Designer de conjunto de dados do Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) para obter um exemplo de como usar procedimentos armazenados.


[![Consultar os dados usando uma instrução de SQL Ad Hoc](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Figura 7**: consultar os dados usando uma instrução de SQL Ad Hoc ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image19.png))


Agora podemos pode digitar na consulta SQL manualmente. Ao criar o primeiro método no TableAdapter normalmente você deseja que a consulta retornar as colunas que precisam ser expressos em DataTable correspondente. Podemos pode fazer isso criando uma consulta que retorna todas as colunas e todas as linhas do **produtos** tabela:


[![Insira a consulta SQL na caixa de texto](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Figura 8**: insira o SQL consulta para a caixa de texto ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image22.png))


Como alternativa, use o construtor de consultas e graficamente construir a consulta, conforme mostrado na Figura 9.


[![Criar a consulta graficamente, por meio do Editor de consulta](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Figura 9**: criar a consulta graficamente, por meio do Editor de consulta ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image25.png))


Depois de criar a consulta, mas antes de passar para a próxima tela, clique no botão Opções avançadas. Em projetos de Site da Web, o "Gerar Insert, Update e Delete instruções" é o único avançado opção selecionada por padrão. Se você executar esse assistente em uma biblioteca de classe ou um projeto do Windows também será selecionada a opção "Usar simultaneidade otimista". Deixe a opção "Usar simultaneidade otimista" desmarcada por enquanto. Vamos examinar a simultaneidade otimista em tutoriais futuros.


[![Selecione apenas as Generate Insert, Update e Delete instruções opção](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Figura 10**: somente os Generate Insert, Update e Delete instruções Select opção ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image28.png))


Depois de verificar as opções avançadas, clique em Avançar para ir para a tela final. Aqui, serão solicitados a selecionar quais métodos para adicionar ao TableAdapter. Há dois padrões de preenchimento de dados:

- **Preencher uma DataTable** com essa abordagem é criado um método que usa uma tabela de dados como um parâmetro e o preenche com base nos resultados da consulta. A classe DataAdapter do ADO.NET, por exemplo, implementa esse padrão com suas **Fill ()** método.
- **Retornar uma DataTable** com essa abordagem o método cria e preenche DataTable para você e o retorna como os métodos retornam um valor.

Você pode ter o TableAdapter implementar uma ou ambas desses padrões. Você também pode renomear os métodos fornecidos aqui. Vamos deixar ambas as caixas de seleção estiver marcadas, mesmo que apenas usaremos o último padrão durante esses tutoriais. Além disso, vamos renomear em vez disso, genérico **GetData** método **GetProducts**.

Se marcada, a caixa de seleção final, "GenerateDBDirectMethods" cria **Insert ()**, **Update ()**, e **Delete ()** métodos para o TableAdapter. Se você deixar essa opção está desmarcada, todas as atualizações precisará ser feito por meio de único do TableAdapter **Update ()** método, que obtém o conjunto de dados tipado, uma tabela de dados, um DataRow único ou uma matriz de DataRows. (Caso você tenha desmarcado as "Gerar Insert, Update e Delete instruções" opção das propriedades avançadas na Figura 9, essa caixa de seleção configuração não terá efeito.) Vamos deixar essa caixa de seleção marcada.


[![Alterar o nome do método de GetData para GetProducts](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Figura 11**: alterar o nome do método de **GetData** para **GetProducts** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image31.png))


Conclua o assistente clicando em Concluir. Depois de fechar o assistente, são retornados para o Designer de conjunto de dados que mostra a DataTable que acabamos de criar. Você pode ver a lista de colunas no **produtos** DataTable (**ProductID**, **ProductName**, e assim por diante), bem como os métodos do  **ProductsTableAdapter** (**Fill ()** e **GetProducts()**).


[![A DataTable de produtos e ProductsTableAdapter foram adicionados ao conjunto de dados tipado](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Figura 12**: O **produtos** DataTable e **ProductsTableAdapter** foram adicionados ao conjunto de dados tipado ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image34.png))


Neste ponto, temos um conjunto de dados tipado com uma única tabela de dados (**Northwind.Products**) e uma classe de DataAdapter fortemente tipado (**NorthwindTableAdapters.ProductsTableAdapter**) com um  **GetProducts()** método. Esses objetos podem ser usados para acessar uma lista de todos os produtos de código como:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Esse código não exige a escrever um pouco de código específico de acesso a dados. Não tínhamos instanciar as classes ADO.NET, não tinham para se referir a qualquer cadeia de caracteres de conexão, consultas SQL ou procedimentos armazenados. Em vez disso, o TableAdapter fornece o código de acesso a dados de baixo nível para nós.

Cada objeto usado neste exemplo também é fortemente tipada, permitindo que o Visual Studio fornecer o IntelliSense e a verificação de tipo de tempo de compilação. E o melhor de todos os as tabelas de dados retornadas pelo TableAdapter pode ser associados a dados ASP.NET controles da Web, como o GridView, DetailsView, DropDownList, CheckBoxList e vários outros. O exemplo a seguir ilustra a associação DataTable retornada pelo **GetProducts()** método um GridView em apenas um exame três linhas de código dentro do **página\_carga** manipulador de eventos.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![A lista de produtos é exibida em um GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Figura 13**: A lista de produtos é exibida em um controle GridView ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image37.png))


Enquanto este exemplo necessário escrevemos três linhas de código em nossa página de ASP.NET **página\_carga** manipulador de eventos, no futuro tutoriais, examinaremos como usar o ObjectDataSource declarativamente recuperar os dados a partir de DAL. Com o ObjectDataSource é não terá que escrever nenhum código e obterá suporte de paginação e classificação também!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Etapa 3: Adicionando com parâmetros de métodos para a camada de acesso a dados

Neste ponto nosso **ProductsTableAdapter** classe tem mas um método, **GetProducts()**, que retorna todos os produtos no banco de dados. Enquanto a capacidade trabalhar com todos os produtos é certamente útil, há tempos quando queremos recuperar informações sobre um produto específico ou todos os produtos que pertencem a uma categoria específica. Para adicionar funcionalidade a nossa camada de acesso a dados, podemos adicionar métodos com parâmetros para o TableAdapter.

Vamos adicionar o **GetProductsByCategoryID (*categoryID*)** método. Para adicionar um novo método para a DAL, retorne ao Designer de conjunto de dados, clique com botão direito no **ProductsTableAdapter** seção e escolha Adicionar consulta.


![Clique com botão direito em TableAdapter e escolha Adicionar consulta](creating-a-data-access-layer-cs/_static/image38.png)

**Figura 14**: com o botão direito em TableAdapter e escolha Adicionar consulta


Vamos primeiro serão perguntados se queremos acessar o banco de dados usando uma instrução de SQL ad hoc ou um procedimento armazenado existente ou novo. Vamos optar por usar uma instrução de SQL ad hoc novamente. Em seguida, são perguntados que tipo de consulta SQL deseja usar. Como queremos retornar todos os produtos que pertencem a uma categoria especificada, queremos gravar um **selecione** instrução que retorna linhas.


[![Escolher criar uma instrução SELECT que retorna linhas](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Figura 15**: optar por criar um **selecione** instrução que retorna linhas ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image41.png))


A próxima etapa é definir a consulta SQL usada para acessar os dados. Como queremos retornar somente os produtos que pertencem a uma determinada categoria, posso usar o mesmo **selecione** instrução do **GetProducts()**, mas adicione o seguinte **onde** cláusula: **onde CategoryID = @CategoryID** . O **@CategoryID** parâmetro indica ao Assistente do TableAdapter que o método que estamos criando exigirá um parâmetro de entrada do tipo correspondente (ou seja, um inteiro anulável).


[![Insira uma consulta para retornar somente os produtos em uma categoria especificada](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Figura 16**: insira uma consulta para apenas produtos de retorno em uma categoria especificada ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image44.png))


Na etapa final que podemos escolher qual padrões para usar, bem como personalizar os nomes dos métodos gerados de acesso a dados. Para o padrão de preenchimento, vamos alterar o nome para **FillByCategoryID** e para retornar uma DataTable retornar padrão (o **obter * X*** métodos), vamos usar **GetProductsByCategoryID**.


[![Escolha os nomes para os métodos TableAdapter](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Figura 17**: escolha os nomes para os métodos TableAdapter ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image47.png))


Depois de concluir o assistente, o Designer de conjunto de dados inclui os novos métodos do TableAdapter.


![Os produtos podem agora ser consultados por categoria](creating-a-data-access-layer-cs/_static/image48.png)

**Figura 18**: os produtos podem agora ser consultados por categoria


Reserve um tempo para adicionar um **GetProductByProductID (*productID*)** método usando a mesma técnica.

Essas consultas com parâmetros podem ser testadas diretamente no Designer de conjunto de dados. Com o botão direito no método em TableAdapter e escolha a visualização de dados. Em seguida, digite os valores para usar para os parâmetros e clique em Visualizar.


[![São mostrados os pertencentes de produtos para a categoria de bebidas](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Figura 19**: os produtos que pertencem à categoria de bebidas são mostrados ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image51.png))


Com o **GetProductsByCategoryID (*categoryID*)** método nosso DAL, agora podemos criar uma página ASP.NET que exibe apenas os produtos em uma categoria especificada. O exemplo a seguir mostra todos os produtos na categoria de bebidas, que têm um **CategoryID** de 1.

Beverages.asp

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![Os produtos na categoria de bebidas são exibidos](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Figura 20**: os produtos na categoria de bebidas são exibidos ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Etapa 4: Inserir, atualizar e excluir dados

Há dois padrões usados para inserir, atualizar e excluir dados. O padrão primeiro, o que vou chamá o padrão de banco de dados direta, envolve a criação de métodos que, quando chamado, problema um **inserir**, **atualização**, ou **excluir** comando para o banco de dados que opera em um registro único banco de dados. Esses métodos normalmente são passados em uma série de valores escalares (inteiros, cadeias de caracteres, boolianos, DateTimes e assim por diante) que correspondem aos valores para inserir, atualizar ou excluir. Por exemplo, com esse padrão para o **produtos** tabela levaria o método delete em um parâmetro de número inteiro indicando o **ProductID** do registro a excluir, enquanto o método de inserção levaria um cadeia de caracteres para o **ProductName**, um decimal para o **UnitPrice**, um inteiro para o **UnitsOnStock**, e assim por diante.


[![Cada inserção, atualização e Excluir solicitação é enviada para o banco de dados imediatamente](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Figura 21**: cada inserção, atualização e Excluir solicitação é enviada para o banco de dados imediatamente ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image57.png))


Padrão que será chamado de como o lote de atualização padrão, é para atualizar um conjunto de dados, DataTable ou coleção de DataRows na chamada de um método inteira. Com esse padrão de um desenvolvedor exclui, insere, modifica DataRows em uma DataTable e, em seguida, passa esses DataRows ou DataTable em um método de atualização. Esse método, em seguida, enumera o DataRows passado, determina se ou não eles já foram modificados, adicionados ou excluídos (por meio de DataRow [propriedade RowState](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) valor) e emite a solicitação de banco de dados apropriado para cada registro.


[![Todas as alterações são sincronizadas com o banco de dados quando o método de atualização é invocado](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**A Figura 22**: todas as alterações são sincronizadas com o banco de dados quando o método de atualização é invocado ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image60.png))


O TableAdapter usa o padrão de atualização em lotes por padrão, mas também suporta o padrão de direta do banco de dados. Já que é selecionado a opção "Gerar Insert, Update e Delete instruções" das propriedades avançadas ao criar nossa TableAdapter o **ProductsTableAdapter** contém um **Update ()** método, que implementa o padrão de atualização em lotes. Especificamente, o TableAdapter contém um **Update ()** método que pode ser passado do conjunto de dados tipado, uma DataTable fortemente tipado ou DataRows um ou mais. Se você deixar a caixa de seleção "GenerateDBDirectMethods" check ao criar pela primeira vez o TableAdapter o padrão direto do banco de dados será também implementada por meio de **Insert ()**, **Update ()**, e **Delete)**  métodos.

Ambos os padrões de modificação de dados usam o TableAdapter **InsertCommand**, **UpdateCommand**, e **DeleteCommand** propriedades para emitir seus **inserir** , **Atualização**, e **excluir** comandos no banco de dados. Você pode inspecionar e modificar o **InsertCommand**, **UpdateCommand**, e **DeleteCommand** propriedades clicando no TableAdapter no Designer de conjunto de dados e, em seguida, vai Na janela Propriedades. (Verifique se você selecionou o TableAdapter e que o **ProductsTableAdapter** objeto é selecionado na lista suspensa na janela Propriedades.)


[![O TableAdapter tem InsertCommand, UpdateCommand e DeleteCommand propriedades](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Figura 23**: tem o TableAdapter **InsertCommand**, **UpdateCommand**, e **DeleteCommand** propriedades ([clique para exibir em tamanho normal imagem](creating-a-data-access-layer-cs/_static/image63.png))


Para examinar ou modificar qualquer uma dessas propriedades de comando de banco de dados, clique no **CommandText** subpropriedade, o que abrirá o construtor de consultas.


[![Configurar instruções DELETE, INSERT e UPDATE no construtor de consultas](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Figura 24**: configurar o **inserir**, **atualização**, e **excluir** instruções no construtor de consultas ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image66.png))


O exemplo de código a seguir mostra como usar o padrão de atualização em lotes para dobrar o preço de todos os produtos que não são interrompidas e que tem 25 unidades em estoque ou menos:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

O código a seguir ilustra como usar o padrão de direta do banco de dados para programaticamente excluir um produto específico e, em seguida, atualizar um e, em seguida, adicione um novo:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Criando personalizado inserir, atualizar e excluir métodos

O **Insert ()**, **Update ()**, e **Delete ()** métodos criados pelo método direto de banco de dados podem ser um pouco complexo, especialmente para tabelas com muitas colunas. Olhando para o exemplo de código anterior, sem IntelliSense ajuda não está particularmente claro qual **produtos** mapeia a coluna de tabela para cada parâmetro de entrada para o **Update ()** e **Insert)**  métodos. Pode haver momentos em que estamos apenas deseja atualizar uma única coluna ou dois, se preferir, um personalizado **Insert ()** método que será, talvez, retornar o valor do registro recém-inserido **identidade** (incremento automático) campo.

Para criar esse método um personalizado, retorne para o Designer de conjunto de dados. Com o botão direito em TableAdapter e escolha Adicionar consulta, retornando ao Assistente do TableAdapter. Na segunda tela, pode indicar o tipo de consulta para criar. Vamos criar um método que adiciona um novo produto e, em seguida, retorna o valor do registro recém-adicionado **ProductID**. Portanto, optar por criar um **inserir** consulta.


[![Criar um método para adicionar uma nova linha à tabela produtos](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Figura 25**: criar um método para adicionar uma nova linha para o **produtos** tabela ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image69.png))


Na próxima tela de **InsertCommand**do **CommandText** é exibida. Aumentar essa consulta adicionando **Selecionar escopo\_IDENTITY()** no final da consulta, que retorna o último valor de identidade inserido em uma **identidade** coluna no mesmo escopo. (Consulte o [documentação técnica](https://msdn.microsoft.com/library/ms190315.aspx) para obter mais informações sobre **escopo\_IDENTITY()** e por quê você provavelmente desejará [usar escopo\_IDENTITY() lieu de @ @IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Certifique-se de que você encerrar o **inserir** instrução com um ponto e vírgula antes de adicionar o **selecione** instrução.


[![Ampliar a consulta para retornar o valor SCOPE_IDENTITY)](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Figura 26**: ampliar a consulta para retornar o **escopo\_IDENTITY()** valor ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image72.png))


Por fim, nomeie o novo método **InsertProduct**.


[![Defina o nome do novo método para InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Figura 27**: definir o novo nome do método **InsertProduct** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image75.png))


Ao retornar para o Designer de conjunto de dados você verá que o **ProductsTableAdapter** contém um novo método, **InsertProduct**. Se esse novo método não tem um parâmetro para cada coluna a **produtos** tabela, a probabilidade é você esqueceu de finalizar o **inserir** instrução com um ponto e vírgula. Configurar o **InsertProduct** método e certifique-se de que você tem um ponto e vírgula de delimitação de **inserir** e **selecione** instruções.

Por padrão, insert métodos sem consulta de problema de métodos, que significa que elas retornam o número de linhas afetadas. No entanto, queremos o **InsertProduct** método para retornar o valor retornado pela consulta, não o número de linhas afetadas. Para fazer isso, ajustar o **InsertProduct** do método **ExecuteMode** propriedade **escalar**.


[![Altere a propriedade ExecuteMode para escalar](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Figura 28**: alteração de **ExecuteMode** propriedade **escalar** ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image78.png))


O código a seguir mostra essa nova **InsertProduct** método em ação:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Etapa 5: Concluir a camada de acesso a dados

Observe que o **ProductsTableAdapters** classe retorna o **CategoryID** e **SupplierID** os valores do **produtos** tabela, mas não inclui o **CategoryName** coluna a partir o **categorias** tabela ou o **CompanyName** coluna a partir o **fornecedores**tabela, embora provavelmente são as colunas que desejamos exibir durante a exibição de informações do produto. É possível aumentar o método do TableAdapter inicial, **GetProducts()**, para incluir o **CategoryName** e **CompanyName** valores de coluna, que atualizarão o DataTable fortemente tipado para incluir as novas colunas.

Isso pode representar um problema, no entanto, como métodos do TableAdapter para inserir, atualizar, e excluir dados baseiam-se desse método inicial. Felizmente, os métodos gerados automaticamente para inserir, atualizar e excluir não são afetados por subconsultas no **selecione** cláusula. Tomando cuidado para adicionar nossas consultas **categorias** e **fornecedores** como subconsultas, em vez de **INGRESSAR** s, nós não terá que reformular esses métodos para modificar dados. Com o botão direito no **GetProducts()** método o **ProductsTableAdapter** e escolha Configure. Em seguida, ajuste o **selecione** cláusula para que ele se parece como:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![Atualize a instrução SELECT para o método GetProducts()](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Figura 29**: atualização de **selecione** instrução para o **GetProducts()** método ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image81.png))


Depois de atualizar o **GetProducts()** método para usar essa consulta nova DataTable inclui duas novas colunas: **CategoryName** e **NomeDoFornecedor**.


![DataTable produtos tem duas novas colunas](creating-a-data-access-layer-cs/_static/image82.png)

**Figura 30**: O **produtos** DataTable tem duas novas colunas


Reserve um tempo para atualizar o **selecione** cláusula de **GetProductsByCategoryID (*categoryID*)** método também.

Se você atualizar o **GetProducts()** **selecione** usando **INGRESSAR** sintaxe o Designer de conjunto de dados não será capaz de gerar automaticamente os métodos para inserir, atualizar e excluir banco de dados usando o padrão de direta do banco de dados. Em vez disso, você precisará criar manualmente-los muito como fizemos com o **InsertProduct** método anteriormente neste tutorial. Além disso, você manualmente precisa fornecer o **InsertCommand**, **UpdateCommand**, e **DeleteCommand** valores de propriedade, se você quiser usar o lote padrão de atualização.

## <a name="adding-the-remaining-tableadapters"></a>Adicionando os TableAdapters restantes

Até agora, vimos apenas trabalhando com um TableAdapter único para uma tabela de banco de dados único. No entanto, o banco de dados Northwind contém várias tabelas relacionadas que vamos precisar trabalhar com nosso aplicativo web. Um conjunto de dados tipado pode conter vários DataTables relacionadas. Portanto, para concluir nosso DAL que precisamos adicionar tabelas de dados para as outras tabelas que usaremos esses tutoriais. Para adicionar um novo TableAdapter a um conjunto de dados tipado, abra o Designer de conjunto de dados, clique com botão direito no Designer e escolha Adicionar / TableAdapter. Isso criará uma nova DataTable e TableAdapter e orientá-lo por meio do assistente examinados anteriormente neste tutorial.

Levar alguns minutos para criar os seguintes TableAdapters e métodos usando as consultas a seguir. Observe que as consultas a **ProductsTableAdapter** incluem subconsultas para obter nomes de categoria e o fornecedor de cada produto. Além disso, se você estiver acompanhando, você já adicionou o **ProductsTableAdapter** da classe **GetProducts()** e **GetProductsByCategoryID (*categoryID* )** métodos.

- **ProductsTableAdapter**

    - **GetProducts**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
    - **GetProductsByCategoryID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
    - **GetProductsBySupplierID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
    - **GetProductByProductID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

    - **GetCategories**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
    - **GetCategoryByCategoryID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

    - **GetSuppliers**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
    - **GetSuppliersByCountry**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
    - **GetSupplierBySupplierID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

    - **GetEmployees**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
    - **GetEmployeesByManager**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
    - **GetEmployeeByEmployeeID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]


[![O Designer de conjunto de dados após terem sido adicionadas quatro TableAdapters](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Figura 31**: O DataSet Designer após a quatro TableAdapters foram adicionados ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Adicionando código personalizado para a DAL

Os TableAdapters e DataTables adicionado ao conjunto de dados digitados são expressas como um arquivo de definição de esquema XML (**Northwind.xsd**). Você pode exibir essas informações de esquema clicando no **Northwind.xsd** arquivo no Gerenciador de soluções e escolha o código de exibição.


[![Conjunto de dados de tipo de arquivo XSD (definição) do esquema XML para a Northwind](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**A Figura 32**: A definição de esquema de XML (XSD) de arquivo para o conjunto de dados tipado Northwind ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image88.png))


Essas informações de esquema são convertidas em código c# ou Visual Basic em tempo de design, quando compilado ou em tempo de execução (se necessário), no ponto em que você possa percorrê-lo com o depurador. Para exibir esse código gerado automaticamente vá para o modo de exibição de classe e detalhada para baixo para as classes do TableAdapter ou conjunto de dados tipado. Se você não vir o modo de exibição de classe na tela, vá para o menu de exibição e selecioná-lo de lá ou ocorrências Ctrl + Shift + C. A exibição de classe, você verá as propriedades, métodos e eventos das classes de conjunto de dados tipado e TableAdapter. Para exibir o código para um método específico, clique duas vezes no nome do método no modo de exibição de classe ou com o botão direito nele e selecione Ir para definição.


![Inspecione o código gerado automaticamente por selecionar Ir para a definição do modo de exibição de classe](creating-a-data-access-layer-cs/_static/image89.png)

**Figura 33**: inspecionar o código gerado automaticamente por selecionar Ir para a definição do modo de exibição de classe


Enquanto o código gerado automaticamente pode ser uma economia de tempo, o código geralmente é muito genérico e precisa ser personalizado para atender às necessidades exclusivas de um aplicativo. O risco de estender o código gerado automaticamente, no entanto, é que a ferramenta que o código gerado pode decidir é hora de "regenerar" e substituir suas personalizações. Com o novo conceito de classe parcial do .NET 2.0, é fácil dividir uma classe em vários arquivos. Isso nos permite adicionar nossa própria métodos, propriedades e eventos para as classes geradas automaticamente, sem precisar se preocupar sobre o Visual Studio, substituindo o nosso personalizações.

Para demonstrar como personalizar a DAL, vamos adicionar uma **GetProducts()** método para o **SuppliersRow** classe. O **SuppliersRow** classe representa um único registro no **fornecedores** tabela; cada provedor de fornecedor pode zero para muitos produtos, para **GetProducts()** retornará aqueles produtos do fornecedor especificado. Para realizar isso cria um novo arquivo de classe no **aplicativo\_código** pasta denominada **SuppliersRow.cs** e adicione o seguinte código:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Essa classe parcial instrui o compilador que, quando criar o **Northwind.SuppliersRow** classe para incluir o **GetProducts()** método que acabamos de definir. Se você compila o projeto e, em seguida, retorna ao modo de exibição de classe você verá **GetProducts()** agora listado como um método de **Northwind.SuppliersRow**.


![O método GetProducts() é agora parte da classe Northwind.SuppliersRow](creating-a-data-access-layer-cs/_static/image90.png)

**A Figura 34**: O **GetProducts()** método é agora parte do **Northwind.SuppliersRow** classe


O **GetProducts()** método agora pode ser usado para enumerar o conjunto de produtos para um fornecedor específico, como mostra o seguinte código:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Esses dados também podem ser exibidos em qualquer uma das ASP. Controles da Web de dados do NET. A página a seguir usa um controle GridView com dois campos:

- Um BoundField que exibe o nome de cada fornecedor e
- Um TemplateField que contém um controle BulletedList que está associado aos resultados retornados pelo **GetProducts()** método para cada fornecedor.

Vamos examinar como exibir esses relatórios de detalhes mestre em tutoriais futuros. Por enquanto, este exemplo é projetado para ilustrar usando o método personalizado adicionado para o **Northwind.SuppliersRow** classe.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![Nome da empresa do fornecedor é listado na coluna esquerda, seus produtos no canto direito](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Figura 35**: o nome do fornecedor a empresa está listado na coluna esquerda, seus produtos no canto direito ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>Resumo

Quando criar um aplicativo da web criando DAL deve ser uma de suas primeiras etapas, antes de começar a criar sua camada de apresentação. Com o Visual Studio, criar uma DAL baseada em conjuntos de dados tipado é uma tarefa que pode ser realizada em 10 a 15 minutos sem escrever uma linha de código. Os tutoriais são encaminhados se baseará este DAL. No [tutorial próxima](creating-a-business-logic-layer-cs.md) vamos definir uma série de regras de negócio e veja como implementá-los em uma camada de lógica de negócios separada.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Criando uma DAL usando TableAdapters fortemente tipado e DataTables no VS 2005 e o ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Criando componentes de camada de dados e transmissão de dados por meio de camadas](https://msdn.microsoft.com/library/ms978496.aspx)
- [Criar uma camada de acesso de dados com o Designer de conjunto de dados do Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Criptografando informações de configuração no ASP.NET 2.0 aplicativos](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Visão geral de TableAdapter](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Trabalhando com um conjunto de dados tipado](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Acesso a dados fortemente tipado no Visual Studio 2005 e o ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Como estender os métodos TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Recuperação de dados escalar de um procedimento armazenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre tópicos contidos neste tutorial

- [Camadas de Acesso a Dados em aplicativos do ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Como associar um conjunto de dados manualmente para uma grade de dados](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Como trabalhar com conjuntos de dados e os filtros de um aplicativo ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Ron Green, Giesenow Hilton, Dennis Patterson, Liz Shulok, Abel Gomez e Carlos Santos. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Avançar](creating-a-business-logic-layer-cs.md)
