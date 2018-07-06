---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: Atualizando o TableAdapter para usar JOINs (c#) | Microsoft Docs
author: rick-anderson
description: Ao trabalhar com um banco de dados é comum para solicitar os dados são distribuídos entre várias tabelas. Para recuperar dados de duas tabelas diferentes, pode usar qualquer um...
ms.author: aspnetcontent
ms.date: 07/18/2007
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: c59ab13eb5e5e548055c6c8c9343a8d5c4cabe7d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841115"
---
<a name="updating-the-tableadapter-to-use-joins-c"></a>Atualizando o TableAdapter para usar JOINs (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) ou [baixar PDF](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> Ao trabalhar com um banco de dados é comum para solicitar os dados são distribuídos entre várias tabelas. Para recuperar dados de duas tabelas diferentes, pode usar uma subconsulta correlacionada ou uma operação de junção. Neste tutorial comparamos subconsultas correlacionadas e a sintaxe de junção antes de examinar como criar um TableAdapter que inclui uma junção em sua consulta principal.


## <a name="introduction"></a>Introdução

Com bancos de dados relacionais os dados que estamos interessados em trabalhar com muitas vezes são distribuídos em várias tabelas. Por exemplo, ao exibir as informações de produto provavelmente queremos listar cada categoria do produto s correspondente e nomes de s do fornecedor. O `Products` a tabela tem `CategoryID` e `SupplierID` valores, mas os nomes de categoria e fornecedor reais estão no `Categories` e `Suppliers` tabelas, respectivamente.

Para recuperar informações de outra tabela relacionada, podemos pode usar *subconsultas correlacionadas* ou `JOIN` *s*. Uma subconsulta correlacionada é aninhado `SELECT` consulta que faz referência a colunas na consulta externa. Por exemplo, nos [criando uma camada de acesso de dados](../introduction/creating-a-data-access-layer-cs.md) tutorial, usamos duas subconsultas correlacionadas no `ProductsTableAdapter` a consulta principal s para retornar os nomes de categoria e fornecedor para cada produto. Um `JOIN` é uma construção SQL que mescla as linhas relacionadas de duas tabelas diferentes. Usamos uma `JOIN` no [consultando dados com o controle SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) tutorial para exibir informações de categoria ao lado de cada produto.

O motivo pelo qual podemos ter abstenção usem `JOIN` s com os TableAdapters é devido às limitações no Assistente para gerar automaticamente correspondente s TableAdapter `INSERT`, `UPDATE`, e `DELETE` instruções. Mais especificamente, se a consulta principal do TableAdapter s contiver quaisquer `JOIN` s, o TableAdapter não é possível criar automaticamente as instruções SQL ad hoc ou procedimentos armazenados para seus `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades.

Neste tutorial, vamos comparar rapidamente e contraste subconsultas correlacionadas e `JOIN` s antes de explorar como criar um TableAdapter que inclui `JOIN` s em sua consulta principal.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Subconsultas correlacionadas de comparar e contrastar e`JOIN` s

Lembre-se de que o `ProductsTableAdapter` criado no primeiro tutorial de `Northwind` subconsultas correlacionadas usa o conjunto de dados para trazer de volta cada nome de categoria e fornecedor de correspondente do produto s. O `ProductsTableAdapter` consulta principal de s é mostrada abaixo.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

Os dois correlacionados subconsultas - `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` e `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -são `SELECT` consultas que retornam um valor único por produto como uma coluna adicional no externo `SELECT` lista de colunas da instrução s.

Como alternativa, um `JOIN` pode ser usado para retornar cada nome de fornecedor e a categoria de produto s. A consulta a seguir retorna as mesmas saída que aquele acima, mas usa `JOIN` s em vez de subconsultas:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

Um `JOIN` mescla os registros de uma tabela com registros de outra tabela com base em alguns critérios. Na consulta acima, por exemplo, o `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` instrui o SQL Server para mesclar cada registro de produto com a categoria registre cuja `CategoryID` valor corresponde ao produto s `CategoryID` valor. O resultado mesclado nos permite trabalhar com os campos de categoria correspondente para cada produto (como `CategoryName`).

> [!NOTE]
> `JOIN` s são usadas ao consultar dados de bancos de dados relacionais. Se você estiver familiarizado com o `JOIN` sintaxe ou necessidade para falar um pouco sobre seu uso, d recomendo o [tutorial SQL Join](http://www.w3schools.com/sql/sql_join.asp) na [W3 escolas](http://www.w3schools.com/). Também vale a pena ler são os [ `JOIN` fundamentos](https://msdn.microsoft.com/library/ms191517.aspx) e [conceitos básicos de subconsulta](https://msdn.microsoft.com/library/ms189575.aspx) seções do [Manuais Online do SQL](https://msdn.microsoft.com/library/ms130214.aspx).


Uma vez que `JOIN` s e subconsultas correlacionadas podem ser usadas para recuperar dados relacionados de outras tabelas, muitos desenvolvedores são deixados uma pequena de cabeça e imaginando qual abordagem usar. Todos as especialistas em SQL eu ve falou disseram aproximadamente a mesma coisa, que ele t importava em termos de desempenho como o SQL Server irá gerar planos de execução aproximadamente idênticos. Seu conselho, em seguida, é usar a técnica que você e sua equipe estão mais familiarizados. Ele merece observar que após dar esse conselho esses especialistas imediatamente expressam suas preferências de `JOIN` s sobre subconsultas correlacionadas.

Ao criar uma camada de acesso a dados usando conjuntos de dados tipados, as ferramentas funcionam melhor quando o uso de subconsultas. Em particular, o Assistente de s TableAdapter não gerará automaticamente correspondente `INSERT`, `UPDATE`, e `DELETE` instruções se a consulta principal contiver quaisquer `JOIN` s, mas irá gerar automaticamente essas instruções quando correlacionadas subconsultas são usados.

Para explorar essa deficiência, crie um conjunto de dados tipado temporário no `~/App_Code/DAL` pasta. Durante o Assistente de configuração do TableAdapter, optar por usar instruções SQL ad hoc e insira o seguinte `SELECT` consulta (consulte a Figura 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]


[![Insira uma consulta principal que contém associações](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Figura 1**: insira uma consulta principal que contém `JOIN` s ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))


Por padrão, o TableAdapter automaticamente criará `INSERT`, `UPDATE`, e `DELETE` instruções com base em consulta principal. Se você clicar no botão Avançado, que você pode ver que esse recurso está habilitado. Apesar dessa configuração, o TableAdapter não será possível criar o `INSERT`, `UPDATE`, e `DELETE` instruções porque a consulta principal contém um `JOIN`.


![Insira uma consulta principal que contém associações](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Figura 2**: insira uma consulta principal que contém `JOIN` s


Clique em Concluir para concluir o assistente. Neste ponto seu Designer de s do conjunto de dados incluirá um único TableAdapter com uma DataTable com colunas para cada um dos campos retornados no `SELECT` lista de colunas de consulta s. Isso inclui o `CategoryName` e `SupplierName`, como mostra a Figura 3.


![A DataTable inclui uma coluna para cada campo retornado na lista de colunas](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Figura 3**: DataTable inclui uma coluna para cada campo retornado na lista de colunas


Enquanto a DataTable tem as colunas apropriadas, o TableAdapter não possui valores para seus `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades. Para confirmar isso, clique no TableAdapter no Designer e, em seguida, vá para a janela de propriedades. Lá você verá que o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades são definidas como (nenhum).


[![O InsertCommand, UpdateCommand e DeleteCommand propriedades são definidas como (nenhum)](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Figura 4**: O `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades são definidas como (nenhum) ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))


Para contornar essa deficiência, podemos pode fornecer manualmente as instruções SQL e os parâmetros para o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades por meio da janela Propriedades. Como alternativa, podemos poderia começar configurando a consulta principal do TableAdapter s para *não* incluir qualquer `JOIN` s. Isso permitirá que o `INSERT`, `UPDATE`, e `DELETE` instruções para ser gerado automaticamente para nós. Depois de concluir o assistente, podemos pode, em seguida, atualizar manualmente o TableAdapter s `SelectCommand` na janela Propriedades para que ela inclua o `JOIN` sintaxe.

Embora essa abordagem funciona, é muito frágil ao usar consultas SQL ad hoc porque sempre que a consulta principal do TableAdapter s é configurado novamente por meio do assistente, geradas automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções são recriadas. Isso significa que todas as personalizações feitas posteriormente seriam perdidas se podemos pequeno no TableAdapter, escolher configurar o menu de contexto e concluir o assistente novamente.

Fragilidade da s TableAdapter geradas automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções for, Felizmente, limitado a instruções SQL ad hoc. Se seu TableAdapter usa procedimentos armazenados, você pode personalizar o `SelectCommand`, `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` procedimentos armazenados e execute novamente o Assistente de configuração do TableAdapter sem ter que temem que serão os procedimentos armazenados modificado.

Ao longo dos próximos várias etapas, criaremos um TableAdapter que, inicialmente, usa uma consulta principal que omite qualquer `JOIN` s, de modo que correspondente inserir, atualizar e procedimentos armazenado de exclusão será gerado automaticamente. Em seguida, atualizaremos o `SelectCommand` assim que usa um `JOIN` que retorna mais colunas de tabelas relacionadas. Por fim, vamos criar uma classe de camada de lógica comercial correspondente e demonstram como usar o TableAdapter em uma página da web ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Etapa 1: Criar o TableAdapter usando uma consulta principal simplificada

Para este tutorial, adicionaremos um TableAdapter e uma DataTable fortemente tipada para o `Employees` na tabela a `NorthwindWithSprocs` conjunto de dados. O `Employees` tabela contém uma `ReportsTo` campo que especificou o `EmployeeID` do gerente do funcionário s. Por exemplo, o funcionário Anne Dodsworth tem um `ReportTo` valor de 5, que é o `EmployeeID` de Steven Buchanan. Consequentemente, Anne reporta a Steven, seu gerente. Juntamente com relatórios de cada funcionário s `ReportsTo` valor, também queremos recuperar o nome do seu gerente. Isso pode ser feito usando um `JOIN`. Mas o uso um `JOIN` quando criar inicialmente o TableAdapter impede o Assistente de geração automática de inserção correspondente, atualizar e excluir recursos. Portanto, vamos começar criando um TableAdapter cuja consulta principal não contém nenhum `JOIN` s. Em seguida, na etapa 2, atualizaremos o procedimento armazenado de consulta principal para recuperar o nome do gerente s por meio de um `JOIN`.

Comece abrindo o `NorthwindWithSprocs` conjunto de dados a `~/App_Code/DAL` pasta. Com o botão direito no Designer, selecione a opção Adicionar no menu de contexto e escolha o item de menu do TableAdapter. Isso iniciará o Assistente de configuração do TableAdapter. Conforme mostra a Figura 5, que o assistente criar novos procedimentos armazenados e clique em Avançar. Para relembrar criando novos procedimentos do Assistente de s TableAdapter de armazenados, consulte o [criando novos procedimentos armazenados para o s TableAdapters do conjunto de dados tipado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) tutorial.


[![Selecione os procedimentos armazenados nova opção de criar](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Figura 5**: selecione Criar novo armazenados procedimentos opção ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))


Use o seguinte `SELECT` instrução da consulta principal do TableAdapter s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Uma vez que essa consulta não inclui nenhum `JOIN` s, o TableAdapter assistente criará automaticamente os procedimentos armazenados com correspondente `INSERT`, `UPDATE`, e `DELETE` instruções, bem como um procedimento armazenado para a execução de principal consulta.

A seguinte etapa nos permite nomear os procedimentos de s armazenados TableAdapter. Use os nomes `Employees_Select`, `Employees_Insert`, `Employees_Update`, e `Employees_Delete`, conforme mostrado na Figura 6.


[![Os procedimentos do TableAdapter s armazenados de nome](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Figura 6**: nomear os procedimentos armazenados do TableAdapter s ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))


A etapa final nos solicita para nomear os métodos do TableAdapter s. Use `Fill` e `GetEmployees` como os nomes de método. Também Certifique-se de deixar os crie métodos para enviar atualizações diretamente para a caixa de seleção de banco de dados (GenerateDBDirectMethods) marcada.


[![Nome do preenchimento de métodos do TableAdapter s e GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Figura 7**: nomear os métodos do TableAdapter s `Fill` e `GetEmployees` ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))


Depois de concluir o assistente, reserve um tempo para examinar os procedimentos armazenados no banco de dados. Você deverá ver quatro novas: `Employees_Select`, `Employees_Insert`, `Employees_Update`, e `Employees_Delete`. Em seguida, inspecione a `EmployeesDataTable` e `EmployeesTableAdapter` acabou de criar. A DataTable contém uma coluna para cada campo retornado pela consulta principal. Clique no TableAdapter e, em seguida, vá para a janela de propriedades. Lá você verá que o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades estão configuradas corretamente para chamar os procedimentos armazenados correspondentes.


[![O TableAdapter inclui Insert, Update e excluir recursos](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Figura 8**: O TableAdapter inclui inserir, atualizar e excluir recursos ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))


Com a inserção, atualização e exclusão de procedimentos armazenados criados automaticamente e o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades configuradas corretamente, você está pronto para personalizar o `SelectCommand` s procedimento armazenado para retornar adicionais informações sobre cada gerente do funcionário s. Especificamente, precisamos atualizar o `Employees_Select` procedimento armazenado para usar um `JOIN` e retornar o Gerenciador de s `FirstName` e `LastName` valores. Depois que o procedimento armazenado tiver sido atualizado, precisamos atualizar a DataTable para que ele inclua essas colunas adicionais. Que abordaremos essas duas tarefas nas etapas 2 e 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Etapa 2: Personalizar o procedimento armazenado para incluir um`JOIN`

Comece indo para o Gerenciador de servidores, drill-down até a pasta de procedimentos armazenados do banco de dados s Northwind e abrindo o `Employees_Select` procedimento armazenado. Se você não vir esse procedimento armazenado, clique com botão direito na pasta de procedimentos armazenados e escolha atualizar. Atualize o procedimento armazenado para que ele use um `LEFT JOIN` para retornar o Gerenciador de s primeiro nome e sobrenome:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

Depois de atualizar o `SELECT` instrução, salve as alterações no menu Arquivo e escolhendo Salvar `Employees_Select`. Como alternativa, você pode clique no ícone Salvar na barra de ferramentas ou pressione Ctrl + S. Depois de salvar suas alterações, clique com botão direito no `Employees_Select` procedimento armazenado no Gerenciador de servidores e escolha Executar. Isso irá executar o procedimento armazenado e mostrar seus resultados na janela de saída (veja a Figura 9).


[![Os resultados de procedimentos armazenados são exibidos na janela de saída](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Figura 9**: os resultados de procedimentos armazenados são exibidos na janela de saída ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Etapa 3: Atualizar as colunas de s de DataTable

Neste ponto, o `Employees_Select` procedimento armazenado retorna `ManagerFirstName` e `ManagerLastName` valores, mas o `EmployeesDataTable` está faltando a essas colunas. Essas colunas ausentes podem ser adicionadas à tabela de dados em uma das duas maneiras:

- **Manualmente** - o com o botão direito na DataTable no DataSet Designer e, no menu Adicionar, escolha a coluna. Em seguida, você pode nomear a coluna e definir suas propriedades de acordo.
- **Automaticamente** -o Assistente de configuração do TableAdapter atualizará as colunas de s DataTable para refletir os campos retornados pela `SelectCommand` procedimento armazenado. Ao usar instruções SQL ad hoc, o assistente também removerá o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades desde o `SelectCommand` agora contém um `JOIN`. Mas, ao usar procedimentos armazenados, essas propriedades de comando permanecem intactas.

Exploramos manualmente adicionando colunas de tabela de dados nos tutoriais anteriores, incluindo [mestre/detalhes usando uma lista com marcadores de registros de mestre com um DataList de detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) e [carregar arquivos](../working-with-binary-files/uploading-files-cs.md), e iremos Examine esse processo novamente em mais detalhes em nosso próximo tutorial. Para este tutorial, porém, deixe s use a abordagem automática por meio do Assistente de configuração do TableAdapter.

Comece clicando com o `EmployeesTableAdapter` e selecionando configurar no menu de contexto. Isso abre o Assistente de configuração do TableAdapter, que lista os procedimentos armazenados usados para selecionar, inserir, atualizar e excluir, junto com seus valores de retorno e parâmetros (se houver). Figura 10 mostra esse assistente. Aqui podemos ver que o `Employees_Select` procedimento armazenado agora retorna o `ManagerFirstName` e `ManagerLastName` campos.


[![Procedimento de armazenado mostra o Assistente para a lista de coluna atualizada para o Employees_Select](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Figura 10**: O assistente mostra a lista de coluna atualizada para o `Employees_Select` procedimento armazenado ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))


Conclua o assistente clicando em Finish. Ao retornar para o Designer de conjunto de dados, o `EmployeesDataTable` inclui duas colunas adicionais: `ManagerFirstName` e `ManagerLastName`.


[![O EmployeesDataTable contém duas novas colunas](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Figura 11**: O `EmployeesDataTable` contém duas novas colunas ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))


Para ilustrar que a atualização `Employees_Select` procedimento armazenado está em vigor e que a inserção, atualização e exclusão de recursos do TableAdapter são funcionais, permitir que o s crie uma página da web que permite aos usuários exibir e excluir os funcionários. Antes de criarmos uma página como essa, no entanto, é necessário primeiro criar uma nova classe na camada de lógica de negócios para trabalhar com os funcionários a `NorthwindWithSprocs` conjunto de dados. Na etapa 4, criaremos um `EmployeesBLLWithSprocs` classe. Na etapa 5, usaremos essa classe em uma página ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Etapa 4: Implementando a camada de lógica de negócios

Criar um novo arquivo de classe na `~/App_Code/BLL` pasta chamada `EmployeesBLLWithSprocs.cs`. Essa classe imita a semântica de existente `EmployeesBLL` classe, somente esse novo uma fornece menos métodos e usa o `NorthwindWithSprocs` conjunto de dados (em vez do `Northwind` conjunto de dados). Adicione o seguinte código para o `EmployeesBLLWithSprocs` classe.


[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

O `EmployeesBLLWithSprocs` classe s `Adapter` propriedade retorna uma instância das `NorthwindWithSprocs` DataSet s `EmployeesTableAdapter`. Isso é usado pela classe s `GetEmployees` e `DeleteEmployee` métodos. O `GetEmployees` chamadas de método de `EmployeesTableAdapter` s correspondente `GetEmployees` método, que invoca o `Employees_Select` procedimento armazenado e preenche seus resultados em um `EmployeeDataTable`. O `DeleteEmployee` chamadas de método da mesma forma a `EmployeesTableAdapter` s `Delete` método, que invoca o `Employees_Delete` procedimento armazenado.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Etapa 5: Trabalhar com os dados na camada de apresentação

Com o `EmployeesBLLWithSprocs` classe concluída, podemos está pronto para trabalhar com dados de funcionários por meio de uma página ASP.NET. Abra o `JOINs.aspx` página na `AdvancedDAL` pasta e arraste um controle GridView da caixa de ferramentas para o Designer, definindo seu `ID` propriedade para `Employees`. Em seguida, associar a grade para um novo controle ObjectDataSource chamado de GridView s marca inteligente, `EmployeesDataSource`.

Configurar o ObjectDataSource para usar o `EmployeesBLLWithSprocs` de classe e, nas guias SELECT e DELETE, certifique-se de que o `GetEmployees` e `DeleteEmployee` métodos são selecionados das listas de lista suspensa. Clique em Concluir para concluir a configuração de s ObjectDataSource.


[![Configurar o ObjectDataSource para usar a classe EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Figura 12**: configurar o ObjectDataSource para usar o `EmployeesBLLWithSprocs` classe ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))


[![Poderá usar o ObjectDataSource os métodos de DeleteEmployee e GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Figura 13**: pode usar o ObjectDataSource a `GetEmployees` e `DeleteEmployee` métodos ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))


Visual Studio adicionará um BoundField ao GridView para cada um do `EmployeesDataTable` colunas s. Remover todos esses BoundFields, exceto `Title`, `LastName`, `FirstName`, `ManagerFirstName`, e `ManagerLastName` e renomeie o `HeaderText` propriedades para os últimos quatro BoundFields sobrenome, nome, nome Manager s, e Gerenciador de s Last Name, respectivamente.

Para permitir que os usuários excluam os funcionários dessa página que precisamos fazer duas coisas. Primeiro, instrua o GridView para fornecer recursos de exclusão, marcando a opção de habilitar a exclusão de sua marca inteligente. Em segundo lugar, altere o s ObjectDataSource `OldValuesParameterFormatString` propriedade do valor definidos pelo assistente ObjectDataSource (`original_{0}`) para seu valor padrão (`{0}`). Depois de fazer essas alterações, sua marcação declarativa de s do GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Testar a página visitando-lo por meio de um navegador. Como mostra a Figura 14, a página listará cada funcionário e seu nome do s Gerenciador (supondo que eles tenham um).


[![A junção no Employees_Select procedimento armazenado retorna o nome do Gerenciador de s](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Figura 14**: O `JOIN` na `Employees_Select` procedimento armazenado retorna o nome do Gerenciador ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))


Clicando no botão Excluir inicia o fluxo de trabalho excluindo, culmina na execução do `Employees_Delete` procedimento armazenado. No entanto, a tentativa `DELETE` instrução no procedimento armazenado falhará devido a uma violação de restrição de chave estrangeira (consulte a Figura 15). Especificamente, cada funcionário tem um ou mais registros no `Orders` tabela, fazendo com que a exclusão falha.


[![Exclusão de um funcionário que tem resultados correspondentes de pedidos em uma violação de restrição de chave estrangeira](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Figura 15**: exclusão de um funcionário que tem resultados correspondentes de pedidos em uma violação de restrição de chave estrangeira ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))


Para permitir que um funcionário para ser excluído, você pode:

- Atualizar a restrição de chave estrangeira para propagar exclusões,
- Exclua manualmente os registros da `Orders` tabela para o funcionário (s) que deseja excluir, ou
- Atualização do `Employees_Delete` procedimento armazenado para primeiro excluir os registros relacionados do `Orders` tabela antes de excluir o `Employees` registro. Abordamos essa técnica na [usando procedimentos armazenados existentes para o s TableAdapters do conjunto de dados tipado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) tutorial.

Posso deixar isso como um exercício para o leitor.

## <a name="summary"></a>Resumo

Ao trabalhar com bancos de dados relacionais, é comum para consultas efetuar pull de seus dados de várias tabelas relacionadas. Subconsultas correlacionadas e `JOIN` s fornecem duas técnicas diferentes para acessar dados de tabelas relacionadas em uma consulta. Nos tutoriais anteriores, fizemos mais comumente usam de subconsultas correlacionadas porque o TableAdapter não é possível gerar automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções para consultas que envolvem `JOIN` s. Embora esses valores podem ser fornecidos manualmente, ao usar instruções SQL ad hoc todas as personalizações serão substituídas quando o Assistente de configuração do TableAdapter é concluído.

Felizmente, TableAdapters criados usando procedimentos armazenados não sofrem das mesma fragilidade como aqueles criados usando instruções SQL ad hoc. Portanto, é possível criar um TableAdapter cuja consulta principal usa um `JOIN` ao usar procedimentos armazenados. Neste tutorial vimos como criar tal um TableAdapter. Começamos usando um `JOIN`-menos `SELECT` consulta para a consulta principal do TableAdapter s para que os correspondente insert, update e procedimentos armazenado de delete seria criado automaticamente. Com a TableAdapter s configuração inicial completa, aumentássemos o `SelectCommand` procedimento armazenado para usar um `JOIN` e executar novamente o Assistente de configuração do TableAdapter para atualizar o `EmployeesDataTable` colunas s.

Executar novamente o Assistente de configuração do TableAdapter atualizado automaticamente a `EmployeesDataTable` colunas para refletir os campos de dados retornados pelo `Employees_Select` procedimento armazenado. Como alternativa, podemos poderia ter adicionado essas colunas manualmente à tabela de dados. Vamos explorar manualmente adicionando colunas à tabela de dados no próximo tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Teresa Murphy, David Suru e Geisenow Hilton. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [Próximo](adding-additional-datatable-columns-cs.md)
