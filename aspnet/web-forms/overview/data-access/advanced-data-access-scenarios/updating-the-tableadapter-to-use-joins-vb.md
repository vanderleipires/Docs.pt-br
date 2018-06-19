---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
title: Atualizar o TableAdapter usar junções (VB) | Microsoft Docs
author: rick-anderson
description: Ao trabalhar com um banco de dados é comum para dados de solicitação que são distribuídos entre várias tabelas. Para recuperar dados de duas tabelas diferentes, pode usar qualquer um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: e624a3e0-061b-4efc-8b0e-5877f9ff6714
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-vb
msc.type: authoredcontent
ms.openlocfilehash: 91d700f3de02dc78692e933644e221e2ac8175a1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878302"
---
<a name="updating-the-tableadapter-to-use-joins-vb"></a>Atualizar o TableAdapter usar junções (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_VB.zip) ou [baixar PDF](updating-the-tableadapter-to-use-joins-vb/_static/datatutorial69vb1.pdf)

> Ao trabalhar com um banco de dados é comum para dados de solicitação que são distribuídos entre várias tabelas. Para recuperar dados de duas tabelas diferentes, pode usar uma subconsulta correlacionada ou uma operação de junção. Neste tutorial comparamos subconsultas correlacionadas e a sintaxe de junção antes de examinar como criar um TableAdapter que inclui uma junção em sua consulta principal.


## <a name="introduction"></a>Introdução

Com bancos de dados relacionais estamos interessados em trabalhar com dados geralmente são distribuídos entre várias tabelas. Por exemplo, ao exibir informações sobre o produto provavelmente queremos lista cada categoria do produto s correspondente e nomes de s do fornecedor. O `Products` tabela tem `CategoryID` e `SupplierID` valores, mas os nomes de categoria e fornecedor reais estão no `Categories` e `Suppliers` tabelas, respectivamente.

Para recuperar informações de outra tabela relacionada, podemos usar *subconsultas correlacionadas* ou `JOIN` *s*. Uma subconsulta correlacionada é uma construção `SELECT` consulta que faz referência a colunas na consulta externa. Por exemplo, no [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-vb.md) tutorial, usamos duas subconsultas correlacionadas no `ProductsTableAdapter` a consulta principal s para retornar os nomes de categoria e fornecedor para cada produto. A `JOIN` é uma construção SQL que mescla as linhas relacionadas de duas tabelas diferentes. Usamos uma `JOIN` no [consultando dados com o controle SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb.md) tutorial para exibir informações de categoria ao lado de cada produto.

O motivo é ter abstained usa `JOIN` com os TableAdapters é devido a limitações no assistente TableAdapter s para gerar automaticamente correspondente `INSERT`, `UPDATE`, e `DELETE` instruções. Mais especificamente, se a consulta principal do TableAdapter s contiver quaisquer `JOIN` s, o TableAdapter não é possível criar automaticamente as instruções SQL ad hoc ou procedimentos armazenados para sua `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades.

Neste tutorial, podemos brevemente compara e contraste subconsultas correlacionadas e `JOIN` s antes de explorar a criação de um TableAdapter que inclui `JOIN` s em sua consulta principal.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Comparar e contrastar subconsultas correlacionadas e`JOIN` s

Lembre-se de que o `ProductsTableAdapter` criado no primeiro tutorial no `Northwind` conjunto de dados usa subconsultas correlacionadas trazer de volta cada nome de categoria e fornecedor de correspondente do produto s. O `ProductsTableAdapter` consulta principal s é mostrada abaixo.


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample1.sql)]

Os dois correlacionados subconsultas - `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` e `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` -são `SELECT` consultas que retornam um valor único por produto como uma coluna adicional no externa `SELECT` lista de colunas da instrução s.

Como alternativa, um `JOIN` pode ser usado para retornar cada nome de fornecedor e a categoria de produto s. A consulta a seguir retorna a mesma saída que o anterior, mas usa `JOIN` s em vez de subconsultas:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample2.sql)]

Um `JOIN` mescla os registros de uma tabela com registros de outra tabela com base em alguns critérios. Na consulta anterior, por exemplo, o `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` instrui o SQL Server para mesclar cada registro de produto com a categoria de registro cujo `CategoryID` valor corresponde ao produto s `CategoryID` valor. O resultado mesclado permite trabalhar com os campos de categoria correspondente para cada produto (como `CategoryName`).

> [!NOTE]
> `JOIN` s são usadas ao consultar dados de bancos de dados relacionais. Se você estiver familiarizado com o `JOIN` sintaxe ou necessidade de rever um pouco sobre seu uso, d recomendo o [SQL Join tutorial](http://www.w3schools.com/sql/sql_join.asp) em [W3 escolas](http://www.w3schools.com/). Também vale a pena leitura são o [ `JOIN` fundamentos](https://msdn.microsoft.com/library/ms191517.aspx) e [conceitos básicos de subconsulta](https://msdn.microsoft.com/library/ms189575.aspx) seções o [Manuais Online do SQL](https://msdn.microsoft.com/library/ms130214.aspx).


Como `JOIN` s e subconsultas correlacionadas podem ser usadas para recuperar dados relacionados de outras tabelas, muitos desenvolvedores são deixados pequena a cabeça e se perguntando qual abordagem usar. Todos as especialistas em SQL, var falou para disseram aproximadamente a mesma coisa, que ele t importa em termos de desempenho como o SQL Server irá gerar planos de execução praticamente idêntico. Seu conselho, em seguida, é usar a técnica que você e sua equipe estiverem mais familiarizados com o. Ele merece observar que após dar esse aviso esses especialistas imediatamente expressam suas preferências de `JOIN` s sobre subconsultas correlacionadas.

Ao criar uma camada de acesso de dados usando conjuntos de dados digitados, as ferramentas funcionam melhor quando o uso de subconsultas. Em particular, o Assistente de s TableAdapter não gerará automaticamente correspondente `INSERT`, `UPDATE`, e `DELETE` instruções se a consulta principal contiver qualquer `JOIN` s, mas gerará automaticamente essas instruções quando correlacionados subconsultas são usados.

Para explorar essa falha, criar um conjunto de dados tipado temporário no `~/App_Code/DAL` pasta. Durante o Assistente de configuração do TableAdapter, optar por usar instruções SQL ad hoc e insira o seguinte `SELECT` consulta (consulte a Figura 1):


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample3.sql)]


[![Insira uma consulta principal que contém associações](updating-the-tableadapter-to-use-joins-vb/_static/image2.png)](updating-the-tableadapter-to-use-joins-vb/_static/image1.png)

**Figura 1**: insira uma consulta principal que contém `JOIN` s ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image3.png))


Por padrão, o TableAdapter criará automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções com base na consulta principal. Se você clicar no botão Avançado, que você pode ver que esse recurso está habilitado. Apesar dessa configuração, o TableAdapter não será capaz de criar o `INSERT`, `UPDATE`, e `DELETE` instruções porque a consulta principal contém um `JOIN`.


![Insira uma consulta principal que contém associações](updating-the-tableadapter-to-use-joins-vb/_static/image4.png)

**Figura 2**: insira uma consulta principal que contém `JOIN` s


Clique em Concluir para concluir o assistente. Neste ponto o Designer de s do conjunto de dados incluirá um TableAdapter único com uma DataTable com colunas para cada um dos campos retornados no `SELECT` lista de colunas de consulta s. Isso inclui o `CategoryName` e `SupplierName`, como mostra a Figura 3.


![A tabela de dados inclui uma coluna para cada campo retornado na lista de colunas](updating-the-tableadapter-to-use-joins-vb/_static/image5.png)

**Figura 3**: DataTable inclui uma coluna para cada campo retornado na lista de colunas


Embora a DataTable tenha as colunas apropriadas, o TableAdapter não possui valores para seus `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades. Para confirmar isso, clique no TableAdapter no Designer e, em seguida, vá para a janela de propriedades. Lá você verá que o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades são definidas como (nenhum).


[![O InsertCommand, UpdateCommand e DeleteCommand propriedades são definidas como (nenhum)](updating-the-tableadapter-to-use-joins-vb/_static/image7.png)](updating-the-tableadapter-to-use-joins-vb/_static/image6.png)

**Figura 4**: O `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades são definidas como (nenhum) ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image8.png))


Para contornar este empecilho, pode fornecer manualmente as instruções SQL e os parâmetros para o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades por meio da janela Propriedades. Como alternativa, pode começar configurando a consulta principal do TableAdapter s para *não* incluir qualquer `JOIN` s. Isso permitirá que o `INSERT`, `UPDATE`, e `DELETE` instruções para ser gerado automaticamente para nós. Depois de concluir o assistente, nós pode atualizar manualmente o TableAdapter s `SelectCommand` na janela Propriedades, para que ela inclua o `JOIN` sintaxe.

Embora essa abordagem funciona, é muito frágil ao usar consultas SQL ad hoc porque sempre que a consulta do TableAdapter s principal é configurado novamente por meio do assistente, o gerado automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções são recriadas. Isso significa que todas as personalizações que fizemos posteriormente seriam perdidas se podemos pequeno no TableAdapter, escolher configurar no menu de contexto e concluir o assistente novamente.

Fragilidade da s TableAdapter geradas automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções é, Felizmente, limitadas a instruções SQL ad hoc. Se o TableAdapter usa procedimentos armazenados, você pode personalizar o `SelectCommand`, `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` procedimentos armazenados e execute novamente o Assistente de configuração do TableAdapter sem a necessidade de tema que serão os procedimentos armazenados modificado.

Sobre a próxima várias etapas, vamos criar um TableAdapter que, inicialmente, usa uma consulta principal que omite qualquer `JOIN` s para que o correspondente inserir, atualizar e excluir procedimentos será gerado automaticamente. Em seguida, atualizaremos o `SelectCommand` assim que usa um `JOIN` que retorna mais colunas de tabelas relacionadas. Por fim, vamos criar uma classe correspondente da camada de lógica comercial e demonstrar o uso de TableAdapter em uma página da web.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Etapa 1: Criando o TableAdapter usando uma consulta principal simplificada

Para este tutorial adicionaremos um TableAdapter e fortemente tipadas DataTable para o `Employees` tabela o `NorthwindWithSprocs` conjunto de dados. O `Employees` tabela contém um `ReportsTo` campo que especificou o `EmployeeID` do Gerenciador de funcionário s. Por exemplo, o funcionário Anne Dodsworth tem um `ReportTo` valor de 5, que é o `EmployeeID` de Steven Buchanan. Consequentemente, Anne relata Steven, seu gerente. Junto com o relatório de cada funcionário s `ReportsTo` valor, podemos também poderá recuperar o nome do seu gerente. Isso pode ser feito usando um `JOIN`. Mas usando um `JOIN` quando criar inicialmente o TableAdapter impede o Assistente de geração automática da inserção correspondente, atualizar e excluir recursos. Portanto, vamos começar criando um TableAdapter cuja consulta principal não contém nenhum `JOIN` s. Em seguida, na etapa 2, atualizaremos o procedimento armazenado de consulta principal para recuperar o nome do gerente s por meio de um `JOIN`.

Comece abrindo o `NorthwindWithSprocs` conjunto de dados a `~/App_Code/DAL` pasta. Com o botão direito no Designer, selecione a opção de adicionar no menu de contexto e selecione o item de menu do TableAdapter. Isso iniciará o Assistente de configuração do TableAdapter. Como mostra a Figura 5, que o assistente crie novos procedimentos armazenados e clique em Avançar. Para uma atualização sobre a criação de novos procedimentos armazenados de TableAdapter wizard s, consulte o [criando novos procedimentos armazenados para o conjunto de dados tipado de s TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial.


[![Selecione os criar novos procedimentos armazenados opção](updating-the-tableadapter-to-use-joins-vb/_static/image10.png)](updating-the-tableadapter-to-use-joins-vb/_static/image9.png)

**Figura 5**: selecione a criar novos armazenados procedimentos opção ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image11.png))


Use a seguinte `SELECT` instrução da consulta principal do TableAdapter s:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample4.sql)]

Desde que esta consulta não incluem nenhum `JOIN` s, o assistente TableAdapter criará automaticamente os procedimentos armazenados com correspondente `INSERT`, `UPDATE`, e `DELETE` instruções, bem como um procedimento armazenado para execução principal consulta.

A seguinte etapa permite nomear os procedimentos de s armazenados TableAdapter. Use os nomes `Employees_Select`, `Employees_Insert`, `Employees_Update`, e `Employees_Delete`, conforme mostrado na Figura 6.


[![Os procedimentos do TableAdapter s armazenados de nome](updating-the-tableadapter-to-use-joins-vb/_static/image13.png)](updating-the-tableadapter-to-use-joins-vb/_static/image12.png)

**Figura 6**: nome de procedimentos armazenados do TableAdapter s ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image14.png))


A etapa final solicita para nomear os métodos TableAdapter. Use `Fill` e `GetEmployees` como os nomes de método. Também Certifique-se de deixar os crie métodos para enviar atualizações diretamente para a caixa de seleção de banco de dados (GenerateDBDirectMethods) marcada.


[![Nome do preenchimento de métodos TableAdapter s e GetEmployees](updating-the-tableadapter-to-use-joins-vb/_static/image16.png)](updating-the-tableadapter-to-use-joins-vb/_static/image15.png)

**Figura 7**: nomear os métodos TableAdapter s `Fill` e `GetEmployees` ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image17.png))


Depois de concluir o assistente, dedique alguns momentos para examinar os procedimentos armazenados no banco de dados. Você deve ver quatro novas: `Employees_Select`, `Employees_Insert`, `Employees_Update`, e `Employees_Delete`. Em seguida, inspecione o `EmployeesDataTable` e `EmployeesTableAdapter` acabou de criar. A tabela de dados contém uma coluna para cada campo retornado pela consulta principal. Clique no TableAdapter e, em seguida, vá para a janela de propriedades. Lá você verá que o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades estão configuradas corretamente para chamar os procedimentos armazenados correspondentes.


[![O TableAdapter inclui instruções Insert, Update e excluir recursos](updating-the-tableadapter-to-use-joins-vb/_static/image19.png)](updating-the-tableadapter-to-use-joins-vb/_static/image18.png)

**Figura 8**: O TableAdapter inclui inserir, atualizar e excluir recursos ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image20.png))


Com a inserção, atualização e exclusão de procedimentos armazenados criados automaticamente e o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades configuradas corretamente, você está pronto para personalizar o `SelectCommand` s procedimento armazenado para retornar adicionais informações sobre cada gerente do funcionário s. Especificamente, precisamos atualizar o `Employees_Select` procedimento armazenado para usar um `JOIN` e retornar o Gerenciador de s `FirstName` e `LastName` valores. Depois que o procedimento armazenado foi atualizado, precisamos atualizar o DataTable para que ela inclua essas colunas adicionais. Que abordaremos essas duas tarefas nas etapas 2 e 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Etapa 2: Personalizar o procedimento armazenado para incluir um`JOIN`

Iniciar indo para o Gerenciador de servidores, drill-down até a pasta de procedimentos armazenados do banco de dados s Northwind e abrindo o `Employees_Select` procedimento armazenado. Se você não vir esse procedimento armazenado, com o botão direito na pasta de procedimentos armazenados e escolha atualizar. Atualize o procedimento armazenado para que ele usa um `LEFT JOIN` para retornar o Gerenciador de s primeiro e último nome:


[!code-sql[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample5.sql)]

Depois de atualizar o `SELECT` instrução, salve as alterações no menu Arquivo e escolhendo Salvar `Employees_Select`. Como alternativa, você pode clique no ícone Salvar na barra de ferramentas ou pressione Ctrl + S. Depois de salvar suas alterações, clique duas vezes no `Employees_Select` procedimento armazenado no Gerenciador de servidores e escolha Executar. Isso irá executar o procedimento armazenado e mostrar os resultados na janela de saída (consulte a Figura 9).


[![Os resultados de procedimentos armazenados são exibidos na janela de saída](updating-the-tableadapter-to-use-joins-vb/_static/image22.png)](updating-the-tableadapter-to-use-joins-vb/_static/image21.png)

**Figura 9**: os resultados de procedimentos armazenados são exibidos na janela de saída ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image23.png))


## <a name="step-3-updating-the-datatable-s-columns"></a>Etapa 3: Atualizar as colunas de s DataTable

Neste ponto, o `Employees_Select` procedimento armazenado retorna `ManagerFirstName` e `ManagerLastName` valores, mas o `EmployeesDataTable` está faltando a essas colunas. Essas colunas ausentes podem ser adicionadas à tabela de dados em uma das duas maneiras:

- **Manualmente** - o com o botão direito na DataTable no Designer de conjunto de dados e, no menu Adicionar, escolha a coluna. Em seguida, você pode nomear a coluna e definir suas propriedades de acordo.
- **Automaticamente** -o Assistente de configuração do TableAdapter atualizará as colunas de s DataTable para refletir os campos retornados pelo `SelectCommand` procedimento armazenado. Ao usar instruções SQL ad hoc, o assistente também removerá o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` propriedades desde o `SelectCommand` agora contém um `JOIN`. Mas, ao usar procedimentos armazenados, essas propriedades de comando permanecem intactas.

Podemos ter explorado manualmente adicionando colunas da DataTable tutoriais anteriores, incluindo [mestre/detalhes usando uma lista com marcadores de registros de mestre com DataList detalhes](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) e [carregando arquivos](../working-with-binary-files/uploading-files-vb.md), e vamos Examine esse processo novamente em mais detalhes em nosso tutorial Avançar. Para este tutorial, no entanto, permitem s a abordagem automática por meio do Assistente de configuração do TableAdapter.

Iniciar clicando no `EmployeesTableAdapter` e configurar a seleção no menu de contexto. Isso abre o Assistente de configuração do TableAdapter, que lista os procedimentos armazenados usados para selecionar, inserir, atualizar e excluir, junto com seus valores de retorno e parâmetros (se houver). A Figura 10 mostra este assistente. Aqui, podemos ver que o `Employees_Select` procedimento armazenado agora retorna o `ManagerFirstName` e `ManagerLastName` campos.


[![Procedimento de armazenado mostra o Assistente para a lista de coluna atualizada para o Employees_Select](updating-the-tableadapter-to-use-joins-vb/_static/image25.png)](updating-the-tableadapter-to-use-joins-vb/_static/image24.png)

**Figura 10**: O assistente mostra a lista de coluna atualizada para o `Employees_Select` procedimento armazenado ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image26.png))


Conclua o assistente clicando em Concluir. Ao retornar ao Designer de conjunto de dados, o `EmployeesDataTable` inclui duas colunas adicionais: `ManagerFirstName` e `ManagerLastName`.


[![O EmployeesDataTable contém duas novas colunas](updating-the-tableadapter-to-use-joins-vb/_static/image28.png)](updating-the-tableadapter-to-use-joins-vb/_static/image27.png)

**Figura 11**: O `EmployeesDataTable` contém duas novas colunas ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image29.png))


Para ilustrar que a atualização `Employees_Select` procedimento armazenado está em vigor e que a inserção, atualização e exclusão de recursos do TableAdapter sejam funcionais, permitem que o s criar uma página da web que permite aos usuários exibir e excluir os funcionários. Antes de criar esse tipo de página, no entanto, é preciso primeiro criar uma nova classe na camada de lógica de negócios para trabalhar com os funcionários a `NorthwindWithSprocs` conjunto de dados. Na etapa 4, criaremos um `EmployeesBLLWithSprocs` classe. Na etapa 5, usaremos esta classe de uma página ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Etapa 4: Implementando a camada de lógica de negócios

Criar um novo arquivo de classe no `~/App_Code/BLL` pasta denominada `EmployeesBLLWithSprocs.vb`. Essa classe imita a semântica de existente `EmployeesBLL` classe, somente essa nova uma fornece menos métodos e usa o `NorthwindWithSprocs` conjunto de dados (em vez da `Northwind` conjunto de dados). Adicione o seguinte código para o `EmployeesBLLWithSprocs` classe.


[!code-vb[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample6.vb)]

O `EmployeesBLLWithSprocs` classe s `Adapter` propriedade retorna uma instância do `NorthwindWithSprocs` conjunto de dados s `EmployeesTableAdapter`. Isso é usado pela classe s `GetEmployees` e `DeleteEmployee` métodos. O `GetEmployees` chamadas de método de `EmployeesTableAdapter` s correspondente `GetEmployees` método, que invoca o `Employees_Select` procedimento armazenado e preenche os resultados em um `EmployeeDataTable`. O `DeleteEmployee` chamadas de método da mesma forma a `EmployeesTableAdapter` s `Delete` método, que invoca o `Employees_Delete` procedimento armazenado.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Etapa 5: Trabalhando com os dados na camada de apresentação

Com o `EmployeesBLLWithSprocs` classe concluído, podemos re pronto para trabalhar com dados de funcionário em uma página ASP.NET. Abra o `JOINs.aspx` página o `AdvancedDAL` pasta e arraste um controle GridView da caixa de ferramentas para o Designer, definindo seu `ID` propriedade `Employees`. Em seguida, associar a grade para um novo controle ObjectDataSource chamado de GridView s marca inteligente, `EmployeesDataSource`.

Configurar o ObjectDataSource para usar o `EmployeesBLLWithSprocs` classe e, nas guias SELECT e DELETE, certifique-se de que o `GetEmployees` e `DeleteEmployee` métodos são selecionados das listas suspensas. Clique em Concluir para concluir a configuração de s ObjectDataSource.


[![Configurar o ObjectDataSource para usar a classe EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-vb/_static/image31.png)](updating-the-tableadapter-to-use-joins-vb/_static/image30.png)

**Figura 12**: configurar o ObjectDataSource para usar o `EmployeesBLLWithSprocs` classe ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image32.png))


[![Ter o uso de ObjectDataSource as GetEmployees e os métodos de DeleteEmployee](updating-the-tableadapter-to-use-joins-vb/_static/image34.png)](updating-the-tableadapter-to-use-joins-vb/_static/image33.png)

**Figura 13**: usar ObjectDataSource o `GetEmployees` e `DeleteEmployee` métodos ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image35.png))


O Visual Studio irá adicionar um BoundField a GridView para cada uma da `EmployeesDataTable` colunas s. Remova todos esses BoundFields exceto `Title`, `LastName`, `FirstName`, `ManagerFirstName`, e `ManagerLastName` e renomeie o `HeaderText` propriedades para os últimos quatro BoundFields sobrenome, nome, nome Manager s, e Gerenciador de s sobrenome, respectivamente.

Para permitir que os usuários excluam os funcionários dessa página, precisamos fazer duas coisas. Primeiro, instrua o GridView para fornecer recursos de exclusão, marcando a opção de habilitar a exclusão de marca inteligente. Em seguida, altere o s ObjectDataSource `OldValuesParameterFormatString` propriedade do valor definidos pelo assistente ObjectDataSource (`original_{0}`) para o valor padrão (`{0}`). Depois de fazer essas alterações, o GridView e ObjectDataSource s declarativo deve ser semelhante ao seguinte:


[!code-aspx[Main](updating-the-tableadapter-to-use-joins-vb/samples/sample7.aspx)]

A página de teste visitando ele por meio de um navegador. Como mostra a Figura 14, a página lista cada funcionário e seu nome s manager (supondo que eles tenham um).


[![A associação no Employees_Select procedimento armazenado retorna o nome do gerente s](updating-the-tableadapter-to-use-joins-vb/_static/image37.png)](updating-the-tableadapter-to-use-joins-vb/_static/image36.png)

**Figura 14**: O `JOIN` no `Employees_Select` procedimento armazenado retorna o Gerenciador de s nome ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image38.png))


Clicando no botão Excluir inicia o fluxo de trabalho excluindo, com a execução do `Employees_Delete` procedimento armazenado. No entanto, a tentativa `DELETE` instrução no procedimento armazenado falha devido a uma violação de restrição de chave estrangeira (consulte a Figura 15). Especificamente, cada funcionário tem um ou mais registros `Orders` tabela, fazendo com que a exclusão falha.


[![Excluindo um funcionário que tem os resultados correspondentes de ordens em uma violação de restrição de chave estrangeira](updating-the-tableadapter-to-use-joins-vb/_static/image40.png)](updating-the-tableadapter-to-use-joins-vb/_static/image39.png)

**Figura 15**: exclusão de um funcionário que tem os resultados correspondentes de ordens em uma violação de restrição de chave estrangeira ([clique para exibir a imagem em tamanho normal](updating-the-tableadapter-to-use-joins-vb/_static/image41.png))


Para permitir que um funcionário para ser excluído, você pode:

- A restrição de chave estrangeira em cascata exclusões de atualização
- Exclua manualmente os registros da `Orders` tabela para o funcionário (s) que deseja excluir, ou
- Atualização de `Employees_Delete` primeiro excluir os registros relacionados do procedimento armazenado a `Orders` tabela antes de excluir o `Employees` registro. Discutimos dessa técnica a [usando existente procedimentos armazenados para o conjunto de dados tipado de s TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial.

Posso deixar isso como um exercício para o leitor.

## <a name="summary"></a>Resumo

Ao trabalhar com bancos de dados relacionais, é comum para consultas extrair os dados de várias tabelas relacionadas. Subconsultas correlacionadas e `JOIN` s fornecem duas técnicas diferentes para acessar dados de tabelas relacionadas em uma consulta. Nos tutoriais anteriores fizemos mais usam de subconsultas correlacionadas porque o TableAdapter não é possível gerar automaticamente `INSERT`, `UPDATE`, e `DELETE` instruções para consultas que envolvem `JOIN` s. Embora esses valores podem ser fornecidos manualmente, ao usar instruções SQL ad hoc todas as personalizações serão substituídas quando concluir o Assistente de configuração do TableAdapter.

Felizmente, TableAdapters criados usando procedimentos armazenados não tem a mesmo fragilidade como aqueles criados usando instruções SQL ad hoc. Portanto, é possível criar um TableAdapter cuja consulta principal usa um `JOIN` ao usar procedimentos armazenados. Neste tutorial, vimos como criar esse um TableAdapter. Começamos usando um `JOIN`-menos `SELECT` consulta para a consulta do TableAdapter s principal para que o correspondente insert, update e os procedimentos armazenado de exclusão será criado automaticamente. O TableAdapter s configuração inicial completa, é aumentada a `SelectCommand` procedimento armazenado para usar um `JOIN` e executar novamente o Assistente de configuração do TableAdapter para atualizar o `EmployeesDataTable` colunas s.

Executar novamente o Assistente de configuração do TableAdapter atualizado automaticamente a `EmployeesDataTable` colunas para refletir os campos de dados retornados pelo `Employees_Select` procedimento armazenado. Como alternativa, poderia adicionamos essas colunas manualmente à tabela de dados. Iremos explorar manualmente adicionando colunas à tabela de dados do tutorial Avançar.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Hilton Geisenow, David Suru e Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Próximo](adding-additional-datatable-columns-vb.md)
