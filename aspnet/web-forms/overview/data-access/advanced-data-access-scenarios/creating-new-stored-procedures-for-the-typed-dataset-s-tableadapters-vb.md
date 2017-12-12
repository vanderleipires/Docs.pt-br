---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Criar novos procedimentos armazenados para TableAdapters do conjunto de dados tipado (VB) | Microsoft Docs
author: rick-anderson
description: "Nos tutoriais anteriores temos criado instruções SQL em nosso código e passado as instruções para o banco de dados a ser executado. Uma abordagem alternativa é usar s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/18/2007
ms.topic: article
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: 5d250a7fb868d712e8039e65f7219f80ccaa780c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Criar novos procedimentos armazenados para TableAdapters do conjunto de dados tipado (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) ou [baixar PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> Nos tutoriais anteriores temos criado instruções SQL em nosso código e passado as instruções para o banco de dados a ser executado. Uma abordagem alternativa é usar procedimentos armazenados, em que as instruções SQL são predefinidas no banco de dados. Neste tutorial, saiba como o TableAdapter assistente gerar novos procedimentos armazenados para nós.


## <a name="introduction"></a>Introdução

Os dados de acesso DAL (camada) para esses tutoriais usa conjuntos de dados digitados. Como discutido o [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-vb.md) tutorial, com conjuntos de dados consistem em tabelas de dados fortemente tipados e TableAdapters. As tabelas de dados representam as entidades lógicas no sistema enquanto a interface TableAdapters com o banco de dados subjacente para executar o trabalho de acesso de dados. Isso inclui popular tabelas de dados com dados, executar consultas que retornam dados escalares, inserindo, atualizando e excluindo registros do banco de dados.

Os comandos SQL executados pelo qual os TableAdapters podem ser qualquer as instruções SQL ad hoc, como `SELECT columnList FROM TableName`, ou procedimentos armazenados. Os TableAdapters em nossa arquitetura usar instruções SQL ad hoc. Muitos desenvolvedores e administradores de banco de dados, no entanto, preferem procedimentos armazenados instruções de SQL ad hoc por motivos de segurança, facilidade de manutenção e a capacidade de atualização. Outras pessoas preferem ardently instruções de SQL ad hoc para sua flexibilidade. No meu próprio trabalho favorecer procedimentos armazenados em instruções SQL ad hoc, mas decidiu usar instruções SQL ad hoc para simplificar os tutoriais anteriores.

Quando definir um TableAdapter ou adição de novos métodos, o TableAdapter s assistente torna tão fáceis de criar novos procedimentos armazenados ou usar procedimentos armazenados existentes, como faz para usar instruções SQL ad hoc. Neste tutorial, examinaremos como TableAdapter s Assistente para geração automática de procedimentos armazenados. O tutorial Avançar examinaremos como configurar os métodos TableAdapter para usar os procedimentos armazenados existentes ou criados manualmente.

> [!NOTE]
> Consulte a entrada de blog de Rob Howard [Don Use ainda procedimentos armazenados t?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) e [Frans Bouma](https://weblogs.asp.net/fbouma/) entrada de blog s [procedimentos armazenados são ruins, Kay M?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) para um debate envolvente sobre os prós e contras do procedimentos armazenados e SQL ad hoc.


## <a name="stored-procedure-basics"></a>Conceitos básicos de procedimento armazenado

Funções são uma construção comum para todas as linguagens de programação. Uma função é uma coleção de instruções que são executadas quando a função é chamada. Funções podem aceitar parâmetros de entrada e, opcionalmente, podem retornar um valor. *[Procedimentos armazenados](http://en.wikipedia.org/wiki/Stored_procedure)*  são construções de banco de dados que compartilham muitas semelhanças com funções em linguagens de programação. Um procedimento armazenado é composto de um conjunto de instruções T-SQL que são executados quando o procedimento armazenado é chamado. Um procedimento armazenado pode aceitar zero para muitos parâmetros de entrada e pode retornar valores escalares, parâmetros de saída, ou, mais comumente, conjuntos de resultados de `SELECT` consultas.

> [!NOTE]
> Procedimentos armazenados são muitas vezes, conhecidos como sprocs ou SPs.


Procedimentos armazenados são criados usando o [ `CREATE PROCEDURE` ](https://msdn.microsoft.com/en-us/library/aa258259(SQL.80).aspx) instrução T-SQL. Por exemplo, o script T-SQL a seguir cria um procedimento armazenado denominado `GetProductsByCategoryID` que assume um parâmetro único chamado `@CategoryID` e retorna o `ProductID`, `ProductName`, `UnitPrice`, e `Discontinued` campos dessas colunas do `Products` tabela que tenha uma correspondência `CategoryID` valor:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Quando esse procedimento armazenado tiver sido criado, ele pode ser chamado usando a seguinte sintaxe:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> O seguinte tutorial, examinaremos Criando procedimentos armazenados por meio do IDE do Visual Studio. Para este tutorial, no entanto, vamos deixar o Assistente de TableAdapter a gerar automaticamente os procedimentos armazenados para nós.


Além de simplesmente retornar dados, procedimentos armazenados são geralmente usados para executar vários comandos de banco de dados dentro do escopo de uma única transação. Um procedimento armazenado denominado `DeleteCategory`, por exemplo, pode usar em um `@CategoryID` parâmetro e executar duas `DELETE` instruções: primeiro, um para excluir os produtos relacionados e uma segunda excluir a categoria especificada. Várias instruções em um procedimento armazenado são *não* automaticamente encapsulado dentro de uma transação. Comandos T-SQL adicionais precisam ser emitido para garantir que o procedimento armazenado s que vários comandos são tratados como uma operação atômica. Veremos como encapsular comandos um procedimento armazenado dentro do escopo de uma transação no tutorial subsequente.

Ao usar procedimentos armazenados dentro de uma arquitetura, métodos de camada de acesso a dados invocar um procedimento armazenado específico em vez de emitir uma instrução de SQL ad hoc. Isso centraliza o local das instruções SQL executadas (no banco de dados) em vez de deixá-la definida dentro da arquitetura do aplicativo s. Essa centralização indiscutivelmente torna mais fácil localizar, analisar e ajustar as consultas e fornece uma imagem mais nítida sobre onde e como o banco de dados está sendo usado.

Para obter mais informações sobre conceitos básicos de procedimento armazenado, consulte os recursos na seção leitura adicional no final deste tutorial.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Etapa 1: Criando páginas da Web avançada de dados acesso camada cenários

Antes de começar nossa discussão sobre como criar uma DAL usando procedimentos armazenados, permitem s primeiro dedicar um tempo para criar as páginas ASP.NET em nosso projeto de site que será necessário para este e os próximos vários tutoriais. Comece adicionando uma nova pasta chamada `AdvancedDAL`. Em seguida, adicione as seguintes páginas do ASP.NET para a pasta, certificando-se de associar cada página com o `Site.master` página mestre:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`


![Adicionar as páginas ASP.NET para os tutoriais de cenários de camada de acesso avançada de dados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: adicionar as páginas ASP.NET para os tutoriais de cenários de camada de acesso avançada de dados


Como em outras pastas, `Default.aspx` no `AdvancedDAL` pasta listará os tutoriais em sua seção. Lembre-se de que o `SectionLevelTutorialListing.ascx` controle de usuário fornece essa funcionalidade. Portanto, adicione este controle de usuário para `Default.aspx` arrastando-o no Gerenciador de soluções para a página de exibição de Design de s.


[![Adicionar o controle de usuário SectionLevelTutorialListing.ascx a Default.aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**Figura 2**: adicionar o `SectionLevelTutorialListing.ascx` controle de usuário `Default.aspx` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))


Por fim, adicione estas páginas como entradas para o `Web.sitemap` arquivo. Especificamente, adicione a seguinte marcação depois de trabalhar com dados em lotes `<siteMapNode>`:


[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

Após a atualização `Web.sitemap`, reserve um tempo para exibir o site de tutoriais através de um navegador. No menu à esquerda agora inclui itens para os tutoriais de cenários avançados DAL.


![O mapa do Site agora inclui entradas para os tutoriais de cenários avançados de DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Figura 3**: O mapa de Site agora inclui entradas para os tutoriais de cenários avançados de DAL


## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Etapa 2: Configurar um TableAdapter para criar novos procedimentos armazenados

Para demonstrar a criação de uma camada de acesso de dados que usa procedimentos armazenados em vez de instruções SQL ad hoc, permitem s criar um conjunto de dados tipado novo no `~/App_Code/DAL` pasta denominada `NorthwindWithSprocs.xsd`. Como podemos ter passado por esse processo em detalhes nos tutoriais anteriores, podemos continuará rapidamente as etapas aqui. Se você ficar presa ou precisar de mais instruções passo a passo na criação e configuração de um conjunto de dados tipado, consultar o [criando uma camada de acesso a dados](../introduction/creating-a-data-access-layer-vb.md) tutorial.

Adicionar um novo conjunto de dados ao projeto clicando no `DAL` pasta, escolha Adicionar Novo Item e selecionando o modelo de conjunto de dados, conforme mostrado na Figura 4.


[![Adicionar um novo conjunto de dados tipado ao projeto nomeado NorthwindWithSprocs.xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Figura 4**: adicionar um novo DataSet digitado para o projeto denominado `NorthwindWithSprocs.xsd` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))


Isso será criar novo conjunto de dados tipado, abra seu Designer, criar um novo TableAdapter e iniciar o Assistente de configuração do TableAdapter. A primeira etapa do Assistente de configuração TableAdapter s solicitará que você selecione o banco de dados para trabalhar com. A cadeia de caracteres de conexão para o banco de dados Northwind deve ser listada na lista suspensa. Selecione esta opção e clique em Avançar.

Nesta próxima tela, podemos escolher como o TableAdapter deve acessar o banco de dados. Nos tutoriais anteriores, selecionamos a primeira opção, as instruções SQL de uso. Para este tutorial, selecione a segunda opção, criar novos procedimentos armazenados e clique em Avançar.


[![Instrua o TableAdpater para criar novos procedimentos armazenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Figura 5**: instruir o TableAdpater para criar novos procedimentos armazenados ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))


Assim como com o uso de instruções SQL ad hoc, na etapa a seguir, são solicitados a fornecer o `SELECT` instrução da consulta principal do TableAdapter s. Mas em vez de usar o `SELECT` instrução digitada aqui para executar uma consulta ad hoc diretamente, o Assistente de s TableAdapter criará um procedimento armazenado que contém essa `SELECT` consulta.

Use a seguinte `SELECT` consulta para essa TableAdapter:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]


[![Insira a consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Figura 6**: insira o `SELECT` consulta ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))


> [!NOTE]
> A consulta acima difere ligeiramente a consulta principal de `ProductsTableAdapter` no `Northwind` conjunto de dados tipado. Lembre-se de que o `ProductsTableAdapter` no `Northwind` conjunto de dados tipado inclui duas subconsultas correlatas para reverter o nome da categoria e o nome da empresa para cada categoria de produto s e fornecedor. No futuros [atualizando o TableAdapter para usar junções](updating-the-tableadapter-to-use-joins-vb.md) tutorial, examinaremos a inclusão dessa dados relacionados a esse TableAdapter.


Aproveite e clique no botão Opções avançadas. Aqui, podemos especificar se o assistente deve também gerar insert, update e delete instruções para o TableAdapter, se deseja usar a simultaneidade otimista e se a tabela de dados deve ser atualizada após inserções e atualizações. A opção de instruções Generate Insert, Update e Delete é marcada por padrão. Deixe marcada. Para este tutorial, deixe as opções de simultaneidade otimista de uso está desmarcada.

Quando tiver os procedimentos armazenados criados automaticamente pelo Assistente de TableAdapter, parece que a atualização, a opção de tabela de dados será ignorada. Independentemente de se essa caixa de seleção estiver marcada, o resultante insert e update procedimentos armazenados recuperar o registro just-inserida ou atualizada imediatamente, conforme veremos na etapa 3.


![Deixe as instruções Generate Insert, Update e Delete opção marcada](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Figura 7**: deixar as instruções Generate Insert, Update e Delete opção marcada


> [!NOTE]
> Se o uso da opção de simultaneidade otimista é marcado, o assistente adicionará as condições adicionais para o `WHERE` cláusula que impedir que os dados sejam atualizados se houve alterações em outros campos. Voltar para o [Implementando a simultaneidade otimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) tutorial para obter mais informações sobre como usar o recurso de controle de simultaneidade otimista internos do TableAdapter s.


Depois de inserir o `SELECT` consultar e confirmar que a opção de instruções Generate Insert, Update e Delete está marcada, clique em Avançar. Essa tela seguinte, mostrada na Figura 8, solicita que os nomes dos procedimentos armazenados, que o assistente criará para selecionar, inserir, atualizar e excluir dados. Alterar os nomes de procedimentos para esses armazenado `Products_Select`, `Products_Insert`, `Products_Update`, e `Products_Delete`.


[![Renomeie os procedimentos armazenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 8**: renomear procedimentos armazenados ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))


Para ver o T-SQL que o Assistente de TableAdapter usará para criar quatro procedimentos armazenados, clique no botão Visualizar Script SQL. Na caixa de diálogo Visualizar Script SQL, você pode salvar o script em um arquivo ou copiá-lo para a área de transferência.


![Visualizar o Script SQL usado para gerar os procedimentos armazenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 9**: visualizar o Script SQL usado para gerar os procedimentos armazenados


Depois de nomear os procedimentos armazenados, clique em Avançar métodos correspondentes do nome de s TableAdapter. Assim como ao usar instruções SQL ad hoc, podemos criar métodos que preencher uma DataTable existente ou retornam uma nova. Podemos também pode especificar se o TableAdapter deve incluir o padrão de direta do banco de dados para inserir, atualizar e excluir registros. Deixe todas as caixas de três seleção verificadas, mas renomear o retorno de um método de DataTable para `GetProducts` (conforme mostrado na Figura 10).


[![Nomeie os métodos de preenchimento e GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Figura 10**: nomear os métodos `Fill` e `GetProducts` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))


Clique em Avançar para ver um resumo das etapas que o assistente executará. Conclua o assistente clicando no botão Concluir. Depois que o assistente for concluído, você será retornado para o s DataSet Designer, que agora deve incluir o `ProductsDataTable`.


[![O Designer de conjunto de dados s mostra o recém-adicionado ProductsDataTable](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Figura 11**: O DataSet Designer mostra adicionado recentemente `ProductsDataTable` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))


## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Etapa 3: Examinar os procedimentos armazenados criados recentemente

O Assistente de TableAdapter usado na etapa 2 automaticamente criou os procedimentos armazenados para selecionar, inserir, atualizar e excluir dados. Esses procedimentos armazenados podem ser exibidos ou modificados por meio do Visual Studio, vá para o Gerenciador de servidores e drill-down até a pasta de procedimentos armazenados do banco de dados s. Como mostra a Figura 12, o banco de dados Northwind contém quatro novos procedimentos armazenados: `Products_Delete`, `Products_Insert`, `Products_Select`, e `Products_Update`.


![Os quatro procedimentos armazenados criados na etapa 2 podem ser encontrados na pasta de procedimentos armazenados do banco de dados s](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Figura 12**: quatro procedimentos armazenados criados na etapa 2 podem ser encontrados na pasta de procedimentos armazenados do banco de dados s


> [!NOTE]
> Se você não vir o Gerenciador de servidores, vá para o menu de exibição e escolha a opção de Gerenciador de servidores. Se você não vir os procedimentos armazenados relacionados ao produto adicionados na etapa 2, tente clicando duas vezes na pasta de procedimentos armazenados e escolha atualizar.


Para exibir ou modificar um procedimento armazenado, clique duas vezes em seu nome no Gerenciador de servidores ou, como alternativa, clique com botão direito no procedimento armazenado e escolha Abrir. A Figura 13 mostra o `Products_Delete` procedimento armazenado, quando aberto.


[![Procedimentos armazenados podem ser abertos e modificados de dentro do Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Figura 13**: armazenados procedimentos podem ser abertos e modificados de no Visual Studio ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))


O conteúdo de ambas as `Products_Delete` e `Products_Select` procedimentos armazenados são bastante simples. O `Products_Insert` e `Products_Update` procedimentos armazenados, por outro lado, garantem um exame como ambos, execute um `SELECT` instrução após sua `INSERT` e `UPDATE` instruções. Por exemplo, o SQL a seguir faz de `Products_Insert` procedimento armazenado:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

O procedimento armazenado aceita como parâmetros de entrada de `Products` colunas que foram retornadas pelo `SELECT` consulta especificada no Assistente de s de TableAdapter e esses valores são usados em um `INSERT` instrução. Após o `INSERT` instrução, uma `SELECT` consulta é usada para retornar o `Products` valores de coluna (incluindo o `ProductID`) do registro recém-adicionada. Esse recurso de atualização é útil quando adicionar um novo registro usando o padrão de atualização em lotes que automaticamente atualiza recém-adicionado `ProductRow` instâncias `ProductID` propriedades com os valores incrementado automaticamente atribuídos pelo banco de dados.

O código a seguir ilustra esse recurso. Ele contém um `ProductsTableAdapter` e `ProductsDataTable` criado para o `NorthwindWithSprocs` conjunto de dados tipado. Um novo produto é adicionado ao banco de dados, criando um `ProductsRow` instância, fornecendo seus valores e, em seguida, chamar o TableAdapter s `Update` método, passando o `ProductsDataTable`. Internamente, o TableAdapter s `Update` método enumera o `ProductsRow` instâncias na DataTable transmitido (neste exemplo há apenas um: um que acabamos de adicionar) e executa apropriada inserir, atualizar ou excluir o comando. Nesse caso, o `Products_Insert` procedimento armazenado é executado, o que adiciona um novo registro para o `Products` de tabela e retorna os detalhes do registro adicionado recentemente. O `ProductsRow` instância s `ProductID` valor é atualizado. Após o `Update` método foi concluída, podemos acessar o registro recém-adicionados s `ProductID` valor por meio de `ProductsRow` s `ProductID` propriedade.


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

O `Products_Update` procedimento armazenado da mesma forma inclui um `SELECT` instrução após seu `UPDATE` instrução.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Observe que este procedimento armazenado inclui dois parâmetros de entrada para `ProductID`: `@Original_ProductID` e `@ProductID`. Essa funcionalidade permite cenários onde a chave primária pode ser alterada. Por exemplo, em um banco de dados de funcionário, cada registro de funcionário pode usar o número de seguro social funcionário s como suas chaves primárias. Para alterar um número de seguro social existente funcionário s, o novo número de seguro social e original devem ser fornecidos. Para o `Products` tabela, essa funcionalidade não é necessária porque o `ProductID` coluna é uma `IDENTITY` coluna e nunca deve ser alterada. Na verdade, o `UPDATE` instrução o `Products_Update` procedimento armazenado incluir o `ProductID` coluna na sua lista de colunas. Portanto, enquanto `@Original_ProductID` é usado no `UPDATE` instrução s `WHERE` cláusula, isso é supérfluo para o `Products` de tabela e pode ser substituído pelo `@ProductID` parâmetro. Ao modificar um procedimento armazenado s de parâmetros é importante que os métodos TableAdapter que usam esse procedimento armazenado também são atualizados.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Etapa 4: Modificar um procedimento armazenado de parâmetros de s e atualizando o TableAdapter

Como o `@Original_ProductID` parâmetro supérfluo, permitem s removê-lo do `Products_Update` procedimento armazenado completamente. Abra o `Products_Update` procedimento armazenado, excluir o `@Original_ProductID` parâmetro e, no `WHERE` cláusula do `UPDATE` instrução, altere o nome do parâmetro usado em `@Original_ProductID` para `@ProductID`. Depois de fazer essas alterações, o T-SQL dentro do procedimento armazenado deve ser semelhante ao seguinte:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Para salvar essas alterações no banco de dados, clique no ícone Salvar na barra de ferramentas ou pressione Ctrl + S. Neste ponto, o `Products_Update` procedimento armazenado não espera um `@Original_ProductID` parâmetro de entrada, mas o TableAdapter está configurado para passar tal parâmetro. Você pode ver os parâmetros TableAdapter enviará para o `Products_Update` procedimento armazenado selecionando o TableAdapter no Designer de conjunto de dados, vai para a janela de propriedades e clicando nas reticências no `UpdateCommand` s `Parameters` coleção. Isso abre a caixa de diálogo Editor de coleção de parâmetros mostrada na Figura 14.


![As listas de Editor de coleção de parâmetros os parâmetros usados passado para o Products_Update procedimento armazenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Figura 14**: as listas de Editor de coleção de parâmetros os parâmetros usados passado para o `Products_Update` procedimento armazenado


Você pode remover esse parâmetro aqui, basta selecionar o `@Original_ProductID` parâmetro da lista de membros e clicar no botão Remover.

Como alternativa, você pode atualizar os parâmetros usados para todos os métodos clicando duas vezes no TableAdapter no Designer e escolhendo Configurar. Isso abrirá o Assistente de configuração do TableAdapter, listando os procedimentos armazenados usados para selecionar, inserir, atualizar, e excluir, junto com os parâmetros de procedimentos armazenados esperam receber. Se você clicar na lista suspensa de atualização que você pode ver o `Products_Update` procedimentos armazenados esperado de parâmetros de entrada, que agora não inclui mais `@Original_ProductID` (consulte a Figura 15). Basta clicar em Concluir para atualizar automaticamente a coleção de parâmetros usada pelo TableAdapter.


[![Você também pode usar o Assistente de configuração do TableAdapter s para atualizar suas coleções de parâmetros de métodos](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 15**: como alternativa, você pode usar o Assistente de configuração para atualizar seu coleções de parâmetros de métodos TableAdapter s ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))


## <a name="step-5-adding-additional-tableadapter-methods"></a>Etapa 5: Adicionando métodos TableAdapter adicionais

Como etapa 2 ilustrado, ao criar um novo TableAdapter é fácil para os procedimentos armazenados correspondente gerado automaticamente. O mesmo é verdadeiro quando a adição de métodos adicionais para um TableAdapter. Para ilustrar isso, permitem s adicionar um `GetProductByProductID(productID)` método para o `ProductsTableAdapter` criado na etapa 2. Esse método levará como entrada uma `ProductID` valor e retornar detalhes sobre o produto especificado.

Iniciar clicando duas vezes no TableAdapter e escolhendo Adicionar consulta no menu de contexto.


![Adicionar uma nova consulta do TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 16**: adicionar uma nova consulta do TableAdapter


Isso iniciará o Assistente de configuração de consulta do TableAdapter, que primeiro solicita como o TableAdapter deve acessar o banco de dados. Para que um novo procedimento armazenado criado, escolha criar uma nova opção de procedimento armazenado e clique em Avançar.


[![Escolher criar um nova opção de procedimento armazenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Figura 17**: escolha a criar um novo procedimento armazenado opção ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))


A próxima tela pergunta para identificar o tipo de consulta a ser executada se irá retornar um conjunto de linhas ou um único valor escalar, ou executar um `UPDATE`, `INSERT`, ou `DELETE` instrução. Desde o `GetProductByProductID(productID)` método de retorno de uma linha, deixe SELECT que retorna a opção de linha selecionado e pressione próximo.


[![Escolha o SELECT que retorna a opção de linha](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**Figura 18**: escolha SELECT que retorna a opção de linha ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))


A próxima tela exibe a TableAdapter s consulta principal, que lista apenas o nome do procedimento armazenado (`dbo.Products_Select`). Substitua o nome do procedimento armazenado a seguir `SELECT` instrução, que retorna todos os campos de produto para um produto específico:


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]


[![Substitua o nome do procedimento armazenado com uma consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**Figura 19**: Substitua o nome do procedimento armazenado com um `SELECT` consulta ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))


A tela subsequente lhe pede para nomear o procedimento armazenado que será criado. Digite o nome `Products_SelectByProductID` e clique em Avançar.


[![Nome do novo Products_SelectByProductID de procedimento armazenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 20**: nome do novo procedimento armazenado `Products_SelectByProductID` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))


A etapa final do assistente permite alterar o método nomes gerados assim como indicam se deseja usar o preenchimento padrão de um DataTable, retornar um padrão de DataTable ou ambos. Para esse método, deixe as duas opções verificadas, mas renomear os métodos para `FillByProductID` e `GetProductByProductID`. Clique em Avançar para exibir um resumo das etapas de executar o assistente e, em seguida, clique em Concluir para concluir o assistente.


[![Renomeie os métodos TableAdapter FillByProductID e GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Figura 21**: renomear os métodos TableAdapter para `FillByProductID` e `GetProductByProductID` ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))


Depois de concluir o assistente, o TableAdapter tem um novo método disponível, `GetProductByProductID(productID)` que, quando chamado, executará o `Products_SelectByProductID` procedimento armazenado que acabou de ser criado. Reserve um tempo para exibir esse novo procedimento armazenado do Gerenciador de servidores, detalhando a pasta de procedimentos armazenados e abrindo `Products_SelectByProductID` (se você não vê-lo, clique com botão direito na pasta de procedimentos armazenados e escolha Atualizar).

Observe que o `SelectByProductID` armazenado procedimento leva `@ProductID` como um parâmetro de entrada e executa o `SELECT` instrução que são inseridas no assistente.


[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Etapa 6: Criando uma classe de camada de lógica de negócios

Em toda a série de tutoriais, podemos ter strived manter uma arquitetura em camadas em que a camada de apresentação feitas todas suas chamadas para os negócios lógica BLL (camada). Para cumprir essa decisão de design, primeiro, precisamos criar uma classe BLL para o novo conjunto de dados digitado antes de acessar dados de produto da camada de apresentação.

Criar um novo arquivo de classe chamado `ProductsBLLWithSprocs.vb` no `~/App_Code/BLL` pasta e adicione a ele o seguinte código:


[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Essa classe imita a `ProductsBLL` classe semântica de tutoriais anteriores, mas usa o `ProductsTableAdapter` e `ProductsDataTable` objetos do `NorthwindWithSprocs` conjunto de dados. Por exemplo, em vez de um `Imports NorthwindTableAdapters` instrução no início do arquivo de classe como `ProductsBLL` faz, o `ProductsBLLWithSprocs` classe usa `Imports NorthwindWithSprocsTableAdapters`. Da mesma forma, o `ProductsDataTable` e `ProductsRow` objetos usados nesta classe são prefixados com o `NorthwindWithSprocs` namespace. O `ProductsBLLWithSprocs` classe fornece métodos de acesso a dois dados `GetProducts` e `GetProductByProductID`, e métodos para adicionar, atualizar e excluir uma instância do produto único.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Etapa 7: Trabalhando com o`NorthwindWithSprocs`conjunto de dados da camada de apresentação

Neste ponto, você criou uma DAL que usa procedimentos armazenados para acessar e modificar os dados do banco de dados subjacente. Também criamos um BLL rudimentar com métodos para recuperar todos os produtos ou um produto específico junto com os métodos para adicionar, atualizar e excluir produtos. Para arredondar neste tutorial, vamos s criar uma página ASP.NET que usa o s BLL `ProductsBLLWithSprocs` classe para exibir, atualizar e excluir registros.

Abra o `NewSprocs.aspx` página o `AdvancedDAL` pasta e arraste um controle GridView da caixa de ferramentas para o Designer, nomeando- `Products`. De GridView marca inteligente s escolha para associá-lo para um novo ObjectDataSource denominado `ProductsDataSource`. Configurar o ObjectDataSource para usar o `ProductsBLLWithSprocs` de classe, conforme mostrado na Figura 22.


[![Configurar o ObjectDataSource para usar a classe ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**Figura 22**: configurar o ObjectDataSource para usar o `ProductsBLLWithSprocs` classe ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))


A lista suspensa na guia selecione tem duas opções: `GetProducts` e `GetProductByProductID`. Como desejamos exibir todos os produtos em GridView, escolha o `GetProducts` método. A lista suspensa em cada guias UPDATE, INSERT e DELETE ter apenas um método. Certifique-se de que cada uma dessas listas suspensas tem seu método apropriado selecionado e, em seguida, clique em Concluir.

Depois de concluir o assistente ObjectDataSource, o Visual Studio adicionará BoundFields e um CheckBoxField GridView para os campos de dados de produto. Ative a edição do GridView s internos e excluir recursos, verificando os habilitar edição e as opções de permitir exclusão presentes na marca inteligente.


[![A página contém um GridView com editar e excluir suporte habilitado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**Figura 23**: A página contém um GridView com edição e exclusão de suporte está habilitado ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))


Como podemos ve discutidos nos tutoriais anteriores, após a conclusão do assistente ObjectDataSource s, conjuntos do Visual Studio a `OldValuesParameterFormatString` propriedade original\_{0}. Isso precisa ser revertido para seu valor padrão de {0} para que os recursos de modificação de dados funcione corretamente fornecido aos parâmetros esperados pelos métodos em nosso BLL. Portanto, certifique-se de definir o `OldValuesParameterFormatString` propriedade {0} ou remova a propriedade completamente a sintaxe declarativa.

Após concluir o Assistente Configurar fonte de dados, ativando editar e excluir suporte em GridView e retornando o s ObjectDataSource `OldValuesParameterFormatString` propriedade valor padrão, sua marcação declarativa s de página deve ser semelhante ao seguinte:


[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

Neste ponto, pode organizar GridView Personalizando a interface de edição para incluir validação, tendo o `CategoryID` e `SupplierID` colunas processam como DropDownLists e assim por diante. Poderíamos também adicionar uma confirmação do lado do cliente para o botão de exclusão, e você deve levar tempo para implementar esses aprimoramentos. Desde que estes tópicos foram abordados nos tutoriais anteriores, no entanto, não abordaremos-los novamente aqui.

Independentemente de se você aumentar o GridView ou não, teste os recursos de núcleo de páginas em um navegador. Como mostra a Figura 24, a página lista os produtos em um GridView fornecidos por linha edição e exclusão de recursos.


[![Os produtos podem ser exibidos, editados e excluídos do GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Figura 24**: os produtos podem ser exibidos, editada e excluídos de GridView ([clique para exibir a imagem em tamanho normal](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))


## <a name="summary"></a>Resumo

Os TableAdapters em um conjunto de dados tipado pode acessar dados do banco de dados usando instruções SQL ad hoc ou através de procedimentos armazenados. Ao trabalhar com procedimentos armazenados, qualquer um dos procedimentos armazenados existentes podem ser usados ou TableAdapter wizard pode ser instruído para criar um novo armazenados procedimentos com base em um `SELECT` consulta. Neste tutorial, explorados como os procedimentos armazenados criados automaticamente para nós.

Embora tenha os procedimentos armazenados gerados automaticamente ajuda a economizar tempo, há alguns casos em que o procedimento armazenado criado pelo assistente t alinhar com o que seria criamos por conta própria. Um exemplo é o `Products_Update` procedimento armazenado, que espera ambos `@Original_ProductID` e `@ProductID` parâmetros de entrada, embora o `@Original_ProductID` parâmetro era supérfluo.

Em muitos cenários, os procedimentos armazenados podem já ter sido criados, ou podemos desejar criá-los manualmente para que um maior grau de controle sobre os comandos de s do procedimento armazenado. Em qualquer caso, queremos instruir o TableAdapter usar procedimentos armazenados existentes para seus métodos. Veremos como fazer isso no tutorial Avançar.

Boa programação!

## <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Criar e manter procedimentos armazenados](https://msdn.microsoft.com/en-us/library/aa214299(SQL.80).aspx)
- [Recuperação de dados escalar de um procedimento armazenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Conceitos básicos de procedimento armazenado do SQL Server](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Procedimentos armazenados: Uma visão geral](http://www.sqlteam.com/item.asp?ItemID=563)
- [Gravar um procedimento armazenado](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Geisenow Hilton. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
[Próximo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
