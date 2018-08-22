---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: Interagindo com a página mestre da página de conteúdo (VB) | Microsoft Docs
author: rick-anderson
description: Examina como chamar métodos, definir propriedades, etc. da página mestra do código na página de conteúdo.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 59a00305cdcaf41ac0b37649382b9c3dc9ce1b0c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825623"
---
<a name="interacting-with-the-master-page-from-the-content-page-vb"></a>Interagindo com a página mestre da página de conteúdo (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Examina como chamar métodos, definir propriedades, etc. da página mestra do código na página de conteúdo.


## <a name="introduction"></a>Introdução

Ao longo dos últimos cinco tutoriais, examinamos como criar uma página mestra, definir áreas de conteúdo, associar as páginas do ASP.NET para uma página mestra e definir o conteúdo específico da página. Quando um visitante solicita uma página de conteúdo específica, o conteúdo e a marcação de páginas mestras são combinados em tempo de execução, resultando no processamento de uma hierarquia de controle unificada. Portanto, nós já vimos uma forma em que a página mestra e uma das suas páginas de conteúdo podem interagir: a página de conteúdo esclarece a marcação para transfuse em controles de ContentPlaceHolder da página mestra.

O que ainda precisamos examinar é como a página mestra e a página de conteúdo podem interagir programaticamente. Além de definir a marcação para controles de ContentPlaceHolder da página mestra, uma página de conteúdo também pode atribuir valores para as propriedades públicas da sua página mestra e invocar seus métodos públicos. Da mesma forma, uma página mestra pode interagir com suas páginas de conteúdo. Enquanto a interação programática entre uma página mestra e de conteúdo é menos comum do que a interação entre suas marcações declarativas, há muitos cenários em que essa interação programática é necessária.

Neste tutorial, podemos examinar como uma página de conteúdo por meio de programação pode interagir com sua página mestra; no próximo tutorial vamos examinar como a página mestra da mesma forma pode interagir com suas páginas de conteúdo.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Exemplos de interação programática entre uma página de conteúdo e a página mestra

Quando uma região específica de uma página precisa ser configurado em uma base de página por página, podemos usar um controle ContentPlaceHolder. Mas e quanto situações nas quais a maioria das páginas precisa emitir uma saída de determinados, mas um pequeno número de páginas necessário personalizá-lo para mostrar algo mais? Um exemplo, que são examinados na [ *vários ContentPlaceHolders e conteúdo padrão* ](multiple-contentplaceholders-and-default-content-vb.md) envolve o tutorial, exibindo uma interface de logon em cada página. Enquanto a maioria das páginas deve incluir uma interface de logon, ele deve ser suprimido para algumas páginas, tais como: a página de logon principal (`Login.aspx`); a página Criar conta; e outras páginas que são acessíveis apenas a usuários autenticados. O [ *vários ContentPlaceHolders e conteúdo padrão* ](multiple-contentplaceholders-and-default-content-vb.md) tutorial mostrou como definir o conteúdo padrão para um ContentPlaceHolder na página mestra e, em seguida, como substituí-lo nessas páginas onde o não foi queria o conteúdo padrão.

Outra opção é criar uma propriedade pública ou método dentro da página mestra que indica se deve mostrar ou ocultar a interface de logon. Por exemplo, a página mestra pode incluir uma propriedade pública denominada `ShowLoginUI` cujo valor foi usado para definir o `Visible` propriedade do controle de logon na página mestra. Essas páginas de conteúdo em que a interface do usuário de logon deve ser suprimida depois pode definir programaticamente a `ShowLoginUI` propriedade para `False`.

Talvez o exemplo mais comum de conteúdo e interação de página mestra ocorre quando os dados exibidos na página mestra precisa ser atualizada depois que alguma ação ocorridos na página de conteúdo. Considere uma página mestra que inclui um GridView que exibe os cinco mais recentemente adicionados registros de uma tabela de banco de dados específico e se uma das suas páginas de conteúdo inclui uma interface para adicionar novos registros para essa mesma tabela.

Quando um usuário visita a página para adicionar um novo registro, ela vê que os cinco mais recentemente adicionado registros exibidos na página mestra. Depois de preencher os valores para colunas do novo registro, ela envia o formulário. Supondo que o GridView na página mestra tem seu `EnableViewState` propriedade definida como True (padrão), seu conteúdo é recarregado do estado de exibição e, consequentemente, os mesmos cinco registros são exibidos, mesmo que um registro mais recente acabou de ser adicionado ao banco de dados. Isso pode confundir o usuário.

> [!NOTE]
> Mesmo se você desabilitar o estado de exibição do GridView, de modo que ele se reconecta à sua fonte de dados subjacente em cada postagem, ele ainda não mostrará o registro adicionado apenas porque os dados serão associados ao GridView anteriormente no ciclo de vida de página que quando o novo registro é adicionado para o datab ASE.


Para corrigir isso, para que o registro just-adicionado é exibido na página mestra do GridView no postback, precisamos instruir o GridView para reassociar a fonte de dados *depois de* foi adicionado o novo registro ao banco de dados. Isso exige interação entre o conteúdo e páginas mestras, porque a interface para adicionar que o novo registro (e seus manipuladores de eventos) estão na página de conteúdo, mas o GridView que precisa ser atualizado está na página mestra.

Como atualizar a exibição da página mestra de um manipulador de eventos na página de conteúdo é uma das necessidades mais comuns para o conteúdo e interação de página mestra, vamos explorar esse tópico em mais detalhes. O download para este tutorial inclui um banco de dados do Microsoft SQL Server 2005 Express Edition denominado `NORTHWIND.MDF` no site do `App_Data` pasta. O banco de dados Northwind armazena informações de vendas de uma empresa fictícia, a Northwind Traders, funcionário e produto.

Etapa 1 explica exibindo os cinco mais recentemente adicionado produtos em um GridView na página mestra. Etapa 2 cria uma página de conteúdo para a adição de novos produtos. Etapa 3 aborda como criar métodos e propriedades públicas na página mestra e etapa 4 ilustra como interagir programaticamente com essas propriedades e métodos de página de conteúdo.

> [!NOTE]
> Este tutorial não me aprofundar nas especificações de trabalhar com dados no ASP.NET. As etapas para configurar a página mestra para exibir dados e a página de conteúdo para a inserção de dados estiverem concluídas, implantações tranquilas ainda. Para obter uma análise mais detalhada na exibição e inserção de dados e usando os controles SqlDataSource e GridView, consulte os recursos na seção Leituras adicionais no final deste tutorial.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Etapa 1: Exibir os cinco mais recentemente adicionado produtos na página mestra

Abra a página mestra do site e adicione um rótulo e um controle GridView para o `leftContent` `<div>`. Desmarque out do rótulo `Text` propriedade, defina seu `EnableViewState` propriedade a ser `False`e sua `ID` propriedade a ser `GridMessage`; definir o GridView `ID` propriedade para `RecentProducts`. Em seguida, no Designer, expanda a marca inteligente do GridView e escolha vinculá-la a uma nova fonte de dados. Isso inicia o Assistente de configuração de fonte de dados. Porque o banco de dados Northwind na `App_Data` pasta é um banco de dados do Microsoft SQL Server, optar por criar um SqlDataSource selecionando (veja a Figura 1); nome SqlDataSource `RecentProductsDataSource`.


[![Associar um controle SqlDataSource chamado RecentProductsDataSource GridView](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Figura 01**: associar o GridView para um controle SqlDataSource chamado `RecentProductsDataSource` ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))


A próxima etapa nos pede para especificar qual banco de dados para se conectar ao. Escolha o `NORTHWIND.MDF` arquivo na lista suspensa de banco de dados e clique em Avançar. Como esta é a primeira vez que usamos esse banco de dados, o assistente oferecerá para armazenar a cadeia de conexão em `Web.config`. Ele tem a armazenar a cadeia de caracteres de conexão usando o nome `NorthwindConnectionString`.


[![Conectar-se ao banco de dados Northwind](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Figura 02**: Conecte-se ao banco de dados Northwind ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))


O Assistente Configurar fonte de dados fornece dois meios pelos quais é possível especificar a consulta usada para recuperar dados:

- Especificando uma instrução SQL personalizada ou procedimento armazenado, ou
- Escolhendo uma tabela ou exibição e, em seguida, especificando as colunas para retornar

Como queremos retornar que apenas os cinco mais recentemente adicionado produtos, precisamos especificar uma instrução SQL personalizada. Use o seguinte `SELECT` consulta:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

O `TOP 5` palavra-chave retorna apenas os cinco primeiros registros da consulta. O `Products` chave primária da tabela `ProductID`, é um `IDENTITY` coluna, que garante a nós que cada produto novo adicionado à tabela terá um valor maior que a entrada anterior. Portanto, classificar os resultados por `ProductID` em ordem decrescente retorna os produtos começando com o último aqueles criados.


[![Retorna os cinco produtos adicionados mais recentemente](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Figura 03**: os cinco mais recentemente adicionado produtos de retorno ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))


Depois de concluir o assistente, o Visual Studio gera dois BoundFields do GridView exibir o `ProductName` e `UnitPrice` campos retornados do banco de dados. Neste ponto marcação declarativa da sua página mestre deve incluir marcação semelhante ao seguinte:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Como você pode ver, a marcação contém: o controle da Web de rótulo (`GridMessage`); o GridView `RecentProducts`, com dois BoundFields; e um controle SqlDataSource que retorna os cinco mais recentemente adicionado produtos.

Com isso GridView criado e seu controle SqlDataSource configurado, visite o site por meio de um navegador. Como mostra a Figura 4, você verá uma grade no canto inferior esquerdo que lista os cinco mais recentemente adicionado produtos.


[![O GridView exibe os cinco produtos adicionados mais recentemente](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Figura 04**: O GridView exibe os cinco mais recentemente adicionado produtos ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))


> [!NOTE]
> Fique à vontade limpar a aparência de GridView. Algumas sugestões incluem a formatação exibidos `UnitPrice` valor como uma moeda e usando fontes e cores de plano de fundo para melhorar a aparência da grade.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Etapa 2: Criando uma página de conteúdo para adicionar novos produtos

Nossa próxima tarefa é criar uma página de conteúdo na qual um usuário pode adicionar um novo produto para o `Products` tabela. Adicione uma nova página de conteúdo para o `Admin` pasta chamada `AddProduct.aspx`, deixando de associá-lo para o `Site.master` página mestra. Figura 5 mostra o Gerenciador de soluções depois que esta página foi adicionada ao site.


[![Adicionar uma nova página ASP.NET para a pasta Admin](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Figura 05**: Adicione uma nova página ASP.NET para o `Admin` pasta ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))


Lembre-se de que no [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial, criamos uma classe de página de base personalizada chamada `BasePage` que gerou o título da página, se ele tiver sido não foi explicitamente definido. Vá para o `AddProduct.aspx` de lógica da página de classe e fazer com que ele derivam `BasePage` (em vez do `System.Web.UI.Page`).

Por fim, atualize o `Web.sitemap` arquivo para incluir uma entrada para esta lição. Adicione a seguinte marcação abaixo o `<siteMapNode>` da lição de problemas de nomenclatura de ID de controle:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Conforme mostrado na Figura 6, a adição deste `<siteMapNode>` elemento é refletido na lista de lições.

Retorne ao `AddProduct.aspx`. No controle de conteúdo para o `MainContent` ContentPlaceHolder, adicione um controle DetailsView e denomine- `NewProduct`. Associar DetailsView para um novo controle SqlDataSource chamado `NewProductDataSource`. Como com o SqlDataSource na etapa 1, o Assistente para configurar para que ele usa o banco de dados Northwind e optar por especificar uma instrução SQL personalizada. Como DetailsView será usado para adicionar itens ao banco de dados, é preciso especificar uma `SELECT` instrução e um `INSERT` instrução. Use o seguinte `SELECT` consulta:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

Em seguida, na guia Inserir, adicione o seguinte `INSERT` instrução:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Depois de concluir o Assistente de ir para a marca inteligente de DetailsView e marque a caixa de seleção "Habilitar inserindo". Isso adiciona um CommandField DetailsView com seu `ShowInsertButton` propriedade definida como True. Porque este DetailsView será usado exclusivamente para inserção de dados, defina o DetailsView `DefaultMode` propriedade para `Insert`.

Isso é tudo para ele! Vamos testar essa página. Visite `AddProduct.aspx` por meio de um navegador, insira um nome e o preço (veja a Figura 6).


[![Adicionar um novo produto no banco de dados](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Figura 06**: adicionar um novo produto no banco de dados ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))


Depois de digitar o nome e o preço para o seu novo produto, clique no botão de inserção. Isso faz com que o formulário de postback. No postback, o controle SqlDataSource `INSERT` instrução é executada; seus dois parâmetros são preenchidos com os valores inseridos pelo usuário em dois controles de caixa de texto de DetailsView. Infelizmente, não há nenhum comentários visuais que ocorreu uma inserção. Seria bom ter uma mensagem exibida, confirmando que foi adicionado um novo registro. Posso deixar isso como um exercício para o leitor. Além disso, depois de adicionar um novo registro de DetailsView GridView na página mestra ainda mostra os mesmos cinco registros como antes; ele não inclui o registro adicionado apenas. Vamos examinar como corrigir isso nas etapas posteriores.

> [!NOTE]
> Além de adicionar alguma forma de comentários visuais que a inserção foi concluída com êxito, sugiro que atualizar também a interface de inserção de DetailsView para incluir a validação. Atualmente, não há nenhuma validação. Se um usuário inserir um valor inválido para o `UnitPrice` campo, como "muito caro," uma exceção será gerada em um postback quando o sistema tenta converter essa cadeia de caracteres em um decimal. Para obter mais informações sobre como personalizar a inserção de interface, consulte o [ *Personalizando a Interface de modificação de dados* tutorial](https://asp.net/learn/data-access/tutorial-20-vb.aspx) do meu [trabalhando com a série de tutoriais de dados](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Etapa 3: Criar propriedades e métodos públicos na página mestra

Na etapa 1, adicionamos um controle de rótulo Web chamado `GridMessage` acima GridView na página mestra. Esse rótulo destina-se a exibir, opcionalmente, uma mensagem. Por exemplo, depois de adicionar um novo registro para o `Products` tabela, queremos mostrar uma mensagem que diz: "*ProductName* foi adicionado ao banco de dados." Em vez de codificar o texto para esse rótulo na página mestra, esperamos que a mensagem seja personalizável com a página de conteúdo.

Como o controle de rótulo é implementado como uma variável de membro protegida dentro da página mestra não pode ser acessado diretamente das páginas de conteúdo. Para trabalhar com o rótulo em uma página mestre da página de conteúdo (ou, de fato, qualquer controle de Web na página mestra) é necessário criar uma propriedade pública na página mestra que expõe o controle da Web ou serve como um proxy pelo qual uma de suas propriedades pode ser  acessado. Adicione a seguinte sintaxe para a classe de code-behind da página mestra para expor o rótulo `Text` propriedade:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Quando um novo registro é adicionado para o `Products` tabela de uma página de conteúdo a `RecentProducts` GridView na página mestra precisa reassociar a fonte de dados subjacente. Para reassociar a chamada de GridView seu `DataBind` método. Como GridView na página mestra não é acessível por meio de programação para as páginas de conteúdo, é necessário criar um método público na página mestra que, quando chamado, associa novamente os dados para o GridView. Adicione o seguinte método à classe de code-behind da página mestra:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Com o `GridMessageText` propriedade e `RefreshRecentProductsGrid` método, qualquer página de conteúdo pode programaticamente definir ou ler o valor da `GridMessage` do rótulo `Text` propriedade ou associar novamente os dados para o `RecentProducts` GridView. Etapa 4 examina como acessar a página mestra propriedades e métodos públicos de uma página de conteúdo.

> [!NOTE]
> Não se esqueça de marcar a página mestra propriedades e métodos como `Public`. Se você não explicitamente denota essas propriedades e métodos como `Public`, eles não poderão ser acessados da página de conteúdo.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Etapa 4: Chamar de membros públicos da página mestre de uma página de conteúdo

Agora que a página mestra tem as propriedades públicas necessárias e os métodos, estamos prontos para chamar esses métodos e propriedades de `AddProduct.aspx` página de conteúdo. Especificamente, precisamos definir a página mestra `GridMessageText` propriedade e chame seu `RefreshRecentProductsGrid` método depois que o novo produto foi adicionado ao banco de dados. Todos os controles de Web de dados ASP.NET disparar eventos imediatamente antes e depois de concluir várias tarefas, o que torna mais fácil para os desenvolvedores de páginas executar alguma ação programática antes ou depois que a tarefa. Por exemplo, quando o usuário final clica o botão de inserção de DetailsView, no postback a DetailsView gera seu `ItemInserting` evento antes de iniciar o fluxo de trabalho de inserção. Em seguida, insere o registro no banco de dados. Depois disso, DetailsView aciona seu `ItemInserted` eventos. Portanto, para trabalhar com a página mestra depois que o novo produto foi adicionado, criar um manipulador de eventos para o DetailsView `ItemInserted` eventos.

Há duas maneiras que uma página de conteúdo por meio de programação pode interagir com sua página mestra:

- Usando o `Page.Master` propriedade, que retorna uma referência fracamente tipada para a página mestra, ou
- Especifique o caminho de arquivo ou o tipo de página mestra da página por meio de um `@MasterType` diretiva; isso adiciona automaticamente uma propriedade fortemente tipados para a página chamada `Master`.

Vamos examinar as duas abordagens.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Usando a tipagem`Page.Master`propriedade

Todas as páginas da web ASP.NET devem derivar de `Page` classe, que está localizado no `System.Web.UI` namespace. O `Page` classe inclui um [ `Master` propriedade](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) que retorna uma referência para a página mestra do título. Se a página não tem uma página mestra `Master` retorna `Nothing`.

O `Master` propriedade retorna um objeto do tipo [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (também localizado no `System.Web.UI` namespace) que é o tipo base da qual todas as páginas mestras derivam. Portanto, para uso de propriedades públicas ou métodos definidos na página mestra do nosso site, é necessário converter o `MasterPage` objeto retornado do `Master` propriedade para o tipo apropriado. Como nomeamos o nosso arquivo de página mestra `Site.master`, a classe code-behind foi nomeada `Site`. Portanto, o código a seguir converte o `Page.Master` propriedade para uma instância da `Site` classe.


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Agora que temos convertidos a tipagem `Page.Master` propriedade para o tipo de Site podemos referenciar as propriedades e métodos específicos do Site. Como mostra a Figura 7, a propriedade pública `GridMessageText` aparece no menu suspenso IntelliSense.


[![O IntelliSense mostra os métodos e propriedades públicas da nossa página mestra](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Figura 07**: o IntelliSense mostra os métodos e propriedades públicas da nossa página mestra ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))


> [!NOTE]
> Se você nomeou seu arquivo de página mestra `MasterPage.master` e em seguida, o nome da classe de lógica da página mestra é `MasterPage`. Isso pode levar a código ambíguo na conversão do tipo `System.Web.UI.MasterPage` ao seu `MasterPage` classe. Em resumo, você precisa qualificar totalmente o tipo que você está convertendo, que pode ser um pouco complicado, ao usar o modelo de projeto de Site. Minha sugestão é se certificar de que quando você cria uma página mestre você o nomear algo diferente de `MasterPage.master` ou, melhor ainda, criar uma referência fortemente tipada para a página mestra.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Criar uma referência fortemente tipada com o`@MasterType`diretiva

Se você examinar de perto você pode ver que a classe code-behind de uma página ASP.NET é uma classe parcial (Observe o `Partial` palavra-chave na definição de classe). Classes parciais foram introduzidas no c# e Visual Basic com o.NET Framework 2.0 e, em resumo, permitem para membros de uma classe a ser definido em vários arquivos. O arquivo de classe code-behind - `AddProduct.aspx.vb`, por exemplo - contém o código que nós, o desenvolvedor da página criar. Além do nosso código, o mecanismo do ASP.NET cria automaticamente um arquivo de classe separada com propriedades e manipuladores de eventos em que signifique a marcação declarativa a hierarquia de classe da página.

A geração de código automática ocorre sempre que uma página ASP.NET é visitada abre caminho para algumas possibilidades bastante interessantes e úteis. No caso de páginas mestras, se dizemos que o mecanismo do ASP.NET que página mestre está sendo usada por nossa página de conteúdo, ele gera um tipo mais acentuado `Master` propriedade para nós.

Use o [ `@MasterType` diretiva](https://msdn.microsoft.com/library/ms228274.aspx) para informar o mecanismo do ASP.NET do tipo de página mestra da página de conteúdo. O `@MasterType` diretiva pode aceitar o nome do tipo da página mestra ou seu caminho de arquivo. Para especificar que o `AddProduct.aspx` página usos `Site.master` como sua página mestra, adicione a seguinte diretiva na parte superior do `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Essa diretiva instrui o mecanismo do ASP.NET para adicionar uma referência fortemente tipada para a página mestra por meio de uma propriedade chamada `Master`. Com o `@MasterType` diretiva em vigor, podemos chamar o `Site.master` mestre da página de propriedades públicas e métodos diretamente por meio de `Master` propriedade sem todas as conversões.

> [!NOTE]
> Se você omitir a `@MasterType` diretiva, a sintaxe `Page.Master` e `Master` retornar a mesma coisa: um objeto tipagem para a página mestra do título. Se você incluir a `@MasterType` , em seguida, a diretiva `Master` retorna uma referência fortemente tipada para a página mestra especificada. `Page.Master`, no entanto, ainda retornará uma referência fracamente tipada. Para obter uma visão mais completa em por que esse é o caso e como a `Master` propriedade é construída quando o `@MasterType` diretiva for incluída, consulte [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)da entrada de blog [ `@MasterType` no ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Atualizando a página mestra depois de adicionar um novo produto

Agora que sabemos como chamar propriedades públicas e métodos de uma página de conteúdo de uma página mestra, estamos prontos para atualizar o `AddProduct.aspx` de página para que a página mestra for atualizada depois de adicionar um novo produto. No início da etapa 4, criamos um manipulador de eventos do controle DetailsView `ItemInserting` evento, que é executado imediatamente após o novo produto foi adicionado ao banco de dados. Adicione o seguinte código ao manipulador de eventos:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

O código acima usa os dois a tipagem `Page.Master` propriedade e fortemente tipado `Master` propriedade. Observe que o `GridMessageText` estiver definida como "*ProductName* adicionada à grade..." Valores do produto adicionado apenas são acessíveis por meio de `e.Values` coleta; como você pode ver, o just-adicionado `ProductName` valor é acessado por meio de `e.Values("ProductName")`.

A Figura 8 mostra o `AddProduct.aspx` imediatamente após um novo produto - de Scott Soda - foi adicionada ao banco de dados. Observe que o nome do produto just-adicionado é observado no rótulo da página mestra e que o GridView tiver sido atualizado para incluir o produto e seu preço.


[![Rótulo da página mestra e o GridView mostram o produto Just-adicionado](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Figura 08**: A página mestra rótulo e GridView mostram o produto Just-Added ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))


## <a name="summary"></a>Resumo

O ideal é que uma página mestra e suas páginas de conteúdo são completamente separadas umas das outras e nenhum nível de interação. Enquanto as páginas mestras e páginas de conteúdo devem ser criadas com esse objetivo em mente, há um número de cenários comuns em que uma página de conteúdo deve interagir com sua página mestra. Um dos motivos mais comuns gira em torno de atualização de uma parte específica da exibição de página mestra com base em alguma ação que ocorreu na página de conteúdo.

A boa notícia é que é relativamente fácil para ter uma página de conteúdo interagir programaticamente com sua página mestra. Comece criando métodos ou propriedades públicas na página mestra que encapsulam a funcionalidade de que precisa ser invocado por uma página de conteúdo. Em seguida, na página de conteúdo, acessar a página mestra propriedades e métodos por meio de tipagem `Page.Master` propriedade ou use o `@MasterType` diretiva para criar uma referência fortemente tipada para a página mestra.

No próximo tutorial, vamos examinar como fazer com que a página mestra interagir programaticamente com uma das suas páginas de conteúdo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Acessar e atualizar dados no ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Páginas mestras do ASP.NET: Dicas, truques e armadilhas](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` no ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Passando informações entre o conteúdo e páginas mestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabalhando com dados nos tutoriais do ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 3.5 in 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Zack Jones. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, escreva-me em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](control-id-naming-in-content-pages-vb.md)
> [Próximo](interacting-with-the-content-page-from-the-master-page-vb.md)
