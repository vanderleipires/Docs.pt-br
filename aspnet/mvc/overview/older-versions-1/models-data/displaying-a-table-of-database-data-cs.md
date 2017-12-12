---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
title: Exibindo uma tabela de banco de dados (c#) | Microsoft Docs
author: microsoft
description: "Neste tutorial, demonstrarei dois métodos de exibição de um conjunto de registros do banco de dados. Mostrar dois métodos de formatação de um conjunto de registros do banco de dados em um HTML ta..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/07/2008
ms.topic: article
ms.assetid: d6e758b6-6571-484d-a132-34ee6c47747a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 37ea081df2ee26e186669b815a4d769e1976ae9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="displaying-a-table-of-database-data-c"></a>Exibindo uma tabela de banco de dados (c#)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_CS.pdf)

> Neste tutorial, demonstrarei dois métodos de exibição de um conjunto de registros do banco de dados. Vou mostrar dois métodos de formatação de um conjunto de registros do banco de dados em uma tabela HTML. Primeiro, vou mostrar como você pode formatar os registros de banco de dados diretamente dentro de um modo de exibição. Em seguida, demonstrarei como você pode tirar proveito das existe meio-termo ao formatar os registros do banco de dados.


O objetivo deste tutorial é explicar como você pode exibir uma tabela HTML do banco de dados em um aplicativo ASP.NET MVC. Primeiro, você aprenderá a usar as ferramentas de scaffolding incluídas no Visual Studio para gerar uma exibição que exibe um conjunto de registros automaticamente. Em seguida, você aprenderá a usar um parcial como um modelo quando os registros do banco de dados de formatação.

## <a name="create-the-model-classes"></a>Criar as Classes de modelo

Vamos para exibir o conjunto de registros da tabela de banco de dados de filmes. A tabela de banco de dados de filmes contém as seguintes colunas:

<a id="0.3_table01"></a>


| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | int | False |
| Título | Nvarchar (200) | False |
| Diretor | Nvarchar (50) | False |
| DateReleased | DateTime | False |


Para representar a tabela de filmes em nosso aplicativo ASP.NET MVC, precisamos criar uma classe de modelo. Neste tutorial, usamos o Entity Framework da Microsoft para criar nossas classes de modelo.

> [!NOTE] 
> 
> Neste tutorial, usamos o Entity Framework da Microsoft. No entanto, é importante entender que você pode usar uma variedade de tecnologias diferentes para interagir com um banco de dados de um aplicativo ASP.NET MVC incluindo LINQ to SQL, o NHibernate ou ADO.NET.


Siga estas etapas para iniciar o Assistente de modelo de dados de entidade:

1. Clique na pasta de modelos na janela do Gerenciador de soluções e selecione a opção de menu **adicionar, o novo Item**.
2. Selecione o **dados** categoria e selecione o **modelo de dados de entidade ADO.NET** modelo.
3. Forneça o nome de seu modelo de dados *MoviesDBModel.edmx* e clique no **adicionar** botão.

Depois de clicar no botão Adicionar, o Assistente de modelo de dados de entidade é exibida (consulte a Figura 1). Siga estas etapas para concluir o Assistente:

1. No **escolher o modelo de conteúdo** etapa, selecione o **gerar do banco de dados** opção.
2. No **escolha sua Conexão de dados** etapa, use o *MoviesDB.mdf* conexão de dados e o nome *MoviesDBEntities* para as configurações de conexão. Clique o **próximo** botão.
3. No **escolher seus objetos de banco de dados** etapa, expanda o nó tabelas, selecione a tabela de filmes. Insira o namespace *modelos* e clique no **concluir** botão.


[![Criando LINQ para classes do SQL](displaying-a-table-of-database-data-cs/_static/image1.jpg)](displaying-a-table-of-database-data-cs/_static/image1.png)

**Figura 01**: Criando classes LINQ to SQL ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image2.png))


Depois de concluir o Assistente de modelo de dados de entidade, abre o Designer de modelo de dados de entidade. O Designer deve exibir a entidade de filmes (consulte a Figura 2).


[![O Designer de modelo de dados de entidade](displaying-a-table-of-database-data-cs/_static/image2.jpg)](displaying-a-table-of-database-data-cs/_static/image3.png)

**Figura 02**: O Designer de modelo de dados de entidade ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image4.png))


É preciso fazer uma alteração antes de continuar. O Assistente de dados de entidade gera uma classe de modelo chamada *filmes* que representa a tabela de banco de dados de filmes. Como vamos usar a classe de filmes para representar um filme específico, é preciso modificar o nome da classe a ser *filme* em vez de *filmes* (singular em vez de no plural).

Clique duas vezes no nome da classe na superfície do designer e alterar o nome da classe de filmes para filme. Depois de fazer essa alteração, clique no **salvar** botão (o ícone de disquete) para gerar a classe de filme.

## <a name="create-the-movies-controller"></a>Criar o controlador de filmes

Agora que temos uma maneira de representar nossos registros do banco de dados, podemos criar um controlador que retorna a coleção de filmes. Dentro da janela do Gerenciador de soluções do Visual Studio, clique na pasta controladores e selecione a opção de menu **adicionar, controlador** (consulte a Figura 3).


[![O Menu do controlador de adição](displaying-a-table-of-database-data-cs/_static/image3.jpg)](displaying-a-table-of-database-data-cs/_static/image5.png)

**Figura 03**: Adicionar controlador Menu ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image6.png))


Quando o **Adicionar controlador** caixa de diálogo for exibida, insira o nome do controlador MovieController (consulte a Figura 4). Clique o **adicionar** botão para adicionar o novo controlador.


[![A caixa de diálogo Adicionar controlador](displaying-a-table-of-database-data-cs/_static/image4.jpg)](displaying-a-table-of-database-data-cs/_static/image7.png)

**Figura 04**: caixa de diálogo a adicionar controlador ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image8.png))


É preciso modificar a ação de Index () exposta pelo controlador de filme, de forma que ele retorna o conjunto de registros do banco de dados. Modificar o controlador para que ele se parece com o controlador na listagem 1.

**Listando 1 – Controllers\MovieController.cs**

[!code-csharp[Main](displaying-a-table-of-database-data-cs/samples/sample1.cs)]

Na listagem 1, a classe MoviesDBEntities é usada para representar o banco de dados MoviesDB. Para usar essa classe, você precisa importar o namespace MvcApplication1.Models como este:

usando MvcApplication1.Models;

A expressão *entidades. MovieSet.ToList()* retorna o conjunto de todos os filmes da tabela de banco de dados de filmes.

## <a name="create-the-view"></a>Criar o modo de exibição

É a maneira mais fácil de exibir um conjunto de registros do banco de dados em uma tabela HTML aproveitar o scaffolding fornecido pelo Visual Studio.

Criar seu aplicativo, selecionando a opção de menu **criar, compilar solução**. Você deve criar seu aplicativo antes de abrir o **adicionar exibição** suas classes de dados ou de caixa de diálogo não aparecerão na caixa de diálogo.

A ação Index () e selecione a opção de menu **adicionar exibição** (consulte a Figura 5).


[![Adicionando uma exibição](displaying-a-table-of-database-data-cs/_static/image5.jpg)](displaying-a-table-of-database-data-cs/_static/image9.png)

**Figura 05**: adicionando uma exibição ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image10.png))


No **adicionar exibição** caixa de diálogo, marque a caixa de seleção **criar uma exibição fortemente tipada**. Selecione a classe de filme como o **exibir dados de classe**. Selecione *lista* como o **exibir conteúdo** (veja a Figura 6). Selecionar essas opções irá gerar uma exibição fortemente tipada que exibe uma lista de filmes.


[![A caixa de diálogo Adicionar modo de exibição](displaying-a-table-of-database-data-cs/_static/image6.jpg)](displaying-a-table-of-database-data-cs/_static/image11.png)

**Figura 06**: caixa de diálogo Adicionar exibir ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image12.png))


Depois de clicar no **adicionar** botão, o modo de exibição na lista 2 é gerado automaticamente. Essa exibição contém o código necessário para iterar pela coleção de filmes e exibir cada uma das propriedades de um filme.

**A listagem 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample2.aspx)]

Você pode executar o aplicativo, selecionando a opção de menu **depurar, iniciar depuração** (ou pressionar a tecla F5). Executando o aplicativo inicia o Internet Explorer. Se você navegar para a URL de /Movie, em seguida, você verá a página na Figura 7.


[![Uma tabela de filmes](displaying-a-table-of-database-data-cs/_static/image7.jpg)](displaying-a-table-of-database-data-cs/_static/image13.png)

**Figura 07**: uma tabela de filmes ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-cs/_static/image14.png))


Se você não gosta nada sobre a aparência da grade de registros do banco de dados na Figura 7, em seguida, você simplesmente pode modificar a exibição do índice. Por exemplo, você pode alterar o *DateReleased* cabeçalho para *data de lançamento* modificando a exibição do índice.

## <a name="create-a-template-with-a-partial"></a>Criar um modelo com um parcial

Quando uma exibição é muito complicada, é recomendável iniciar invadir o modo de exibição existe meio-termo. Usar existe meio-termo torna mais fácil de entender e manter seus modos de exibição. Vamos criar um parcial que podemos usar como modelo para formatar cada um dos registros de banco de dados do filme.

Siga estas etapas para criar o parcial:

1. Clique na pasta Views\Movie e selecione a opção de menu **adicionar exibição**.
2. Marque a caixa de seleção *criar uma exibição parcial (. ascx)*.
3. Nomeie o parcial *MovieTemplate*.
4. Marque a caixa de seleção **criar uma exibição fortemente tipada**.
5. Selecione um filme, como o *exibir dados de classe*.
6. Selecione vazio como o *exibir conteúdo*.
7. Clique o **adicionar** botão para adicionar o parcial para seu projeto.

Depois de concluir essas etapas, modifique o MovieTemplate parcial para se parecer com a listagem 3.

**A listagem 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample3.aspx)]

Parcial na listagem 3 contém um modelo para uma única linha de registros.

O modo de exibição do índice modificado na listagem 4 usa o MovieTemplate parcial.

**A listagem 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-cs/samples/sample4.aspx)]

O modo de exibição na listagem 4 contém um loop foreach que itera por meio de todos os filmes. Para cada filme, o MovieTemplate parcial é usado para formatar o filme. O MovieTemplate é renderizado, chamando o método auxiliar RenderPartial().

O modo de exibição do índice modificado renderiza a mesma tabela HTML de registros do banco de dados. No entanto, o modo de exibição foi bastante simplificado.


O método RenderPartial() é diferente do que a maioria dos outros métodos auxiliares porque ele não retorna uma cadeia de caracteres. Portanto, você deve chamar o método RenderPartial() usando &lt;% % Html.RenderPartial();&gt; em vez de &lt;% = Html.RenderPartial(); %&gt;.


## <a name="summary"></a>Resumo

O objetivo deste tutorial era explicam como você pode exibir um conjunto de registros do banco de dados em uma tabela HTML. Primeiro, você aprendeu como retornar um conjunto de registros do banco de dados de uma ação do controlador, aproveitando o Entity Framework da Microsoft. Em seguida, você aprendeu a usar scaffolding do Visual Studio para gerar uma exibição que exibe uma coleção de itens automaticamente. Por fim, você aprendeu a simplificar a exibição aproveitando um parcial. Você aprendeu a usar um parcial como um modelo para que você pode formatar cada registro do banco de dados.

>[!div class="step-by-step"]
[Anterior](creating-model-classes-with-linq-to-sql-cs.md)
[Próximo](performing-simple-validation-cs.md)
