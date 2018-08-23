---
uid: web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
title: Aninhado páginas mestras (c#) | Microsoft Docs
author: rick-anderson
description: Mostra como aninhar uma página mestra dentro de outra.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 32b7fb6e-d74b-4048-91f8-70631b2523ee
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/nested-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 1b60b0b7ce4be66bc24ccbc1d25ce4dc56766815
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831628"
---
<a name="nested-master-pages-c"></a>Páginas mestras aninhadas (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_10_CS.pdf)

> Mostra como aninhar uma página mestra dentro de outra.


## <a name="introduction"></a>Introdução

Ao longo dos últimos nove tutoriais nós vimos como implementar um layout de todo o site com páginas mestras. Em resumo, páginas mestras permitem que nós, se o desenvolvedor de página para definir a marcação comuns na página mestra, juntamente com a regiões específicas que podem ser personalizados em uma base de página de conteúdo a página de conteúdo. Controles em uma página mestra ContentPlaceHolder indicam as regiões personalizáveis; a marcação personalizada para controles ContentPlaceHolder são definidas na página de conteúdo por meio de controles de conteúdo.

As técnicas de página mestra que exploramos até o momento são ótimas, se você tiver um único layout usado em todo o site. No entanto, muitos sites grandes têm um layout de site personalizada em várias seções. Por exemplo, considere um aplicativo de serviços de saúde usado pela equipe do hospital para gerenciar a cobrança, atividades e informações do paciente. Pode haver três tipos de páginas da web neste aplicativo:

- Páginas de específicas de membro da equipe em que os membros da equipe podem atualizar a disponibilidade, Exibir agendas ou solicitação de férias.
- Páginas de paciente específicas em que os membros da equipe exibir ou editar informações do paciente específico.
- Páginas de cobrança específico em que os contadores examinar atual de declaração de status e relatórios financeiros.

Todas as páginas podem compartilhar um layout comum, como um menu na parte superior e uma série de links usados com frequência na parte inferior. Mas a equipe, pacientes e páginas específicas de cobrança, talvez seja necessário personalizar esse layout genérico. Por exemplo, talvez todas as páginas específicas da equipe devem incluir uma lista de calendário e tarefas que mostra a disponibilidade e uma agenda diária do usuário conectado no momento. Talvez precisam de todas as páginas específicas do paciente mostrar o nome, endereço e informações de seguro para o paciente cujas informações está sendo editadas.

É possível criar esses layouts personalizados usando *páginas mestras aninhadas*. Para implementar o cenário acima, seria começamos criando uma página mestra que definiu o conteúdo em todo o site de layout, o menu e o rodapé, com ContentPlaceHolders definindo as regiões personalizáveis. Criaremos três páginas mestras aninhadas, uma para cada tipo de página da web. Cada página mestra aninhada definiria o conteúdo entre os tipos de páginas de conteúdo que use a página mestra. Em outras palavras, a página mestra aninhada para páginas de conteúdo específico do paciente incluiria marcação e lógica programática para exibir informações sobre o paciente que está sendo editado. Ao criar uma nova página específica do paciente seria a ligamos para essa página mestra aninhada.

Este tutorial começa, realçando os benefícios das páginas mestras aninhadas. Em seguida, ele mostra como criar e usar as páginas mestras aninhadas.

> [!NOTE]
> Páginas mestras aninhadas tem sido possíveis desde a versão 2.0 do .NET Framework. No entanto, o Visual Studio 2005 não incluíam suporte de tempo de design para páginas mestras aninhadas. A boa notícia é que o Visual Studio 2008 oferece uma experiência avançada de tempo de design para páginas mestras aninhadas. Se você estiver interessado em usar páginas mestras aninhadas, mas ainda estiver usando o Visual Studio 2005, fazer check-out [Scott Guthrie](https://weblogs.asp.net/scottgu/)da entrada de blog [dicas para páginas mestras aninhadas em tempo de Design do VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx).


## <a name="the-benefits-of-nested-master-pages"></a>Os benefícios de páginas mestras aninhadas

Muitos sites têm um design abrangente do site, bem como mais designs personalizados específicos para determinados tipos de páginas. Por exemplo, em nosso aplicativo web de demonstração criamos uma seção de administração rudimentar (as páginas no `~/Admin` pasta). Atualmente, as páginas da web na `~/Admin` pasta usar a mesma página mestra como essas páginas não na seção administration (ou seja, `Site.master` ou `Alternate.master`, dependendo da seleção do usuário).

> [!NOTE]
> Por enquanto, fingir que nosso site tem apenas uma página mestra, `Site.master`. Vamos abordar usando páginas mestras aninhadas com duas (ou mais) páginas mestras, começando com "Usando um aninhada mestre de página para a seção Administração" posteriormente no tutorial.


Imagine que foi solicitado que personalizar o layout das páginas de administração para incluir informações adicionais ou links que normalmente não estariam presentes em outras páginas no site. Há quatro técnicas para implementar esse requisito:

1. Adicionar manualmente as informações específicas da administração e links para cada página de conteúdo no `~/Admin` pasta.
2. Atualização de `Site.master` página mestra para incluir as informações da seção específica de administração e links e, em seguida, adicione o código para a página mestra para mostrar ou ocultar essas seções com base em se uma das páginas de administração está sendo visitada.
3. Criar uma nova página mestra especificamente para a seção de administração, copiar sobre a marcação de `Site.master`, adicione as informações da seção específica de administração e os links e, em seguida, atualize as páginas de conteúdo no `~/Admin` pasta a usar esse novo mestre página.
4. Criar uma página mestra aninhada que associa a `Site.master` e têm as páginas de conteúdo no `~/Admin` aninhados de pasta use essa nova página mestra. Essa página mestra aninhada incluiria apenas as informações adicionais e links específicos para as páginas de administração e não precisa repetir a marcação já definida no `Site.master`.

A primeira opção é menos agradável. É o ponto de uso de páginas mestras deixar de ter que copiar e colar marcação comum para as páginas ASP.NET manualmente. A segunda opção é aceitável, mas torna o aplicativo menos capaz de manutenção como ele dá volume às páginas mestras com a marcação que é exibido apenas ocasionalmente e exige que os desenvolvedores editando a página mestra para contornar essa marcação e precisa se lembrar de quando, exatamente, determinada marcação é exibida em vez de quando ele está oculto. Essa abordagem seria menos tenable como personalizações de cada vez mais tipos de páginas da web precisava ser acomodado por essa única página mestra.

A terceira opção remove a confusão e complexidade emite a superfície com a segunda opção. No entanto, a principal desvantagem de opção de três é requer us copiar e colar o layout comum de `Site.master` para a nova página mestre da seção específica de administração. Se, posteriormente, decidir alterar o layout de todo o site que temos de lembrar para alterá-lo em dois locais.

Páginas mestras aninhadas de quarta opção, envie-no melhor das opções de segundo e terceiro. As informações de layout de todo o site são mantidas em um arquivo: a página mestre de nível superior – enquanto o conteúdo específico para regiões específicas é separado em arquivos diferentes.

Este tutorial começa com uma olhada em como criar e usar uma página mestra aninhada simples. Criamos uma página mestre de nível superior totalmente nova, duas páginas mestras aninhadas e duas páginas de conteúdo. Começando com "Usando um aninhados mestre para a administração seção da página", vamos examinar atualizando nossa arquitetura de página mestra existente para incluir o uso de páginas mestras aninhadas. Especificamente, criamos uma página mestra aninhada e usá-lo para incluir mais conteúdo personalizado para as páginas de conteúdo no `~/Admin` pasta.

## <a name="step-1-creating-a-simple-top-level-master-page"></a>Etapa 1: Criando uma página simples de mestre de nível superior

Criar um mestre aninhado com base em um do mestre existente de páginas e, em seguida, atualizar uma página de conteúdo existente para usar essa nova página mestra aninhada em vez de nível superior página mestra envolve alguma complexidade porque as páginas de conteúdo existentes já esperam que certas Controles de ContentPlaceHolder definidos na página mestre de nível superior. Portanto, a página mestra aninhada também deve incluir os mesmos controles ContentPlaceHolder com os mesmos nomes. Além disso, nosso aplicativo de demonstração específica tem duas páginas mestras (`Site.master` e `Alternate.master`) que são atribuídos dinamicamente uma página de conteúdo com base nas preferências do usuário, que aumenta ainda mais essa complexidade. Examinaremos de atualização do aplicativo existente para usar páginas mestras aninhadas mais tarde neste tutorial, mas vamos primeiro enfocam um simples aninhados do exemplo de páginas mestras.

Criar uma nova pasta chamada `NestedMasterPages` e, em seguida, adicione um novo arquivo de página mestra para essa pasta denominada `Simple.master`. (Consulte a Figura 1 para uma captura de tela do Gerenciador de soluções depois que essa pasta e arquivo foram adicionadas.) Arraste o `AlternateStyles.css` arquivo de folha de estilo do Gerenciador de soluções para o Designer. Isso adiciona uma `<link>` elemento para o arquivo de folha de estilos em de `<head>` elemento após o qual a página mestra `<head>` marcação do elemento deve parecer com:


[!code-aspx[Main](nested-master-pages-cs/samples/sample1.aspx)]

Em seguida, adicione a seguinte marcação dentro do formulário da Web de `Simple.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample2.aspx)]

Essa marcação exibe um link intitulado "Nested as páginas mestras (simples)" na parte superior da página em uma fonte branca grande em um plano de fundo azul marinho. Além disso, é o `MainContent` ContentPlaceHolder. A Figura 1 mostra o `Simple.master` página mestra quando carregado no Designer do Visual Studio.


[![A página mestra aninhada define o conteúdo específico para as páginas na seção Administração](nested-master-pages-cs/_static/image2.png)](nested-master-pages-cs/_static/image1.png)

**Figura 01**: O aninhados mestre página define conteúdo específicos para as páginas na seção Administration ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image3.png))


## <a name="step-2-creating-a-simple-nested-master-page"></a>Etapa 2: Criando uma página mestra aninhada simples

`Simple.master` contém dois controles ContentPlaceHolder: o `MainContent` ContentPlaceHolder adicionamos dentro do formulário da Web juntamente com o `head` ContentPlaceHolder no `<head>` elemento. Se fôssemos criar uma página de conteúdo e associá-lo para `Simple.master` a página de conteúdo teria dois controles de conteúdo, fazendo referência as duas ContentPlaceHolders. Da mesma forma, se podemos criar uma página mestra aninhada e associá-lo para `Simple.master` e em seguida, a página mestra aninhada terá dois controles de conteúdo.

Vamos adicionar uma nova página mestra aninhada para o `NestedMasterPages` pasta chamada `SimpleNested.master`. Clique com botão direito no `NestedMasterPages` pasta e escolha Add New Item. Isso abre a caixa de diálogo Add New Item mostrada na Figura 2. Selecione o tipo de modelo de página mestra e digite o nome a nova página mestra. Para indicar que a nova página mestra deve ser uma página mestra aninhada, marque a caixa de seleção "Selecionar página mestra".

Em seguida, clique no botão Adicionar. Isso exibirá o mesmo selecione uma caixa de diálogo de página mestra que você vê quando uma página de conteúdo de associação a uma página mestra (consulte a Figura 3). Escolha o `Simple.master` página mestra no `NestedMasterPages` pasta e clique em Okey.

> [!NOTE]
> Se você tiver criado seu site da Web ASP.NET usando o modelo de projeto de aplicativo Web em vez do modelo de projeto de Site, você não verá a caixa de seleção "Selecionar a página mestra" na caixa de diálogo Add New Item, mostrada na Figura 2. Para criar uma página mestra aninhada ao usar o modelo de projeto de aplicativo Web, você deve escolher o modelo de página mestra aninhada (em vez do modelo de página mestra). Depois de selecionar o modelo de página mestra aninhada e clicando em Adicionar, o mesmo selecionar uma página mestra que será exibida a caixa de diálogo mostrada na Figura 3.


[![Verifique a &quot;página mestra selecione&quot; caixa de seleção para adicionar uma página de mestra aninhada](nested-master-pages-cs/_static/image5.png)](nested-master-pages-cs/_static/image4.png)

**Figura 02**: marque a caixa de seleção "Selecionar a página mestra" para adicionar uma página de mestra aninhada ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image6.png))


[![Associar a página mestra aninhada para a página mestra Simple](nested-master-pages-cs/_static/image8.png)](nested-master-pages-cs/_static/image7.png)

**Figura 03**: associar a página mestra aninhada para o `Simple.master` página mestra ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image9.png))


Marcação declarativa da página mestra aninhada, mostrada abaixo, contém dois controles de conteúdo, fazendo referência a dois controles de ContentPlaceHolder o nível superior da página mestra.


[!code-aspx[Main](nested-master-pages-cs/samples/sample3.aspx)]

Exceto para o `<%@ Master %>` diretiva, marcação declarativa do inicial da página mestra aninhada é idêntica à marcação que é gerada inicialmente ao associar uma página de conteúdo para a mesma página mestre de nível superior. Como uma página de conteúdo `<%@ Page %>` diretiva, o `<%@ Master %>` diretiva aqui inclui um `MasterPageFile` atributo que especifica a página mestra do pai da página mestra aninhada. A principal diferença entre a página mestra aninhada e uma página de conteúdo associado para a mesma página mestre de nível superior é que a página mestra aninhada pode incluir controles ContentPlaceHolder. Controles de ContentPlaceHolder da página mestra aninhada definem as regiões onde as páginas de conteúdo podem personalizar a marcação.

Atualizar esta página mestra aninhada para que ele exibe o texto "Olá, de SimpleNested!" no controle de conteúdo que corresponde ao `MainContent` controle ContentPlaceHolder.


[!code-aspx[Main](nested-master-pages-cs/samples/sample4.aspx)]

Depois de fazer essa adição, salve a página mestra aninhada e, em seguida, adicione uma nova página de conteúdo para o `NestedMasterPages` pasta chamada `Default.aspx`e associá-lo para o `SimpleNested.master` página mestra. Após a adição desta página, você pode se surpreender ao ver que ele contém sem controles de conteúdo (consulte a Figura 4)! Uma página de conteúdo só pode acessar seus *pai* mestre ContentPlaceHolders da página. `SimpleNested.master` não contém todos os controles ContentPlaceHolder; Portanto, qualquer página de conteúdo associada a essa página mestra não pode conter quaisquer controles de conteúdo.


[![Nova página de conteúdo não contém nenhum controle de conteúdo](nested-master-pages-cs/_static/image11.png)](nested-master-pages-cs/_static/image10.png)

**Figura 04**: A nova página contém nenhum conteúdo controles de conteúdo ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image12.png))


O que precisamos fazer é atualizar a página mestra aninhada (`SimpleNested.master`) para incluir controles ContentPlaceHolder. Normalmente você desejará que suas páginas mestras aninhadas para incluir um ContentPlaceHolder para cada ContentPlaceHolder definido pela página mestra pai, permitindo assim que sua página mestra filha ou a página conteúdo para trabalhar com qualquer um ContentPlaceHolder de nível superior da página mestra controles.

Atualização de `SimpleNested.master` página mestra para incluir um ContentPlaceHolder em seus dois controles de conteúdo. Dão aos controles ContentPlaceHolder o mesmo nome que o controle de ContentPlaceHolder a que seu controle de conteúdo refere-se. Ou seja, adicione um controle de ContentPlaceHolder chamado `MainContent` para o conteúdo do controle no `SimpleNested.master` que faz referência a `MainContent` ContentPlaceHolder em `Simple.master`. Fazer a mesma coisa no controle de conteúdo que faz referência a `head` ContentPlaceHolder.

> [!NOTE]
> Embora eu recomende nomear os controles de ContentPlaceHolder na página mestra aninhada o mesmo que o ContentPlaceHolders na página mestre de nível superior, essa nomenclatura simetria não é necessária. Você pode atribuir controles ContentPlaceHolder na sua página mestra aninhada qualquer nome que você deseja. No entanto, eu acho mais fácil lembrar o que ContentPlaceHolders correspondem com quais regiões da página se minha página mestre de nível superior e páginas mestras aninhadas usam os mesmos nomes.


Depois de fazer essas adições seu `SimpleNested.master` marcação declarativa da página mestra deve ser semelhante ao seguinte:


[!code-aspx[Main](nested-master-pages-cs/samples/sample5.aspx)]

Excluir o `Default.aspx` que acabamos de criar de página de conteúdo e, em seguida, adicioná-la novamente, associando-o para o `SimpleNested.master` página mestra. Desta vez o Visual Studio adiciona dois controles de conteúdo para o `Default.aspx`, referenciar o ContentPlaceHolders agora definido no `SimpleNested.master` (veja a Figura 6). Adicione o texto, "Olá, de Default. aspx!" no conteúdo do controle que referenciado `MainContent`.

A Figura 5 mostra as três entidades envolvidas aqui – `Simple.master`, `SimpleNested.master`, e `Default.aspx` – e como elas se relacionam entre si. Como mostra o diagrama, a página mestra aninhada implementa controles de conteúdo para ContentPlaceHolder do seu pai. Se essas regiões precisam estar acessíveis para a página de conteúdo, a página mestra aninhada deve adicionar seu próprio ContentPlaceHolders aos controles de conteúdo.


[![As páginas mestras aninhadas e de alto nível das ditam o Layout da página de conteúdo](nested-master-pages-cs/_static/image14.png)](nested-master-pages-cs/_static/image13.png)

**Figura 05**: as páginas mestras aninhadas e de alto nível ditam o Layout da página de conteúdo ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image15.png))


Esse comportamento ilustra como uma página de conteúdo ou a página mestra só está ciente da página mestra pai. Esse comportamento também é indicado pelo Designer do Visual Studio. A Figura 6 mostra o Designer para `Default.aspx`. Enquanto o Designer mostra claramente quais regiões são editáveis da página de conteúdo e não são as partes, ele não resolver a ambiguidade do que são regiões não editáveis da página mestra aninhada e quais regiões são da página mestre de nível superior.


[![O conteúdo agora de página inclui controles de conteúdo para ContentPlaceHolders a página mestra aninhada](nested-master-pages-cs/_static/image17.png)](nested-master-pages-cs/_static/image16.png)

**Figura 06**: O conteúdo página agora inclui controles de conteúdo para ContentPlaceHolders a página mestra aninhada ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image18.png))


## <a name="step-3-adding-a-second-simple-nested-master-page"></a>Etapa 3: Adicionando uma segunda página de mestre aninhados simples

O benefício de páginas mestras aninhadas é mais evidente quando há várias páginas mestras aninhadas. Para ilustrar esse benefício, crie outra página mestra aninhada na `NestedMasterPages` pasta; Nomeie esse novo aninhado de página mestra `SimpleNestedAlternate.master` e associá-lo para o `Simple.master` página mestra. Adicione controles ContentPlaceHolder em dois controles de conteúdo da página mestra aninhada, como fizemos na etapa 2. Também adicione o texto, "Olá, de SimpleNestedAlternate!" no controle de conteúdo que corresponde da nível superior da página mestra `MainContent` ContentPlaceHolder. Depois de fazer essas alterações marcação declarativa do sua nova página mestra aninhada deve ser semelhante ao seguinte:


[!code-aspx[Main](nested-master-pages-cs/samples/sample6.aspx)]

Criar uma página de conteúdo denominada `Alternate.aspx` no `NestedMasterPages` pasta e associá-lo para o `SimpleNestedAlternate.master` página mestra aninhada. Adicione o texto, "Olá, da alternativa!" no controle de conteúdo que corresponde ao `MainContent`. A Figura 7 mostra `Alternate.aspx` quando visualizado no Designer do Visual Studio.


[![Alternate.aspx é vinculada à página mestra SimpleNestedAlternate.master](nested-master-pages-cs/_static/image20.png)](nested-master-pages-cs/_static/image19.png)

**Figura 07**: `Alternate.aspx` está associado a `SimpleNestedAlternate.master` página mestra ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image21.png))


Compare o Designer na Figura 7 para o Designer na Figura 6. Ambas as páginas de conteúdo compartilham o mesmo layout definido na página mestre de nível superior (`Simple.master`), ou seja, o título de "Aninhado mestre Tutorial das páginas (simples)". Ainda têm conteúdo distinto definido em suas páginas mestras do pai – o texto "Olá, de SimpleNested!" na Figura 6 e "Olá, de SimpleNestedAlternate!" Figura 7. Tudo bem, essas diferenças aqui são triviais, mas você pode estender este exemplo para incluir as diferenças mais significativas. Por exemplo, o `SimpleNested.master` página pode incluir um menu com opções específicas para suas páginas de conteúdo, enquanto `SimpleNestedAlternate.master` pode ter informações pertinentes para as páginas de conteúdo que se associam a ela.

Agora, imagine que precisávamos fazer uma alteração para o layout geral do site. Por exemplo, imagine que gostaríamos de adicionar uma lista de links comuns a todas as páginas de conteúdo. Para fazer isso, podemos atualizar a página mestre de nível superior, `Simple.master`. Lá, as alterações são refletidas imediatamente em suas páginas mestras aninhadas e, por extensão, suas páginas de conteúdo.

Para demonstrar a facilidade com a qual podemos alterar o layout geral do site, abra o `Simple.master` página mestra e adicione a seguinte marcação entre o `topContent` e `mainContent` `<div>` elementos:


[!code-aspx[Main](nested-master-pages-cs/samples/sample7.aspx)]

Isso adiciona dois links na parte superior de cada página que é associado a `Simple.master`, `SimpleNested.master`, ou `SimpleNestedAlternate.master`; essas alterações se aplicam a todas as páginas mestras e de suas páginas de conteúdo imediatamente. A Figura 8 mostra `Alternate.aspx` quando visualizado por meio de um navegador. Observe a adição dos links na parte superior da página (em comparação comparada a Figura 7).


[![Alterado para a página mestra de nível superior são refletidas imediatamente em suas páginas mestras aninhadas e suas páginas de conteúdo](nested-master-pages-cs/_static/image23.png)](nested-master-pages-cs/_static/image22.png)

**Figura 08**: alterado para a página mestra de nível superior são refletidas imediatamente em suas páginas mestras aninhadas e suas páginas de conteúdo ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image24.png))


## <a name="using-a-nested-master-page-for-the-administration-section"></a>Usando uma página mestra aninhada para a seção de administração

Agora que examinamos as vantagens do mestre aninhada de páginas e tenha visto o como criar e usá-los em um aplicativo ASP.NET. Os exemplos nas etapas 1, 2 e 3, no entanto, envolveu a criação de uma nova página mestre de nível superior, novas páginas mestras aninhadas e páginas de conteúdo novo. E quanto adicionar uma nova página mestra aninhada em um site com uma página mestre de nível superior existente e páginas de conteúdo?

Integrar uma página mestra aninhada em um site existente e associá-la a páginas de conteúdo existentes requer um pouco mais complexa que a partir do zero. As etapas 4, 5, 6 e 7 explorar esses desafios, como nós ampliamos nosso aplicativo de demonstração para incluir uma nova página mestra aninhada denominada `AdminNested.master` que contém instruções para o administrador e é usado pelas páginas do ASP.NET no `~/Admin` pasta.

Integrar uma página mestra aninhada em nosso aplicativo de demonstração apresenta os obstáculos a seguir:

- As páginas de conteúdo existente no `~/Admin` pasta têm certas expectativas de sua página mestra. Para os iniciantes, eles esperam certos controles ContentPlaceHolder esteja presente. Além disso, o `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` páginas chamar público da página mestra `RefreshRecentProductsGrid` método, defina seu `GridMessageText` propriedade, ou tem um manipulador de eventos para seu `PricesDoubled` eventos. Consequentemente, nossa página mestra aninhada deve fornecer o mesmo ContentPlaceHolders e membros públicos.
- No tutorial anterior aprimoramos o `BasePage` classe para definir dinamicamente o `Page` do objeto `MasterPageFile` propriedade com base em uma variável de sessão. Como a oferecemos suporte a páginas mestras dinâmicas ao usar páginas mestras aninhadas?

Esses dois desafios superfície serão como criamos a página mestra aninhada e usá-lo em nossas páginas de conteúdo existentes. Vamos investigar e superar esses problemas conforme eles surgem.

## <a name="step-4-creating-the-nested-master-page"></a>Etapa 4: Criando a página mestra aninhada

Nossa primeira tarefa é criar a página mestra aninhada a ser usado pelas páginas na seção Administração. Como vimos na etapa 2, quando adicionar um novo aninhados página mestra, precisamos especificar a página mestra do pai da página mestra aninhada. Mas temos duas páginas mestras de nível superior: `Site.master` e `Alternate.master`. Lembre-se que criamos `Alternate.master` no tutorial anterior e escreveu o código na `BasePage` classe que defina o objeto de página `MasterPageFile` propriedade em tempo de execução para um `Site.master` ou `Alternate.master` dependendo do valor da `MyMasterPage` Variável de sessão.

Como podemos configurar nosso página mestra aninhada para que ele use a página mestre de nível superior apropriada? Temos duas opções:

- Criar duas páginas mestras aninhadas `AdminNestedSite.master` e `AdminNestedAlternate.master`e associá-las para as páginas mestras de nível superior `Site.master` e `Alternate.master`, respectivamente. Na `BasePage`, em seguida, definiríamos o `Page` do objeto `MasterPageFile` à página mestra aninhada apropriada.
- Criar uma única página mestra aninhada e fazer com que as páginas de conteúdo use essa página mestra específica. Em seguida, em tempo de execução, seria preciso definir da página mestra aninhada `MasterPageFile` propriedade para a página mestre de nível superior apropriada em tempo de execução. (Como você talvez já descobriu agora, as páginas mestras também têm um `MasterPageFile` propriedade.)

Vamos usar a segunda opção. Criar um único aninhada mestre arquivo de paginação na `~/Admin` pasta chamada `AdminNested.master`. Porque ambos `Site.master` e `Alternate.master` têm o mesmo conjunto de controles ContentPlaceHolder, não importa qual página mestre você associá-lo, embora eu recomendo que você associe-o a `Site.master` para exemplificar entre a consistência.


[![Adicione uma página mestra aninhada para a pasta ~/Admin.](nested-master-pages-cs/_static/image26.png)](nested-master-pages-cs/_static/image25.png)

**Figura 09**: adicionar uma página mestra aninhada para o `~/Admin` pasta. ([Clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image27.png))


Como a página mestra aninhada é associada a uma página mestra com quatro controles ContentPlaceHolder, o Visual Studio adiciona quatro controles de conteúdo para marcação de inicial do novo arquivo de página mestra aninhada. Como fizemos nas etapas 2 e 3, adicione um controle ContentPlaceHolder em cada controle de conteúdo, dando a ele o mesmo nome que o controle de ContentPlaceHolder o nível superior da página mestra. Também adicione a seguinte marcação para o controle de conteúdo que corresponde ao `MainContent` ContentPlaceHolder:


[!code-html[Main](nested-master-pages-cs/samples/sample8.html)]

Em seguida, defina as `instructions` da classe CSS em de `Styles.css` e `AlternateStyles.css` arquivos CSS. As regras CSS a seguir fazem com que os elementos HTML com o estilo de `instructions` classe a ser exibido com uma cor de plano de fundo amarelo claro e uma borda preta sólida:


[!code-css[Main](nested-master-pages-cs/samples/sample9.css)]

Porque essa marcação foi adicionada à página mestra aninhada, ele será exibido apenas nessas páginas que usam essa página mestra aninhada (ou seja, as páginas na seção Administration).

Depois de fazer essas adições para sua página mestra aninhada, sua marcação declarativa deve ser semelhante ao seguinte:


[!code-aspx[Main](nested-master-pages-cs/samples/sample10.aspx)]

Observe que cada controle de conteúdo tem um controle ContentPlaceHolder e que os controles ContentPlaceHolder `ID` propriedades recebem os mesmos valores que os controles de ContentPlaceHolder correspondentes na página mestre de nível superior. Além disso, a marcação da seção específica de administração aparece no `MainContent` ContentPlaceHolder.

A Figura 10 mostra a `AdminNested.master` página mestra aninhada quando visualizado no Designer do Visual Studio. Você pode ver as instruções na caixa amarela na parte superior do `MainContent` controle de conteúdo.


[![A página mestra aninhada estende a página mestre de nível superior para incluir instruções para o administrador.](nested-master-pages-cs/_static/image29.png)](nested-master-pages-cs/_static/image28.png)

**Figura 10**: A página mestra aninhada estende a página mestre de nível superior para incluir instruções para o administrador. ([Clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image30.png))


## <a name="step-5-updating-the-existing-content-pages-to-use-the-new-nested-master-page"></a>Etapa 5: Atualizando as páginas de conteúdo existentes para usar a nova página mestra aninhada

A qualquer momento que podemos adicionar uma nova página de conteúdo para a seção de administração, é preciso associá-lo para o `AdminNested.master` página mestra que acabamos de criar. Mas e quanto existente páginas de conteúdo? Atualmente, todas as páginas de conteúdo no site derivam o `BasePage` classe, que define programaticamente o conteúdo da página mestra em tempo de execução. Isso não é o comportamento que queremos para as páginas de conteúdo na seção Administração. Em vez disso, queremos que essas páginas de conteúdo para usar sempre o `AdminNested.master` página. É responsabilidade da página mestra aninhada para escolher a página de conteúdo de nível superior à direita em tempo de execução.

A melhor maneira de conseguir isso desejado comportamento é criar uma nova classe de página de base personalizada denominada `AdminBasePage` que se estende a `BasePage` classe. `AdminBasePage` em seguida, pode substituir a `SetMasterPageFile` e defina o `Page` do objeto `MasterPageFile` ao valor embutido em código "~ / Admin/AdminNested.master". Dessa forma, qualquer página que deriva de `AdminBasePage` usará `AdminNested.master`, enquanto que qualquer página que é derivado de `BasePage` terá seu `MasterPageFile` propriedade definido dinamicamente como "~ / Master" ou "~ / Alternate.master" com base no valor da `MyMasterPage` Variável de sessão.

Comece adicionando um novo arquivo de classe para o `App_Code` pasta chamada `AdminBasePage.cs`. Ter `AdminBasePage` estender `BasePage` e, em seguida, substituir o `SetMasterPageFile` método. Nesse método, atribua o `MasterPageFile` o valor "~ / Admin/AdminNested.master". Depois de fazer essas alterações em sua classe de arquivo deve ser semelhante ao seguinte:


[!code-csharp[Main](nested-master-pages-cs/samples/sample11.cs)]

Agora, é necessário ter as páginas de conteúdo existentes na administração seção derivam `AdminBasePage` em vez de `BasePage`. Vá para o arquivo de classe code-behind para cada página de conteúdo no `~/Admin` pasta e fazer essa alteração. Por exemplo, no `~/Admin/Default.aspx` você alteraria a declaração de classe de lógica de:


[!code-csharp[Main](nested-master-pages-cs/samples/sample12.cs)]

Para:


[!code-csharp[Main](nested-master-pages-cs/samples/sample13.cs)]

Figura 11 ilustra como a página mestre de nível superior (`Site.master` ou `Alternate.master`), a página mestra aninhada (`AdminNested.master`), e as páginas de conteúdo seção Administração se relacionam entre si.


[![A página mestra aninhada define o conteúdo específico para as páginas na seção Administração](nested-master-pages-cs/_static/image32.png)](nested-master-pages-cs/_static/image31.png)

**Figura 11**: O aninhados mestre página define conteúdo específicos para as páginas na seção Administration ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image33.png))


## <a name="step-6-mirroring-the-master-pages-public-methods-and-properties"></a>Etapa 6: Espelhamento de propriedades e métodos públicos da página mestra

Lembre-se de que o `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` páginas interagem programaticamente com a página mestra: `~/Admin/AddProduct.aspx` chamadas a página mestra do pública `RefreshRecentProductsGrid` método e define seu `GridMessageText` propriedade; `~/Admin/Products.aspx` tem um manipulador de eventos para o `PricesDoubled` eventos. No tutorial anterior, criamos um resumo `BaseMasterPage` classe que definiu esses membros públicos.

O `~/Admin/AddProduct.aspx` e `~/Admin/Products.aspx` páginas supõem que sua página mestra deriva o `BaseMasterPage` classe. O `AdminNested.master` página, no entanto, atualmente, estende o `System.Web.UI.MasterPage` classe. Como resultado, ao visitar `~/Admin/Products.aspx` uma `InvalidCastException` será lançada com a mensagem: "não é possível converter o objeto do tipo ' ASP.admin\_adminnested\_mestre ' para o tipo 'BaseMasterPage'."

Para corrigir isso, precisamos ter o `AdminNested.master` estender classe code-behind `BaseMasterPage`. Atualize a declaração de classe da página mestra aninhada de lógica de:


[!code-csharp[Main](nested-master-pages-cs/samples/sample14.cs)]

Para:


[!code-csharp[Main](nested-master-pages-cs/samples/sample15.cs)]

Nós não ainda terminou. Porque o `BaseMasterPage` classe é abstrata, é preciso substituir o `abstract` membros `RefreshRecentProductsGrid` e `GridMessageText`. Esses membros são usados pelas páginas mestras nível superior para atualizar suas interfaces de usuário. (Na verdade, somente o `Site.master` página mestre usa esses métodos, embora ambas as páginas mestras de nível superior para implementar esses métodos, já que ambos estendem `BaseMasterPage`.)

Embora, precisamos implementar esses membros de `AdminNested.master`, tudo o que essas implementações precisam fazer é simplesmente chamar o mesmo membro no nível superior página mestra usada pela página mestra aninhada. Por exemplo, quando uma página de conteúdo na seção Administração chama da página mestra aninhada `RefreshRecentProductsGrid` método, a página mestra aninhada precisa fazer é, por sua vez, chamar `Site.master` ou `Alternate.master`do `RefreshRecentProductsGrid` método.

Para fazer isso, comece adicionando o seguinte `@MasterType` diretiva na parte superior do `AdminNested.master`:


[!code-aspx[Main](nested-master-pages-cs/samples/sample16.aspx)]

Lembre-se de que o `@MasterType` diretiva adiciona uma propriedade fortemente tipada para a classe code-behind chamado `Master`. Em seguida, substituir os `RefreshRecentProductsGrid` e `GridMessageText` membros e simplesmente delegar a chamada para o `Master`correspondente do método:


[!code-csharp[Main](nested-master-pages-cs/samples/sample17.cs)]

Com esse código em vigor, você poderá visitar e usar as páginas de conteúdo na seção Administração. A Figura 12 mostra o `~/Admin/Products.aspx` página quando visualizado por meio de um navegador. Como você pode ver, a página inclui a caixa de instruções de administração, que é definida na página mestra aninhada.


[![As páginas de conteúdo na seção Administração incluem instruções na parte superior de cada página](nested-master-pages-cs/_static/image35.png)](nested-master-pages-cs/_static/image34.png)

**Figura 12**: as páginas de conteúdo a seção Administração incluem instruções na parte superior de cada página ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image36.png))


## <a name="step-7-using-the-appropriate-top-level-master-page-at-runtime"></a>Etapa 7: Usando a página de mestre de nível superior apropriado em tempo de execução

Enquanto todas as páginas de conteúdo na seção Administração estão totalmente funcionais, eles usam a mesma página mestra de nível superior e ignorar a página mestra selecionada pelo usuário em `ChooseMasterPage.aspx`. Esse comportamento é devido ao fato de que a página mestra aninhada tem seu `MasterPageFile` propriedade estaticamente definida como `Site.master` em seu `<%@ Master %>` diretiva.

Para usar a página mestre de nível superior selecionada pelo usuário final, precisamos definir a `AdminNested.master`do `MasterPageFile` propriedade para o valor no `MyMasterPage` variável de sessão. Porque definimos as conteúdo das páginas `MasterPageFile` propriedades na `BasePage`, você pode achar que nós a definiríamos da página mestra aninhada `MasterPageFile` propriedade na `BaseMasterPage` ou no `AdminNested.master`da classe code-behind. Isso não funcionar, no entanto, porque é preciso ter definido a `MasterPageFile` propriedade até o final do estágio PreInit. O horário mais cedo que podemos programaticamente explorar o ciclo de vida da página de uma página mestre é o estágio de inicialização (o que ocorre após o estágio de PreInit).

Portanto, precisamos definir da página mestra aninhada `MasterPageFile` propriedade de páginas de conteúdo. O único conteúdo de páginas que usam o `AdminNested.master` derivam de página mestra `AdminBasePage`. Portanto, podemos colocar essa lógica existe. Na etapa 5 ou seja, substituímos o `SetMasterPageFile` método, configurando a `Page` do objeto `MasterPageFile` propriedade como "~ / Admin/AdminNested.master". Atualização `SetMasterPageFile` para definir também a página mestra `MasterPageFile` propriedade para o resultado armazenado na sessão:


[!code-csharp[Main](nested-master-pages-cs/samples/sample18.cs)]

O `GetMasterPageFileFromSession` método, que adicionamos ao `BasePage` classe no tutorial anterior, retorna o caminho do arquivo de página mestra apropriado com base no valor da variável de sessão.

Com essa alteração em vigor, a seleção do usuário página mestra é transportado para a seção de administração. Figura 13 mostra a mesma página como a Figura 12, mas depois que o usuário alterou sua seleção de página mestra para `Alternate.master`.


[![A página de administração aninhada usa a página mestre de nível superior selecionado pelo usuário](nested-master-pages-cs/_static/image38.png)](nested-master-pages-cs/_static/image37.png)

**Figura 13**: A página de administração Nested usa a página mestre de nível superior selecionado pelo usuário ([clique para exibir a imagem em tamanho normal](nested-master-pages-cs/_static/image39.png))


## <a name="summary"></a>Resumo

Muito páginas de conteúdo como like podem associar a uma página mestra, é possível criar aninhado páginas mestras, fazendo com que uma página mestra filha associar a uma página mestra pai. A página mestra filha pode definir controles de conteúdo para cada um dos ContentPlaceHolders do pai; ele pode adicionar seus próprios controles ContentPlaceHolder (bem como outra marcação) a esses controles de conteúdo. Páginas mestras aninhadas são bastante útil em aplicativos web grandes, onde todas as páginas compartilhem uma aparência abrangente, mas determinadas seções do site exigem personalizações exclusivas.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Páginas mestras do ASP.NET aninhadas](https://msdn.microsoft.com/library/x2b3ktt7.aspx)
- [Dicas para páginas mestras aninhadas e tempo de Design do VS 2005](https://weblogs.asp.net/scottgu/archive/2005/11/11/430382.aspx)
- [VS 2008 aninhados suporte a página mestra](https://weblogs.asp.net/scottgu/archive/2007/07/09/vs-2008-nested-master-page-support.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 3.5 in 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, escreva-me em [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](specifying-the-master-page-programmatically-cs.md)
> [Próximo](creating-a-site-wide-layout-using-master-pages-vb.md)
