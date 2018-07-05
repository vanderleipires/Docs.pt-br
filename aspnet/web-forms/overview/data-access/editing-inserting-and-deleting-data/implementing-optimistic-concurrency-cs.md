---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
title: Implementando a simultaneidade otimista (c#) | Microsoft Docs
author: rick-anderson
description: Para um aplicativo web que permite aos usuários editarem dados, há o risco de que dois usuários podem estar editando os mesmos dados ao mesmo tempo. Nesse tutori...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 56e15b33-93b8-43ad-8e19-44c6647ea05c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
msc.type: authoredcontent
ms.openlocfilehash: 120fdca43b7e68127277f6889504f173938117e4
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397764"
---
<a name="implementing-optimistic-concurrency-c"></a>Implementando a simultaneidade otimista (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_CS.exe) ou [baixar PDF](implementing-optimistic-concurrency-cs/_static/datatutorial21cs1.pdf)

> Para um aplicativo web que permite aos usuários editarem dados, há o risco de que dois usuários podem estar editando os mesmos dados ao mesmo tempo. Neste tutorial, implementaremos o controle de simultaneidade otimista para tratar desse risco.


## <a name="introduction"></a>Introdução

Para aplicativos web que só permitem que os usuários exibam dados ou para aqueles que incluem apenas um único usuário quem pode modificar dados, não há nenhuma ameaça de dois usuários simultâneos substituição acidental de alterações uma da outra. Para aplicativos web que permitem que vários usuários atualizar ou excluir dados, no entanto, há o potencial para que as modificações de um usuário podem conflitar com outro usuário simultâneo. Sem qualquer política de simultaneidade em vigor, quando dois usuários simultaneamente estiver editando um único registro, o usuário que confirma as alterações pela última vez substituirá as alterações feitas pelo primeiro.

Por exemplo, imagine que ambos dois usuários, Jisun e Sam, foram visitar uma página em nosso aplicativo que os visitantes para atualizar e excluir os produtos por meio de um controle GridView de permissão. Ambos clique no botão Editar em GridView quase ao mesmo tempo. Jisun altera o nome do produto para "Chai chá" e clicar no botão de atualização. O resultado é um `UPDATE` instrução é enviada para o banco de dados que define *todas as* dos campos de atualizável do produto (embora Jisun atualizados somente um campo, `ProductName`). Neste momento, o banco de dados tem os valores "Chai chá," a categoria Bebidas, o fornecedor líquidos exóticos, e assim por diante para este produto específico. No entanto, o GridView na tela de Samuel ainda mostra o nome do produto na linha de GridView editável como "Chai". Alguns segundos depois de alterações do Jisun foram confirmadas, Sam atualiza a categoria para Condimentos e clica em Atualizar. Isso resulta em uma `UPDATE` instrução enviada ao banco de dados que define o nome do produto a Chai"," o `CategoryID` para a ID da categoria Bebidas correspondente e assim por diante. Alterações do Jisun para o nome do produto foram substituídas. Figura 1 representa graficamente a esta série de eventos.


[![Quando dois usuários simultaneamente atualizam um registro lá s potencial para um usuário s é alterada para substituir os outros s](implementing-optimistic-concurrency-cs/_static/image2.png)](implementing-optimistic-concurrency-cs/_static/image1.png)

**Figura 1**: quando dois usuários simultaneamente a atualização um registro lá s potencial para um usuário s muda para substituir os outros s ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image3.png))


Da mesma forma, quando dois usuários visitam uma página, um usuário pode ser no meio da atualização de um registro quando ele é excluído por outro usuário. Ou então, entre quando um usuário carrega uma página e ao clicar no botão Excluir, outro usuário pode ter modificado o conteúdo desse registro.

Há três [controle de simultaneidade](http://en.wikipedia.org/wiki/Concurrency_control) estratégias disponíveis:

- **Não faça nada** -se usuários simultâneos estiver modificando o mesmo registro, permitem que a última confirmação win (o comportamento padrão)
- [**A simultaneidade otimista** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -supõem que embora possa haver simultaneidade está em conflito vira e mexe, a grande maioria das vezes esses conflitos não ocorrem; portanto, se ocorrer um conflito, basta informar ao usuário que suas alterações não pode ser salvo porque outro usuário modificou os mesmos dados
- **Simultaneidade pessimista** -pressupõem que conflitos de simultaneidade são comuns e que os usuários não toleram sendo informado de suas alterações não foram salvas devido à atividade simultânea de outro usuário; portanto, quando um usuário inicia a atualização de um registro, bloqueá-la , impedindo assim a quaisquer outros usuários de editar ou excluir esse registro até que o usuário confirma suas modificações

Até agora todos os nossos tutoriais de tem usado a estratégia de resolução de simultaneidade padrão -, ou seja, permitimos já que a última gravação win. Neste tutorial, examinaremos como implementar o controle de simultaneidade otimista.

> [!NOTE]
> Não vamos ver exemplos de simultaneidade pessimista nessa série de tutoriais. Simultaneidade pessimista é raramente usada porque, como bloqueios, se não for liberada à medida corretamente, pode impedir que outros usuários atualizem os dados. Por exemplo, se um usuário bloqueia um registro para edição e, em seguida, sai para o dia anterior desbloqueá-lo, nenhum outro usuário será capaz de atualizar esse registro até que o usuário original retorna e conclui sua atualização. Portanto, em situações em que a simultaneidade pessimista é usada, normalmente há um tempo limite que, se atingido, cancela o bloqueio. Sites de vendas tíquete, que bloquearem um local específico de assentos por curto período enquanto o usuário conclui o processo de pedido, são um exemplo de controle de simultaneidade pessimista.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Etapa 1: Examinar como a simultaneidade otimista é implementada.

Controle de simultaneidade otimista funciona, garantindo que o registro que está sendo atualizada ou excluída tem os mesmos valores de quando a atualização ou exclusão de processo iniciado. Por exemplo, ao clicar no botão Editar em um GridView editável, os valores do registro são de leitura do banco de dados e exibidos em caixas de texto e outros controles de Web. Esses valores originais são salvos por GridView. Posteriormente, depois que o usuário faz as alterações dela e clica no botão de atualização, os valores originais e os novos valores são enviados para a camada de lógica de negócios e, em seguida, para baixo até a camada de acesso a dados. A camada de acesso a dados deve emitir uma instrução SQL que será apenas atualizar o registro se os valores originais que o usuário iniciou a edição são idênticos aos valores ainda no banco de dados. Figura 2 ilustra essa sequência de eventos.


[![Para a atualização ou exclusão seja bem-sucedida, os valores originais devem ser iguais aos valores atuais do banco de dados](implementing-optimistic-concurrency-cs/_static/image5.png)](implementing-optimistic-concurrency-cs/_static/image4.png)

**Figura 2**: para Update ou Delete para forem bem-sucedidas, o Original valores deve ser igual aos valores atuais do banco de dados ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image6.png))


Há várias abordagens para implementar a simultaneidade otimista (consulte [Peter A. Bromberg](http://peterbromberg.net/)do [Optmistic simultaneidade atualizando lógica](http://www.eggheadcafe.com/articles/20050719.asp) para examinar uma série de opções). O conjunto de dados tipados do ADO.NET fornece uma implementação que pode ser configurada com a escala de uma caixa de seleção. Habilitar a simultaneidade otimista para um TableAdapter no conjunto de dados tipado aumenta o TableAdapter `UPDATE` e `DELETE` instruções para incluir uma comparação de todos os valores originais no `WHERE` cláusula. O seguinte `UPDATE` instrução, por exemplo, atualiza o nome e o preço de um produto somente se os valores atuais do banco de dados são iguais aos valores que foram originalmente recuperados ao atualizar o registro em um GridView. O `@ProductName` e `@UnitPrice` parâmetros contêm os novos valores inseridos pelo usuário, enquanto `@original_ProductName` e `@original_UnitPrice` contêm os valores que foram carregados originalmente no GridView, quando o botão de edição foi clicado:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample1.sql)]

> [!NOTE]
> Isso `UPDATE` instrução foi simplificada para facilitar a leitura. Na prática, o `UnitPrice` check-in a `WHERE` cláusula seria mais complicada desde `UnitPrice` pode conter `NULL` s e verificando se `NULL = NULL` sempre retorna False (em vez disso, você deve usar `IS NULL`).


Além de usar um diferente subjacente `UPDATE` métodos diretos de instrução, configurando um TableAdapter para usar otimista de simultaneidade também modifica a assinatura do seu banco de dados. Lembre-se de nosso primeiro tutorial, [ *criando uma camada de acesso de dados*](../introduction/creating-a-data-access-layer-cs.md), que o banco de dados de métodos diretos são aquelas que aceita uma lista de escalar valores como parâmetros de entrada (em vez de uma DataRow fortemente tipada ou Instância de DataTable). Ao usar a simultaneidade otimista, o banco de dados direta `Update()` e `Delete()` métodos incluem parâmetros de entrada para os valores originais. Além disso, o código na BLL para usar o lote atualizar padrão (o `Update()` sobrecargas de método que aceitam DataRows e tabelas de dados em vez de valores escalares) deve ser alterada também.

Em vez disso, que estendem o nosso existente TableAdapters da DAL para usar a simultaneidade otimista (que necessitaria de alteração para acomodar a BLL), vamos criar um novo conjunto de dados tipado nomeado `NorthwindOptimisticConcurrency`, para que, adicionaremos um `Products` TableAdapter que usa a simultaneidade otimista. Depois disso, vamos criar um `ProductsOptimisticConcurrencyBLL` classe da camada de lógica de negócios com as modificações apropriadas para dar suporte a simultaneidade otimista DAL. Depois que esse trabalho de fundação layout foi definido, estaremos prontos para criar a página ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Etapa 2: Criando uma camada de acesso a dados que dá suporte à simultaneidade otimista

Para criar um novo DataSet digitado, clique duas vezes no `DAL` pasta dentro de `App_Code` pasta e adicione um novo DataSet denominado `NorthwindOptimisticConcurrency`. Como vimos no primeiro tutorial, fazer irá adicionar um novo TableAdapter para o conjunto de dados tipado, inicie automaticamente o Assistente de configuração do TableAdapter. Na primeira tela, serão solicitados a especificar o banco de dados para se conectar ao – conectar ao mesmo banco de dados Northwind usando o `NORTHWNDConnectionString` configuração do `Web.config`.


[![Conectar-se ao mesmo banco de dados Northwind](implementing-optimistic-concurrency-cs/_static/image8.png)](implementing-optimistic-concurrency-cs/_static/image7.png)

**Figura 3**: Conecte-se ao mesmo banco de dados Northwind ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image9.png))


Em seguida, podemos são solicitados a confirmar como consultar os dados: por meio de uma instrução SQL ad hoc, um novo procedimento armazenado ou procedimento armazenado de um existente. Como usamos consultas SQL ad hoc em nossa DAL original, use essa opção aqui também.


[![Especifique os dados a serem recuperados usando uma instrução de SQL Ad Hoc](implementing-optimistic-concurrency-cs/_static/image11.png)](implementing-optimistic-concurrency-cs/_static/image10.png)

**Figura 4**: especifica os dados para recuperar usando uma instrução de SQL Ad Hoc ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image12.png))


Na tela seguinte, insira a consulta SQL a ser usada para recuperar as informações do produto. Vamos usar a mesma exata consulta SQL usada para o `Products` TableAdapter de nossa DAL original, que retorna todos os `Product` colunas junto com os nomes de categoria e fornecedor do produto:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample2.sql)]


[![Usar a mesma consulta SQL do TableAdapter de produtos em DAL Original](implementing-optimistic-concurrency-cs/_static/image14.png)](implementing-optimistic-concurrency-cs/_static/image13.png)

**Figura 5**: usar a mesma consulta SQL do `Products` TableAdapter no Original DAL ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image15.png))


Antes de passar para a próxima tela, clique no botão Opções avançadas. Para que esse controle de simultaneidade otimista empregam TableAdapter, basta marcar a caixa de seleção "Usar simultaneidade otimista".


[![Habilitar o controle de simultaneidade otimista, verificando o &quot;usar a simultaneidade otimista&quot; caixa de seleção](implementing-optimistic-concurrency-cs/_static/image17.png)](implementing-optimistic-concurrency-cs/_static/image16.png)

**Figura 6**: habilitar o controle de simultaneidade otimista, marcando a caixa de seleção "Usar simultaneidade otimista" ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image18.png))


Por fim, indicam que o TableAdapter deve usar os padrões de acesso a dados que preencher uma DataTable e retornam uma DataTable; também indicam que os métodos diretos do banco de dados devem ser criados. Altere o nome do método para o retorno de um padrão de DataTable de GetData GetProducts, para espelhar as convenções de nomenclatura que usamos em nossa DAL original.


[![Ter o TableAdapter utilizar todos os padrões de acesso a dados](implementing-optimistic-concurrency-cs/_static/image20.png)](implementing-optimistic-concurrency-cs/_static/image19.png)

**Figura 7**: tem o TableAdapter utilizar todos os dados de padrões de acesso ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image21.png))


Depois de concluir o assistente, o Designer de conjunto de dados incluirá fortemente tipado `Products` DataTable e TableAdapter. Reserve um tempo para renomear o DataTable do `Products` para `ProductsOptimisticConcurrency`, que pode ser feito clicando duas vezes na barra de título da DataTable e escolhendo Renomear no menu de contexto.


[![A DataTable e TableAdapter foram adicionados ao conjunto de dados tipado](implementing-optimistic-concurrency-cs/_static/image23.png)](implementing-optimistic-concurrency-cs/_static/image22.png)

**Figura 8**: um DataTable e TableAdapter foram adicionados ao conjunto de dados tipado ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image24.png))


Para ver as diferenças entre o `UPDATE` e `DELETE` consultas entre o `ProductsOptimisticConcurrency` TableAdapter (que usa a simultaneidade otimista) e o TableAdapter de produtos (que não), clique no TableAdapter e vá para a janela Propriedades. No `DeleteCommand` e `UpdateCommand` propriedades `CommandText` subpropriedades que você pode ver a sintaxe SQL real que é enviada para o banco de dados quando a DAL update ou métodos de exclusão são invocados. Para o `ProductsOptimisticConcurrency` TableAdapter o `DELETE` instrução usada é:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample3.sql)]

Enquanto o `DELETE` instrução para o TableAdapter de produto na nossa DAL original é muito mais simples:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample4.sql)]

Como você pode ver, o `WHERE` cláusula em de `DELETE` instrução para o TableAdapter que usa a simultaneidade otimista inclui uma comparação entre cada um do `Product` tabela existente de valores de coluna e os valores originais no momento a GridView (ou DetailsView ou FormView) foi populado pela última vez. Desde que todos os campos diferente de `ProductID`, `ProductName`, e `Discontinued` pode ter `NULL` valores, parâmetros adicionais e verificações estão incluídas para comparar corretamente `NULL` valores no `WHERE` cláusula.

Nós não adicionar qualquer DataTables adicionais ao DataSet habilitados para simultaneidade otimista para este tutorial, conforme nossa página do ASP.NET só fornecerá a atualização e exclusão de informações do produto. No entanto, ainda precisamos adicionar o `GetProductByProductID(productID)` método para o `ProductsOptimisticConcurrency` TableAdapter.

Para fazer isso, clique com botão direito na barra de título do TableAdapter (à direita da área acima de `Fill` e `GetProducts` nomes de método) e escolha Add Query no menu de contexto. Isso iniciará o Assistente de configuração de consulta do TableAdapter. Como com a configuração inicial do nosso TableAdapter, optar por criar o `GetProductByProductID(productID)` método usando uma instrução de SQL ad hoc (veja a Figura 4). Uma vez que o `GetProductByProductID(productID)` método retorna informações sobre um produto específico, para indicar que esta consulta é um `SELECT` tipo que retorna linhas de consulta.


[![Marque o tipo de consulta como um &quot;SELECT que retorna linhas&quot;](implementing-optimistic-concurrency-cs/_static/image26.png)](implementing-optimistic-concurrency-cs/_static/image25.png)

**Figura 9**: marque o tipo de consulta como uma "`SELECT` que retorna linhas" ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image27.png))


Na próxima tela, serão solicitados para a consulta SQL usar com a consulta do TableAdapter padrão previamente carregada. Ampliar a consulta existente para incluir a cláusula `WHERE ProductID = @ProductID`, conforme mostrado na Figura 10.


[![Adicionar um onde cláusula para a consulta previamente carregada para retornar um registro de produto específico](implementing-optimistic-concurrency-cs/_static/image29.png)](implementing-optimistic-concurrency-cs/_static/image28.png)

**Figura 10**: adicionar um `WHERE` cláusula para a consulta Pre-Loaded para retornar um registro de produto específico ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image30.png))


Por fim, altere os nomes de método gerado `FillByProductID` e `GetProductByProductID`.


[![Renomear os métodos FillByProductID e GetProductByProductID](implementing-optimistic-concurrency-cs/_static/image32.png)](implementing-optimistic-concurrency-cs/_static/image31.png)

**Figura 11**: renomear os métodos a serem `FillByProductID` e `GetProductByProductID` ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image33.png))


Com este assistente for concluído, o TableAdapter agora contém dois métodos para recuperar dados: `GetProducts()`, que retorna *todas as* produtos; e `GetProductByProductID(productID)`, que retorna o produto especificado.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Etapa 3: Criando uma camada de lógica de negócios para DAL habilitados para simultaneidade otimista

Nosso existente `ProductsBLL` classe tem exemplos de como usar a atualização em lotes e os padrões diretos do banco de dados. O `AddProduct` método e `UpdateProduct` ambas as sobrecargas usam o padrão de atualização de lote, passando um `ProductRow` instância para o método Update do TableAdapter. O `DeleteProduct` método, por outro lado, usa o padrão de direto DB, chamando o TableAdapter `Delete(productID)` método.

Com o novo `ProductsOptimisticConcurrency` TableAdapter, o BD direcionar métodos agora exigem que os valores originais também ser transmitidos. Por exemplo, o `Delete` agora, o método espera dez parâmetros de entrada: original `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, e `Discontinued`. Ele usa valores desses parâmetros de entrada adicionais `WHERE` cláusula do `DELETE` instrução enviada ao banco de dados, somente o registro especificado a exclusão se os valores atuais do banco de dados mapeiam até os originais.

Enquanto a assinatura do método para o TableAdapter `Update` método usado no padrão de atualização de lote não foi alterado, tem o código necessário para registrar os valores novos e originais. Portanto, em vez de tentar usar o DAL habilitados para simultaneidade otimista com nosso existentes `ProductsBLL` de classe, vamos criar uma nova classe de camada de lógica comercial para trabalhar com nosso novo DAL.

Adicione uma classe chamada `ProductsOptimisticConcurrencyBLL` para o `BLL` pasta dentro de `App_Code` pasta.


![Adicione a classe ProductsOptimisticConcurrencyBLL à pasta BLL](implementing-optimistic-concurrency-cs/_static/image34.png)

**Figura 12**: adicionar o `ProductsOptimisticConcurrencyBLL` classe para a pasta BLL


Em seguida, adicione o seguinte código para o `ProductsOptimisticConcurrencyBLL` classe:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample5.cs)]

Observe o uso `NorthwindOptimisticConcurrencyTableAdapters` instrução acima do início da declaração de classe. O `NorthwindOptimisticConcurrencyTableAdapters` namespace contém a `ProductsOptimisticConcurrencyTableAdapter` classe, que fornece os métodos da DAL. Também antes da declaração de classe você encontrará o `System.ComponentModel.DataObject` atributo, que instrui o Visual Studio para incluir essa classe na lista suspensa do assistente ObjectDataSource.

O `ProductsOptimisticConcurrencyBLL`do `Adapter` propriedade fornece acesso rápido a uma instância das `ProductsOptimisticConcurrencyTableAdapter` de classe e segue o padrão usado em nossas classes BLL originais (`ProductsBLL`, `CategoriesBLL`e assim por diante). Por fim, o `GetProducts()` método simplesmente chama para baixo a DAL `GetProducts()` método e retorna um `ProductsOptimisticConcurrencyDataTable` objeto preenchido com um `ProductsOptimisticConcurrencyRow` instância para cada registro de produto no banco de dados.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Excluindo um produto usando o padrão de banco de dados direto com a simultaneidade otimista

Ao usar o padrão de banco de dados direto contra uma DAL que usa a simultaneidade otimista, os métodos devem ser passados para os valores novos e originais. Para excluir, não existem novos valores, portanto, apenas os valores originais precisam ser passados. Em nosso BLL, em seguida, podemos deve aceitar todos os parâmetros originais como parâmetros de entrada. Vamos ter o `DeleteProduct` método no `ProductsOptimisticConcurrencyBLL` classe usar o método direto de banco de dados. Isso significa que esse método precisa levar em todos os campos de dados de produto dez como parâmetros de entrada e passá-los para o DAL a conforme mostrado no código a seguir:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample6.cs)]

Se os valores originais - os valores que foram carregados pela última vez no GridView (ou DetailsView ou FormView) - diferem dos valores no banco de dados quando o usuário clica no botão excluir o `WHERE` cláusula não corresponde a qualquer registro de banco de dados e não há registros serão afetados. Portanto, o TableAdapter `Delete` método retornará `0` e a BLL `DeleteProduct` método retornará `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Atualizar um produto usando o padrão de atualização de lote com a simultaneidade otimista

Conforme observado anteriormente, o TableAdapter `Update` método para o padrão de atualização em lotes tem a mesma assinatura do método independentemente de estarem ou não a simultaneidade otimista é utilizada. Ou seja, o `Update` método espera um DataRow, uma matriz de DataRows, um DataTable ou um conjunto de dados tipado. Não há nenhum parâmetro de entrada adicional para especificar os valores originais. Isso é possível porque a DataTable mantém controle sobre os valores originais e modificados para seu DataRow(s). Quando a DAL emite sua `UPDATE` instrução, o `@original_ColumnName` parâmetros são preenchidos com valores originais de DataRow, enquanto o `@ColumnName` parâmetros são preenchidos com os valores modificados do DataRow.

No `ProductsBLL` classe (que usa nosso simultaneidade original, não otimista DAL), ao usar o padrão de atualização em lotes para atualizar as informações de produto nosso código executa a seguinte sequência de eventos:

1. Ler as informações de produto do banco de dados atual em um `ProductRow` instância usando o TableAdapter `GetProductByProductID(productID)` método
2. Atribuir os novos valores para o `ProductRow` instância da etapa 1
3. Chame o TableAdapter `Update` método, passando o `ProductRow` instância

Essa sequência de etapas, no entanto, não dar suporte corretamente à simultaneidade otimista porque o `ProductRow` preenchidos na etapa 1 é populado diretamente do banco de dados, que significa que os valores originais, usados pelo DataRow são aqueles que existem atualmente no banco de dados e não aquelas que estavam vinculados a GridView no início do processo de edição. Em vez disso, quando usar uma otimista habilitados para simultaneidade DAL, precisamos alterar o `UpdateProduct` sobrecargas de método para usar as seguintes etapas:

1. Ler as informações de produto do banco de dados atual em um `ProductsOptimisticConcurrencyRow` instância usando o TableAdapter `GetProductByProductID(productID)` método
2. Atribuir a *original* valores para o `ProductsOptimisticConcurrencyRow` instância da etapa 1
3. Chame o `ProductsOptimisticConcurrencyRow` da instância `AcceptChanges()` método, que instrui o DataRow que seus valores atuais são aqueles "originais"
4. Atribuir a *novos* valores para o `ProductsOptimisticConcurrencyRow` instância
5. Chame o TableAdapter `Update` método, passando o `ProductsOptimisticConcurrencyRow` instância

Leituras de etapa 1 em todos os valores atuais do banco de dados para o registro do produto especificado. Esta etapa é supérflua na `UpdateProduct` sobrecarga que atualiza *todos os* das colunas de produto (como esses valores são substituídos na etapa 2), mas é essencial para essas sobrecargas em que apenas um subconjunto dos valores da coluna são passados como parâmetros de entrada. Depois que os valores originais foram atribuídos à `ProductsOptimisticConcurrencyRow` instância, o `AcceptChanges()` método é chamado, que marca os valores atuais de DataRow como os valores originais para ser usado na `@original_ColumnName` parâmetros no `UPDATE` instrução. Em seguida, os novos valores de parâmetro são atribuídos para o `ProductsOptimisticConcurrencyRow` e, finalmente, o `Update` método é chamado, passando o DataRow.

O seguinte código mostra o `UpdateProduct` sobrecarga que aceita todos os dados de produto campos como parâmetros de entrada. Embora não mostrado aqui, o `ProductsOptimisticConcurrencyBLL` classe incluído no download para este tutorial também contém um `UpdateProduct` sobrecarga que aceita apenas o produto nome e preço como parâmetros de entrada.


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample7.cs)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Etapa 4: Passando os valores novos e originais da página ASP.NET aos métodos BLL

Com o DAL e BLL concluída, tudo o que resta é criar uma página ASP.NET que pode utilizar a lógica de simultaneidade otimista interna no sistema. Especificamente, os dados de controle da Web (o GridView, DetailsView ou FormView) devem lembrar de seus valores originais e o ObjectDataSource devem passar os dois conjuntos de valores para a camada de lógica de negócios. Além disso, a página ASP.NET deve ser configurada para manipular normalmente as violações de simultaneidade.

Comece abrindo o `OptimisticConcurrency.aspx` página o `EditInsertDelete` pasta e adicionando um GridView para o Designer, definindo seu `ID` propriedade para `ProductsGrid`. Na marca inteligente do GridView, optar por criar um novo ObjectDataSource chamado `ProductsOptimisticConcurrencyDataSource`. Como queremos que essa ObjectDataSource para usar a DAL que dá suporte à simultaneidade otimista, configurá-lo para usar o `ProductsOptimisticConcurrencyBLL` objeto.


[![Poderá usar o ObjectDataSource o objeto ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-cs/_static/image36.png)](implementing-optimistic-concurrency-cs/_static/image35.png)

**Figura 13**: pode usar o ObjectDataSource a `ProductsOptimisticConcurrencyBLL` objeto ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image37.png))


Escolha o `GetProducts`, `UpdateProduct`, e `DeleteProduct` métodos de listas suspensas no assistente. Para o método UpdateProduct, use a sobrecarga que aceita todos os campos de dados do produto.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Configurando propriedades do controle ObjectDataSource

Depois de concluir o assistente, a marcação declarativa do ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample8.aspx)]

Como você pode ver, o `DeleteParameters` coleção contém um `Parameter` instância para cada um dos parâmetros de entrada dez na `ProductsOptimisticConcurrencyBLL` da classe `DeleteProduct` método. Da mesma forma, o `UpdateParameters` coleção contém um `Parameter` instância para cada um dos parâmetros de entrada no `UpdateProduct`.

Para esses tutoriais anteriores que envolvia a modificação de dados, podemos removeria o ObjectDataSource `OldValuesParameterFormatString` propriedade neste ponto, já que essa propriedade indica que o método BLL espera que os valores antigos (ou originais) a ser passado em, bem como os novos valores. Além disso, o valor dessa propriedade indica os nomes de parâmetro de entrada para os valores originais. Uma vez que estamos passando os valores originais para a BLL, faça *não* remover esta propriedade.

> [!NOTE]
> O valor da `OldValuesParameterFormatString` propriedade deve mapear para os nomes de parâmetro de entrada na BLL que espera que os valores originais. Uma vez que estamos esses parâmetros nomeados `original_productName`, `original_supplierID`e assim por diante, você pode deixar o `OldValuesParameterFormatString` valor da propriedade como `original_{0}`. Se, no entanto, os parâmetros de entrada de métodos a BLL tinham nomes como `old_productName`, `old_supplierID`e assim por diante, você precisa para atualizar o `OldValuesParameterFormatString` propriedade `old_{0}`.


Há uma configuração de propriedade final que precisa ser feita para que o ObjectDataSource passar corretamente os valores originais para os métodos BLL. O ObjectDataSource tem um [propriedade ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) que pode ser atribuído a [um dos dois valores](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -o valor padrão; não envia os valores originais para parâmetros de entrada original dos métodos BLL
- `CompareAllValues` -enviar os valores originais para os métodos BLL; Escolha esta opção ao usar a simultaneidade otimista

Dedique uns momentos para definir a `ConflictDetection` propriedade para `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Configurando o GridView propriedades e campos

Com as propriedades do ObjectDataSource corretamente configuradas, vamos voltar a atenção para configurar o GridView. Em primeiro lugar, como queremos que o GridView para dar suporte à edição e exclusão, clique nas caixas de seleção Permitir edição e habilitar a exclusão de marca inteligente do GridView. Isso adicionará um CommandField cujos `ShowEditButton` e `ShowDeleteButton` são definidos como `true`.

Quando ligado ao `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView contém um campo para cada um dos campos de dados do produto. Embora tal um GridView pode ser editado, a experiência do usuário é nada, mas é aceitável. O `CategoryID` e `SupplierID` BoundFields serão renderizados como caixas de texto, exigir que o usuário insira a categoria apropriada e fornecedor como números de ID. Não haverá nenhuma formatação para os campos numéricos e não há controles de validação para garantir que o nome do produto foi fornecido e que o preço unitário, unidades em estoque, unidades na ordem e os valores de nível de reordenação são os dois valores numéricos adequados e são maiores que ou igual a como zero.

Como discutimos na *adicionando controles de validação para a edição e inserção de Interfaces* e *Personalizando a Interface de modificação de dados* tutoriais, a interface do usuário pode ser personalizada pelo substituindo o BoundFields TemplateFields. Eu modifiquei esse GridView e sua interface de edição das seguintes maneiras:

- Removida a `ProductID`, `SupplierName`, e `CategoryName` BoundFields
- Convertido o `ProductName` BoundField em um TemplateField e adicionou um controle RequiredFieldValidation.
- Convertido a `CategoryID` e `SupplierID` BoundFields para TemplateFields e ajustá-lo o interface de edição para usar DropDownLists em vez de caixas de texto. Em 'esses TemplateFields `ItemTemplates`, o `CategoryName` e `SupplierName` campos de dados são exibidos.
- Convertido a `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` BoundFields TemplateFields e controles de CompareValidator adicionados.

Uma vez que já examinamos como realizar essas tarefas nos tutoriais anteriores, vou apenas lista a sintaxe final declarativa aqui e deixar a implementação prática.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample9.aspx)]

Estamos muito perto tendo um exemplo totalmente funcional. No entanto, existem algumas sutilezas que surgir durante backup e causar nos problemas. Além disso, ainda precisamos alguma interface que alerta o usuário quando ocorreu uma violação de simultaneidade.

> [!NOTE]
> Para que um controle da Web de dados para passar corretamente os valores originais para o ObjectDataSource (que são então passados para a BLL), é vital que o GridView `EnableViewState` estiver definida como `true` (o padrão). Se você desabilitar o estado de exibição, os valores originais são perdidos no postback.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Passando os valores originais corretos para o ObjectDataSource

Há dois problemas com a maneira que o GridView foi configurado. Se o ObjectDataSource `ConflictDetection` estiver definida como `CompareAllValues` (como é nosso), quando o ObjectDataSource `Update()` ou `Delete()` métodos são invocados pelo GridView (ou DetailsView ou FormView), o ObjectDataSource tenta copiar o Valores originais do GridView em seu apropriado `Parameter` instâncias. Consulte novamente a Figura 2 para uma representação gráfica deste processo.

Especificamente, os valores originais do GridView recebem os valores nas instruções de associação de dados bidirecional cada vez que os dados serão associados ao GridView. Portanto, é essencial que os valores originais necessários todas as são capturados por meio de associação de dados bidirecional e que eles são fornecidos em um formato que pode ser convertido.

Para ver por que isso é importante, reserve um tempo para visite nossa página em um navegador. Conforme o esperado, GridView lista cada produto com um botão Editar e excluir na coluna mais à esquerda.


[![Os produtos são listados em um GridView](implementing-optimistic-concurrency-cs/_static/image39.png)](implementing-optimistic-concurrency-cs/_static/image38.png)

**Figura 14**: os produtos são listados em um GridView ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image40.png))


Se você clicar no botão Excluir para qualquer produto, um `FormatException` é gerada.


[![Tentativa de excluir qualquer resultado de produto em uma FormatException](implementing-optimistic-concurrency-cs/_static/image42.png)](implementing-optimistic-concurrency-cs/_static/image41.png)

**Figura 15**: a tentativa de excluir qualquer produto os resultados em um `FormatException` ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image43.png))


O `FormatException` é gerado quando o ObjectDataSource tenta ler no original `UnitPrice` valor. Uma vez que o `ItemTemplate` tem o `UnitPrice` formatada como uma moeda (`<%# Bind("UnitPrice", "{0:C}") %>`), ele inclui um símbolo de moeda como US $ 19,95 para venda. O `FormatException` ocorre conforme o ObjectDataSource tenta converter essa cadeia de caracteres em um `decimal`. Para contornar esse problema, temos várias opções:

- Remover formatação de moeda a `ItemTemplate`. Ou seja, em vez de usar `<%# Bind("UnitPrice", "{0:C}") %>`, basta usar `<%# Bind("UnitPrice") %>`. A desvantagem disso é que o preço não está formatado.
- Exibição de `UnitPrice` formatado como uma moeda no `ItemTemplate`, mas usar o `Eval` palavra-chave para fazer isso. Lembre-se de que `Eval` executa a associação de dados unidirecional. Precisamos fornecer o `UnitPrice` valor para os valores originais, portanto, ainda precisaremos de uma instrução de vinculação de dados bidirecional na `ItemTemplate`, mas isso pode ser colocado em um controle de rótulo Web cuja `Visible` estiver definida como `false`. Poderíamos usar a seguinte marcação do ItemTemplate:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample10.aspx)]

- Remover formatação de moeda a `ItemTemplate`, usando `<%# Bind("UnitPrice") %>`. No GridView `RowDataBound` manipulador de eventos, por meio de programação de acesso Web rótulo de controle dentro do qual o `UnitPrice` valor é exibido e defina seu `Text` propriedade para a versão formatada.
- Deixe o `UnitPrice` formatado como uma moeda. No GridView `RowDeleting` manipulador de eventos, substituir o original existente `UnitPrice` valor (US $19,95) com um valor decimal real usando `Decimal.Parse`. Vimos como fazer algo semelhante na `RowUpdating` manipulador de eventos na [ *BLL tratando - e exceções de nível DAL em uma página ASP.NET* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) tutorial.

Para o meu exemplo, escolhi a segunda abordagem, adicionando uma Web oculto do rótulo de controle cuja `Text` propriedade é bidirecional associado a dados não formatada `UnitPrice` valor.

Depois de resolver esse problema, tente clicar no botão Excluir para qualquer produto novamente. Neste momento, você obterá um `InvalidOperationException` quando o ObjectDataSource tenta invocar a BLL `UpdateProduct` método.


[![O ObjectDataSource não é possível encontrar um método com os parâmetros de entrada, ele quer enviar](implementing-optimistic-concurrency-cs/_static/image45.png)](implementing-optimistic-concurrency-cs/_static/image44.png)

**Figura 16**: O ObjectDataSource não é possível encontrar um método com os parâmetros de entrada que deseja enviar ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image46.png))


Examinando a mensagem de exceção, é claro que o ObjectDataSource quer invocar um BLL `DeleteProduct` método inclui `original_CategoryName` e `original_SupplierName` parâmetros de entrada. Isso ocorre porque o `ItemTemplate` s para o `CategoryID` e `SupplierID` TemplateFields atualmente contêm instruções de ligação bidirecionais com o `CategoryName` e `SupplierName` campos de dados. Em vez disso, precisamos incluir `Bind` instruções com o `CategoryID` e `SupplierID` campos de dados. Para fazer isso, substitua as instruções existentes de associação com `Eval` instruções e, em seguida, adicionar controles de rótulo oculto cujo `Text` propriedades são vinculadas aos `CategoryID` e `SupplierID` campos de dados usando a associação de dados bidirecional, conforme mostrado abaixo:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample11.aspx)]

Com essas alterações, agora estamos prontos excluir e editar as informações de produto com êxito! Etapa 5, examinaremos como verificar que as violações de simultaneidade estão sendo detectadas. Mas por enquanto, levar alguns minutos para tentar a atualização e exclusão de alguns registros para garantir que a atualização e exclusão para um único usuário funciona conforme o esperado.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Etapa 5: Testando o suporte a simultaneidade otimista

Para verificar se as violações de simultaneidade estão sendo detectadas (em vez de resultando em dados que estão sendo substituídos cegamente), precisamos abrir duas janelas de navegador para essa página. Em ambas as instâncias do navegador, clique no botão Editar para Chai. Em seguida, em apenas um dos navegadores, altere o nome para "Chai chá" e clique em Atualizar. A atualização deve bem-sucedida e retorne o GridView para seu estado de edição previamente, com "Chai chá" como o novo nome do produto.

A outra instância de janela do navegador, no entanto, o caixa de texto Nome do produto ainda mostra "Chai". Na janela de navegador segundo, atualize o `UnitPrice` para `25.00`. Sem suporte a simultaneidade otimista, clicando em Atualizar na segunda instância de navegador alteraria o nome do produto para "Chai", substituindo as alterações feitas pela primeira instância do navegador. Com a simultaneidade otimista empregada, no entanto, clicando no botão de atualização na segunda instância de navegador resulta em uma [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Quando uma violação de simultaneidade é detectada, um DBConcurrencyException é lançada](implementing-optimistic-concurrency-cs/_static/image48.png)](implementing-optimistic-concurrency-cs/_static/image47.png)

**Figura 17**: quando uma violação de simultaneidade é detectada, um `DBConcurrencyException` é gerada ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image49.png))


O `DBConcurrencyException` é acionada somente quando padrão de atualização em lotes da DAL é utilizada. O padrão de banco de dados direto não gera uma exceção, ela simplesmente indica que nenhuma linha foi afetada. Para ilustrar isso, retorne GridView de ambas as instâncias do navegador para seu estado de edição previamente. Em seguida, na primeira instância do navegador, clique no botão Editar e alterar o nome do produto de "Chai chá" para "Chai" e clique em Atualizar. Na segunda janela do navegador, clique no botão Excluir para Chai.

Ao clicar em Excluir, a página é postada, o GridView invoca o ObjectDataSource `Delete()` método e o ObjectDataSource chama para baixo a `ProductsOptimisticConcurrencyBLL` da classe `DeleteProduct` método, passando os valores originais. O original `ProductName` de valor para a segunda instância do navegador é "Chai chá", que não corresponde à atual `ProductName` valor no banco de dados. Portanto, o `DELETE` instrução emitida para o banco de dados afeta zero linhas, pois não há nenhum registro no banco de dados que o `WHERE` atende a cláusula. O `DeleteProduct` método retorna `false` e dados do ObjectDataSource são reassociados ao GridView.

Da perspectiva do usuário final, clicando no botão Excluir para Chá Chai na segunda janela do navegador causou a tela para flash e, após a voltar, o produto ainda está lá, embora agora ele está listado como "Chai" (a produto alteração de nome feita pelo navegador primeiro instância). Se o usuário clica no botão Excluir novamente, a exclusão será bem-sucedida, como GridView original `ProductName` valor ("Chai") agora correspondências com o valor no banco de dados.

Em ambos os casos, a experiência do usuário está longe de ser ideal. Claramente não queremos Mostrar ao usuário os detalhes sobre o `DBConcurrencyException` exceção ao usar o padrão de atualização em lotes. E o comportamento ao usar o padrão de banco de dados direto é um pouco confuso, como o comando usuários falhou, mas havia uma indicação precisa do porquê.

Para corrigir esses dois problemas, podemos criar controles da Web do rótulo da página que fornecem uma explicação de por que uma atualização ou Falha ao excluir. Para o padrão de atualização em lotes, podemos determinar se ou não um `DBConcurrencyException` ocorreu uma exceção no manipulador de evento de pós-nível do GridView, exibindo o rótulo de aviso, conforme necessário. O método direto de banco de dados, podemos examinar o valor de retorno do método BLL (que é `true` se uma linha foi afetada, `false` caso contrário) e exibir uma mensagem informativa, conforme necessário.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Etapa 6: Adicionando mensagens informativas e exibi-los em caso de uma violação de simultaneidade

Quando ocorre uma violação de simultaneidade, o comportamento exibido depende se foi utilizado o DAL atualização em lotes ou padrão de banco de dados direta. Nosso tutorial usa ambos os padrões, com o padrão de atualização de lote que está sendo usado para atualizar e o padrão de direta de banco de dados usada para excluir. Para começar, vamos adicionar dois controles de Web de rótulo à nossa página que explicam o que ocorreu uma violação de simultaneidade durante a tentativa de excluir ou atualizar dados. Defina o controle de rótulo `Visible` e `EnableViewState` propriedades a serem `false`; isso fará com que-los para serem ocultadas em cada visita de página, exceto para aqueles que determinada página visita onde seus `Visible` propriedade é definida por meio de programação como `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample12.aspx)]

Além de configuração de suas `Visible`, `EnabledViewState`, e `Text` propriedades, defini também o `CssClass` propriedade `Warning`, que faz com que o rótulo da ser exibido em uma fonte grande, vermelha, itálico, negrito. Esse CSS `Warning` classe foi definida e adicionado ao Styles volta a *examinando os eventos associados inserindo, atualizando e excluindo* tutorial.

Depois de adicionar esses rótulos, o Designer no Visual Studio deve ser semelhante a Figura 18.


[![Foram adicionados dois controles de rótulo para a página](implementing-optimistic-concurrency-cs/_static/image51.png)](implementing-optimistic-concurrency-cs/_static/image50.png)

**Figura 18**: dois rótulo controles foram adicionados à página ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image52.png))


Com esses controles de Web Label em vigor, estamos prontos para examinar como determinar quando uma violação de simultaneidade tiver ocorrido, em que ponto o rótulo apropriado `Visible` propriedade pode ser definida como `true`, exibindo a mensagem informativa.

## <a name="handling-concurrency-violations-when-updating"></a>Tratar de violações de simultaneidade ao atualizar

Vamos primeiro examinar como lidar com violações de simultaneidade ao usar o padrão de atualização em lotes. Desde que tais violações com o lote atualização causa padrão uma `DBConcurrencyException` exceção seja lançada, precisamos adicionar código à nossa página do ASP.NET para determinar se um `DBConcurrencyException` exceção ocorreu durante o processo de atualização. Se assim, podemos deve exibir uma mensagem para o usuário explicando que suas alterações não foram salvas porque outro usuário tive modificado os mesmos dados entre quando começaram a edição do registro, e quando eles clicou no botão de atualização.

Como vimos na *tratamento BLL - e exceções de nível DAL em uma página ASP.NET* tutorial, essas exceções podem ser detectadas e suprimidas em manipuladores de evento de pós-nível do controle de Web de dados. Portanto, precisamos criar um manipulador de eventos para o GridView `RowUpdated` evento verifica se um `DBConcurrencyException` exceção foi lançada. Esse manipulador de eventos é passado uma referência a qualquer exceção gerada durante o processo de atualização, conforme mostrado no caso de manipulador de código abaixo:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample13.cs)]

A enfrentar de um `DBConcurrencyException` esse manipulador de eventos de exceção, exibe o `UpdateConflictMessage` controle de rótulo e indica que a exceção foi tratada. Com esse código, quando uma violação de simultaneidade ocorre ao atualizar um registro, as alterações do usuário são perdidas, já que eles talvez tenham sido substituídos modificações de outro usuário ao mesmo tempo. Em particular, o GridView é retornado para seu estado de edição previamente e associado aos dados do banco de dados atual. Isso atualizará a linha de GridView com as outras alterações do usuário, que eram anteriormente não é visíveis. Além disso, o `UpdateConflictMessage` controle de rótulo será explicar ao usuário que acabou de ocorrer. Essa sequência de eventos é detalhada na Figura 19.


[![Um usuário s atualizações são perdidas na Face de uma violação de simultaneidade](implementing-optimistic-concurrency-cs/_static/image54.png)](implementing-optimistic-concurrency-cs/_static/image53.png)

**Figura 19**: um usuário s atualizações são perdidas na Face de uma violação de simultaneidade ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image55.png))


> [!NOTE]
> Como alternativa, em vez de retornar o GridView para o estado de edição previamente, podemos pode deixar o GridView em seu estado de edição, definindo o `KeepInEditMode` propriedade do passado `GridViewUpdatedEventArgs` objeto como true. Se você usar essa abordagem, no entanto, certifique-se associar novamente os dados para o GridView (invocando seus `DataBind()` método) para que os outros valores do usuário são carregados na interface de edição. O código disponível para download com este tutorial tem estas duas linhas de código no `RowUpdated` comentado do manipulador de eventos; simplesmente remova essas linhas de código para que o GridView permanecem no modo de edição após uma violação de simultaneidade.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Responder a violações de simultaneidade ao excluir

Com o padrão de banco de dados direto, não há nenhuma exceção gerada em caso de uma violação de simultaneidade. Em vez disso, a instrução de banco de dados simplesmente não afeta nenhum registro, como a cláusula WHERE não coincide com nenhum registro. Todos os métodos de modificação de dados criados na BLL foram criados, de modo que elas retornam um valor booliano que indica se eles afetados com precisão de um registro. Portanto, para determinar se ocorreu uma violação de simultaneidade quando a exclusão de um registro, podemos examinar o valor de retorno a BLL `DeleteProduct` método.

O valor de retorno para um método BLL pode ser examinado em manipuladores de evento de pós-nível do ObjectDataSource por meio de `ReturnValue` propriedade do `ObjectDataSourceStatusEventArgs` objeto passado para o manipulador de eventos. Como estamos interessados na determinação do valor de retorno de `DeleteProduct` método, precisamos criar um manipulador de eventos para o ObjectDataSource `Deleted` eventos. O `ReturnValue` propriedade é do tipo `object` e pode ser `null` se uma exceção foi gerada e o método foi interrompido antes que ele poderia retornar um valor. Portanto, podemos deve certificar-se que o `ReturnValue` propriedade não é `null` e é um valor booliano. Supondo que essa verificação passa, vamos mostrar o `DeleteConflictMessage` controle de rótulo, se o `ReturnValue` é `false`. Isso pode ser feito usando o código a seguir:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample14.cs)]

Diante de uma violação de simultaneidade, a solicitação de exclusão do usuário será cancelada. O GridView é atualizado, mostrando as alterações que ocorreram para esse registro entre a hora em que o usuário carregar a página e quando ele clica no botão Excluir. Quando ocorre uma violação desse tipo, o `DeleteConflictMessage` rótulo é exibido, explicando o que aconteceu (consulte a Figura 20).


[![Um usuário s Delete é cancelado diante de uma violação de simultaneidade](implementing-optimistic-concurrency-cs/_static/image57.png)](implementing-optimistic-concurrency-cs/_static/image56.png)

**Figura 20**: s de um usuário excluir é cancelada diante de uma violação de simultaneidade ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image58.png))


## <a name="summary"></a>Resumo

Existem oportunidades para violações de simultaneidade em todos os aplicativos que permite que vários usuários simultâneos para atualizar ou excluir dados. Se tais violações não são levados em consideração, quando dois usuários atualiza simultaneamente os mesmos dados quem obtém a última gravação "vence", substituir o outro usuário altera as alterações. Como alternativa, os desenvolvedores podem implementar um controle de simultaneidade otimista ou pessimista. Controle de simultaneidade otimista pressupõe que as violações de simultaneidade são frequentes e simplesmente não permite uma atualização ou excluir um comando que constituiria uma violação de simultaneidade. Controle de simultaneidade pessimista pressupõe que a simultaneidade violações são frequentes e rejeitando a um usuário simplesmente atualizar ou excluir o comando não é aceitável. Com o controle de simultaneidade pessimista, atualizando um registro envolve bloqueá-lo, impedindo assim a quaisquer outros usuários modifiquem ou excluam o registro, enquanto ele estiver bloqueado.

O conjunto de dados tipado no .NET fornece a funcionalidade para dar suporte a controle de simultaneidade otimista. Em particular, o `UPDATE` e `DELETE` instruções emitidas para o banco de dados incluem todas as colunas da tabela, garantindo assim que a atualização ou exclusão só ocorrerá se os dados do registro atual correspondem aos dados originais, o usuário tinha quando realizando sua atualização ou exclusão. Depois que a DAL tiver sido configurada para dar suporte à simultaneidade otimista, os métodos BLL precisam ser atualizados. Além disso, a página ASP.NET que chama a BLL deve ser configurada, de modo que o ObjectDataSource recupera os valores originais de seu controle de Web de dados e passa-os para baixo para a BLL.

Como vimos neste tutorial, a implementação de controle de simultaneidade otimista em um aplicativo web ASP.NET envolve atualizando a BLL e DAL e adição de suporte na página ASP.NET. Se esse trabalho adicional é um bom investimento de tempo e esforço depende de seu aplicativo. Se você tiver com pouca frequência a atualização de dados de usuários simultâneos, ou os dados que eles estão atualizando são diferentes uns dos outros, controle de simultaneidade não é uma questão importante. Se, no entanto, você normalmente tem vários usuários em seu site funcionando com os mesmos dados, controle de simultaneidade pode ajudar a impedir que as atualizações de um usuário ou exclusões substitua inadvertidamente da outra.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](customizing-the-data-modification-interface-cs.md)
> [Próximo](adding-client-side-confirmation-when-deleting-cs.md)
