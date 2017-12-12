---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
title: Trabalhando com colunas computadas (VB) | Microsoft Docs
author: rick-anderson
description: "Ao criar uma tabela de banco de dados, o Microsoft SQL Server permite que você defina uma coluna computada cujo valor é calculado a partir de uma expressão que geralmente referen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 5811b8ff-ed56-40fc-9397-6b69ae09a8f6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-vb
msc.type: authoredcontent
ms.openlocfilehash: a6ff0df27e19d6feecde27a77d4b212d1e9bc45e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="working-with-computed-columns-vb"></a>Trabalhando com colunas computadas (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_VB.zip) ou [baixar PDF](working-with-computed-columns-vb/_static/datatutorial71vb1.pdf)

> Ao criar uma tabela de banco de dados, o Microsoft SQL Server permite que você defina uma coluna computada cujo valor é calculado de uma expressão que normalmente faz referência a outros valores no mesmo registro de banco de dados. Esses valores são somente leitura no banco de dados, o que requer considerações especiais ao trabalhar com TableAdapters. Neste tutorial, Aprenda a enfrentar os desafios causados por colunas computadas.


## <a name="introduction"></a>Introdução

Microsoft SQL Server permite  *[colunas computadas](https://msdn.microsoft.com/en-us/library/ms191250.aspx)*, que são colunas cujos valores são calculados a partir de uma expressão que normalmente faz referência os valores de outras colunas na mesma tabela. Por exemplo, um modelo de dados de controle de tempo pode ter uma tabela chamada `ServiceLog` com colunas incluindo `ServicePerformed`, `EmployeeID`, `Rate`, e `Duration`, entre outros. Enquanto o valor devido por serviço item (sendo a taxa multiplicada pela duração) pode ser calculada por meio de uma página da web ou outro interface de programação, pode ser útil para incluir uma coluna no `ServiceLog` tabela denominada `AmountDue` que relataram isso informações. Esta coluna pode ser criada como uma coluna normal, mas ele precisa ser atualizado a qualquer momento o `Rate` ou `Duration` valores da coluna alterados. Uma abordagem melhor seria tornar o `AmountDue` coluna uma coluna computada usando a expressão `Rate * Duration`. Isso faria com que o SQL Server calcular automaticamente o `AmountDue` o valor da coluna sempre que ele foi mencionado em uma consulta.

Como um valor de coluna computada s é determinado por uma expressão, essas colunas são somente leitura e, portanto, não podem ter valores atribuídas a eles no `INSERT` ou `UPDATE` instruções. No entanto, quando as colunas computadas são parte da consulta principal para um TableAdapter que usa instruções SQL ad hoc, eles são incluídos automaticamente no gerado automaticamente `INSERT` e `UPDATE` instruções. Consequentemente, o TableAdapter s `INSERT` e `UPDATE` consultas e `InsertCommand` e `UpdateCommand` propriedades devem ser atualizadas para remover referências a quaisquer colunas computadas.

Um desafio de usar computada colunas com um TableAdapter que usa instruções SQL ad hoc é que o s TableAdapter `INSERT` e `UPDATE` consultas forem gerados novamente automaticamente qualquer tempo concluir o Assistente de configuração do TableAdapter. Portanto, as colunas computadas removidos manualmente do `INSERT` e `UPDATE` consultas reaparecerá se executar novamente o assistente. Embora TableAdapters que usam procedimentos armazenados don t sofrem com este fragilidade, têm seus próprios quirks que serão abordados na etapa 3.

Neste tutorial, adicionaremos uma coluna computada para a `Suppliers` de tabela no banco de dados Northwind e, em seguida, crie um TableAdapter correspondente para trabalhar com essa tabela e uma coluna computada. Teremos nosso TableAdapter usar procedimentos armazenados em vez de instruções SQL ad hoc para que nossos t de personalizações perdido quando o Assistente de configuração do TableAdapter é usado.

Permitir que o s começar!

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Etapa 1: Adicionar uma coluna computada para a`Suppliers`tabela

O banco de dados Northwind não tem qualquer coluna computada, portanto, precisamos adicionar um sozinhos. Para este tutorial permitem s adicionar uma coluna computada para a `Suppliers` tabela chamada `FullContactName` que retorna o nome do contato s, título e a empresa que funcionam no seguinte formato: `ContactName` (`ContactTitle`, `CompanyName`). Isto computado de coluna pode ser usada em relatórios ao exibir informações sobre fornecedores.

Comece abrindo o `Suppliers` definição da tabela clicando no `Suppliers` de tabela no Gerenciador de servidores e escolhendo abrir definição de tabela no menu de contexto. Isso exibirá as colunas da tabela e suas propriedades, como o tipo de dados, se eles permitem `NULL` s e assim por diante. Para adicionar uma coluna computada, comece digitando o nome da coluna na definição da tabela. Em seguida, digite a expressão na caixa de texto (fórmula) na seção de especificação de coluna computada na janela Propriedades de coluna (consulte a Figura 1). Nome da coluna computada `FullContactName` e use a seguinte expressão:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample1.sql)]

Observe que as cadeias de caracteres podem ser concatenadas em SQL usando o `+` operador. O `CASE` instrução pode ser usada como uma condicional em uma linguagem de programação tradicional. Na expressão acima de `CASE` instrução pode ser lido como: se `ContactTitle` não é `NULL` de saída, em seguida, o `ContactTitle` valor concatenado com uma vírgula, caso contrário, emitir nada. Para obter mais informações sobre a utilidade do `CASE` instrução, consulte [Power de SQL `CASE` instruções](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> Em vez de usar um `CASE` instrução aqui, poderíamos ter Alternativamente usado `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/en-us/library/ms184325.aspx)Retorna *checkExpression* se for não nulo, caso contrário, retornará *replacementValue*. Enquanto o `ISNULL` ou `CASE` funcionará nesta instância, há cenários mais complexos onde a flexibilidade do `CASE` instrução não pode ser correspondida por `ISNULL`.


Depois de adicionar a coluna computada sua tela deve ser semelhante a tela na Figura 1.


[![Adicionar uma coluna calculada denominada FullContactName à tabela fornecedores](working-with-computed-columns-vb/_static/image2.png)](working-with-computed-columns-vb/_static/image1.png)

**Figura 1**: Adicione um chamado de coluna computada `FullContactName` para o `Suppliers` tabela ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image3.png))


Depois de nomear a coluna computada e inserir sua expressão, salve as alterações à tabela clicando no ícone Salvar na barra de ferramentas, pressionando Ctrl + S ou no menu Arquivo e escolhendo Salvar `Suppliers`.

Salvar a tabela deverá atualizar o Gerenciador de servidores, incluindo a coluna adicionada a apenas o `Suppliers` lista de colunas de tabela s. Além disso, a expressão digitada na caixa de texto (fórmula) ajustará automaticamente para uma expressão equivalente que retira o desnecessário espaço em branco, contorna os nomes de coluna entre colchetes (`[]`) e inclui parênteses para mostrar mais explicitamente a ordem das operações:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample2.sql)]

Para obter mais informações sobre colunas computadas no Microsoft SQL Server, consulte o [documentação técnica](https://msdn.microsoft.com/en-us/library/ms191250.aspx). Verifique também o [como: especificar colunas computadas](https://msdn.microsoft.com/en-us/library/ms188300.aspx) para obter uma explicação passo a passo de criação de colunas computadas.

> [!NOTE]
> Por padrão, as colunas computadas não são fisicamente armazenadas na tabela, mas em vez disso, são recalculadas sempre que eles são referenciados em uma consulta. Ao marcar a caixa de seleção é mantido, no entanto, você pode instruir SQL Server para armazenar fisicamente a coluna computada na tabela. Isso permite que um índice a ser criado na coluna computada, o que pode melhorar o desempenho das consultas que usam o valor de coluna computada em seus `WHERE` cláusulas. Consulte [Criando índices em colunas computadas](https://msdn.microsoft.com/en-us/library/ms189292.aspx) para obter mais informações.


## <a name="step-2-viewing-the-computed-column-s-values"></a>Etapa 2: Exibir os s valores de coluna computada

Antes de começar trabalho na camada de acesso a dados, permitem s levar um minuto para exibir o `FullContactName` valores. No Gerenciador de servidores, clique duas vezes no `Suppliers` nome da tabela e escolha nova consulta no menu de contexto. Isso abrirá uma janela de consulta que solicita a escolha de quais tabelas a serem incluídas na consulta. Adicionar o `Suppliers` de tabela e clique em Fechar. Em seguida, verifique o `CompanyName`, `ContactName`, `ContactTitle`, e `FullContactName` colunas da tabela fornecedores. Por fim, clique no ícone de ponto de exclamação vermelho na barra de ferramentas para executar a consulta e exibir os resultados.

Como mostra a Figura 2, os resultados incluem `FullContactName`, que lista o `CompanyName`, `ContactName`, e `ContactTitle` colunas usando o formato `ContactName` (`ContactTitle`, `CompanyName`).


[![O FullContactName usa o formato ContactName (TítuloDoContato, CompanyName)](working-with-computed-columns-vb/_static/image5.png)](working-with-computed-columns-vb/_static/image4.png)

**Figura 2**: O `FullContactName` usa o formato `ContactName` (`ContactTitle`, `CompanyName`) ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image6.png))


## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Etapa 3: Adicionando a`SuppliersTableAdapter`a camada de acesso a dados

Para trabalhar com as informações do fornecedor em nosso aplicativo, precisamos criar primeiro um TableAdapter e DataTable no nosso DAL. Idealmente, isso deve ser feito usando as mesmas etapas simples examinadas em tutoriais anteriores. No entanto, trabalhar com colunas computadas apresenta alguns dobras que merecem discussão.

Se você estiver usando um TableAdapter que usa instruções SQL ad hoc, você simplesmente pode incluir a coluna computada na consulta principal s TableAdapter por meio do Assistente de configuração do TableAdapter. Isso, no entanto, gerará automaticamente `INSERT` e `UPDATE` instruções que incluem a coluna computada. Se você tentar executar um desses métodos, um `SqlException` com a mensagem a coluna *ColumnName* não pode ser modificada porque é uma coluna computada ou é o resultado de um operador UNION será lançado. Enquanto o `INSERT` e `UPDATE` instrução pode ser ajustada manualmente por meio do TableAdapter s `InsertCommand` e `UpdateCommand` propriedades, essas atualizações serão perdidas sempre que o Assistente de configuração do TableAdapter é executado de novo.

Devido a fragilidade do TableAdapters que usam instruções SQL ad hoc, é recomendável que podemos usar procedimentos armazenados ao trabalhar com colunas computadas. Se você estiver usando procedimentos armazenados existentes, basta configurar o TableAdapter conforme discutido no [usando existente procedimentos armazenados para o conjunto de dados tipado de s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial. No entanto, se você tiver o TableAdapter Assistente de criar os procedimentos armazenados para você, é importante inicialmente omitir qualquer coluna computada da consulta principal. Se você incluir uma coluna computada na consulta principal, o Assistente de configuração do TableAdapter informará, após a conclusão, se ele não é possível criar os procedimentos armazenados correspondentes. Em resumo, é preciso configurar inicialmente o TableAdapter usando uma consulta principal livre de coluna computada e, em seguida, atualizar manualmente o procedimento armazenado correspondente e o TableAdapter s `SelectCommand` para incluir a coluna computada. Essa abordagem é semelhante àquela usada no [atualizando o TableAdapter usar](updating-the-tableadapter-to-use-joins-vb.md)`JOIN`*s* tutorial.

Para este tutorial, deixe s adicionar um novo TableAdapter e criar automaticamente os procedimentos armazenados para nós. Consequentemente, será necessário omitir inicialmente o `FullContactName` coluna computada da consulta principal.

Comece abrindo o `NorthwindWithSprocs` conjunto de dados a `~/App_Code/DAL` pasta. Com o botão direito no Designer e, no menu de contexto, escolha Adicionar um novo TableAdapter. Isso iniciará o Assistente de configuração do TableAdapter. Especifique o banco de dados para consultar dados do (`NORTHWNDConnectionString` de `Web.config`) e clique em Avançar. Já que estamos ainda não tiver criado todos os procedimentos armazenados para consultar ou modificar o `Suppliers` da tabela, selecione a criar novos procedimentos armazenados de opção para que o assistente vai criá-los para nós e clique em Avançar.


[![Escolha os procedimentos armazenados nova opção de criar](working-with-computed-columns-vb/_static/image8.png)](working-with-computed-columns-vb/_static/image7.png)

**Figura 3**: escolha os procedimentos armazenados nova opção de criar ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image9.png))


A próxima etapa solicita nós da consulta principal. Digite a seguinte consulta retorna o `SupplierID`, `CompanyName`, `ContactName`, e `ContactTitle` colunas para cada fornecedor. Observe que essa consulta intencionalmente omite a coluna computada (`FullContactName`); atualizaremos o procedimento armazenado correspondente para incluir essa coluna na etapa 4.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample3.sql)]

Depois de inserir a consulta principal e clicar em Avançar, o assistente permite nomear os quatro procedimentos armazenados que ele irá gerar. Nomear esses procedimentos armazenados `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`, e `Suppliers_Delete`, como mostra a Figura 4.


[![Personalize os nomes dos procedimentos armazenados gerados automaticamente](working-with-computed-columns-vb/_static/image11.png)](working-with-computed-columns-vb/_static/image10.png)

**Figura 4**: personalize os nomes dos procedimentos armazenados Auto-Generated ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image12.png))


A próxima etapa do assistente permite nomear os métodos TableAdapter e especificar os padrões usados para acessar e atualizar dados. Deixe todas as caixas de três seleção verificadas, mas renomear o `GetData` método `GetSuppliers`. Clique em Concluir para concluir o assistente.


[![Renomeie o método GetData para GetSuppliers](working-with-computed-columns-vb/_static/image14.png)](working-with-computed-columns-vb/_static/image13.png)

**Figura 5**: renomear a `GetData` método `GetSuppliers` ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image15.png))


Após clicar em Concluir, o assistente criar quatro procedimentos armazenados e adicione o TableAdapter e DataTable correspondente ao conjunto de dados tipado.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Etapa 4: Incluindo a coluna computada na consulta TableAdapter s principal

Agora, precisamos atualizar o TableAdapter e DataTable criado na etapa 3 para incluir o `FullContactName` coluna computada. Isso envolve duas etapas:

1. Atualizando o `Suppliers_Select` procedimento armazenado para retornar o `FullContactName` coluna computada, e
2. Atualizando a DataTable para incluir um correspondente `FullContactName` coluna.

Comece navegando até o Gerenciador de servidores e drill-down até a pasta de procedimentos armazenados. Abra o `Suppliers_Select` procedimento armazenado e atualização de `SELECT` consulta para incluir o `FullContactName` coluna computada:


[!code-sql[Main](working-with-computed-columns-vb/samples/sample4.sql)]

Salvar as alterações para o procedimento armazenado, clicando no ícone Salvar na barra de ferramentas, pressionando Ctrl + S ou escolhendo Salvar `Suppliers_Select` opção no menu arquivo.

Em seguida, retornar para o Designer de conjunto de dados, clique duas vezes no `SuppliersTableAdapter`e escolha Configure no menu de contexto. Observe que o `Suppliers_Select` coluna agora inclui o `FullContactName` coluna em sua coleção de colunas de dados.


[![Execute o Assistente de configuração do TableAdapter s para atualizar as colunas de s DataTable](working-with-computed-columns-vb/_static/image17.png)](working-with-computed-columns-vb/_static/image16.png)

**Figura 6**: executar o Assistente de configuração para atualizar o s DataTable colunas TableAdapter s ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image18.png))


Clique em Concluir para concluir o assistente. Isso adicionará automaticamente uma coluna correspondente para o `SuppliersDataTable`. O assistente TableAdapter é suficientemente inteligente para detectar que o `FullContactName` coluna é uma coluna calculada e, portanto, somente leitura. Consequentemente, ele define a coluna s `ReadOnly` propriedade `true`. Para verificar isso, selecione a coluna de `SuppliersDataTable` e, em seguida, vá para a janela de propriedades (consulte a Figura 7). Observe que o `FullContactName` coluna s `DataType` e `MaxLength` propriedades também são definidas adequadamente.


[![A coluna FullContactName está marcada como somente leitura](working-with-computed-columns-vb/_static/image20.png)](working-with-computed-columns-vb/_static/image19.png)

**Figura 7**: O `FullContactName` coluna está marcada como somente leitura ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image21.png))


## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Etapa 5: Adicionando um`GetSupplierBySupplierID`método para o TableAdapter

Para este tutorial, criaremos uma página ASP.NET que exibe os fornecedores em uma grade atualizável. No passado tutoriais atualizamos um único registro da camada de lógica de negócios recuperando que determinado registro de DAL como um DataTable fortemente tipada, atualizar suas propriedades e, em seguida, enviar a DataTable atualizada de volta para a DAL para propagar as alterações o banco de dados. Para realizar esta etapa primeiro - recuperar o registro que está sendo atualizado da DAL - precisamos adicionar primeiro uma `GetSupplierBySupplierID(supplierID)` método DAL.

Clique com botão direito no `SuppliersTableAdapter` no Design do conjunto de dados e escolha a opção de consulta adicionar no menu de contexto. Como foi feito na etapa 3, permitem que o Assistente para gerar um novo procedimento armazenado para nós, selecionando a opção de procedimento armazenado criar novo (consulte novamente a Figura 3 para a captura de tela nesta etapa do assistente). Desde que este método retornará um registro com várias colunas, indicam que queremos usar uma consulta SQL que é um SELECT que retorna linhas e clique em Avançar.


[![Escolha o SELECT que retorna linhas opção](working-with-computed-columns-vb/_static/image23.png)](working-with-computed-columns-vb/_static/image22.png)

**Figura 8**: escolha o SELECT que retorna linhas opção ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image24.png))


A próxima etapa solicita para a consulta a ser usado para este método. Digite o seguinte, que retorna os mesmos campos de dados como a consulta principal, mas para um fornecedor específico.


[!code-sql[Main](working-with-computed-columns-vb/samples/sample5.sql)]

A próxima tela pede para nomear o procedimento armazenado que será gerado automaticamente. Nomeie esse procedimento armazenado `Suppliers_SelectBySupplierID` e clique em Avançar.


[![Nome do procedimento armazenado Suppliers_SelectBySupplierID](working-with-computed-columns-vb/_static/image26.png)](working-with-computed-columns-vb/_static/image25.png)

**Figura 9**: nome do procedimento armazenado `Suppliers_SelectBySupplierID` ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image27.png))


Por fim, a assistente solicita para os dados de acesso padrões e nomes de método a ser usado para o TableAdapter. Deixe ambas as caixas de seleção marcadas, mas renomear o `FillBy` e `GetDataBy` métodos para `FillBySupplierID` e `GetSupplierBySupplierID`, respectivamente.


[![Nome do TableAdapter métodos FillBySupplierID e GetSupplierBySupplierID](working-with-computed-columns-vb/_static/image29.png)](working-with-computed-columns-vb/_static/image28.png)

**Figura 10**: nomear os métodos TableAdapter `FillBySupplierID` e `GetSupplierBySupplierID` ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image30.png))


Clique em Concluir para concluir o assistente.

## <a name="step-6-creating-the-business-logic-layer"></a>Etapa 6: Criando a camada de lógica de negócios

Antes de criar uma página ASP.NET que usa a coluna computada criada na etapa 1, primeiro é preciso adicionar os métodos correspondentes na BLL. Nossa página de ASP.NET, vamos criar na etapa 7, permitirá que os usuários exibir e editar fornecedores. Portanto, precisamos de nosso BLL para fornecer, no mínimo, um método para obter todos os fornecedores e outro para atualizar um fornecedor específico.

Criar um novo arquivo de classe chamado `SuppliersBLLWithSprocs` no `~/App_Code/BLL` pasta e adicione o seguinte código:


[!code-vb[Main](working-with-computed-columns-vb/samples/sample6.vb)]

Como as outras classes BLL, `SuppliersBLLWithSprocs` tem um `Protected` `Adapter` propriedade que retorna uma instância do `SuppliersTableAdapter` classe junto com dois `Public` métodos: `GetSuppliers` e `UpdateSupplier`. O `GetSuppliers` chamadas de método e retorna o `SuppliersDataTable` retornado por correspondente `GetSupplier` método na camada de acesso a dados. O `UpdateSupplier` método recupera as informações sobre o fornecedor específico que está sendo atualizada por meio de uma chamada para o s DAL `GetSupplierBySupplierID(supplierID)` método. Em seguida, atualiza o `CategoryName`, `ContactName`, e `ContactTitle` propriedades e confirma essas alterações para o banco de dados chamando a camada de acesso a dados s `Update` método, passando o `SuppliersRow` objeto.

> [!NOTE]
> Exceto para `SupplierID` e `CompanyName`, permitir que todas as colunas na tabela fornecedores `NULL` valores. Portanto, se passado `contactName` ou `contactTitle` parâmetros são `Nothing` , precisamos definir correspondente `ContactName` e `ContactTitle` propriedades para um `NULL` banco de dados de valor usando o `SetContactNameNull` e `SetContactTitleNull`métodos, respectivamente.


## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Etapa 7: Trabalhando com a coluna computada da camada de apresentação

Com a coluna computada adicionada a `Suppliers` tabela e a DAL e BLL atualizadas, você está pronto para criar uma página ASP.NET que funciona com o `FullContactName` coluna computada. Comece abrindo o `ComputedColumns.aspx` página o `AdvancedDAL` pasta e arraste um controle GridView da caixa de ferramentas para o Designer. Definir o GridView s `ID` propriedade `Suppliers` e, na marca inteligente, de associá-lo a um novo ObjectDataSource denominado `SuppliersDataSource`. Configurar o ObjectDataSource para usar o `SuppliersBLLWithSprocs` classe adicionamos fazer na etapa 6 e clique em Avançar.


[![Configurar o ObjectDataSource para usar a classe SuppliersBLLWithSprocs](working-with-computed-columns-vb/_static/image32.png)](working-with-computed-columns-vb/_static/image31.png)

**Figura 11**: configurar o ObjectDataSource para usar o `SuppliersBLLWithSprocs` classe ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image33.png))


Há apenas dois métodos definidos no `SuppliersBLLWithSprocs` classe: `GetSuppliers` e `UpdateSupplier`. Certifique-se de que esses dois métodos são especificados em SELECT e atualizar guias, respectivamente e clique em Concluir para concluir a configuração do ObjectDataSource.

Após a conclusão do Assistente de configuração de fonte de dados, o Visual Studio irá adicionar um BoundField para cada um dos campos de dados retornados. Remover o `SupplierID` BoundField e altere o `HeaderText` propriedades do `CompanyName`, `ContactName`, `ContactTitle`, e `FullContactName` BoundFields da empresa, entre em contato com nome, título e nome completo do contato, respectivamente. De marca inteligente, marque a caixa de seleção Habilitar edição ativar os recursos de edição GridView s internos.

Além de adicionar BoundFields a GridView, conclusão do Assistente de fonte de dados também faz com que o Visual Studio para definir a s ObjectDataSource `OldValuesParameterFormatString` propriedade original\_{0}. Reverta essa configuração novamente para o valor padrão, {0}.

Depois de fazer essas edições GridView e ObjectDataSource, sua marcação declarativa deve ser semelhante ao seguinte:


[!code-aspx[Main](working-with-computed-columns-vb/samples/sample7.aspx)]

Em seguida, visite esta página por meio de um navegador. Como mostra a Figura 12, cada fornecedor é listado em uma grade que inclui o `FullContactName` coluna, cujo valor é simplesmente a concatenação de três colunas formatados como `ContactName` (`ContactTitle`, `CompanyName`).


[![Cada fornecedor está listado na grade](working-with-computed-columns-vb/_static/image35.png)](working-with-computed-columns-vb/_static/image34.png)

**Figura 12**: cada fornecedor está listado na grade ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image36.png))


Clicar no botão Editar para um fornecedor específico causa um postback e tem linha renderizado em sua edição de interface (consulte a Figura 13). As três primeiras colunas renderizam em sua interface de edição padrão – controle de uma caixa de texto cuja `Text` propriedade é definida como o valor do campo de dados. O `FullContactName` coluna, no entanto, permanece como texto. Quando os BoundFields foram adicionadas a GridView, após a conclusão do Assistente de configuração de fonte de dados, o `FullContactName` BoundField s `ReadOnly` propriedade foi definida como `True` porque correspondente `FullContactName` coluna o `SuppliersDataTable` tem seu `ReadOnly` propriedade definida como `True`. Conforme observado na etapa 4, o `FullContactName` s `ReadOnly` propriedade foi definida como `True` porque o TableAdapter detectou que a coluna era uma coluna computada.


[![A coluna FullContactName não é editável](working-with-computed-columns-vb/_static/image38.png)](working-with-computed-columns-vb/_static/image37.png)

**Figura 13**: O `FullContactName` coluna não é editável ([clique para exibir a imagem em tamanho normal](working-with-computed-columns-vb/_static/image39.png))


Vá em frente e atualizar o valor de uma ou mais das colunas editáveis e clique em Atualizar. Observe como o `FullContactName` valor é atualizado automaticamente para refletir a alteração.

> [!NOTE]
> GridView atualmente usa BoundFields para os campos editáveis, resultando no padrão de interface de edição. Desde o `CompanyName` campo é obrigatório, ele deverá ser convertido em um TemplateField que inclui um RequiredFieldValidator. Posso deixar isso como um exercício para o leitor de interesse. Consulte o [adicionar controles de validação para a edição e inserindo Interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) tutorial para obter instruções detalhadas sobre como converter um BoundField em um TemplateField e adicionando controles de validação.


## <a name="summary"></a>Resumo

Ao definir o esquema para uma tabela, o Microsoft SQL Server permite a inclusão de colunas computadas. Essas são as colunas cujos valores são calculados a partir de uma expressão que normalmente faz referência os valores de outras colunas no mesmo registro. Desde que os valores para as colunas computadas são baseadas em uma expressão, eles são somente leitura e não pode ser atribuídos um valor em uma `INSERT` ou `UPDATE` instrução. Isso apresenta desafios ao usar uma coluna computada na consulta principal de um TableAdapter que tenta gerar automaticamente correspondente `INSERT`, `UPDATE`, e `DELETE` instruções.

Neste tutorial, discutimos técnicas para evitar os desafios causados por colunas computadas. Em particular, usamos os procedimentos armazenados em nosso TableAdapter para superar a fragilidade inerente TableAdapters que usam instruções SQL ad hoc. Com o assistente TableAdapter criar novos procedimentos armazenados, é importante ter a consulta principal inicialmente omitir qualquer coluna computada porque sua presença impede que os procedimentos armazenado de modificação de dados que está sendo gerado. Depois que o TableAdapter foi configurado inicialmente, seu `SelectCommand` procedimento armazenado pode ser alterado para incluir quaisquer colunas computadas.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Geisenow Hilton e Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](adding-additional-datatable-columns-vb.md)
[Próximo](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
