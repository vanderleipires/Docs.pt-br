---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
title: Criando novos procedimentos armazenados para os TableAdapters do conjunto de dados tipado (c#) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores temos criado instruções SQL em nosso código e passado as instruções para o banco de dados a ser executado. Uma abordagem alternativa é usar s...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 751282ca-5870-4d66-84e4-6cefae23eb4a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs
msc.type: authoredcontent
ms.openlocfilehash: 0932749d6cf1665eedd5f452ab5dd63ed8678962
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834074"
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-c"></a>Criando novos procedimentos armazenados para os TableAdapters do conjunto de dados tipado (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_CS.zip) ou [baixar PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/datatutorial67cs1.pdf)

> Nos tutoriais anteriores temos criado instruções SQL em nosso código e passado as instruções para o banco de dados a ser executado. Uma abordagem alternativa é usar procedimentos armazenados, em que as instruções SQL são predefinidas no banco de dados. Neste tutorial, saiba como TableAdapter assistente gerar novos procedimentos armazenados para nós.


## <a name="introduction"></a>Introdução

A camada de acesso de dados (DAL) para esses tutoriais usa DataSets tipados. Conforme discutido na [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) tutorial, conjuntos de dados tipados consistem em DataTables fortemente tipados e TableAdapters. As tabelas de dados representam as entidades lógicas no sistema enquanto a interface de TableAdapters com o banco de dados subjacente para realizar o trabalho de acesso de dados. Isso inclui popular tabelas de dados com dados, execução de consultas que retornam dados escalares, inserindo, atualizando e excluindo registros do banco de dados.

Os comandos SQL executados pelo TableAdapters podem ser qualquer um das instruções SQL ad hoc, como `SELECT columnList FROM TableName`, ou procedimentos armazenados. Os TableAdapters em nossa arquitetura usar instruções SQL ad hoc. Muitos desenvolvedores e administradores de banco de dados, no entanto, preferem procedimentos armazenados instruções de SQL ad hoc por motivos de segurança, facilidade de manutenção e atualização. Outros preferem ardently instruções de SQL ad hoc para a sua flexibilidade. Em meu próprio trabalho favorecer procedimentos armazenados ao longo de instruções SQL ad hoc, mas optar por usar instruções SQL ad hoc para simplificar os tutoriais anteriores.

Quando a definição de um TableAdapter ou adicionar novos métodos, o TableAdapter s assistente torna tão fácil de criar novos procedimentos armazenados ou use os procedimentos armazenados existentes, como faz para usar instruções SQL ad hoc. Neste tutorial, examinaremos como fazer com que o Assistente de s TableAdapter geração automática de procedimentos armazenados. No próximo tutorial vamos examinar como configurar os métodos do TableAdapter s para usar procedimentos armazenados existentes ou criados manualmente.

> [!NOTE]
> Consulte a entrada de blog de Rob Howard [não o procedimentos do armazenados de uso ainda?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) e [Frans Bouma](https://weblogs.asp.net/fbouma/) entrada de blog s [procedimentos armazenados são ruins, Kay M?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) para um debate vívido dos prós e contras de procedimentos armazenados e SQL ad hoc.


## <a name="stored-procedure-basics"></a>Conceitos básicos de procedimento armazenado

As funções são uma construção comum a todas as linguagens de programação. Uma função é uma coleção de instruções que são executadas quando a função é chamada. Funções podem aceitar parâmetros de entrada e, opcionalmente, podem retornar um valor. *[Procedimentos armazenados](http://en.wikipedia.org/wiki/Stored_procedure)*  são construções de banco de dados que compartilham várias semelhanças com funções em linguagens de programação. Um procedimento armazenado é composto por um conjunto de instruções T-SQL que são executadas quando o procedimento armazenado é chamado. Um procedimento armazenado pode aceitar zero a muitos parâmetros de entrada e pode retornar valores escalares, parâmetros de saída, ou, mais comumente, os conjuntos de resultados de `SELECT` consultas.

> [!NOTE]
> Procedimentos armazenados são muitas vezes chamados de sprocs ou SPs.


Procedimentos armazenados são criados usando o [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) instrução T-SQL. Por exemplo, o script T-SQL a seguir cria um procedimento armazenado denominado `GetProductsByCategoryID` que usa um parâmetro único chamado `@CategoryID` e retorna o `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` campos dessas colunas no `Products` tabela que tenha uma correspondência `CategoryID` valor:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample1.sql)]

Quando esse procedimento armazenado tiver sido criado, ele pode ser chamado usando a seguinte sintaxe:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample2.sql)]

> [!NOTE]
> O próximo tutorial, examinaremos a criação de procedimentos armazenados por meio do IDE do Visual Studio. Para este tutorial, no entanto, vamos permitir que o Assistente de TableAdapter gerar automaticamente os procedimentos armazenados para nós.


Além de simplesmente retornar dados, procedimentos armazenados são geralmente usados para executar vários comandos de banco de dados dentro do escopo de uma única transação. Um procedimento armazenado denominado `DeleteCategory`, por exemplo, pode levar dentro de uma `@CategoryID` parâmetro e executar duas `DELETE` instruções: primeiro, um para excluir os produtos relacionados e uma segunda excluindo a categoria especificada. São várias instruções em um procedimento armazenado *não* automaticamente encapsulado dentro de uma transação. Comandos adicionais do T-SQL precisam ser emitido para garantir que o procedimento armazenado s que vários comandos são tratados como uma operação atômica. Veremos como encapsular os comandos de s um procedimento armazenado dentro do escopo de uma transação no tutorial subsequente.

Ao usar procedimentos armazenados dentro de uma arquitetura, os métodos de camada de acesso a dados s invocam um procedimento armazenado específico em vez de emitir uma instrução de SQL ad hoc. Isso centraliza o local das instruções SQL executadas (no banco de dados) em vez de deixá-la definida dentro da arquitetura do aplicativo s. Sem dúvida, essa centralização torna mais fácil localizar, analisar e ajustar as consultas e fornece uma visão mais clara sobre onde e como o banco de dados está sendo usado.

Para obter mais informações sobre os conceitos básicos de procedimento armazenado, consulte os recursos na seção leitura adicional no final deste tutorial.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Etapa 1: Criar páginas de Web de cenários de camada para acesso a dados avançados

Antes de começar nossa discussão sobre como criar uma DAL usando procedimentos armazenados, permitem que s levar alguns instantes para criar as páginas do ASP.NET em nosso projeto de site que precisamos para isso e os próximo vários tutoriais pela primeira vez. Comece adicionando uma nova pasta chamada `AdvancedDAL`. Em seguida, adicione as seguintes páginas do ASP.NET para essa pasta, certificando-se de associar cada página com o `Site.master` página mestra:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Adicione as páginas do ASP.NET para que os tutoriais de cenários de camada de acesso avançada de dados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image1.png)

**Figura 1**: adicionar as páginas do ASP.NET para que os tutoriais de cenários de camada de acesso avançada de dados


Como em outras pastas `Default.aspx` no `AdvancedDAL` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicionar esse controle de usuário `Default.aspx` arrastando-no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx para default. aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image2.png)

**Figura 2**: Adicione o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image4.png))


Por fim, adicione essas páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a marcação a seguir após o trabalho com dados em lote `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample3.xml)]

Depois de atualizar `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para os tutoriais de cenários avançados do DAL.


![O mapa do Site agora inclui entradas para os tutoriais de cenários avançados de DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image5.png)

**Figura 3**: O mapa do Site agora inclui entradas para os tutoriais de cenários avançados de DAL


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Etapa 2: Configurar um TableAdapter para criar novos procedimentos armazenados

Para demonstrar a criação de uma camada de acesso de dados que usa procedimentos armazenados, em vez de instruções SQL ad hoc, let s criar um novo DataSet digitado na `~/App_Code/DAL` pasta chamada `NorthwindWithSprocs.xsd`. Uma vez que podemos ter passado por meio desse processo em detalhes nos tutoriais anteriores, prosseguiremos rapidamente as etapas aqui. Se você ficar preso ou precisar de mais instruções passo a passo na criação e configuração de um DataSet tipado, consulte o [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-cs.md) tutorial.

Adicionar um novo conjunto de dados ao projeto clicando com o `DAL` pasta, escolha Add New Item e selecionando o modelo de conjunto de dados, conforme mostrado na Figura 4.


[![Adicionar um novo conjunto de dados tipado ao projeto nomeado NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image6.png)

**Figura 4**: adicionar um novo DataSet tipado ao projeto nomeado `NorthwindWithSprocs.xsd` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image8.png))


Isso será criar novo conjunto de dados tipado, abra o Designer, criar um novo TableAdapter e iniciar o Assistente de configuração do TableAdapter. A primeira etapa do Assistente de configuração TableAdapter s nos pede para selecionar o banco de dados para trabalhar com. A cadeia de caracteres de conexão ao banco de dados Northwind deve constar na lista suspensa. Selecione esta opção e clique em Avançar.

Nesta próxima tela, podemos escolher como o TableAdapter deve acessar o banco de dados. Nos tutoriais anteriores, selecionamos a primeira opção, usar instruções SQL. Para este tutorial, selecione a segunda opção, criar novos procedimentos armazenados e clique em Avançar.


[![Instrua o TableAdpater para criar novos procedimentos armazenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image9.png)

**Figura 5**: instruir o TableAdpater para criar novos procedimentos armazenados ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image11.png))


Assim como com o uso de instruções SQL ad hoc, na etapa a seguir somos solicitados a fornecer o `SELECT` instrução da consulta principal do TableAdapter s. Mas, em vez de usar o `SELECT` instrução inserida aqui para executar uma consulta ad hoc diretamente, o Assistente de s TableAdapter criará um procedimento armazenado que contém este `SELECT` consulta.

Use o seguinte `SELECT` consulta para esse TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample4.sql)]


[![Insira a consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image12.png)

**Figura 6**: insira o `SELECT` consulta ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image14.png))


> [!NOTE]
> A consulta acima difere um pouco a consulta principal a `ProductsTableAdapter` no `Northwind` conjunto de dados tipado. Lembre-se de que o `ProductsTableAdapter` no `Northwind` conjunto de dados tipado inclui duas subconsultas correlatas para trazer de volta o nome da categoria e o nome da empresa para cada categoria de produto s e fornecedor. Na próxima [atualizando o TableAdapter para usar junções](updating-the-tableadapter-to-use-joins-cs.md) tutorial, veremos adicionar isso dados relacionados a esse TableAdapter.


Reserve um tempo para clicar no botão Opções avançadas. A partir daqui, podemos especificar se o assistente deve também gerar insert, update e delete instruções para o TableAdapter, se é necessário usar a simultaneidade otimista e se a tabela de dados deve ser atualizada após inserções e atualizações. A opção de gerar Insert, Update e Delete instruções é marcada por padrão. Deixe marcada. Para este tutorial, deixe as opções de simultaneidade otimista de uso não verificado.

Quando precisar os procedimentos armazenados criados automaticamente pelo Assistente do TableAdapter, parece que a atualização, a opção de tabela de dados será ignorada. Independentemente de se essa caixa de seleção estiver marcada, o resultante insert e update a procedimentos armazenados recuperar o registro de just-inseridos ou atualizados just, como veremos na etapa 3.


![Deixe as instruções de gerar Insert, Update e Delete opção marcada](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image15.png)

**Figura 7**: deixe as instruções de gerar Insert, Update e Delete opção marcada


> [!NOTE]
> Se a opção de simultaneidade otimista usar estiver marcada, o assistente adicionará as condições adicionais para o `WHERE` cláusula que impedir que os dados sejam atualizados se houve alterações em outros campos. Voltar para o [Implementando a simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md) para obter mais informações sobre como usar o recurso de controle de simultaneidade otimista internos do TableAdapter s.


Depois de inserir o `SELECT` consultar e confirmando que a opção de gerar Insert, Update e Delete instruções está selecionada, clique em Avançar. Esta próxima tela, mostrada na Figura 8, solicita que os nomes dos procedimentos armazenados, que o assistente criará para selecionar, inserir, atualizar e excluir dados. Altere esses nomes de procedimentos para armazenados `Products_Select`, `Products_Insert`, `Products_Update`, e `Products_Delete`.


[![Renomear os procedimentos armazenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image16.png)

**Figura 8**: renomear os procedimentos armazenados ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image18.png))


Para ver o T-SQL, o assistente TableAdapter usará para criar quatro procedimentos armazenados, clique no botão Visualizar Script SQL. Na caixa de diálogo Visualizar Script SQL, você pode salvar o script em um arquivo ou copiá-lo para a área de transferência.


![Visualizar o Script SQL usado para gerar os procedimentos armazenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image19.png)

**Figura 9**: visualizar o Script SQL usado para gerar os procedimentos armazenados


Depois de nomear os procedimentos armazenados, clique em Avançar métodos correspondentes do nome do TableAdapter s. Assim como ao usar instruções SQL ad hoc, podemos criar métodos que preencher uma DataTable existente ou retornam um novo. Também podemos especificar se o TableAdapter deve incluir o padrão de diretos do banco de dados para inserir, atualizar e excluir registros. Deixe todas as três caixas de seleção marcadas, mas renomear o retorno de um método de DataTable para `GetProducts` (conforme mostrado na Figura 10).


[![Nomeie os métodos Fill e GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image20.png)

**Figura 10**: nomear os métodos `Fill` e `GetProducts` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image22.png))


Clique em Avançar para ver um resumo das etapas que o assistente executará. Conclua o assistente clicando no botão Concluir. Depois que o assistente for concluído, você será retornado para o s de DataSet Designer, que agora deve incluir o `ProductsDataTable`.


[![O Designer de conjunto de dados s mostra o ProductsDataTable adicionado recentemente](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image23.png)

**Figura 11**: O DataSet Designer de s mostra o recém-adicionado `ProductsDataTable` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Etapa 3: Examinar os procedimentos armazenados recém-criado

O Assistente de TableAdapter usado na etapa 2 automaticamente criou os procedimentos armazenados para selecionar, inserir, atualizar e excluir dados. Esses procedimentos armazenados podem ser exibidos ou modificados por meio do Visual Studio, acessando o Gerenciador de servidores e drill-down até a pasta de procedimentos armazenados do banco de dados s. Como mostra a Figura 12, o banco de dados Northwind contém quatro novos procedimentos armazenados: `Products_Delete`, `Products_Insert`, `Products_Select`, e `Products_Update`.


![Os quatro procedimentos armazenados que criou na etapa 2 podem ser encontrados na pasta de procedimentos armazenados do banco de dados s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image26.png)

**Figura 12**: quatro procedimentos armazenados criados na etapa 2 podem ser encontrados na pasta de procedimentos armazenados do banco de dados s


> [!NOTE]
> Se você não vir o Server Explorer, vá até o menu Exibir e escolha a opção de Gerenciador de servidores. Se você não vir os procedimentos armazenados relacionados a produtos adicionados na etapa 2, tente clicando duas vezes na pasta de procedimentos armazenados e escolha atualizar.


Para exibir ou modificar um procedimento armazenado, clique duas vezes em seu nome no Gerenciador de servidores ou, Alternativamente, clique com botão direito no procedimento armazenado e escolha Abrir. A Figura 13 mostra o `Products_Delete` procedimento armazenado, quando aberto.


[![Procedimentos armazenados podem ser abertos e modificados de dentro do Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image27.png)

**Figura 13**: armazenados procedimentos podem ser abertos e modificados de dentro do Visual Studio ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image29.png))


O conteúdo de ambas as `Products_Delete` e `Products_Select` procedimentos armazenados são bastante simples. O `Products_Insert` e `Products_Update` procedimentos armazenados, por outro lado, garantem um exame detalhado a medida que os dois executam uma `SELECT` instrução após sua `INSERT` e `UPDATE` instruções. Por exemplo, o SQL a seguir faz de `Products_Insert` procedimento armazenado:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample5.sql)]

O procedimento armazenado aceita como parâmetros de entrada a `Products` colunas que foram retornadas pela `SELECT` consulta especificada no Assistente para s TableAdapter e esses valores são usados em um `INSERT` instrução. Seguindo a `INSERT` instrução, uma `SELECT` consulta é usada para retornar o `Products` valores de coluna (incluindo o `ProductID`) do registro adicionado recentemente. Esse recurso de atualização é útil quando adicionar um novo registro usando o padrão de atualização em lotes como ela automaticamente atualiza recém-adicionada `ProductRow` instâncias `ProductID` propriedades com os valores incrementado automaticamente atribuídos pelo banco de dados.

O código a seguir ilustra esse recurso. Ele contém um `ProductsTableAdapter` e `ProductsDataTable` criado para o `NorthwindWithSprocs` conjunto de dados tipado. Um novo produto é adicionado ao banco de dados com a criação de um `ProductsRow` instância, fornecendo seus valores e, em seguida, chamar o TableAdapter s `Update` método, passando o `ProductsDataTable`. Internamente, o TableAdapter s `Update` método enumera a `ProductsRow` instâncias na DataTable no passado (neste exemplo há apenas uma - aquele que acabamos de adicionar) e executa apropriado inserir, atualizar ou excluir o comando. Nesse caso, o `Products_Insert` procedimento armazenado é executado, o que adiciona um novo registro para o `Products` de tabela e retorna os detalhes do registro adicionado recentemente. O `ProductsRow` instância s `ProductID` valor, em seguida, é atualizado. Após o `Update` método for concluído, podemos acessar o registro recém-adicionado s `ProductID` de valor por meio de `ProductsRow` s `ProductID` propriedade.


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample6.cs)]

O `Products_Update` procedimento armazenado da mesma forma inclui um `SELECT` instrução após sua `UPDATE` instrução.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample7.sql)]

Observe que este procedimento armazenado inclui dois parâmetros de entrada para `ProductID`: `@Original_ProductID` e `@ProductID`. Essa funcionalidade permite cenários em que a chave primária pode ser alterada. Por exemplo, em um banco de dados do funcionário, cada registro de funcionário pode usar o s número do INPS como sua chave primária. Para alterar um número do seguro social existente funcionário s, o novo número do seguro social e original devem ser fornecidos. Para o `Products` tabela, essa funcionalidade não é necessária porque o `ProductID` coluna é um `IDENTITY` coluna e nunca deve ser alterada. Na verdade, o `UPDATE` instrução em de `Products_Update` t do procedimento armazenado incluem o `ProductID` coluna na sua lista de colunas. Dessa forma, enquanto `@Original_ProductID` é usado na `UPDATE` instrução s `WHERE` cláusula, isso é supérfluo para o `Products` de tabela e pode ser substituído pelo `@ProductID` parâmetro. Ao modificar um procedimento armazenado s de parâmetros é importante que os métodos TableAdapter que usam esse procedimento armazenado também são atualizados.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Etapa 4: Modificar um procedimento armazenado de parâmetros de s e atualizando o TableAdapter

Uma vez que o `@Original_ProductID` parâmetro é supérfluo, let s removê-lo do `Products_Update` procedimento armazenado completamente. Abra o `Products_Update` procedimento armazenado, excluir o `@Original_ProductID` parâmetro e, na `WHERE` cláusula da `UPDATE` instrução, altere o nome do parâmetro usado no `@Original_ProductID` para `@ProductID`. Depois de fazer essas alterações, o T-SQL dentro do procedimento armazenado deve ser semelhante ao seguinte:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample8.sql)]

Para salvar essas alterações no banco de dados, clique no ícone Salvar na barra de ferramentas ou pressione Ctrl + S. Neste ponto, o `Products_Update` procedimento armazenado não espera um `@Original_ProductID` parâmetro de entrada, mas o TableAdapter está configurado para passar esse parâmetro. Você pode ver os parâmetros TableAdapter enviará para o `Products_Update` procedimento armazenado selecionando o TableAdapter no Designer de conjunto de dados, vai para a janela de propriedades, e clicando nas reticências na `UpdateCommand` s `Parameters` coleção. Isso abre a caixa de diálogo do Editor de coleção de parâmetros mostrada na Figura 14.


![As listas de Editor de coleção de parâmetros os parâmetros usados passado para o Products_Update procedimento armazenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image30.png)

**Figura 14**: as listas de Editor de coleção de parâmetros os parâmetros usados passado para o `Products_Update` procedimento armazenado


Você pode remover esse parâmetro aqui simplesmente selecionando o `@Original_ProductID` parâmetro na lista de membros e clicando no botão Remover.

Como alternativa, você pode atualizar os parâmetros usados para todos os métodos de botão direito do mouse no TableAdapter no Designer e escolhendo Configurar. Isso abrirá o Assistente de configuração do TableAdapter, listando os procedimentos armazenados usados para seleção, inserção, atualização, e excluir, junto com os parâmetros de procedimentos armazenados esperam receber. Se você clicar na lista suspensa atualização você pode ver os `Products_Update` procedimentos armazenados esperado parâmetros de entrada, que agora não inclui mais `@Original_ProductID` (consulte a Figura 15). Basta clicar em Concluir para atualizar automaticamente a coleção de parâmetros usada pelo TableAdapter.


[![Como alternativa, você pode usar o Assistente de configuração do TableAdapter s para atualizar suas coleções de parâmetro de métodos](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image31.png)

**Figura 15**: como alternativa, você pode usar o Assistente de configuração para atualizar seu coleções de parâmetros de métodos do TableAdapter s ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Etapa 5: Adicionando métodos adicionais de TableAdapter

Como a etapa 2 ilustrado, ao criar um novo TableAdapter é fácil ter correspondentes procedimentos armazenados gerados automaticamente. O mesmo é verdadeiro ao adicionar métodos adicionais a um TableAdapter. Para ilustrar isso, permitem que s adicione uma `GetProductByProductID(productID)` método para o `ProductsTableAdapter` criado na etapa 2. Esse método levará como entrada um `ProductID` de valor e retornar detalhes sobre o produto especificado.

Inicie o botão direito do mouse no TableAdapter e selecionando Add Query no menu de contexto.


![Adicionar uma nova consulta ao TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image34.png)

**Figura 16**: adicionar uma nova consulta ao TableAdapter


Isso iniciará o Assistente de configuração de consulta do TableAdapter, que primeiro solicita como o TableAdapter deve acessar o banco de dados. Para que um novo procedimento armazenado criado, escolha a criar uma nova opção de procedimento armazenado e clique em Avançar.


[![Escolha a criar um nova opção de procedimento armazenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image35.png)

**Figura 17**: escolha a criar um nova opção de procedimento armazenado ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image37.png))


A próxima tela pergunta conosco para identificar o tipo de consulta a ser executada, se ela retornar um conjunto de linhas ou de um único valor escalar ou executar uma `UPDATE`, `INSERT`, ou `DELETE` instrução. Uma vez que o `GetProductByProductID(productID)` método de retorno de uma linha, deixe o SELECT que retorna a opção de linha selecionada e clique em Avançar.


[![Escolha o SELECT que retorna a opção de linha](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image38.png)

**Figura 18**: escolha a SELECT que retorna a opção de linha ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image40.png))


A próxima tela exibe a TableAdapter s consulta principal, que lista apenas o nome do procedimento armazenado (`dbo.Products_Select`). Substitua o nome do procedimento armazenado a seguir `SELECT` instrução, que retorna todos os campos de produto para um produto especificado:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample9.sql)]


[![Substitua o nome do procedimento armazenado com uma consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image41.png)

**Figura 19**: substituir o nome do procedimento armazenado com um `SELECT` consulta ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image43.png))


A tela subsequente pede para você nomear o procedimento armazenado que será criado. Insira o nome `Products_SelectByProductID` e clique em Avançar.


[![Nomeie o novo Products_SelectByProductID de procedimento armazenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image44.png)

**Figura 20**: Nomeie o novo procedimento armazenado `Products_SelectByProductID` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image46.png))


A etapa final do assistente permite alterar o método nomes gerou, bem como indicam se deseja usar o preenchimento padrão de um DataTable, retornar um DataTable padrão ou ambos. Para esse método, deixe ambas as opções de check- mas renomear os métodos a serem `FillByProductID` e `GetProductByProductID`. Clique em Avançar para exibir um resumo das etapas para executar o assistente e, em seguida, clique em Concluir para concluir o assistente.


[![Renomear os métodos do TableAdapter s FillByProductID e GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image47.png)

**Figura 21**: renomear os métodos do TableAdapter s para `FillByProductID` e `GetProductByProductID` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image49.png))


Depois de concluir o assistente, o TableAdapter tem um novo método disponível, `GetProductByProductID(productID)` que, quando invocada, executará o `Products_SelectByProductID` procedimento armazenado que acabou de ser criado. Reserve um tempo para exibir esse novo procedimento armazenado do Gerenciador de servidores, detalhando a pasta de procedimentos armazenados e abrindo `Products_SelectByProductID` (se você não vê-lo, clique com botão direito na pasta de procedimentos armazenados e escolha Atualizar).

Observe que o `SelectByProductID` procedimento armazenado usa `@ProductID` como um parâmetro de entrada e executa o `SELECT` instrução que inserimos no assistente.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Etapa 6: Criando uma classe de camada de lógica de negócios

Em toda a série de tutoriais, podemos ter strived manter uma arquitetura em camadas em que a camada de apresentação feitas todas as suas chamadas para os negócios lógica BLL (camada). Para cumprir essa decisão de design, primeiro precisamos criar uma classe BLL para o novo DataSet tipado possam acessar dados de produto da camada de apresentação.

Criar um novo arquivo de classe chamado `ProductsBLLWithSprocs.cs` no `~/App_Code/BLL` pasta e adicionar a ele o código a seguir:


[!code-csharp[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample11.cs)]

Essa classe imita a `ProductsBLL` classe semântica de tutoriais anteriores, mas usa o `ProductsTableAdapter` e `ProductsDataTable` objetos do `NorthwindWithSprocs` conjunto de dados. Por exemplo, em vez de ter uma `using NorthwindTableAdapters` instrução no início do arquivo à classe `ProductsBLL` faz, o `ProductsBLLWithSprocs` classe usa `using NorthwindWithSprocsTableAdapters`. Da mesma forma, o `ProductsDataTable` e `ProductsRow` objetos usados nesta classe são prefixados com o `NorthwindWithSprocs` namespace. O `ProductsBLLWithSprocs` classe fornece métodos de acesso a dados de dois `GetProducts` e `GetProductByProductID`, e métodos para adicionar, atualizar e excluir uma instância do produto único.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Etapa 7: Trabalhando com o`NorthwindWithSprocs`conjunto de dados da camada de apresentação

Neste ponto, criamos uma DAL que usa procedimentos armazenados para acessar e modificar os dados do banco de dados subjacente. Também criamos um BLL rudimentar com métodos para recuperar todos os produtos ou um produto específico, juntamente com métodos para adicionar, atualizar e excluir produtos. Para arredondar neste tutorial, let s criar uma página ASP.NET que usa o s BLL `ProductsBLLWithSprocs` classe para exibir, atualizar e excluir registros.

Abra o `NewSprocs.aspx` página o `AdvancedDAL` pasta e arraste um controle GridView na caixa de ferramentas para o Designer, nomeando-o `Products`. De GridView SmartTag s escolha vinculá-la a um novo ObjectDataSource chamado `ProductsDataSource`. Configurar o ObjectDataSource para usar o `ProductsBLLWithSprocs` de classe, conforme mostrado na Figura 22.


[![Configurar o ObjectDataSource para usar a classe ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image50.png)

**Figura 22**: configurar o ObjectDataSource para usar o `ProductsBLLWithSprocs` classe ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image52.png))


A lista suspensa na guia SELECT tem duas opções: `GetProducts` e `GetProductByProductID`. Como queremos exibir todos os produtos no GridView, escolha o `GetProducts` método. As listas suspensas em cada guias UPDATE, INSERT e DELETE tem apenas um método. Certifique-se de que cada uma dessas listas suspensos tem seu método apropriado selecionado e, em seguida, clique em Concluir.

Após o assistente ObjectDataSource, o Visual Studio adicionará BoundFields e um CheckBoxField a GridView para os campos de dados do produto. Ative a edição de GridView s internos e excluir recursos, verificando as opções de habilitar exclusão presentes na marca inteligente e a habilitar edição.


[![A página contém um GridView com editar e excluir suporte habilitado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image53.png)

**Figura 23**: A página contém um GridView com edição e exclusão de suporte habilitado ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image55.png))


Como podemos ve discutido nos tutoriais anteriores, após a conclusão do assistente ObjectDataSource s, conjuntos de Visual Studio a `OldValuesParameterFormatString` propriedade original\_{0}. Isso precisa ser revertido para seu valor padrão de {0} para que os recursos de modificação de dados funcione corretamente, considerando os parâmetros esperados pelos métodos no nosso BLL. Portanto, certifique-se de definir as `OldValuesParameterFormatString` propriedade para {0} ou remova a propriedade completamente a sintaxe declarativa.

Depois de concluir o Assistente Configurar fonte de dados, ativar a edição e exclusão de suporte no GridView e retornando o s ObjectDataSource `OldValuesParameterFormatString` propriedade valor padrão, sua marcação declarativa de página s deve ser semelhante ao seguinte:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/samples/sample12.aspx)]

Neste ponto podemos pode limpar o GridView Personalizando o interface de edição para incluir a validação, tendo a `CategoryID` e `SupplierID` colunas renderizar como DropDownLists e assim por diante. Poderíamos também adicionar uma confirmação do lado do cliente para o botão de exclusão, e eu recomendo que você reserve um tempo para implementar esses aprimoramentos. Uma vez que esses tópicos foram abordados nos tutoriais anteriores, no entanto, não abordaremos-los novamente aqui.

Independentemente se você aumentar o GridView ou não, teste os principais recursos de página s em um navegador. Como mostra a Figura 24, a página lista os produtos em um GridView que fornece por linha edição e exclusão de recursos.


[![Os produtos podem ser exibidos, editados e excluídos do GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image56.png)

**Figura 24**: os produtos podem ser exibidos, editado e excluído do GridView ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs/_static/image58.png))


## <a name="summary"></a>Resumo

Os TableAdapters em um DataSet tipado pode acessar dados do banco de dados usando instruções SQL ad hoc ou por meio de procedimentos armazenados. Ao trabalhar com procedimentos armazenados, qualquer um dos procedimentos armazenados existentes podem ser usados ou o TableAdapter assistente pode ser instruído para criar um novo armazenados procedimentos com base em um `SELECT` consulta. Neste tutorial, exploramos como fazer com que os procedimentos armazenados criados automaticamente para nós.

Tendo os procedimentos armazenados gerados automaticamente ajuda a economizar tempo, há certos casos em que o procedimento armazenado criado pelo assistente t se alinhar com o que seria criamos por conta própria. Um exemplo é o `Products_Update` procedimento armazenado, que espera-se ambos `@Original_ProductID` e `@ProductID` parâmetros de entrada, embora o `@Original_ProductID` parâmetro era supérfluo.

Em muitos cenários, os procedimentos armazenados podem já ter sido criados, ou podemos querer criá-los manualmente para ter um grau de controle sobre os comandos do procedimento armazenado s mais. Em ambos os casos, queremos instruir o TableAdapter para usar procedimentos armazenados existentes para seus métodos. Veremos como fazer isso no próximo tutorial.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Criar e manter procedimentos armazenados](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Recuperação de dados escalar de um procedimento armazenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Conceitos básicos de procedimento armazenado do SQL Server](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Procedimentos armazenados: Uma visão geral](http://www.sqlteam.com/item.asp?ItemID=563)
- [Escrever um procedimento armazenado](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Hilton Geisenow. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Avançar](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
