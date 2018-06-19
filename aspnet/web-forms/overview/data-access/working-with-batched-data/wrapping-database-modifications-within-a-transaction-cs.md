---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: Quebra automática de modificações de banco de dados em uma transação (c#) | Microsoft Docs
author: rick-anderson
description: Este tutorial é o primeiro de quatro que examina a atualização, exclusão e inserção de lotes de dados. Neste tutorial, saber como permitir que as transações do banco de dados...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: a3f8ec2de7b9259e4bb83f4346bde8abfd643fb4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888949"
---
<a name="wrapping-database-modifications-within-a-transaction-c"></a>Quebra automática de modificações de banco de dados em uma transação (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) ou [baixar PDF](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> Este tutorial é o primeiro de quatro que examina a atualização, exclusão e inserção de lotes de dados. Neste tutorial, saiba como transações de banco de dados permitem modificações de lote a ser executado como uma operação atômica, o que garante que todas as etapas tiverem êxito ou falham de todas as etapas.


## <a name="introduction"></a>Introdução

Como vimos começando com o [uma visão geral de inserção de, atualizando e excluindo dados](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) tutorial, GridView fornece suporte interno para o nível de linha de edição e exclusão. Com alguns cliques do mouse, é possível criar uma interface de modificação de dados avançados sem escrever uma linha de código, desde que você está conteúdo com a edição e exclusão em uma base por linha. No entanto, em determinados cenários, isso é suficiente e precisamos fornecer aos usuários a capacidade de editar ou excluir um lote de registros.

Por exemplo, baseado na web mais clientes de email usam uma grade para listar cada mensagem em que cada linha incluem uma caixa de seleção junto com as informações de s email (assunto, remetente e assim por diante). Essa interface permite que o usuário exclua mensagens vários verificá-los e, em seguida, clicando em um botão Excluir mensagens selecionadas. Um lote de interface de edição é ideal em situações onde os usuários geralmente editar vários registros diferentes. Em vez de forçar o usuário clique em Editar, faça a alteração e, em seguida, clique em atualizar para cada registro que precisa ser modificado, um lote edição interface processa cada linha com sua interface de edição. O usuário pode modificar rapidamente o conjunto de linhas que precisam ser alteradas e então salvar essas alterações, clicando em um botão Atualizar tudo. Esse conjunto de tutoriais, examinaremos como criar interfaces para inserção, edição e exclusão de lotes de dados.

Ao executar operações em lote-importante determinar se ele deve ser possível para algumas das operações em lote tenha êxito enquanto outros falhar. Considere a possibilidade de um lote interface - o que deve acontecer se o primeiro registro selecionado é excluído com êxito, mas o outro falha, digamos, devido a uma violação de restrição de chave estrangeira a exclusão? Deve primeiro excluir de registro s ser revertido ou é aceitável para o primeiro registro permaneça excluído?

Se desejar que a operação de lote deve ser tratado como um [operação atômica](http://en.wikipedia.org/wiki/Atomic_operation), um onde a todas as etapas de êxito ou falharem por todas as etapas e a camada de acesso a dados precisa ser aumentados para incluir suporte [banco de dados transações](http://en.wikipedia.org/wiki/Database_transaction). Transações de banco de dados garantem a atomicidade para o conjunto de `INSERT`, `UPDATE`, e `DELETE` instruções executadas sob a proteção da transação e são um recurso suportado pela maioria dos todos os sistemas de banco de dados moderno.

Neste tutorial, examinaremos como estender a DAL para usar transações de banco de dados. Tutoriais subsequentes examinará implementação páginas da web para o lote inserindo, atualizando e excluindo interfaces. Permitir que o s começar!

> [!NOTE]
> Ao modificar dados em uma transação em lote, não é necessário sempre atomicidade. Em alguns cenários, pode ser aceitável ter algumas modificações de dados tenha êxito e outros no mesmo lote falharem, por exemplo, ao excluir um conjunto de endereços de email de um cliente de email com base na web. Se não houver s um centro de erro de banco de dados por meio da exclusão processar, ele s provavelmente aceitável que os registros processados sem erro permaneçam excluídos. Nesses casos, a DAL não precisa ser modificado para oferecer suporte a transações do banco de dados. Há outros lote operação cenários, no entanto, onde a atomicidade é vital. Quando um cliente se move seus fundos de uma conta bancária para outra, duas operações devem ser executadas: os fundos devem ser deduzidos da primeira conta e, em seguida, adicionados para o segundo. Enquanto o banco não pode se importar com a primeira etapa bem-sucedida, mas a segunda etapa falhar, seus clientes é compreensível seria aborrecidos. Recomendo que você trabalhe com este tutorial e implementar os aprimoramentos para a DAL para oferecer suporte a transações do banco de dados mesmo se você não planeja usá-los na inserção de lote, atualizando e excluindo interfaces que criarei em três tutoriais a seguir.


## <a name="an-overview-of-transactions"></a>Uma visão geral de transações

A maioria dos bancos de dados incluem suporte para *transações*, que permitem que vários comandos de banco de dados a ser agrupada em uma única unidade lógica de trabalho. Os comandos de banco de dados que compõem uma transação têm garantia atômica, o que significa que todos os comandos falharão ou todos terá êxito.

Em geral, as transações são implementadas por meio de instruções SQL usando o seguinte padrão:

1. Indica o início de uma transação.
2. Execute as instruções SQL que compõem a transação.
3. Se houver um erro em uma das instruções na etapa 2, reverter a transação.
4. Se todas as instruções da etapa 2 concluída sem erros, confirme a transação.

As instruções SQL usadas para criar, confirmar e reverter a transação pode ser inserida manualmente ao escrever scripts SQL ou criando procedimentos armazenados ou por meio de programação significa usar ADO.NET ou as classes de [ `System.Transactions` namespace](https://msdn.microsoft.com/library/system.transactions.aspx). Neste tutorial, examinaremos somente Gerenciando transações usando o ADO.NET. Um tutorial futuras examinaremos como usar procedimentos armazenados na camada de acesso a dados, que vamos explorar as instruções SQL para criar, reverter e confirmar transações. Enquanto isso, consulte [gerenciar transações em procedimentos armazenados do SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml) para obter mais informações.

> [!NOTE]
> O [ `TransactionScope` classe](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) no `System.Transactions` namespace permite que os desenvolvedores programaticamente encapsular uma série de instruções dentro do escopo de uma transação e inclui suporte para transações complexas que envolvem vários fontes, como dois bancos de dados diferentes ou até mesmo heterogêneos tipos de armazenamentos de dados, como um banco de dados do Microsoft SQL Server, um banco de dados Oracle e um serviço Web. Eu ve decidiu usar transações ADO.NET para este tutorial, em vez da `TransactionScope` classe como ADO.NET é mais específico para transações de banco de dados e, em muitos casos, é muito menos recursos. Além disso, em alguns cenários de `TransactionScope` classe usa Microsoft Distributed Transaction coordenador (MSDTC). Os problemas de desempenho, implementação e configuração do MSDTC ao redor torna um tópico em vez disso, especializado e avançado e além do escopo esses tutoriais.


Ao trabalhar com o provedor SqlClient no ADO.NET, as transações são iniciadas por meio de uma chamada para o [ `SqlConnection` classe](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [ `BeginTransaction` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), que retorna um [ `SqlTransaction` objeto](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). As instruções de modificação de dados composição da transação são colocados dentro de um `try...catch` bloco. Se ocorrer um erro em uma instrução no `try` bloquear transferências de execução para o `catch` onde a transação pode ser revertida por meio do bloco de `SqlTransaction` objeto s [ `Rollback` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Se todas as instruções concluída com êxito, uma chamada para o `SqlTransaction` objeto s [ `Commit` método](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) no final do `try` bloco confirma a transação. O trecho de código a seguir ilustra esse padrão. Consulte [manter a consistência de banco de dados com transações](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) para sintaxe adicional e exemplos do uso de transações com o ADO.NET.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

Por padrão, os TableAdapters em um conjunto de dados tipado não usar transações. Para fornecer suporte para transações que precisamos para ampliar as classes de TableAdapter para incluir métodos adicionais que usam o padrão acima para executar uma série de instruções de modificação de dados dentro do escopo de uma transação. Na etapa 2, veremos como usar classes parciais para adicionar esses métodos.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Etapa 1: Criar o trabalho com páginas da Web de dados em lotes

Antes de começarmos a explorar como aumentar a DAL para oferecer suporte a transações do banco de dados, permitem s primeiro dedicar um tempo para criar páginas da web ASP.NET que será necessário para este tutorial e as três a seguir. Comece adicionando uma nova pasta chamada `BatchData` e, em seguida, adicione as seguintes páginas do ASP.NET, associar cada página com o `Site.master` página mestra.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![Adicionar as páginas ASP.NET para os tutoriais de SqlDataSource](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Figura 1**: adicionar as páginas ASP.NET para os tutoriais de SqlDataSource


Assim como acontece com as outras pastas, `Default.aspx` usará o `SectionLevelTutorialListing.ascx` controle de usuário para listar os tutoriais em sua seção. Portanto, adicione este controle de usuário para `Default.aspx` arrastando-o no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Figura 2**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))


Por fim, adicione esses quatro páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação após a personalização do mapa do Site `<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para trabalhar com os tutoriais de dados em lotes.


![O mapa do Site agora inclui entradas para trabalhar com os tutoriais de dados em lotes](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Figura 3**: O mapa de Site agora inclui entradas para trabalhar com os tutoriais de dados em lotes


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Etapa 2: Atualizando camada de acesso a dados para oferecer suporte a transações do banco de dados

Conforme abordado no primeiro tutorial, [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md), o conjunto de dados tipado no nosso DAL é composto de tabelas de dados e TableAdapters. As tabelas de dados contêm dados enquanto os TableAdapters fornecem a funcionalidade para ler dados do banco de dados para tabelas de dados, para atualizar o banco de dados com as alterações feitas em tabelas de dados e assim por diante. Lembre-se de que os TableAdapters fornecem dois padrões de atualização de dados, o que eu chamada de atualização em lotes e direta do banco de dados. Com o padrão de atualização em lotes, o TableAdapter é passado um conjunto de dados, DataTable ou coleção de DataRows. Esses dados são enumerados e para cada inseridos, modificados ou excluídos de linha, o `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` é executado. Com o padrão de direta do banco de dados, o TableAdapter é passado em vez disso, os valores das colunas necessárias para a inserção, atualização ou exclusão de um único registro. O método padrão direta do banco de dados, em seguida, usa os valores passados para executar apropriada `InsertCommand`, `UpdateCommand`, ou `DeleteCommand` instrução.

Independentemente do padrão de atualização usado, os métodos de TableAdapters gerado automaticamente não usar transações. Por padrão, cada insert, update ou delete executadas pelo TableAdapter é tratada como uma única operação discreta. Por exemplo, imagine que o padrão de direta do banco de dados é usado por alguns códigos na BLL para inserir dez registros no banco de dados. Este código poderia chamar o TableAdapter s `Insert` método dez vezes. Se as primeiras cinco inserções tenha êxito, mas um sexto resultou em uma exceção, os cinco primeiros registros inseridos permaneceria no banco de dados. Da mesma forma, se o padrão de atualização em lotes é usado para executar inserções, atualizações e exclusões para os dados inseridos, modificação e excluir linhas em uma DataTable, se as modificações de vários primeiro foi bem-sucedida, mas um posterior encontrou um erro, essas modificações anteriores que concluída permanecerá no banco de dados.

Em determinados cenários, queremos garantir a atomicidade em uma série de modificações. Para fazer isso manualmente, deve estender o TableAdapter adicionando novos métodos que executam o `InsertCommand`, `UpdateCommand`, e `DeleteCommand` s sob a proteção de uma transação. Em [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) examinamos usando [classes parciais](http://en.wikipedia.org/wiki/Partial_type) para estender a funcionalidade de tabelas de dados no conjunto de dados digitados. Essa técnica também pode ser usada com TableAdapters.

O conjunto de dados tipado `Northwind.xsd` está localizado no `App_Code` pasta s `DAL` subpasta. Criar uma subpasta no `DAL` pasta denominada `TransactionSupport` e adicione um novo arquivo de classe chamado `ProductsTableAdapter.TransactionSupport.cs` (consulte a Figura 4). Esse arquivo conterá a implementação parcial do `ProductsTableAdapter` que inclui métodos para executar modificações de dados usando uma transação.


![Adicionar uma pasta chamada TransactionSupport e um arquivo de classe chamado ProductsTableAdapter.TransactionSupport.cs](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Figura 4**: adicionar uma pasta chamada `TransactionSupport` e um arquivo de classe chamado. `ProductsTableAdapter.TransactionSupport.cs`


Digite o seguinte código para o `ProductsTableAdapter.TransactionSupport.cs` arquivo:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

O `partial` palavra-chave na declaração da classe aqui indica para o compilador os membros adicionados do devem ser adicionados para o `ProductsTableAdapter` classe no `NorthwindTableAdapters` namespace. Observe o `using System.Data.SqlClient` instrução na parte superior do arquivo. Como o TableAdapter foi configurado para usar o provedor SqlClient, internamente ele usa um [ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) objeto para emitir os comandos no banco de dados. Consequentemente, é preciso usar o `SqlTransaction` classe para iniciar a transação e, em seguida, para confirmá-la ou revertê-lo. Se você estiver usando um armazenamento de dados diferente do Microsoft SQL Server, você precisará usar o provedor apropriado.

Esses métodos fornecem os blocos de construção necessários para iniciar, reversão e confirmar uma transação. Eles serão marcados `public`, possibilitando a ser usado de dentro a `ProductsTableAdapter`, de outra classe em DAL ou de outra camada na arquitetura, como o BLL. `BeginTransaction` Abre o TableAdapter s interno `SqlConnection` (se necessário), inicia a transação e o atribui para a `Transaction` propriedade e anexa a transação para o interno `SqlDataAdapter` s `SqlCommand` objetos. `CommitTransaction` e `RollbackTransaction` chamar o `Transaction` objeto s `Commit` e `Rollback` métodos, respectivamente, antes de fechar o interno `Connection` objeto.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Etapa 3: Adicionando métodos para atualizar e excluir dados sob a proteção de uma transação

Com esses métodos completos, podemos re pronto para adicionar métodos para `ProductsDataTable` ou BLL que executar uma série de comandos sob a proteção de uma transação. O método a seguir usa o padrão de atualização em lotes para atualizar um `ProductsDataTable` instância usando uma transação. Ele inicia uma transação chamando o `BeginTransaction` método e, em seguida, usa um `try...catch` bloco para emitir as instruções de modificação de dados. Se a chamada para o `Adapter` objeto s `Update` método resulta em uma exceção, a execução será transferido para o `catch` blocos em que a transação será revertida e a exceção lançada novamente. Lembre-se de que o `Update` método implementa o padrão de atualização em lotes enumerando as linhas de fornecido `ProductsDataTable` e executar o necessário `InsertCommand`, `UpdateCommand`, e `DeleteCommand` s. Se qualquer um desses resultados de comandos em um erro, a transação será revertida, desfazendo as modificações anteriores feitas durante o tempo de vida de s de transação. Deve o `Update` instrução concluída sem erros, a transação é confirmada em sua totalidade.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Adicionar o `UpdateWithTransaction` método para o `ProductsTableAdapter` classe por meio da classe parcial em `ProductsTableAdapter.TransactionSupport.cs`. Como alternativa, esse método pôde ser adicionado para a camada de lógica comercial s `ProductsBLL` classe com algumas pequenas alterações sintáticas. Ou seja, a palavra-chave isso em `this.BeginTransaction()`, `this.CommitTransaction()`, e `this.RollbackTransaction()` precisa ser substituído por `Adapter` (Lembre-se de que `Adapter` é o nome de uma propriedade em `ProductsBLL` do tipo `ProductsTableAdapter`).

O `UpdateWithTransaction` método usa o padrão de atualização em lotes, mas uma série de chamadas diretas de banco de dados também pode ser usada dentro do escopo de uma transação, como mostra o método a seguir. O `DeleteProductsWithTransaction` método o aceita como entrada uma `List<T>` do tipo `int`, que são o `ProductID` s a serem excluídos. O método inicia a transação por meio de uma chamada para `BeginTransaction` e, em seguida, no `try` em bloco, itera através da lista fornecida chamando o padrão de DB diretos `Delete` método para cada `ProductID` valor. Se qualquer uma das chamadas para `Delete` falhar, o controle é transferido para o `catch` blocos em que a transação é revertida e a exceção lançada novamente. Se todas as chamadas para `Delete` bem-sucedida, em seguida, a transação é confirmada. Adicione esse método para o `ProductsBLL` classe.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Aplicação de transações em vários TableAdapters

Permite que o código relacionadas à transação examinado neste tutorial para várias instruções em relação a `ProductsTableAdapter` deve ser tratada como uma operação atômica. Mas e se precisam ser executada atomicamente várias modificações em tabelas de banco de dados diferente? Por exemplo, ao excluir uma categoria, pode primeiro queremos reatribuir seus produtos atuais para alguma outra categoria. Essas duas etapas Reatribuindo os produtos e a exclusão da categoria devem ser executadas como uma operação atômica. Mas o `ProductsTableAdapter` inclui somente os métodos para modificar o `Products` tabela e o `CategoriesTableAdapter` inclui somente os métodos para modificar o `Categories` tabela. Então, como uma transação pode abranger ambos os TableAdapters?

Uma opção é adicionar um método para o `CategoriesTableAdapter` chamado `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` e ter esse método chamar um procedimento armazenado que reatribui os produtos e exclui a categoria dentro do escopo de uma transação definida dentro do procedimento armazenado. Vamos examinar como iniciar, confirmar e reverter transações em procedimentos armazenados em um tutorial futuras.

Outra opção é criar uma classe auxiliar no DAL que contém o `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` método. Esse método cria uma instância do `CategoriesTableAdapter` e `ProductsTableAdapter` e, em seguida, defina-os dois TableAdapters `Connection` propriedades ao mesmo `SqlConnection` instância. AT desse ponto, um dos dois TableAdapters possam iniciar a transação com uma chamada para `BeginTransaction`. Os métodos de TableAdapters para reatribuir os produtos e excluir a categoria seriam invocados em um `try...catch` bloco com a transação confirmada ou revertida volta conforme necessário.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Etapa 4: Adicionando a`UpdateWithTransaction`método para a camada de lógica de negócios

Na etapa 3, adicionamos uma `UpdateWithTransaction` método para o `ProductsTableAdapter` em DAL. Devemos acrescentar um método correspondente para o BLL. Embora a camada de apresentação pode chamar diretamente para baixo até a DAL para chamar o `UpdateWithTransaction` método, esses tutoriais tem strived definir uma arquitetura em camadas que isola a DAL da camada de apresentação. Portanto, cabe a continuar essa abordagem.

Abra o `ProductsBLL` arquivo de classe e adicione um método chamado `UpdateWithTransaction` que chama simplesmente até o método DAL correspondente. Agora deve ser dois novos métodos em `ProductsBLL`: `UpdateWithTransaction`, que você acabou de adicionar e `DeleteProductsWithTransaction`, que foi adicionado na etapa 3.


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Esses métodos não incluem o `DataObjectMethodAttribute` atributo atribuído a maioria dos outros métodos de `ProductsBLL` classe porque estamos invocar esses métodos diretamente a partir de classes de code-behind de páginas do ASP.NET. Lembre-se de que `DataObjectMethodAttribute` é usada para sinalizar que métodos devem aparecer em ObjectDataSource s configurar fonte de dados do assistente e em qual guia (SELECT, UPDATE, INSERT ou DELETE). Desde que o GridView não tem suporte interno para edição ou exclusão de lote, teremos que chamar esses métodos programaticamente em vez de usar a abordagem declarativa sem código.


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Etapa 5: Atomicamente atualizando dados de banco de dados da camada de apresentação

Para ilustrar o efeito que a transação tenha ao atualizar um lote de registros, permitem s criar uma interface de usuário que lista todos os produtos em um GridView e inclui um botão Web controlar que, quando clicado, reatribui os produtos `CategoryID` valores. Em particular, a reatribuição de categoria pode Avançar para que os vários produtos primeiro recebem válido `CategoryID` valor enquanto outras são intencionalmente atribuído um inexistente `CategoryID` valor. Se tentar atualizar o banco de dados com um produto cujo `CategoryID` não corresponde a uma categoria existente s `CategoryID`, ocorrerá uma violação de restrição de chave estrangeira e será gerada uma exceção. O que veremos neste exemplo é que, ao usar uma transação a exceção gerada da violação da restrição de chave estrangeira fará com que o anterior válido `CategoryID` alterações sejam revertidas. Quando não estiver usando uma transação, no entanto, as modificações nas categorias inicial permanecerá.

Comece abrindo o `Transactions.aspx` página o `BatchData` pasta e arraste um controle GridView da caixa de ferramentas para o Designer. Definir seu `ID` para `Products` e, na marca inteligente, de associá-lo a um novo ObjectDataSource denominado `ProductsDataSource`. Configurar ObjectDataSource para efetuar o pull de seus dados a partir de `ProductsBLL` classe s `GetProducts` método. Isso será um GridView somente leitura, portanto definido as listas suspensas na atualização, inserção e excluir guias como (nenhum) e clique em Concluir.


[![Figura 5: Configurar o ObjectDataSource para usar o método de GetProducts ProductsBLL classe s](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Figura 5**: Figura 5: configurar o ObjectDataSource para usar o `ProductsBLL` classe s `GetProducts` método ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))


[![Definir as listas suspensas na atualização, inserção e excluir guias como (nenhum)](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Figura 6**: definir a lista suspensa na atualização, inserir e excluir guias como (nenhum) ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))


Depois de concluir o Assistente Configurar fonte de dados, o Visual Studio criará BoundFields e um CheckBoxField para os campos de dados de produto. Remova todos os campos, exceto para `ProductID`, `ProductName`, `CategoryID`, e `CategoryName` e renomeie o `ProductName` e `CategoryName` BoundFields `HeaderText` propriedades para o produto e categoria, respectivamente. Marca inteligente, marque a opção habilitar paginação. Depois de fazer essas modificações, a marcação de declarativa s GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

Em seguida, adicione os três controles de botão Web acima GridView. Defina o primeiro botão s propriedade Text de atualização de grade, o segundo s para modificar categorias (com transações) e o um terceiro s para modificar categorias (sem transação).


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

Neste ponto o modo de exibição de Design no Visual Studio deve ser semelhante ao mostrado na Figura 7 de captura de tela.


[![A página contém um controle GridView e três controles da Web de botão](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Figura 7**: A página contém um GridView e os três controles da Web de botão ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))


Criar manipuladores de eventos para cada botão três s `Click` eventos e use o código a seguir:


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

A atualização botão s `Click` manipulador de eventos simplesmente reconecta os dados do GridView chamando o `Products` GridView s `DataBind` método.

O segundo manipulador de eventos reatribui os produtos `CategoryID` s e usa o novo método de transação de BLL para executar o banco de dados atualiza sob a proteção de uma transação. Observe que cada produto s `CategoryID` arbitrariamente é definido como o mesmo valor como seu `ProductID`. Isso funciona bem para a primeira alguns produtos, como os produtos têm `ProductID` valores acontecem mapear para válido `CategoryID` s. Uma vez, mas o `ProductID` início s fique muito grande, essa sobreposição uma coincidência de `ProductID` s e `CategoryID` s já não se aplica.

O terceiro `Click` os produtos de atualizações do manipulador de eventos `CategoryID` s da mesma maneira, mas envia a atualização para o banco de dados usando o `ProductsTableAdapter` padrão s `Update` método. Isso `Update` método não estiver disposto a série de comandos em uma transação, para que essas alterações são feitas antes do primeiro erro de violação de restrição de chave estrangeira encontrado será mantido.

Para demonstrar esse comportamento, visite esta página por meio de um navegador. Inicialmente, você verá a primeira página de dados conforme mostrado na Figura 8. Em seguida, clique no botão Modificar categorias (com transação). Isso irá causar um postback e tentar atualizar todos os produtos `CategoryID` valores, mas resultará em uma violação de restrição de chave estrangeira (consulte a Figura 9).


[![Os produtos são exibidos em um GridView paginável](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Figura 8**: os produtos são exibidos em um GridView paginável ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))


[![Reatribuindo os resultados de categorias em uma violação de restrição de chave estrangeira](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Figura 9**: Reatribuindo os resultados de categorias em uma violação de restrição de chave estrangeira ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))


Agora visitas o botão Voltar do navegador s e, em seguida, clique no botão de atualização de grade. Após a atualização de dados você verá a mesma saída exatamente como mostrado na Figura 8. Ou seja, mesmo se alguns produtos `CategoryID` s foram valores alterados para legal e atualizadas no banco de dados, foram revertidas quando ocorreu a violação de restrição de chave estrangeira.

Agora, tente clicar no botão Modificar categorias (sem transação). Isso resultará no mesmo erro de violação de restrição de chave estrangeira (consulte a Figura 9), mas desta vez os produtos cujos `CategoryID` valores foram alterados para um legal valor serão não será revertido. Atingido o botão Voltar do navegador s e, em seguida, no botão de atualização de grade. Como mostra a Figura 10, o `CategoryID` s dos oito primeiros produtos foram reatribuídos. Por exemplo, na Figura 8, lterar tinha um `CategoryID` de 1, mas na Figura 10 it s foi reatribuído para 2.


[![Alguns valores de CategoryID produtos foram atualizados enquanto outros foram não](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Figura 10**: alguns produtos `CategoryID` valores foram atualizados enquanto outros foram não ([clique para exibir a imagem em tamanho normal](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))


## <a name="summary"></a>Resumo

Por padrão, os métodos TableAdapter não encapsule as instruções do banco de dados executado dentro do escopo de uma transação, mas com um pouco de trabalho podemos adicionar métodos que criará, confirmação e reversão de uma transação. Neste tutorial, nós criamos três esses métodos de `ProductsTableAdapter` classe: `BeginTransaction`, `CommitTransaction`, e `RollbackTransaction`. Vimos como usar esses métodos junto com um `try...catch` bloco para tornar uma série de instruções de modificação de dados atômico. Em particular, criamos o `UpdateWithTransaction` método o `ProductsTableAdapter`, que usa o padrão de atualização em lotes para executar as modificações necessárias para as linhas de um fornecido `ProductsDataTable`. Também adicionamos a `DeleteProductsWithTransaction` método para o `ProductsBLL` classe na BLL, que aceita um `List` de `ProductID` valores como entrada e chama o método padrão de DB diretos `Delete` para cada `ProductID`. Ambos os métodos Iniciar criação de uma transação e, em seguida, executando as instruções de modificação de dados dentro de um `try...catch` bloco. Se ocorrer uma exceção, a transação é revertida, caso contrário, ele é confirmado.

Etapa 5 ilustra o efeito de atualizações em lote transacionais versus atualizações em lote que inativos para usar uma transação. Os próximos três tutoriais vamos criar com base apresentado neste tutorial e criar interfaces do usuário para executar atualizações em lotes, inserções e exclusões.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Mantém a consistência do banco de dados com transações](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Procedimentos armazenados de gerenciamento de transações no SQL Server](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transações ficou mais fácil: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope e DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Usando transações de banco de dados Oracle no .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Dave Gardner Hilton Giesenow e Teresa Murphy. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](batch-updating-cs.md)
