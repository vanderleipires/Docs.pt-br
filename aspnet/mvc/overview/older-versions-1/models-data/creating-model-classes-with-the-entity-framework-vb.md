---
uid: mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
title: Criando Classes de modelo com o Entity Framework (VB) | Microsoft Docs
author: microsoft
description: Neste tutorial, você aprenderá como usar o ASP.NET MVC com o Entity Framework da Microsoft. Você aprenderá a usar o Assistente de entidade para criar um de entidade ADO.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: ff8322c9-12f3-4e24-aba6-a38046b9bb0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/creating-model-classes-with-the-entity-framework-vb
msc.type: authoredcontent
ms.openlocfilehash: 3442435c7b2b9ce2ce6bd016ba74fe671eb76f62
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="creating-model-classes-with-the-entity-framework-vb"></a>Criando Classes de modelo com o Entity Framework (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Neste tutorial, você aprenderá como usar o ASP.NET MVC com o Entity Framework da Microsoft. Você aprenderá a usar o Assistente de entidade para criar um modelo de dados de entidade ADO.NET. No decorrer deste tutorial, vamos criar um aplicativo web que ilustra como selecionar, inserir, atualizar e excluir dados do banco de dados usando o Entity Framework.


O objetivo deste tutorial é explicar como você pode criar classes de acesso de dados usando o Entity Framework da Microsoft ao compilar um aplicativo ASP.NET MVC. Este tutorial não pressupõe que nenhum conhecimento anterior de Microsoft Entity Framework. No final deste tutorial, compreender como usar o Entity Framework para selecionar, inserir, atualizar e excluir registros do banco de dados.

O Entity Framework da Microsoft é uma ferramenta de mapeamento relacional objeto (O/RM) que permite que você gere uma camada de acesso a dados de um banco de dados automaticamente. O Entity Framework permite que você evite o trabalho tedioso de compilação de suas classes de acesso de dados manualmente.

> [!NOTE] 
> 
> Não há nenhuma conexão essencial entre ASP.NET MVC e o Entity Framework da Microsoft. Há várias alternativas para o Entity Framework que você pode usar com o ASP.NET MVC. Por exemplo, você pode criar suas classes de modelo MVC usando outras ferramentas do / RM, como Microsoft LINQ to SQL, o NHibernate ou SubSonic.


Para ilustrar como você pode usar o Entity Framework da Microsoft com o ASP.NET MVC, criaremos um aplicativo de exemplo simples. Vamos criar um aplicativo de banco de dados do filme que permite exibir e editar registros de banco de dados do filme.

Este tutorial presume que você tenha o Visual Studio 2008 ou o Visual Web Developer 2008 com Service Pack 1. Você precisa Service Pack 1 para usar o Entity Framework. Você pode baixar o Visual Studio 2008 Service Pack 1 ou o Visual Web Developer com Service Pack 1 do seguinte endereço:

> [https://www.asp.net/downloads/](https://www.asp.net/downloads)


## <a name="creating-the-movie-sample-database"></a>Criando o banco de dados de exemplo do filme

O aplicativo de banco de dados do filme usa uma tabela de banco de dados denominada filmes que contém as seguintes colunas:

| Nome da coluna | Tipo de dados | Permitir nulos? | É a chave primária? |
| --- | --- | --- | --- |
| Id | int | False | verdadeiro |
| Título | nvarchar(100) | False | False |
| Diretor | nvarchar(100) | False | False |

Você pode adicionar essa tabela para um projeto ASP.NET MVC seguindo estas etapas:

1. Clique com botão direito do aplicativo\_pasta de dados na janela do Gerenciador de soluções e selecione a opção de menu **Add, o novo Item.**
2. Do **Adicionar Novo Item** caixa de diálogo, selecione **o banco de dados do SQL Server**, forneça o nome MoviesDB.mdf de banco de dados e clique no **adicionar** botão.
3. Clique duas vezes no arquivo de MoviesDB.mdf para abrir a janela do Gerenciador do servidor Explorer/banco de dados.
4. Expanda a conexão de banco de dados MoviesDB.mdf, com o botão direito na pasta tabelas e selecione a opção de menu **adicionar nova tabela**.
5. No Designer de tabela, adicione as colunas de Id, título e Director.
6. Clique o **salvar** botão (ele tem o ícone de disquete) para salvar a nova tabela com os nome de filmes.

Depois de criar a tabela de banco de dados de filmes, você deve adicionar alguns dados de exemplo para a tabela. A tabela de filmes e selecione a opção de menu **Mostrar dados da tabela**. Você pode inserir dados do filme falso para a grade que aparece.

## <a name="creating-the-adonet-entity-data-model"></a>Criar o modelo de dados de entidade ADO.NET

Para usar o Entity Framework, você precisa criar um modelo de dados de entidade. Você pode tirar proveito do Visual Studio *Assistente de modelo de dados de entidade* para gerar um modelo de dados de entidade de banco de dados automaticamente.

Siga estas etapas:

1. Clique na pasta de modelos na janela do Gerenciador de soluções e selecione a opção de menu **adicionar, o novo Item**.
2. No **Adicionar Novo Item** caixa de diálogo, selecione a categoria de dados (consulte a Figura 1).
3. Selecione o **modelo de dados de entidade ADO.NET** modelo, nomeie o modelo de dados de entidade de MoviesDBModel.edmx e clique no **adicionar** botão. Clique o **adicionar** botão inicia o Assistente de modelo de dados.
4. No **escolher o modelo de conteúdo** etapa, escolha o **gerar um banco de dados** opção e clique no **próximo** botão (consulte a Figura 2).
5. No **escolha sua Conexão de dados** etapa, selecione a conexão de banco de dados MoviesDB.mdf, insira as entidades de configurações de conexão nome MoviesDBEntities e clique no **próximo** botão (consulte a Figura 3).
6. No **escolher seus objetos de banco de dados** etapa, selecione a tabela de banco de dados do filme e clique no **concluir** botão (consulte a Figura 4).

Depois de concluir essas etapas, o Designer de modelo de dados de entidade ADO.NET (Designer de entidade) é aberto.

**Figura 1 – criar um novo modelo de dados de entidade**

![clip_image002](creating-model-classes-with-the-entity-framework-vb/_static/image1.jpg)

**Figura 2 – Escolha a etapa de conteúdo do modelo**

![clip_image004](creating-model-classes-with-the-entity-framework-vb/_static/image2.jpg)

**Figura 3 – Escolha sua Conexão de dados**

![clip_image006](creating-model-classes-with-the-entity-framework-vb/_static/image3.jpg)

**Figura 4 – Escolha seus objetos de banco de dados**

![clip_image008](creating-model-classes-with-the-entity-framework-vb/_static/image4.jpg)

## <a name="modifying-the-adonet-entity-data-model"></a>Modificar o modelo de dados de entidade ADO.NET

Depois de criar um modelo de dados de entidade, você pode modificar o modelo, aproveitando o Entity Designer (consulte a Figura 5). Você pode abrir o Designer de entidade a qualquer momento clicando no arquivo MoviesDBModel.edmx contido na pasta modelos dentro da janela do Gerenciador de soluções.

**Figura 5 – o Designer de modelo de dados de entidade ADO.NET**

![clip_image010](creating-model-classes-with-the-entity-framework-vb/_static/image5.jpg)

Por exemplo, você pode usar o Entity Designer para alterar os nomes das classes que gera o Assistente de dados de modelo de entidade. O assistente criou uma nova classe de acesso a dados, denominada filmes. Em outras palavras, o assistente deu a classe com o mesmo nome que a tabela de banco de dados. Pois usaremos esta classe para representar uma instância específica do filme, podemos deve renomear a classe de filmes para filme.

Se você quiser renomear uma classe de entidade, você pode clique duas vezes no nome da classe no Entity Designer e digite um novo nome (veja a Figura 6). Como alternativa, você pode alterar o nome de uma entidade na janela Propriedades depois de selecionar uma entidade no Entity Designer.

**Figura 6 – alterar um nome de entidade**

![clip_image012](creating-model-classes-with-the-entity-framework-vb/_static/image6.jpg)

Lembre-se de salvar o modelo de dados de entidade depois de fazer uma modificação clicando no botão Salvar (o ícone de disquete). Nos bastidores, o Designer de entidade gera um conjunto de classes do Visual Basic .NET. Você pode exibir essas classes abrindo o arquivo MoviesDBModel.Designer.vb da janela do Gerenciador de soluções.


Não modifique o código no arquivo de designer desde que suas alterações serão substituídas na próxima vez que você usar o Designer de entidade. Se desejar estender a funcionalidade das classes de entidade definidas no arquivo de Designer, você pode criar *classes parciais* arquivos separados em.


#### <a name="selecting-database-records-with-the-entity-framework"></a>Selecionando registros do banco de dados com o Entity Framework

Vamos começar criando o nosso aplicativo de banco de dados do filme, criando uma página que exibe uma lista de registros do filme. O controlador Home na listagem 1 expõe uma ação chamada index (). A ação Index () retorna todos os registros de filme da tabela de banco de dados do filme, tirando proveito do Entity Framework.

**Listando 1 – Controllers\HomeController.vb**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample1.vb)]

Observe que o controlador na listagem 1 inclui um construtor. O construtor inicializa um campo de nível de classe chamado \_banco de dados. O \_campo do banco de dados representa as entidades de banco de dados geradas pelo Microsoft Entity Framework. O \_campo de banco de dados é uma instância da classe MoviesDBEntities que foi gerada pelo Designer de entidade.

O \_campo de banco de dados é usado na ação Index () para recuperar os registros da tabela de banco de dados de filmes. A expressão \_banco de dados. MovieSet representa todos os registros da tabela de banco de dados de filmes. O método ToList() é usado para converter o conjunto de filmes em uma coleção genérica de objetos de filme: lista (de filme).

Os registros de filme são recuperados com a Ajuda do LINQ to Entities. A ação de Index () na listagem 1 usa LINQ *sintaxe de método* para recuperar o conjunto de registros do banco de dados. Se preferir, você pode usar o LINQ *sintaxe de consulta* em vez disso. As duas instruções a seguir são a mesma coisa:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample2.vb)]

Use a sintaxe LINQ – sintaxe de método ou sintaxe de consulta – que você consideram mais intuitiva. Não há nenhuma diferença no desempenho entre as duas abordagens – a única diferença é o estilo.

O modo de exibição na lista 2 é usado para exibir os registros do filme.

**A listagem 2 – Views\Home\Index.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample3.aspx)]

O modo de exibição na lista 2 contém um **para cada** loop que itera por meio de cada registro de filme e exibe os valores das propriedades de título e diretor do registro do filme. Observe que um link de edição e exclusão é exibido ao lado de cada registro. Além disso, um link Adicionar filme aparece na parte inferior do modo de exibição (consulte a Figura 7).

**Figura 7 – o modo de exibição do índice**

![clip_image014](creating-model-classes-with-the-entity-framework-vb/_static/image7.jpg)

A exibição do índice é um *digitado exibição*. A exibição do índice tem um &lt;% @ Page %&gt; diretiva que inclui um atributo Inherits. O atributo Inherits converte a propriedade ViewData.Model para uma coleção fortemente tipada genérica lista de objetos de filme – uma lista (de filme).

## <a name="inserting-database-records-with-the-entity-framework"></a>Inserindo registros do banco de dados com o Entity Framework

Você pode usar o Entity Framework para tornar mais fácil inserir novos registros em uma tabela de banco de dados. A listagem 3 contém duas novas ações adicionadas para a classe do controlador inicial que você pode usar para inserir novos registros na tabela de banco de dados do filme.

**A listagem 3 – Controllers\HomeController.vb (Adicionar métodos)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample4.vb)]

A primeira ação Add simplesmente retorna uma exibição. O modo de exibição contém um formulário para adicionar um novo banco de dados do filme registrar (consulte a Figura 8). Quando você enviar o formulário, a segunda ação Add () é chamada.

Observe que a segunda ação Add está decorada com o atributo AcceptVerbs. Esta ação pode ser invocada somente quando uma operação HTTP POST. Em outras palavras, essa ação só pode ser chamada durante o lançamento de um formulário HTML.

A segunda ação Add () cria uma nova instância da classe filme do Entity Framework com a Ajuda do método TryUpdateModel() do ASP.NET MVC. O método TryUpdateModel() usa os campos de FormCollection passado para o método Add () e atribui os valores desses campos de formulário HTML para a classe do filme.


Ao usar o Entity Framework, você deve fornecer uma "lista de permissões" de propriedades ao usar os métodos TryUpdateModel ou UpdateModel para atualizar as propriedades de uma classe de entidade.


Em seguida, a ação Add () executa uma validação de formulário simples. A ação verifica se o título e o Diretor propriedades têm valores. Se houver um erro de validação, uma mensagem de erro de validação é adicionada para o ModelState.

Se não houver nenhum erro de validação um novo registro de filme é adicionado à tabela de banco de dados de filmes com a Ajuda do Entity Framework. O novo registro é adicionado ao banco de dados com as duas linhas de código a seguir:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample5.vb)]

A primeira linha do código adiciona a nova entidade de filme ao conjunto de filmes que estão sendo rastreados pelo Entity Framework. A segunda linha do código salva as alterações foram feitas para filmes sendo rastreados no banco de dados subjacente.

**Figura 8 – o modo de exibição de adicionar**

![clip_image016](creating-model-classes-with-the-entity-framework-vb/_static/image8.jpg)

## <a name="updating-database-records-with-the-entity-framework"></a>Atualizando os registros do banco de dados com o Entity Framework

Você pode seguir quase a mesma abordagem para editar um registro de banco de dados com o Entity Framework, como o método que seguimos apenas para inserir um novo registro de banco de dados. A listagem 4 contém duas novas ações de controlador, denominadas Edit(). A primeira ação Edit() retorna um formulário HTML para editar um registro de filme. A segunda ação Edit() tenta atualizar o banco de dados.

**A listagem 4 – Controllers\HomeController.vb (editar métodos)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample6.vb)]

A segunda ação Edit() inicia, recuperando o registro de filme de banco de dados que corresponde à Id do filme que está sendo editado. O LINQ seguinte à instrução de entidades captura o primeiro registro de banco de dados que corresponde a uma determinada identificação:

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample7.vb)]

Em seguida, o método TryUpdateModel() é usado para atribuir os valores dos campos de formulário HTML para as propriedades da entidade do filme. Observe que uma lista de permissões é fornecida para especificar as propriedades exatas para atualizar.

Em seguida, uma validação simple é executada para verificar se o título do filme e diretor propriedades têm valores. Se a propriedade está faltando um valor, em seguida, uma mensagem de erro de validação é adicionada ao ModelState e ModelState retorna o valor false.

Por fim, se não houver nenhum erro de validação, a tabela de banco de dados de filmes subjacente é atualizada com as alterações chamando o método SaveChanges ().

Durante a edição de registros do banco de dados, você precisa passar a Id do registro que está sendo editado para a ação de controlador que executa a atualização do banco de dados. Caso contrário, a ação de controlador não saberá qual registro será atualizado no banco de dados subjacente. O modo de exibição de edição, contido na listagem 5, inclui um campo de formulário oculto que representa a Id do registro de banco de dados que está sendo editado.

**Listando 5 – Views\Home\Edit.aspx**

[!code-aspx[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample8.aspx)]

## <a name="deleting-database-records-with-the-entity-framework"></a>Excluir registros do banco de dados com o Entity Framework

A operação de banco de dados final, precisamos abordar neste tutorial, é excluir registros do banco de dados. Você pode usar a ação de controlador na listagem 6 para excluir um registro de banco de dados específico.

**Listando 6 – \Controllers\HomeController.vb (ação de exclusão)**

[!code-vb[Main](creating-model-classes-with-the-entity-framework-vb/samples/sample9.vb)]

A ação Delete () primeiro recupera o filme que corresponde à Id da entidade é passada para a ação. Em seguida, o filme é excluído do banco de dados chamando o método DeleteObject() seguido pelo método SaveChanges (). Por fim, o usuário é redirecionado de volta para o modo de exibição do índice.

## <a name="summary"></a>Resumo

O objetivo deste tutorial foi demonstrar como você pode criar aplicativos da web para banco de dados, aproveitando o ASP.NET MVC e o Entity Framework da Microsoft. Você aprendeu a criar um aplicativo que permite que você selecionar, inserir, atualizar e excluir registros do banco de dados.

Primeiro, discutimos como você pode usar o Assistente de modelo de dados de entidade para gerar um modelo de dados de entidade de dentro do Visual Studio. Em seguida, você aprenderá a usar o LINQ to Entities para recuperar um conjunto de registros do banco de dados de uma tabela de banco de dados. Finalmente, usamos o Entity Framework para inserir, atualizar e excluir registros do banco de dados.

> [!div class="step-by-step"]
> [Anterior](validation-with-the-data-annotation-validators-cs.md)
> [Próximo](creating-model-classes-with-linq-to-sql-vb.md)
