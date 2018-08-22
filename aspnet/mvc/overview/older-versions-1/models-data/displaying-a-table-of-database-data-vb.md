---
uid: mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
title: Exibindo uma tabela de banco de dados (VB) | Microsoft Docs
author: microsoft
description: Neste tutorial, demonstrarei dois métodos de exibição de um conjunto de registros do banco de dados. Eu mostro dois métodos de formatação de um conjunto de registros de banco de dados em um HTML ta...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: 5bb4587f-5bcd-44f5-b368-3c1709162b35
msc.legacyurl: /mvc/overview/older-versions-1/models-data/displaying-a-table-of-database-data-vb
msc.type: authoredcontent
ms.openlocfilehash: d96f574c9284ab259b8733b3b8109ecd0b689aa8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824374"
---
<a name="displaying-a-table-of-database-data-vb"></a>Exibindo uma tabela de banco de dados (VB)
====================
por [Microsoft](https://github.com/microsoft)

[Baixar PDF](http://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_11_VB.pdf)

> Neste tutorial, demonstrarei dois métodos de exibição de um conjunto de registros do banco de dados. Eu mostro dois métodos de formatação de um conjunto de registros de banco de dados em uma tabela HTML. Primeiro, vou mostrar como você pode formatar os registros de banco de dados diretamente dentro de um modo de exibição. Em seguida, demonstrarei como você pode tirar proveito de parciais ao formatar os registros de banco de dados.


O objetivo deste tutorial é explicar como você pode exibir uma tabela HTML de dados do banco de dados em um aplicativo ASP.NET MVC. Primeiro, você aprenderá a usar as ferramentas de scaffolding incluídas no Visual Studio para gerar uma exibição que exibe um conjunto de registros automaticamente. Em seguida, você aprenderá a usar um parcial como um modelo ao formatar os registros de banco de dados.

## <a name="create-the-model-classes"></a>Criar as Classes de modelo

Vamos para exibir o conjunto de registros da tabela de banco de dados de filmes. A tabela de banco de dados de filmes contém as seguintes colunas:

<a id="0.4_table01"></a>


| **Nome da coluna** | **Tipo de dados** | **Permitir nulos** |
| --- | --- | --- |
| Id | int | False |
| Título | Nvarchar(200) | False |
| Diretor | Nvarchar (50) | False |
| DateReleased | DateTime | False |


Para representar a tabela de filmes em nosso aplicativo ASP.NET MVC, precisamos criar uma classe de modelo. Neste tutorial, usamos o Microsoft Entity Framework para criar nossas classes de modelo.

> [!NOTE] 
> 
> Neste tutorial, usamos o Microsoft Entity Framework. No entanto, é importante entender que você pode usar uma variedade de tecnologias diferentes para interagir com um banco de dados de um aplicativo ASP.NET MVC incluindo LINQ to SQL, NHibernate ou ADO.NET.


Siga estas etapas para iniciar o Assistente de modelo de dados de entidade:

1. Clique com botão direito na pasta de modelos na janela do Gerenciador de soluções e selecione a opção de menu **adicionar, Item novo**.
2. Selecione o **dados** categoria e selecione o **modelo de dados de entidade ADO.NET** modelo.
3. Dê o nome de seu modelo de dados *MoviesDBModel.edmx* e clique no **Add** botão.

Depois de clicar no botão Add, o Assistente de modelo de dados de entidade é exibida (veja a Figura 1). Siga estas etapas para concluir o Assistente:

1. No **escolher conteúdo do modelo** etapa, selecione o **gerar a partir do banco de dados** opção.
2. No **escolha sua Conexão de dados** etapa, use o *MoviesDB.mdf* conexão de dados e o nome *MoviesDBEntities* para as configurações de conexão. Clique o **próxima** botão.
3. No **Choose Your Database Objects** etapa, expanda o nó Tables, selecione a tabela de filmes. Insira o namespace *modelos* e clique no **concluir** botão.


[![Criando o LINQ para classes SQL](displaying-a-table-of-database-data-vb/_static/image1.jpg)](displaying-a-table-of-database-data-vb/_static/image1.png)

**Figura 01**: Criando classes LINQ to SQL ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image2.png))


Depois de concluir o Assistente de modelo de dados de entidade, o Designer de modelo de dados de entidade é aberto. O Designer deve exibir a entidade de filmes (veja a Figura 2).


[![O Designer de modelo de dados de entidade](displaying-a-table-of-database-data-vb/_static/image2.jpg)](displaying-a-table-of-database-data-vb/_static/image3.png)

**Figura 02**: O Designer de modelo de dados de entidade ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image4.png))


Precisamos fazer uma alteração antes de continuar. O Assistente de dados de entidade gera uma classe de modelo chamada *filmes* que representa a tabela de banco de dados de filmes. Como vamos usar a classe de filmes para representar um filme específico, precisamos modificar o nome da classe para ser *filme* em vez de *filmes* (singular em vez de no plural).

Duas vezes no nome da classe na superfície do designer e altere o nome da classe de filmes para filme. Após fazer essa alteração, clique o **salvar** botão (o ícone de disquete) para gerar a classe de filme.

## <a name="create-the-movies-controller"></a>Criar o controlador de filmes

Agora que temos uma forma de representar nossos registros do banco de dados, podemos criar um controlador que retorna a coleção de filmes. Dentro da janela do Gerenciador de soluções do Visual Studio, clique com botão direito na pasta controladores e selecione a opção de menu **Add, controlador** (veja a Figura 3).


[![O controlador Menu adicionar](displaying-a-table-of-database-data-vb/_static/image3.jpg)](displaying-a-table-of-database-data-vb/_static/image5.png)

**Figura 03**: O adicionar controlador Menu ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image6.png))


Quando o **Adicionar controlador** caixa de diálogo for exibida, insira o nome do controlador MovieController (veja a Figura 4). Clique o **adicionar** botão para adicionar o novo controlador.


[![A caixa de diálogo Adicionar controlador](displaying-a-table-of-database-data-vb/_static/image4.jpg)](displaying-a-table-of-database-data-vb/_static/image7.png)

**Figura 04**: caixa de diálogo o adicionar controlador ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image8.png))


Precisamos modificar a ação Index () exposta pelo controlador de filmes para que ele retorne o conjunto de registros do banco de dados. Modificar o controlador para que ele se parece com o controlador na listagem 1.

**Listagem 1 – Controllers\MovieController.vb**

[!code-vb[Main](displaying-a-table-of-database-data-vb/samples/sample1.vb)]

Na listagem 1, a classe MoviesDBEntities é usada para representar o banco de dados MoviesDB. A expressão *entidades. MovieSet.ToList()* retorna o conjunto de todos os filmes da tabela de banco de dados de filmes.

## <a name="create-the-view"></a>Criar o modo de exibição

A maneira mais fácil para exibir um conjunto de registros do banco de dados em uma tabela HTML é aproveitar o scaffolding fornecido pelo Visual Studio.

Criar seu aplicativo, selecionando a opção de menu **Build Build Solution**. Você deve criar seu aplicativo antes de abrir o **adicionar exibição** suas classes de dados ou de caixa de diálogo não aparecerão na caixa de diálogo.

A ação Index () com o botão direito e selecione a opção de menu **adicionar exibição** (consulte a Figura 5).


[![Adicionando uma exibição](displaying-a-table-of-database-data-vb/_static/image5.jpg)](displaying-a-table-of-database-data-vb/_static/image9.png)

**Figura 05**: adicionando uma exibição ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image10.png))


No **adicionar exibição** caixa de diálogo, marque a caixa de seleção **criar uma exibição fortemente tipada**. Selecione a classe de filme como a **exibir dados de classe**. Selecione *lista* como o **exibir conteúdo** (veja a Figura 6). Selecione essas opções irá gerar uma exibição fortemente tipada que exibe uma lista de filmes.


[![A caixa de diálogo Adicionar modo de exibição](displaying-a-table-of-database-data-vb/_static/image6.jpg)](displaying-a-table-of-database-data-vb/_static/image11.png)

**Figura 06**: caixa de diálogo Adicionar exibição do ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image12.png))


Depois de clicar na **adicionar** botão, o modo de exibição na listagem 2 é gerado automaticamente. Essa exibição contém o código necessário para iterar pela coleção de filmes e exibir cada uma das propriedades de um filme.

**Listagem 2 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample2.aspx)]

Você pode executar o aplicativo, selecionando a opção de menu **depurar, iniciar depuração** (ou pressionando a tecla F5). Executando o aplicativo inicia o Internet Explorer. Se você navegar até a URL /Movie, em seguida, você verá a página na Figura 7.


[![Uma tabela de filmes](displaying-a-table-of-database-data-vb/_static/image7.jpg)](displaying-a-table-of-database-data-vb/_static/image13.png)

**Figura 07**: uma tabela de filmes ([clique para exibir a imagem em tamanho normal](displaying-a-table-of-database-data-vb/_static/image14.png))


Se você não gostar nada sobre a aparência da grade de registros de banco de dados na Figura 7, em seguida, você pode simplesmente modificar o modo de exibição de índice. Por exemplo, você pode alterar o *DateReleased* cabeçalho *data de lançamento* modificando a exibição de índice.

## <a name="create-a-template-with-a-partial"></a>Criar um modelo com um parcial

Quando um modo de exibição fica muito complicado, ele é uma boa ideia começar invadir o modo de exibição parciais. Usar parciais torna mais fácil de entender e manter seus modos de exibição. Vamos criar um parcial que podemos usar como modelo para formatar cada um dos registros de banco de dados do filme.

Siga estas etapas para criar parcial:

1. Clique com botão direito na pasta Views\Movie e selecione a opção de menu **adicionar exibição**.
2. Marque a caixa de seleção rotulada *criar uma exibição parcial (. ascx)*.
3. Nome parcial *MovieTemplate*.
4. Marque a caixa de seleção rotulada **criar uma exibição fortemente tipada**.
5. Selecione o filme como a *exibir dados de classe*.
6. Selecione vazio como o *exibir o conteúdo*.
7. Clique o **adicionar** botão Adicionar parcial ao seu projeto.

Depois de concluir essas etapas, modifique o MovieTemplate parcial para se parecer com a listagem 3.

**Listagem 3 – Views\Movie\MovieTemplate.ascx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample3.aspx)]

Parcial na listagem 3 contém um modelo para uma única linha de registros.

O modo de exibição do índice modificado na listagem 4 usa o MovieTemplate parcial.

**Listagem 4 – Views\Movie\Index.aspx**

[!code-aspx[Main](displaying-a-table-of-database-data-vb/samples/sample4.aspx)]

A exibição na listagem 4 contém um para cada loop que itera em todos os filmes. Para cada filme, o MovieTemplate parcial é usado para formatar o filme. O MovieTemplate é renderizado chamando o método auxiliar RenderPartial().

O modo de exibição do índice modificado renderiza a mesma tabela HTML de registros de banco de dados. No entanto, o modo de exibição foi bastante simplificado.


O método RenderPartial() é diferente da maioria dos outros métodos auxiliares, porque ele não retorna uma cadeia de caracteres. Portanto, você deve chamar o método RenderPartial() usando &lt;% Html.RenderPartial()&gt; em vez de &lt;% = % Html.RenderPartial()&gt;.


## <a name="summary"></a>Resumo

O objetivo deste tutorial era explicar como você pode exibir um conjunto de registros do banco de dados em uma tabela HTML. Primeiro, você aprendeu como retornar um conjunto de registros do banco de dados de uma ação do controlador, tirando proveito do Microsoft Entity Framework. Em seguida, você aprendeu como usar o scaffolding do Visual Studio para gerar uma exibição que exibe uma coleção de itens automaticamente. Por fim, você aprendeu a simplificar a exibição, aproveitando um parcial. Você aprendeu a usar um parcial como um modelo para que você pode formatar cada registro de banco de dados.

> [!div class="step-by-step"]
> [Anterior](creating-model-classes-with-linq-to-sql-vb.md)
> [Próximo](performing-simple-validation-vb.md)
