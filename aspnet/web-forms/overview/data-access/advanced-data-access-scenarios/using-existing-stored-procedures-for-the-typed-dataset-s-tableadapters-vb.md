---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Usar existente procedimentos armazenados para TableAdapters do conjunto de dados tipado (VB) | Microsoft Docs
author: rick-anderson
description: No tutorial anterior aprendemos como usar o Assistente de TableAdapter para gerar novos procedimentos armazenados. Neste tutorial, saiba como o mesmo TableAdapter...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: ac319b67c9215c5dde8e7507076ed45a1f7825c6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Usar existente procedimentos armazenados para TableAdapters do conjunto de dados tipado (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) ou [baixar PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> No tutorial anterior aprendemos como usar o Assistente de TableAdapter para gerar novos procedimentos armazenados. Neste tutorial, saiba como o mesmo assistente TableAdapter pode trabalhar com procedimentos armazenados existentes. Podemos também aprenderá a adicionar manualmente os novos procedimentos armazenados para nosso banco de dados.


## <a name="introduction"></a>Introdução

No [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) , vimos como s o conjunto de dados tipado TableAdapters podem ser configurados para usar os procedimentos armazenados para instruções de SQL de dados em vez de ad hoc de acesso. Em particular, examinamos como TableAdapter assistente criar automaticamente esses procedimentos armazenados. Ao portar aplicativos herdados para ASP.NET 2.0, ou ao criar um site ASP.NET 2.0 em torno de um modelo de dados existente, a probabilidade é que o banco de dados já contém os procedimentos armazenados que precisamos. Como alternativa, você pode preferir criar os procedimentos armazenados manualmente ou por meio de uma ferramenta que não seja o assistente TableAdapter que gera automaticamente os procedimentos armazenados.

Este tutorial examinaremos como configurar o TableAdapter para usar os procedimentos armazenados existentes. Como o banco de dados Northwind tem apenas um pequeno conjunto de procedimentos armazenados internos, podemos também examinar as etapas necessárias para adicionar manualmente os novos procedimentos armazenados no banco de dados por meio do ambiente do Visual Studio. Permitir que o s começar!

> [!NOTE]
> No [encapsulamento modificações de banco de dados em uma transação](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial adicionamos métodos TableAdapter para oferecer suporte a transações (`BeginTransaction`, `CommitTransaction`e assim por diante). Como alternativa, as transações podem ser gerenciadas totalmente dentro de um procedimento armazenado, que não requer modificações no código da camada de acesso a dados. Neste tutorial, exploraremos os comandos T-SQL usados para executar instruções de s um procedimento armazenado dentro do escopo de uma transação.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Etapa 1: Adicionar procedimentos armazenados no banco de dados Northwind

O Visual Studio facilita a adicionar novos procedimentos armazenados para um banco de dados. Permitem s adicionar um novo procedimento armazenado no banco de dados Northwind que retorna todas as colunas da `Products` tabela para aqueles que têm um determinado `CategoryID` valor. Na janela Gerenciador de servidores, expanda o banco de dados Northwind para que suas pastas de diagramas de banco de dados, tabelas, exibições e assim por diante - são exibidas. Como vimos no tutorial anterior, a pasta de procedimentos armazenados contém os banco de dados s os procedimentos armazenados existentes. Para adicionar um novo procedimento armazenado, clique na pasta de procedimentos armazenados e escolha a opção de adicionar novo procedimento armazenado no menu de contexto.


[![Clique na pasta de procedimentos armazenados e adicionar um novo procedimento armazenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: clique na pasta de procedimentos armazenados e adicionar um novo procedimento armazenado ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))


Como mostra a Figura 1, selecionando a opção de adicionar novo procedimento armazenado abre uma janela de script no Visual Studio com o contorno do script SQL necessário para criar o procedimento armazenado. É nosso trabalho concretizar esse script e executá-lo, no ponto em que o procedimento armazenado será adicionado ao banco de dados.

Insira o seguinte script:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Este script, quando executado, adicionará um novo procedimento armazenado ao banco de dados Northwind denominado `Products_SelectByCategoryID`. Esse procedimento armazenado aceita um único parâmetro de entrada (`@CategoryID`, do tipo `int`) e retorna todos os campos para os produtos com uma correspondência `CategoryID` valor.

Para executar esta `CREATE PROCEDURE` script e adicione o procedimento armazenado no banco de dados, clique no ícone Salvar na barra de ferramentas ou Ctrl + S de ocorrências. Depois de fazer isso, as atualizações de pasta de procedimentos armazenados, mostrar o recém-criado procedimento armazenado. Além disso, o script na janela mudará sutilmente de `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` para `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Adiciona um novo procedimento armazenado no banco de dados, enquanto `ALTER PROCEDURE` atualiza uma existente. Desde o início do script foi alterado para `ALTER PROCEDURE`, alterar os procedimentos armazenados de entrada parâmetros ou instruções SQL e clicando no ícone Salvar atualizará o procedimento armazenado com essas alterações.

A Figura 2 mostra o Visual Studio após o `Products_SelectByCategoryID` procedimento armazenado foi salvo.


[![O procedimento armazenado Products_SelectByCategoryID foi adicionado ao banco de dados](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Figura 2**: O procedimento armazenado `Products_SelectByCategoryID` foi adicionado ao banco de dados ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Etapa 2: Configurar o TableAdapter para usar um procedimento armazenado existente

Agora que o `Products_SelectByCategoryID` procedimento armazenado foi adicionado ao banco de dados, podemos configurar nossa camada de acesso de dados para usar esse procedimento armazenado quando um de seus métodos é invocado. Em particular, adicionaremos um `GetProducstByCategoryID(<_i22_>categoryID)<!--_i22_-->` método para o `ProductsTableAdapter` no `NorthwindWithSprocs` conjunto de dados tipado que chama o `Products_SelectByCategoryID` que acabamos de criar do procedimento armazenado.

Comece abrindo o `NorthwindWithSprocs` conjunto de dados. Clique com botão direito no `ProductsTableAdapter` e escolha Adicionar consulta para iniciar o Assistente de configuração de consulta do TableAdapter. No [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) é incluído para criar um novo procedimento armazenado para que possamos TableAdapter. Para este tutorial, queremos conectar o novo método de TableAdapter para existente `Products_SelectByCategoryID` procedimento armazenado. Portanto, escolha a opção de procedimento armazenado existente usar a primeira etapa do assistente s e, em seguida, clique em Avançar.


[![Escolha o uso existente de opção de procedimento armazenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Figura 3**: escolher usar existente armazenado procedimento opção ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))


A tela a seguir fornece que uma lista suspensa preenchida com o banco de dados s procedimentos armazenados. Selecionar um procedimento armazenado lista seus parâmetros de entrada à esquerda e os campos de dados retornados (se houver) à direita. Escolha o `Products_SelectByCategoryID` procedimento armazenado a partir da lista e clique em Avançar.


[![Escolha o Products_SelectByCategoryID procedimento armazenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Figura 4**: escolha o `Products_SelectByCategoryID` procedimento armazenado ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))


A próxima tela pergunta que tipo de dados é retornado pelo procedimento armazenado e nossa resposta aqui determina o tipo retornado pelo método s TableAdapter. Por exemplo, se indicamos que dados de tabela são retornados, o método retornará um `ProductsDataTable` instância preenchida com os registros retornados pelo procedimento armazenado. Por outro lado, se indicamos que esse procedimento armazenado retorna um valor único TableAdapter retornará um `Object` que é atribuído o valor na primeira coluna do primeiro registro retornado pelo procedimento armazenado.

Desde o `Products_SelectByCategoryID` procedimento armazenado retorna todos os produtos que pertencem a uma determinada categoria, escolha a primeira resposta - dados de tabela - e clique em Avançar.


[![Indicar que o procedimento armazenado retorna dados tabulares](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Figura 5**: indicar que o procedimento armazenado retorna dados tabulares ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))


Tudo o que resta é para indicar quais padrões de método para usar seguido de nomes para esses métodos. Deixe os dois o preenchimento uma DataTable e retornar uma DataTable opções verificada, mas renomear os métodos para `FillByCategoryID` e `GetProductsByCategoryID`. Clique em Avançar para revisar um resumo das tarefas que o assistente executará. Se tudo estiver correto, clique em Concluir.


[![Nome de métodos FillByCategoryID e GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 6**: nomear os métodos `FillByCategoryID` e `GetProductsByCategoryID` ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


> [!NOTE]
> Os métodos TableAdapter que acabamos de criar, `FillByCategoryID` e `GetProductsByCategoryID`, espera um parâmetro de entrada do tipo `Integer`. Esse valor de parâmetro de entrada é passado para o procedimento armazenado por meio de seu `@CategoryID` parâmetro. Se você modificar o `Products_SelectByCategory` armazenado parâmetros de procedimento s, será necessário também atualizar os parâmetros para esses métodos TableAdapter. Como discutido o [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), isso pode ser feito de duas maneiras: manualmente adicionando ou removendo parâmetros da coleção de parâmetros ou executando novamente o Assistente de TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Etapa 3: Adicionando um`GetProductsByCategoryID(categoryID)`método BLL

Com o `GetProductsByCategoryID` DAL concluída, a próxima etapa é fornecer acesso a esse método na camada de lógica de negócios. Abra o `ProductsBLLWithSprocs` arquivo de classe e adicione o seguinte método:


[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Esse método BLL simplesmente retorna o `ProductsDataTable` retornado do `ProductsTableAdapter` s `GetProductsByCategoryID` método. O `DataObjectMethodAttribute` atributo fornece metadados usados pelo assistente ObjectDataSource s configurar fonte de dados. Em particular, esse método será exibido na lista suspensa Selecione guia s.

## <a name="step-4-displaying-products-by-category"></a>Etapa 4: Exibindo produtos por categoria

Para testar o recém-adicionado `Products_SelectByCategoryID` procedimento armazenado e os métodos DAL e BLL correspondentes, permitem s a criar uma página ASP.NET que contém um DropDownList e GridView. DropDownList listará todas as categorias no banco de dados enquanto o GridView exibirá os produtos que pertencem à categoria selecionada.

> [!NOTE]
> Podemos interfaces de mestre/detalhes ve criado usando DropDownLists nos tutoriais anteriores. Para obter uma visão mais detalhada de implementar esse relatório mestre/detalhes, consulte o [filtragem de mestre/detalhes com um DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) tutorial.


Abra o `ExistingSprocs.aspx` página o `AdvancedDAL` pasta e arraste DropDownList da caixa de ferramentas para o Designer. Definir o s DropDownList `ID` propriedade `Categories` e sua `AutoPostBack` propriedade `True`. Em seguida, na marca inteligente, de ligação DropDownList para um novo ObjectDataSource denominado `CategoriesDataSource`. Configurar o ObjectDataSource para que ele recupere seus dados a partir de `CategoriesBLL` classe s `GetCategories` método. Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Recuperar dados do método GetCategories CategoriesBLL classe s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 7**: recuperar dados a partir de `CategoriesBLL` classe s `GetCategories` método ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Figura 8**: definir a lista suspensa na atualização, inserir e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))


Depois de concluir o assistente ObjectDataSource, configure DropDownList para exibir o `CategoryName` campo de dados e usar o `CategoryID` campo como o `Value` para cada `ListItem`.

Neste ponto, a marcação de declarativa s DropDownList e ObjectDataSource deve semelhante à seguinte:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

Em seguida, arraste um controle GridView para o Designer, colocando-o abaixo DropDownList. Definir o GridView s `ID` para `ProductsByCategory` e, na marca inteligente, de associá-lo a um novo ObjectDataSource denominado `ProductsByCategoryDataSource`. Configurar o `ProductsByCategoryDataSource` ObjectDataSource para usar o `ProductsBLLWithSprocs` classe, fazendo com que recuperar seus dados usando o `GetProductsByCategoryID(categoryID)` método. Pois este GridView somente será usado para exibir dados, definir as listas suspensas na atualização, inserção e excluir guias como (nenhum) e clique em Avançar.


[![Configurar o ObjectDataSource para usar a classe ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Figura 9**: configurar o ObjectDataSource para usar o `ProductsBLLWithSprocs` classe ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))


[![Recuperar dados do método GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Figura 10**: recuperar dados a partir de `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))


O método escolhido na guia selecione espera um parâmetro para a etapa final do assistente nos solicitará a origem do parâmetro s. Definir a lista suspensa de origem de parâmetros para controle e escolha o `Categories` controle da lista suspensa ControlID. Clique em Concluir para concluir o assistente.


[![Use DropDownList categorias como a origem do parâmetro categoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 11**: Use o `Categories` DropDownList como a origem do `categoryID` parâmetro ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


Após concluir o Assistente de ObjectDataSource, o Visual Studio adicionará BoundFields e um CheckBoxField para cada um dos campos de dados de produto. Fique à vontade para personalizar esses campos como desejar.

Visite a página por meio de um navegador. Ao visitar a página que está selecionada na categoria de bebidas e os produtos correspondentes listados na grade. Alterando a lista suspensa para uma categoria de alternativa, como a Figura 12 mostra, causa um postback e recarrega a grade com os produtos da categoria selecionada recentemente.


[![Os produtos na categoria produzir são exibidos](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 12**: os produtos na categoria produzir são exibidos ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Etapa 5: Encapsulamento instruções de s um procedimento armazenado dentro do escopo de uma transação

No [encapsulamento modificações de banco de dados em uma transação](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) tutorial abordamos as técnicas para executar uma série de instruções de modificação do banco de dados dentro do escopo de uma transação. Lembre-se que as modificações executadas sob a proteção de uma transação ou em todos os tenha êxito ou falha de todos os, garantindo atomicidade. Técnicas para usar transações incluem:

- Usando as classes de `System.Transactions` namespace
- Com a camada de acesso de dados usar classes do ADO.NET como `SqlTransaction`, e
- Adicionar os comandos de transação T-SQL diretamente dentro do procedimento armazenado

O *encapsulamento modificações de banco de dados em uma transação* tutorial usado as classes ADO.NET em DAL. O restante deste tutorial examina como gerenciar uma transação usando comandos T-SQL de um procedimento armazenado.

Os três comandos SQL chave para iniciar manualmente, confirmação e reversão de uma transação são `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, e `ROLLBACK TRANSACTION`, respectivamente. Como com a abordagem do ADO.NET, ao usar transações em um procedimento armazenado que é necessário aplicar o seguinte padrão:

1. Indica o início de uma transação.
2. Execute as instruções SQL que compõem a transação.
3. Se houver um erro em qualquer uma das instruções na etapa 2, reverter a transação.
4. Se todas as instruções da etapa 2 concluída sem erros, confirme a transação.

Esse padrão pode ser implementado na sintaxe do T-SQL usando o modelo a seguir:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

O modelo é iniciado, definindo um `TRY...CATCH` bloquear uma construção de nova no SQL Server 2005. Como com `Try...Catch` blocos no Visual Basic, o SQL `TRY...CATCH` as instruções no bloco é executado o `TRY` bloco. Se nenhuma instrução gera um erro, o controle é transferido imediatamente para o `CATCH` bloco.

Se não houver nenhum erro de executar as instruções SQL que a transação de composição de `COMMIT TRANSACTION` instrução confirma as alterações e conclui a transação. Se, no entanto, uma das instruções resulta em um erro, o `ROLLBACK TRANSACTION` no `CATCH` bloco retorna o banco de dados para seu estado anterior ao início da transação. O procedimento armazenado também gera um erro usando o [comando RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), que faz com que um `SqlException` a ser gerado no aplicativo.

> [!NOTE]
> Desde o `TRY...CATCH` bloco é novo no SQL Server 2005, o modelo acima não funcionará se você estiver usando versões anteriores do Microsoft SQL Server. Se você não estiver usando o SQL Server 2005, consulte [gerenciar transações em procedimentos armazenados do SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) para um modelo que funcione com outras versões do SQL Server.


Permitir que o s examinar um exemplo concreto. Existe uma restrição de chave estrangeira entre a `Categories` e `Products` tabelas, o que significa que cada `CategoryID` campo o `Products` tabela deve ser mapeado para um `CategoryID` valor o `Categories` tabela. Todas as ações que possam violar a essa restrição, como a tentativa de excluir uma categoria de produtos, associada resulta em uma violação de restrição de chave estrangeira. Para verificar isso, revise o exemplo atualizando e excluindo dados binários existentes a seção como trabalhar com dados binários (`~/BinaryData/UpdatingAndDeleting.aspx`). Esta página lista cada categoria no sistema junto com os botões Editar e excluir (consulte a Figura 13), mas se você tentar excluir uma categoria de produtos - como bebidas - associada a exclusão falha devido a uma violação de restrição de chave estrangeira (consulte a Figura 14).


[![Cada categoria é exibida em um GridView com botões Editar e excluir](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Figura 13**: cada categoria é exibida em um GridView com botões Editar e excluir ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))


[![Você não pode excluir uma categoria de produtos existentes](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Figura 14**: não é possível excluir uma categoria de produtos existentes ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))


No entanto, imagine que queremos permitir categorias a serem excluídos, independentemente se que estejam associados a produtos. Uma categoria de produtos deve ser excluída, imagine que queremos também excluirá seus produtos existentes (embora outra opção seria simplesmente definir seus produtos `CategoryID` valores `NULL`). Essa funcionalidade pode ser implementada por meio de regras de cascata da restrição foreign key. Como alternativa, é possível criar um procedimento armazenado que aceita um `@CategoryID` parâmetro de entrada e, quando chamado, explicitamente exclui todos os produtos associados e, em seguida, na categoria especificada.

Nossa primeira tentativa de tal procedimento armazenado pode parecer com o seguinte:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Enquanto isso definitivamente excluirá os produtos associados e a categoria, ele não faz isso sob a proteção de uma transação. Imagine que haja outra restrição de chave estrangeira em `Categories` que impeça a exclusão de um determinado `@CategoryID` valor. O problema é que nesse caso todos os produtos serão excluídos antes de tentar excluir a categoria. O resultado líquido é que para categoria, esse procedimento armazenado deve remover todos os seus produtos enquanto a categoria permaneceu desde que ele ainda tem registros relacionados de alguma outra tabela.

Se o procedimento armazenado foi encapsulado dentro do escopo de uma transação, no entanto, as exclusões para o `Products` tabela seria revertida no caso de uma falha ao excluir `Categories`. O script de procedimento armazenado a seguir usa uma transação para garantir a atomicidade entre os dois `DELETE` instruções:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Reserve um tempo para adicionar o `Categories_Delete` procedimento armazenado para o banco de dados Northwind. Voltar para a etapa 1 para obter instruções sobre como adicionar procedimentos armazenados para um banco de dados.

## <a name="step-6-updating-thecategoriestableadapter"></a>Etapa 6: Atualizando a`CategoriesTableAdapter`

Embora ve adicionado o `Categories_Delete` procedimento armazenado no banco de dados, a DAL está configurado para usar instruções SQL ad hoc para executar a exclusão. Precisamos atualizar o `CategoriesTableAdapter` e instrui-lo para usar o `Categories_Delete` procedimento armazenado em vez disso.

> [!NOTE]
> Anteriormente neste tutorial estivéssemos trabalhando com o `NorthwindWithSprocs` conjunto de dados. Mas esse conjunto de dados tem apenas uma única entidade, `ProductsDataTable`, e precisamos com categorias de trabalho. Portanto, para o restante deste tutorial quando falar sobre o m dados acesso camada I referindo-se ao `Northwind` conjunto de dados, que são criados pela primeira vez o [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-vb.md) tutorial.


Abra o Northwind DataSet, selecione o `CategoriesTableAdapter`e vá para a janela de propriedades. As listas de janela de propriedades de `InsertCommand`, `UpdateCommand`, `DeleteCommand`, e `SelectCommand` usada pelo TableAdapter, bem como suas informações de nome e a conexão. Expanda o `DeleteCommand` propriedade para ver seus detalhes. Como mostra a Figura 15, o `DeleteCommand` s `ComamndType` propriedade estiver definida como texto, que instrui-lo para enviar o texto no `CommandText` a propriedade como uma consulta SQL ad hoc.


![Selecione o CategoriesTableAdapter no Designer para exibir suas propriedades na janela Propriedades](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Figura 15**: selecione o `CategoriesTableAdapter` no Designer para exibir suas propriedades na janela Propriedades


Para alterar essas configurações, selecione o texto (DeleteCommand) na janela Propriedades e escolha (novo) na lista suspensa. Isso limpará as configurações para o `CommandText`, `CommandType`, e `Parameters` propriedades. Em seguida, defina o `CommandType` propriedade `StoredProcedure` e, em seguida, digite o nome do procedimento armazenado para o `CommandText` (`dbo.Categories_Delete`). Se você inserir as propriedades nesta ordem - primeiro o `CommandType` e, em seguida, o `CommandText` -Visual Studio populará automaticamente a coleção de parâmetros. Se você não inserir essas propriedades nesta ordem, você precisará adicionar manualmente os parâmetros por meio do Editor de coleção de parâmetros. Em ambos os casos, ele s Convém clique nas elipses na propriedade de parâmetros para trazer o Editor de coleção de parâmetros para verificar se as alterações de configurações de parâmetro corretos foram efetuadas (consulte a Figura 16). Se você não vir quaisquer parâmetros na caixa de diálogo, adicione o `@CategoryID` parâmetro manualmente (você não precisa adicionar o `@RETURN_VALUE` parâmetro).


![Certifique-se de que as configurações de parâmetros estão corretas](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 16**: Certifique-se de que as configurações de parâmetros estão corretas


Depois que a DAL tiver sido atualizada, a exclusão de uma categoria automaticamente excluirá todos os seus produtos associados e fazê-lo sob a proteção de uma transação. Para verificar isso, retornar à página de atualização e exclusão de dados binários existentes e clique no botão Excluir para uma das categorias. Com um único clique do mouse, a categoria e todos os seus produtos associados serão excluídos.

> [!NOTE]
> Antes de testar o `Categories_Delete` procedimento armazenado, o que excluirá um número de produtos juntamente com a categoria selecionada, talvez seja aconselhável fazer uma cópia de backup do banco de dados. Se você estiver usando o `NORTHWND.MDF` banco de dados `App_Data`, simplesmente fechar o Visual Studio e copie os arquivos MDF e LDF no `App_Data` para outra pasta. Depois de testar a funcionalidade, você pode restaurar o banco de dados fechando o Visual Studio e substituir atual MDF e LDF arquivos `App_Data` com as cópias de backup.


## <a name="summary"></a>Resumo

Enquanto o Assistente de s TableAdapter gerará automaticamente procedimentos armazenados para nós, há tempos quando podemos pode já ter esses procedimentos armazenados criados ou deseja criá-los manualmente ou com outras ferramentas em vez disso. Para acomodar esses cenários, o TableAdapter também pode ser configurado para apontar para um procedimento armazenado existente. Neste tutorial vimos como adicionar manualmente os procedimentos armazenados para um banco de dados por meio do ambiente do Visual Studio e como conectar os métodos TableAdapter para esses procedimentos armazenados. Também examinamos os comandos T-SQL e o padrão de script usada para iniciar, confirmar e reverter as transações de dentro de um procedimento armazenado.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Hilton Geisenow, S ren Lauritsen Jacó e Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Próximo](updating-the-tableadapter-to-use-joins-vb.md)
