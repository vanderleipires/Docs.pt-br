---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
title: "Páginas mestras (c#) aninhadas | Microsoft Docs"
author: rick-anderson
description: "Mostra como aninhar uma página mestra dentro de outra."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/28/2008
ms.topic: article
ms.assetid: 32b7fb6e-d74b-4048-91f8-70631b2523ee
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 97513a5a6ac7a958a03626f16a328ecb0b85c03f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="nested-master-pages-c"></a>Páginas mestras aninhadas (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.pdf)

> Mostra como aninhar uma página mestra dentro de outra.


## <a name="introduction"></a>Introdução

Ao longo dos últimos nove tutoriais, vimos como implementar um layout de todo o site com páginas mestras. Em resumo, páginas mestras processada, o desenvolvedor de página para definir marcação comuns na página mestra junto com a regiões específicas que podem ser personalizados em cada página de conteúdo por página de conteúdo. Os controles ContentPlaceHolder em uma página mestre indicam as regiões personalizáveis; a marcação personalizada para controles ContentPlaceHolder são definidas na página de conteúdo por meio de controles de conteúdo.

As técnicas de página mestra que já tenha sido explorados até o momento são excelentes se você tiver um único layout usado em todo o site. No entanto, muitos sites grandes têm um layout de site personalizada em várias seções. Por exemplo, considere um aplicativo de assistência médica usado pela equipe do hospital para gerenciar informações de pacientes, atividades e cobrança. Pode haver três tipos de páginas da web neste aplicativo:

- Páginas de membro específicas de equipe onde os membros da equipe podem atualizar a disponibilidade, Exibir agendas ou solicite férias.
- Páginas de paciente específicas em que os membros da equipe exibir ou editar informações do paciente específico.
- Páginas de cobrança específico em que os contadores examinar atual de declaração de status e relatórios financeiros.

Todas as páginas podem compartilhar um layout comuns, como um menu na parte superior e uma série de links usados com frequência na parte inferior. Mas a equipe, pacientes e páginas específicas de cobrança talvez seja necessário personalizar este layout genérico. Por exemplo, talvez todas as páginas de equipe específico devem incluir uma lista de calendário e tarefas mostrando a disponibilidade e a agenda diária do usuário conectado no momento. Talvez precisam de todas as páginas específicas do paciente mostrar o nome, endereço e informações de seguro de paciente cujas informações está sendo editadas.

É possível criar esses layouts personalizados usando *páginas mestras aninhadas*. Para implementar o cenário acima, podemos comece criando uma página mestra que definiu o conteúdo de layout, o menu e o rodapé todo o site com ContentPlaceHolders definindo as regiões personalizáveis. Criaremos três páginas mestras aninhadas, uma para cada tipo de página da web. Cada página mestre aninhada deve definir o conteúdo entre o tipo de conteúdo páginas que usam a página mestra. Em outras palavras, a página mestre aninhada para páginas de conteúdo específico de paciente incluiria marcação e lógica de programação para exibir informações sobre o paciente que está sendo editado. Ao criar uma nova página específica do paciente seria a ligamos para essa página mestre aninhada.

Este tutorial é iniciado, realçando os benefícios de páginas mestras aninhadas. Em seguida, ele mostra como criar e usar páginas mestras aninhadas.

> [!NOTE]
> Páginas mestras aninhadas tem sido possível desde a versão 2.0 do .NET Framework. No entanto, o Visual Studio 2005 não incluíam suporte de tempo de design para páginas mestras aninhadas. A boa notícia é que o Visual Studio 2008 oferece uma experiência avançada de tempo de design para páginas mestras aninhadas. Se você estiver interessado em usar páginas mestras aninhadas, mas ainda estiver usando o Visual Studio 2005, check-out [Scott Guthrie](https://weblogs.asp.net/scottgu/)da entrada de blog, [dicas para páginas mestras aninhadas em tempo de Design do VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>Os benefícios de páginas mestras aninhadas

Muitos sites têm um design abrangente do site, bem como mais designs personalizados específicos para certos tipos de páginas. Por exemplo, em nosso aplicativo da web de demonstração criamos uma seção Administração rudimentar (as páginas de `~/Admin` pasta). Atualmente, as páginas da web no `~/Admin` pasta usar a mesma página mestra como essas páginas na seção de administração (isto é, `Site.master` ou `Alternate.master`, dependendo da seleção do usuário).

> [!NOTE]
> Por enquanto, suponhamos que nosso site tenha apenas uma página mestre, `Site.master`. Vamos abordá usar páginas mestras aninhadas com dois (ou mais) páginas mestras, começando com "Usando um aninhada mestre para a administração seção da página", mais adiante neste tutorial.


Imagine que foi solicitado para personalizar o layout das páginas de administração para incluir informações adicionais ou links que não seriam presente em outras páginas no site. Há quatro técnicas para implementar este requisito:

1. Adicionar manualmente as informações específicas de administração e links para cada página de conteúdo no `~/Admin` pasta.
2. Atualização de `Site.master` página mestra para incluir as informações da seção específicas de administração e os links e, em seguida, adicione o código para a página mestra para mostrar ou ocultar essas seções com base em se uma das páginas de administração que está sendo acessada.
3. Criar uma nova página mestra especificamente para a seção de administração, copie a marcação de `Site.master`, adicione as informações da seção específicas de administração e os links e, em seguida, atualize as páginas de conteúdo no `~/Admin` pasta a usar esse novo mestre página.
4. Criar uma página mestre aninhada que associa a `Site.master` e ter as páginas de conteúdo no `~/Admin` essa nova do uso de pastas aninhadas página mestra. Essa página mestre aninhada inclui apenas as informações adicionais e links específicas para as páginas de administração e não precisa repetir a marcação já definida em `Site.master`.

A primeira opção é menos agradável. O objetivo de usar páginas mestras é fique longe de copiar e colar a marcação comuns às novas páginas ASP.NET manualmente. A segunda opção é aceitável, mas faz com que o aplicativo menos manutenção conforme ele dá volume às páginas mestras com marcação que é exibido apenas ocasionalmente e exige que os desenvolvedores editando a página mestra para contornar essa marcação e precise se lembrar quando, exatamente, determinada marcação é exibida em vez de quando ele está oculto. Essa abordagem seria menos tenable como personalizações de cada vez mais tipos de páginas da web precisa ser acomodada por esta página mestre único.

A terceira opção remove a confusão e complexidade emite a superfície com a segunda opção. No entanto, a principal desvantagem de opção de três é que ela requer copiar e colar o layout comuns de `Site.master` para a nova página mestre da seção específica de administração. Se posteriormente, decidir alterar o layout de todo o site temos de lembrar para alterá-la em dois locais.

Páginas mestras aninhadas de quarta opção, nos fornecem o melhor das opções de segundo e terceiros. As informações de layout de todo o site são mantidas em um arquivo - a página mestra nível superior - enquanto o conteúdo específico para regiões específicas separado nos arquivos diferentes.

Este tutorial começa com uma olhada em como criar e usar uma página mestre aninhada simples. Podemos criar uma nova página de mestre de nível superior, duas páginas mestras aninhadas e duas páginas de conteúdo. Começando com "Usando um aninhados mestre para a administração seção da página", vamos examinar nosso arquitetura de página mestra existente para incluir o uso de páginas mestras aninhadas a atualização. Especificamente, podemos criar uma página mestre aninhada e usá-lo para incluir conteúdo personalizado adicional para as páginas de conteúdo no `~/Admin` pasta.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Etapa 1: Criando uma página mestre de nível superior simples

Criar um mestre aninhado com base em um mestre existente páginas e, em seguida, atualizar uma página de conteúdo existente para usar essa nova página mestre aninhada em vez da página mestra de nível superior inclui alguma complexidade porque as páginas de conteúdo existentes já esperam que certos Controles ContentPlaceHolder definidos na página mestre de nível superior. Portanto, a página mestre aninhada também deve incluir os mesmos controles ContentPlaceHolder com os mesmos nomes. Além disso, o nosso aplicativo de demonstração específica tem duas páginas mestras (`Site.master` e `Alternate.master`) que são atribuídos dinamicamente a uma página de conteúdo com base nas preferências do usuário, que adiciona essa complexidade adicional. Abordaremos a atualização do aplicativo existente para usar páginas mestras aninhadas em breve neste tutorial, mas vamos primeiro enfocam um simples aninhados exemplo páginas mestras.

Criar uma nova pasta chamada `NestedMasterPages` e, em seguida, adicionar um novo arquivo de página mestra para a pasta denominada `Simple.master`. (Consulte a Figura 1 para uma captura de tela do Gerenciador de soluções depois que a pasta e o arquivo foi adicionados.) Arraste o `AlternateStyles.css` arquivo de folha de estilo do Gerenciador de soluções para o Designer. Isso adiciona uma `<link>` elemento para o arquivo de folha de estilo no `<head>` elemento, após o qual a página mestra `<head>` marcação do elemento deve parecer com:


[!code-aspx[Main](nested-master-pages-cs/samples/sample1.aspx)]

Em seguida, adicione a seguinte marcação dentro do formulário da Web de `Simple.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample2.aspx)]

Essa marcação exibe um link chamado "Páginas mestras do aninhadas (simples)" na parte superior da página em uma fonte grande branca em um plano de fundo azul marinho. Abaixo que é o `MainContent` ContentPlaceHolder. A Figura 1 mostra a `Simple.master` página mestra quando carregado no Designer do Visual Studio.


[![A página mestre aninhada define o conteúdo específico para as páginas na seção Administração](nested-master-pages-cs/_static/image2.png)](nested-master-pages-cs/_static/image1.png)

**Figura 01**: O aninhados mestre página define conteúdo específicos para as páginas na seção Administração ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Etapa 2: Criando uma página mestre aninhada simples

`Simple.master`contém dois controles ContentPlaceHolder: o `MainContent` ContentPlaceHolder adicionamos dentro do formulário da Web juntamente com o `head` ContentPlaceHolder no `<head>` elemento. Se criar uma página de conteúdo e associá-lo para `Simple.master` a página de conteúdo terá dois controles de conteúdo, fazendo referência as duas ContentPlaceHolders. Da mesma forma, se podemos criar uma página mestre aninhada e associá-lo para `Simple.master` a página mestre aninhada terá dois controles de conteúdo.

Vamos adicionar uma nova página mestre aninhada para o `NestedMasterPages` pasta denominada `SimpleNested.master`. Clique com botão direito no `NestedMasterPages` pasta e escolha Adicionar Novo Item. Isso abre a caixa de diálogo Adicionar Novo Item mostrada na Figura 2. Selecione o tipo de modelo de página mestra e digite o nome a nova página mestra. Para indicar que a nova página mestra deve ser uma página mestre aninhada, marque a caixa de seleção "Selecione página mestra".

Em seguida, clique no botão Adicionar. Isso exibirá o mesmo selecione uma caixa de diálogo de página mestra que você vê quando a associação de uma página de conteúdo a uma página mestra (consulte a Figura 3). Escolha o `Simple.master` página mestra o `NestedMasterPages` pasta e clique em Okey.

> [!NOTE]
> Se você tiver criado seu site da Web ASP.NET usando o modelo de projeto de aplicativo Web em vez do modelo de projeto de Site você não verá a caixa de seleção "Selecionar página mestra" na caixa de diálogo Adicionar Novo Item, mostrada na Figura 2. Para criar uma página mestre aninhada ao usar o modelo de projeto de aplicativo Web, você deve escolher o modelo de página mestre aninhada (em vez do modelo de página mestra). Após selecionar o modelo de página mestra dos aninhados e clicar em Adicionar, o mesmo selecionar uma página mestra que será exibida a caixa de diálogo mostrada na Figura 3.


[![Verifique o &quot;página mestra selecione&quot; caixa de seleção para adicionar uma página mestre aninhada](nested-master-pages-cs/_static/image5.png)](nested-master-pages-cs/_static/image4.png)

**Figura 02**: marque a caixa de seleção "Selecionar página mestra" para adicionar uma página mestre aninhada ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image6.png))


[![Associar a página mestre aninhada para a página mestra Simple](nested-master-pages-cs/_static/image8.png)](nested-master-pages-cs/_static/image7.png)

**Figura 03**: associar a página mestre aninhada para o `Simple.master` página mestra ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image9.png))


Declarativo da página mestre aninhada, mostrado abaixo, contém dois controles de conteúdo, fazendo referência a dois controles de ContentPlaceHolder o nível superior da página mestra.


[!code-aspx[Main](nested-master-pages-cs/samples/sample3.aspx)]

Exceto para o `<%@ Master %>` diretiva, marcação de declarativa inicial da página mestre aninhada é idêntica a marcação que inicialmente é gerado quando uma página de conteúdo de associação para a mesma página mestre de nível superior. Como uma página de conteúdo `<%@ Page %>` diretiva, o `<%@ Master %>` diretiva aqui inclui um `MasterPageFile` atributo que especifica a página mestra do pai da página mestre aninhada. A principal diferença entre a página mestre aninhada e uma página de conteúdo associado à mesma página de nível superior ou mestre é que a página mestre aninhada pode incluir controles ContentPlaceHolder. Controles ContentPlaceHolder da página mestre aninhada definem as regiões em que as páginas de conteúdo podem personalizar a marcação.

Atualizar esta página mestre aninhada para que ele exibe o texto "Olá, de SimpleNested!" no controle do conteúdo que corresponde do `MainContent` controle ContentPlaceHolder.


[!code-aspx[Main](nested-master-pages-cs/samples/sample4.aspx)]

Depois de fazer essa adição, salve a página mestre aninhada e, em seguida, adicione uma nova página de conteúdo para o `NestedMasterPages` pasta denominada `Default.aspx`e associá-lo para o `SimpleNested.master` página mestra. Após adicionar a esta página será surpreso ao ver que ele contém sem controles de conteúdo (consulte a Figura 4)! Uma página de conteúdo pode acessar somente seus *pai* mestre ContentPlaceHolders da página. `SimpleNested.master`contém controles ContentPlaceHolder; Portanto, qualquer página de conteúdo associada a esta página mestre não pode conter controles de conteúdo.


[![A nova página de conteúdo contém sem controles de conteúdo](nested-master-pages-cs/_static/image11.png)](nested-master-pages-cs/_static/image10.png)

**Figura 04**: A nova página contém nenhum conteúdo controles de conteúdo ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image12.png))


O que precisamos fazer é atualizar a página mestre aninhada (`SimpleNested.master`) para incluir controles ContentPlaceHolder. Normalmente, você desejará suas páginas mestras aninhadas para incluir um ContentPlaceHolder para cada ContentPlaceHolder definido pela sua página mestra pai, permitindo assim que sua página mestra filho ou uma página de conteúdo para trabalhar com qualquer ContentPlaceHolder o nível superior da página mestra controles.

Atualização de `SimpleNested.master` página mestra para incluir um ContentPlaceHolder nos seus dois controles de conteúdo. Fornecer controles ContentPlaceHolder o mesmo nome que o controle ContentPlaceHolder a que seu controle de conteúdo refere-se. Ou seja, adicionar um controle ContentPlaceHolder chamado `MainContent` para o conteúdo do controle em `SimpleNested.master` que faz referência a `MainContent` ContentPlaceHolder em `Simple.master`. Fazer a mesma coisa no controle do conteúdo que faz referência a `head` ContentPlaceHolder.

> [!NOTE]
> Enquanto é recomendável nomear os controles ContentPlaceHolder na página mestre aninhada igual ContentPlaceHolders na página mestre de nível superior, esta simetria de nomenclatura não é necessária. Você pode atribuir controles ContentPlaceHolder em sua página mestre aninhada qualquer nome que desejar. No entanto, seja mais fácil lembrar que ContentPlaceHolders correspondem com quais regiões da página se minha página mestre de nível superior e páginas mestras aninhadas usam os mesmos nomes.


Depois de fazer essas adições seu `SimpleNested.master` declarativo da página mestra deve ser semelhante à seguinte:


[!code-aspx[Main](nested-master-pages-cs/samples/sample5.aspx)]

Excluir o `Default.aspx` conteúdo de página que acabamos de criar e, em seguida, adicioná-lo novamente, associando-a para o `SimpleNested.master` página mestra. Neste momento o Visual Studio adiciona dois controles de conteúdo para o `Default.aspx`, fazendo referência a ContentPlaceHolders agora estão definidos no `SimpleNested.master` (veja a Figura 6). Adicione o texto "Olá, de Default.aspx!" Conteúdo de controle que referenciado `MainContent`.

A Figura 5 mostra as três entidades envolvidas aqui - `Simple.master`, `SimpleNested.master`, e `Default.aspx` - e como elas se relacionam entre si. Como mostra o diagrama, a página mestre aninhada implementa controles de conteúdo para ContentPlaceHolder de seu pai. Se essas regiões precisam estar acessíveis para a página de conteúdo, a página mestre aninhada deve adicionar seu próprio ContentPlaceHolders aos controles de conteúdo.


[![As páginas mestras aninhadas e de alto nível determinam o Layout da página de conteúdo](nested-master-pages-cs/_static/image14.png)](nested-master-pages-cs/_static/image13.png)

**Figura 05**: as páginas mestras aninhadas e de alto nível determinam o Layout da página de conteúdo ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image15.png))


Esse comportamento ilustra como uma página de conteúdo ou a página mestra somente é conhecimento de sua página mestra pai. Esse comportamento também é indicado pelo Designer do Visual Studio. A Figura 6 mostra o Designer para `Default.aspx`. Enquanto o Designer mostra claramente quais regiões são editáveis da página de conteúdo e não são partes, ele não resolver a ambiguidade de quais são regiões não editáveis na página mestre aninhada e quais regiões são da página mestre de nível superior.


[![O conteúdo agora de página inclui controles de conteúdo para ContentPlaceHolders da página mestre aninhada](nested-master-pages-cs/_static/image17.png)](nested-master-pages-cs/_static/image16.png)

**Figura 06**: O conteúdo página agora inclui controles de conteúdo para ContentPlaceHolders aninhados mestre da página ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Etapa 3: Adicionando uma segunda página de mestre aninhada simples

O benefício de páginas mestras aninhadas fica mais evidente quando há várias páginas mestras aninhadas. Para ilustrar esse benefício, crie outra página mestre aninhada no `NestedMasterPages` pasta; página mestre aninhada Nomeie esse novo `SimpleNestedAlternate.master` e associá-lo para o `Simple.master` página mestra. Adicione controles ContentPlaceHolder nos dois controles de conteúdo da página mestre aninhada como fizemos na etapa 2. Também adicionar o texto "Olá, de SimpleNestedAlternate!" no controle do conteúdo que corresponde da nível superior da página mestra `MainContent` ContentPlaceHolder. Após fazer essas alterações declarativo do sua nova página mestre aninhada deve ser semelhante ao seguinte:


[!code-aspx[Main](nested-master-pages-cs/samples/sample6.aspx)]

Criar uma página de conteúdo chamada `Alternate.aspx` no `NestedMasterPages` pasta e associá-lo para o `SimpleNestedAlternate.master` página mestre aninhada. Adicione o texto "Olá, de alternativa!" no controle do conteúdo que corresponde ao `MainContent`. A Figura 7 mostra `Alternate.aspx` quando visualizado no Designer do Visual Studio.


[![Alternate.aspx é vinculada à página mestra SimpleNestedAlternate.master](nested-master-pages-cs/_static/image20.png)](nested-master-pages-cs/_static/image19.png)

**Figura 07**: `Alternate.aspx` está associado para o `SimpleNestedAlternate.master` página mestra ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image21.png))


Compare o Designer na Figura 7 para o Designer na Figura 6. Ambas as páginas de conteúdo compartilham o mesmo layout definido na página mestre de nível superior (`Simple.master`), ou seja, o título "Aninhadas mestre páginas Tutorial (simples)". Ainda têm conteúdo distinto definido em suas páginas mestras de pai - o texto "Olá, de SimpleNested!" na Figura 6 e "Hello, de SimpleNestedAlternate!" na Figura 7. Concedido, essas diferenças aqui são simples, mas você pode estender esse exemplo para incluir diferenças mais significativas. Por exemplo, o `SimpleNested.master` página pode incluir um menu com opções específicas para suas páginas de conteúdo, enquanto `SimpleNestedAlternate.master` pode ter informações pertinentes para as páginas de conteúdo que se associar a ele.

Agora, imagine que precisávamos fazer uma alteração para o layout geral do site. Por exemplo, imagine que queremos adicionar uma lista de links comuns a todas as páginas de conteúdo. Para fazer isso podemos atualizar página mestre de nível superior, `Simple.master`. Quaisquer alterações são refletidas imediatamente em suas páginas mestras aninhadas e, por extensão, suas páginas de conteúdo.

Para demonstrar a facilidade com que podemos alterar o layout geral do site, abra o `Simple.master` página mestra e adicione a seguinte marcação entre o `topContent` e `mainContent` `<div>` elementos:


[!code-aspx[Main](nested-master-pages-cs/samples/sample7.aspx)]

Isso adiciona dois links para a parte superior de cada página que vincula a `Simple.master`, `SimpleNested.master`, ou `SimpleNestedAlternate.master`; essas alterações serão aplicadas imediatamente aninhadas todas as páginas mestras e suas páginas de conteúdo. A Figura 8 mostra `Alternate.aspx` quando visualizada através de um navegador. Observe a adição de links na parte superior da página (em comparação comparada a Figura 7).


[![Alterado para a página mestra de nível superior são refletidas imediatamente em suas páginas mestras aninhadas e suas páginas de conteúdo](nested-master-pages-cs/_static/image23.png)](nested-master-pages-cs/_static/image22.png)

**Figura 08**: alterado para a página mestra de nível superior são refletidas imediatamente em suas páginas mestras aninhadas e suas páginas de conteúdo ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Usando uma página mestre aninhada para a seção de administração

Neste ponto, analisamos as vantagens do mestre aninhada de páginas e vimos como criar e usá-los em um aplicativo ASP.NET. Os exemplos nas etapas 1, 2 e 3, no entanto, envolvidos criando uma nova página mestre de nível superior, novas páginas mestras aninhadas e novas páginas de conteúdo. Como adicionar uma nova página mestre aninhada em um site com uma página mestre de nível superior existente e páginas de conteúdo?

Integrar uma página mestre aninhada em um site existente e associá-la a páginas de conteúdo existentes requer um pouco mais esforço que começar do zero. As etapas 4, 5, 6 e 7 explorar esses desafios, como podemos aumentar o nosso aplicativo de demonstração para incluir uma nova página mestre aninhada chamada `AdminNested.master` que contém instruções para o administrador e é usado por páginas ASP.NET o `~/Admin` pasta.

Integrar uma página mestre aninhada em nosso aplicativo de demonstração apresenta as barreiras a seguir:

- O conteúdo existente páginas o `~/Admin` pasta têm certas expectativas de sua página mestra. Para começar, eles esperam que certos controles ContentPlaceHolder esteja presente. Além disso, o `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` páginas chamar público da página mestra `RefreshRecentProductsGrid` método, defina seu `GridMessageText` propriedade, ou tem um manipulador de eventos para sua `PricesDoubled` eventos. Consequentemente, nossa página mestre aninhada deve fornecer a mesma ContentPlaceHolders e membros públicos.
- No tutorial anterior é aprimorada a `BasePage` classe para definir dinamicamente o `Page` do objeto `MasterPageFile` propriedade com base em uma variável de sessão. Como a oferecemos suporte a páginas mestras dinâmicas ao usar páginas mestras aninhadas?

Esses dois desafios serão superficial criar a página mestre aninhada e usá-lo de nossas páginas de conteúdo existentes. Vamos investigar e superar esses problemas que ocorrerem.

## <a name="step-4-creating-the-nested-master-page"></a>Etapa 4: Criar a página mestre aninhada

A primeira tarefa é criar a página mestre aninhada a ser usado pelas páginas na seção Administração. Como vimos na etapa 2, quando adicionar um novo aninhados página mestra é necessário especificar a página mestra do pai da página mestre aninhada. Mas temos duas páginas mestras de nível superior: `Site.master` e `Alternate.master`. Lembre-se que criamos `Alternate.master` no tutorial anterior e escrevemos código o `BasePage` classe que definem o objeto de página `MasterPageFile` propriedade em tempo de execução para o `Site.master` ou `Alternate.master` dependendo do valor da `MyMasterPage` Variável de sessão.

Como podemos configurar nossa página mestre aninhada para que ele use a página mestra de nível superior apropriada? Temos duas opções:

- Criar duas páginas mestras aninhadas, `AdminNestedSite.master` e `AdminNestedAlternate.master`e associá-las para as nível superior páginas mestras `Site.master` e `Alternate.master`, respectivamente. Em `BasePage`, em seguida, definiríamos o `Page` do objeto `MasterPageFile` à página mestre aninhada apropriada.
- Crie uma única página mestre aninhada e faça com que as páginas de conteúdo use essa página mestra específica. Em seguida, em tempo de execução, é necessário definir da página mestre aninhada `MasterPageFile` propriedade para a página mestra de nível superior apropriada em tempo de execução. (Como você pode ter entendido agora, páginas mestras também têm um `MasterPageFile` propriedade.)

Vamos usar a segunda opção. Criar um único aninhada mestre arquivo de paginação no `~/Admin` pasta denominada `AdminNested.master`. Porque ambos `Site.master` e `Alternate.master` têm o mesmo conjunto de controles ContentPlaceHolder, não importa qual página mestra você vinculá-lo, embora você deve associá-lo a `Site.master` para a mesma da consistência.


[![Adicione uma página mestre aninhada para a pasta ~/Admin.](nested-master-pages-cs/_static/image26.png)](nested-master-pages-cs/_static/image25.png)

**Figura 09**: adicionar uma página mestre aninhada para o `~/Admin` pasta. ([Clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image27.png))


Como a página mestre aninhada está associada a uma página mestra com quatro controles ContentPlaceHolder, o Visual Studio adiciona quatro controles de conteúdo para marcação inicial do novo arquivo de página mestre aninhada. Como nas etapas 2 e 3, adicionar um controle ContentPlaceHolder em cada controle de conteúdo, dando a ele o mesmo nome que o controle de ContentPlaceHolder o nível superior da página mestra. Além disso, adicione a seguinte marcação para o controle de conteúdo que corresponde do `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-cs/samples/sample8.html)]

Em seguida, defina o `instructions` da classe CSS no `Styles.css` e `AlternateStyles.css` arquivos CSS. As seguintes regras CSS causam elementos HTML com o estilo de `instructions` classe a ser exibido com uma cor de plano de fundo amarelo claro e uma borda preta sólida:


[!code-css[Main](nested-master-pages-cs/samples/sample9.css)]

Porque esta marcação foi adicionada à página mestre aninhada, ele só aparecerá em páginas que usem essa página mestre aninhada (ou seja, as páginas na seção Administração).

Depois de fazer essas adições para sua página mestre aninhada, sua marcação declarativa deve ser semelhante ao seguinte:


[!code-aspx[Main](nested-master-pages-cs/samples/sample10.aspx)]

Observe que cada controle de conteúdo tem um controle ContentPlaceHolder e que os controles ContentPlaceHolder `ID` propriedades recebem os mesmos valores que os controles ContentPlaceHolder correspondentes na página mestre de nível superior. Além disso, a marcação de seção específica de administração aparece no `MainContent` ContentPlaceHolder.

A Figura 10 mostra a `AdminNested.master` página mestre aninhada quando visualizado no Designer do Visual Studio. Você pode ver as instruções na caixa amarela na parte superior do `MainContent` controle de conteúdo.


[![A página mestre aninhada estende a página mestre de nível superior para incluir instruções para o administrador.](nested-master-pages-cs/_static/image29.png)](nested-master-pages-cs/_static/image28.png)

**Figura 10**: A página mestre aninhada estende a página mestre de nível superior para incluir instruções para o administrador. ([Clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Etapa 5: Atualizando as páginas de conteúdo existentes para usar a nova página mestre aninhada

A qualquer momento, adicionamos uma nova página de conteúdo para a seção de administração, é preciso associá-lo para o `AdminNested.master` página mestra que acabamos de criar. Mas e quanto existente páginas de conteúdo? No momento, todas as páginas de conteúdo no site derivam de `BasePage` classe, que define programaticamente o conteúdo da página mestra em tempo de execução. Isso não é o comportamento que queremos para as páginas de conteúdo na seção Administração. Em vez disso, queremos que essas páginas de conteúdo para usar sempre o `AdminNested.master` página. É responsabilidade da página mestre aninhada para escolher a página de conteúdo no nível superior no tempo de execução.

A melhor maneira de fazer isso desejado comportamento é criar uma nova classe de página de base personalizada denominada `AdminBasePage` que estende o `BasePage` classe. `AdminBasePage`pode, em seguida, substituir o `SetMasterPageFile` e defina o `Page` do objeto `MasterPageFile` com o valor inserido no código "~ / Admin/AdminNested.master". Dessa forma, qualquer página que deriva de `AdminBasePage` usará `AdminNested.master`, enquanto qualquer página que deriva de `BasePage` terá seu `MasterPageFile` propriedade definido dinamicamente para "~ / Site.master" ou "~ / Alternate.master" com base no valor da `MyMasterPage` Variável de sessão.

Comece adicionando um novo arquivo de classe para o `App_Code` pasta denominada `AdminBasePage.cs`. Ter `AdminBasePage` estender `BasePage` e, em seguida, substituir o `SetMasterPageFile` método. Nesse método, atribua o `MasterPageFile` o valor "~ / Admin/AdminNested.master". Após fazer essas alterações em sua classe de arquivo deve ser semelhante ao seguinte:


[!code-csharp[Main](nested-master-pages-cs/samples/sample11.cs)]

Agora precisamos as páginas de conteúdo existentes na administração seção derivam `AdminBasePage` em vez de `BasePage`. Vá para o arquivo de classe code-behind para cada página de conteúdo no `~/Admin` pasta e fazer essa alteração. Por exemplo, no `~/Admin/Default.aspx` altere a declaração da classe por trás do código de:


[!code-csharp[Main](nested-master-pages-cs/samples/sample12.cs)]

Para:


[!code-csharp[Main](nested-master-pages-cs/samples/sample13.cs)]

Figura 11 mostra como a página mestra de nível superior (`Site.master` ou `Alternate.master`), a página mestre aninhada (`AdminNested.master`), e as páginas de conteúdo de seção Administração se relacionam entre si.


[![A página mestre aninhada define o conteúdo específico para as páginas na seção Administração](nested-master-pages-cs/_static/image32.png)](nested-master-pages-cs/_static/image31.png)

**Figura 11**: O aninhados mestre página define conteúdo específicos para as páginas na seção Administração ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Etapa 6: Espelhamento de propriedades e métodos públicos da página mestra

Lembre-se de que o `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` páginas interagem programaticamente com a página mestra: `~/Admin/AddProduct.aspx` chamadas a página mestra do pública `RefreshRecentProductsGrid` método e define seu `GridMessageText` propriedade; `~/Admin/Products.aspx` tem um manipulador de eventos para o `PricesDoubled` evento. No tutorial anterior, criamos um resumo `BaseMasterPage` classe que definiu esses membros públicos.

O `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` páginas supõem que sua página mestra deriva o `BaseMasterPage` classe. O `AdminNested.master` página, no entanto, atualmente estende o `System.Web.UI.MasterPage` classe. Como resultado, quando visitar `~/Admin/Products.aspx` um `InvalidCastException` será lançada com a mensagem: "não é possível converter o objeto do tipo ' ASP.admin\_adminnested\_mestre ' no tipo 'BaseMasterPage'."

Para corrigir isso, precisamos ter a `AdminNested.master` estender a classe code-behind `BaseMasterPage`. Atualize a declaração de classe por trás do código da página mestre aninhada de:


[!code-csharp[Main](nested-master-pages-cs/samples/sample14.cs)]

Para:


[!code-csharp[Main](nested-master-pages-cs/samples/sample15.cs)]

Não está pronto ainda. Porque o `BaseMasterPage` classe é abstrata, é preciso substituir o `abstract` membros, `RefreshRecentProductsGrid` e `GridMessageText`. Esses membros são usados pelas páginas mestras nível superior para atualizar suas interfaces de usuário. (Na verdade, somente o `Site.master` página mestra usa esses métodos, embora ambas as páginas de nível superior mestres implementam esses métodos, desde que ambos estendem `BaseMasterPage`.)

Quando é necessário implementar esses membros de `AdminNested.master`, todas essas implementações precisam fazer é simplesmente chamar o mesmo membro no nível superior página mestra usada pela página mestre aninhada. Por exemplo, quando uma página de conteúdo na seção Administração chama da página mestre aninhada `RefreshRecentProductsGrid` , a página mestre aninhada precisa fazer é, por sua vez, chame `Site.master` ou `Alternate.master`do `RefreshRecentProductsGrid` método.

Para fazer isso, comece adicionando o seguinte `@MasterType` diretiva na parte superior da `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample16.aspx)]

Lembre-se de que o `@MasterType` diretiva adiciona uma propriedade fortemente tipado a classe code-behind chamado `Master`. Em seguida, substituir o `RefreshRecentProductsGrid` e `GridMessageText` membros e simplesmente delegar a chamada para o `Master`correspondente do método:


[!code-csharp[Main](nested-master-pages-cs/samples/sample17.cs)]

Com esse código, você poderá visite e use as páginas de conteúdo na seção Administração. A Figura 12 mostra o `~/Admin/Products.aspx` página quando visualizada através de um navegador. Como você pode ver, a página inclui a caixa de instruções de administração, que é definida na página mestre aninhada.


[![As páginas de conteúdo na seção Administração incluem instruções na parte superior de cada página](nested-master-pages-cs/_static/image35.png)](nested-master-pages-cs/_static/image34.png)

**Figura 12**: as páginas de conteúdo a seção Administração incluem instruções na parte superior de cada página ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Etapa 7: Usando a página de mestre de nível superior apropriado em tempo de execução

Enquanto todas as páginas de conteúdo na seção administração são totalmente funcionais, eles usam a mesma página mestre de nível superior e ignorar a página mestra selecionada pelo usuário em `ChooseMasterPage.aspx`. Esse comportamento é devido ao fato de que a página mestre aninhada tem seu `MasterPageFile` propriedade estaticamente definida como `Site.master` no seu `<%@ Master %>` diretiva.

Para usar a página de nível superior mestre selecionada pelo usuário final, precisamos definir o `AdminNested.master`do `MasterPageFile` propriedade para o valor de `MyMasterPage` variável de sessão. Porque definimos as conteúdo das páginas `MasterPageFile` propriedades no `BasePage`, talvez você ache que definiríamos o da página mestre aninhada `MasterPageFile` propriedade no `BaseMasterPage` ou o `AdminNested.master`da classe code-behind. Isso não funcionar, no entanto, porque é preciso ter o `MasterPageFile` propriedade até o final do estágio PreInit. A hora mais antiga que é capaz de explorar programaticamente o ciclo de vida da página de uma página mestra é o estágio de inicialização (que ocorre depois da fase PreInit).

Portanto, precisamos definir da página mestre aninhada `MasterPageFile` propriedade das páginas de conteúdo. O conteúdo apenas páginas que usam o `AdminNested.master` página mestra derivam `AdminBasePage`. Portanto, podemos pode colocar essa lógica existe. Na etapa 5, substitui o `SetMasterPageFile` método, configurando o `Page` do objeto `MasterPageFile` propriedade como "~ / Admin/AdminNested.master". Atualização `SetMasterPageFile` também definir a página mestra `MasterPageFile` propriedade para o resultado armazenado na sessão:


[!code-csharp[Main](nested-master-pages-cs/samples/sample18.cs)]

O `GetMasterPageFileFromSession` método, que adicionamos ao `BasePage` classe no tutorial anterior, retorna o caminho do arquivo de página mestra apropriada com base no valor da variável de sessão.

Com essa alteração no local, a seleção do usuário página mestra traz para a seção de administração. Figura 13 mostra a mesma página como Figura 12, mas depois que o usuário alterou sua seleção de página mestra para `Alternate.master`.


[![A página Administração aninhada usa a página de nível superior mestre selecionado pelo usuário](nested-master-pages-cs/_static/image38.png)](nested-master-pages-cs/_static/image37.png)

**Figura 13**: A página de administração aninhadas usa a página mestra de nível superior selecionado pelo usuário ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image39.png))


## <a name="summary"></a>Resumo

Quantidade páginas de conteúdo como like podem vincular a uma página mestre, é possível criar aninhada páginas mestras por uma página mestra filho vinculam a uma página mestra pai. A página mestra filha pode definir os controles de conteúdo para cada ContentPlaceHolders de seu pai; em seguida, ela pode adicionar seus próprios controles ContentPlaceHolder (bem como outros marcação) para esses controles de conteúdo. Páginas mestras aninhadas são muito úteis em aplicativos web grande em que todas as páginas compartilham uma aparência abrangente, embora algumas seções do site exigem personalizações exclusivas.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Páginas mestras ASP.NET aninhadas](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Dicas para páginas mestras aninhadas e tempo de Design do VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 aninhado o suporte da página mestra](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 3.5 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou em seu blog [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha em[mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](specifying-the-master-page-programmatically-cs.md)
[Próximo](creating-a-site-wide-layout-using-master-pages-vb.md)
