---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
title: Lote de atualização (c#) | Microsoft Docs
author: rick-anderson
description: Saiba como atualizar vários registros de banco de dados em uma única operação. Na camada de Interface do usuário, criamos um GridView onde cada linha é editável. Nos dados...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: 4e849bcc-c557-4bc3-937e-f7453ee87265
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-cs
msc.type: authoredcontent
ms.openlocfilehash: 9f1bad4f0b58175a8437ebfedf161db057bb2bd2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="batch-updating-c"></a>Lote de atualização (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_CS.zip) ou [baixar PDF](batch-updating-cs/_static/datatutorial64cs1.pdf)

> Saiba como atualizar vários registros de banco de dados em uma única operação. Na camada de Interface do usuário, criamos um GridView onde cada linha é editável. Na camada de acesso a dados encerrar as várias operações de atualização em uma transação para garantir que todas as atualizações de êxito ou todas as atualizações são revertidas.


## <a name="introduction"></a>Introdução

No [tutorial anterior](wrapping-database-modifications-within-a-transaction-cs.md) , vimos como estender a camada de acesso a dados para adicionar suporte para transações de banco de dados. Transações de banco de dados garantem que uma série de instruções de modificação de dados será tratada como uma operação atômica, que garante que todas as modificações apresentarão falha ou tudo será bem-sucedida. Com essa funcionalidade DAL baixo nível do caminho, podemos re pronto para voltar nossa atenção para a criação de interfaces de modificação de dados de lote.

Neste tutorial, criaremos um GridView onde cada linha é editável (veja a Figura 1). Como cada linha é renderizada em sua interface de edição, há s sem a necessidade de uma coluna de editar, atualizar e botões de cancelamento. Em vez disso, há dois botões atualizar produtos na página que, quando clicado, enumerar as linhas de GridView e atualizar o banco de dados.


[![Cada linha em GridView é editável](batch-updating-cs/_static/image1.gif)](batch-updating-cs/_static/image1.png)

**Figura 1**: cada linha em GridView é editável ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image2.png))


Permitir que o s começar!

> [!NOTE]
> No [executando as atualizações em lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) tutorial criamos uma edição de lote interface usando o controle DataList. Este tutorial difere do anterior estiver em que usa um controle GridView e a atualização em lotes é executada dentro do escopo de uma transação. Após concluir este tutorial recomendo para retornar ao tutorial anterior e atualizá-lo para usar a funcionalidade de relacionadas à transação de banco de dados adicionada no tutorial anterior.


## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Examinando as etapas para fazer todas as linhas de GridView editável

Como discutido o [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, GridView oferece suporte interno para editar seus dados subjacentes em uma base por linha. Internamente, o GridView notas qual linha é editável por meio de seu [ `EditIndex` propriedade](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). Como o GridView está sendo associada à sua fonte de dados, ele verifica cada linha para ver se o índice da linha é igual ao valor de `EditIndex`. Nesse caso, essa linha s campos são renderizados usando a edição de suas interfaces. Para BoundFields, a interface de edição é uma caixa de texto cuja `Text` propriedade é atribuída o valor do campo de dados especificado para o s BoundField `DataField` propriedade. Para TemplateFields, o `EditItemTemplate` é usado em vez do `ItemTemplate`.

Lembre-se de que o fluxo de trabalho de edição é iniciado quando um usuário clica em um botão de edição de linha s. Isso causa um postback, define o GridView s `EditIndex` propriedade para o índice da linha clicada s e reassociações os dados à grade. Quando um botão de cancelamento de linha s é clicado, em um postback o `EditIndex` é definido como um valor de `-1` antes de associar novamente os dados à grade. Como as linhas de s GridView iniciar indexação em zero, definindo `EditIndex` para `-1` tem o efeito de exibir o GridView no modo somente leitura.

O `EditIndex` propriedade funciona bem para edição por linha, mas não foi projetada para edição de lote. Para tornar editável a GridView inteira, precisamos ter cada linha renderizar usando sua interface de edição. A maneira mais fácil de fazer isso é criar onde cada campo editável é implementado como um TemplateField com sua interface de edição definido no `ItemTemplate`.

Sobre a próxima várias etapas, criaremos um GridView totalmente editável. Na etapa 1 vamos começar criando GridView e seu ObjectDataSource e converter seus BoundFields e CheckBoxField TemplateFields. Nas etapas 2 e 3, veremos as interfaces de edição do TemplateFields `EditItemTemplate` s para seus `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Etapa 1: Exibindo informações de produto

Antes que se preocupar sobre a criação de um controle GridView onde estão as linhas é editável, permitem que o s iniciar simplesmente exibindo as informações de produto. Abra o `BatchUpdate.aspx` página o `BatchData` pasta e arraste um controle GridView da caixa de ferramentas para o Designer. Definir o GridView s `ID` para `ProductsGrid` e, na sua marca inteligente, escolha para associá-lo para um novo ObjectDataSource denominado `ProductsDataSource`. Configurar o ObjectDataSource para recuperar seus dados a partir de `ProductsBLL` classe s `GetProducts` método.


[![Configurar o ObjectDataSource para usar a classe ProductsBLL](batch-updating-cs/_static/image2.gif)](batch-updating-cs/_static/image3.png)

**Figura 2**: configurar o ObjectDataSource para usar o `ProductsBLL` classe ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image4.png))


[![Recuperar os dados de produto usando o método GetProducts](batch-updating-cs/_static/image3.gif)](batch-updating-cs/_static/image5.png)

**Figura 3**: recuperar os dados de produto usando o `GetProducts` método ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image6.png))


Como o GridView, os recursos de modificação de s ObjectDataSource são projetados para funcionar em uma base por linha. Para atualizar um conjunto de registros, é necessário escrever um pouco de código na classe ASP.NET s de página por trás do código que processa em lotes de dados e a transmite a BLL. Portanto, defina a lista suspensa em ObjectDataSource s guias UPDATE, INSERT e DELETE para (nenhum). Clique em Concluir para concluir o assistente.


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](batch-updating-cs/_static/image4.gif)](batch-updating-cs/_static/image7.png)

**Figura 4**: definir a lista suspensa na atualização, inserir e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image8.png))


Depois de concluir o Assistente Configurar fonte de dados, a marcação declarativa de s ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](batch-updating-cs/samples/sample1.aspx)]

Concluindo o Assistente Configurar fonte de dados também faz com que o Visual Studio para criar BoundFields e um CheckBoxField para os campos de dados de produto em GridView. Para este tutorial, permitem s apenas permitir que o usuário exibir e editar a s nome do produto, categoria, preço e status descontinuados. Remover tudo, exceto o `ProductName`, `CategoryName`, `UnitPrice`, e `Discontinued` campos e renomear o `HeaderText` propriedades dos três primeiros campos ao produto, categoria e preço, respectivamente. Por fim, verifique se as caixas de seleção Habilitar paginação e habilitar a classificação da marca inteligente de s GridView.

Neste ponto, o GridView tem três BoundFields (`ProductName`, `CategoryName`, e `UnitPrice`) e um CheckBoxField (`Discontinued`). Precisamos converter esses quatro campos TemplateFields e, em seguida, mova a interface de edição de s TemplateField `EditItemTemplate` para seu `ItemTemplate`.

> [!NOTE]
> Podemos explorados criando e personalizando TemplateFields no [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial. Vamos examinar as etapas de conversão de CheckBoxField e BoundFields em TemplateFields e definindo a edição de suas interfaces em seus `ItemTemplate` s, mas se você tiver problemas ou precisa de uma atualização, Anibal t hesitam fazem referência a este tutorial anterior.


De GridView s marca inteligente, clique no link de colunas Editar para abrir a caixa de diálogo de campos. Em seguida, selecione cada campo e clique em converter este campo em um TemplateField link.


![Converter a BoundFields existente e CheckBoxField em TemplateField](batch-updating-cs/_static/image5.gif)

**Figura 5**: converter a BoundFields existente e CheckBoxField em TemplateField


Agora que cada campo é um TemplateField, podemos re pronto para mover a edição da interface do `EditItemTemplate` s para o `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Etapa 2: Criando o`ProductName`,`UnitPrice`, e`Discontinued`edição Interfaces

Criando o `ProductName`, `UnitPrice`, e `Discontinued` edição interfaces são o tópico desta etapa e são muito simples, como cada interface já está definido em TemplateField s `EditItemTemplate`. Criando o `CategoryName` Editar interface é um pouco mais envolvida, já que precisamos criar DropDownList as categorias aplicáveis. Isso `CategoryName` interface de edição é resolvido na etapa 3.

Permitir que o s começar com o `ProductName` TemplateField. Clique no link Editar modelos de marca inteligente GridView s e fazer drill down até o `ProductName` TemplateField s `EditItemTemplate`. Selecione a caixa de texto, copie-o para a área de transferência e, em seguida, cole-o `ProductName` TemplateField s `ItemTemplate`. Alterar a caixa de texto s `ID` propriedade `ProductName`.

Em seguida, adicione um RequiredFieldValidator para o `ItemTemplate` para garantir que o usuário fornece um valor para cada nome de produto s. Definir o `ControlToValidate` propriedade ProductName, o `ErrorMessage` propriedade a você deve fornecer o nome do produto. e o `Text` propriedade \*. Depois de fazer essas adições para o `ItemTemplate`, sua tela deve ser semelhante à Figura 6.


[![O ProductName TemplateField agora inclui uma caixa de texto e um RequiredFieldValidator](batch-updating-cs/_static/image6.gif)](batch-updating-cs/_static/image9.png)

**Figura 6**: O `ProductName` TemplateField agora inclui uma caixa de texto e uma RequiredFieldValidator ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image10.png))


Para o `UnitPrice` edição de interface, iniciada por meio da cópia da caixa de texto de `EditItemTemplate` para o `ItemTemplate`. Em seguida, coloque um $ na frente de caixa de texto e defina seu `ID` propriedade UnitPrice e sua `Columns` propriedade para 8.

Também adicionar uma CompareValidator para o `UnitPrice` s `ItemTemplate` para garantir que o valor inserido pelo usuário é um valor de moeda válido maior ou igual a $0,00. Definir o validador s `ControlToValidate` propriedade UnitPrice, seu `ErrorMessage` propriedade a você deve inserir um valor de moeda válidos. . Omitir qualquer moeda símbolos., seu `Text` propriedade \*, sua `Type` propriedade para `Currency`, sua `Operator` propriedade `GreaterThanEqual`e sua `ValueToCompare` propriedade como 0.


[![Adicionar um CompareValidator para garantir que o preço inserido é um valor de moeda não negativo](batch-updating-cs/_static/image7.gif)](batch-updating-cs/_static/image11.png)

**Figura 7**: adicionar um CompareValidator para garantir que o preço inserido é um valor de moeda não negativo ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image12.png))


Para o `Discontinued` TemplateField, você pode usar a caixa de seleção já definida o `ItemTemplate`. Basta definir seu `ID` para descontinuado e seu `Enabled` propriedade `true`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Etapa 3: Criando o`CategoryName`Interface de edição

A interface de edição no `CategoryName` TemplateField s `EditItemTemplate` contém uma caixa de texto que exibe o valor da `CategoryName` campo de dados. É preciso substituir pelo DropDownList que lista as categorias possíveis.

> [!NOTE]
> O [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial contém uma discussão mais completa e completa sobre como personalizar um modelo para incluir DropDownList em vez de uma caixa de texto. Enquanto as etapas forem concluídas, são apresentados tersely. Para obter uma visão mais detalhada de criando e configurando as categorias DropDownList, consultar o [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial.


Arraste DropDownList da caixa de ferramentas para a `CategoryName` TemplateField s `ItemTemplate`, configuração seu `ID` para `Categories`. Neste ponto é geralmente definirá a fonte de dados s DropDownLists por meio de sua marca inteligente, criando um novo ObjectDataSource. No entanto, isso adicionará o ObjectDataSource dentro de `ItemTemplate`, que resultará em uma instância de ObjectDataSource criada para cada linha em GridView. Em vez disso, permitem s criar ObjectDataSource fora o s GridView TemplateFields. Finalizar a edição do modelo e arraste um ObjectDataSource da caixa de ferramentas para o Designer sob o `ProductsDataSource` ObjectDataSource. Nomeie o novo ObjectDataSource `CategoriesDataSource` e configurá-lo para usar o `CategoriesBLL` classe s `GetCategories` método.


[![Configurar o ObjectDataSource para usar o CategoriesBLL Clas](batch-updating-cs/_static/image8.gif)](batch-updating-cs/_static/image13.png)

**Figura 8**: configurar o ObjectDataSource para usar o `CategoriesBLL` Clas ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image14.png))


[![Recuperar os dados de categoria usando o método GetCategories](batch-updating-cs/_static/image9.gif)](batch-updating-cs/_static/image15.png)

**Figura 9**: recuperar os dados de categoria usando o `GetCategories` método ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image16.png))


Como esse ObjectDataSource é usado apenas para recuperar dados, defina as listas suspensas nas guias de UPDATE e DELETE para (nenhum). Clique em Concluir para concluir o assistente.


[![Conjunto de listas suspensas na atualização e excluir guias como (nenhum)](batch-updating-cs/_static/image10.gif)](batch-updating-cs/_static/image17.png)

**Figura 10**: definir a lista suspensa na atualização e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image18.png))


Após concluir o assistente, o `CategoriesDataSource` declarativo s deve ser semelhante ao seguinte:


[!code-aspx[Main](batch-updating-cs/samples/sample2.aspx)]

Com o `CategoriesDataSource` criado e configurado, retornar para o `CategoryName` TemplateField s `ItemTemplate` e, de DropDownList s marca inteligente, clique no link Escolher fonte de dados. No Assistente de configuração de fonte de dados, selecione o `CategoriesDataSource` opção na primeira lista suspensa e optar por ter `CategoryName` usado para a exibição e `CategoryID` como o valor.


[![Associar o CategoriesDataSource DropDownList](batch-updating-cs/_static/image11.gif)](batch-updating-cs/_static/image19.png)

**Figura 11**: associar DropDownList para o `CategoriesDataSource` ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image20.png))


Neste ponto o `Categories` DropDownList lista todas as categorias, mas ele não ainda automaticamente Selecione a categoria apropriada para o produto associado à linha GridView. Para fazer isso, precisamos definir o `Categories` DropDownList s `SelectedValue` para o produto s `CategoryID` valor. Clique no link Editar DataBindings de marca inteligente s DropDownList e associar o `SelectedValue` propriedade com o `CategoryID` campo de dados, conforme mostrado na Figura 12.


![Associar o valor de CategoryID produto s à propriedade SelectedValue DropDownList s](batch-updating-cs/_static/image12.gif)

**Figura 12**: associar o produto s `CategoryID` valor para o s DropDownList `SelectedValue` propriedade


Uma última permanece do problema: se o produto t tiver um `CategoryID` especificado um valor, em seguida, a instrução de associação de dados no `SelectedValue` resultará em uma exceção. Isso ocorre porque o DropDownList contém apenas os itens para as categorias e não oferece uma opção para os produtos que têm uma `NULL` banco de dados do valor para `CategoryID`. Para corrigir este problema, defina o s DropDownList `AppendDataBoundItems` propriedade `true` e adicionar um novo item ao DropDownList, omitindo o `Value` propriedade da sintaxe declarativa. Ou seja, certifique-se de que o `Categories` sintaxe declarativa de s DropDownList é semelhante ao seguinte:


[!code-aspx[Main](batch-updating-cs/samples/sample3.aspx)]

Observe como o `<asp:ListItem Value="">` – selecione um – tem seu `Value` atributo definido explicitamente como uma cadeia de caracteres vazia. Voltar para o [Personalizando a Interface de modificação de dados](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md) tutorial para uma discussão mais completa sobre por que esse item DropDownList adicional é necessária para lidar com o `NULL` caso e por que atribuição do `Value` propriedade como uma cadeia de caracteres vazia é essencial.

> [!NOTE]
> Há um desempenho e escalabilidade problema potencial aqui que vale a pena mencionar. Como cada linha tem DropDownList que usa o `CategoriesDataSource` como sua fonte de dados, o `CategoriesBLL` classe s `GetCategories` método será chamado *n* visitarem vezes por página, onde *n* é o número de linhas em GridView. Essas *n* chamadas para `GetCategories` resultar em *n* consultas no banco de dados. Esse impacto no banco de dados pode ser reduzido armazenando em cache as categorias retornadas em um cache por solicitação ou por meio da camada de cache usando uma dependência ou uma expiração muito com base em curto período de tempo de cache de SQL. Para obter mais informações sobre a por solicitação cache opção, consulte [ `HttpContext.Items` um repositório de Cache por solicitação](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).


## <a name="step-4-completing-the-editing-interface"></a>Etapa 4: Concluindo a Interface de edição

Podemos ve feitas algumas alterações nos modelos de s GridView sem pausar para exibir o nosso progresso. Dedicar um tempo para exibir nosso andamento por meio de um navegador. Como mostra a Figura 13, cada linha é processada usando seu `ItemTemplate`, que contém o s célula interface de edição.


[![Cada linha GridView é editável](batch-updating-cs/_static/image13.gif)](batch-updating-cs/_static/image21.png)

**Figura 13**: cada linha GridView é editável ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image22.png))


Há alguns problemas de formatação secundários devem cuidar de neste momento. Primeiro, observe que o `UnitPrice` valor contém quatro pontos decimais. Para corrigir isso, retornar o `UnitPrice` TemplateField s `ItemTemplate` e, na caixa de texto s marca inteligente, clique no link Editar DataBindings. Em seguida, especifique que o `Text` propriedade deve ser formatada como um número.


![Formatar a propriedade de texto como um número](batch-updating-cs/_static/image14.gif)

**Figura 14**: formato de `Text` propriedade como um número


Em segundo lugar, permitem s centralizar a caixa de seleção de `Discontinued` coluna (em vez de tê-la alinhado à esquerda). Clique em Editar colunas de marca inteligente GridView s e selecione o `Discontinued` TemplateField da lista de campos no canto inferior esquerdo. Detalhar `ItemStyle` e defina o `HorizontalAlign` propriedade Center, conforme mostrado na Figura 15.


![Centralize a caixa de seleção descontinuada](batch-updating-cs/_static/image15.gif)

**Figura 15**: Center o `Discontinued` caixa de seleção


Em seguida, adicione um controle ValidationSummary para a página e defina seu `ShowMessageBox` propriedade `true` e sua `ShowSummary` propriedade `false`. Também adicione que controles de botão Web que, quando clicado, será atualizado as alterações do usuário s. Especificamente, adicione dois controles da Web de botão, uma acima GridView e abaixo dela, definir ambos os controles `Text` propriedades para produtos de atualização.

Desde o s GridView edição interface é definida no seu TemplateFields `ItemTemplate` s, o `EditItemTemplate` s são supérfluos e pode ser excluído.

Depois de fazer as opções acima mencionadas alterações de formatação, adicionar os controles de botão e removendo o desnecessária `EditItemTemplate` s, sua sintaxe declarativa de página s deve parecer com o seguinte:


[!code-aspx[Main](batch-updating-cs/samples/sample4.aspx)]

A Figura 16 mostra essa página quando visualizada através de um navegador depois que os controles da Web de botão foram adicionados e as alterações de formatação feitas.


[![A página agora inclui dois botões de produtos de atualização](batch-updating-cs/_static/image16.gif)](batch-updating-cs/_static/image23.png)

**Figura 16**: A página agora inclui dois botões atualizar dos produtos ([clique para exibir a imagem em tamanho normal](batch-updating-cs/_static/image24.png))


## <a name="step-5-updating-the-products"></a>Etapa 5: Atualizando os produtos

Quando um usuário visita esta página eles fazer suas modificações e, em seguida, clique em um dos dois botões de produtos de atualização. Nesse ponto, precisamos salvar alguma forma os valores inseridos pelo usuário para cada linha em uma `ProductsDataTable` instância e passá-lo para um método BLL que, em seguida, passará que `ProductsDataTable` instância para o s DAL `UpdateWithTransaction` método. O `UpdateWithTransaction` método, que criamos no [tutorial anterior](wrapping-database-modifications-within-a-transaction-cs.md), garante que o lote de alterações será atualizado como uma operação atômica.

Crie um método chamado `BatchUpdate` em `BatchUpdate.aspx.cs` e adicione o seguinte código:


[!code-csharp[Main](batch-updating-cs/samples/sample5.cs)]

Este método começa obtendo todos os produtos em um `ProductsDataTable` por meio de uma chamada para o s BLL `GetProducts` método. Em seguida, enumera o `ProductGrid` GridView s [ `Rows` coleção](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). O `Rows` coleção contém um [ `GridViewRow` instância](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) para cada linha exibida em GridView. Já que estamos mostrando no máximo dez linhas por página, o s GridView `Rows` coleção será ter não mais do que 10 itens.

Para cada linha de `ProductID` é capturado do `DataKeys` coleta e apropriada `ProductsRow` é selecionado do `ProductsDataTable`. Os controles de entrada TemplateField quatro programaticamente são referenciados e seus valores atribuídos para a `ProductsRow` s propriedades da instância. Depois de cada GridView linha s valores foram usados para atualizar o `ProductsDataTable`, ele s passado para o s BLL `UpdateWithTransaction` método que, conforme vimos no tutorial anterior, simplesmente chama para baixo em s DAL `UpdateWithTransaction` método.

O algoritmo de atualização do lote usado para este tutorial atualiza cada linha de `ProductsDataTable` que corresponde a uma linha em GridView, independentemente se as informações de produto s foi alteradas. Enquanto esse blind atualiza t são geralmente um problema de desempenho, elas podem gerar registros supérfluos se re auditoria as alterações na tabela de banco de dados. Volta o [executando as atualizações em lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) tutorial, vamos explorou um lote atualizando a interface com o controle DataList e adicionado código que atualizará apenas os registros que realmente foram modificados pelo usuário. Fique à vontade para usar as técnicas de [executando as atualizações em lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs.md) para atualizar o código neste tutorial, se desejado.

> [!NOTE]
> Ao associar a fonte de dados a GridView por meio de sua marca inteligente, o Visual Studio automaticamente atribui os valores de chave primária do fonte s dados para o s GridView `DataKeyNames` propriedade. Se você não se associaram ObjectDataSource GridView por meio de marca inteligente GridView s conforme descrito na etapa 1, você precisará definir manualmente a s GridView `DataKeyNames` propriedade ProductID para acessar o `ProductID` valor para cada linha por meio de `DataKeys` coleção.


O código usado na `BatchUpdate` é semelhante àquela usada em s BLL `UpdateProduct` métodos, a principal diferença é que o `UpdateProduct` métodos um único `ProductRow` instância é recuperada da arquitetura. O código que atribui as propriedades do `ProductRow` é o mesmo entre o `UpdateProducts` métodos e o código dentro de `foreach` loop na `BatchUpdate`, como é o padrão geral.

Para concluir este tutorial, é preciso ter o `BatchUpdate` método invocado quando qualquer um dos botões de produtos de atualização é clicado. Criar manipuladores de eventos para o `Click` eventos desses dois controles de botão e adicione o seguinte código nos manipuladores de eventos:


[!code-csharp[Main](batch-updating-cs/samples/sample6.cs)]

Primeiro é feita uma chamada para `BatchUpdate`. Em seguida, o `ClientScript property` é usado para injetar JavaScript que exibirá uma caixa de mensagem que lê os produtos foram atualizados.

Reserve um minuto para testar esse código. Visite `BatchUpdate.aspx` através de um navegador, um número de linhas de editar e clique em um dos botões de produtos de atualização. Supondo que não há nenhum erro de validação de entrada, você deve ver uma caixa de mensagem que lê que os produtos foram atualizados. Para verificar a atomicidade da atualização, considere adicionar um aleatório `CHECK` restrição, como um que não permite `UnitPrice` valores de 1234,56. Depois de `BatchUpdate.aspx`, editar um número de registros, certificando-se de definir um produto s `UnitPrice` valor para o valor proibido (1234,56). Isso deve resultar em um erro quando clicar em produtos de atualização com as outras alterações durante a operação em lote revertidas para seus valores originais.

## <a name="an-alternativebatchupdatemethod"></a>Uma alternativa`BatchUpdate`método

O `BatchUpdate` método simplesmente examinadas recupera *todos os* dos produtos de s BLL `GetProducts` método e, em seguida, atualiza apenas os registros que aparecem em GridView. Essa abordagem é ideal se GridView não usar a paginação, mas, em caso afirmativo, pode haver centenas, milhares ou dezenas de milhares de produtos, mas apenas dez linhas em GridView. Nesse caso, obter todos os produtos do banco de dados somente para modificar 10 delas é inferior ao ideal.

Para esses tipos de situações, considere usar o seguinte `BatchUpdateAlternate` método em vez disso:


[!code-csharp[Main](batch-updating-cs/samples/sample7.cs)]

`BatchMethodAlternate` inicia criando uma nova vazia `ProductsDataTable` chamado `products`. Em seguida, percorre o GridView s `Rows` coleção e, para cada linha obtém as informações de produto específico usando o s BLL `GetProductByProductID(productID)` método. Recuperada `ProductsRow` instância tem suas propriedades atualizadas da mesma maneira como `BatchUpdate`, mas depois de atualizar a linha será importado para o `products``ProductsDataTable` por meio de DataTable s [ `ImportRow(DataRow)` método](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Após o `foreach` loop estiver concluído, `products` contém um `ProductsRow` instância para cada linha em GridView. Desde que cada um do `ProductsRow` instâncias foram adicionadas para o `products` (em vez de atualizado), se cegamente de passá-lo para o `UpdateWithTransaction` método o `ProductsTableAdatper` tentará inserir cada um dos registros no banco de dados. Em vez disso, é necessário especificar que cada uma dessas linhas foi modificada (não adicionado).

Isso pode ser feito adicionando um novo método para BLL denominado `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, conforme mostrados abaixo, conjuntos a `RowState` de cada uma da `ProductsRow` instâncias o `ProductsDataTable` para `Modified` e, em seguida, passa o `ProductsDataTable` no s DAL `UpdateWithTransaction` método.


[!code-csharp[Main](batch-updating-cs/samples/sample8.cs)]

## <a name="summary"></a>Resumo

GridView fornece recursos de edição de linha de internos, mas não tem suporte para a criação de interfaces totalmente editáveis. Como vimos neste tutorial, essas interfaces são possíveis, mas exigem um pouco de trabalho. Para criar um GridView onde cada linha é editável, precisamos converter os campos de s GridView TemplateFields e definir a interface de edição dentro de `ItemTemplate` s. Além disso, atualize todos os - tipo de controles da Web do botão deve ser adicionado à página, separada do GridView. Esses botões `Click` precisam de manipuladores de eventos enumerar os s GridView `Rows` coleção, armazenar as alterações em um `ProductsDataTable`e passar as informações atualizadas para o método BLL apropriado.

O tutorial Avançar veremos como criar uma interface para exclusão em lotes. Em particular, cada linha GridView será incluem uma caixa de seleção e, em vez de atualizar todos os-digite botões, teremos botões excluir linhas selecionadas.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Teresa Murphy e David Suru. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](wrapping-database-modifications-within-a-transaction-cs.md)
> [Próximo](batch-deleting-cs.md)
