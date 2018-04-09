---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
title: Interagindo com a página mestra da página de conteúdo (VB) | Microsoft Docs
author: rick-anderson
description: Examina como chamar métodos, definir propriedades, etc. da página mestra do código na página de conteúdo.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: 081fe010-ba0f-4e7d-b4ba-774840b601c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-master-page-from-the-content-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 7e92e7abbc69a0998d7563db0d5b206d7ba3d6be
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="interacting-with-the-master-page-from-the-content-page-vb"></a>Interagindo com a página mestra da página de conteúdo (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_06_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_06_VB.pdf)

> Examina como chamar métodos, definir propriedades, etc. da página mestra do código na página de conteúdo.


## <a name="introduction"></a>Introdução

Ao longo dos últimos cinco tutoriais que vimos como criar uma página mestre, definir áreas de conteúdo, associar páginas ASP.NET para uma página mestra e definir o conteúdo específico da página. Quando um visitante solicita uma página de conteúdo particular, o conteúdo e a marcação de páginas mestras são combinadas em tempo de execução, resultando no processamento de uma hierarquia de controle unificada. Portanto, já vimos uma forma em que a página mestra e uma das suas páginas de conteúdo podem interagir: a página de conteúdo explica a marcação para transfuse em controles ContentPlaceHolder da página mestra.

O que ainda precisamos examinar é como a página mestra e a página de conteúdo podem interagir programaticamente. Além de definir a marcação para controles ContentPlaceHolder da página mestra, uma página de conteúdo também pode atribuir valores às propriedades públicas da sua página mestra e chamar seus métodos públicos. Da mesma forma, uma página mestra pode interagir com suas páginas de conteúdo. Enquanto a interação entre uma página mestra e o conteúdo de modo programático é menos comum do que a interação entre suas marcações declarativas, há muitos cenários em que essa interação de modo programático é necessária.

Neste tutorial, podemos examinar como uma página de conteúdo pode interagir programaticamente com sua página mestra; o seguinte tutorial, examinaremos como a página mestra da mesma forma pode interagir com suas páginas de conteúdo.

## <a name="examples-of-programmatic-interaction-between-a-content-page-and-its-master-page"></a>Exemplos de programação interação entre uma página de conteúdo e sua página mestra

Quando uma região específica de uma página precisa ser configurado em uma base de página por página, usamos um controle ContentPlaceHolder. Mas e quanto situações em que a maioria das páginas necessário para emitir um determinado tipo de saída, mas um pequeno número de páginas necessário personalizá-lo para mostrar algo mais? Um exemplo, que são examinados no [ *ContentPlaceHolders vários e conteúdo padrão* ](multiple-contentplaceholders-and-default-content-vb.md) envolve o tutorial, exibindo uma interface de logon em cada página. Embora a maioria das páginas deve incluir uma interface de logon, ele deve ser suprimido para uma série de páginas, como: a página de logon principal (`Login.aspx`); a página de criação de conta; e outras páginas que são acessíveis apenas a usuários autenticados. O [ *ContentPlaceHolders vários e conteúdo padrão* ](multiple-contentplaceholders-and-default-content-vb.md) tutorial mostrou como definir o conteúdo padrão para um ContentPlaceHolder na página mestra e, em seguida, como substituí-la nas páginas onde o conteúdo padrão não foi desejado.

Outra opção é criar uma propriedade pública ou método dentro da página mestra que indica se deve mostrar ou ocultar a interface de logon. Por exemplo, a página mestra pode incluir uma propriedade pública denominada `ShowLoginUI` cujo valor foi usado para definir o `Visible` propriedade do controle de logon na página mestra. Essas páginas de conteúdo em que a interface de usuário do logon deve ser suprimida depois podem definir programaticamente o `ShowLoginUI` propriedade `False`.

Talvez o exemplo mais comum de conteúdo e interação de página mestra ocorre quando os dados exibidos na página mestra precisa ser atualizada depois que alguma ação ocorridos na página de conteúdo. Considere uma página mestra que inclui um GridView que exibe os cinco mais recentemente adicionada registros de uma tabela de banco de dados específico e que uma das suas páginas de conteúdo inclui uma interface para adição de novos registros para a mesma tabela.

Quando um usuário acessa a página para adicionar um novo registro, ela vê que cinco adicionada mais recentemente registros exibidos na página mestra. Depois de preencher os valores para colunas do novo registro, ela envia o formulário. Supondo que o GridView na página mestra tem seu `EnableViewState` propriedade definida como True (padrão), seu conteúdo é recarregado do estado de exibição e, consequentemente, os mesmos cinco registros são exibidos, mesmo que um novo registro acabou de ser adicionado ao banco de dados. Isso pode confundir o usuário.

> [!NOTE]
> Mesmo se você desabilitar o estado de exibição do GridView para que ele se reconecta à sua fonte de dados subjacente em cada postagem, ele ainda não mostrar o registro adicionados apenas porque os dados serão associados à GridView anteriormente no ciclo de vida de página que quando o novo registro é adicionado para o datab ASE.


Para corrigir isso para que o registro just-adicionado é exibido na página mestra do GridView, precisamos instruir o GridView associar novamente a fonte de dados de postagem *depois* o novo registro foi adicionado ao banco de dados. Isso requer a interação entre o conteúdo e páginas mestras porque é a interface para adicionar que o novo registro (e seus manipuladores de eventos) estão em GridView que precisa ser atualizado, mas a página de conteúdo na página mestra.

Como atualizar a exibição da página mestra de um manipulador de eventos na página de conteúdo é uma das necessidades mais comuns de conteúdo e interação de página mestra, vamos explorar esse tópico em mais detalhes. O download para este tutorial inclui um banco de dados do Microsoft SQL Server 2005 Express Edition denominado `NORTHWIND.MDF` do site `App_Data` pasta. O banco de dados Northwind armazena informações de vendas de uma empresa fictícia, a Northwind Traders, funcionário e produto.

Etapa 1 aborda exibindo cinco mais recentemente adicionado produtos em um GridView na página mestra. Etapa 2 cria uma página de conteúdo para a adição de novos produtos. Etapa 3 explica como criar propriedades e métodos públicos na página mestra e etapa 4 ilustra como a interface programaticamente com essas propriedades e métodos da página de conteúdo.

> [!NOTE]
> Este tutorial não Aprofunde-se as especificações de trabalhar com dados no ASP.NET. As etapas para configurar a página mestra para exibir dados e a página de conteúdo para a inserção de dados estiverem concluídas, ainda breezy. Para uma visão mais detalhada de exibição e inserção de dados e usando os controles de SqlDataSource e GridView, consulte os recursos na seção Leituras adicionais no final deste tutorial.


## <a name="step-1-displaying-the-five-most-recently-added-products-in-the-master-page"></a>Etapa 1: Exibir os cinco mais recentemente adicionado produtos na página mestra

Abra a página mestra Site.master e adicione um rótulo e um controle GridView para o `leftContent` `<div>`. Desmarque fora do rótulo `Text` propriedade, definida seu `EnableViewState` propriedade para `False`e sua `ID` propriedade `GridMessage`; definir o GridView `ID` propriedade para `RecentProducts`. Em seguida, no Designer, expanda a marca inteligente do GridView e escolha para associá-lo para uma nova fonte de dados. Isso inicia o Assistente de configuração de fonte de dados. Porque o banco de dados Northwind no `App_Data` pasta é um banco de dados do Microsoft SQL Server, optar por criar um SqlDataSource selecionando (consulte a Figura 1); nome SqlDataSource `RecentProductsDataSource`.


[![Associar um controle SqlDataSource chamado RecentProductsDataSource GridView](interacting-with-the-master-page-from-the-content-page-vb/_static/image2.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image1.png)

**Figura 01**: associar o GridView para um controle SqlDataSource chamado `RecentProductsDataSource` ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image3.png))


A próxima etapa pergunta para especificar o banco de dados para se conectar ao. Escolha o `NORTHWIND.MDF` banco de dados do arquivo na lista suspensa e clique em Avançar. Como esta é a primeira vez que usamos este banco de dados, o assistente oferecerá para armazenar a cadeia de caracteres de conexão em `Web.config`. Que permite armazenar a cadeia de caracteres de conexão usando o nome `NorthwindConnectionString`.


[![Conecte-se ao banco de dados Northwind](interacting-with-the-master-page-from-the-content-page-vb/_static/image5.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image4.png)

**Figura 02**: conectar-se ao banco de dados Northwind ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image6.png))


O Assistente Configurar fonte de dados fornece dois meios pelos quais podemos especificar a consulta usada para recuperar dados:

- Especificando uma instrução SQL personalizada ou procedimento armazenado, ou
- A seleção de uma tabela ou exibição e, em seguida, especificando as colunas para retornar

Como queremos retornar que apenas os cinco mais recentemente adicionado produtos, é preciso especificar uma instrução SQL personalizada. Use a seguinte `SELECT` consulta:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample1.sql)]

O `TOP 5` palavra-chave retorna apenas os cinco primeiros registros da consulta. O `Products` chave primária da tabela `ProductID`, é um `IDENTITY` coluna, que garante a nós que cada novo produto adicionado à tabela terá um valor maior que a entrada anterior. Portanto, classificar os resultados por `ProductID` em ordem decrescente retorna os produtos começando com a última aqueles criados.


[![Retorna os cinco produtos adicionados mais recentemente](interacting-with-the-master-page-from-the-content-page-vb/_static/image8.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image7.png)

**Figura 03**: retornar os cinco mais recentemente adicionado produtos ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image9.png))


Depois de concluir o assistente, o Visual Studio gera dois BoundFields do GridView exibir o `ProductName` e `UnitPrice` campos retornados do banco de dados. Neste ponto declarativo da sua página mestre deve incluir marcação semelhante ao seguinte:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample2.aspx)]

Como você pode ver, a marcação contém: o controle de rótulo Web (`GridMessage`); o GridView `RecentProducts`, com dois BoundFields; e um controle SqlDataSource que retorna os cinco mais recentemente adicionado produtos.

Com isso GridView criado e seu controle SqlDataSource configurado, visite o site por meio de um navegador. Como mostra a Figura 4, você verá uma grade no canto inferior esquerdo que lista os cinco mais recentemente adicionado produtos.


[![O GridView exibe os cinco produtos adicionados mais recentemente](interacting-with-the-master-page-from-the-content-page-vb/_static/image11.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image10.png)

**Figura 04**: GridView exibe os cinco mais recentemente adicionado produtos ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image12.png))


> [!NOTE]
> Fique à vontade para limpar a aparência de GridView. Algumas sugestões incluem formatação exibido `UnitPrice` valor como uma moeda e o uso de fontes e cores de plano de fundo para melhorar a aparência da grade.


## <a name="step-2-creating-a-content-page-to-add-new-products"></a>Etapa 2: Criando uma página de conteúdo para adicionar novos produtos

Nossa próxima tarefa é criar uma página de conteúdo na qual um usuário pode adicionar um novo produto para o `Products` tabela. Adicione uma nova página de conteúdo para o `Admin` pasta denominada `AddProduct.aspx`, tornando-se para associá-lo para o `Site.master` página mestra. A Figura 5 mostra o Gerenciador de soluções depois que esta página foi adicionada para o site.


[![Adicionar uma nova página ASP.NET para a pasta Admin](interacting-with-the-master-page-from-the-content-page-vb/_static/image14.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image13.png)

**Figura 05**: adicionar uma nova página ASP.NET para o `Admin` pasta ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image15.png))


Lembre-se de que o [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial criamos uma classe de página de base personalizada chamada `BasePage` que gerou o título da página, se ele foi não foi explicitamente definido. Vá para o `AddProduct.aspx` por trás do código da página de classe e a derivam `BasePage` (em vez de `System.Web.UI.Page`).

Por fim, atualize o `Web.sitemap` arquivo para incluir uma entrada para esta lição. Adicione a seguinte marcação sob o `<siteMapNode>` da lição problemas de nomenclatura de ID de controle:


[!code-xml[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample3.xml)]

Conforme mostrado na Figura 6, a adição deste `<siteMapNode>` elemento será refletido na lista de lições.

Retorne ao `AddProduct.aspx`. No controle de conteúdo para o `MainContent` ContentPlaceHolder, adicione um controle DetailsView e denomine- `NewProduct`. Associar o DetailsView para um novo controle SqlDataSource chamado `NewProductDataSource`. Como com o SqlDataSource na etapa 1, configurar o Assistente para que ele usa o banco de dados Northwind e optar por especificar uma instrução SQL personalizada. Como o DetailsView será usado para adicionar itens ao banco de dados, é preciso especificar ambos um `SELECT` instrução e um `INSERT` instrução. Use a seguinte `SELECT` consulta:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample4.sql)]

Em seguida, na guia Inserir, adicione o seguinte `INSERT` instrução:


[!code-sql[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample5.sql)]

Após concluir o assistente, vá para marcas inteligentes de DetailsView e marque a caixa de seleção "Habilitar inserindo". Isso adiciona um CommandField a DetailsView com seu `ShowInsertButton` propriedade definida como True. Como esse DetailsView será usado apenas para a inserção de dados, defina a DetailsView `DefaultMode` propriedade `Insert`.

Isso é tudo que é necessário para que ele! Vamos testar esta página. Visite `AddProduct.aspx` através de um navegador, digite um nome e o preço (veja a Figura 6).


[![Adicionar um novo produto no banco de dados](interacting-with-the-master-page-from-the-content-page-vb/_static/image17.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image16.png)

**Figura 06**: adicionar um novo produto no banco de dados ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image18.png))


Depois de digitar o nome e o preço para o novo produto, clique no botão de inserção. Isso faz com que o formulário de postback. Em um postback, o controle SqlDataSource `INSERT` instrução é executada; seus dois parâmetros são preenchidos com os valores inseridos pelo usuário em dois controles de caixa de texto de DetailsView. Infelizmente, não há nenhum comentários visuais que ocorreu uma inserção. Seria ótimo ter uma mensagem exibida, confirmando que foi adicionado um novo registro. Posso deixar isso como um exercício para o leitor. Além disso, depois de adicionar um novo registro de DetailsView GridView na página mestra ainda mostra os mesmos cinco registros como antes; ele não inclui o registro adicionado imediatamente. Vamos examinar como corrigir isso nas etapas futuras.

> [!NOTE]
> Além de adicionar alguma forma de comentários visuais que a inserção foi bem-sucedida, eu recomendo que também atualizar a interface de inserção de DetailsView para incluir validação. Atualmente, não há nenhuma validação. Se um usuário inserir um valor inválido para o `UnitPrice` campo, como "muito caro," uma exceção será lançada em um postback quando o sistema tenta converter a cadeia de caracteres em um decimal. Para obter mais informações sobre como personalizar a inserção de interface, consulte o [ *Personalizando a Interface de modificação de dados* tutorial](https://asp.net/learn/data-access/tutorial-20-vb.aspx) do meu [trabalhando com série de tutoriais de dados](../../data-access/index.md).


## <a name="step-3-creating-public-properties-and-methods-in-the-master-page"></a>Etapa 3: Criar propriedades e métodos públicos na página mestra

Na etapa 1, adicionamos um controle de rótulo Web chamado `GridMessage` acima GridView na página mestra. Este rótulo destina-se a exibir, opcionalmente, uma mensagem. Por exemplo, depois de adicionar um novo registro para o `Products` tabela, que talvez desejemos mostrar a mensagem: "*ProductName* foi adicionado ao banco de dados." Em vez de embutir em código o texto para este rótulo na página mestre, que talvez desejemos a mensagem seja personalizável, a página de conteúdo.

Como o controle de rótulo é implementado como uma variável de membro protegido dentro da página mestra não pode ser acessado diretamente das páginas de conteúdo. Para trabalhar com o rótulo em uma página mestre da página de conteúdo (ou, na verdade, qualquer controle de Web na página mestra), precisamos criar uma propriedade pública na página mestra que expõe o controle da Web ou serve como um proxy pelo qual uma de suas propriedades pode ser  acessado. Adicione a seguinte sintaxe para a classe de code-behind da página mestra para expor o rótulo `Text` propriedade:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample6.vb)]

Quando um novo registro é adicionado para o `Products` tabela de uma página de conteúdo a `RecentProducts` GridView na página mestra precisa reassociar a fonte de dados subjacente. Para associar novamente a chamada de GridView seu `DataBind` método. Como GridView na página mestre não está acessível por meio de programação para as páginas de conteúdo, é necessário criar um método público na página mestra que, quando chamado, reconecta os dados do GridView. Adicione o seguinte método à classe de code-behind da página mestra:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample7.vb)]

Com o `GridMessageText` propriedade e `RefreshRecentProductsGrid` método, qualquer página de conteúdo pode programaticamente definir ou ler o valor da `GridMessage` do rótulo `Text` propriedade ou associar novamente os dados para o `RecentProducts` GridView. Etapa 4 examina como acessar a página mestra propriedades e métodos públicos de uma página de conteúdo.

> [!NOTE]
> Não se esqueça de marcar a página mestra propriedades e métodos como `Public`. Se você não explicitamente denotam essas propriedades e métodos como `Public`, eles não estarão acessíveis a partir da página de conteúdo.


## <a name="step-4-calling-the-master-pages-public-members-from-a-content-page"></a>Etapa 4: Chamando membros públicos da página mestra de uma página de conteúdo

Agora que a página mestra tem as propriedades públicas necessárias e os métodos, estamos prontos para chamar esses métodos e propriedades de `AddProduct.aspx` página de conteúdo. Especificamente, é preciso definir a página mestra `GridMessageText` propriedade e chame seu `RefreshRecentProductsGrid` método depois que o novo produto foi adicionado ao banco de dados. Todos os controles de Web de dados ASP.NET disparar eventos imediatamente antes e depois de concluir as diversas tarefas, que tornam mais fácil para os desenvolvedores de página executar alguma ação programática antes ou depois da tarefa. Por exemplo, quando o usuário final clicar botão de inserção de DetailsView, no postback gera o DetailsView seu `ItemInserting` evento antes de iniciar o fluxo de trabalho de inserção. Em seguida, ele insere o registro no banco de dados. Depois disso, DetailsView gera seu `ItemInserted` eventos. Portanto, para trabalhar com a página mestra depois de adicionar o novo produto, criar um manipulador de eventos para o DetailsView `ItemInserted` eventos.

Há duas maneiras que uma página de conteúdo pode interagir programaticamente com sua página mestra:

- Usando o `Page.Master` propriedade, que retorna uma referência tipadas vagamente à página mestre, ou
- Especifique o caminho de arquivo ou tipo de página mestra da página por meio de um `@MasterType` diretiva; isso adiciona automaticamente uma propriedade fortemente tipado para a página chamada `Master`.

Vamos examinar as duas abordagens.

### <a name="using-the-loosely-typedpagemasterproperty"></a>Usando o tipadas vagamente`Page.Master`propriedade

Todas as páginas da web ASP.NET deve derivar do `Page` classe, que está localizado no `System.Web.UI` namespace. O `Page` classe inclui um [ `Master` propriedade](https://msdn.microsoft.com/library/system.web.ui.page.master.aspx) que retorna uma referência à página mestre da página. Se a página não tem uma página mestra `Master` retorna `Nothing`.

O `Master` propriedade retorna um objeto do tipo [ `MasterPage` ](https://msdn.microsoft.com/library/system.web.ui.masterpage.aspx) (também localizado no `System.Web.UI` namespace) que é o tipo base da qual todas as páginas mestras derivam. Portanto, para usar as propriedades públicas nem os métodos definidos na página mestre do nosso site, é necessário converter o `MasterPage` objeto retornado do `Master` propriedade para o tipo apropriado. Como podemos nomeado nosso arquivo de página mestra `Site.master`, a classe code-behind foi nomeada `Site`. Portanto, o código a seguir conversões de `Page.Master` propriedade a uma instância do `Site` classe.


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample8.vb)]

Agora que temos convertida a tipadas vagamente `Page.Master` propriedade para o tipo de Site pode referenciar as propriedades e métodos específicos do Site. Como mostra a Figura 7, a propriedade pública `GridMessageText` aparece na lista suspensa IntelliSense.


[![IntelliSense mostra os métodos e propriedades públicas da nossa página mestra](interacting-with-the-master-page-from-the-content-page-vb/_static/image20.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image19.png)

**Figura 07**: IntelliSense mostra os métodos e propriedades públicas da nossa página mestra ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image21.png))


> [!NOTE]
> Se você nomeou seu arquivo de página mestra `MasterPage.master` , em seguida, o nome da classe por trás do código da página mestra é `MasterPage`. Isso pode resultar em código ambíguo na conversão do tipo `System.Web.UI.MasterPage` para sua `MasterPage` classe. Em resumo, você precisa qualificar totalmente o tipo de que conversão de, que pode ser um pouco confuso ao usar o modelo de projeto de Site. Minha sugestão é para se certificar de que quando você cria uma página mestre você o nomear algo diferente de `MasterPage.master` ou, melhor ainda, crie uma referência fortemente tipada para a página mestra.


### <a name="creating-a-strongly-typed-reference-with-themastertypedirective"></a>Criar uma referência fortemente tipada com a`@MasterType`diretiva

Se você examinar detalhadamente você pode ver que a classe de code-behind da página do ASP.NET é uma classe parcial (Observe o `Partial` palavra-chave na definição de classe). Classes parciais foram introduzidas em c# e Visual Basic com o.NET Framework 2.0 e, em resumo, permitem a membros da classe a ser definido em vários arquivos. O arquivo de classe code-behind - `AddProduct.aspx.vb`, por exemplo - contém o código, o desenvolvedor de página, criamos. Além de nosso código, o mecanismo do ASP.NET cria automaticamente um arquivo de classe separada com propriedades e manipuladores de eventos que traduzem a marcação declarativa para a hierarquia de classe da página.

A geração de código automática ocorre sempre que uma página for visitada abre caminho para algumas possibilidades bastante úteis e interessantes. No caso de páginas mestras, se falarmos sobre o mecanismo do ASP.NET que página mestre está sendo usada por nossa página de conteúdo, ele gera fortemente tipado `Master` propriedade para nós.

Use o [ `@MasterType` diretiva](https://msdn.microsoft.com/library/ms228274.aspx) para informar o mecanismo do ASP.NET do tipo de página mestra da página de conteúdo. O `@MasterType` diretiva pode aceitar o nome do tipo da página mestra ou seu caminho de arquivo. Para especificar que o `AddProduct.aspx` página usa `Site.master` como sua página mestra, adicione a seguinte diretiva na parte superior da `AddProduct.aspx`:


[!code-aspx[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample9.aspx)]

Essa diretiva instrui o mecanismo do ASP.NET para adicionar uma referência fortemente tipada para a página mestra por meio de uma propriedade chamada `Master`. Com o `@MasterType` diretiva em vigor, podemos chamar o `Site.master` mestre da página de propriedades e métodos públicos diretamente por meio de `Master` propriedade sem qualquer conversão.

> [!NOTE]
> Se você omitir o `@MasterType` diretiva, a sintaxe `Page.Master` e `Master` retornar a mesma coisa: um objeto tipadas vagamente a página mestra da página. Se você incluir o `@MasterType` , em seguida, a diretiva `Master` retorna uma referência fortemente tipada para a página mestra especificada. `Page.Master`, no entanto, ainda retorna uma referência tipadas vagamente. Para uma visão mais completa de por que esse é o caso e como o `Master` propriedade é construída quando o `@MasterType` diretiva for incluída, consulte [K. Scott Allen](http://odetocode.com/blogs/scott/default.aspx)da entrada de blog [ `@MasterType` no ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx).


### <a name="updating-the-master-page-after-adding-a-new-product"></a>Atualizando a página mestra depois de adicionar um novo produto

Agora que sabemos como invocar uma página mestra propriedades e métodos públicos de uma página de conteúdo, estamos prontos para atualizar o `AddProduct.aspx` página para que a página mestra for atualizada depois de adicionar um novo produto. No início da etapa 4, criamos um manipulador de eventos para o controle de DetailsView `ItemInserting` evento, que é executado imediatamente depois que o novo produto foi adicionado ao banco de dados. Adicione o seguinte código ao manipulador de eventos:


[!code-vb[Main](interacting-with-the-master-page-from-the-content-page-vb/samples/sample10.vb)]

O código acima usa ambos o tipadas vagamente `Page.Master` propriedade e o tipo mais acentuado `Master` propriedade. Observe que o `GridMessageText` está definida como "*ProductName* adicionada à grade..." Valores do produto adicionados apenas são acessíveis por meio de `e.Values` coleção; como você pode ver, a apenas adicionados `ProductName` valor é acessada por meio de `e.Values("ProductName")`.

A Figura 8 mostra o `AddProduct.aspx` imediatamente após um novo produto - Scott Soda - foi adicionada ao banco de dados. Observe que o nome do produto adicionados apenas está disponível no rótulo da página mestra e que o GridView tiver sido atualizada para incluir o produto e seu preço.


[![Rótulo da página mestra e o GridView mostram o produto adicionados apenas](interacting-with-the-master-page-from-the-content-page-vb/_static/image23.png)](interacting-with-the-master-page-from-the-content-page-vb/_static/image22.png)

**Figura 08**: A página mestra rótulo e GridView mostram o produto Just-Added ([clique para exibir a imagem em tamanho normal](interacting-with-the-master-page-from-the-content-page-vb/_static/image24.png))


## <a name="summary"></a>Resumo

Idealmente, uma página mestra e suas páginas de conteúdo são completamente separadas uns dos outros e nenhum nível de interação. Enquanto as páginas mestras e páginas de conteúdo devem ser criadas com esse objetivo em mente, há inúmeros cenários comuns em que uma página de conteúdo deve interagir com sua página mestra. Um dos motivos mais comuns gira em torno de uma parte específica da exibição de página mestra com base em alguma ação que ocorreu na página de conteúdo de atualização.

A boa notícia é que é relativamente simples para que uma página de conteúdo interagir programaticamente com sua página mestra. Comece criando propriedades públicas nem os métodos na página mestra que encapsula a funcionalidade que precisa ser chamado por uma página de conteúdo. Em seguida, na página de conteúdo, acessar a página mestra propriedades e métodos por meio de tipadas vagamente `Page.Master` propriedade ou use o `@MasterType` diretiva para criar uma referência fortemente tipada para a página mestra.

O tutorial Avançar examinamos como fazer com que a página mestra interagir programaticamente com uma das suas páginas de conteúdo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Acessar e atualizar dados no ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Páginas mestras do ASP.NET: Dicas, truques e interceptações](http://www.odetocode.com/articles/450.aspx)
- [`@MasterType` no ASP.NET 2.0](http://odetocode.com/Blogs/scott/archive/2005/07/16/1944.aspx)
- [Passando informações entre o conteúdo e páginas mestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabalhando com dados nos tutoriais do ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 3.5 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Zack Jones. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](control-id-naming-in-content-pages-vb.md)
> [Próximo](interacting-with-the-content-page-from-the-master-page-vb.md)
