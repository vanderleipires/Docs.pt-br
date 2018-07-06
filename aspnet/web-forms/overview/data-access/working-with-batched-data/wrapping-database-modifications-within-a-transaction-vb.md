---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: Encapsulamento de modificações de banco de dados em uma transação (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial é o primeiro de quatro que examina a atualização, exclusão e inserção de lotes de dados. Neste tutorial, saiba como permitem transações de banco de dados...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 877174bad08970eed0cab52d0f1d8a521f7d2cc0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840739"
---
<a name="wrapping-database-modifications-within-a-transaction-vb"></a>Encapsulamento de modificações de banco de dados em uma transação (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip) ou [baixar PDF](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> Este tutorial é o primeiro de quatro que examina a atualização, exclusão e inserção de lotes de dados. Neste tutorial, saiba como as transações de banco de dados permitem que as modificações de lote a ser realizada como uma operação atômica, o que garante que todas as etapas de êxito ou falham de todas as etapas.


## <a name="introduction"></a>Introdução

Como vimos começando com o [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) tutorial, o GridView fornece suporte interno para o nível de linha de edição e exclusão. Com alguns cliques do mouse, é possível criar uma interface de modificação de dados avançados sem escrever uma linha de código, desde que você está o conteúdo com a edição e exclusão em uma base por linha. No entanto, em determinados cenários, isso é insuficiente e precisamos fornecer aos usuários a capacidade de editar ou excluir um lote de registros.

Por exemplo, mais clientes de email baseado na web usam uma grade para listar cada mensagem em que cada linha inclui uma caixa de seleção junto com as informações de s email (assunto, remetente e assim por diante). Essa interface permite que o usuário para excluir várias mensagens verificá-los e, em seguida, clicando em um botão Excluir mensagens selecionadas. Um lote de interface de edição é ideal em situações onde os usuários normalmente editar vários registros diferentes. Em vez de forçar o usuário clique em Editar, fazer suas alterações e, em seguida, clique em atualizar para cada registro que precisa ser modificado, um lote de interface de edição processa cada linha com sua interface de edição. O usuário pode modificar rapidamente o conjunto de linhas que precisam ser alteradas e, em seguida, salvar essas alterações, clicando em um botão Atualizar tudo. Esse conjunto de tutoriais, examinaremos como criar interfaces para inserção, edição e exclusão de lotes de dados.

Ao executar operações em lote-s importante para determinar se deve ser possível para algumas das operações no lote tenha êxito quando outro falhar. Considere um lote de exclusão de interface – o que deve acontecer se o primeiro registro selecionado é excluído com êxito, mas o outro falha, por exemplo, devido a uma violação de restrição de chave estrangeira? Deve primeiro excluir de registro s ser revertido ou é aceitável para o primeiro registro permanecer excluído?

Se desejar que a operação de lote deve ser tratado como um [operação atômica](http://en.wikipedia.org/wiki/Atomic_operation), um em que um dos ter êxito em todas as etapas ou todas as etapas falharem, a camada de acesso a dados precisa ser aumentada para incluir suporte para [banco de dados transações](http://en.wikipedia.org/wiki/Database_transaction). Transações de banco de dados garantem a atomicidade para o conjunto de `INSERT`, `UPDATE`, e `DELETE` instruções executadas sob o guarda-chuva da transação e são um recurso compatível com a maioria dos sistemas de banco de dados moderno todos os.

Neste tutorial, examinaremos como estender a DAL para usar transações de banco de dados. Tutoriais subsequentes examinará implementando páginas da web para o lote de inserção, atualização e exclusão de interfaces. Permitir que o s começar!

> [!NOTE]
> Ao modificar dados em uma transação em lote, atomicidade nem sempre é necessário. Em alguns cenários, pode ser aceitável ter algumas modificações de dados tenha êxito e outros no mesmo lote falharem, por exemplo, quando a exclusão de um conjunto de emails de um cliente de email baseado na web. Se não houver s um midway de erro de banco de dados por meio da exclusão processar, ele s provavelmente aceitável que os registros processados sem erro permaneçam excluídos. Nesses casos, a DAL não precisa ser modificado para dar suporte a transações de banco de dados. Há outros lote cenários de operação, no entanto, onde é vital a atomicidade. Quando um cliente move-lhe os fundos de uma conta bancária para outra, as duas operações devem ser executadas: os fundos devem ser deduzidos da primeira conta e, em seguida, adicionados ao segundo. Enquanto o banco não pode se importar com a primeira etapa ter êxito, mas a segunda etapa falhar, seus clientes compreensivelmente seria aborrecidos. Eu recomendo que você trabalhe com este tutorial e implementar os aprimoramentos para a DAL para dar suporte a transações de banco de dados, mesmo se você não planeja usá-los na inserção de lote, atualização e exclusão de interfaces, criarei nos tutoriais de três a seguir.


## <a name="an-overview-of-transactions"></a>Uma visão geral de transações

A maioria dos bancos de dados incluem suporte para *transações*, que permitem que vários comandos de banco de dados a ser agrupada em uma única unidade lógica de trabalho. Os comandos de banco de dados que compõem uma transação têm garantia de ser atômica, o que significa que todos os comandos falharão ou tudo será bem-sucedida.

Em geral, as transações são implementadas por meio de instruções SQL usando o seguinte padrão:

1. Indica o início de uma transação.
2. Execute as instruções SQL que compõem a transação.
3. Se houver um erro em uma das instruções da etapa 2, reverta a transação.
4. Se todas as instruções da etapa 2 concluída sem erro, confirme a transação.

As instruções SQL usadas para criar, confirmar e reverter a transação pode ser inserida manualmente quando procedimentos armazenados de escrever scripts SQL ou criando ou por meio de programação significa usar ADO.NET ou as classes de [ `System.Transactions` namespace](https://msdn.microsoft.com/library/system.transactions.aspx). Neste tutorial, que vamos examinar apenas gerenciamento de transações usando o ADO.NET. Um tutorial futuro vamos examinar como usar procedimentos armazenados na camada de acesso a dados, momento em que vamos explorar as instruções SQL para criar, reverter e confirmar transações. Enquanto isso, consulte [gerenciamento de transações em procedimentos armazenados do SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) para obter mais informações.

> [!NOTE]
> O [ `TransactionScope` classe](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) no `System.Transactions` namespace permite que os desenvolvedores por meio de programação encapsular uma série de instruções dentro do escopo de uma transação e inclui suporte para transações complexas que envolvem vários fontes, como dois bancos de dados diferentes ou até mesmo heterogêneos tipos de armazenamentos de dados, como um banco de dados do Microsoft SQL Server, um banco de dados Oracle e um serviço Web. Eu ve decidiu usar as transações do ADO.NET para este tutorial em vez do `TransactionScope` classe porque ADO.NET é mais específica para transações de banco de dados e, em muitos casos, é muito menor com uso intensivo de recursos. Além disso, em alguns cenários de `TransactionScope` classe usa o Microsoft Distributed Transaction coordenador (MSDTC). Os problemas de desempenho, implementação e configuração ao redor do MSDTC torna um tópico em vez disso, especializado e avançado e além do escopo destes tutoriais.


Ao trabalhar com o provedor do SqlClient no ADO.NET, as transações são iniciadas por meio de uma chamada para o [ `SqlConnection` classe](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), que retorna um [ `SqlTransaction` objeto](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). As instruções de modificação de dados que a transação de composição são colocados dentro de um `try...catch` bloco. Se ocorrer um erro em uma instrução no `try` bloquear transferências de execução para o `catch` bloco em que a transação pode ser revertida por meio de `SqlTransaction` objeto s [ `Rollback` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Se todas as instruções concluída com êxito, uma chamada para o `SqlTransaction` objeto s [ `Commit` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) no final o `try` bloco confirma a transação. O trecho de código a seguir ilustra esse padrão. Ver [manter a consistência de banco de dados com transações](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) para sintaxe adicional e exemplos do uso de transações com o ADO.NET.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

Por padrão, os TableAdapters em um DataSet tipado não usar transações. Para fornecer suporte para transações que precisamos para aumentar as classes TableAdapter para incluir métodos adicionais que usam o padrão acima para executar uma série de instruções de modificação de dados dentro do escopo de uma transação. Na etapa 2, veremos como usar as classes parciais para adicionar esses métodos.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Etapa 1: Criando o trabalho com páginas da Web de dados em lote

Antes de começarmos a explorar como incrementar a DAL para dar suporte a transações de banco de dados, permitem que s primeiro Reserve um tempo para criar as páginas da web ASP.NET que precisamos para este tutorial e as três seguintes. Comece adicionando uma nova pasta chamada `BatchData` e, em seguida, adicione as seguintes páginas do ASP.NET, associando cada página com o `Site.master` página mestra.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Adicione as páginas do ASP.NET para que os tutoriais relacionados SqlDataSource](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**Figura 1**: adicionar as páginas do ASP.NET para que os tutoriais relacionados SqlDataSource


Assim como acontece com as outras pastas `Default.aspx` usará o `SectionLevelTutorialListing.ascx` controle de usuário para listar os tutoriais dentro da própria seção. Portanto, adicionar esse controle de usuário `Default.aspx` arrastando-no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**Figura 2**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))


Por fim, adicione esses quatro páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após Personalizando o mapa do Site `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

Depois de atualizar `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para o trabalho com os tutoriais de dados em lote.


![O mapa do Site agora inclui entradas para o trabalho com os tutoriais de dados em lote](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**Figura 3**: O mapa do Site agora inclui entradas para o trabalho com os tutoriais de dados em lote


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Etapa 2: Atualizar a camada de acesso a dados para dar suporte a transações de banco de dados

Como discutimos no primeiro tutorial [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-vb.md), o conjunto de dados tipado em nossa DAL é composto de tabelas de dados e TableAdapters. As tabelas de dados contêm dados, enquanto os TableAdapters fornecem a funcionalidade para ler dados do banco de dados em tabelas de dados, para atualizar o banco de dados com as alterações feitas em tabelas de dados e assim por diante. Lembre-se de que os TableAdapters fornecem dois padrões para a atualização de dados, o que eu conhecido como atualização em lotes e diretos do banco de dados. Com o padrão de atualização em lotes, o TableAdapter é passado um DataSet, DataTable ou coleção de DataRows. Esses dados são enumerados e para cada inseridos, modificados ou excluídos de linha, o `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` é executado. Com o padrão de DB-Direct, o TableAdapter é passado em vez disso, os valores das colunas necessárias para inserção, atualização ou exclusão de um único registro. O método direto de banco de dados de padrão, em seguida, usa esses valores no passado para executar apropriado `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` instrução.

Independentemente do padrão de atualização usado, os métodos gerados automaticamente de TableAdapters não usam transações. Por padrão, cada inserção, atualização ou exclusão executadas pelo TableAdapter é tratado como uma única operação discreta. Por exemplo, imagine que o padrão de diretos do banco de dados é usado por algum código na BLL inserir dez registros no banco de dados. Esse código poderia chamar o TableAdapter s `Insert` método dez vezes. Se as primeiras cinco inserções tenha êxito, mas um sexto resultou em uma exceção, os cinco primeiros registros inseridos permanecerão no banco de dados. Da mesma forma, se o padrão de atualização em lotes é usado para executar inserções, atualizações e exclusões para inseridos, modificados e de linhas excluídas em uma DataTable, se o primeiro várias modificações foi bem-sucedida, mas um posterior encontrou um erro, essas modificações anteriores que concluídas permanecerão no banco de dados.

Em determinados cenários, queremos garantir a atomicidade em uma série de modificações. Para fazer isso, deve estender manualmente o TableAdapter, adicionando novos métodos que executam o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` s sob o guarda-chuva de uma transação. Na [criando uma camada de acesso de dados](../introduction/creating-a-data-access-layer-vb.md) analisamos usando [classes parciais](http://en.wikipedia.org/wiki/Partial_type) para estender a funcionalidade de tabelas de dados no conjunto de dados tipado. Essa técnica também pode ser usada com TableAdapters.

O conjunto de dados tipado `Northwind.xsd` está localizado em de `App_Code` pasta s `DAL` subpasta. Crie uma subpasta na `DAL` pasta chamada `TransactionSupport` e adicione um novo arquivo de classe chamado `ProductsTableAdapter.TransactionSupport.vb` (veja a Figura 4). Esse arquivo conterá a implementação parcial do `ProductsTableAdapter` que inclui métodos para executar modificações de dados usando uma transação.


![Adicionar uma pasta chamada TransactionSupport e um arquivo de classe chamado ProductsTableAdapter.TransactionSupport.vb](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**Figura 4**: Adicione uma pasta chamada `TransactionSupport` e um arquivo de classe chamado `ProductsTableAdapter.TransactionSupport.vb`


Digite o seguinte código para o `ProductsTableAdapter.TransactionSupport.vb` arquivo:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

O `Partial` palavra-chave na declaração da classe aqui indica ao compilador que os membros adicionados dentro devem ser adicionados para o `ProductsTableAdapter` classe no `NorthwindTableAdapters` namespace. Observação o `Imports System.Data.SqlClient` instrução na parte superior do arquivo. Uma vez que o TableAdapter foi configurado para usar o provedor SqlClient, internamente ele usa um [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) objeto emitir comandos ao banco de dados. Consequentemente, precisamos usar o `SqlTransaction` classe para iniciar a transação e, em seguida, para confirmá-la ou distribuí-lo novamente. Se você estiver usando um armazenamento de dados diferente do Microsoft SQL Server, você precisará usar o provedor apropriado.

Esses métodos fornecem os blocos de construção necessários para iniciar a reversão e confirmar uma transação. Eles são marcados `Public`, permitindo que eles a ser usado de dentro de `ProductsTableAdapter`, de outra classe DAL ou de outra camada na arquitetura, como a BLL. `BeginTransaction` Abre o TableAdapter s internos `SqlConnection` (se necessário), inicia a transação e a atribui a `Transaction` propriedade e anexa a transação para o interno `SqlDataAdapter` s `SqlCommand` objetos. `CommitTransaction` e `RollbackTransaction` chamar o `Transaction` objeto s `Commit` e `Rollback` métodos, respectivamente, antes de fechar o interno `Connection` objeto.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Etapa 3: Adicionando métodos para atualizar e excluir dados sob o guarda-chuva de uma transação

Com esses métodos é concluídos, podemos re está pronto para adicionar métodos para `ProductsDataTable` ou a BLL que executam uma série de comandos sob o guarda-chuva de uma transação. O método a seguir usa o padrão de atualização em lotes para atualizar um `ProductsDataTable` instância usando uma transação. Inicia uma transação chamando o `BeginTransaction` método e, em seguida, usa um `Try...Catch` bloco para emitir as instruções de modificação de dados. Se a chamada para o `Adapter` objeto s `Update` método resulta em uma exceção, a execução será transferido para o `catch` bloco em que a transação será revertida e a exceção gerada novamente. Lembre-se de que o `Update` método implementa o padrão de atualização em lotes, enumerando as linhas de fornecido `ProductsDataTable` e executar o necessário `InsertCommand`, `UpdateCommand`, e `DeleteCommand` s. Se qualquer um desses resultados de comandos em um erro, a transação será revertida, desfazendo as modificações anteriores feitas durante o tempo de vida de s de transação. Deve o `Update` instrução concluída sem erros, a transação é confirmada em sua totalidade.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

Adicione a `UpdateWithTransaction` método para o `ProductsTableAdapter` classe por meio da classe parcial no `ProductsTableAdapter.TransactionSupport.vb`. Como alternativa, esse método pode ser adicionado ao s a camada de lógica comercial `ProductsBLL` classe com algumas pequenas alterações sintáticas. Ou seja, a palavra-chave `Me` na `Me.BeginTransaction()`, `Me.CommitTransaction()`, e `Me.RollbackTransaction()` precisaria ser substituídos por `Adapter` (Lembre-se de que `Adapter` é o nome de uma propriedade em `ProductsBLL` do tipo `ProductsTableAdapter`).

O `UpdateWithTransaction` método usa o padrão de atualização em lotes, mas uma série de chamadas diretas de banco de dados também pode ser usada dentro do escopo de uma transação, como mostra o método a seguir. O `DeleteProductsWithTransaction` método aceita como entrada uma `List(Of T)` do tipo `Integer`, que são o `ProductID` s a serem excluídos. O método inicia a transação por meio de uma chamada para `BeginTransaction` e, em seguida, no `Try` blocos, itera através da lista fornecida chamando o padrão de DB-Direct `Delete` método para cada `ProductID` valor. Se qualquer uma das chamadas para `Delete` falhar, o controle é transferido para o `Catch` bloco em que a transação é revertida e a exceção gerada novamente. Se todas as chamadas para `Delete` bem-sucedida, em seguida, a transação é confirmada. Adicione este método para o `ProductsBLL` classe.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Aplicar transações entre vários TableAdapters

Permite que o código relacionadas à transação examinado neste tutorial para várias instruções em relação a `ProductsTableAdapter` seja tratado como uma operação atômica. Mas e se precisam ser executada atomicamente várias modificações às tabelas de banco de dados diferente? Por exemplo, ao excluir uma categoria, primeiro queremos reatribuir seus produtos atuais para alguma outra categoria. Essas duas etapas Reatribuindo os produtos e excluir a categoria devem ser executadas como uma operação atômica. Mas o `ProductsTableAdapter` inclui somente os métodos para modificar os `Products` tabela e o `CategoriesTableAdapter` inclui apenas os métodos para modificar o `Categories` tabela. Então, como uma transação pode incluir ambos os TableAdapters?

Uma opção é adicionar um método para o `CategoriesTableAdapter` chamado `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` e ter esse método a chamar um procedimento armazenado que reatribui os produtos e exclui a categoria de dentro do escopo de uma transação definida dentro do procedimento armazenado. Vamos examinar como begin, commit e rollback transações em procedimentos armazenados em um tutorial futuro.

Outra opção é criar uma classe auxiliar no DAL que contém o `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` método. Esse método cria uma instância das `CategoriesTableAdapter` e o `ProductsTableAdapter` e, em seguida, definir essas duas TableAdapters `Connection` propriedades ao mesmo `SqlConnection` instância. AT desse ponto, qualquer um dos dois TableAdapters iniciaria a transação com uma chamada para `BeginTransaction`. Os métodos de TableAdapters para reatribuir os produtos e a exclusão da categoria seriam invocados em um `Try...Catch` bloco com a transação confirmada ou revertida novamente conforme necessário.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Etapa 4: Adicionando o`UpdateWithTransaction`método para a camada de lógica de negócios

Na etapa 3, adicionamos uma `UpdateWithTransaction` método para o `ProductsTableAdapter` em DAL. Devemos acrescentar um método correspondente para a BLL. Embora a camada de apresentação pode chamar diretamente para baixo até a DAL para invocar o `UpdateWithTransaction` método, esses tutoriais tem strived definir uma arquitetura em camadas que isola o DAL na camada de apresentação. Portanto, cabe a nós para continuar essa abordagem.

Abra o `ProductsBLL` arquivo de classe e adicione um método chamado `UpdateWithTransaction` que simplesmente chama para baixo até o método correspondente da DAL. Agora deve haver dois novos métodos na `ProductsBLL`: `UpdateWithTransaction`, que você acabou de adicionar e `DeleteProductsWithTransaction`, que foi adicionado na etapa 3.


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> Esses métodos não incluem o `DataObjectMethodAttribute` atributo atribuído para a maioria dos outros métodos no `ProductsBLL` classe porque podemos invocar esses métodos diretamente as classes de code-behind de páginas ASP.NET. Lembre-se de que `DataObjectMethodAttribute` é usada para sinalizar a quais métodos devem aparecer em s ObjectDataSource configurar fonte de dados do assistente e em quais guia (SELECT, UPDATE, INSERT ou DELETE). Uma vez que o GridView não tem suporte interno para o lote, editando ou excluindo, teremos que invocar esses métodos por meio de programação em vez de usar a abordagem sem código declarativa.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Etapa 5: Atualizar atomicamente o banco de dados da camada de apresentação

Para ilustrar o efeito que a transação tenha ao atualizar um lote de registros, let s criar uma interface do usuário que lista todos os produtos em um GridView e inclui uma Web de botão de controle que, quando clicado, reatribui os produtos `CategoryID` valores. Em particular, a reatribuição de categoria progredirão para que os vários produtos primeiro seja atribuídos uma validade `CategoryID` enquanto outras são propositadamente de valor atribuído não existente `CategoryID` valor. Se tentar atualizar o banco de dados com um produto cujos `CategoryID` não corresponde a uma categoria existente s `CategoryID`, ocorrerá uma violação de restrição de chave estrangeira e uma exceção será gerada. O que veremos neste exemplo é que, ao usar uma transação a exceção gerada de violação de restrição de chave estrangeira fará com que o anterior válido `CategoryID` alterações sejam revertidas. Quando não estiver usando uma transação, no entanto, as modificações às categorias inicias permanecerá.

Comece abrindo o `Transactions.aspx` página o `BatchData` pasta e arraste um controle GridView na caixa de ferramentas para o Designer. Defina suas `ID` ao `Products` e, na marca inteligente, de associá-lo a um novo ObjectDataSource chamado `ProductsDataSource`. Configurar o ObjectDataSource para efetuar pull de seus dados a partir de `ProductsBLL` classe s `GetProducts` método. Isso será um GridView somente leitura, portanto, defina as listas suspensas na atualização, inserção e excluir guias como (nenhum) e clique em Concluir.


[![Configurar o ObjectDataSource para usar o método de GetProducts ProductsBLL classe s](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**Figura 5**: configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProducts` método ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**Figura 6**: defina a lista suspensa no UPDATE, INSERT e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio criará BoundFields e um CheckBoxField para os campos de dados do produto. Remover todos esses campos, exceto `ProductID`, `ProductName`, `CategoryID`, e `CategoryName` e renomeie o `ProductName` e `CategoryName` BoundFields `HeaderText` propriedades para o produto e categoria, respectivamente. Na marca inteligente, marque a opção de habilitar a paginação. Depois de fazer essas modificações, GridView e ObjectDataSource s marcação declarativa deve ser semelhante ao seguinte:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

Em seguida, adicione três controles da Web de botão acima GridView. Defina o primeiro botão s propriedade Text para atualização de grade, o segundo s para modificar categorias (com transação) e o terceiro s para modificar categorias (sem transação).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

Neste ponto, o modo de exibição de Design no Visual Studio deve ser semelhante à mostrada na Figura 7 de captura de tela.


[![A página contém um GridView e três controles de Web de botão](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**Figura 7**: A página contém um GridView e três controles de Web de botão ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))


Criar manipuladores de eventos para cada um dos três botão s `Click` eventos e use o código a seguir:


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

A atualização do botão s `Click` manipulador de eventos simplesmente associa novamente os dados para o GridView, chamando o `Products` GridView s `DataBind` método.

O segundo manipulador de eventos reatribui os produtos `CategoryID` s e usa o novo método de transação da BLL para executar o banco de dados atualiza sob o guarda-chuva de uma transação. Observe que cada produto s `CategoryID` á arbitrariamente definido como o mesmo valor que seu `ProductID`. Isso funcionará bem para os primeiros poucos produtos, uma vez que esses produtos têm `ProductID` valores que ocorrem mapear para válido `CategoryID` s. Uma vez, mas o `ProductID` início s fique muito grande, essa sobreposição uma coincidência dos `ProductID` s e `CategoryID` s não é mais aplicável.

O terceiro `Click` manipulador de eventos atualiza os produtos `CategoryID` s da mesma maneira, mas envia a atualização para o banco de dados usando o `ProductsTableAdapter` padrão s `Update` método. Isso `Update` não quebra a série de comandos em uma transação de método, para que essas alterações são feitas antes do primeiro erro de violação de restrição de chave estrangeira encontrados serão mantidas.

Para demonstrar esse comportamento, visite esta página por meio de um navegador. Inicialmente, você verá a primeira página de dados conforme mostrado na Figura 8. Em seguida, clique no botão Modificar categorias (com transação). Isso fará com que um postback-lo e tentar atualizar todos os produtos `CategoryID` valores, mas resultará em uma violação de restrição de chave estrangeira (consulte a Figura 9).


[![Os produtos são exibidos em um GridView paginável](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**Figura 8**: os produtos são exibidos em um GridView paginável ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))


[![Reatribuindo os resultados de categorias em uma violação de restrição de chave estrangeira](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**Figura 9**: Reatribuindo os resultados de categorias em uma violação de restrição de chave estrangeira ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))


Agora, pressione o botão de voltar do navegador s e, em seguida, clique no botão de atualização de grade. Após atualizar os dados você deverá ver a mesma saída exatamente conforme mostrado na Figura 8. Ou seja, mesmo que alguns dos produtos `CategoryID` s eram valores alterados para legal e atualizado no banco de dados, eles foram revertidos quando ocorreu a violação de restrição de chave estrangeira.

Agora, tente clicar no botão Modificar categorias (sem transação). Isso resultará no mesmo erro de violação de restrição de chave estrangeira (consulte a Figura 9), mas desta vez, esses produtos cuja `CategoryID` valores foram alterados para um legal valor serão não será revertido. Pressione o botão de voltar do navegador s e, em seguida, no botão de atualização de grade. Como mostra a Figura 10, o `CategoryID` s os oito primeiros produtos foram reatribuídos. Por exemplo, na Figura 8, Chang tinha um `CategoryID` de 1, mas na Figura 10 it s foi reatribuída a 2.


[![Alguns produtos CategoryID valores não pudessem ser atualizada enquanto outros foram](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**Figura 10**: alguns produtos `CategoryID` valores não pudessem ser atualizada enquanto outros eram ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))


## <a name="summary"></a>Resumo

Por padrão, os métodos do TableAdapter s não encapsulam as instruções de banco de dados executado dentro do escopo de uma transação, mas com um pouco de trabalho podemos adicionar métodos que criará, confirmação e reversão de uma transação. Neste tutorial, criamos três métodos na `ProductsTableAdapter` classe: `BeginTransaction`, `CommitTransaction`, e `RollbackTransaction`. Vimos como usar esses métodos junto com um `Try...Catch` bloco para fazer uma série de instruções de modificação de dados atômico. Em particular, criamos a `UpdateWithTransaction` método na `ProductsTableAdapter`, que usa o padrão de atualização em lotes para executar as modificações necessárias para as linhas de uma fornecida `ProductsDataTable`. Também adicionamos a `DeleteProductsWithTransaction` método para o `ProductsBLL` classe na BLL, que aceita um `List` de `ProductID` valores como entrada e chama o método padrão de DB-Direct `Delete` para cada `ProductID`. Ambos os métodos começam, criando uma transação e, em seguida, executando as instruções de modificação de dados dentro de um `Try...Catch` bloco. Se ocorrer uma exceção, a transação é revertida, caso contrário, ela será confirmada.

Etapa 5 ilustra o efeito de atualizações de lote transacional versus atualizações em lotes que inativos para usar uma transação. Os tutoriais de três nós aproveitam a base apresentada neste tutorial e criar interfaces do usuário para executar inserções, exclusões e atualizações em lotes.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Manter a consistência do banco de dados com transações](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Procedimentos armazenados de gerenciamento de transações no SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transações ficou mais fácil: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope e DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Usando transações de banco de dados Oracle no .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Dave Gardner, Hilton Giesenow e Teresa Murphy. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](batch-inserting-cs.md)
> [Próximo](batch-updating-vb.md)
