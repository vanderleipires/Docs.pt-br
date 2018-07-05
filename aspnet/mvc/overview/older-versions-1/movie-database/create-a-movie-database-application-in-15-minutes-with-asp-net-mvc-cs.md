---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: Criar um aplicativo de banco de dados de filme em 15 minutos com o ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther compila um controlado por banco de dados ASP.NET MVC aplicativo inteiro do início ao fim. Este tutorial é uma excelente introdução para as pessoas que são nova t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: 2cf41eb0622d032578a77dd2d3f7427b244b2e29
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395861"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a>Criar um aplicativo de banco de dados de filme em 15 minutos com o ASP.NET MVC (c#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Baixar o código](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> Stephen Walther compila um controlado por banco de dados ASP.NET MVC aplicativo inteiro do início ao fim. Este tutorial é uma excelente introdução para as pessoas que são novos para o ASP.NET MVC Framework e que desejam ter uma ideia do processo de criação de um aplicativo ASP.NET MVC.


O objetivo deste tutorial é para lhe dar uma noção de "como é" para criar um aplicativo ASP.NET MVC. Neste tutorial, posso utilizar ao máximo o por meio da criação de um aplicativo do ASP.NET MVC inteiro do início ao fim. Vou mostrar como criar um aplicativo controlado por banco de dados simples que ilustra como você pode listar, criar e editar registros de banco de dados.

Para simplificar o processo de criação de nosso aplicativo, podemos aproveitar os recursos de scaffolding do Visual Studio 2008. Apresentaremos Visual Studio gerar o código inicial e o conteúdo para nosso controladores, modelos e modos de exibição.

Se você tiver trabalhado com páginas ASP ou ASP.NET, em seguida, você deve encontrar ASP.NET MVC bastante familiar. Exibições do ASP.NET MVC são muito parecido com as páginas em um aplicativo Active Server Pages. E, assim como um aplicativo Web Forms do ASP.NET tradicional, ASP.NET MVC oferece a você com acesso total a um conjunto sofisticado de linguagens e classes fornecidas pelo .NET framework.

Minha esperança é que este tutorial lhe dará uma ideia de como a experiência de criação de um aplicativo ASP.NET MVC é diferente da experiência de criação de um aplicativo de páginas ASP ou ASP.NET Web Forms e semelhantes.

## <a name="overview-of-the-movie-database-application"></a>Visão geral do aplicativo de banco de dados do filme

Como nosso objetivo é manter as coisas simples, vamos criar um aplicativo muito simples de banco de dados do filme. Nosso aplicativo de banco de dados de filme simple, poderemos fazer três coisas:

1. Listar um conjunto de registros de banco de dados do filme
2. Criar um novo registro de banco de dados do filme
3. Editar um registro existente de banco de dados do filme

Novamente, porque queremos manter as coisas simples, vamos dar aproveitar o número mínimo de recursos do ASP.NET MVC framework necessárias para compilar nosso aplicativo. Por exemplo, podemos não estar aproveitando desenvolvimento orientado por teste.

Para criar nosso aplicativo, é necessário concluir cada uma das seguintes etapas:

1. Criar o projeto de aplicativo Web ASP.NET MVC
2. Criar o banco de dados
3. Criar o modelo de banco de dados
4. Criar o controlador do ASP.NET MVC
5. Criar modos de exibição de ASP.NET MVC

## <a name="preliminaries"></a>Etapas preliminares

Será necessário o Visual Studio 2008 ou o Visual Web Developer 2008 Express para criar um aplicativo ASP.NET MVC. Você também precisará baixar o ASP.NET MVC framework.

Se você não tem o Visual Studio 2008, você pode baixar uma versão de avaliação de 90 dias do Visual Studio 2008 neste site:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Como alternativa, você pode criar o ASP.NET MVC aplicativos com o Visual Web Developer Express 2008. Se você decidir usar o Visual Web Developer Express, em seguida, você deve ter o Service Pack 1 instalado. Você pode baixar o Visual Web Developer 2008 Express com Service Pack 1 neste site:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp; displaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Depois de instalar o Visual Studio 2008 ou o Visual Web Developer 2008, você precisará instalar a estrutura ASP.NET MVC. Você pode baixar o ASP.NET MVC framework do seguinte site:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Em vez de baixar o ASP.NET framework e o ASP.NET MVC framework individualmente, você pode aproveitar o Web Platform Installer. O Web Platform Installer é um aplicativo que permite que você gerencie facilmente os aplicativos instalados são seu computador:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Criar um projeto de aplicativo Web ASP.NET MVC

Vamos começar criando um novo projeto de aplicativo Web ASP.NET MVC no Visual Studio 2008. Selecione a opção de menu **arquivo, novo projeto** e você verá a caixa de diálogo Novo projeto na Figura 1. Selecione c# como linguagem de programação e selecione o modelo de projeto de aplicativo Web ASP.NET MVC. Dê o nome MovieApp ao seu projeto e clique no botão Okey.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)

**Figura 01**: caixa de diálogo New Project ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))


Certifique-se de que você selecionar o .NET Framework 3.5 na lista suspensa na parte superior da caixa de diálogo Novo projeto ou o modelo de projeto de aplicativo Web ASP.NET MVC não aparecerá.


Sempre que você cria um novo projeto de aplicativo Web MVC, o Visual Studio solicita que você criar um projeto de teste de unidade separada. Na Figura 2 a caixa de diálogo é exibida. Como não criaremos testes neste tutorial devido a restrições de tempo (e, Sim, podemos deve se sentir um pouco culpados por isso) selecione a **nenhuma** opção e clique no **Okey** botão.

> [!NOTE] 
> 
> O Visual Web Developer não oferece suporte a projetos de teste.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)

**Figura 02**: caixa de diálogo do projeto de teste de unidade de criar a ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))


Um aplicativo ASP.NET MVC tem um conjunto padrão de pastas: uma pasta de modelos, exibições e controladores. Você pode ver este conjunto padrão de pastas na janela do Gerenciador de soluções. Precisaremos adicionar arquivos a cada uma das pastas de modelos, exibições e controladores a fim de criar nosso aplicativo de banco de dados do filme.

Quando você cria um novo aplicativo MVC com o Visual Studio, você pode obter um exemplo de aplicativo. Como queremos começar do zero, é preciso excluir o conteúdo para este aplicativo de exemplo. Você precisa excluir o arquivo a seguir e a seguinte pasta:

- Controllers\HomeController.cs
- Views\Home

## <a name="creating-the-database"></a>Criando o banco de dados

Precisamos criar um banco de dados para manter nossos registros de banco de dados do filme. Felizmente, o Visual Studio inclui um banco de dados gratuito chamado SQL Server Express. Siga estas etapas para criar o banco de dados:

1. O aplicativo com o botão direito\_pasta de dados na janela do Gerenciador de soluções e selecione a opção de menu **adicionar, Item novo**.
2. Selecione o **dados** categoria e selecione o **banco de dados do SQL Server** modelo (consulte a Figura 3).
3. Nomeie seu novo banco de dados *MoviesDB.mdf* e clique no **Add** botão.

Depois de criar seu banco de dados, você pode se conectar ao banco de dados clicando no arquivo MoviesDB.mdf localizado no aplicativo do\_pasta de dados. Duas vezes no arquivo MoviesDB.mdf abre a janela do Gerenciador de servidores.

> [!NOTE] 
> 
> A janela do Gerenciador de servidores é chamada a janela do Gerenciador de banco de dados no caso do Visual Web Developer.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)

**Figura 03**: criar um banco de dados do Microsoft SQL Server ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))


Em seguida, precisamos criar uma nova tabela de banco de dados. De dentro da janela do Gerenciador do servidor, clique com botão direito na pasta tabelas e selecione a opção de menu **adicionar nova tabela**. Selecionar essa opção de menu abre o designer de tabela do banco de dados. Crie as seguintes colunas de banco de dados:

<a id="0.1_table01"></a>


| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | int | False |
| Título | Nvarchar(100) | False |
| Diretor | Nvarchar(100) | False |
| DateReleased | DateTime | False |


A primeira coluna, a coluna de Id tem duas propriedades especiais. Primeiro, você precisa marcar a coluna de Id como a coluna de chave primária. Depois de selecionar a coluna de Id, clique no **definir chave primária** botão (é o ícone que se parece com uma chave). Em segundo lugar, você precisa marcar a coluna de Id como uma coluna de identidade. Na janela Propriedades da coluna, role para baixo até a seção de especificação de identidade e expandi-lo. Alterar o **é identidade** propriedade para o valor **Sim**. Quando tiver terminado, a tabela deve ter aparência semelhante à Figura 4.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)

**Figura 04**: tabela de banco de dados de filmes a ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))


A etapa final é salvar a nova tabela. Clique no botão de salvar (o ícone de disquete) e dê a nova tabela de filmes de nome.

Depois de concluir a criação da tabela, adicione alguns registros de filmes para a tabela. A tabela de filmes na janela do Gerenciador de servidores com o botão direito e selecione a opção de menu **Mostrar dados da tabela**. Insira uma lista de seus filmes favoritos (consulte a Figura 5).


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)

**Figura 05**: inserir registros de filme ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))


## <a name="creating-the-model"></a>Criando o modelo

Em seguida, precisamos criar um conjunto de classes para representar o nosso banco de dados. É necessário criar um modelo de banco de dados. Podemos aproveitar o Microsoft Entity Framework para gerar as classes para nosso modelo de banco de dados automaticamente.

> [!NOTE] 
> 
> O ASP.NET MVC framework não está ligado à Microsoft Entity Framework. Você pode criar classes de modelo de seu banco de dados, tirando proveito de uma variedade de mapeamento relacional de objeto (ou / M) ferramentas, incluindo o LINQ to SQL, o Subsonic e NHibernate.


Siga estas etapas para iniciar o Assistente de modelo de dados de entidade:

1. Clique com botão direito na pasta de modelos na janela do Gerenciador de soluções e selecione a opção de menu **adicionar, Item novo**.
2. Selecione o **dados** categoria e selecione o **modelo de dados de entidade ADO.NET** modelo.
3. Dê o nome de seu modelo de dados *MoviesDBModel.edmx* e clique no **Add** botão.

Depois de clicar no botão Add, o Assistente de modelo de dados de entidade é exibida (veja a Figura 6). Siga estas etapas para concluir o Assistente:

1. No **escolher conteúdo do modelo** etapa, selecione o **gerar a partir do banco de dados** opção.
2. No **escolha sua Conexão de dados** etapa, use o *MoviesDB.mdf* conexão de dados e o nome *MoviesDBEntities* para as configurações de conexão. Clique o **próxima** botão.
3. No **Choose Your Database Objects** etapa, expanda o nó Tables, selecione a tabela de filmes. Insira o namespace *MovieApp.Models* e clique no **concluir** botão.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)

**Figura 06**: gerar um modelo de banco de dados com o Assistente de modelo de dados de entidade ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))


Depois de concluir o Assistente de modelo de dados de entidade, o Designer de modelo de dados de entidade é aberto. O Designer deve exibir a tabela de banco de dados de filmes (veja a Figura 7).


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)

**Figura 07**: O Designer de modelo de dados de entidade ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))


Precisamos fazer uma alteração antes de continuar. O Assistente de dados de entidade gera uma classe de modelo chamada *filmes* que representa a tabela de banco de dados de filmes. Como vamos usar a classe de filmes para representar um filme específico, precisamos modificar o nome da classe para ser *filme* em vez de *filmes* (singular em vez de no plural).

Duas vezes no nome da classe na superfície do designer e altere o nome da classe de filmes para filme. Após fazer essa alteração, clique o **salvar** botão (o ícone de disquete) para gerar a classe de filme.

## <a name="creating-the-aspnet-mvc-controller"></a>Criando o controlador do ASP.NET MVC

A próxima etapa é criar o controlador MVC do ASP.NET. Um controlador é responsável por controlar como um usuário interage com um aplicativo ASP.NET MVC.

Siga estas etapas:

1. Na janela Gerenciador de soluções, clique com botão direito na pasta controladores e selecione a opção de menu **Add, controlador**.
2. Na caixa de diálogo Adicionar controlador, digite o nome *HomeController* e marque a caixa de seleção **adicionar métodos de ação para criar, atualizar e detalhes cenários** (consulte a Figura 8).
3. Clique o **adicionar** botão para adicionar o novo controlador ao seu projeto.

Depois de concluir essas etapas, o controlador na listagem 1 é criado. Observe que ele contém métodos chamados de índice, detalhes, criar e editar. As seções a seguir, adicionaremos o código necessário para obter esses métodos funcionem.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)

**Figura 08**: adicionar um novo controlador de MVC do ASP.NET ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))


**Listagem 1 – Controllers\HomeController.cs**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a>Listagem de registros de banco de dados

O método Index () do controlador inicial é o método padrão para um aplicativo ASP.NET MVC. Quando você executa um aplicativo ASP.NET MVC, o método Index () é o primeiro método de controlador é chamado.

Usaremos o método Index () para exibir a lista de registros da tabela de banco de dados de filmes. Podemos aproveitar o banco de dados classes de modelo que criamos anteriormente para recuperar os registros do banco de dados de filmes com o método Index ().

Eu modifiquei a classe HomeController na listagem 2 para que ele contenha um novo campo privado chamado \_db. A classe MoviesDBEntities representa nosso modelo de banco de dados e vamos usar essa classe para se comunicar com o nosso banco de dados.

Eu também modifiquei o método Index () na listagem 2. O método Index () usa a classe MoviesDBEntities para recuperar todos os registros de filme da tabela de banco de dados de filmes. A expressão  *\_db. MovieSet.ToList()* retorna uma lista de todos os registros de filme da tabela de banco de dados de filmes.

A lista de filmes é passada para o modo de exibição. Tudo o que é passado para o método View() é passado para o modo de exibição como dados de exibição.

**Listagem 2 – Controllers (método de índice modificado)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

O método Index () retorna uma exibição nomeada de índice. É necessário criar essa exibição para exibir a lista de registros de banco de dados do filme. Siga estas etapas:


Você deve compilar o projeto (selecione a opção de menu **construir, compilar solução**) antes de abrir o **adicionar exibição** sem classes ou caixa de diálogo aparecerá no **exibir dados de classe** lista suspensa.


1. O método Index () no editor de códigos com o botão direito e selecione a opção de menu **adicionar exibição** (veja a Figura 9).
2. Na caixa de diálogo Adicionar modo de exibição, verifique se que a caixa de seleção rotulada **criar uma exibição fortemente tipada** é verificada.
3. Dos **exibir conteúdo** lista suspensa, selecione o valor *lista*.
4. Dos **exibir dados de classe** lista suspensa, selecione o valor *MovieApp.Models.Movie*.
5. Clique no botão Adicionar para criar a nova exibição (consulte a Figura 10).

Depois de concluir essas etapas, uma nova exibição chamada aspx é adicionada à pasta Views\Home. O conteúdo da exibição índice é incluído na listagem 3.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)

**Figura 09**: adicionando uma exibição de uma ação do controlador ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)

**Figura 10**: criar uma nova exibição com a caixa de diálogo Adicionar modo de exibição ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))


**Listagem 3 – Views\Home\Index.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

A exibição de índice exibe todos os registros de filme da tabela de banco de dados de filmes dentro de uma tabela HTML. O modo de exibição contém um loop foreach que itera por meio de cada filme representado pela propriedade ViewData.Model. Se você executar o aplicativo pressionando a tecla F5, você verá a página da web na Figura 11.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)

**Figura 11**: exibição o índice ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))


## <a name="creating-new-database-records"></a>Criação de novos registros de banco de dados

O modo de exibição de índice que criamos na seção anterior inclui um link para a criação de novos registros de banco de dados. Vamos prosseguir e implementar a lógica e criar o modo de exibição necessárias para a criação de novos registros de banco de dados do filme.

O controlador Home contém dois métodos chamados Create (). O primeiro método Create () não tem parâmetros. Essa sobrecarga do método Create () é usada para exibir o formulário HTML para criar um novo registro de banco de dados do filme.

O segundo método Create () tem um parâmetro de FormCollection. Essa sobrecarga do método Create () é chamada quando o formulário HTML para a criação de um novo filme é postado no servidor. Observe que esse segundo método Create () tem um atributo AcceptVerbs que impede que o método que está sendo chamado, a menos que uma operação HTTP POST é executada.

Esse segundo método Create () foi modificado na classe HomeController atualizado na listagem 4. A nova versão do método Create () aceita um parâmetro de filme e contém a lógica para inserir um novo filme na tabela de banco de dados de filmes.

> [!NOTE] 
> 
> Observe o atributo Bind. Como não queremos atualizar a propriedade de Id de filme de formulário HTML, é preciso excluir explicitamente essa propriedade.


**Listagem 4 – Controllers\HomeController.cs (método de criação modificado)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

Visual Studio torna fácil criar o formulário para criar um novo banco de dados do filme registrar (veja a Figura 12). Siga estas etapas:

1. O método Create () no editor de códigos com o botão direito e selecione a opção de menu **adicionar exibição**.
2. Verifique se que a caixa de seleção rotulada **criar uma exibição fortemente tipada** é verificada.
3. Dos **exibir conteúdo** lista suspensa, selecione o valor *criar*.
4. Dos **exibir dados de classe** lista suspensa, selecione o valor *MovieApp.Models.Movie*.
5. Clique o **adicionar** botão para criar o novo modo de exibição.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)

**Figura 12**: adição da exibição Create ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))


Visual Studio gera automaticamente o modo de exibição na listagem 5. Essa exibição contém um formulário HTML que inclui campos que correspondem a cada uma das propriedades da classe filme.

**Listagem 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> O formulário HTML gerado pela caixa de diálogo Adicionar modo de exibição gera um campo de formulário de Id. Como a coluna Id é uma coluna de identidade, não precisamos este campo de formulário e você poderá removê-lo com segurança.


Depois de adicionar o Create view, você pode adicionar novos registros de filme ao banco de dados. Execute seu aplicativo pressionando a tecla F5 e clique no link Criar novo para ver o formulário na Figura 13. Se você concluir e enviar o formulário, é criado um novo registro de banco de dados do filme.

Observe que você obtenha a validação do formulário automaticamente. Se você não inserir uma data de lançamento de um filme ou se você inserir uma data de lançamento inválido, em seguida, o formulário é reexibido e o campo de data de lançamento é realçado.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)

**Figura 13**: criar um novo registro de banco de dados de filme ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))


## <a name="editing-existing-database-records"></a>Editando registros existentes do banco de dados

Nas seções anteriores, discutimos como você pode listar e criar novos registros de banco de dados. Nesta seção final, discutiremos como você pode editar os registros existentes do banco de dados.

Primeiro, é necessário gerar o formulário de edição. Esta etapa é fácil, pois o Visual Studio irá gerar o formulário de edição para nós automaticamente. Abra a classe HomeController.cs no editor de código do Visual Studio e siga estas etapas:

1. O método Edit () no editor de códigos com o botão direito e selecione a opção de menu **adicionar exibição** (veja a Figura 14).
2. Marque a caixa de seleção rotulada **criar uma exibição fortemente tipada**.
3. Dos **exibir conteúdo** lista suspensa, selecione o valor *editar*.
4. Dos **exibir dados de classe** lista suspensa, selecione o valor *MovieApp.Models.Movie*.
5. Clique o **adicionar** botão para criar o novo modo de exibição.

Concluir essas etapas adiciona uma nova exibição chamada Edit para a pasta Views\Home. Essa exibição contém um formulário HTML para editar um registro de filme.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)

**Figura 14**: adicionar a exibição de edição ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))


> [!NOTE] 
> 
> A exibição de edição contém um campo de formulário HTML que corresponde à propriedade de Id de filme. Porque você não deseja que pessoas editando o valor da propriedade Id, você deve remover esse campo de formulário.


Por fim, precisamos modificar o controlador Home para que ele dá suporte à edição de um registro de banco de dados. A classe HomeController atualizada está contida na listagem 6.

**Listagem 6 – Controllers\HomeController.cs (métodos de edição)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

Na listagem 6, adicionei lógica adicional para ambas as sobrecargas do método Edit (). O primeiro método Edit () retorna o registro de banco de dados do filme que corresponde ao parâmetro de Id passado para o método. A segunda sobrecarga executa as atualizações para um registro de filme no banco de dados.

Observe que você deve recuperar o filme original e, em seguida, chame ApplyPropertyChanges() para atualizar o filme existente no banco de dados.

## <a name="summary"></a>Resumo

A finalidade deste tutorial era lhe dar uma noção da experiência de criação de um aplicativo ASP.NET MVC. Espero que você descobriu que criar aplicativo web de ASP.NET MVC é muito semelhante à experiência de criação de um aplicativo de páginas ASP ou ASP.NET.

Neste tutorial, examinamos apenas os recursos mais básicos da estrutura MVC do ASP.NET. Em tutoriais futuros, podemos mergulhar mais fundo tópicos como controladores, ações do controlador, exibições, exibir dados e auxiliares HTML.

> [!div class="step-by-step"]
> [Avançar](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
