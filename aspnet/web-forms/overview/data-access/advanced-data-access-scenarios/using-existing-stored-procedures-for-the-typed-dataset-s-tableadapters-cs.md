---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Usando existente de procedimentos armazenados para os TableAdapters do conjunto de dados tipado (c#) | Microsoft Docs
author: rick-anderson
description: No tutorial anterior, aprendemos como usar o Assistente do TableAdapter para gerar novos procedimentos armazenados. Neste tutorial, saiba como o mesmo TableAdapter...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 440bef2a-1641-4238-99e3-8e2d44e7d94c
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: df095a7eeac0910078cfa206ed1ba7be9a1334d2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831002"
---
<a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Usando existente de procedimentos armazenados para os TableAdapters do conjunto de dados tipado (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_CS.zip) ou [baixar PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial68cs1.pdf)

> No tutorial anterior, aprendemos como usar o Assistente do TableAdapter para gerar novos procedimentos armazenados. Neste tutorial, saiba como o mesmo assistente TableAdapter pode trabalhar com procedimentos armazenados existentes. Podemos também aprenderá a adicionar manualmente novos procedimentos armazenados ao nosso banco de dados.


## <a name="introduction"></a>Introdução

No [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) , vimos como o s TableAdapters do conjunto de dados tipado pode ser configurado para usar procedimentos armazenados para acesso dados em vez de ad hoc de instruções de SQL. Em particular, examinamos como TableAdapter assistente criar automaticamente esses procedimentos armazenados. Ao portar um aplicativo herdado para o ASP.NET 2.0 ou ao criar um site do ASP.NET 2.0 em torno de um modelo de dados existente, as chances são que o banco de dados já contém os procedimentos armazenados que precisamos. Como alternativa, talvez você prefira criar seus procedimentos armazenados, manualmente ou por meio de uma ferramenta que não seja o Assistente do TableAdapter que gera automaticamente seus procedimentos armazenados.

Neste tutorial vamos examinar como configurar o TableAdapter para usar procedimentos armazenados existentes. Como o banco de dados Northwind tem apenas um pequeno conjunto de procedimentos armazenados internos, podemos também examinar as etapas necessárias para adicionar manualmente novos procedimentos armazenados no banco de dados por meio do ambiente do Visual Studio. Permitir que o s começar!

> [!NOTE]
> No [de encapsulamento de modificações de banco de dados em uma transação](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) tutorial adicionamos métodos ao TableAdapter para dar suporte a transações (`BeginTransaction`, `CommitTransaction`e assim por diante). Como alternativa, as transações podem ser gerenciadas totalmente dentro de um procedimento armazenado, o que não exige que nenhuma modificação no código da camada de acesso a dados. Neste tutorial, exploraremos os comandos T-SQL usados para executar instruções de s um procedimento armazenado dentro do escopo de uma transação.


## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Etapa 1: Adicionar procedimentos armazenados no banco de dados Northwind

Visual Studio torna fácil adicionar novos procedimentos armazenados para um banco de dados. Let s adicionar um novo procedimento armazenado no banco de dados Northwind que retorna todas as colunas do `Products` tabela para aqueles que têm um determinado `CategoryID` valor. Na janela do Gerenciador de servidores, expanda o banco de dados Northwind, para que suas pastas - diagramas de banco de dados, tabelas, exibições e assim por diante - são exibidas. Como vimos no tutorial anterior, a pasta de procedimentos armazenados contém os banco de dados s os procedimentos armazenados existentes. Para adicionar um novo procedimento armazenado, clique com botão direito na pasta de procedimentos armazenados e escolha a opção de adicionar novo procedimento armazenado no menu de contexto.


[![Clique na pasta de procedimentos armazenados e adicione um novo procedimento armazenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Figura 1**: clique com botão direito na pasta de procedimentos armazenados e adicione um novo procedimento armazenado ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png))


Como mostra a Figura 1, selecionando a opção de adicionar novo procedimento armazenado abre uma janela de script no Visual Studio com a estrutura de tópicos do script SQL necessária para criar o procedimento armazenado. É nosso trabalho concretizar esse script e executá-lo, no ponto em que o procedimento armazenado será adicionado ao banco de dados.

Insira o seguinte script:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Esse script, quando executado, adicionará um novo procedimento armazenado ao banco de dados Northwind denominado `Products_SelectByCategoryID`. Esse procedimento armazenado aceita um parâmetro de entrada único (`@CategoryID`, do tipo `int`) e retorna todos os campos para esses produtos com uma correspondência `CategoryID` valor.

Para executar esta `CREATE PROCEDURE` de script e adicione o procedimento armazenado no banco de dados, clique no ícone Salvar na barra de ferramentas ou pressione Ctrl + S. Depois de fazer isso, as atualizações de pasta de procedimentos armazenados, mostrar o recém-criado procedimento armazenado. Além disso, o script na janela mudará sutileza do `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` à `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` Adiciona um novo procedimento armazenado no banco de dados, enquanto `ALTER PROCEDURE` atualiza uma existente. Desde o início do script foi alterado para `ALTER PROCEDURE`, alterar os procedimentos armazenados de entrada parâmetros ou instruções SQL e clicando no ícone Salvar, o procedimento armazenado será atualizado com essas alterações.

Figura 2 mostra o Visual Studio após a `Products_SelectByCategoryID` procedimento armazenado foi salvo.


[![O procedimento armazenado Products_SelectByCategoryID foi adicionado ao banco de dados](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png)

**Figura 2**: O procedimento armazenado `Products_SelectByCategoryID` foi adicionado ao banco de dados ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png))


## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Etapa 2: Configurar o TableAdapter para usar um procedimento armazenado existente

Agora que o `Products_SelectByCategoryID` procedimento armazenado foi adicionado ao banco de dados, podemos concigure nossa camada de acesso de dados para usar esse procedimento armazenado quando um de seus métodos é invocado. Em particular, vamos adicionar um `GetProducstByCategoryID(categoryID)` método para o `ProductsTableAdapter` na `NorthwindWithSprocs` conjunto de dados tipado que chama o `Products_SelectByCategoryID` que acabamos de criar do procedimento armazenado.

Comece abrindo o `NorthwindWithSprocs` conjunto de dados. Clique com botão direito no `ProductsTableAdapter` e escolha Add Query para iniciar o Assistente de configuração de consulta do TableAdapter. No [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) , optei por ter o TableAdapter criar um novo procedimento armazenado para nós. Para este tutorial, no entanto, que desejamos conectar o novo método do TableAdapter para existente `Products_SelectByCategoryID` procedimento armazenado. Portanto, escolher a opção de procedimento armazenado existente usar a primeira etapa do assistente s e, em seguida, clique em Avançar.


[![Escolha o uso de opção de procedimento armazenado de existente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)

**Figura 3**: escolha usar existente armazenado procedimento opção ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png))


A tela a seguir fornece que uma lista suspensa preenchida com o banco de dados s procedimentos armazenados. Selecionar um procedimento armazenado lista seus parâmetros de entrada à esquerda e os campos de dados retornados (se houver) à direita. Escolha o `Products_SelectByCategoryID` procedimento armazenado a partir da lista e clique em Avançar.


[![Escolher o Products_SelectByCategoryID procedimento armazenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)

**Figura 4**: escolha o `Products_SelectByCategoryID` procedimento armazenado ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png))


A próxima tela pergunta que tipo de dados é retornado pelo procedimento armazenado e nossa resposta aqui determina o tipo retornado pelo método s TableAdapter. Por exemplo, se podemos indicar que os dados de tabela são retornados, o método retornará um `ProductsDataTable` instância preenchida com os registros retornados pelo procedimento armazenado. Por outro lado, se podemos indicar que esse procedimento armazenado retorna um valor único TableAdapter retornará um `object` que recebe o valor na primeira coluna do primeiro registro retornado pelo procedimento armazenado.

Uma vez que o `Products_SelectByCategoryID` procedimento armazenado retorna todos os produtos que pertencem a uma categoria específica, escolha a primeira resposta - dados tabulares – e clique em Avançar.


[![Indicar que o procedimento armazenado retorna dados tabulares](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)

**Figura 5**: indica que o procedimento armazenado retorna dados de tabela ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png))


Tudo o que resta é indicar quais padrões de método para usar seguido de nomes para esses métodos. Deixe os dois o preenchimento de uma DataTable e retornar uma DataTable opções verificada, mas renomear os métodos para `FillByCategoryID` e `GetProductsByCategoryID`. Em seguida, clique em Avançar para examinar um resumo das tarefas que o assistente executará. Se tudo estiver correto, clique em Concluir.


[![Nome FillByCategoryID os métodos e GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Figura 6**: nomear os métodos `FillByCategoryID` e `GetProductsByCategoryID` ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


> [!NOTE]
> Os métodos TableAdapter que acabamos de criar `FillByCategoryID` e `GetProductsByCategoryID`, espera um parâmetro de entrada do tipo `int`. Esse valor de parâmetro de entrada é passado para o procedimento armazenado por meio de seu `@CategoryID` parâmetro. Se você modificar o `Products_SelectByCategory` armazenados parâmetros de procedimento s, será necessário também atualizar os parâmetros para esses métodos do TableAdapter. Conforme discutido na [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md), isso pode ser feito de duas maneiras: manualmente adicionando ou removendo parâmetros da coleção de parâmetros ou executar novamente o Assistente do TableAdapter.


## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Etapa 3: Adicionando um`GetProductsByCategoryID(categoryID)`método para a BLL

Com o `GetProductsByCategoryID` método DAL completo, a próxima etapa é fornecer acesso a esse método na camada de lógica de negócios. Abra o `ProductsBLLWithSprocs` arquivo de classe e adicione o seguinte método:


[!code-csharp[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.cs)]

Esse método BLL simplesmente retorna o `ProductsDataTable` retornado do `ProductsTableAdapter` s `GetProductsByCategoryID` método. O `DataObjectMethodAttribute` atributo fornece metadados usados pelo assistente ObjectDataSource s configurar fonte de dados. Em particular, este método será exibido na lista suspensa Selecione guia s.

## <a name="step-4-displaying-products-by-category"></a>Etapa 4: Exibindo produtos por categoria

Para testar o recém-adicionado `Products_SelectByCategoryID` procedimento armazenado e os métodos correspondentes de permitir que o s crie uma página ASP.NET que contém uma DropDownList e GridView DAL e BLL. DropDownList listará todas as categorias no banco de dados enquanto o GridView exibirá os produtos que pertencem à categoria selecionada.

> [!NOTE]
> Podemos var criado interfaces de mestre/detalhes usando DropDownLists nos tutoriais anteriores. Para obter uma visão mais detalhada sobre a implementação de tal relatório mestre/detalhes, consulte o [filtragem de mestre/detalhes com uma DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) tutorial.


Abra o `ExistingSprocs.aspx` página o `AdvancedDAL` pasta e arraste uma DropDownList da caixa de ferramentas para o Designer. Definir o s DropDownList `ID` propriedade para `Categories` e seu `AutoPostBack` propriedade `true`. Em seguida, na marca inteligente, de associar a DropDownList a um novo ObjectDataSource chamado `CategoriesDataSource`. Configurar o ObjectDataSource para que ele recupere seus dados a partir de `CategoriesBLL` classe s `GetCategories` método. Defina as listas suspensas na atualização, inserção e excluir guias como (nenhum).


[![Recuperar dados do método GetCategories CategoriesBLL classe s](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Figura 7**: recupere dados do `CategoriesBLL` classe s `GetCategories` método ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png))


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png)

**Figura 8**: defina a lista suspensa no UPDATE, INSERT e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png))


Depois de concluir o assistente ObjectDataSource, configurar a DropDownList para exibir o `CategoryName` campo de dados e usar o `CategoryID` campo como o `Value` para cada `ListItem`.

Neste ponto, DropDownList e ObjectDataSource s marcação declarativa deve semelhante ao seguinte:


[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.aspx)]

Em seguida, arraste um GridView para o Designer, colocando-o abaixo de DropDownList. Definir o s GridView `ID` à `ProductsByCategory` e, na marca inteligente, de associá-lo a um novo ObjectDataSource chamado `ProductsByCategoryDataSource`. Configurar o `ProductsByCategoryDataSource` ObjectDataSource para usar o `ProductsBLLWithSprocs` classe, que ela recupere seus dados usando o `GetProductsByCategoryID(categoryID)` método. Porque este GridView somente será usado para exibir dados, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum) e clique em Avançar.


[![Configurar o ObjectDataSource para usar a classe ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png)

**Figura 9**: configurar o ObjectDataSource para usar o `ProductsBLLWithSprocs` classe ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png))


[![Recuperar dados do método GetProductsByCategoryID(categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)

**Figura 10**: recupere dados do `GetProductsByCategoryID(categoryID)` método ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png))


O método escolhido na guia selecione espera um parâmetro, portanto, a etapa final do assistente nos solicita a origem do parâmetro s. Defina a lista de lista suspensa de origem do parâmetro para o controle e escolha o `Categories` controle na lista suspensa ControlID. Clique em Concluir para concluir o assistente.


[![Use Categories DropDownList como a origem do parâmetro categoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Figura 11**: Use o `Categories` DropDownList como a origem do `categoryID` parâmetro ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


Depois de concluir o assistente ObjectDataSource, o Visual Studio adicionará BoundFields e um CheckBoxField para cada um dos campos de dados do produto. Fique à vontade personalizar esses campos conforme necessário.

Visite a página por meio de um navegador. Ao visitar a página de que categoria Bebidas está selecionada e os produtos correspondentes listados na grade. Alterando a lista suspensa para uma categoria de alternativa, como a Figura 12 mostra, faz com que um postback e recarrega a grade com os produtos da categoria selecionada recentemente.


[![Os produtos na categoria de produzir são exibidos](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Figura 12**: os produtos na categoria de produzir são exibidos ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png))


## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Etapa 5: Quebra automática de instruções de s um procedimento armazenado dentro do escopo de uma transação

No [de encapsulamento de modificações de banco de dados em uma transação](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md) tutorial discutimos as técnicas para executar uma série de instruções de modificação do banco de dados dentro do escopo de uma transação. Lembre-se que as modificações executadas sob o guarda-chuva de uma transação ou em todos os tenha êxito ou falha de todos os, garantindo atomicidade. Técnicas para usar transações incluem:

- Usando as classes no `System.Transactions` namespace,
- Com a camada de acesso a dados usar classes do ADO.NET como `SqlTransaction`, e
- Adicionando os comandos de transação do T-SQL diretamente dentro do procedimento armazenado

O *de encapsulamento de modificações de banco de dados em uma transação* tutorial usou as classes ADO.NET no DAL. O restante deste tutorial examina como gerenciar uma transação usando comandos do T-SQL de dentro de um procedimento armazenado.

Os três comandos SQL principais para iniciar manualmente, confirmar e reverter uma transação são `BEGIN TRANSACTION`, `COMMIT TRANSACTION`, e `ROLLBACK TRANSACTION`, respectivamente. Como com a abordagem do ADO.NET, ao usar transações de dentro de um procedimento armazenado, é necessário aplicar o seguinte padrão:

1. Indica o início de uma transação.
2. Execute as instruções SQL que compõem a transação.
3. Se houver um erro em qualquer uma das instruções da etapa 2, reverta a transação
4. Se todas as instruções da etapa 2 concluída sem erro, confirme a transação.

Esse padrão pode ser implementado na sintaxe do T-SQL usando o modelo a seguir:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]

O modelo começa definindo um `TRY...CATCH` bloquear uma construção de nova no SQL Server 2005. Como com `try...catch` blocos em c#, o SQL `TRY...CATCH` as instruções no bloco é executado o `TRY` bloco. Se nenhuma instrução gera um erro, o controle é transferido imediatamente para o `CATCH` bloco.

Se não houver nenhum erro de execução de instruções SQL que a transação de composição a `COMMIT TRANSACTION` instrução confirma as alterações e conclui a transação. Se, no entanto, uma das instruções resulta em um erro, o `ROLLBACK TRANSACTION` no `CATCH` bloco retorna o banco de dados para seu estado antes do início da transação. O procedimento armazenado também gerará um erro usando o [comando RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), que faz com que um `SqlException` a ser gerado no aplicativo.

> [!NOTE]
> Uma vez que o `TRY...CATCH` bloco é novo no SQL Server 2005, o modelo acima não funcionarão se você estiver usando versões mais antigas do Microsoft SQL Server. Se você não estiver usando o SQL Server 2005, consulte [gerenciamento de transações em procedimentos armazenados do SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) de um modelo que funcionará com outras versões do SQL Server.


Deixe o s examinar um exemplo concreto. Uma restrição de chave estrangeira existe entre o `Categories` e `Products` tabelas, o que significa que cada `CategoryID` campo o `Products` tabela deve ser mapeado para um `CategoryID` valor no `Categories` tabela. Qualquer ação que faria violam essa restrição, como a tentativa de excluir uma categoria que associou os produtos, resulta em uma violação de restrição de chave estrangeira. Para verificar isso, examine o exemplo atualizando e excluindo dados binários existentes no seção como trabalhar com dados binários (`~/BinaryData/UpdatingAndDeleting.aspx`). Esta página lista cada categoria no sistema, juntamente com os botões Editar e excluir (veja a Figura 13), mas se você tentar excluir uma categoria que associou os produtos - como bebidas - a exclusão falha devido a uma violação de restrição de chave estrangeira (consulte a Figura 14).


[![Cada categoria é exibida em um GridView com botões Editar e excluir](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png)

**Figura 13**: cada categoria é exibida em um GridView com botões Editar e excluir ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png))


[![Você não pode excluir uma categoria de produtos existentes](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png)

**Figura 14**: você não pode excluir uma categoria de produtos existentes ([clique para exibir a imagem em tamanho normal](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png))


No entanto, imagine que desejamos permitir categorias a serem excluídos, independentemente se eles têm associados a produtos. Uma categoria de produtos deve ser excluída, imagine que queremos também excluir seus produtos existentes (embora a outra opção seria simplesmente definir seus produtos `CategoryID` valores `NULL`). Essa funcionalidade pode ser implementada por meio das regras em cascata de restrição de chave estrangeira. Como alternativa, podemos pode criar um procedimento armazenado que aceita um `@CategoryID` parâmetro de entrada e, quando invocado, explicitamente exclui todos os produtos associados e, em seguida, a categoria especificada.

Nossa primeira tentativa de tal procedimento armazenado pode parecer com o seguinte:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

Enquanto isso definitivamente excluirá os produtos associados e a categoria, ele não faz isso sob o guarda-chuva de uma transação. Imagine que há outra restrição de chave estrangeira na `Categories` seria proibido a exclusão de um determinado `@CategoryID` valor. O problema é que nesse caso, todos os produtos serão excluídos antes de tentar a excluir a categoria. O resultado líquido é que para a categoria, esse procedimento armazenado deve remover todos os seus produtos enquanto a categoria permaneceu, pois ele ainda tem registros relacionados de alguma outra tabela.

Se o procedimento armazenado foram encapsulado dentro do escopo de uma transação, no entanto, as exclusões para o `Products` tabela seria revertida em caso de uma falha ao excluir no `Categories`. O script de procedimento armazenado a seguir usa uma transação para garantir a atomicidade entre os dois `DELETE` instruções:


[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.sql)]

Reserve um tempo para adicionar o `Categories_Delete` procedimento armazenado para o banco de dados Northwind. Voltar para a etapa 1 para obter instruções sobre como adicionar procedimentos armazenados para um banco de dados.

## <a name="step-6-updating-thecategoriestableadapter"></a>Etapa 6: Atualizando a`CategoriesTableAdapter`

Enquanto criamos adicionou o `Categories_Delete` procedimento armazenado no banco de dados, a DAL está atualmente configurado para usar instruções SQL ad hoc para executar a exclusão. Precisamos atualizar o `CategoriesTableAdapter` e instruí-lo a usar o `Categories_Delete` procedimento armazenado em vez disso.

> [!NOTE]
> Neste tutorial que estávamos trabalhando com o `NorthwindWithSprocs` conjunto de dados. Mas esse conjunto de dados tem apenas uma única entidade, `ProductsDataTable`, e precisamos trabalhar com categorias. Portanto, para o restante deste tutorial, quando eu falar sobre o Data Access camada I m referindo-se à `Northwind` conjunto de dados, que primeiro criamos na [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) tutorial.


Abra o Northwind DataSet, selecione o `CategoriesTableAdapter`e vá para a janela Propriedades. As listas de janela de propriedades de `InsertCommand`, `UpdateCommand`, `DeleteCommand`, e `SelectCommand` usados pelo TableAdapter, assim como suas informações de nome e a conexão. Expanda o `DeleteCommand` propriedade para ver seus detalhes. Como mostra a Figura 15, o `DeleteCommand` s `ComamndType` estiver definida como texto, que instrui a enviar o texto no `CommandText` a propriedade como uma consulta SQL ad hoc.


![Selecione o CategoriesTableAdapter no Designer para exibir suas propriedades na janela Propriedades](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png)

**Figura 15**: selecione o `CategoriesTableAdapter` no Designer para exibir suas propriedades na janela Propriedades


Para alterar essas configurações, selecione o texto (DeleteCommand) na janela Propriedades e escolha (novo) na lista suspensa. Isso limpará as configurações para o `CommandText`, `CommandType`, e `Parameters` propriedades. Em seguida, defina as `CommandType` propriedade para `StoredProcedure` e, em seguida, digite o nome do procedimento armazenado para o `CommandText` (`dbo.Categories_Delete`). Se você certifique-se de inserir as propriedades nesta ordem - pela primeira vez o `CommandType` e, em seguida, o `CommandText` -Visual Studio populará automaticamente a coleção de parâmetros. Se você não inserir essas propriedades nesta ordem, você precisará adicionar manualmente os parâmetros por meio do Editor de coleção de parâmetros. Em ambos os casos, ele s prudente clicar nas elipses na propriedade parâmetros para transferir anterior o Editor de coleção de parâmetros para verificar que as alterações de configurações de parâmetro corretos foram feitas (consulte a Figura 16). Se você não vir quaisquer parâmetros na caixa de diálogo, adicione a `@CategoryID` parâmetro manualmente (você não precisará adicionar o `@RETURN_VALUE` parâmetro).


![Certifique-se de que as configurações de parâmetros estão corretas](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Figura 16**: Certifique-se de que as configurações de parâmetros estão corretas


Depois que a DAL tiver sido atualizada, excluir uma categoria automaticamente excluirá todos os seus produtos associados e fazê-lo sob o guarda-chuva de uma transação. Para verificar isso e retornar à página atualizando e excluindo dados binários existentes e clique no botão Excluir para uma das categorias. Com um único clique do mouse, a categoria e todos os seus produtos associados serão excluídos.

> [!NOTE]
> Antes de testar o `Categories_Delete` procedimento armazenado, o que excluirá um número de produtos juntamente com a categoria selecionada, talvez seja prudente ao fazer uma cópia de backup do banco de dados. Se você estiver usando o `NORTHWND.MDF` banco de dados no `App_Data`, basta fechar o Visual Studio e copie os arquivos MDF e LDF no `App_Data` para outra pasta. Depois de testar a funcionalidade, você pode restaurar o banco de dados fechando o Visual Studio e substituir atual MDF e LDF arquivos no `App_Data` com as cópias de backup.


## <a name="summary"></a>Resumo

Enquanto o Assistente de s TableAdapter gerará automaticamente os procedimentos armazenados para nós, há vezes quando podemos pode já ter esses procedimentos armazenados criados ou deseja criá-las manualmente ou com outras ferramentas em vez disso. Para acomodar esses cenários, o TableAdapter também pode ser configurado para apontar para um procedimento armazenado existente. Neste tutorial vimos como adicionar manualmente os procedimentos armazenados para um banco de dados por meio do ambiente do Visual Studio e como conectar os métodos do TableAdapter s a esses procedimentos armazenados. Também examinamos os comandos T-SQL e o padrão de script usado para iniciar, confirmar e reverter as transações de dentro de um procedimento armazenado.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Geisenow Hilton, S ren Jacob Lauritsen e Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [Próximo](updating-the-tableadapter-to-use-joins-cs.md)
