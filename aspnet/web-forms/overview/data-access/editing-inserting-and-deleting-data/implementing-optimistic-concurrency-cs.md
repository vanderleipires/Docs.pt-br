---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
title: Implementando a concorrência otimista (c#) | Microsoft Docs
author: rick-anderson
description: Para um aplicativo web que permite que vários usuários editar dados, há o risco de que dois usuários podem editar os mesmos dados ao mesmo tempo. Neste tutori...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/17/2006
ms.topic: article
ms.assetid: 56e15b33-93b8-43ad-8e19-44c6647ea05c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs
msc.type: authoredcontent
ms.openlocfilehash: 27441ea9343055b3139468036fc6f201c77667e5
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="implementing-optimistic-concurrency-c"></a>Implementando a concorrência otimista (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_CS.exe) ou [baixar PDF](implementing-optimistic-concurrency-cs/_static/datatutorial21cs1.pdf)

> Para um aplicativo web que permite que vários usuários editar dados, há o risco de que dois usuários podem editar os mesmos dados ao mesmo tempo. Neste tutorial, implementaremos o controle de simultaneidade otimista para tratar esse risco.


## <a name="introduction"></a>Introdução

Para aplicativos web que só permitem que os usuários exibam dados ou para aqueles que incluem apenas um único usuário que pode modificar dados, não há nenhuma ameaça de dois usuários simultâneos substituição acidental de alterações do outro. Para aplicativos web que permitem que vários usuários atualizar ou excluir dados, no entanto, há o potencial para que as modificações de um usuário podem conflitar com outro usuário simultâneo. Sem qualquer política de simultaneidade em vigor, quando dois usuários simultaneamente estiver editando um único registro, o usuário que confirme suas alterações última substituirá as alterações feitas pelo primeiro.

Por exemplo, imagine que dois usuários, Jisun e Sam, foram ambas de visita uma página em nosso aplicativo permitido visitantes atualizar e excluir os produtos por meio de um controle GridView. Ambos clique no botão Editar em GridView ao mesmo tempo. Jisun altera o nome do produto para "Chai chá" e clica no botão de atualização. O resultado é um `UPDATE` instrução é enviada para o banco de dados, que define *todos os* de campos atualizável do produto (embora Jisun atualizado apenas um campo, `ProductName`). Neste momento, o banco de dados tem os valores "Chai chá," a categoria de bebidas, o fornecedor exóticas líquidos, e assim por diante para este produto específico. No entanto, o GridView na tela do Sam ainda mostrará o nome do produto na linha GridView editável como "Chai". Alguns segundos depois de alterações do Jisun foram confirmadas, Sam atualiza a categoria para Condimentos e clica em Atualizar. Isso resulta em uma `UPDATE` instrução enviada para o banco de dados que define o nome do produto para "Chai", o `CategoryID` para a ID da categoria de bebidas correspondente e assim por diante. Alterações do Jisun para o nome do produto foram substituídas. A Figura 1 mostra graficamente esta série de eventos.


[![Quando dois usuários simultaneamente atualizar um registro existe potencial s para um usuário s muda para substituir os outros s](implementing-optimistic-concurrency-cs/_static/image2.png)](implementing-optimistic-concurrency-cs/_static/image1.png)

**Figura 1**: alterações de um registro existe s potencial para um usuário s quando dois usuários simultaneamente a atualização para substituir os outros s ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image3.png))


Da mesma forma, quando dois usuários são visita uma página, um usuário pode ser no meio da atualização de um registro excluído por outro usuário. Ou então, entre quando um usuário carrega uma página e quando clicar no botão Excluir, outro usuário pode ter modificado o conteúdo desse registro.

Há três [controle de simultaneidade](http://en.wikipedia.org/wiki/Concurrency_control) estratégias disponíveis:

- **Não fazer nada** -se a usuários simultâneos estão modificando o mesmo registro, permitem que a última confirmação win (o comportamento padrão)
- [**Simultaneidade otimista** ](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) -supõem que embora possa haver conflitos de simultaneidade vez em seguida, a maioria do tempo tais conflitos não ocorrer; portanto, se ocorrer um conflito, simplesmente informar ao usuário que suas alterações não pode ser salvo porque outro usuário modificou os mesmos dados
- **Simultaneidade pessimista** -supor que os conflitos de simultaneidade são comuns e que os usuários não toleram sendo informado de suas alterações não foram salvas devido à atividade simultânea de outro usuário; portanto, quando um usuário inicia a atualização de um registro, bloqueá-la , impedindo assim a quaisquer outros usuários de editar ou excluir este registro até que o usuário confirme suas modificações

Todos os nossos tutoriais até o momento têm usada a estratégia de resolução padrão simultaneidade - isto é, deixamos já que a última gravação win. Neste tutorial, examinaremos como implementar o controle de simultaneidade otimista.

> [!NOTE]
> Não examinaremos exemplos de simultaneidade pessimista nesta série tutorial. Simultaneidade pessimista raramente é usada, pois tais bloqueios, se não for liberada à medida corretamente, pode impedir que outros usuários atualizem dados. Por exemplo, se um usuário bloqueia um registro para edição e, em seguida, sai para o dia anterior desbloqueá-la, nenhum outro usuário será capaz de atualizar o registro até que o usuário original retorna e conclui sua atualização. Portanto, em situações onde a simultaneidade pessimista é usada, normalmente há um tempo limite que, se atingido, cancela o bloqueio. Sites vendas tíquete, que bloqueia um local específico de assentos por curto período enquanto o usuário conclui o processo de ordem, é um exemplo de controle de simultaneidade pessimista.


## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Etapa 1: Examinar como a simultaneidade otimista é implementado.

Controle de simultaneidade otimista funciona, garantindo que o registro que está sendo atualizada ou excluída tem os mesmos valores de quando a atualização ou exclusão de processo iniciado. Por exemplo, ao clicar no botão de edição em um GridView editável, valores do registro são de leitura do banco de dados e exibidos em caixas de texto e outros controles da Web. Esses valores originais são salvos por GridView. Posteriormente, depois que o usuário faz alterações e clica no botão de atualização, os valores originais e os novos valores são enviados para a camada de lógica de negócios e, em seguida, para a camada de acesso de dados. Camada de acesso a dados deve emitir uma instrução SQL que atualizará somente o registro se os valores originais que o usuário iniciou a edição são idênticos aos valores ainda no banco de dados. Figura 2 mostra esta sequência de eventos.


[![Para a atualização ou exclusão seja bem-sucedida, os valores originais devem ser iguais para os valores atuais do banco de dados](implementing-optimistic-concurrency-cs/_static/image5.png)](implementing-optimistic-concurrency-cs/_static/image4.png)

**Figura 2**: For Update ou Delete para sucesso, o Original valores deve ser igual para os valores atuais do banco de dados ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image6.png))


Há várias abordagens para implementar a simultaneidade otimista (consulte [Peter A. Bromberg](http://peterbromberg.net/)do [lógica de atualização de simultaneidade de Optmistic](http://www.eggheadcafe.com/articles/20050719.asp) para examinar uma série de opções). O conjunto de dados tipado do ADO.NET fornece uma implementação que pode ser configurada com a escala de uma caixa de seleção. Habilitar a simultaneidade otimista um TableAdapter no conjunto de dados tipado aumenta o TableAdapter `UPDATE` e `DELETE` instruções para incluir uma comparação de todos os valores originais no `WHERE` cláusula. O seguinte `UPDATE` instrução, por exemplo, atualiza o nome e o preço de um produto somente se os valores atuais do banco de dados são iguais aos valores originalmente recuperados ao atualizar o registro em GridView. O `@ProductName` e `@UnitPrice` parâmetros contêm os novos valores inseridos pelo usuário, enquanto `@original_ProductName` e `@original_UnitPrice` contêm os valores que foram carregados originalmente em GridView, quando o botão de edição foi clicado:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample1.sql)]

> [!NOTE]
> Isso `UPDATE` instrução foi simplificada para facilitar a leitura. Na prática, o `UnitPrice` check-in a `WHERE` cláusula seria mais envolvida desde `UnitPrice` pode conter `NULL` s e verificando se `NULL = NULL` sempre retorna False (em vez disso, você deve usar `IS NULL`).


Além de usar uma base diferentes `UPDATE` métodos de direcionar instrução, configurando um TableAdapter usar otimista de simultaneidade também modifica a assinatura do seu banco de dados. Lembre-se do nosso primeiro tutorial, [ *criando uma camada de acesso a dados*](../introduction/creating-a-data-access-layer-cs.md), que métodos diretos do banco de dados são aquelas que aceita uma lista de escalar valores como parâmetros de entrada (em vez de um DataRow fortemente tipado ou Instância de DataTable). Ao usar a simultaneidade otimista, o banco de dados direta `Update()` e `Delete()` métodos incluem parâmetros de entrada para os valores originais. Além disso, o código na BLL para usar o lote atualizar padrão (o `Update()` sobrecargas do método que aceitam DataRows e tabelas de dados em vez de valores escalares) deve ser alterada também.

Em vez de estender o nosso existente TableAdapters da DAL usar simultaneidade otimista (que necessitaria BLL para acomodar a alteração), vamos criar um novo conjunto de dados tipado chamado `NorthwindOptimisticConcurrency`, para que, adicionaremos um `Products` TableAdapter que usa a simultaneidade otimista. Depois disso, vamos criar um `ProductsOptimisticConcurrencyBLL` classe da camada de lógica de negócios com as modificações apropriadas para dar suporte a simultaneidade otimista DAL. Depois que essa base layout foi definido, podemos estará prontos para criar a página ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Etapa 2: Criar uma camada de acesso a dados que oferece suporte a simultaneidade otimista

Para criar um novo DataSet digitado, clique duas vezes no `DAL` pasta dentro de `App_Code` pasta e adicionar um novo conjunto de dados chamado `NorthwindOptimisticConcurrency`. Como vimos no primeiro tutorial, fazer irá adicionar um novo TableAdapter para o conjunto de dados tipado, iniciar automaticamente o Assistente de configuração do TableAdapter. Na primeira tela, serão solicitados a especificar o banco de dados para conectar ao - conectar-se ao mesmo banco de dados Northwind usando o `NORTHWNDConnectionString` configuração de `Web.config`.


[![Conecte-se ao mesmo banco de dados Northwind](implementing-optimistic-concurrency-cs/_static/image8.png)](implementing-optimistic-concurrency-cs/_static/image7.png)

**Figura 3**: conectar-se ao mesmo banco de dados Northwind ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image9.png))


Em seguida, são solicitados a confirmar como consultar os dados: por meio de uma instrução SQL ad hoc, um novo procedimento armazenado ou uma existente de procedimento armazenado. Como usamos consultas SQL ad hoc em nosso DAL original, use essa opção aqui também.


[![Especifique os dados serem recuperados usando uma instrução de SQL Ad Hoc](implementing-optimistic-concurrency-cs/_static/image11.png)](implementing-optimistic-concurrency-cs/_static/image10.png)

**Figura 4**: especificar os dados para recuperar usando uma instrução de SQL Ad Hoc ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image12.png))


Na tela seguinte, digite a consulta SQL a ser usado para recuperar as informações de produto. Vamos usar a mesma exata consulta SQL usada para o `Products` TableAdapter de nosso DAL original, que retorna todos os a `Product` colunas junto com os nomes de fornecedor e a categoria do produto:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample2.sql)]


[![Use a mesma consulta SQL de TableAdapter produtos no DAL Original](implementing-optimistic-concurrency-cs/_static/image14.png)](implementing-optimistic-concurrency-cs/_static/image13.png)

**Figura 5**: usar a mesma consulta SQL do `Products` TableAdapter no DAL Original ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image15.png))


Antes de passar para a próxima tela, clique no botão Opções avançadas. Para que este controle de simultaneidade otimista de empregar TableAdapter, basta marcar a caixa de seleção "Usar simultaneidade otimista".


[![Habilitar o controle de simultaneidade otimista, verificando o &quot;usar simultaneidade otimista&quot; caixa de seleção](implementing-optimistic-concurrency-cs/_static/image17.png)](implementing-optimistic-concurrency-cs/_static/image16.png)

**Figura 6**: habilitar o controle de simultaneidade otimista, marcando a caixa de seleção "Usar simultaneidade otimista" ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image18.png))


Por fim, indicam que o TableAdapter deve usar os padrões de acesso a dados que preencher uma DataTable e retornam uma DataTable; também indicam que os métodos diretos do banco de dados devem ser criados. Altere o nome do método para retornar um padrão de DataTable de GetData GetProducts para espelhar as convenções de nomenclatura usadas na nossa DAL original.


[![Ter o TableAdapter utilizar todos os padrões de acesso a dados](implementing-optimistic-concurrency-cs/_static/image20.png)](implementing-optimistic-concurrency-cs/_static/image19.png)

**Figura 7**: tem o TableAdapter utilizar todos os dados de padrões de acesso ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image21.png))


Depois de concluir o assistente, o Designer de conjunto de dados incluirá fortemente tipado `Products` DataTable e TableAdapter. Reserve um tempo para renomear DataTable de `Products` para `ProductsOptimisticConcurrency`, que pode ser feito clicando duas vezes na barra de título da DataTable e escolha Renomear no menu de contexto.


[![A DataTable e TableAdapter foram adicionados ao conjunto de dados tipado](implementing-optimistic-concurrency-cs/_static/image23.png)](implementing-optimistic-concurrency-cs/_static/image22.png)

**Figura 8**: uma DataTable e TableAdapter foram adicionados ao conjunto de dados tipado ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image24.png))


Para ver as diferenças entre o `UPDATE` e `DELETE` consultas entre o `ProductsOptimisticConcurrency` TableAdapter (que usa a simultaneidade otimista) e o TableAdapter de produtos (que não), clique no TableAdapter e vá para a janela de propriedades. No `DeleteCommand` e `UpdateCommand` propriedades `CommandText` subpropriedades, você pode ver a sintaxe SQL real que é enviada para o banco de dados quando a DAL update ou métodos relacionados a exclusão são invocados. Para o `ProductsOptimisticConcurrency` TableAdapter o `DELETE` instrução usada é:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample3.sql)]

Enquanto o `DELETE` instrução para o TableAdapter de produto na nossa DAL original é muito mais simples:


[!code-sql[Main](implementing-optimistic-concurrency-cs/samples/sample4.sql)]

Como você pode ver, o `WHERE` cláusula no `DELETE` instrução para o TableAdapter que usa a simultaneidade otimista inclui uma comparação entre cada o `Product` tabela existente de valores de coluna e os valores originais no momento em que o GridView (ou DetailsView ou FormView) foi populado pela última. Desde que todos os campos de `ProductID`, `ProductName`, e `Discontinued` pode ter `NULL` valores, parâmetros adicionais e verificações estão incluídas para comparar corretamente `NULL` valores no `WHERE` cláusula.

Nós não adicionar qualquer DataTables adicionais ao DataSet habilitado de simultaneidade otimista para este tutorial, conforme nossa página ASP.NET fornecerá apenas atualizando e excluindo informações de produto. No entanto, ainda precisamos adicionar o `GetProductByProductID(productID)` método para o `ProductsOptimisticConcurrency` TableAdapter.

Para fazer isso, clique na barra de título do TableAdapter (à direita da área acima de `Fill` e `GetProducts` nomes de método) e escolha Adicionar consulta no menu de contexto. Isso iniciará o Assistente de configuração de consulta do TableAdapter. Como com a configuração inicial do nosso TableAdapter, optar por criar o `GetProductByProductID(productID)` método usando uma instrução de SQL ad hoc (consulte a Figura 4). Como o `GetProductByProductID(productID)` método retorna informações sobre um produto específico, para indicar que esta consulta é um `SELECT` tipo que retorna linhas de consulta.


[![Marque o tipo de consulta como um &quot;SELECT que retorna linhas&quot;](implementing-optimistic-concurrency-cs/_static/image26.png)](implementing-optimistic-concurrency-cs/_static/image25.png)

**Figura 9**: marcar o tipo de consulta como um "`SELECT` que retorna linhas" ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image27.png))


Na próxima tela, serão solicitados para a consulta SQL a ser usado com a consulta do TableAdapter padrão pré-carregado. Ampliar a consulta existente para incluir a cláusula `WHERE ProductID = @ProductID`, conforme mostrado na Figura 10.


[![Adicionar um onde cláusula para a consulta pré-carregado para retornar um registro de produto específico](implementing-optimistic-concurrency-cs/_static/image29.png)](implementing-optimistic-concurrency-cs/_static/image28.png)

**Figura 10**: adicionar um `WHERE` cláusula à consulta Pre-Loaded para retornar um registro de produto específico ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image30.png))


Por fim, altere os nomes de método gerado para `FillByProductID` e `GetProductByProductID`.


[![Renomeie os métodos FillByProductID e GetProductByProductID](implementing-optimistic-concurrency-cs/_static/image32.png)](implementing-optimistic-concurrency-cs/_static/image31.png)

**Figura 11**: renomear os métodos para `FillByProductID` e `GetProductByProductID` ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image33.png))


Com este assistente for concluído, o TableAdapter agora contém dois métodos para recuperar dados: `GetProducts()`, que retorna *todos os* produtos; e `GetProductByProductID(productID)`, que retorna o produto especificado.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Etapa 3: Criar uma camada de lógica de negócios para a DAL de habilitada a simultaneidade otimista

Nosso existente `ProductsBLL` classe tem exemplos de como usar a atualização em lotes e os padrões diretas do banco de dados. O `AddProduct` método e `UpdateProduct` ambas as sobrecargas usam o padrão de atualização em lotes, passando um `ProductRow` instância para o método Update do TableAdapter. O `DeleteProduct` método, por outro lado, usa o padrão de direta do banco de dados, chamando o TableAdapter `Delete(productID)` método.

Com o novo `ProductsOptimisticConcurrency` TableAdapter, o banco de dados direta métodos agora exigem que os valores originais também ser passado. Por exemplo, o `Delete` método agora espera dez parâmetros de entrada: original `ProductID`, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`, e `Discontinued`. Ele usa valores desses parâmetros de entrada adicionais `WHERE` cláusula do `DELETE` instrução enviada para o banco de dados somente para excluir o registro especificado se os valores atuais do banco de dados do mapa até os originais.

Enquanto a assinatura do método para o TableAdapter `Update` método usado no padrão de atualização de lote não for alterado, o código necessário para registrar os valores novos e originais tem. Portanto, em vez de tentar usar DAL habilitado a simultaneidade otimista com nosso existente `ProductsBLL` classe, vamos criar uma nova classe de camada de lógica comercial para trabalhar com nosso novo DAL.

Adicione uma classe denominada `ProductsOptimisticConcurrencyBLL` para o `BLL` pasta dentro de `App_Code` pasta.


![Adicione a classe de ProductsOptimisticConcurrencyBLL para a pasta BLL](implementing-optimistic-concurrency-cs/_static/image34.png)

**Figura 12**: adicionar o `ProductsOptimisticConcurrencyBLL` classe para a pasta BLL


Em seguida, adicione o seguinte código para o `ProductsOptimisticConcurrencyBLL` classe:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample5.cs)]

Observe o uso `NorthwindOptimisticConcurrencyTableAdapters` instrução acima do início da declaração de classe. O `NorthwindOptimisticConcurrencyTableAdapters` namespace contém o `ProductsOptimisticConcurrencyTableAdapter` classe, que fornece os métodos da DAL. Também antes da declaração de classe, você encontrará o `System.ComponentModel.DataObject` atributo, que instrui o Visual Studio para incluir essa classe na lista suspensa de ObjectDataSource do assistente.

O `ProductsOptimisticConcurrencyBLL`do `Adapter` propriedade fornece acesso rápido a uma instância do `ProductsOptimisticConcurrencyTableAdapter` classe e segue o padrão usado em nosso classes BLL originais (`ProductsBLL`, `CategoriesBLL`e assim por diante). Por fim, o `GetProducts()` método simplesmente chama para baixo a DAL `GetProducts()` método e retorna um `ProductsOptimisticConcurrencyDataTable` objeto preenchido com um `ProductsOptimisticConcurrencyRow` instância para cada registro de produto no banco de dados.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Excluindo um produto usando o padrão de direta do banco de dados com simultaneidade otimista

Ao usar o padrão de direta do banco de dados em relação a uma DAL que usa a simultaneidade otimista, os métodos devem ser passados para os valores novos e originais. Para excluir, não existem novos valores, portanto apenas os valores originais precisam ser passados. Em nosso BLL, em seguida, podemos deve aceitar todos os parâmetros originais como parâmetros de entrada. Vamos dar o `DeleteProduct` método o `ProductsOptimisticConcurrencyBLL` classe usar o método direto do banco de dados. Isso significa que esse método precisa levar em todos os campos de dados de produto dez como parâmetros de entrada e passe para a DAL, conforme mostrado no código a seguir:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample6.cs)]

Se os valores originais - os valores que foram carregada pela última vez em GridView (ou DetailsView ou FormView) - diferem dos valores no banco de dados quando o usuário clica no botão excluir o `WHERE` cláusula não corresponde a qualquer registro de banco de dados e não há registros serão afetados. Portanto, o TableAdapter `Delete` método retornará `0` e o BLL `DeleteProduct` método retornará `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Atualizar um produto usando o padrão de atualização em lotes com simultaneidade otimista

Conforme observado anteriormente, o TableAdapter `Update` método para o padrão de atualização em lotes tem a mesma assinatura de método, independentemente de estarem ou não a simultaneidade otimista é utilizada. Ou seja, o `Update` método espera um DataRow, uma matriz de DataRows, uma DataTable ou um conjunto de dados tipado. Não há nenhum parâmetro de entrada adicional para especificar os valores originais. Isso é possível porque a DataTable mantém controle sobre os valores originais e modificados para seu DataRow(s). Quando a DAL emite seu `UPDATE` instrução, o `@original_ColumnName` parâmetros são preenchidos com os valores originais da DataRow, enquanto o `@ColumnName` parâmetros são preenchidos com os valores modificados do DataRow.

No `ProductsBLL` classe (que usa o nosso simultaneidade original, não otimista DAL), ao usar o padrão de atualização em lotes para atualizar as informações de produto nosso código executa a seguinte sequência de eventos:

1. Leia as informações de produto do banco de dados atual em um `ProductRow` instância usando o TableAdapter `GetProductByProductID(productID)` método
2. Atribuir novos valores para a `ProductRow` instância da etapa 1
3. Chamar o TableAdapter `Update` método, passando o `ProductRow` instância

Esta sequência de etapas, no entanto, não suporta corretamente a simultaneidade otimista porque o `ProductRow` preenchidos na etapa 1 é populado diretamente do banco de dados, que significa que os valores originais usados pela DataRow são aqueles que existem atualmente do banco de dados e não para aqueles que foram associados a GridView no início do processo de edição. Em vez disso, ao usar um otimista simultaneidade-habilitado DAL, precisamos alterar o `UpdateProduct` sobrecargas do método para usar as etapas a seguir:

1. Leia as informações de produto do banco de dados atual em um `ProductsOptimisticConcurrencyRow` instância usando o TableAdapter `GetProductByProductID(productID)` método
2. Atribuir o *original* valores para a `ProductsOptimisticConcurrencyRow` instância da etapa 1
3. Chamar o `ProductsOptimisticConcurrencyRow` da instância `AcceptChanges()` método, que instrui o DataRow que seus valores atuais são aqueles "originais"
4. Atribuir o *novo* valores para a `ProductsOptimisticConcurrencyRow` instância
5. Chamar o TableAdapter `Update` método, passando o `ProductsOptimisticConcurrencyRow` instância

Etapa 1 leituras em todos os valores atuais do banco de dados para o registro do produto especificado. Esta etapa é supérflua no `UpdateProduct` sobrecarga que atualiza *todas as* das colunas de produto (como esses valores são substituídos na etapa 2), mas é essencial para essas sobrecargas em que apenas um subconjunto dos valores de coluna são passados como parâmetros de entrada. Depois que os valores originais foram atribuídos ao `ProductsOptimisticConcurrencyRow` instância, o `AcceptChanges()` método é chamado, que marca os valores de DataRow atuais como os valores originais a serem usados no `@original_ColumnName` parâmetros no `UPDATE` instrução. Em seguida, os novos valores de parâmetro são atribuídos para a `ProductsOptimisticConcurrencyRow` e, finalmente, o `Update` método é chamado, passando a DataRow.

O código a seguir mostra o `UpdateProduct` como parâmetros de entrada de campos de sobrecarga que aceita todos os dados de produto. Enquanto não mostrados aqui, o `ProductsOptimisticConcurrencyBLL` classe incluído no download para este tutorial também contém um `UpdateProduct` sobrecarga que aceita apenas o produto nome e preço como parâmetros de entrada.


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample7.cs)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Etapa 4: Passando os valores novos e originais de página ASP.NET para os métodos BLL

Com a DAL e BLL concluída, tudo o que permanece é criar uma página ASP.NET que pode utilizar a lógica de simultaneidade otimista interna do sistema. Especificamente, os dados de controle da Web (o GridView, DetailsView ou FormView) lembre-se que aos valores originais e ObjectDataSource devem passar os dois conjuntos de valores para a camada de lógica de negócios. Além disso, a página ASP.NET deve ser configurada para lidar com violações de concorrência.

Comece abrindo o `OptimisticConcurrency.aspx` página o `EditInsertDelete` pasta e adicionar um controle GridView para o Designer, definindo seu `ID` propriedade `ProductsGrid`. De marca inteligente do GridView, optar por criar um novo ObjectDataSource denominado `ProductsOptimisticConcurrencyDataSource`. Como queremos que este ObjectDataSource para usar a DAL que dá suporte a simultaneidade otimista, configurá-lo para usar o `ProductsOptimisticConcurrencyBLL` objeto.


[![Ter o uso de ObjectDataSource o objeto ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-cs/_static/image36.png)](implementing-optimistic-concurrency-cs/_static/image35.png)

**Figura 13**: usar ObjectDataSource o `ProductsOptimisticConcurrencyBLL` objeto ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image37.png))


Escolha o `GetProducts`, `UpdateProduct`, e `DeleteProduct` métodos de listas suspensas no assistente. Para o método UpdateProduct, use a sobrecarga que aceita todos os campos de dados do produto.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Configurando propriedades do controle ObjectDataSource

Depois de concluir o assistente, declarativo do ObjectDataSource deve parecer com o seguinte:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample8.aspx)]

Como você pode ver, o `DeleteParameters` coleção contém um `Parameter` instância para cada um dos parâmetros de entrada dez no `ProductsOptimisticConcurrencyBLL` da classe `DeleteProduct` método. Da mesma forma, o `UpdateParameters` coleção contém um `Parameter` instância para cada um dos parâmetros de entrada em `UpdateProduct`.

Para esses tutoriais anteriores que envolvidos modificação de dados, removeríamos o ObjectDataSource `OldValuesParameterFormatString` propriedade neste ponto, desde que esta propriedade indica que o método BLL espera os valores antigos (ou originais) deve ser passado nas, bem como os novos valores. Além disso, o valor dessa propriedade indica os nomes de parâmetro de entrada para os valores originais. Como, passa os valores originais em BLL, fazer *não* remover esta propriedade.

> [!NOTE]
> O valor de `OldValuesParameterFormatString` propriedade deve mapear para os nomes de parâmetro de entrada na BLL que espera que os valores originais. Desde que esses parâmetros nomeados `original_productName`, `original_supplierID`, e assim por diante, você pode deixar o `OldValuesParameterFormatString` valor da propriedade como `original_{0}`. Se, no entanto, os parâmetros de entrada dos métodos BLL tinham nomes como `old_productName`, `old_supplierID`, e assim por diante, você precisa atualizar o `OldValuesParameterFormatString` propriedade `old_{0}`.


Há uma configuração de propriedade final que precisa ser feita em ordem para ObjectDataSource corretamente passar os valores originais para os métodos BLL. ObjectDataSource tem um [propriedade ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) que podem ser atribuídos a [um de dois valores](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges` -o valor padrão. não envia os valores originais para parâmetros de entrada original dos métodos BLL
- `CompareAllValues` -envia os valores originais para os métodos BLL; Escolha esta opção ao usar a simultaneidade otimista

Reserve um tempo para definir a `ConflictDetection` propriedade `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Configurando o GridView propriedades e campos

Com as propriedades do ObjectDataSource configuradas corretamente, vamos examinar a configuração de GridView. Primeiro, como queremos que o GridView para oferecer suporte a edição e exclusão, clique nas caixas de seleção Permitir edição e habilitar a exclusão de marca inteligente do GridView. Isso adicionará uma CommandField cujo `ShowEditButton` e `ShowDeleteButton` são definidos como `true`.

Quando associado para o `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView contém um campo para cada um dos campos de dados do produto. Enquanto um controle de GridView pode ser editado, a experiência do usuário é qualquer coisa aceitável. O `CategoryID` e `SupplierID` BoundFields será renderizado como caixas de texto, exigir que o usuário insira a categoria apropriada e fornecedor como números de ID. Não haverá nenhuma formatação para os campos numéricos e não há controles de validação para garantir que o nome do produto foi fornecido e que o preço unitário unidades em estoque, unidades em ordem e os valores de nível de estoque são valores numéricos adequados e são maiores do que ou igual a como zero.

Conforme abordado no *adicionar controles de validação para a edição e inserindo Interfaces* e *Personalizando a Interface de modificação de dados* tutoriais, a interface do usuário pode ser personalizada com substituindo o BoundFields com TemplateFields. Eu tenha modificado Este GridView e sua interface de edição das seguintes maneiras:

- Removida a `ProductID`, `SupplierName`, e `CategoryName` BoundFields
- Converter o `ProductName` BoundField em um TemplateField e adicionou um controle RequiredFieldValidation.
- Converter o `CategoryID` e `SupplierID` BoundFields para TemplateFields e ajustado a interface de edição para usar DropDownLists em vez de caixas de texto. Em 'esses TemplateFields `ItemTemplates`, o `CategoryName` e `SupplierName` campos de dados são exibidos.
- Converter o `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` BoundFields TemplateFields e controles CompareValidator adicionados.

Como já examinamos como realizar essas tarefas nos tutoriais anteriores, vou apenas lista a sintaxe final declarativa aqui e deixe a implementação prática.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample9.aspx)]

Estamos muito perto de ter um exemplo totalmente funcional. No entanto, há algumas sutilezas que surgir backup e causar nos problemas. Além disso, ainda precisamos de alguma interface que alerta o usuário quando ocorreu uma violação de concorrência.

> [!NOTE]
> Para dados de um controle da Web para passar corretamente os valores originais para ObjectDataSource (que são passados para o BLL), é essencial que o GridView `EnableViewState` está definida como `true` (o padrão). Se você desabilitar o estado de exibição, os valores originais são perdidos em um postback.


## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Passando os valores originais corretos para ObjectDataSource

Há alguns problemas com a maneira como o GridView foi configurado. Se o ObjectDataSource `ConflictDetection` está definida como `CompareAllValues` (como é nosso), quando o ObjectDataSource `Update()` ou `Delete()` métodos são chamados pelo GridView (ou DetailsView ou FormView), o ObjectDataSource tenta copiar o Valores de GridView originais em seu apropriado `Parameter` instâncias. Consulte a Figura 2 para uma representação gráfica desse processo.

Especificamente, os valores originais do GridView são atribuídos os valores nas instruções de associação de dados bidirecional cada vez que os dados serão associados à GridView. Portanto, é essencial que os valores necessários originais todos os são capturados por meio de associação de dados bidirecional e que são fornecidas em um formato que pode ser convertido.

Para ver por que isso é importante, dedique alguns momentos para visite nossa página em um navegador. Conforme o esperado, GridView lista cada produto com um botão Editar e excluir na coluna mais à esquerda.


[![Os produtos são listados em um GridView](implementing-optimistic-concurrency-cs/_static/image39.png)](implementing-optimistic-concurrency-cs/_static/image38.png)

**Figura 14**: os produtos listados em um GridView ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image40.png))


Se você clicar no botão Excluir para qualquer produto, um `FormatException` é gerada.


[![Tentativa de excluir qualquer resultado de produto em um FormatException](implementing-optimistic-concurrency-cs/_static/image42.png)](implementing-optimistic-concurrency-cs/_static/image41.png)

**Figura 15**: tentativa de excluir qualquer produto resulta em uma `FormatException` ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image43.png))


O `FormatException` é gerado quando o ObjectDataSource tenta ler no original `UnitPrice` valor. Como o `ItemTemplate` tem o `UnitPrice` formatado como uma moeda (`<%# Bind("UnitPrice", "{0:C}") %>`), ele inclui um símbolo de moeda, como $19,95. O `FormatException` ocorre por ObjectDataSource tenta converter essa cadeia de caracteres em um `decimal`. Para contornar esse problema, temos um número de opções:

- Remover formatação de moeda a `ItemTemplate`. Isto é, em vez de usar `<%# Bind("UnitPrice", "{0:C}") %>`, basta usar `<%# Bind("UnitPrice") %>`. A desvantagem é que o preço não está formatado.
- Exibição de `UnitPrice` formatado como uma moeda no `ItemTemplate`, mas usar o `Eval` palavra-chave para fazer isso. Lembre-se de que `Eval` executa a associação unidirecional. Ainda é preciso fornecer o `UnitPrice` valor para os valores originais, ainda será necessário uma instrução de associação de dados bidirecional no `ItemTemplate`, mas isso pode ser colocado em um controle de rótulo Web cujo `Visible` está definida como `false`. Podemos usar a seguinte marcação ItemTemplate:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample10.aspx)]

- Remover formatação de moeda a `ItemTemplate`usando `<%# Bind("UnitPrice") %>`. No GridView `RowDataBound` manipulador de eventos, por meio de programação de acesso Web rótulo de controle dentro do qual o `UnitPrice` valor é exibido e defina seu `Text` propriedade para a versão formatada.
- Deixe o `UnitPrice` formatado como uma moeda. No GridView `RowDeleting` manipulador de eventos, substituir a existente original `UnitPrice` valor (US $19,95) com um valor decimal real usando `Decimal.Parse`. Vimos como fazer algo semelhante a `RowUpdating` manipulador de eventos a [ *tratamento BLL e DAL nível exceções em uma página ASP.NET* ](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) tutorial.

Para o exemplo que escolheram a segunda abordagem, adicionar uma Web rótulo oculto controle cuja `Text` propriedade é bidirecionais dados associados para o não formatado `UnitPrice` valor.

Depois de resolver esse problema, tente clicar no botão Excluir para qualquer produto novamente. Neste momento, você obterá um `InvalidOperationException` quando tenta invocar a BLL ObjectDataSource `UpdateProduct` método.


[![ObjectDataSource não é possível encontrar um método com os parâmetros de entrada que deseja enviar](implementing-optimistic-concurrency-cs/_static/image45.png)](implementing-optimistic-concurrency-cs/_static/image44.png)

**Figura 16**: ObjectDataSource não é possível encontrar um método com os parâmetros de entrada que deseja enviar ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image46.png))


Examinar a mensagem da exceção, fica claro que ObjectDataSource deseja invocar um BLL `DeleteProduct` método inclui `original_CategoryName` e `original_SupplierName` parâmetros de entrada. Isso ocorre porque o `ItemTemplate` s para o `CategoryID` e `SupplierID` TemplateFields atualmente contêm instruções de associação bidirecionais com o `CategoryName` e `SupplierName` campos de dados. Em vez disso, é preciso incluir `Bind` instruções com o `CategoryID` e `SupplierID` campos de dados. Para fazer isso, substitua as instruções de associação existentes com `Eval` instruções e, em seguida, adicionar controles de rótulo oculto cujo `Text` propriedades associadas a `CategoryID` e `SupplierID` campos de dados usando a associação de dados bidirecional, conforme mostrado abaixo:


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample11.aspx)]

Com essas alterações, agora é possível excluir e editar as informações de produto com êxito! Etapa 5, examinaremos como verificar se as violações de concorrência estão sendo detectadas. Mas por enquanto, levar alguns minutos e tente a atualização e exclusão de alguns registros para garantir que a atualização e exclusão para um único usuário funciona conforme o esperado.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Etapa 5: Testando o suporte a simultaneidade otimista

Para verificar se as violações de concorrência estão sendo detectado (em vez de resultando em dados que está sendo substituídos cegamente), precisamos abrir duas janelas de navegador para esta página. Em ambas as instâncias do navegador, clique no botão Editar para Chai. Em seguida, em apenas um dos navegadores, altere o nome para "Chai chá" e clique em Atualizar. A atualização deve ter êxito e retornar GridView para seu estado de edição previamente, com "Chai chá" como o novo nome do produto.

A outra instância de janela do navegador, no entanto, o nome do produto TextBox ainda mostra "Chai". Essa segunda janela do navegador, atualizar o `UnitPrice` para `25.00`. Sem suporte a simultaneidade otimista, clicando em Atualizar na segunda instância de navegador alteraria o nome do produto para "Chai", substituindo as alterações feitas pela primeira instância do navegador. Com simultaneidade otimista empregada, contudo, clicando no botão de atualização na segunda instância de navegador resulta em uma [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).


[![Quando for detectada uma violação de concorrência, um DBConcurrencyException é gerada](implementing-optimistic-concurrency-cs/_static/image48.png)](implementing-optimistic-concurrency-cs/_static/image47.png)

**Figura 17**: quando uma violação de concorrência é detectada, um `DBConcurrencyException` é gerada ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image49.png))


O `DBConcurrencyException` só é gerada quando o padrão de atualização em lotes da DAL é utilizado. O padrão de direta do banco de dados não gerará uma exceção, ela simplesmente indica que nenhuma linha foi afetada. Para ilustrar isso, retorne GridView ambas as instâncias do navegador para seu estado de edição previamente. Em seguida, na primeira instância do navegador, clique no botão Editar e alterar o nome do produto de "Chai chá" para "Chai" e clique em Atualizar. Na segunda janela do navegador, clique no botão Excluir para Chai.

Ao clicar em Excluir, a página envia de volta, GridView invoca o ObjectDataSource `Delete()` método e ObjectDataSource chama para baixo o `ProductsOptimisticConcurrencyBLL` da classe `DeleteProduct` método, passando os valores originais. O original `ProductName` valor para a segunda instância do navegador é "Chai chá", que não corresponde a atual `ProductName` valor no banco de dados. Portanto o `DELETE` instrução emitida para o banco de dados afeta zero linhas porque não há nenhum registro no banco de dados que o `WHERE` atenda a cláusula. O `DeleteProduct` método retorna `false` e dados do ObjectDataSource é vinculada outra vez a GridView.

Da perspectiva do usuário final, clicando no botão Excluir para Chá Chai na segunda janela do navegador causou a tela piscar e, após voltem, o produto ainda estiver ativa, embora agora ele está listado como "Chai" (a produto nome alteração feita pelo navegador primeiro instância). Se o usuário clica no botão Excluir novamente, a exclusão terá êxito, como original do GridView `ProductName` valor ("Chai") agora coincide com com o valor no banco de dados.

Em ambos os casos, a experiência do usuário é distantes ideal. Claramente não queremos Mostrar ao usuário os detalhes sobre o `DBConcurrencyException` exceção ao usar o padrão de atualização em lotes. E o comportamento ao usar o padrão de direta do banco de dados é um pouco confuso, como o comando usuários falhou, mas não havia nenhuma indicação precisa do motivo.

Para solucionar esses dois problemas, podemos criar controles da Web de rótulo na página que fornecem uma explicação de por que uma atualização ou exclusão falhou. Para o padrão de atualização em lotes, é possível determinar se um `DBConcurrencyException` ocorreu uma exceção no manipulador de evento de pós-nível do GridView exibindo o rótulo de aviso, conforme necessário. Para o método direto do banco de dados, podemos examinar o valor de retorno do método BLL (que é `true` se uma linha foi afetada, `false` contrário) e exibir uma mensagem informativa conforme o necessário.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Etapa 6: Adicionando mensagens informativas e exibi-los no caso de uma violação de concorrência

Quando ocorre uma violação de concorrência, o comportamento apresentado depende se a DAL atualização em lotes ou padrão direta do banco de dados foi usado. Nosso tutorial usa ambos os padrões, com o padrão de atualização do lote que está sendo usado para atualizar e o padrão de direta de banco de dados usado para excluir. Para começar, vamos adicionar dois controles da Web de rótulo à página que explicam o que ocorreu uma violação de concorrência durante a tentativa de excluir ou atualizar dados. Definir o controle de rótulo `Visible` e `EnableViewState` propriedades `false`; isso causará a ser ocultada na visita cada página, exceto para aqueles determinada página visita onde seus `Visible` propriedade é definida por meio de programação como `true`.


[!code-aspx[Main](implementing-optimistic-concurrency-cs/samples/sample12.aspx)]

Além de configuração de suas `Visible`, `EnabledViewState`, e `Text` propriedades, também defini o `CssClass` propriedade `Warning`, que faz com que o rótulo da ser exibido em uma fonte grande, vermelha, itálico, negrito. Este CSS `Warning` classe foi definida e adicionado ao Styles novamente o *examinando os eventos associados inserindo, atualizando e excluindo* tutorial.

Depois de adicionar esses rótulos, o Designer no Visual Studio deve ser semelhante à Figura 18.


[![Foram adicionados dois controles de rótulo para a página](implementing-optimistic-concurrency-cs/_static/image51.png)](implementing-optimistic-concurrency-cs/_static/image50.png)

**Figura 18**: dois rótulo controles foram adicionados à página ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image52.png))


Com esses controles da Web de rótulo em vigor, estamos prontos para examinar como determinar quando uma violação de concorrência ocorreu, em que ponto o rótulo apropriado `Visible` propriedade pode ser definida como `true`, exibindo a mensagem informativa.

## <a name="handling-concurrency-violations-when-updating"></a>Tratar de violações de simultaneidade ao atualizar

Vamos primeiro examinar como lidar com violações de concorrência ao usar o padrão de atualização em lotes. Como essas violações com o lote atualizar causa padrão um `DBConcurrencyException` exceção seja lançada, precisamos adicionar código para nossa página ASP.NET para determinar se um `DBConcurrencyException` exceção ocorreu durante o processo de atualização. Se assim, podemos deve exibir uma mensagem para o usuário explicando que suas alterações não foram salvas porque outro usuário tiver modificado os mesmos dados entre quando eles começaram a edição do registro e quando eles clicou no botão de atualização.

Como vimos no *tratamento BLL e DAL nível exceções em uma página ASP.NET* tutorial, essas exceções podem ser detectadas e suprimidas em manipuladores de evento de pós-nível do controle da Web de dados. Portanto, precisamos criar um manipulador de eventos do GridView `RowUpdated` evento verifica se um `DBConcurrencyException` exceção foi gerada. Este manipulador de eventos é passado uma referência a qualquer exceção que foi gerada durante o processo de atualização, conforme mostrado no evento manipulador de código abaixo:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample13.cs)]

A face de um `DBConcurrencyException` este manipulador de eventos de exceção, exibe o `UpdateConflictMessage` controle de rótulo e indica que a exceção foi tratada. Com esse código, quando uma violação de concorrência ocorre ao atualizar um registro, as alterações do usuário são perdidas, desde que eles talvez tenham sido substituídos modificações de outro usuário ao mesmo tempo. Em particular, GridView é retornada ao seu estado de edição previamente e associada ao banco de dados atual. Isso atualizará a linha GridView com as outras alterações do usuário, que foram anteriormente não é visíveis. Além disso, o `UpdateConflictMessage` controle de rótulo explicará ao usuário ocorrência. Esta sequência de eventos é detalhada na Figura 19.


[![Um usuário s atualizações serão perdidas em Face de uma violação de concorrência](implementing-optimistic-concurrency-cs/_static/image54.png)](implementing-optimistic-concurrency-cs/_static/image53.png)

**Figura 19**: um usuário s atualizações serão perdidas em Face de uma violação de concorrência ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image55.png))


> [!NOTE]
> Como alternativa, em vez de retornar o GridView para o estado de edição previamente, pode deixar o GridView em seu estado de edição definindo o `KeepInEditMode` propriedade transmitido `GridViewUpdatedEventArgs` objeto como true. Se você usar essa abordagem, no entanto, certifique-se associar novamente os dados do GridView (invocando seu `DataBind()` método) para que os valores do outro usuário são carregados para a interface de edição. O código disponível para download com este tutorial tem estas duas linhas de código a `RowUpdated` manipulador de eventos comentados; simplesmente remova essas linhas de código para que o GridView permanecem no modo de edição após uma violação de concorrência.


## <a name="responding-to-concurrency-violations-when-deleting"></a>Respondendo a violações de concorrência ao excluir

Com o padrão de direta do banco de dados, não há nenhuma exceção no caso de uma violação de concorrência. Em vez disso, a instrução de banco de dados simplesmente não afeta nenhum registro, como a cláusula WHERE não coincide com qualquer registro. Todos os métodos de modificação de dados criados na BLL foram criados, de modo que elas retornam um valor Boolean indicando se eles afetados exatamente um registro. Portanto, para determinar se ocorreu uma violação de simultaneidade ao excluir um registro, podemos examinar o valor de retorno do BLL `DeleteProduct` método.

O valor de retorno de um método BLL pode ser examinado em manipuladores de evento de pós-nível do ObjectDataSource por meio de `ReturnValue` propriedade o `ObjectDataSourceStatusEventArgs` objeto passado para o manipulador de eventos. Como estamos interessados em determinar o valor de retorno de `DeleteProduct` método, é preciso criar um manipulador de eventos para o ObjectDataSource `Deleted` eventos. O `ReturnValue` é de propriedade do tipo `object` e pode ser `null` se uma exceção foi gerada e o método foi interrompido antes que ele poderia retornar um valor. Portanto, podemos deve certificar-se que o `ReturnValue` propriedade não é `null` e é um valor booleano. Supondo que a verificação passar, mostramos o `DeleteConflictMessage` controle de rótulo, se o `ReturnValue` é `false`. Isso pode ser feito usando o código a seguir:


[!code-csharp[Main](implementing-optimistic-concurrency-cs/samples/sample14.cs)]

No caso de uma violação de concorrência, a solicitação de exclusão do usuário será cancelada. GridView é atualizada, mostrando as alterações que ocorreram para o registro entre a hora em que o usuário carregar a página e quando ele clica no botão Excluir. Quando ocorre uma violação desse tipo, o `DeleteConflictMessage` rótulo é exibido, explicando o que aconteceu apenas (consulte a Figura 20).


[![Um usuário s Delete é cancelado no caso de uma violação de concorrência](implementing-optimistic-concurrency-cs/_static/image57.png)](implementing-optimistic-concurrency-cs/_static/image56.png)

**Figura 20**: um usuário s Delete é cancelado no caso de uma violação de concorrência ([clique para exibir a imagem em tamanho normal](implementing-optimistic-concurrency-cs/_static/image58.png))


## <a name="summary"></a>Resumo

Oportunidades de violações de concorrência existem em todos os aplicativos que permite que vários usuários simultâneos para atualizar ou excluir dados. Se essas violações não são contadas, quando dois usuários simultaneamente atualizam os mesmos dados quem obtém o última gravação "wins", substituindo o outro usuário para alterações de alterações. Como alternativa, os desenvolvedores podem implementar o controle de simultaneidade otimista ou pessimista. Controle de simultaneidade otimista pressupõe que as violações de simultaneidade são incomuns e simplesmente não permite uma atualização ou excluir o comando que seria constituem uma violação de concorrência. Controle de simultaneidade pessimista pressupõe que a simultaneidade violações são frequentes e simplesmente rejeitar um usuário atualizar ou excluir o comando não é aceitável. Com o controle de simultaneidade pessimista, atualizando um registro envolve bloqueá-lo, impedindo assim a quaisquer outros usuários de modificar ou excluir o registro enquanto ele estiver bloqueado.

O conjunto de dados tipado no .NET fornece funcionalidade para oferecer suporte a controle de simultaneidade otimista. Em particular, o `UPDATE` e `DELETE` instruções emitidas para o banco de dados incluem todas as colunas da tabela, garantindo assim que a atualização ou exclusão só ocorrerá se os dados do registro atual correspondem com os dados originais, o usuário tinha quando executar o update ou delete. Depois que a DAL tiver sido configurada para dar suporte a simultaneidade otimista, os métodos BLL precisam ser atualizados. Além disso, a página ASP.NET que chama o BLL deve ser configurada, de modo que o ObjectDataSource recupera os valores originais de controle da Web seus dados e os passa para em BLL.

Como vimos neste tutorial, implementar o controle de simultaneidade otimista em um aplicativo web ASP.NET envolve atualizar a DAL e BLL e adição de suporte na página ASP.NET. Se esse trabalho adicional é um bom investimento de tempo e esforço depende de seu aplicativo. Se você raramente tem a atualização de dados de usuários simultâneos, ou os dados que estão atualizando são diferentes uns dos outros, controle de simultaneidade não é um problema de chave. Se, no entanto, você normalmente tem vários usuários em seu site trabalhando com os mesmos dados, controle de simultaneidade pode ajudar a evitar atualizações de um usuário ou exclusões substituam inadvertidamente da outra.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](customizing-the-data-modification-interface-cs.md)
> [Próximo](adding-client-side-confirmation-when-deleting-cs.md)
