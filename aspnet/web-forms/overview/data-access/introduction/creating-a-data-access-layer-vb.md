---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
title: Criando uma camada de acesso de dados (VB) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, vamos começar desde o início e criar o Data Access DAL (camada), usando DataSets tipados, para acessar as informações em um banco de dados.
ms.author: aspnetcontent
ms.date: 04/05/2010
ms.assetid: 6227233a-6254-4b6b-9a89-947efef22330
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-vb
msc.type: authoredcontent
ms.openlocfilehash: bfd4a1d9e47101594140ad4357bb467128ce26ee
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833083"
---
<a name="creating-a-data-access-layer-vb"></a>Criando uma camada de acesso de dados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_1_VB.exe) ou [baixar PDF](creating-a-data-access-layer-vb/_static/datatutorial01vb1.pdf)

> Neste tutorial, vamos começar desde o início e criar o Data Access DAL (camada), usando DataSets tipados, para acessar as informações em um banco de dados.


## <a name="introduction"></a>Introdução

Como os desenvolvedores da web, nossas vidas giram em torno de trabalhar com dados. Podemos criar bancos de dados para armazenar os dados, o código para recuperar e modificar e páginas da web para coletar e resumi-la. Este é o primeiro tutorial de uma série de longa que irá explorar técnicas para implementar esses padrões comuns no ASP.NET 2.0. Vamos começar com a criação de um [arquitetura de software](http://en.wikipedia.org/wiki/Software_architecture) composto de uma camada de dados acesso (DAL) usando DataSets tipados, uma camada de lógica de negócios (BLL), que impõe regras de negócio personalizadas, e uma camada de apresentação composta de ASP.NET páginas que Compartilhe um layout de página comuns. Depois que esse trabalho de fundação de back-end foram desenvolvido, vamos então para emissão de relatórios, que mostra como exibir, resumir, coletar e validar os dados de um aplicativo web. Esses tutoriais se destinam a ser concisas e fornecem instruções passo a passo com capturas de tela suficiente para orientá-lo pelo processo visualmente. Cada tutorial está disponível nas versões do Visual Basic e do c# e inclui um download do código completo usado. (Este primeiro tutorial for muito longo, mas o restante são apresentados em blocos de fácil de entender muito mais).

Para esses tutoriais que usaremos uma versão do Microsoft SQL Server 2005 Express Edition do banco de dados Northwind, colocado no `App_Data` directory. Além do arquivo de banco de dados, o `App_Data` pasta também contém os scripts SQL para a criação de banco de dados, caso você deseje usar uma versão de banco de dados diferente. Esses scripts podem ser também [baixadas diretamente da Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), se você preferir. Se você usar uma versão diferente do SQL Server do banco de dados Northwind, você precisará atualizar o `NORTHWNDConnectionString` definindo na caixa de diálogo `Web.config` arquivo. O aplicativo web foi criado usando o Visual Studio 2005 Professional Edition como um projeto de site da Web baseado no sistema de arquivos. No entanto, todos os tutoriais funcionará igualmente bem com a versão gratuita do Visual Studio 2005, [o Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).

Neste tutorial vamos começar desde o início e criar o Data Access DAL (camada), seguido de criação de [camada de lógica de negócios (BLL)](creating-a-business-logic-layer-vb.md) na segunda tutorial e trabalhar em [navegaçãoelayoutdapágina](master-pages-and-site-navigation-vb.md) na terceira. Os tutoriais depois que um terceiro se baseará a base apresentados em três primeiras. Nós temos muita coisa para cobrir este primeiro tutorial, então, inicie o Visual Studio e vamos começar!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Etapa 1: Criando um projeto Web e conectar-se ao banco de dados

Antes de criarmos nossa camada de acesso de dados (DAL), primeiro precisamos criar um site da web e configurar o nosso banco de dados. Comece criando um novo arquivo com base no sistema site ASP.NET. Para fazer isso, vá para o menu Arquivo e escolha o novo Site, exibindo a caixa de diálogo Novo Site da Web. Escolha o modelo de Site da Web ASP.NET, defina a lista suspensa de local para o sistema de arquivos, escolha uma pasta para colocar o site da web e definir o idioma para o Visual Basic.


[![Criar um novo arquivo com base no sistema Web Site](creating-a-data-access-layer-vb/_static/image2.png)](creating-a-data-access-layer-vb/_static/image1.png)

**Figura 1**: criar um Site New File System-Based ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image3.png))


Isso criará um novo site com uma `Default.aspx` página do ASP.NET, um `App_Data` pasta e um `Web.config` arquivo.

Com o site da web criada, a próxima etapa é adicionar uma referência ao banco de dados no Gerenciador de servidores do Visual Studio. Adicionando um banco de dados para o Gerenciador de servidores, você pode adicionar tabelas, procedimentos armazenados, exibições e assim por diante tudo de dentro do Visual Studio. Você também pode exibir dados da tabela ou criar suas próprias consultas manual ou graficamente via o construtor de consultas. Além disso, quando criamos o DataSets tipados para a DAL precisaremos para apontar o Visual Studio para o banco de dados do qual os conjuntos de dados tipado deve ser criados. Enquanto estamos pode fornecer essas informações de conexão no momento, o Visual Studio preenche automaticamente uma lista suspensa dos bancos de dados já está registrado no Gerenciador de servidores.

As etapas para adicionar banco de dados Northwind para o Gerenciador de servidores dependem se você deseja usar o banco de dados do SQL Server 2005 Express Edition no `App_Data` pasta ou se você tiver um Microsoft SQL Server 2000 ou a instalação do server 2005 de banco de dados que você deseja usar em vez disso.

## <a name="using-a-database-in-theappdatafolder"></a>Usando um banco de dados a`App_Data`pasta

Se você não tiver um SQL Server 2000 ou 2005 servidor de banco de dados para se conectar ao, ou você simplesmente deseja evitar a necessidade de adicionar o banco de dados para um servidor de banco de dados, você pode usar a versão do SQL Server 2005 Express Edition do banco de dados Northwind está localizado na site baixado da Web pré-existente e's `App_Data` pasta (`NORTHWND.MDF`).

Um banco de dados colocados no `App_Data` pasta é adicionada automaticamente para o Gerenciador de servidores. Supondo que você tenha o SQL Server 2005 Express Edition instalada em seu computador, você deverá ver um nó chamado northwnd não. Arquivos MDF no Gerenciador de servidores, que você pode expandir e explore suas tabelas, exibições, procedimentos armazenados e assim por diante (veja a Figura 2).

O `App_Data` pasta também pode manter o Microsoft Access `.mdb` arquivos, que, assim como suas contrapartes do SQL Server, são adicionados automaticamente para o Gerenciador de servidores. Se você não quiser usar qualquer uma das opções do SQL Server, você pode sempre [baixar uma versão do arquivo de banco de dados Northwind do Microsoft Access](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) e solte para o `App_Data` directory. Tenha em mente, no entanto, que acessar bancos de dados não são tão elaboradas que o SQL Server e não foram projetados para ser usado em cenários de site da web. Além disso, alguns dos tutoriais 35 + utilizará determinados recursos de nível de banco de dados que não têm suporte pelo Access.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Conectar-se ao banco de dados em um servidor de banco de dados do Microsoft SQL Server 2000 ou 2005

Como alternativa, você pode se conectar a um banco de dados Northwind instalado em um servidor de banco de dados. Se o servidor de banco de dados ainda não tiver o banco de dados Northwind instalado, você primeiro deve adicioná-lo ao servidor de banco de dados, executando o script de instalação incluído no download deste tutorial ou pelo [baixando a versão do SQL Server 2000 do Northwind e o script de instalação](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) diretamente do site da Microsoft.

Depois que o banco de dados instalado, vá para o Gerenciador de servidores no Visual Studio, clique com botão direito no nó de conexões de dados e escolha Adicionar Conexão. Se você não vir o Gerenciador de servidores de ir para a exibição / Gerenciador de servidores ou ocorrências Ctrl + Alt + S. Isso abrirá a caixa de diálogo Adicionar Conexão, onde é possível especificar o servidor para se conectar ao, as informações de autenticação e o nome do banco de dados. Depois que você configurou as informações de conexão de banco de dados e clicar no botão Okey com êxito, o banco de dados será adicionado como um nó sob o nó de conexões de dados. Você pode expandir o nó de banco de dados para explorar suas tabelas, exibições, procedimentos armazenados e assim por diante.


![Adicionar uma Conexão ao banco de dados do seu servidor de banco de dados Northwind](creating-a-data-access-layer-vb/_static/image4.png)

**Figura 2**: adicionar uma Conexão ao banco de dados do seu servidor de banco de dados Northwind


## <a name="step-2-creating-the-data-access-layer"></a>Etapa 2: Criando a camada de acesso a dados

Ao trabalhar com uma opção de dados é incorporar a lógica específica de dados diretamente para a camada de apresentação (em um aplicativo web, a marca de páginas ASP.NET-se a camada de apresentação). Isso pode assumir a forma de escrever código ADO.NET na parte de código da página ASP.NET ou usando o controle SqlDataSource da parte de marcação. Em ambos os casos, essa abordagem acoplada rigidamente a lógica de acesso a dados com a camada de apresentação. A abordagem recomendada, no entanto, é separar a lógica de acesso de dados da camada de apresentação. Essa camada separada é conhecida como a camada de acesso a dados, a DAL para abreviar e geralmente é implementada como um projeto de biblioteca de classes separado. Os benefícios dessa arquitetura em camadas sejam bem documentados (consulte a seção de "Leituras adicionais" no final deste tutorial para obter informações sobre essas vantagens) e é a abordagem que faremos nesta série.

Todo o código que é específico à fonte de dados subjacente, como a criação de uma conexão ao banco de dados, emitindo `SELECT`, `INSERT`, `UPDATE`, e `DELETE` comandos e assim por diante devem estar localizado no DAL. A camada de apresentação não deve conter todas as referências a esse código de acesso de dados, mas em vez disso, deve fazer chamadas para a DAL para solicitações de todos os dados. Camadas de acesso de dados normalmente contêm métodos para acessar o banco de dados subjacente. O banco de dados Northwind, por exemplo, tem `Products` e `Categories` tabelas que registram os produtos para venda e as categorias às quais eles pertencem. Nossa DAL teremos métodos, como:

- `GetCategories(),` que retornará informações sobre todas as categorias
- `GetProducts()`, que retornará informações sobre todos os produtos
- `GetProductsByCategoryID(categoryID)`, que retornará todos os produtos que pertencem a uma categoria especificada
- `GetProductByProductID(productID)`, que retornará informações sobre um produto específico

Esses métodos, quando invocada, conecte-se ao banco de dados, emita a consulta apropriada e retornar os resultados. Como devemos retornar esses resultados é importante. Esses métodos simplesmente poderia retornar um conjunto de dados ou o DataReader preenchida pela consulta de banco de dados, mas o ideal é que esses resultados devem ser retornados usando *objetos fortemente tipados*. Um objeto fortemente tipado é um cujo esquema rigidamente é definida em tempo de compilação, ao passo que o oposto, um objeto fracamente tipada, é aquele cujo esquema não é conhecida até o tempo de execução.

Por exemplo, o DataReader e o conjunto de dados (por padrão) são objetos com tipagem como seu esquema é definida pelas colunas retornadas pela consulta de banco de dados usada para preenchê-los. Para acessar uma determinada coluna de um DataTable tipagem, precisamos usar uma sintaxe como: `DataTable.Rows(index)("columnName")`. A DataTable rigidez de tipos. neste exemplo é apresentado pelo fato de que precisamos para acessar o nome da coluna usando uma cadeia de caracteres ou um índice ordinal. Uma DataTable fortemente tipado, por outro lado, terá cada uma de suas colunas implementadas como propriedades, resultando em código que se parece com: `DataTable.Rows(index).columnName`.

Para retornar objetos fortemente tipados, os desenvolvedores podem criar seus próprios objetos comerciais personalizados ou usar conjuntos de dados tipados. Um objeto de negócios é implementado pelo desenvolvedor como representa uma classe cujas propriedades normalmente refletem as colunas da tabela de banco de dados subjacente do objeto comercial. Um conjunto de dados tipado é uma classe gerada para você pelo Visual Studio com base em um esquema de banco de dados e cujos membros são fortemente tipadas de acordo com esse esquema. O conjunto de dados tipado em si consiste em classes que estendem as classes de DataRow, DataTable e DataSet do ADO.NET. Além das tabelas de dados fortemente tipados, DataSets tipados agora também incluem TableAdapters, que são classes com métodos para popular tabelas de dados do conjunto de dados e propagar as modificações dentro de tabelas de dados no banco de dados.

> [!NOTE]
> Para obter mais informações sobre as vantagens e desvantagens do uso de DataSets tipados versus objetos comerciais personalizados, consulte [criação de componentes da camada de dados e passando dados através de camadas](https://msdn.microsoft.com/library/ms978496.aspx).


Vamos usar conjuntos de dados fortemente tipados para a arquitetura desses tutoriais. Figura 3 ilustra o fluxo de trabalho entre as diferentes camadas de um aplicativo que usa conjuntos de dados tipados.


[![Todo código de acesso de dados é relegados a DAL](creating-a-data-access-layer-vb/_static/image6.png)](creating-a-data-access-layer-vb/_static/image5.png)

**Figura 3**: todos os código de acesso de dados é relegados a DAL ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Criação de um conjunto de dados tipado e o adaptador de tabela

Para começar a criar nossa DAL, vamos começar adicionando um DataSet tipado ao nosso projeto. Para fazer isso, clique com botão direito no nó do projeto no Gerenciador de soluções e escolha Adicionar um novo Item. Selecione a opção de conjunto de dados na lista de modelos e nomeie- `Northwind.xsd`.


[![Optar por adicionar um novo conjunto de dados ao seu projeto](creating-a-data-access-layer-vb/_static/image9.png)](creating-a-data-access-layer-vb/_static/image8.png)

**Figura 4**: escolha Adicionar um novo conjunto de dados ao seu projeto ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image10.png))


Depois de clicar em Adicionar, quando solicitado a adicionar o conjunto de dados para o `App_Code` pasta, escolha Sim. O Designer para o conjunto de dados tipado, em seguida, será exibido e iniciará o Assistente de configuração do TableAdapter, permitindo que você adicione seu primeiro TableAdapter ao conjunto de dados tipado.

Um conjunto de dados tipado serve como uma coleção fortemente tipada dos dados. ele é composto de instâncias de DataTable fortemente tipadas, cada um dos quais por sua vez é composta de instâncias de DataRow fortemente tipada. Vamos criar uma tabela de dados fortemente tipado para cada uma das tabelas de banco de dados subjacentes que precisamos para trabalhar com esta série de tutoriais. Vamos começar criando um DataTable para o `Products` tabela.

Tenha em mente que DataTables fortemente tipadas não incluem todas as informações sobre como acessar dados da tabela de banco de dados subjacente. Para recuperar os dados para preencher a DataTable, usamos uma classe de TableAdapter, que funciona como nossa camada de acesso a dados. Para nosso `Products` DataTable, o TableAdapter conterá os métodos `GetProducts()`, `GetProductByCategoryID(categoryID)`e assim por diante que vai invocamos da camada de apresentação. Função da DataTable é servir como os usado para passar dados entre as camadas de objetos fortemente tipados.

Começa solicitando que você selecione qual banco de dados para trabalhar com o Assistente de configuração do TableAdapter. A lista suspensa mostra esses bancos de dados no Gerenciador de servidores. Se você não adicionou o banco de dados Northwind para o Gerenciador de servidores, você pode clicar no botão Nova Conexão no momento para fazê-lo.


[![Escolha o banco de dados Northwind na lista suspensa](creating-a-data-access-layer-vb/_static/image12.png)](creating-a-data-access-layer-vb/_static/image11.png)

**Figura 5**: escolha o banco de dados Northwind na lista suspensa ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image13.png))


Depois de selecionar o banco de dados e clicar em Avançar, você será solicitado se você deseja salvar a cadeia de conexão no `Web.config` arquivo. Salvando a cadeia de caracteres de conexão, você evitará tendo rígido codificados nas classes TableAdapter, que simplifica as coisas se as informações de cadeia de caracteres de conexão é alterado no futuro. Se você optar por salvar a cadeia de caracteres de conexão no arquivo de configuração que ele é colocado na `<connectionStrings>` seção, que pode ser [opcionalmente criptografado](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) para segurança aprimorada ou modificado posteriormente com a nova página de propriedade do ASP.NET 2.0 dentro o IIS Admin ferramenta GUI, que é mais ideal para os administradores.


[![Salvar a cadeia de caracteres de Conexão em Web. config](creating-a-data-access-layer-vb/_static/image15.png)](creating-a-data-access-layer-vb/_static/image14.png)

**Figura 6**: salvar a cadeia de Conexão para `Web.config` ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image16.png))


Em seguida, precisamos definir o esquema para a primeira DataTable fortemente tipados e fornecem o primeiro método para nosso TableAdapter usar ao popular o conjunto de dados fortemente tipados. Essas duas etapas são realizadas simultaneamente com a criação de uma consulta que retorna as colunas da tabela que queremos que apareçam nos nossos DataTable. No final do assistente, você terá um nome de método para esta consulta. Depois que o que é feito, esse método pode ser chamado na nossa camada de apresentação. O método executará a consulta definida e preencher uma DataTable fortemente tipada.

Para começar a definição da consulta SQL primeiro deve indicar que desejamos TableAdapter para emitir a consulta. Podemos usar uma instrução de SQL ad hoc, crie um novo procedimento armazenado ou usar um procedimento armazenado existente. Para esses tutoriais, usaremos instruções SQL ad hoc. Consulte a [Brian Noyes](http://briannoyes.net/)do artigo [criar uma camada de acesso a dados com o Visual Studio 2005 DataSet Designer](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) para obter um exemplo de como usar procedimentos armazenados.


[![Consultar os dados usando uma instrução de SQL Ad Hoc](creating-a-data-access-layer-vb/_static/image18.png)](creating-a-data-access-layer-vb/_static/image17.png)

**Figura 7**: consultar os dados usando uma instrução de SQL Ad Hoc ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image19.png))


Nesse ponto podemos pode digitar na consulta SQL manualmente. Ao criar o primeiro método no TableAdapter, você geralmente deseja que a consulta retornar as colunas que precisam ser expressos na DataTable correspondente. Podemos pode fazer isso criando uma consulta que retorna todas as colunas e todas as linhas do `Products` tabela:


[![Insira a consulta SQL na caixa de texto](creating-a-data-access-layer-vb/_static/image21.png)](creating-a-data-access-layer-vb/_static/image20.png)

**Figura 8**: insira o SQL consulta para a caixa de texto ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image22.png))


Como alternativa, use o construtor de consultas e graficamente construir a consulta, conforme mostrado na Figura 9.


[![Criar a consulta graficamente, por meio do Editor de consultas](creating-a-data-access-layer-vb/_static/image24.png)](creating-a-data-access-layer-vb/_static/image23.png)

**Figura 9**: criar a consulta graficamente, por meio do Editor de consultas ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image25.png))


Depois de criar a consulta, mas antes de passar para a próxima tela, clique no botão Opções avançadas. Em projetos de Site da Web, o "Gerar Insert, Update e Delete instruções" é a única opção selecionada por padrão; avançada Se você executar esse assistente em uma biblioteca de classes ou um projeto do Windows a opção "Usar simultaneidade otimista" também será selecionada. Deixe a opção "Usar simultaneidade otimista" desmarcada por enquanto. Vamos examinar a simultaneidade otimista em tutoriais futuros.


[![Selecione apenas as gerar Insert, Update e Delete instruções de opção](creating-a-data-access-layer-vb/_static/image27.png)](creating-a-data-access-layer-vb/_static/image26.png)

**Figura 10**: selecione apenas as gerar Insert, Update e Delete instruções de opção ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image28.png))


Depois de verificar as opções avançadas, clique em Avançar para ir para a tela final. Aqui, são solicitados a selecionar quais métodos para adicionar ao TableAdapter. Há dois padrões de preenchimento de dados:

- **Preencher uma DataTable** com essa abordagem, um método é criado que leva em uma DataTable como um parâmetro e a preenche com base nos resultados da consulta. A classe DataAdapter do ADO.NET, por exemplo, implementa esse padrão com seu `Fill()` método.
- **Retornar uma DataTable** com essa abordagem o método cria e preenche o DataTable para você e retorna-a como os métodos retornam um valor.

Você pode ter o TableAdapter implementar um ou ambos esses padrões. Você também pode renomear os métodos fornecidos aqui. Vamos deixar ambas as caixas de seleção marcadas, mesmo que apenas usaremos o último padrão durante esses tutoriais. Além disso, vamos renomear o genérico `GetData` método para `GetProducts`.

Se marcada, a caixa de seleção final, "GenerateDBDirectMethods", cria `Insert()`, `Update()`, e `Delete()` métodos para o TableAdapter. Se você deixar essa opção desmarcada, todas as atualizações precisam ser feitas por meio de sole do TableAdapter `Update()` método, que usa o conjunto de dados tipado, um DataTable, uma DataRow única ou uma matriz de DataRows. (Se você não verificado as "Gerar Insert, Update e Delete instruções" opção das propriedades avançadas na Figura 9, essa caixa de seleção configuração não terá efeito.) Vamos deixar essa caixa de seleção marcada.


[![Alterar o nome do método de GetData para GetProducts](creating-a-data-access-layer-vb/_static/image30.png)](creating-a-data-access-layer-vb/_static/image29.png)

**Figura 11**: altere o nome do método de `GetData` à `GetProducts` ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image31.png))


Conclua o assistente clicando em Finish. Depois de fechar o assistente, são retornados para o Designer de conjunto de dados que mostra a DataTable que acabamos de criar. Você pode ver a lista de colunas na `Products` DataTable (`ProductID`, `ProductName`e assim por diante), bem como os métodos dos `ProductsTableAdapter` (`Fill()` e `GetProducts()`).


[![A DataTable de produtos e ProductsTableAdapter foram adicionados ao conjunto de dados tipados](creating-a-data-access-layer-vb/_static/image33.png)](creating-a-data-access-layer-vb/_static/image32.png)

**Figura 12**: O `Products` DataTable e `ProductsTableAdapter` foram adicionadas ao conjunto de dados tipado ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image34.png))


Neste ponto, temos um conjunto de dados tipado com uma única tabela de dados (`Northwind.Products`) e uma classe de DataAdapter fortemente tipado (`NorthwindTableAdapters.ProductsTableAdapter`) com um `GetProducts()` método. Esses objetos podem ser usados para acessar uma lista de todos os produtos de código da seguinte forma:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample1.vb)]

Esse código não exigia gente escreva um pouco de código específicos de acesso a dados. Não tínhamos instanciar as classes ADO.NET, podemos não tinha para se referir a qualquer cadeia de caracteres de conexão, consultas SQL ou procedimentos armazenados. Em vez disso, o TableAdapter nos fornece o código de acesso a dados de nível baixo.

Cada objeto usado neste exemplo também é fortemente tipada, permitindo que o Visual Studio fornecer o IntelliSense e a verificação de tipo de tempo de compilação. E o melhor de todos os o DataTables retornado pelo TableAdapter pode ser associado a dados do ASP.NET controles de Web, como o GridView, DetailsView, DropDownList, CheckBoxList e várias outras. O exemplo a seguir ilustra a associação DataTable retornada pela `GetProducts()` método em um GridView em apenas um exame três linhas de código dentro de `Page_Load` manipulador de eventos.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample2.aspx)]

AllProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample3.vb)]


[![A lista de produtos é exibida em um GridView](creating-a-data-access-layer-vb/_static/image36.png)](creating-a-data-access-layer-vb/_static/image35.png)

**Figura 13**: lista de produtos é exibida em um GridView ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image37.png))


Embora este exemplo necessário que escrevemos três linhas de código em nossa página do ASP.NET `Page_Load` manipulador de eventos, no futuro tutoriais, vamos examinar como usar o ObjectDataSource para declarativamente recuperar os dados da DAL. Com o ObjectDataSource, não terá que escrever nenhum código e obterá suporte de paginação e classificação também!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Etapa 3: Adicionando parametrizadas métodos para a camada de acesso a dados

Neste ponto nossos `ProductsTableAdapter` classe tem apenas um método, `GetProducts()`, que retorna todos os produtos no banco de dados. Enquanto que está sendo capaz de trabalhar com todos os produtos é certamente útil, há vezes quando queremos recuperar informações sobre um produto específico ou todos os produtos que pertencem a uma categoria específica. Para adicionar essa funcionalidade à nossa camada de acesso a dados, podemos adicionar métodos com parâmetros ao TableAdapter.

Vamos adicionar o `GetProductsByCategoryID(categoryID)` método. Para adicionar um novo método para a DAL, retorne ao Designer de conjunto de dados, clique com botão direito no `ProductsTableAdapter` seção e, em seguida, escolha Add Query.


![Clique com botão direito no TableAdapter e escolha Adicionar consulta](creating-a-data-access-layer-vb/_static/image38.png)

**Figura 14**: clique com botão direito no TableAdapter e escolha Adicionar consulta


Primeiro, estamos for solicitados a se quisermos acessar o banco de dados usando uma instrução de SQL ad hoc ou um procedimento armazenado de novo ou existente. Vamos escolher usar uma instrução de SQL ad hoc novamente. Em seguida, são perguntados que tipo de consulta SQL gostaríamos de usar. Como queremos retornar todos os produtos que pertencem a uma categoria especificada, vamos querer gravar um `SELECT` instrução que retorna linhas.


[![Optar por criar uma instrução SELECT que retorna linhas](creating-a-data-access-layer-vb/_static/image40.png)](creating-a-data-access-layer-vb/_static/image39.png)

**Figura 15**: escolha para criar um `SELECT` instrução que retorna linhas ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image41.png))


A próxima etapa é definir a consulta SQL usada para acessar os dados. Como queremos retornar apenas os produtos que pertencem a uma determinada categoria, posso usar o mesmo `SELECT` instrução from `GetProducts()`, mas adicione o seguinte `WHERE` cláusula: `WHERE CategoryID = @CategoryID`. O `@CategoryID` parâmetro indica ao Assistente do TableAdapter que o método que estamos criando exigirá um parâmetro de entrada do tipo correspondente (ou seja, um inteiro anulável).


[![Insira uma consulta para retornar somente os produtos em uma categoria especificada](creating-a-data-access-layer-vb/_static/image43.png)](creating-a-data-access-layer-vb/_static/image42.png)

**Figura 16**: insira uma consulta para somente retornam os produtos em uma categoria especificada ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image44.png))


Na etapa final que podemos escolher quais padrões para usar, bem como personalizar os nomes dos métodos gerados de acesso a dados. Para o padrão de preenchimento, vamos alterar o nome a ser `FillByCategoryID` e para o retorno de um DataTable retorno padrão (o `GetX` métodos), vamos usar `GetProductsByCategoryID`.


[![Escolha os nomes dos métodos do TableAdapter](creating-a-data-access-layer-vb/_static/image46.png)](creating-a-data-access-layer-vb/_static/image45.png)

**Figura 17**: escolha os nomes dos métodos do TableAdapter ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image47.png))


Depois de concluir o assistente, o Designer de conjunto de dados inclui os novos métodos do TableAdapter.


![Os produtos podem agora ser consultados por categoria](creating-a-data-access-layer-vb/_static/image48.png)

**Figura 18**: os produtos podem agora ser consultados por categoria


Reserve um tempo para adicionar um `GetProductByProductID(productID)` usando a mesma técnica de método.

Essas consultas com parâmetros podem ser testadas diretamente do Designer de conjunto de dados. Clique com botão direito no método no TableAdapter e escolha a visualização de dados. Em seguida, insira os valores para usar para os parâmetros e clique em Visualizar.


[![Pertencendo esses produtos na categoria Bebidas são mostrados](creating-a-data-access-layer-vb/_static/image50.png)](creating-a-data-access-layer-vb/_static/image49.png)

**Figura 19**: os produtos que pertencem à categoria de bebidas são mostrados ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image51.png))


Com o `GetProductsByCategoryID(categoryID)` método nossa DAL, agora podemos criar uma página ASP.NET que exibe apenas os produtos em uma categoria especificada. O exemplo a seguir mostra todos os produtos que estão na categoria Bebidas, que têm um `CategoryID` de 1.

Beverages.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample4.aspx)]

Beverages.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample5.vb)]


[![Esses produtos na categoria Bebidas são exibidos](creating-a-data-access-layer-vb/_static/image53.png)](creating-a-data-access-layer-vb/_static/image52.png)

**Figura 20**: os produtos na categoria Bebidas são exibidos ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Etapa 4: Inserindo, atualizando e excluindo dados

Há dois padrões comumente usados para inserir, atualizar e excluir dados. O primeiro padrão, que chamarei de padrão de banco de dados direta, envolve a criação de métodos que, quando invocada, problema de um `INSERT`, `UPDATE`, ou `DELETE` comando no banco de dados que opera em um registro de banco de dados individual. Normalmente, esses métodos são passados em uma série de valores escalares (inteiros, cadeias de caracteres, booleanos, DateTimes e assim por diante) que correspondem aos valores para inserir, atualizar ou excluir. Por exemplo, com esse padrão para o `Products` tabela, o método delete seria necessário um parâmetro de número inteiro que indica a `ProductID` do registro a excluir, enquanto o método insert levaria em uma cadeia de caracteres para o `ProductName`, um decimal para o `UnitPrice`, um inteiro para o `UnitsOnStock`e assim por diante.


[![Cada inserção, atualização e Excluir solicitação é enviada para o banco de dados imediatamente](creating-a-data-access-layer-vb/_static/image56.png)](creating-a-data-access-layer-vb/_static/image55.png)

**Figura 21**: cada inserção, atualização e Excluir solicitação é enviada para o banco de dados imediatamente ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image57.png))


O outro padrão, eu irei me referir como o lote de atualização padrão, é atualizar um conjunto de dados, DataTable ou coleção de DataRows em uma chamada de método todo. Com esse padrão de um desenvolvedor exclui, insere e modifica o DataRows em uma DataTable e, em seguida, passa esses DataRows ou DataTable para um método de atualização. Esse método, em seguida, enumera DataRows passado, determina se eles já foram modificados, adicionados ou excluídos (por meio do DataRow [RowState propriedade](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) valor) e emite a solicitação de banco de dados apropriado para cada registro.


[![Todas as alterações são sincronizadas com o banco de dados quando o método de atualização é invocado](creating-a-data-access-layer-vb/_static/image59.png)](creating-a-data-access-layer-vb/_static/image58.png)

**Figura 22**: todas as alterações são sincronizadas com o banco de dados quando o método de atualização é invocado ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image60.png))


O TableAdapter usa o padrão de atualização de lote por padrão, mas também suporta o padrão de banco de dados direto. Uma vez que selecionamos a opção "Gerar Insert, Update e Delete instruções" do the Advanced Properties durante a criação de nosso TableAdapter, o `ProductsTableAdapter` contém um `Update()` método, que implementa o padrão de atualização em lotes. Especificamente, o TableAdapter contém um `Update()` método que pode ser passado o conjunto de dados tipado, uma tabela de dados fortemente tipados ou DataRows um ou mais. Se você deixou a caixa de seleção "GenerateDBDirectMethods" marcada quando primeiro criar o TableAdapter o padrão de banco de dados direto será também implementado por meio `Insert()`, `Update()`, e `Delete()` métodos.

Ambos os padrões de modificação de dados usam o TableAdapter `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades emitir seus `INSERT`, `UPDATE`, e `DELETE` comandos no banco de dados. Você pode inspecionar e modificar as `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades clicando no TableAdapter no Designer de conjunto de dados e, em seguida, indo para a janela Propriedades. (Verifique se você tiver selecionado o TableAdapter e que o `ProductsTableAdapter` objeto é selecionado na lista suspensa na janela Propriedades.)


[![O TableAdapter tem InsertCommand, UpdateCommand e DeleteCommand propriedades](creating-a-data-access-layer-vb/_static/image62.png)](creating-a-data-access-layer-vb/_static/image61.png)

**Figura 23**: tem o TableAdapter `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image63.png))


Para examinar ou modificar qualquer uma dessas propriedades de comando de banco de dados, clique no `CommandText` subpropriedade, que abre o construtor de consultas.


[![Configurar o INSERT, UPDATE e DELETE instruções no construtor de consultas](creating-a-data-access-layer-vb/_static/image65.png)](creating-a-data-access-layer-vb/_static/image64.png)

**Figura 24**: configurar o `INSERT`, `UPDATE`, e `DELETE` instruções no construtor de consultas ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image66.png))


O exemplo de código a seguir mostra como usar o padrão de atualização em lotes para duas vezes o preço de todos os produtos que não são interrompidas e que têm 25 unidades em estoque ou menos:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample6.vb)]

O código a seguir ilustra como usar o padrão de banco de dados direto por meio de programação excluir um produto específico e, em seguida, atualizar um e, em seguida, adicione um novo:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample7.vb)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Criação personalizada inserir, atualizar e excluir métodos

O `Insert()`, `Update()`, e `Delete()` métodos criados pelo método direto de banco de dados podem ser um pouco complicados, especialmente para tabelas com muitas colunas. Examinando o exemplo de código anterior, sem a Ajuda do IntelliSense do não é particularmente claro o que `Products` coluna de tabela é mapeado para cada parâmetro de entrada para o `Update()` e `Insert()` métodos. Pode haver ocasiões em que só queremos atualizar uma única coluna ou dois, ou deseja um personalizado `Insert()` método que será, talvez, retornar o valor do registro inserido recentemente `IDENTITY` campo (incremento automático).

Para criar um método personalizado desse tipo, retorne ao Designer de conjunto de dados. Clique com botão direito no TableAdapter e escolha Add Query, retornando ao Assistente do TableAdapter. Na segunda tela, podemos pode indicar o tipo de consulta para criar. Vamos criar um método que adiciona um novo produto e, em seguida, retorna o valor do registro recém-adicionado `ProductID`. Portanto, optar por criar um `INSERT` consulta.


[![Crie um método para adicionar uma nova linha à tabela produtos](creating-a-data-access-layer-vb/_static/image68.png)](creating-a-data-access-layer-vb/_static/image67.png)

**Figura 25**: criar um método para adicionar uma linha nova para o `Products` tabela ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image69.png))


Na próxima tela do `InsertCommand`do `CommandText` é exibida. Aumentar essa consulta adicionando `SELECT SCOPE_IDENTITY()` no final da consulta, que retornará o último valor de identidade inserido em um `IDENTITY` coluna no mesmo escopo. (Consulte a [documentação técnica](https://msdn.microsoft.com/library/ms190315.aspx) para obter mais informações sobre `SCOPE_IDENTITY()` e por que você provavelmente desejará [usar escopo\_IDENTITY() no lugar de @@IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Certifique-se de que você termina a `INSERT` instrução com ponto e vírgula antes de adicionar o `SELECT` instrução.


[![Ampliar a consulta para retornar o valor de SCOPE_IDENTITY)](creating-a-data-access-layer-vb/_static/image71.png)](creating-a-data-access-layer-vb/_static/image70.png)

**Figura 26**: aumentar a consulta para retornar o `SCOPE_IDENTITY()` valor ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image72.png))


Por fim, nomeie o novo método `InsertProduct`.


[![Defina o nome do novo método para InsertProduct](creating-a-data-access-layer-vb/_static/image74.png)](creating-a-data-access-layer-vb/_static/image73.png)

**Figura 27**: defina o novo nome do método como `InsertProduct` ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image75.png))


Quando você retornar para o DataSet Designer você verá que o `ProductsTableAdapter` contém um novo método, `InsertProduct`. Se esse novo método não tem um parâmetro para cada coluna na `Products` tabela, as chances são que você se esqueceu de encerrar o `INSERT` instrução com ponto e vírgula. Configurar o `InsertProduct` método e verifique se você tem um ponto e vírgula que delimita o `INSERT` e `SELECT` instruções.

Por padrão, insert métodos sem consulta de problema de métodos, que significa que elas retornam o número de linhas afetadas. No entanto, queremos que o `InsertProduct` método para retornar o valor retornado pela consulta, não o número de linhas afetadas. Para fazer isso, ajustar o `InsertProduct` do método `ExecuteMode` propriedade `Scalar`.


[![Altere a propriedade de ExecuteMode para escalar](creating-a-data-access-layer-vb/_static/image77.png)](creating-a-data-access-layer-vb/_static/image76.png)

**Figura 28**: altere o `ExecuteMode` propriedade `Scalar` ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image78.png))


O código a seguir mostra essa nova `InsertProduct` método em ação:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample8.vb)]

## <a name="step-5-completing-the-data-access-layer"></a>Etapa 5: Concluir a camada de acesso a dados

Observe que o `ProductsTableAdapters` classe retorna o `CategoryID` e `SupplierID` os valores do `Products` de tabela, mas não inclui o `CategoryName` coluna a partir o `Categories` tabela ou o `CompanyName` coluna a partir o `Suppliers` a tabela, embora essas provavelmente são as colunas que desejamos exibir durante a exibição de informações do produto. Podemos pode ampliar o método do TableAdapter inicial, `GetProducts()`, para incluir ambos o `CategoryName` e `CompanyName` valores de coluna, que atualizarão o DataTable fortemente tipada para incluir as novas colunas.

Isso pode apresentar um problema, no entanto, como métodos do TableAdapter para inserção, atualização, e exclusão de dados é baseada nesse método inicial. Felizmente, os métodos gerados automaticamente para inserir, atualizar e excluir não são afetados pela subconsultas no `SELECT` cláusula. Tomando cuidado para adicionar nossas consultas para `Categories` e `Suppliers` como subconsultas, em vez de `JOIN` s, iremos evitar ter que refazer esses métodos para modificar dados. Clique com botão direito no `GetProducts()` método no `ProductsTableAdapter` e escolha Configure. Em seguida, ajuste o `SELECT` cláusula para que ele fique, como:

[!code-sql[Main](creating-a-data-access-layer-vb/samples/sample9.sql)]


[![Atualize a instrução SELECT para o método GetProducts()](creating-a-data-access-layer-vb/_static/image80.png)](creating-a-data-access-layer-vb/_static/image79.png)

**Figura 29**: atualizar o `SELECT` instrução para o `GetProducts()` método ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image81.png))


Depois de atualizar o `GetProducts()` método para usar essa consulta nova DataTable inclui duas novas colunas: `CategoryName` e `SupplierName`.


![A DataTable de produtos tem duas novas colunas](creating-a-data-access-layer-vb/_static/image82.png)

**Figura 30**: O `Products` DataTable tem duas novas colunas


Reserve um tempo para atualizar o `SELECT` cláusula no `GetProductsByCategoryID(categoryID)` método também.

Se você atualizar o `GetProducts()` `SELECT` usando `JOIN` sintaxe o Designer de conjunto de dados não será capaz de gerar automaticamente os métodos de inserção, atualização, e exclusão de dados banco de dados usando o DB direcionar padrão. Em vez disso, você precisará criá-las manualmente muito, como fizemos com o `InsertProduct` método neste tutorial. Além disso, manualmente será necessário fornecer o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` valores de propriedade, se você quiser usar o padrão de atualização do lote.

## <a name="adding-the-remaining-tableadapters"></a>Adicionando os TableAdapters restantes

Até agora, vimos apenas trabalhando com um único TableAdapter para uma tabela de banco de dados individual. No entanto, o banco de dados Northwind contém várias tabelas relacionadas que precisaremos para trabalhar com em nosso aplicativo web. Um conjunto de dados tipado pode conter vários, DataTables relacionadas. Portanto, para concluir nossa DAL precisamos adicionar DataTables para as outras tabelas que usaremos nestes tutoriais. Para adicionar um novo TableAdapter para um conjunto de dados tipado, abra o Designer de conjunto de dados, clique com botão direito no Designer e escolha Adicionar / TableAdapter. Isso criará uma nova DataTable e TableAdapter e orientá-lo por meio do assistente, examinamos neste tutorial.

Levar alguns minutos para criar os seguintes TableAdapters e métodos usando as consultas a seguir. Observe que as consultas a `ProductsTableAdapter` incluem subconsultas para capturar os nomes de categoria e fornecedor de cada produto. Além disso, se você estiver acompanhando, você já adicionou o `ProductsTableAdapter` da classe `GetProducts()` e `GetProductsByCategoryID(categoryID)` métodos.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-vb/samples/sample21.sql)]


[![O Designer de conjunto de dados depois que foram adicionadas as quatro TableAdapters](creating-a-data-access-layer-vb/_static/image84.png)](creating-a-data-access-layer-vb/_static/image83.png)

**Figura 31**: O DataSet Designer após os quatro TableAdapters foram adicionados ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Adição de código personalizado para o DAL

Os TableAdapters e DataTables adicionado ao conjunto de dados tipado são expressos como um arquivo de definição de esquema XML (`Northwind.xsd`). Você pode exibir essas informações de esquema clicando com o `Northwind.xsd` no Gerenciador de soluções e escolher exibir código.


[![O arquivo de definição (XSD) de esquema XML para a Northwind Typed Dataset&lt;2}&lt;1}](creating-a-data-access-layer-vb/_static/image87.png)](creating-a-data-access-layer-vb/_static/image86.png)

**Figura 32**: A definição de esquema de XML (XSD) de arquivo para o conjunto de dados tipados do Northwind ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image88.png))


Essas informações de esquema são convertidas em código c# ou Visual Basic em tempo de design, quando compilado ou no tempo de execução (se necessário) no ponto em que você pode percorrê-lo com o depurador. Para exibir esse código gerado automaticamente ir para o modo de exibição e análise para baixo para as classes TableAdapter ou conjunto de dados tipado. Se você não vir o modo de exibição de classe em sua tela, vá para o menu de exibição e selecioná-lo a partir daí ou pressione Ctrl + Shift + C. Do modo de exibição de classe, você pode ver as propriedades, métodos e eventos das classes de conjunto de dados tipado e TableAdapter. Para exibir o código para um método específico, clique duas vezes no nome do método no modo de exibição de classe ou com o botão direito nela e escolher Ir para definição.


![Inspecione o código gerado automaticamente pela seleção de ir para definição do modo de exibição de classe](creating-a-data-access-layer-vb/_static/image89.png)

**Figura 33**: inspecionar o código gerado automaticamente pela seleção de ir para definição do modo de exibição de classe


Embora o código gerado automaticamente pode ser um grande Economizador de tempo, o código geralmente é muito genérico e precisa ser personalizado para atender às necessidades exclusivas de um aplicativo. O risco de estender o código gerado automaticamente, no entanto, é que a ferramenta que gerou o código pode decidir chegou a hora "regenerar" e substituir as suas personalizações. Com o novo conceito de classe parcial do .NET 2.0, é fácil de dividir uma classe em vários arquivos. Isso nos permite adicionar nossos próprios métodos, propriedades e eventos para as classes geradas automaticamente, sem a necessidade de se preocupar sobre o Visual Studio substituir nosso personalizações.

Para demonstrar como personalizar a DAL, vamos adicionar um `GetProducts()` método para o `SuppliersRow` classe. O `SuppliersRow` classe representa um único registro na `Suppliers` tabela; cada provedor de fornecedor pode zero para muitos produtos, portanto, `GetProducts()` retornará aqueles produtos do fornecedor especificado. Para realizar isso crie um novo arquivo de classe na `App_Code` pasta chamada `SuppliersRow.vb` e adicione o seguinte código:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample22.vb)]

Essa classe parcial instrui o compilador que, quando criar o `Northwind.SuppliersRow` classe para incluir o `GetProducts()` método que acabamos de definir. Se você compila seu projeto e, em seguida, retorna para a exibição de classe você verá `GetProducts()` agora está listado como um método de `Northwind.SuppliersRow`.


![O método GetProducts() é agora parte da classe Northwind.SuppliersRow](creating-a-data-access-layer-vb/_static/image90.png)

**Figura 34**: O `GetProducts()` método é agora parte do `Northwind.SuppliersRow` classe


O `GetProducts()` método agora pode ser usado para enumerar o conjunto de produtos para um determinado fornecedor, como mostra o seguinte código:

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample23.vb)]

Esses dados também podem ser exibidos em qualquer uma das ASP. Controles da Web de dados da rede. A página a seguir usa um controle GridView com dois campos:

- Um BoundField que exibe o nome de cada fornecedor, e
- Um TemplateField que contém um controle BulletedList que está associado aos resultados retornados pelo `GetProducts()` método para cada fornecedor.

Vamos examinar como exibir esses relatórios mestre / detalhes em tutoriais futuros. Por enquanto, este exemplo é projetado para ilustrar a usando o método personalizado adicionado para o `Northwind.SuppliersRow` classe.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-vb/samples/sample24.aspx)]

SuppliersAndProducts.aspx.vb

[!code-vb[Main](creating-a-data-access-layer-vb/samples/sample25.vb)]


[![Nome da empresa do fornecedor está listado na coluna esquerda, seus produtos no canto direito](creating-a-data-access-layer-vb/_static/image92.png)](creating-a-data-access-layer-vb/_static/image91.png)

**Figura 35**: nome do fornecedor para o da empresa está listado na coluna esquerda, seus produtos da direita ([clique para exibir a imagem em tamanho normal](creating-a-data-access-layer-vb/_static/image93.png))


## <a name="summary"></a>Resumo

Quando criar um aplicativo web que criar a DAL deve ser uma das suas primeiras etapas que ocorrem antes de começar a criar sua camada de apresentação. Com o Visual Studio, criar uma DAL baseada em conjuntos de dados tipados é uma tarefa que pode ser realizada de 10 a 15 minutos sem escrever uma linha de código. Os tutoriais avançando se baseará esse DAL. No [próximo tutorial](creating-a-business-logic-layer-vb.md) vamos definir um número de regras de negócio e veja como implementá-los em uma camada de lógica de negócios separada.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Criando uma DAL usando os TableAdapters fortemente tipados e DataTables no VS 2005 e o ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Criando componentes de camada de dados e transmissão de dados por meio das camadas](https://msdn.microsoft.com/library/ms978496.aspx)
- [Criar uma camada de acesso a dados com o Visual Studio 2005 DataSet Designer](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Criptografando informações de configuração no ASP.NET 2.0 Applications](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Visão geral de TableAdapter](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Trabalhando com um DataSet tipado](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Usando o acesso de dados fortemente tipados no Visual Studio 2005 e no ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Como estender os métodos do TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Recuperação de dados escalar de um procedimento armazenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Treinamento em vídeo sobre os tópicos contidos neste tutorial

- [Camadas de Acesso a Dados em aplicativos do ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Como associar manualmente um conjunto de dados a um Datagrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Como trabalhar com conjuntos de dados e filtros de um aplicativo ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Ron Green, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez e Carlos Santos. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-pages-and-site-navigation-cs.md)
> [Próximo](creating-a-business-logic-layer-vb.md)
