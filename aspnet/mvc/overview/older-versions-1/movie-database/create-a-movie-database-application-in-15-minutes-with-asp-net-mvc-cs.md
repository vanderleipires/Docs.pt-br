---
uid: mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
title: Criar um aplicativo de banco de dados do filme em 15 minutos com o ASP.NET MVC (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther cria um inteiro para banco de dados ASP.NET MVC aplicativo a partir do início ao fim. Este tutorial é uma introdução excelente para pessoas t novo...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: dd1be137-91c5-47a8-8137-fecf0789c7f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/movie-database/create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs
msc.type: authoredcontent
ms.openlocfilehash: 81e0ae42bc3e7656c933ba70920eaeeffa4c4bd6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877028"
---
<a name="create-a-movie-database-application-in-15-minutes-with-aspnet-mvc-c"></a>Criar um aplicativo de banco de dados do filme em 15 minutos com o ASP.NET MVC (c#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

[Baixar o código](http://download.microsoft.com/download/7/2/8/728F8794-E59A-4D18-9A56-7AD2DB05BD9D/MovieApp_CS.zip)

> Stephen Walther cria um inteiro para banco de dados ASP.NET MVC aplicativo a partir do início ao fim. Este tutorial é uma introdução excelente para pessoas que são novos para a estrutura do ASP.NET MVC e que desejam ter uma ideia do processo de criação de um aplicativo ASP.NET MVC.


O objetivo deste tutorial é dar uma ideia de "o que é como" para criar um aplicativo ASP.NET MVC. Neste tutorial, posso utilizar ao máximo na criação de um aplicativo ASP.NET MVC inteiro do início ao fim. Vou mostrar como criar um aplicativo simples para banco de dados que ilustra como você pode listar, criar e editar registros de banco de dados.

Para simplificar o processo de criação de nosso aplicativo, vamos dar aproveitar os recursos de scaffolding do Visual Studio 2008. Vamos Visual Studio gere o código inicial e o conteúdo para nosso controladores, modelos e modos de exibição.

Se você trabalhou com o Active Server Pages ou o ASP.NET, em seguida, você deve encontrar ASP.NET MVC muito familiar. Modos de exibição do ASP.NET MVC são muito parecido com as páginas em um aplicativo de Active Server Pages. E, assim como um aplicativo de Web Forms do ASP.NET tradicional, o ASP.NET MVC fornece com acesso total a um conjunto sofisticado de idiomas e classes fornecidas pelo .NET framework.

Espero é que este tutorial lhe dará uma ideia de como a experiência de criação de um aplicativo ASP.NET MVC é diferente de experiência de criação de um aplicativo Web Forms do ASP.NET ou de Active Server Pages e semelhantes.

## <a name="overview-of-the-movie-database-application"></a>Visão geral do aplicativo de banco de dados do filme

Como nosso objetivo é manter as coisas simples, criaremos um aplicativo de banco de dados do filme muito simples. Nosso aplicativo de banco de dados do filme simple nos permitirá executar três ações:

1. Listar um conjunto de registros de banco de dados do filme
2. Criar um novo registro de banco de dados do filme
3. Editar um registro de banco de dados existente do filme

Novamente, como queremos manter as coisas simples, daremos aproveitar o número mínimo de recursos de estrutura do ASP.NET MVC necessárias para compilar nosso aplicativo. Por exemplo, podemos não aproveitando Driven Development.

Para criar o nosso aplicativo, precisamos concluir cada uma das seguintes etapas:

1. Criar o projeto de aplicativo Web ASP.NET MVC
2. Criar o banco de dados
3. Criar o modelo de banco de dados
4. Criar o controlador do ASP.NET MVC
5. Criar exibições de ASP.NET MVC

## <a name="preliminaries"></a>Etapas preliminares

Será necessário o Visual Studio 2008 ou Visual Web Developer 2008 Express para criar um aplicativo ASP.NET MVC. Você também precisa baixar a estrutura ASP.NET MVC.

Se você não tiver o Visual Studio 2008, você pode baixar uma versão de avaliação de 90 dias do Visual Studio 2008 este site:

[https://msdn.microsoft.com/vs2008/products/cc268305.aspx](https://msdn.microsoft.com/vs2008/products/cc268305.aspx)

Como alternativa, você pode criar o ASP.NET MVC aplicativos com o Visual Web Developer Express 2008. Se você decidir usar o Visual Web Developer Express, em seguida, você deve ter o Service Pack 1 instalado. Você pode baixar o Visual Web Developer 2008 Express com Service Pack 1 do site:

[https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en](https://www.microsoft.com/downloads/details.aspx?FamilyId=BDB6391C-05CA-4036-9154-6DF4F6DEBD14&amp;displaylang=en)

Depois de instalar o Visual Studio 2008 ou o Visual Web Developer 2008, você precisa instalar a estrutura ASP.NET MVC. Você pode baixar a estrutura ASP.NET MVC do seguinte site:

[https://www.asp.net/mvc/](../../../index.md)

> [!NOTE] 
> 
> Em vez de baixar a estrutura do ASP.NET e a estrutura ASP.NET MVC individualmente, você pode aproveitar o Web Platform Installer. O Web Platform Installer é um aplicativo que permite que você gerencie facilmente os aplicativos instalados são seu computador:
> 
> [https://www.microsoft.com/web/gallery/Install.aspx](https://www.microsoft.com/web/gallery/Install.aspx)


## <a name="creating-an-aspnet-mvc-web-application-project"></a>Criando um projeto de aplicativo Web ASP.NET MVC

Vamos começar criando um novo projeto de aplicativo Web ASP.NET MVC no Visual Studio 2008. Selecione a opção de menu **arquivo, novo projeto** e você verá a caixa de diálogo Novo projeto na Figura 1. Selecione c# como linguagem de programação e selecione o modelo de projeto de aplicativo Web ASP.NET MVC. Nomeie o projeto a MovieApp e clique no botão Okey.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image1.png)

**Figura 01**: caixa de diálogo Novo projeto a ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.png))


Certifique-se de que você selecione o .NET Framework 3.5 na lista suspensa na parte superior da caixa de diálogo Novo projeto ou o modelo de projeto de aplicativo Web ASP.NET MVC não aparecerá.


Sempre que você criar um novo projeto de aplicativo Web MVC, o Visual Studio solicitará que você crie um projeto de teste de unidade separada. A caixa de diálogo na Figura 2 é exibida. Como podemos não criando testes neste tutorial devido a restrições de tempo (e, Sim, podemos deve ser um pouco culpados sobre isso) selecione o **não** opção e clique no **Okey** botão.

> [!NOTE] 
> 
> O Visual Web Developer não oferece suporte a projetos de teste.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image2.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.png)

**Figura 02**: caixa de diálogo a criar projeto de teste de unidade ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.png))


Um aplicativo ASP.NET MVC tem um conjunto de pastas padrão: uma pasta de modelos, exibições e controladores. Você pode ver esse conjunto padrão de pastas na janela do Gerenciador de soluções. Precisaremos adicionar arquivos para cada uma das pastas modelos, exibições e os controladores para compilar nosso aplicativo de banco de dados do filme.

Quando você cria um novo aplicativo MVC com o Visual Studio, você pode obter um exemplo de aplicativo. Como queremos começar do zero, é preciso excluir o conteúdo para este aplicativo de exemplo. Você precisa excluir o arquivo a seguir e a pasta a seguir:

- Controllers\HomeController.cs
- Views\Home

## <a name="creating-the-database"></a>Criando o banco de dados

É preciso criar um banco de dados para manter nossos registros de banco de dados do filme. Felizmente, o Visual Studio inclui um banco de dados gratuito chamado SQL Server Express. Siga estas etapas para criar o banco de dados:

1. Clique com botão direito do aplicativo\_pasta de dados na janela do Gerenciador de soluções e selecione a opção de menu **adicionar, o novo Item**.
2. Selecione o **dados** categoria e selecione o **o banco de dados do SQL Server** modelo (consulte a Figura 3).
3. Nome do novo banco de dados *MoviesDB.mdf* e clique no **adicionar** botão.

Depois de criar seu banco de dados, você pode se conectar ao banco de dados clicando no arquivo MoviesDB.mdf localizado no aplicativo do\_pasta de dados. Duas vezes no arquivo MoviesDB.mdf abre a janela do Gerenciador de servidores.

> [!NOTE] 
> 
> A janela do Gerenciador de servidores é chamada a janela do Gerenciador de banco de dados no caso do Visual Web Developer.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image3.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.png)

**Figura 03**: Criando um banco de dados do Microsoft SQL Server ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.png))


Em seguida, é preciso criar uma nova tabela de banco de dados. De dentro da janela do Gerenciador do servidor, clique na pasta de tabelas e selecione a opção de menu **adicionar nova tabela**. Selecionar essa opção de menu abre o designer de tabela do banco de dados. Crie as seguintes colunas de banco de dados:

<a id="0.1_table01"></a>


| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | Int | False |
| Título | Nvarchar(100) | False |
| Diretor | Nvarchar(100) | False |
| DateReleased | DateTime | False |


A primeira coluna, a coluna de Id tem duas propriedades especiais. Primeiro, você precisa marcar a coluna Id como a coluna de chave primária. Depois de selecionar a coluna de Id, clique no **definir chave primária** botão (é o ícone que se parece com uma chave). Em segundo lugar, você precisa marcar a coluna Id como uma coluna de identidade. Na janela de propriedades da coluna, role até a seção de especificação de identidade e expandi-lo. Alterar o **é identidade** propriedade para o valor **Sim**. Quando tiver terminado, a tabela deve parecer com a Figura 4.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image4.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.png)

**Figura 04**: tabela de banco de dados de filmes a ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.png))


A etapa final é salvar a nova tabela. Clique no botão Salvar (o ícone de disquete) e dê a nova tabela de filmes de nome.

Depois de terminar de criar a tabela, adicione alguns registros de filme à tabela. A tabela de filmes na janela do Gerenciador de servidores e selecione a opção de menu **Mostrar dados da tabela**. Insira uma lista de filmes favoritos (consulte a Figura 5).


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image5.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.png)

**Figura 05**: inserindo registros de filme ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.png))


## <a name="creating-the-model"></a>Criando o modelo

Em seguida, precisamos criar um conjunto de classes para representar o nosso banco de dados. É preciso criar um modelo de banco de dados. Vamos dar vantagem da Microsoft Entity Framework para gerar as classes para nosso modelo de banco de dados automaticamente.

> [!NOTE] 
> 
> A estrutura MVC do ASP.NET não está ligada à Microsoft Entity Framework. Você pode criar classes de modelo de seu banco de dados, tirando proveito de uma variedade de mapeamento relacional de objeto (ou / M) incluindo LINQ to SQL, Subsonic e NHibernate de ferramentas.


Siga estas etapas para iniciar o Assistente de modelo de dados de entidade:

1. Clique na pasta de modelos na janela do Gerenciador de soluções e selecione a opção de menu **adicionar, o novo Item**.
2. Selecione o **dados** categoria e selecione o **modelo de dados de entidade ADO.NET** modelo.
3. Forneça o nome de seu modelo de dados *MoviesDBModel.edmx* e clique no **adicionar** botão.

Depois de clicar no botão Adicionar, o Assistente de modelo de dados de entidade é exibida (veja a Figura 6). Siga estas etapas para concluir o Assistente:

1. No **escolher o modelo de conteúdo** etapa, selecione o **gerar do banco de dados** opção.
2. No **escolha sua Conexão de dados** etapa, use o *MoviesDB.mdf* conexão de dados e o nome *MoviesDBEntities* para as configurações de conexão. Clique o **próximo** botão.
3. No **escolher seus objetos de banco de dados** etapa, expanda o nó tabelas, selecione a tabela de filmes. Insira o namespace *MovieApp.Models* e clique no **concluir** botão.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image6.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.png)

**Figura 06**: gerar um modelo de banco de dados com o Assistente de modelo de dados de entidade ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.png))


Depois de concluir o Assistente de modelo de dados de entidade, abre o Designer de modelo de dados de entidade. O Designer deve exibir a tabela de banco de dados de filmes (consulte a Figura 7).


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image7.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.png)

**Figura 07**: O Designer de modelo de dados de entidade ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.png))


É preciso fazer uma alteração antes de continuar. O Assistente de dados de entidade gera uma classe de modelo chamada *filmes* que representa a tabela de banco de dados de filmes. Como vamos usar a classe de filmes para representar um filme específico, é preciso modificar o nome da classe a ser *filme* em vez de *filmes* (singular em vez de no plural).

Clique duas vezes no nome da classe na superfície do designer e alterar o nome da classe de filmes para filme. Depois de fazer essa alteração, clique no **salvar** botão (o ícone de disquete) para gerar a classe de filme.

## <a name="creating-the-aspnet-mvc-controller"></a>Criando o controlador MVC do ASP.NET

A próxima etapa é criar o controlador do ASP.NET MVC. Um controlador é responsável por controlar como um usuário interage com um aplicativo ASP.NET MVC.

Siga estas etapas:

1. Na janela do Gerenciador de soluções, clique na pasta controladores e selecione a opção de menu **adicionar, controlador**.
2. Na caixa de diálogo Adicionar controlador, digite o nome *HomeController* e marque a caixa de seleção **adicionar métodos de ação para criar, atualizar e detalhes cenários** (consulte a Figura 8).
3. Clique o **adicionar** botão para adicionar o novo controlador para seu projeto.

Depois de concluir essas etapas, o controlador na listagem 1 é criado. Observe que ele contém métodos chamados de índice, detalhes, criar e editar. As seções a seguir, adicionaremos o código necessário para obter esses métodos funcionem.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image8.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image15.png)

**Figura 08**: adicionando um novo controlador de MVC do ASP.NET ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image16.png))


**Listando 1 – Controllers\HomeController.cs**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample1.cs)]

## <a name="listing-database-records"></a>Registros de banco de dados de lista

O método Index do controlador inicial é o método padrão para um aplicativo ASP.NET MVC. Quando você executa um aplicativo ASP.NET MVC, o método Index () é o primeiro método de controlador que é chamado.

Usaremos o método Index () para exibir a lista de registros da tabela de banco de dados de filmes. Vamos dar aproveitar o banco de dados de classes de modelo que criamos anteriormente para recuperar os registros de banco de dados do filme com o método Index ().

Eu tenha modificado a classe HomeController na listagem 2 para que ele contém um novo campo particular chamado \_banco de dados. A classe MoviesDBEntities representa nosso modelo de banco de dados e vamos usar essa classe para se comunicar com nosso banco de dados.

Eu também tenha modificado o método Index () na listagem 2. O método Index () usa a classe MoviesDBEntities para recuperar todos os registros de filme da tabela de banco de dados de filmes. A expressão  *\_banco de dados. MovieSet.ToList()* retorna uma lista de todos os registros de filme da tabela de banco de dados de filmes.

A lista de filmes é passada para o modo de exibição. Tudo o que é passada para o método View() é passado para o modo de exibição como dados de exibição.

**A listagem 2 – Controllers/HomeController.cs (método de índice modificado)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample2.cs)]

O método Index () retorna uma exibição chamada índice. É necessário criar essa exibição para exibir a lista de registros de banco de dados do filme. Siga estas etapas:


Você deve compilar o projeto (selecione a opção de menu **criar, compilar solução**) antes de abrir o **adicionar modo de exibição** caixa de diálogo ou nenhuma classe será exibida no **exibir dados de classe** lista suspensa.


1. O método Index () no editor de código e selecione a opção de menu **adicionar exibição** (consulte a Figura 9).
2. Na caixa de diálogo Adicionar modo de exibição, verifique se que a caixa de seleção rotulada **criar uma exibição fortemente tipada** é verificada.
3. Do **exibir conteúdo** lista suspensa, selecione o valor *lista*.
4. Do **exibir dados de classe** lista suspensa, selecione o valor *MovieApp.Models.Movie*.
5. Clique no botão Adicionar para criar a nova exibição (consulte a Figura 10).

Depois de concluir essas etapas, uma nova exibição chamada Index.aspx é adicionada à pasta Views\Home. O conteúdo da exibição de índice é incluído na listagem 3.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image9.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image17.png)

**Figura 09**: adicionando uma exibição de uma ação do controlador ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image18.png))


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image10.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image19.png)

**Figura 10**: Criando um novo modo de exibição com a caixa de diálogo Adicionar modo de exibição ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image20.png))


**A listagem 3 – Views\Home\Index.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample3.aspx)]

A exibição índice exibe todos os registros de filme da tabela de banco de dados de filmes em uma tabela HTML. O modo de exibição contém um loop foreach que itera por meio de cada filme representado pela propriedade ViewData.Model. Se você executar o aplicativo pressionando a tecla F5, você verá a página da web na Figura 11.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image11.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image21.png)

**Figura 11**: exibir o índice ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image22.png))


## <a name="creating-new-database-records"></a>Criação de novos registros de banco de dados

O modo de exibição de índice que criamos na seção anterior inclui um link para a criação de novos registros de banco de dados. Vamos prosseguir e implementar a lógica e criar o modo de exibição necessários para criar novos registros de banco de dados do filme.

O controlador Home contém dois métodos chamados Create (). O primeiro método Create () não tem parâmetros. Esta sobrecarga do método Create () é usada para exibir o formulário HTML para criar um novo registro de banco de dados do filme.

O segundo método Create () tem um parâmetro de FormCollection. Esta sobrecarga do método Create () é chamada quando o formulário HTML para criar um novo filme é enviado para o servidor. Observe que este segundo método Create () tem um atributo AcceptVerbs que impede que o método que está sendo chamado, a menos que uma operação HTTP POST é executada.

Este segundo método Create () foi modificado na classe HomeController atualizado na listagem 4. A nova versão do método Create () aceita um parâmetro de filme e contém a lógica para inserir um novo filme na tabela de banco de dados de filmes.

> [!NOTE] 
> 
> Observe o atributo de associação. Como não queremos atualizar a propriedade de Id de filme de formulário HTML, é preciso excluir explicitamente essa propriedade.


**A listagem 4 – Controllers\HomeController.cs (método de criação modificado)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample4.cs)]

O Visual Studio facilita a criar o formulário para criar um novo banco de dados do filme registrar (veja a Figura 12). Siga estas etapas:

1. O método Create () no editor de código e selecione a opção de menu **adicionar exibição**.
2. Verifique se que a caixa de seleção rotulada **criar uma exibição fortemente tipada** é verificada.
3. Do **exibir conteúdo** lista suspensa, selecione o valor *criar*.
4. Do **exibir dados de classe** lista suspensa, selecione o valor *MovieApp.Models.Movie*.
5. Clique o **adicionar** botão para criar o novo modo de exibição.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image12.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image23.png)

**Figura 12**: adicionar o modo de criar ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image24.png))


O Visual Studio gera automaticamente a exibição na listagem 5. Essa exibição contém um formulário HTML que inclua campos correspondem a cada uma das propriedades da classe filme.

**Listando 5 – Views\Home\Create.aspx**

[!code-aspx[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample5.aspx)]

> [!NOTE] 
> 
> O formato HTML gerado pela caixa de diálogo Adicionar modo de exibição gera um campo de formulário de Id. Como a coluna Id é uma coluna de identidade, não precisamos este campo de formulário e você poderá removê-lo com segurança.


Depois de adicionar o modo de exibição de criar, você pode adicionar novos registros de filme para o banco de dados. Execute o aplicativo pressionando a tecla F5 e clique no link Criar novo para ver o formulário na Figura 13. Se você preencher e enviar o formulário, é criado um novo registro de banco de dados do filme.

Observe que você obtém automaticamente validação do formulário. Se você não insira uma data de lançamento de um filme, ou insira uma data de versão inválido, o formulário é exibida novamente, e o campo de data de lançamento é realçado.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image13.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image25.png)

**Figura 13**: Criando um novo registro de banco de dados do filme ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image26.png))


## <a name="editing-existing-database-records"></a>Edição de registros do banco de dados existente

Nas seções anteriores, discutimos como lista e criar novos registros de banco de dados. Esta última seção, discutiremos como você pode editar os registros existentes do banco de dados.

Primeiro, é necessário gerar o formulário de edição. Esta etapa é fácil, desde que o Visual Studio irá gerar o formulário de edição para nós automaticamente. Abra a classe HomeController no editor de código do Visual Studio e siga estas etapas:

1. O método Edit() no editor de código e selecione a opção de menu **adicionar exibição** (consulte a Figura 14).
2. Marque a caixa de seleção **criar uma exibição fortemente tipada**.
3. Do **exibir conteúdo** lista suspensa, selecione o valor *editar*.
4. Do **exibir dados de classe** lista suspensa, selecione o valor *MovieApp.Models.Movie*.
5. Clique o **adicionar** botão para criar o novo modo de exibição.

Essas etapas adiciona uma nova exibição chamada Edit.aspx para a pasta Views\Home. Essa exibição contém um formulário HTML para editar um registro de filme.


[![A caixa de diálogo Novo projeto](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image14.jpg)](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image27.png)

**Figura 14**: adicionar o modo de exibição de edição ([clique para exibir a imagem em tamanho normal](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/_static/image28.png))


> [!NOTE] 
> 
> A exibição de edição contém um campo de formulário HTML que corresponde à propriedade de Id de filme. Porque você não quiser pessoas editando o valor da propriedade Id, você deve remover este campo de formulário.


Por fim, é preciso modificar o controlador Home para que ele oferece suporte à edição de um registro de banco de dados. A classe HomeController atualizada está contida na listagem 6.

**Listando 6 – Controllers\HomeController.cs (editar métodos)**

[!code-csharp[Main](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-cs/samples/sample6.cs)]

Na listagem 6, adicionei lógica adicional para ambas as sobrecargas do método Edit(). O primeiro método Edit() retorna o registro de banco de dados do filme que corresponde ao parâmetro de Id passado para o método. A segunda sobrecarga executa as atualizações para um registro de filme no banco de dados.

Observe que você deve recuperar o filme original e, em seguida, chame ApplyPropertyChanges() para atualizar o filme existente no banco de dados.

## <a name="summary"></a>Resumo

O objetivo deste tutorial era dar uma ideia da experiência de criação de um aplicativo ASP.NET MVC. Espero que você descobriu que criar um ASP.NET MVC de aplicativo web é muito semelhante à experiência de criação de um aplicativo ASP.NET ou de Active Server Pages.

Neste tutorial, examinamos somente os recursos mais básicos da estrutura ASP.NET MVC. Em tutoriais futuros, podemos mergulhar mais profundamente em tópicos como controladores, ações do controlador, modos de exibição, dados de exibição e auxiliares HTML.

> [!div class="step-by-step"]
> [Avançar](create-a-movie-database-application-in-15-minutes-with-asp-net-mvc-vb.md)
