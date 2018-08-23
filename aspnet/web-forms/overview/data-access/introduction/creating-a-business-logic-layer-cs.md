---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: Criando uma camada de lógica de negócios (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como centralizar suas regras de negócios em um negócio lógica BLL (camada) que serve como um intermediário para troca de dados entre o t...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 0db90f1e87bcaac51ca08ef1a8b258c93be8f613
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832453"
---
<a name="creating-a-business-logic-layer-c"></a>Criando uma camada de lógica de negócios (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe) ou [baixar PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> Neste tutorial, veremos como centralizar suas regras de negócios em um negócio lógica BLL (camada) que serve como um intermediário para troca de dados entre a camada de apresentação e a DAL.


## <a name="introduction"></a>Introdução

A camada de acesso de dados (DAL) criado na [primeiro tutorial](creating-a-data-access-layer-cs.md) separa claramente os dados de acessar a lógica da lógica de apresentação. No entanto, enquanto a DAL separa claramente os detalhes de acesso de dados da camada de apresentação, ele não impõe quaisquer regras de negócios que podem ser aplicadas. Por exemplo, para nosso aplicativo podemos querer não permitir a `CategoryID` ou `SupplierID` campos da `Products` a tabela a ser modificada quando a `Discontinued` campo é definido como 1 ou queremos impor regras de precedência, proibindo situações nas quais um funcionário é gerenciado por alguém que foi contratado depois deles. Outro cenário comum é que os usuários talvez somente autorização em uma função específica podem excluir produtos ou podem alterar o `UnitPrice` valor.

Neste tutorial, veremos como centralizar essas regras de negócios em um negócio lógica BLL (camada) que serve como um intermediário para troca de dados entre a camada de apresentação e a DAL. Em um aplicativo do mundo real, a BLL deve ser implementada como um projeto de biblioteca de classes separado; No entanto, para esses tutoriais, implementaremos a BLL como uma série de classes em nosso `App_Code` pasta para simplificar a estrutura do projeto. Figura 1 ilustra as relações de arquiteturas entre a camada de apresentação, BLL e DAL.


![A BLL separa a camada de apresentação da camada de acesso a dados e impõe regras de negócio](creating-a-business-logic-layer-cs/_static/image1.png)

**Figura 1**: A BLL separa a camada de apresentação da camada de acesso a dados e impõe regras de negócio


## <a name="step-1-creating-the-bll-classes"></a>Etapa 1: Criar as Classes de BLL

Nosso BLL será ser composto de quatro classes, uma para cada TableAdapter no DAL; cada uma dessas classes BLL terá métodos para recuperar, inserindo, atualizando e excluindo do respectivo TableAdapter na DAL, aplicando as regras de negócios apropriada.

Para mais clareza, separar as classes relacionadas a BLL e DAL, vamos criar duas subpastas na `App_Code` pasta, `DAL` e `BLL`. Simplesmente clique duas vezes no `App_Code` pasta no Gerenciador de soluções e escolha a nova pasta. Depois de criar essas duas pastas, mover o conjunto de dados tipados criados no primeiro tutorial para o `DAL` subpasta.

Em seguida, crie os quatro arquivos de classe BLL no `BLL` subpasta. Para fazer isso, clique com botão direito no `BLL` subpasta, escolha Adicionar um novo Item e escolha o modelo de classe. Nomeie as quatro classes `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, e `EmployeesBLL`.


![Adiciona quatro novas Classes para a pasta App_Code](creating-a-business-logic-layer-cs/_static/image2.png)

**Figura 2**: adiciona quatro novas Classes para o `App_Code` pasta


Em seguida, vamos adicionar métodos para cada uma das classes simplesmente encapsular os métodos definidos para os TableAdapters do primeiro tutorial. Por enquanto, esses métodos serão apenas chamar diretamente o DAL; Vamos retornar posteriormente para adicionar qualquer lógica de negócios necessários.

> [!NOTE]
> Se você estiver usando o Visual Studio Standard Edition ou superior (ou seja, você está *não* usando o Visual Web Developer), opcionalmente, você pode criar suas classes visualmente usando o [Designer de classe](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Consulte a [Blog de Designer de classe](https://blogs.msdn.com/classdesigner/default.aspx) para obter mais informações sobre esse novo recurso no Visual Studio.


Para o `ProductsBLL` , precisamos adicionar um total de sete métodos de classe:

- `GetProducts()` Retorna todos os produtos
- `GetProductByProductID(productID)` Retorna o produto com a ID de produto especificado
- `GetProductsByCategoryID(categoryID)` Retorna todos os produtos da categoria especificada
- `GetProductsBySupplier(supplierID)` Retorna todos os produtos do fornecedor especificado
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Insere um novo produto no banco de dados usando os valores passados no; Retorna o `ProductID` valor do registro inserido recentemente
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` atualiza um produto existente no banco de dados usando os valores passados em; Retorna `true` se exatamente uma linha foi atualizada, `false` caso contrário,
- `DeleteProduct(productID)` Exclui o produto especificado do banco de dados

ProductsBLL.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

Os métodos que simplesmente retornam dados `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, e `GetProductBySuppliersID` são bem simples, conforme eles simplesmente chamam para baixo da DAL. Embora em alguns cenários talvez existam regras de negócios que precisam ser implementados nesse nível (como as regras de autorização com base no usuário conectado no momento ou a função à qual pertence o usuário), vamos simplesmente deixar esses métodos como-está. Para esses métodos, em seguida, a BLL serve apenas como um proxy por meio do qual a camada de apresentação acessa os dados subjacentes da camada de acesso de dados.

O `AddProduct` e `UpdateProduct` métodos tanto usam como parâmetros de valores para os vários campos de produto e adicionar um novo produto ou atualizar um existente, respectivamente. Já que muitas as `Product` colunas da tabela podem aceitar `NULL` valores (`CategoryID`, `SupplierID`, e `UnitPrice`, para citar alguns exemplos), os parâmetros de entrada `AddProduct` e `UpdateProduct` que são mapeados para esse uso de colunas [tipos anuláveis](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Tipos anuláveis são novos para o .NET 2.0 e fornecer uma técnica para que indica se um tipo de valor deve, em vez disso, ser `null`. No c# você pode sinalizar um tipo de valor como um tipo anulável, adicionando `?` após o tipo (como `int? x;`). Consulte a [tipos anuláveis](https://msdn.microsoft.com/library/1t3y8s4s.aspx) seção o [guia de programação em c#](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx) para obter mais informações.

Todos os três métodos retornam um valor booliano que indica se uma linha foi inserida, atualizada ou excluída desde que a operação não pode resultar em uma linha afetada. Por exemplo, se o desenvolvedor da página chama `DeleteProduct` passando um `ProductID` para um produto inexistente, o `DELETE` instrução emitida para o banco de dados terão nenhum efeito e, portanto, o `DeleteProduct` método retornará `false`.

Observe que, ao adicionar um novo produto ou atualizar uma existente, adotamos nos valores de campo do produto novo ou modificado como uma lista de escalares em vez de aceitar um `ProductsRow` instância. Essa abordagem foi escolhida porque o `ProductsRow` classe deriva de ADO.NET `DataRow` classe, que não tem um construtor padrão sem parâmetros. Para criar um novo `ProductsRow` instância, primeiro devemos criar uma `ProductsDataTable` da instância e, em seguida, invocar seus `NewProductRow()` método (que fazemos `AddProduct`). Essa deficiência rears sua cabeça quando transformamos a inserção e atualização de produtos usando o ObjectDataSource. Em resumo, o ObjectDataSource tentará criar uma instância dos parâmetros de entrada. Se o método BLL espera um `ProductsRow` instância, o ObjectDataSource tentará criar um, mas falhar devido à falta de um construtor padrão sem parâmetros. Para obter mais informações sobre esse problema, consulte as seguintes duas postagens de fóruns do ASP.NET: [ObjectDataSources atualizando com conjuntos de dados fortemente definida](https://forums.asp.net/1098630/ShowPost.aspx), e [problema com o ObjectDataSource e conjunto de dados fortemente definida](https://forums.asp.net/1048212/ShowPost.aspx).

Em seguida, em ambos os `AddProduct` e `UpdateProduct`, o código cria um `ProductsRow` da instância e o preenche com os valores passados apenas. Ao atribuir valores a colunas de dados de uma DataRow podem ocorrer várias verificações de validação de nível de campo. Portanto, manualmente colocar os valores passados de volta em uma DataRow ajuda a garantir a validade dos dados que está sendo passados para o método BLL. Infelizmente as classes de DataRow fortemente tipada geradas pelo Visual Studio não usam tipos anuláveis. Em vez disso, para indicar que uma coluna de dados específica em uma DataRow deve corresponder a um `NULL` banco de dados valor, devemos usar o `SetColumnNameNull()` método.

Na `UpdateProduct` carregarmos pela primeira vez no produto para atualizar usando `GetProductByProductID(productID)`. Embora isso possa parecer desnecessário ao banco de dados, essa viagem extra revelará que vale a pena em tutoriais futuros que exploram a simultaneidade otimista. Simultaneidade otimista é uma técnica para garantir que dois usuários que trabalham simultaneamente nos mesmos dados não sejam substituídas acidentalmente as alterações uma da outra. Pegar todo o registro também torna mais fácil criar métodos de atualização na BLL que modifique apenas um subconjunto de colunas do DataRow. Quando vamos explorar o `SuppliersBLL` , veremos um exemplo de classe.

Observe que, por fim, o `ProductsBLL` classe tem o [atributo DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) aplicado a ele (a `[System.ComponentModel.DataObject]` sintaxe logo antes da instrução de classe na parte superior do arquivo) e os métodos têm [ Atributos de DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). O `DataObject` atributo marca a classe como sendo um objeto adequado para associação a um [controle ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), enquanto o `DataObjectMethodAttribute` indica a finalidade do método. Como veremos no futuro tutoriais, ObjectDataSource do ASP.NET 2.0 facilita declarativamente acessar dados de uma classe. Para ajudar a filtrar a lista de classes possíveis para associar a no Assistente do ObjectDataSource, por padrão, apenas essas classes marcadas como `DataObjects` são mostrados na lista suspensa do assistente. O `ProductsBLL` classe funcione tão bem sem esses atributos, mas adicioná-los torna mais fácil trabalhar com o Assistente do ObjectDataSource.

## <a name="adding-the-other-classes"></a>Adição de outras Classes

Com o `ProductsBLL` classe completa, ainda assim será preciso adicionar as classes para trabalhar com categorias, fornecedores e funcionários. Reserve um tempo para criar as seguintes classes e métodos com os conceitos do exemplo acima:

- **CategoriesBLL.cs**

    - `GetCategories()`
    - `GetCategoryByCategoryID(categoryID)`
- **SuppliersBLL.cs**

    - `GetSuppliers()`
    - `GetSupplierBySupplierID(supplierID)`
    - `GetSuppliersByCountry(country)`
    - `UpdateSupplierAddress(supplierID, address, city, country)`
- **EmployeesBLL.cs**

    - `GetEmployees()`
    - `GetEmployeeByEmployeeID(employeeID)`
    - `GetEmployeesByManager(managerID)`

Um método vale a pena observar é a `SuppliersBLL` da classe `UpdateSupplierAddress` método. Esse método fornece uma interface para atualizar apenas as informações de endereço do fornecedor. Internamente, esse método lê a `SupplierDataRow` o objeto especificado `supplierID` (usando `GetSupplierBySupplierID`), define suas propriedades relacionadas a endereço e, em seguida, chama para baixo o `SupplierDataTable`do `Update` método. O `UpdateSupplierAddress` método segue:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

Consulte o download deste artigo para minha implementação completa de classes a BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Etapa 2: Acessar datasets tipados através das Classes BLL

No primeiro tutorial vimos exemplos de como trabalhar diretamente com o conjunto de dados tipado, por meio de programação, mas com a adição de nossas aulas BLL, a camada de apresentação deve funcionar com a BLL em vez disso. No `AllProducts.aspx` exemplo do primeiro tutorial, o `ProductsTableAdapter` foi usado para associar a lista de produtos em um GridView, conforme mostrado no código a seguir:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

Para usar a BLL nova classes, tudo o que precisa ser alterado é a primeira linha do código simplesmente substituir a `ProductsTableAdapter` do objeto com um `ProductBLL` objeto:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

As classes BLL também podem ser acessadas declarativamente (como podem fazer o conjunto de dados tipado) usando o ObjectDataSource. Vamos abordar o ObjectDataSource mais detalhadamente nos tutoriais a seguir.


[![A lista de produtos é exibida em um GridView](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**Figura 3**: lista de produtos é exibida em um GridView ([clique para exibir a imagem em tamanho normal](creating-a-business-logic-layer-cs/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Etapa 3: Adicionando validação de nível de campo para as Classes de DataRow

Validação de nível de campo são verificações que pertence aos valores de propriedade dos objetos de negócios, ao inserir ou atualizar. Algumas regras de validação de nível de campo para os produtos incluem:

- O `ProductName` campo deve ser 40 caracteres ou menos
- O `QuantityPerUnit` campo deve ser 20 caracteres ou menos
- O `ProductID`, `ProductName`, e `Discontinued` campos são obrigatórios, mas todos os outros campos são opcionais
- O `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` campos devem ser maior que ou igual a zero

Essas regras podem e devem ser expressos no nível do banco de dados. O limite de caracteres na `ProductName` e `QuantityPerUnit` campos são capturados pelos tipos de dados dessas colunas na `Products` tabela (`nvarchar(40)` e `nvarchar(20)`, respectivamente). Se os campos são obrigatórios e opcionais são expressos por se a coluna de tabela do banco de dados permite `NULL` s. Quatro [restrições de verificação](https://msdn.microsoft.com/library/ms188258.aspx) existe que certifique-se de que o somente valores maiores que ou iguais a zero podem torná-lo para o `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, ou `ReorderLevel` colunas.

Além de aplicar essas regras no banco de dados que eles também devem ser impostos no nível do conjunto de dados. Na verdade, o tamanho do campo e se um valor é obrigatório ou opcional já são capturados para conjunto do cada DataTable de colunas de dados. Para ver a validação de nível de campo existente fornecida automaticamente, vá para o Designer de conjunto de dados, selecione um campo de uma do DataTables e, em seguida, vá para a janela de propriedades. Como mostra a Figura 4, o `QuantityPerUnit` DataColumn em de `ProductsDataTable` tem um comprimento máximo de 20 caracteres e permitir `NULL` valores. Se tentarmos definir a `ProductsDataRow`do `QuantityPerUnit` propriedade para um valor de cadeia de caracteres mais de 20 caracteres um `ArgumentException` será lançada.


[![A DataColumn fornece validação básica de nível de campo](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**Figura 4**: A DataColumn fornece nível de campo validação básica ([clique para exibir a imagem em tamanho normal](creating-a-business-logic-layer-cs/_static/image8.png))


Infelizmente, estamos não é possível especificar verificações de limites, como o `UnitPrice` valor deve ser maior que ou igual a zero, na janela Propriedades. Para fornecer esse tipo de validação de nível de campo é necessário criar um manipulador de eventos para a DataTable [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) eventos. Conforme mencionado na [tutorial anterior](creating-a-data-access-layer-cs.md), os objetos de conjunto de dados, DataTables e DataRow criados pelo conjunto de dados tipado que podem ser estendidos com o uso de classes parciais. Usando essa técnica podemos criar uma `ColumnChanging` manipulador de eventos para o `ProductsDataTable` classe. Comece criando uma classe de `App_Code` pasta chamada `ProductsDataTable.ColumnChanging.cs`.


[![Adicione uma nova classe para a pasta App_Code](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**Figura 5**: Adicione uma nova classe para o `App_Code` pasta ([clique para exibir a imagem em tamanho normal](creating-a-business-logic-layer-cs/_static/image11.png))


Em seguida, crie um manipulador de eventos para o `ColumnChanging` evento que garante que o `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` valores de coluna (se não for `NULL`) são maiores que ou igual a zero. Se qualquer coluna desse tipo está fora do intervalo, lançar um `ArgumentException`.

ProductsDataTable.ColumnChanging.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Etapa 4: Adicionando regras de negócio personalizadas às Classes da BLL

Além de validação de nível de campo, pode haver regras de negócios personalizada de alto nível que envolvem entidades diferentes ou conceitos não podem ser expressados no nível de coluna única, como:

- Se um produto foi descontinuado, seu `UnitPrice` não pode ser atualizado
- País de um funcionário de residência deve ser o mesmo país do seu gerente de residência
- Um produto não pode ser descontinuado se ele é o único produto fornecido pelo fornecedor

As classes BLL devem conter verificações para garantir a aderência às regras de negócios do aplicativo. Essas verificações podem ser adicionadas diretamente para os métodos aos quais se aplicam.

Imagine que o nosso regras de negócios determinam que um produto não puderam ser marcado Descontinuado se fosse o único produto de um determinado fornecedor. Ou seja, se produto *X* era o único produto que adquirimos de fornecedor *Y*, não foi possível marcar *X* conforme descontinuada; se, no entanto, fornecedor *Y*fornecidos nos com três produtos, *um*, *B*, e *C*, podemos pode marcar qualquer e todos eles como descontinuado. Uma regra de negócio estranho, mas as regras de negócio e senso comum não estão alinhadas sempre!

Para impor essa regra de negócio no `UpdateProducts` método começamos verificando se `Discontinued` foi definida como `true` e, se assim, devemos chamar `GetProductsBySupplierID` para determinar quantos produtos adquirimos de fornecedor deste produto. Se apenas um produto é comprado esse fornecedor, geramos um `ApplicationException`.


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Respondendo a erros de validação na camada de apresentação

Ao chamar a BLL de camada de apresentação, pode decidir se deseja tentar tratar todas as exceções que possam ser geradas ou deixá-los propagado no ASP.NET (que irá gerar o `HttpApplication`do `Error` evento). Para lidar com uma exceção ao trabalhar programaticamente com a BLL, podemos usar um [try... catch](https://msdn.microsoft.com/library/0yd65esw.aspx) bloco, como mostra o exemplo a seguir:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

Como veremos em tutoriais futuros, tratando exceções da BLL ao usar dados de um controle da Web para inserir, atualizar, ou exclusão de dados pode ser manipulada diretamente em um manipulador de eventos em vez de encapsular o código em `try...catch` blocos.

## <a name="summary"></a>Resumo

Um aplicativo bem arquitetado é elaborado em camadas distintas, cada uma encapsulando uma determinada função. No primeiro tutorial desta série de artigo, criamos uma camada de acesso a dados usando conjuntos de dados tipados; Neste tutorial criamos uma camada de lógica de negócios como uma série de classes em nosso aplicativo `App_Code` pasta chamar nossa DAL. A BLL implementa a lógica de nível de campo e o nível de negócios para nosso aplicativo. Além de criar um BLL separado, como fizemos neste tutorial, outra opção é estender os métodos dos TableAdapters com o uso de classes parciais. No entanto, usar essa técnica permite substituir os métodos existentes nem-separar nossa DAL e nossa BLL corretamente como a abordagem tomadas neste artigo.

Com o DAL e BLL completa, estamos prontos para iniciar em nossa camada de apresentação. No [próximo tutorial](master-pages-and-site-navigation-cs.md) Vamos tomar um breve desvio de tópicos de acesso de dados e definir um layout de página consistente para uso em toda os tutoriais.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Liz Shulok, Dennis Patterson, Carlos Santos e Hilton Giesenow. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-a-data-access-layer-cs.md)
> [Próximo](master-pages-and-site-navigation-cs.md)
