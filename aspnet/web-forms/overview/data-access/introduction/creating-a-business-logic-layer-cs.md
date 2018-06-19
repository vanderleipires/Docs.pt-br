---
uid: web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
title: Criando uma camada de lógica de negócios (c#) | Microsoft Docs
author: rick-anderson
description: Neste tutorial, veremos como centralizar as regras de negócio em um negócio lógica BLL (camada) que serve como um intermediário para troca de dados entre t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 85554606-47cb-4e4f-9848-eed9da579056
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-business-logic-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 6e73e9e68e4abb0d382baa7da925c167809e417a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886934"
---
<a name="creating-a-business-logic-layer-c"></a>Criando uma camada de lógica de negócios (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixe o aplicativo de exemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_2_CS.exe) ou [baixar PDF](creating-a-business-logic-layer-cs/_static/datatutorial02cs1.pdf)

> Neste tutorial, veremos como centralizar as regras de negócio em um negócio lógica BLL (camada) que serve como um intermediário para troca de dados entre a camada de apresentação e a DAL.


## <a name="introduction"></a>Introdução

A camada de acesso de dados (DAL) criado na [primeiro tutorial](creating-a-data-access-layer-cs.md) separa corretamente os dados de acessar a lógica da lógica de apresentação. No entanto, enquanto a DAL separa claramente os detalhes de acesso de dados da camada de apresentação, ele não impõe quaisquer regras de negócios que podem ser aplicadas. Por exemplo, para nosso aplicativo podemos querer impedir o `CategoryID` ou `SupplierID` campos do `Products` tabela a ser modificada quando o `Discontinued` campo é definido como 1, ou podem ser impor regras de precedência, proibindo situações nas quais um funcionário é gerenciado por alguém que foi contratado depois deles. Outro cenário comum é talvez somente os usuários de autorização em uma função específica podem excluir produtos ou podem alterar o `UnitPrice` valor.

Neste tutorial, veremos como centralizar essas regras de negócio em um negócio lógica BLL (camada) que serve como um intermediário para troca de dados entre a camada de apresentação e a DAL. Em um aplicativo do mundo real, BLL deve ser implementada como um projeto de biblioteca de classes separado; No entanto, para esses tutoriais implementaremos BLL como uma série de classes em nosso `App_Code` pasta para simplificar a estrutura do projeto. A Figura 1 ilustra as relações de arquitetura entre a camada de apresentação, BLL e DAL.


![BLL separa a camada de apresentação da camada de acesso a dados e impõe regras de negócio](creating-a-business-logic-layer-cs/_static/image1.png)

**Figura 1**: BLL separa a camada de apresentação da camada de acesso a dados e impõe regras de negócio


## <a name="step-1-creating-the-bll-classes"></a>Etapa 1: Criando Classes BLL

Nosso BLL será ser composto de quatro classes, uma para cada TableAdapter no DAL; cada uma dessas classes BLL terá métodos para recuperar, inserindo, atualizando e excluindo do respectivo TableAdapter no DAL, aplicando as regras de negócio apropriado.

Para mais corretamente, separar as classes relacionadas a DAL e BLL, vamos criar duas subpastas no `App_Code` pasta `DAL` e `BLL`. Simplesmente clique duas vezes no `App_Code` pasta no Gerenciador de soluções e escolha a nova pasta. Depois de criar essas duas pastas, mover o conjunto de dados tipado criado no primeiro tutorial para o `DAL` subpasta.

Em seguida, crie os quatro arquivos de classe BLL no `BLL` subpasta. Para fazer isso, clique duas vezes no `BLL` subpasta, escolha Adicionar um novo Item e escolha o modelo de classe. Nome de quatro classes `ProductsBLL`, `CategoriesBLL`, `SuppliersBLL`, e `EmployeesBLL`.


![Adicionar quatro novas Classes para a pasta App_Code](creating-a-business-logic-layer-cs/_static/image2.png)

**Figura 2**: adicionar quatro novas Classes para o `App_Code` pasta


Em seguida, vamos adicionar métodos para cada uma das classes para incluir apenas os métodos definidos para os TableAdapters do primeiro tutorial. Agora, esses métodos apenas chamará diretamente a DAL; retornaremos posteriormente para adicionar qualquer lógica de negócios necessários.

> [!NOTE]
> Se você estiver usando o Visual Studio Standard Edition ou superior (ou seja, você está *não* usando o Visual Web Developer), opcionalmente, você pode criar suas classes visualmente usando o [Designer de classe](https://msdn.microsoft.com/library/default.asp?url=/library/dv_vstechart/html/clssdsgnr.asp). Consulte o [Blog do Designer de classe](https://blogs.msdn.com/classdesigner/default.aspx) para obter mais informações sobre esse novo recurso no Visual Studio.


Para o `ProductsBLL` que precisamos adicionar um total de sete métodos de classe:

- `GetProducts()` Retorna todos os produtos
- `GetProductByProductID(productID)` Retorna o produto com a identificação de produto especificada
- `GetProductsByCategoryID(categoryID)` Retorna todos os produtos na categoria especificada
- `GetProductsBySupplier(supplierID)` Retorna todos os produtos do fornecedor especificado
- `AddProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued)` Insere um novo produto no banco de dados usando os valores passados-in; Retorna o `ProductID` valor do registro recém-inserido
- `UpdateProduct(productName, supplierID, categoryID, quantityPerUnit, unitPrice, unitsInStock, unitsOnOrder, reorderLevel, discontinued, productID)` atualiza um produto existente no banco de dados usando os valores passados. Retorna `true` se exatamente uma linha foi atualizada, `false` contrário
- `DeleteProduct(productID)` Exclui o produto especificado do banco de dados

ProductsBLL.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample1.cs)]

Os métodos que simplesmente retornam dados `GetProducts`, `GetProductByProductID`, `GetProductsByCategoryID`, e `GetProductBySuppliersID` são bem simples, como eles simplesmente chamam para baixo para a DAL. Embora em alguns cenários pode haver regras de negócios que precisam ser implementados nesse nível (por exemplo, regras de autorização com base no usuário conectado no momento ou a função à qual o usuário pertence), vamos simplesmente deixar esses métodos como-é. Para esses métodos, em seguida, BLL serve simplesmente como um proxy por meio do qual a camada de apresentação acessa os dados subjacentes de camada de acesso a dados.

O `AddProduct` e `UpdateProduct` métodos tanto levar em como parâmetros de valores para os vários campos de produto e adicionar um novo produto ou atualizar uma existente, respectivamente. Já que muitas o `Product` colunas da tabela podem aceitar `NULL` valores (`CategoryID`, `SupplierID`, e `UnitPrice`, entre outros), os parâmetros de entrada `AddProduct` e `UpdateProduct` que são mapeados para esse uso de colunas [tipos anuláveis](https://msdn.microsoft.com/library/1t3y8s4s(v=vs.80).aspx). Tipos anuláveis são novos para o .NET 2.0 e fornecer uma técnica para que indica se um tipo de valor deve, em vez disso, ser `null`. No c# pode sinalizar um tipo de valor como um tipo anulável adicionando `?` após o tipo (como `int? x;`). Consulte o [tipos anuláveis](https://msdn.microsoft.com/library/1t3y8s4s.aspx) seção o [guia de programação em c#](https://msdn.microsoft.com/library/67ef8sbd%28VS.80%29.aspx) para obter mais informações.

Todos os três métodos retornam um valor booliano que indica se uma linha foi inserida, atualizada ou excluída desde que a operação não pode resultar em uma linha afetada. Por exemplo, se o desenvolvedor de página chama `DeleteProduct` passando um `ProductID` para um produto inexistente, o `DELETE` instrução emitida para o banco de dados não terá nenhum efeito e, portanto, o `DeleteProduct` método retornará `false`.

Observe que ao adicionar um novo produto ou atualizando uma existente faremos em valores de campo do produto novos ou modificados como uma lista de escalares em vez de aceitar um `ProductsRow` instância. Essa abordagem foi escolhida porque a `ProductsRow` classe deriva ADO.NET `DataRow` classe, que não tem um construtor sem parâmetros padrão. Para criar um novo `ProductsRow` instância, é necessário criar primeiro uma `ProductsDataTable` de instância e, em seguida, invocar seus `NewProductRow()` método (que fazemos em `AddProduct`). Este empecilho rears seu cabeçalho quando desativamos para inserir e atualizar produtos usando o ObjectDataSource. Em resumo, o ObjectDataSource tentará criar uma instância dos parâmetros de entrada. Se o método BLL espera um `ProductsRow` instância, ObjectDataSource tentará criá-lo, mas falhar devido à falta de um construtor sem parâmetros padrão. Para obter mais informações sobre esse problema, consulte as seguintes duas postagens de fóruns do ASP.NET: [ObjectDataSources atualizando com conjuntos de dados fortemente definida](https://forums.asp.net/1098630/ShowPost.aspx), e [problema com ObjectDataSource e conjunto de dados fortemente definida](https://forums.asp.net/1048212/ShowPost.aspx).

Em seguida, em ambos os `AddProduct` e `UpdateProduct`, o código cria um `ProductsRow` de instância e o preenche com os valores passados apenas no. Ao atribuir valores às colunas de dados de um DataRow podem ocorrer várias verificações de validação de nível de campo. Portanto, manualmente colocar os valores passados de volta em um DataRow ajuda a garantir a validade dos dados que está sendo passados para o método BLL. Infelizmente as classes de DataRow fortemente tipado geradas pelo Visual Studio não usam tipos anuláveis. Em vez disso, para indicar que uma coluna de dados específica em um DataRow deve corresponder a um `NULL` banco de dados valor, deve usar o `SetColumnNameNull()` método.

Em `UpdateProduct` carregamos primeiro no produto para atualizar usando `GetProductByProductID(productID)`. Embora isso possa parecer uma viagem desnecessária ao banco de dados, esse processamento extra comprove vale a pena em tutoriais futuros que exploram a simultaneidade otimista. Simultaneidade otimista é uma técnica para garantir que dois usuários que estão trabalhando simultaneamente nos mesmos dados acidentalmente não substitua as alterações uma da outra. Captura de todo o registro também torna mais fácil criar métodos de atualização na BLL que modificam somente um subconjunto de colunas do DataRow. Quando vamos explorar o `SuppliersBLL` veremos um exemplo de classe.

Observe que, por fim, o `ProductsBLL` classe tem o [atributo DataObject](https://msdn.microsoft.com/library/system.componentmodel.dataobjectattribute.aspx) aplicada a ele (a `[System.ComponentModel.DataObject]` sintaxe antes da instrução de classe na parte superior do arquivo) e os métodos têm [ Atributos de DataObjectMethodAttribute](https://msdn.microsoft.com/library/system.componentmodel.dataobjectmethodattribute.aspx). O `DataObject` atributo marcar a classe como sendo um objeto adequado para associação a um [controle ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), enquanto o `DataObjectMethodAttribute` indica a finalidade do método. Como veremos no futuro tutoriais, ObjectDataSource do ASP.NET 2.0 torna fácil declarativamente acessar dados de uma classe. Para ajudar a filtrar a lista de possíveis classes para associar ao Assistente do ObjectDataSource, por padrão, somente as classes marcadas como `DataObjects` são mostrados na lista suspensa do assistente. O `ProductsBLL` classe funcione tão bem sem esses atributos, mas adicioná-los torna mais fácil o trabalho com o Assistente do ObjectDataSource.

## <a name="adding-the-other-classes"></a>Adicionando as outras Classes

Com o `ProductsBLL` classe completa, ainda precisamos adicionar as classes para trabalhar com categorias, fornecedores e funcionários. Reserve um tempo para criar as seguintes classes e métodos com os conceitos do exemplo acima:

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

É o único método vale a pena observar o `SuppliersBLL` da classe `UpdateSupplierAddress` método. Esse método fornece uma interface para atualizar apenas as informações de endereço do fornecedor. Internamente, esse método lê a `SupplierDataRow` objeto especificado `supplierID` (usando `GetSupplierBySupplierID`), define suas propriedades de endereço e, em seguida, chama para baixo o `SupplierDataTable`do `Update` método. O `UpdateSupplierAddress` método segue:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample2.cs)]

Consulte o download deste artigo para minha implementação completa das classes BLL.

## <a name="step-2-accessing-the-typed-datasets-through-the-bll-classes"></a>Etapa 2: Acessando os DataSets tipados pelas Classes BLL

No primeiro tutorial vimos exemplos de como trabalhar diretamente com o conjunto de dados tipado programaticamente, mas com a adição do nosso classes BLL, a camada de apresentação deve funcionar em relação a BLL em vez disso. No `AllProducts.aspx` exemplo do primeiro tutorial, o `ProductsTableAdapter` foi usado para associar a lista de produtos para um GridView, conforme mostrado no código a seguir:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample3.cs)]

Para usar o novo BLL classes, tudo o que precisa ser modificada é a primeira linha do código simplesmente substituir o `ProductsTableAdapter` do objeto com um `ProductBLL` objeto:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample4.cs)]

As classes BLL também podem ser acessadas declarativamente (como o conjunto de dados tipado) usando o ObjectDataSource. Que discutiremos ObjectDataSource mais detalhadamente nos tutoriais a seguir.


[![A lista de produtos é exibida em um GridView](creating-a-business-logic-layer-cs/_static/image4.png)](creating-a-business-logic-layer-cs/_static/image3.png)

**Figura 3**: A lista de produtos é exibida em um controle GridView ([clique para exibir a imagem em tamanho normal](creating-a-business-logic-layer-cs/_static/image5.png))


## <a name="step-3-adding-field-level-validation-to-the-datarow-classes"></a>Etapa 3: Adicionando validação de nível de campo para as Classes de DataRow

Validação de nível de campo são as verificações que pertence aos valores de propriedade dos objetos de negócios ao inserir ou atualizar. Algumas regras de validação de nível de campo para os produtos incluem:

- O `ProductName` campo deve ser 40 caracteres ou menos
- O `QuantityPerUnit` campo deve ser 20 caracteres ou menos
- O `ProductID`, `ProductName`, e `Discontinued` campos são necessários, mas todos os outros campos são opcionais
- O `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` campos devem ser maior que ou igual a zero

Essas regras podem e devem ser expressos no nível do banco de dados. O limite de caracteres no `ProductName` e `QuantityPerUnit` campos são capturados pelos tipos de dados das colunas no `Products` tabela (`nvarchar(40)` e `nvarchar(20)`, respectivamente). Se os campos são obrigatórios e opcionais são expressos por se a coluna de tabela de banco de dados permite `NULL` s. Quatro [restrições de verificação](https://msdn.microsoft.com/library/ms188258.aspx) existe que certifique-se de que somente valores maiores que ou iguais a zero podem fazer no `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, ou `ReorderLevel` colunas.

Além de aplicar essas regras no banco de dados que eles também devem ser impostos no nível do conjunto de dados. Na verdade, o tamanho do campo e se um valor é obrigatório ou opcional já são capturados para o conjunto de cada tabela de dados de colunas de dados. Para ver a validação de nível de campo existente fornecida automaticamente, vá para o Designer de conjunto de dados, selecione uma das tabelas de dados um campo e, em seguida, vá para a janela de propriedades. Como mostra a Figura 4, o `QuantityPerUnit` DataColumn no `ProductsDataTable` tem um comprimento máximo de 20 caracteres e permitir `NULL` valores. Se tentar definir o `ProductsDataRow`do `QuantityPerUnit` propriedade para um valor de cadeia de caracteres mais de 20 caracteres um `ArgumentException` será lançada.


[![DataColumn fornece validação de nível de campo básica](creating-a-business-logic-layer-cs/_static/image7.png)](creating-a-business-logic-layer-cs/_static/image6.png)

**Figura 4**: O DataColumn fornece nível de campo validação básica ([clique para exibir a imagem em tamanho normal](creating-a-business-logic-layer-cs/_static/image8.png))


Infelizmente, não é possível especificamos verificações de limites, como o `UnitPrice` valor deve ser maior que ou igual a zero, usando a janela Propriedades. Para fornecer esse tipo de validação de nível de campo, precisamos criar um manipulador de eventos para o DataTable [ColumnChanging](https://msdn.microsoft.com/library/system.data.datatable.columnchanging%28VS.80%29.aspx) eventos. Conforme mencionado no [tutorial anterior](creating-a-data-access-layer-cs.md), objetos de conjunto de dados, DataTables e DataRow criados pelo conjunto de dados tipado podem ser estendidos com o uso de classes parciais. Usando essa técnica, podemos criar um `ColumnChanging` manipulador de eventos para o `ProductsDataTable` classe. Comece criando uma classe no `App_Code` pasta denominada `ProductsDataTable.ColumnChanging.cs`.


[![Adicionar uma nova classe para a pasta App_Code](creating-a-business-logic-layer-cs/_static/image10.png)](creating-a-business-logic-layer-cs/_static/image9.png)

**Figura 5**: adicionar uma nova classe para o `App_Code` pasta ([clique para exibir a imagem em tamanho normal](creating-a-business-logic-layer-cs/_static/image11.png))


Em seguida, crie um manipulador de eventos para o `ColumnChanging` evento que garante que o `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, e `ReorderLevel` valores de coluna (se não `NULL`) é maior ou igual a zero. Se qualquer coluna desse tipo está fora do intervalo, gerar um `ArgumentException`.

ProductsDataTable.ColumnChanging.cs


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample5.cs)]

## <a name="step-4-adding-custom-business-rules-to-the-blls-classes"></a>Etapa 4: Adicionando regras de negócio personalizadas para Classes do BLL

Além de validação de nível de campo, pode haver regras de negócios personalizada de alto nível que envolvem entidades diferentes ou conceitos não pode ser expressados no nível de coluna única, como:

- Se um produto foi descontinuado, seu `UnitPrice` não pode ser atualizado
- País de um funcionário de residência deve ser o mesmo país do gerente de residência
- Um produto não pode ser descontinuado se é o único produto fornecido pelo fornecedor

As classes BLL devem conter verificações para garantir a conformidade com as regras de negócio do aplicativo. Essas verificações podem ser adicionadas diretamente para os métodos aos quais eles se aplicam.

Imagine que nossas regras de negócios determinarem que um produto não puderam ser marcado Descontinuado se fosse o único produto de um fornecedor específico. Ou seja, se produto *X* foi o único produto adquirimos de fornecedor *Y*, não foi possível marcar *X* como descontinuada; se, no entanto, fornecedor *Y*fornecidos nos com três produtos, *um*, *B*, e *C*, podemos pode marcar qualquer e todos esses como descontinuado. Uma regra de negócio ímpar, mas as regras de negócio e senso sempre não estiverem alinhados!

Para impor essa regra de negócio no `UpdateProducts` método começamos verificando se `Discontinued` foi definida como `true` e, portanto, devemos chamar `GetProductsBySupplierID` para determinar quantos produtos adquirimos de fornecedor deste produto. Se apenas um produto é comprado esse fornecedor, lançamos uma `ApplicationException`.


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample6.cs)]

## <a name="responding-to-validation-errors-in-the-presentation-tier"></a>Respondendo a erros de validação na camada de apresentação

Ao chamar BLL da camada de apresentação, pode decidir se deseja tentar manipular as exceções que podem ser geradas ou permitir que eles vão para o ASP.NET (que irá gerar o `HttpApplication`do `Error` evento). Para manipular uma exceção ao trabalhar programaticamente com BLL, podemos usar um [try... catch](https://msdn.microsoft.com/library/0yd65esw.aspx) bloco, como mostra o exemplo a seguir:


[!code-csharp[Main](creating-a-business-logic-layer-cs/samples/sample7.cs)]

Como veremos em tutoriais futuros, tratamento de exceções do BLL quando usando dados de um controle da Web para inserir, atualizar, ou exclusão de dados pode ser manipulada diretamente de um manipulador de eventos em vez de encapsular o código em `try...catch` blocos.

## <a name="summary"></a>Resumo

Um aplicativo bem projetado é elaborado em camadas distintas, cada uma delas encapsula uma função específica. No primeiro tutorial esta série de artigos, criamos uma camada de acesso a dados usando conjuntos de dados digitados; Neste tutorial que criamos uma camada de lógica de negócios como uma série de classes em nosso aplicativo `App_Code` pasta que chamam nosso DAL. BLL implementa a lógica de nível de campo e o nível de negócios para nosso aplicativo. Além de criar um BLL separado, como fizemos neste tutorial, outra opção é estender métodos os TableAdapters com o uso de classes parciais. No entanto, usar esta técnica não permite a substituir os métodos existentes nem-separar nosso DAL e nossa BLL como perfeitamente a abordagem tomadas neste artigo.

Com a DAL e BLL completa, estamos prontos para iniciar em nossa camada de apresentação. No [tutorial próxima](master-pages-and-site-navigation-cs.md) Vamos tomar um breve desvio de tópicos de acesso de dados e definir um layout de página consistente para uso em toda os tutoriais.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Liz Shulok, Dennis Patterson, Carlos Santos e Giesenow Hilton. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-a-data-access-layer-cs.md)
> [Próximo](master-pages-and-site-navigation-cs.md)
