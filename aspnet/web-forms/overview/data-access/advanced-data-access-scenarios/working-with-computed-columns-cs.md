---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: Trabalhando com colunas computadas (c#) | Microsoft Docs
author: rick-anderson
description: Ao criar uma tabela de banco de dados, o Microsoft SQL Server permite que você defina uma coluna computada, cujo valor é calculado a partir de uma expressão que normalmente referen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: 96a6411fe72e044b8f35091192ecaaac0d376349
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385710"
---
<a name="working-with-computed-columns-c"></a>Trabalhando com colunas computadas (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) ou [baixar PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Ao criar uma tabela de banco de dados, o Microsoft SQL Server permite que você defina uma coluna computada, cujo valor é calculado a partir de uma expressão que geralmente faz referência a outros valores no mesmo registro de banco de dados. Esses valores são somente leitura no banco de dados, que requer consideração especial ao trabalhar com TableAdapters. Neste tutorial, saiba como enfrentar os desafios apresentados por colunas computadas.


## <a name="introduction"></a>Introdução

Microsoft SQL Server permite  *[as colunas computadas](https://msdn.microsoft.com/library/ms191250.aspx)*, que são colunas cujos valores são calculados a partir de uma expressão que geralmente faz referência os valores de outras colunas na mesma tabela. Por exemplo, um modelo de dados de acompanhamento de tempo pode ter uma tabela denominada `ServiceLog` com colunas incluindo `ServicePerformed`, `EmployeeID`, `Rate`, e `Duration`, entre outros. Enquanto o valor devido por serviço de item (que está sendo a taxa multiplicada pela duração) pode ser calculado por meio de uma página da web ou outra interface programática, talvez seja útil para incluir uma coluna na `ServiceLog` tabela chamada `AmountDue` que relataram isso informações. Esta coluna pode ser criada como uma coluna normal, mas ela precisará ser atualizado a qualquer momento a `Rate` ou `Duration` valores da coluna alteradas. Uma abordagem melhor seria tornar a `AmountDue` uma coluna computada usando a expressão de coluna `Rate * Duration`. Isso faria com que o SQL Server calcular automaticamente o `AmountDue` valor da coluna sempre que ele foi referenciado em uma consulta.

Uma vez que um valor de coluna computada s é determinado por uma expressão, essas colunas são somente leitura e, portanto, não é possível ter valores atribuídos a eles no `INSERT` ou `UPDATE` instruções. No entanto, quando as colunas computadas são parte da consulta principal para um TableAdapter que usa instruções SQL ad hoc, eles são incluídos automaticamente no geradas automaticamente `INSERT` e `UPDATE` instruções. Consequentemente, o TableAdapter s `INSERT` e `UPDATE` consultas e `InsertCommand` e `UpdateCommand` propriedades devem ser atualizadas para remover referências a colunas computadas.

Um desafio de usar computado colunas com um TableAdapter que usa instruções SQL ad hoc é que o TableAdapter s `INSERT` e `UPDATE` consultas forem gerados novamente automaticamente sempre que o Assistente de configuração do TableAdapter é concluído. Portanto, as colunas computadas removidos manualmente do `INSERT` e `UPDATE` consultas reaparecerá se executadas novamente o assistente. Embora TableAdapters que usam procedimentos armazenados don t sofrem esse fragilidade, eles têm suas próprias peculiaridades que serão abordados na etapa 3.

Neste tutorial, adicionaremos uma coluna computada para a `Suppliers` de tabela no banco de dados Northwind e, em seguida, crie um TableAdapter correspondente para trabalhar com essa tabela e uma coluna computada. Temos nosso TableAdapter usar procedimentos armazenados, em vez de instruções SQL ad hoc para que nossa t são inválidas de personalizações perdido quando o Assistente de configuração do TableAdapter é usado.

Permitir que o s começar!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Etapa 1: Adicionando uma coluna computada para a`Suppliers`tabela

Banco de dados Northwind não tem qualquer coluna computada, portanto, precisamos adicionar um mesmos. Para este tutorial deixe s adicionar uma coluna computada para a `Suppliers` tabela chamada `FullContactName` que retorna o nome de contato s, título e a empresa que trabalham no seguinte formato: `ContactName` (`ContactTitle`, `CompanyName`). Essa computada poderá ser usada em relatórios ao exibir as informações sobre fornecedores.

Comece abrindo o `Suppliers` definição da tabela clicando com o `Suppliers` de tabela no Gerenciador de servidores e escolhendo abrir definição de tabela no menu de contexto. Isso exibirá as colunas da tabela e suas propriedades, como seu tipo de dados, se eles permitem que `NULL` s e assim por diante. Para adicionar uma coluna computada, comece digitando o nome da coluna para a definição da tabela. Em seguida, insira sua expressão na caixa de texto (fórmula) na seção de especificação de coluna computada na janela Propriedades de coluna (consulte a Figura 1). Nome da coluna computada `FullContactName` e use a seguinte expressão:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Observe que as cadeias de caracteres podem ser concatenadas em SQL usando o `+` operador. O `CASE` instrução pode ser usada como uma condicional em uma linguagem de programação tradicional. Na expressão acima de `CASE` instrução pode ser lido como: se `ContactTitle` não está `NULL` de saída, em seguida, o `ContactTitle` emissão de valor concatenado com uma vírgula, caso contrário, nada. Para obter mais informações sobre a utilidade do `CASE` instrução, consulte [energia de SQL `CASE` instruções](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Em vez de usar um `CASE` instrução aqui, poderíamos ter Alternativamente usado `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) Retorna *checkExpression* se for não nulo, caso contrário, retornará *replacementValue*. Enquanto o `ISNULL` ou `CASE` funcionará nessa instância, há cenários mais complexos em que a flexibilidade da `CASE` instrução não pode ser correspondida por `ISNULL`.


Depois de adicionar a coluna computada sua tela deve ser semelhante a tela na Figura 1.


[![Adicionar uma coluna computada chamada FullContactName à tabela fornecedores](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Figura 1**: Adicione um chamado de coluna computada `FullContactName` para o `Suppliers` tabela ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image3.png))


Depois de nomear a coluna computada e inserir sua expressão, salvar as alterações na tabela clicando no ícone Salvar na barra de ferramentas, pressionando Ctrl + S ou indo para o menu Arquivo e escolher salvar `Suppliers`.

Salvar a tabela deverá atualizar o Gerenciador de servidores, incluindo a coluna adicionada apenas a `Suppliers` lista de colunas da tabela s. Além disso, a expressão inserida na caixa de texto (fórmula) ajustará automaticamente a uma expressão equivalente que retira o espaço em branco desnecessário, ao redor de nomes de coluna entre colchetes (`[]`) e inclui parênteses para mostrar mais explicitamente a ordem das operações:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Para obter mais informações sobre colunas computadas no Microsoft SQL Server, consulte o [documentação técnica](https://msdn.microsoft.com/library/ms191250.aspx). Confira também o [como: especificar colunas computadas](https://msdn.microsoft.com/library/ms188300.aspx) para obter uma explicação passo a passo da criação de colunas computadas.

> [!NOTE]
> Por padrão, as colunas computadas não são armazenadas fisicamente na tabela, mas em vez disso, são recalculadas sempre que eles são referenciados em uma consulta. Ao marcar a caixa de seleção é persistente, no entanto, você pode instruir do SQL Server para armazenar fisicamente a coluna computada na tabela. Isso permite que um índice a ser criado na coluna computada, que pode melhorar o desempenho das consultas que usam o valor de coluna computada em seus `WHERE` cláusulas. Ver [Criando índices em colunas computadas](https://msdn.microsoft.com/library/ms189292.aspx) para obter mais informações.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Etapa 2: Exibir os valores de coluna computada s

Antes de começarmos o trabalho na camada de acesso a dados, let s Reserve um minuto para exibir o `FullContactName` valores. No Server Explorer, clique com botão direito no `Suppliers` nome da tabela e escolha nova consulta no menu de contexto. Isso abrirá uma janela de consulta que solicita a escolha de quais tabelas a serem incluídas na consulta. Adicionar o `Suppliers` de tabela e clique em Fechar. Em seguida, verifique as `CompanyName`, `ContactName`, `ContactTitle`, e `FullContactName` colunas da tabela fornecedores. Por fim, clique no ícone de ponto de exclamação vermelho na barra de ferramentas para executar a consulta e exibir os resultados.

Como mostra a Figura 2, os resultados incluirão `FullContactName`, que lista os `CompanyName`, `ContactName`, e `ContactTitle` colunas usando o formato ldquo;`ContactName` (`ContactTitle`, `CompanyName`) .


[![O FullContactName usa formato ContactName (TítuloDoContato, CompanyName)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Figura 2**: O `FullContactName` usa o formato `ContactName` (`ContactTitle`, `CompanyName`) ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Etapa 3: Adicionando o`SuppliersTableAdapter`para a camada de acesso a dados

Para trabalhar com as informações de fornecedor em nosso aplicativo é necessário primeiro criar um TableAdapter e uma DataTable em nossa DAL. Idealmente, isso poderia ser feito usando as mesmas etapas simples examinadas nos tutoriais anteriores. No entanto, trabalhar com colunas computadas apresenta algumas dobras que merecem uma discussão.

Se você estiver usando um TableAdapter que usa instruções SQL ad hoc, você pode simplesmente incluir a coluna computada na consulta principal s TableAdapter via o Assistente de configuração do TableAdapter. Isso, no entanto, irá gerar automaticamente `INSERT` e `UPDATE` instruções que incluem a coluna computada. Se você tentar executar um desses métodos, um `SqlException` com a mensagem a coluna *ColumnName* não pode ser modificado porque ele é uma coluna computada ou é o resultado de um operador UNION será lançado. Enquanto o `INSERT` e `UPDATE` instrução pode ser ajustada manualmente por meio do TableAdapter s `InsertCommand` e `UpdateCommand` propriedades, essas personalizações serão perdidas sempre que o Assistente de configuração do TableAdapter é executado novamente.

Devido à fragilidade dos TableAdapters que usam instruções SQL ad hoc, é recomendável que podemos usar procedimentos armazenados ao trabalhar com colunas computadas. Se você estiver usando procedimentos armazenados existentes, basta configurar o TableAdapter conforme discutido na [usando procedimentos armazenados existentes para o s TableAdapters do conjunto de dados tipado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) tutorial. Se você tiver o TableAdapter assistente criar os procedimentos armazenados para você, no entanto, é importante omitir inicialmente colunas computadas da consulta principal. Se você incluir uma coluna computada na consulta principal, o Assistente de configuração do TableAdapter irá informá-lo, após a conclusão, que ele não é possível criar os procedimentos armazenados correspondentes. Em resumo, precisamos configurar inicialmente o TableAdapter usando uma consulta principal livre de coluna computada e, em seguida, atualizar manualmente o procedimento armazenado correspondente e o TableAdapter s `SelectCommand` para incluir a coluna computada. Essa abordagem é semelhante à usada na [atualizando o TableAdapter para usar](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s* tutorial.

Para este tutorial, deixe s adicionar um novo TableAdapter e fazê-la automaticamente criar os procedimentos armazenados para nós. Consequentemente, precisaremos inicialmente omitir o `FullContactName` coluna computada da consulta principal.

Comece abrindo o `NorthwindWithSprocs` conjunto de dados a `~/App_Code/DAL` pasta. Clique com botão direito no Designer e, no menu de contexto, escolha Adicionar um novo TableAdapter. Isso iniciará o Assistente de configuração do TableAdapter. Especifique o banco de dados para consultar dados do (`NORTHWNDConnectionString` de `Web.config`) e clique em Avançar. Uma vez que estamos ainda não tiver criado todos os procedimentos armazenados para consultar ou modificar o `Suppliers` da tabela, selecione a criar novos procedimentos armazenados de opção para que o assistente vai criá-los para nós e clique em Avançar.


[![Escolha os procedimentos armazenados nova opção de criar](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Figura 3**: escolha os procedimentos armazenados nova opção de criar ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image9.png))


Etapa subsequente nos solicita a consulta principal. Insira a seguinte consulta retorna o `SupplierID`, `CompanyName`, `ContactName`, e `ContactTitle` colunas para cada fornecedor. Observe que essa consulta propositadamente omite a coluna computada (`FullContactName`); atualizaremos o procedimento armazenado correspondente para incluir essa coluna na etapa 4.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Depois de inserir a consulta principal e clicar em Avançar, o assistente permite nomear os quatro procedimentos armazenados, que ele irá gerar. Nomeie esses procedimentos armazenados `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, e `Suppliers_Delete`, como mostra a Figura 4.


[![Personalizar os nomes dos procedimentos armazenados gerados automaticamente](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Figura 4**: personalizar os nomes dos procedimentos armazenados Auto-Generated ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image12.png))


A próxima etapa do assistente permite nomear os métodos do TableAdapter s e especificar os padrões usados para acessar e atualizar dados. Deixe todas as três caixas de seleção marcadas, mas renomear os `GetData` método para `GetSuppliers`. Clique em Concluir para concluir o assistente.


[![Renomeie o método GetData para GetSuppliers](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Figura 5**: renomear a `GetData` método `GetSuppliers` ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image15.png))


Ao clicar em Concluir, o assistente criará quatro procedimentos armazenados e adicionar o TableAdapter e uma DataTable correspondente ao conjunto de dados tipado.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Etapa 4: Incluindo a coluna computada da consulta principal do TableAdapter s

Agora, precisamos atualizar o TableAdapter e DataTable criada na etapa 3 para incluir o `FullContactName` coluna computada. Isso envolve duas etapas:

1. Atualizando o `Suppliers_Select` procedimento armazenado para retornar o `FullContactName` coluna computada, e
2. Atualizando a DataTable para incluir um correspondente `FullContactName` coluna.

Comece navegando até o Gerenciador de servidores e fazer uma busca detalhada para a pasta de procedimentos armazenados. Abra o `Suppliers_Select` procedimento armazenado e atualização a `SELECT` consulta para incluir o `FullContactName` coluna computada:


[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Salve as alterações para o procedimento armazenado, clicando no ícone Salvar na barra de ferramentas, pressionando Ctrl + S ou escolhendo o salvamento `Suppliers_Select` opção no menu arquivo.

Em seguida, retornar para o Designer de conjunto de dados, clique com botão direito no `SuppliersTableAdapter`e escolha Configure no menu de contexto. Observe que o `Suppliers_Select` coluna agora inclui o `FullContactName` coluna em sua coleção de colunas de dados.


[![Execute o Assistente de configuração do TableAdapter s para atualizar as colunas de s DataTable](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Figura 6**: execute o Assistente de configuração para atualizar o s DataTable colunas TableAdapter s ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image18.png))


Clique em Concluir para concluir o assistente. Isso adicionará automaticamente uma coluna correspondente para o `SuppliersDataTable`. O Assistente do TableAdapter é inteligente o suficiente para detectar que o `FullContactName` coluna é uma coluna computada e, portanto, somente leitura. Consequentemente, ele define a coluna s `ReadOnly` propriedade para `true`. Para verificar isso, selecione a coluna do `SuppliersDataTable` e, em seguida, vá para a janela Propriedades (consulte a Figura 7). Observe que o `FullContactName` coluna s `DataType` e `MaxLength` propriedades também serão definidas de acordo.


[![A coluna FullContactName está marcada como somente leitura](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Figura 7**: O `FullContactName` coluna está marcada como somente leitura ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Etapa 5: Adicionando um`GetSupplierBySupplierID`método ao TableAdapter

Para este tutorial, criaremos uma página ASP.NET que exibe os fornecedores em uma grade atualizável. No passado tutoriais atualizamos um único registro da camada de lógica de negócios recuperando que determinado registro da DAL como um DataTable fortemente tipadas, Atualizando suas propriedades e, em seguida, enviando a DataTable atualizada de volta para a DAL para propagar as alterações para o banco de dados. Para realizar esta etapa primeiro - recuperar o registro que está sendo atualizado da DAL - é necessário primeiro adicionar um `GetSupplierBySupplierID(supplierID)` método para o DAL.

Clique com botão direito no `SuppliersTableAdapter` no Design do conjunto de dados e escolha a opção de adicionar consulta no menu de contexto. Como fizemos na etapa 3, permitir que o assistente gerar um novo procedimento armazenado para nós, selecionando a opção de procedimento armazenado criar novo (consulte novamente a Figura 3 para uma captura de tela nesta etapa do assistente). Uma vez que esse método retornará um registro com várias colunas, indica que desejamos usar uma consulta SQL que é um SELECT que retorna linhas e clique em Avançar.


[![Escolha o SELECT que retorna linhas opção](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Figura 8**: escolha a SELECT que retorna linhas opção ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image24.png))


Etapa subsequente nos solicita para a consulta a ser usado para esse método. Insira o seguinte, que retorna os mesmos campos de dados como a consulta principal, mas para um determinado fornecedor.


[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

A próxima tela pergunta para nomear o procedimento armazenado que será gerado automaticamente. Nomeie esse procedimento armazenado `Suppliers_SelectBySupplierID` e clique em Avançar.


[![Nome do procedimento armazenado Suppliers_SelectBySupplierID](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Figura 9**: nomear o procedimento armazenado `Suppliers_SelectBySupplierID` ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image27.png))


Por fim, a assistente solicita nós para os dados de acesso padrões e nomes de método a ser usado para o TableAdapter. Deixe ambas as caixas de seleção marcadas, mas renomear os `FillBy` e `GetDataBy` métodos `FillBySupplierID` e `GetSupplierBySupplierID`, respectivamente.


[![Nome do TableAdapter métodos FillBySupplierID e GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Figura 10**: nomear os métodos TableAdapter `FillBySupplierID` e `GetSupplierBySupplierID` ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image30.png))


Clique em Concluir para concluir o assistente.

## <a name="step-6-creating-the-business-logic-layer"></a>Etapa 6: Criando a camada de lógica de negócios

Antes de criarmos uma página ASP.NET que usa a coluna computada criada na etapa 1, primeiro precisamos adicionar os métodos correspondentes na BLL. Nossa página do ASP.NET, que será criado na etapa 7, permitirá que os usuários exibam e editem fornecedores. Portanto, precisamos de nosso BLL a fornecer, no mínimo, um método para obter todos os fornecedores e outro para atualizar um fornecedor específico.

Criar um novo arquivo de classe chamado `SuppliersBLLWithSprocs` no `~/App_Code/BLL` pasta e adicione o seguinte código:


[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

Como as outras classes BLL, `SuppliersBLLWithSprocs` tem uma `protected` `Adapter` propriedade que retorna uma instância das `SuppliersTableAdapter` classe junto com dois `public` métodos: `GetSuppliers` e `UpdateSupplier`. O `GetSuppliers` chamadas de método e retorna o `SuppliersDataTable` retornado pela correspondente `GetSupplier` método na camada de acesso a dados. O `UpdateSupplier` método recupera informações sobre o fornecedor específico que está sendo atualizada por meio de uma chamada para o s DAL `GetSupplierBySupplierID(supplierID)` método. Em seguida, ele atualiza o `CategoryName`, `ContactName`, e `ContactTitle` propriedades e confirma essas alterações para o banco de dados chamando a camada de acesso a dados s `Update` método, passando o modificado `SuppliersRow` objeto.

> [!NOTE]
> Exceto para `SupplierID` e `CompanyName`, todas as colunas na tabela Fornecedores permitem `NULL` valores. Portanto, se passado `contactName` ou `contactTitle` parâmetros são `null` precisamos definir correspondente `ContactName` e `ContactTitle` propriedades para um `NULL` banco de dados de valor usando o `SetContactNameNull` e `SetContactTitleNull`métodos, respectivamente.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Etapa 7: Trabalhando com a coluna computada da camada de apresentação

Com a coluna computada adicionada para o `Suppliers` tabela e o DAL e BLL atualizado adequadamente, estamos prontos para criar uma página ASP.NET que funciona com o `FullContactName` coluna computada. Comece abrindo o `ComputedColumns.aspx` página o `AdvancedDAL` pasta e arraste um controle GridView na caixa de ferramentas para o Designer. Definir o s GridView `ID` propriedade para `Suppliers` e, na marca inteligente, de associá-lo a um novo ObjectDataSource chamado `SuppliersDataSource`. Configurar o ObjectDataSource para usar o `SuppliersBLLWithSprocs` classe adicionamos na etapa 6 de volta e clique em Avançar.


[![Configurar o ObjectDataSource para usar a classe SuppliersBLLWithSprocs](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Figura 11**: configurar o ObjectDataSource para usar o `SuppliersBLLWithSprocs` classe ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image33.png))


Há apenas dois métodos definidos na `SuppliersBLLWithSprocs` classe: `GetSuppliers` e `UpdateSupplier`. Garantir que esses dois métodos são especificados em SELECT e atualizar guias, respectivamente e clique em Concluir para concluir a configuração do ObjectDataSource.

Após a conclusão do Assistente de configuração de fonte de dados, o Visual Studio adicionará um BoundField para cada um dos campos de dados retornados. Remover o `SupplierID` BoundField e altere o `HeaderText` propriedades da `CompanyName`, `ContactName`, `ContactTitle`, e `FullContactName` BoundFields da empresa, entre em contato com nome, título e nome completo do contato, respectivamente. Na marca inteligente, marque a caixa de seleção Habilitar edição para ativar os recursos de edição GridView s internos.

Além de adicionar BoundFields a GridView, conclusão do Assistente de fonte de dados também faz com que o Visual Studio para definir o s ObjectDataSource `OldValuesParameterFormatString` propriedade original\_{0}. Reverter essa configuração novamente para seu valor padrão, {0} .

Depois de fazer essas edições GridView e ObjectDataSource, sua marcação declarativa deve ser semelhante ao seguinte:


[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

Em seguida, visite esta página por meio de um navegador. Como mostra a Figura 12, cada fornecedor está listado em uma grade que inclui o `FullContactName` coluna, cujo valor é simplesmente a concatenação de três colunas formatadas como `ContactName` (`ContactTitle`, `CompanyName`).


[![Cada fornecedor é listada na grade](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Figura 12**: cada fornecedor é listada na grade ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image36.png))


Clique no botão Editar para um determinado fornecedor faz com que um postback e renderizou aquela linha na sua edição de interface (veja a Figura 13). As três primeiras colunas renderizam em sua interface de edição padrão – controle de uma caixa de texto cuja `Text` estiver definida como o valor do campo de dados. O `FullContactName` coluna, no entanto, permanece como texto. Quando os BoundFields foram adicionadas a GridView após a conclusão do Assistente de configuração de fonte de dados, o `FullContactName` s BoundField `ReadOnly` propriedade foi definida como `true` porque correspondente `FullContactName` coluna o `SuppliersDataTable` tem seu `ReadOnly` propriedade definida como `true`. Conforme observado na etapa 4, o `FullContactName` s `ReadOnly` propriedade foi definida como `true` porque o TableAdapter detectou que a coluna era uma coluna computada.


[![A coluna FullContactName não é editável](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Figura 13**: O `FullContactName` coluna não é editável ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-cs/_static/image39.png))


Vá em frente e atualize o valor de um ou mais das colunas editáveis e clique em Atualizar. Observe como o `FullContactName` valor s é atualizado automaticamente para refletir a alteração.

> [!NOTE]
> O GridView atualmente usa BoundFields para os campos editáveis, resultando no padrão de interface de edição. Uma vez que o `CompanyName` campo é obrigatório, ele deverá ser convertido em um TemplateField que inclui um RequiredFieldValidator. Vou deixar isso como um exercício para os leitores interessados. Consulte a [adicionando controles de validação para a edição e inserção de Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) tutorial para obter instruções detalhadas sobre como converter um BoundField em um TemplateField e adicionando controles de validação.


## <a name="summary"></a>Resumo

Ao definir o esquema para uma tabela, o Microsoft SQL Server permite a inclusão das colunas computadas. Essas são colunas cujos valores são calculados a partir de uma expressão que geralmente faz referência os valores de outras colunas no mesmo registro. Desde os valores para colunas computadas são com base em uma expressão, eles são somente leitura e não pode ser atribuídos um valor em uma `INSERT` ou `UPDATE` instrução. Isso apresenta desafios ao usar uma coluna computada na consulta principal de um TableAdapter que tenta gerar automaticamente correspondente `INSERT`, `UPDATE`, e `DELETE` instruções.

Neste tutorial, estamos discute técnicas para contornar os desafios apresentados por colunas computadas. Em particular, usamos os procedimentos armazenados em nosso TableAdapter para superar a fragilidade inerente TableAdapters que usam instruções SQL ad hoc. Ao ter o TableAdapter assistente criar novos procedimentos armazenados, é importante que temos a consulta principal inicialmente omitir qualquer coluna computada porque sua presença impede que os procedimentos armazenado de modificação de dados que está sendo gerado. Depois que o TableAdapter foi configurado inicialmente, seu `SelectCommand` procedimento armazenado pode ser alterado para incluir quaisquer colunas computadas.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Hilton Geisenow e Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-additional-datatable-columns-cs.md)
> [Próximo](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
