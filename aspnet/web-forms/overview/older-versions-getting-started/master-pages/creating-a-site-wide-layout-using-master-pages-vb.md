---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: Criação de um Layout de todo o Site usando páginas mestras (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial mostrará as Noções básicas de página mestra. Ou seja, quais são as páginas mestras, como faz um criar uma página mestra, o que são o conteúdo de espaços reservados, como faz uma cr...
ms.author: aspnetcontent
ms.date: 05/21/2008
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 182f45c28dc37633b429fead333d401818299e36
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827816"
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Criação de um Layout de todo o Site usando páginas mestras (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> Este tutorial mostrará as Noções básicas de página mestra. Ou seja, quais são as páginas mestras, como é criar uma página mestra, o que são o conteúdo de espaços reservados, como um cria uma página ASP.NET que usa uma página mestra, como modificar a página mestra é refletida automaticamente em suas páginas de conteúdo associadas e assim por diante.


## <a name="introduction"></a>Introdução

Um atributo de um site bem projetado é um layout de página consistente em todo o site. Veja o site www.asp.net, por exemplo. No momento da redação deste artigo, cada página tem o mesmo conteúdo na parte superior e inferior da página. Como mostra a Figura 1, a parte superior de cada página exibe uma barra cinza com uma lista de Communities da Microsoft. Abaixo do que é o logotipo do site, a lista de idiomas para a qual o site foi traduzido e as seções principais: Home, introdução, saiba mais, Downloads e assim por diante. Da mesma forma, a parte inferior da página inclui informações sobre anúncios em www.asp.net, uma declaração de direitos autorais e um link para a declaração de privacidade.


[![O site www.asp.net emprega uma aparência consistente em todas as páginas](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>Figura 01</strong>: O site www.asp.net emprega uma aparência consistente e sinta-se em todas as páginas ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


Outro atributo de um site bem projetado é a facilidade com que a aparência do site pode ser alterada. A Figura 1 mostra a home page de www.asp.net a partir de março de 2008, mas entre agora e publicação deste tutorial, a aparência pode ter sido alterado. Talvez os itens de menu na parte superior se expandirá para incluir uma nova seção para a estrutura do MVC. Ou talvez um radicalmente novo design com diferentes cores, fontes e layout será unveiled. Aplicar essas alterações para o site inteiro deve ser um processo rápido e simple que não requer modificação de milhares de páginas da web que compõem o site.

Criando um modelo de página de todo o site no ASP.NET é possível por meio do uso do *páginas mestras*. Em poucas palavras, uma página mestra é um tipo especial de página ASP.NET que define a marcação que é comum entre todos os *páginas de conteúdo* , bem como as regiões são personalizáveis em uma base de página de conteúdo a página de conteúdo. (Uma página de conteúdo é uma página ASP.NET que está associada à página mestra.) Sempre que uma página mestra layout ou formatação é alterada, toda saída de seu conteúdo das páginas é da mesma forma imediatamente atualizado, que facilita a aplicação de alterações da aparência de todo o site tão fácil quanto a atualização e implantação de um único arquivo (ou seja, a página mestra).

Este é o primeiro tutorial em uma série de tutoriais que exploram o uso de páginas mestras. No decorrer desta série de tutoriais é:

- Examinar a criação de páginas mestras e suas páginas de conteúdo associadas,
- Discutir a uma variedade de dicas, truques e armadilhas,
- Identificar as armadilhas comuns da página mestra e explore as soluções alternativas,
- Saber como acessar a página mestra de uma página de conteúdo e vice-versa um
- Saiba como especificar um conteúdo da página mestra em tempo de execução, e
- Outros avançados tópicos da página mestra.

Esses tutoriais se destinam a ser concisas e fornecem instruções passo a passo com capturas de tela suficiente para orientá-lo pelo processo visualmente. Cada tutorial está disponível nas versões do Visual Basic e do c# e inclui um download do código completo usado.

Este tutorial inaugural começa com uma análise de Noções básicas sobre a página mestra. Discuta como páginas mestras funcionam, examinar a criação de uma página mestra e páginas de conteúdo associadas usando o Visual Web Developer e ver como as alterações a uma página mestra são refletidas imediatamente em suas páginas de conteúdo. Vamos começar!

## <a name="understanding-how-master-pages-work"></a>Noções básicas sobre como páginas mestras funcionam

Compilando um site com um layout de página consistente em todo o site requer que cada página da web emitir marcações de formatação comuns, além de seu conteúdo personalizado. Por exemplo, embora cada postagem tutorial ou fórum em www.asp.net tenha seu próprio conteúdo exclusivo, cada uma dessas páginas também processam uma série de comum `<div>` elementos que exibem os links da seção de nível superior: Home, começar, saiba mais e assim por diante.

Há uma variedade de técnicas para criação de páginas da web com uma aparência consistente. Uma abordagem simples é simplesmente copiar e colar a marcação de layout comum em todas as páginas da web, mas essa abordagem tem um número de desvantagens. Para os iniciantes, sempre que uma nova página é criada, você deve se lembrar copiar e colar o conteúdo compartilhado na página. Tais copiando e colando as operações estão propícios para o erro conforme você acidentalmente pode copiar somente um subconjunto da marcação compartilhada em uma nova página. E, para finalizar bem, essa abordagem torna a substituir a aparência de todo o site existente com um novo um real problemáticos porque cada página individual no site deve ser editada para usar a nova aparência.

Antes do ASP.NET versão 2.0, os desenvolvedores geralmente são colocados marcação comum em página [controles de usuário](https://msdn.microsoft.com/library/y6wb1a0e.aspx) e, em seguida, adicionados a esses controles de usuário para cada página. Essa abordagem é necessário que o desenvolvedor de página não se esqueça de adicionar manualmente os controles de usuário a cada nova página, mas permitido para modificações de todo o site mais fácil, porque ao atualizar a marcação comum somente os controles de usuário precisava ser modificado. Infelizmente, o Visual Studio .NET 2002 e 2003 – as versões do Visual Studio usado para criar aplicativos do ASP.NET 1. x - renderizados controles de usuário no modo de exibição de Design, como caixas em cinza. Consequentemente, os desenvolvedores de páginas usando essa abordagem não aproveitou um ambiente de tempo de design WYSIWYG.

As limitações do uso de controles de usuário foram resolvidas no ASP.NET versão 2.0 e o Visual Studio 2005 com a introdução da *páginas mestras*. Uma página mestra é um tipo especial de página ASP.NET que define a marcação em todo o site e o *regiões* onde associado *páginas de conteúdo* definir sua marcação personalizada. Como veremos na etapa 1, essas regiões são definidas por controles ContentPlaceHolder. O controle ContentPlaceHolder simplesmente indica uma posição na hierarquia de controle da página mestra onde o conteúdo personalizado pode ser injetado por uma página de conteúdo.

> [!NOTE]
> Os principais conceitos e a funcionalidade das páginas mestras não foi alterado desde o ASP.NET versão 2.0. No entanto, o Visual Studio 2008 oferece suporte de tempo de design para páginas mestras aninhadas, um recurso que não existia no Visual Studio 2005. Vamos examinar usando páginas mestras aninhadas em um tutorial futuro.


Figura 2 mostra a página mestra para www.asp.net aparência. Observe que a página mestra define o layout de todo o site - a marcação na parte superior, inferior e à direita de cada página - comuns, bem como um ContentPlaceHolder na parte intermediária esquerda, onde se encontra o conteúdo exclusivo para cada página da web.


![Uma página mestra define o Layout de todo o Site e as regiões editáveis em uma base de página de conteúdo a página conteúdo](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Figura 02**: uma página mestra define o Layout de todo o Site e as regiões editáveis em uma base de página de conteúdo a página conteúdo


Depois que uma página mestra foi definida, ele pode ser ligado a novas páginas do ASP.NET por meio de escala de uma caixa de seleção. Essas páginas ASP.NET - chamadas páginas de conteúdo - incluem um controle de conteúdo para cada um dos controles de ContentPlaceHolder da página mestra. Quando a página de conteúdo é visitada por meio de um navegador o mecanismo do ASP.NET cria a hierarquia de controle da página mestra e injeta a hierarquia de controle da página de conteúdo em locais apropriados. Essa hierarquia de controle combinado é renderizada e o HTML resultante é retornado ao navegador do usuário final. Consequentemente, a página de conteúdo emite a marcação comuns definidos na sua página mestre fora controles ContentPlaceHolder e a marcação de específico da página definida dentro de seu próprios controles de conteúdo. Figura 3 ilustra esse conceito.


[![Marcação da página solicitada tem fusível para a página mestra](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Figura 03**: marcação da página solicitado o é fundida na página mestra ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


Agora que já discutimos como páginas mestras funcionam, vamos dar uma olhada na criação de uma página mestra e páginas de conteúdo associadas usando o Visual Web Developer.

> [!NOTE]
> Para acessar o maior público possível, o site do ASP.NET que criamos em toda esta série de tutoriais será criado usando ASP.NET 3.5 com a versão gratuita da Microsoft do Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Se você ainda não foram atualizados para o ASP.NET 3.5, não se preocupe – conceitos discutidos esses tutoriais de trabalho igualmente bem com o ASP.NET 2.0 e o Visual Studio 2005. No entanto, alguns aplicativos de demonstração podem usar recursos que são novos para o .NET Framework versão 3.5; Quando são usados recursos específicos de 3.5, posso incluir uma observação que discute como implementar uma funcionalidade semelhante na versão 2.0. Tenha em mente que os aplicativos de demonstração disponíveis para baixar cada destino do tutorial de .NET Framework versão 3.5, que resulta em um `Web.config` arquivo que inclui elementos de configuração de 3.5 específicos. Encurtar a história, se você ainda precisa instalar o .NET 3.5 em seu computador, em seguida, o aplicativo web que pode ser baixado não funcionará sem primeiro remover a marcação específica do 3.5 de `Web.config`. Ver [do dissecando ASP.NET versão 3.5 `Web.config` arquivo](http://www.4guysfromrolla.com/articles/121207-1.aspx) para obter mais informações sobre esse tópico.


## <a name="step-1-creating-a-master-page"></a>Etapa 1: Criando uma página mestra

Antes que podemos explorar criando e usando páginas mestras e de conteúdo, primeiro precisamos de um site ASP.NET. Comece criando um novo arquivo com base no sistema site da Web ASP.NET. Para fazer isso, inicie o Visual Web Developer e, em seguida, vá para o menu Arquivo e escolha New Web Site, exibindo a caixa de diálogo New Web Site caixa (veja a Figura 4). Escolha o modelo de Site da Web ASP.NET, defina a lista suspensa de local para o sistema de arquivos, escolha uma pasta para colocar o site da web e definir o idioma para o Visual Basic. Isso criará um novo site com uma `Default.aspx` página do ASP.NET, um `App_Data` pasta e um `Web.config` arquivo.

> [!NOTE]
> Visual Studio dá suporte a dois modos de gerenciamento de projeto: os projetos de Site da Web e projetos de aplicativos Web. Projetos de Site não têm um arquivo de projeto, enquanto que o Web Application Projects imitar a arquitetura do projeto no Visual Studio .NET 2002/2003 – eles incluem um arquivo de projeto e compilar o código-fonte do projeto em um único assembly, que é colocado no `/bin` pasta. O Visual Studio 2005 inicialmente apenas sites da Web com suporte de projetos, embora o modelo de projeto de aplicativo Web foi reintroduzido com Service Pack 1. O Visual Studio 2008 oferece os dois modelos de projeto. Visual Web Developer 2005 e edições de 2008, no entanto, somente dão suporte a projetos de Site da Web. Posso usar o modelo de projeto de Site para minhas demonstrações nessa série de tutoriais. Se você estiver usando uma edição não Express e deseja usar o [modelo de projeto de aplicativo Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) em vez disso, fique à vontade para fazer isso, mas lembre-se de que pode haver algumas discrepâncias entre o que você vê na tela e as etapas que você deve tomar em comparação com o capturas de tela mostradas e instruções fornecidas nestes tutoriais.


[![Criar um novo arquivo com base no sistema Web Site](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Figura 04**: criar um Site New File System-Based ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


Em seguida, adicione uma página mestra para o site no diretório raiz clicando duas vezes no nome do projeto, escolhendo Add New Item e selecionando o modelo de página mestra. Observe que as páginas mestras terminam com a extensão `.master`. Nomeie essa nova página mestra `Site.master` e clique em Adicionar.


[![Adicionar uma página mestra chamado site a site](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Figura 05**: adicionar um nome de página mestra `Site.master` para o site ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


Adicionando um novo arquivo de página mestra por meio do Visual Web Developer cria uma página mestra com a seguinte marcação declarativa:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

A primeira linha na marcação declarativa é a [ `@Master` diretiva](https://msdn.microsoft.com/library/ms228176.aspx). O `@Master` diretiva é semelhante de [ `@Page` diretiva](https://msdn.microsoft.com/library/ydy4x04a.aspx) que aparece nas páginas do ASP.NET. Ele define o idioma do lado do servidor (VB) e informações sobre o local e a herança de classe de code-behind da página mestra.

O `DOCTYPE` e a marcação da página declarativa aparece sob o `@Master` diretiva. A página inclui HTML estático, juntamente com quatro controles de servidor:

- **Um formulário da Web (o `<form runat="server">`)** – porque todas as páginas ASP.NET normalmente têm um formulário da Web - e como a página mestra pode incluir controles da Web que devem aparecer dentro de um formulário da Web - Certifique-se de adicionar o formulário da Web para a página mestra (em vez de adicionar um formulário da Web a e ACH página de conteúdo).
- **Um controle de ContentPlaceHolder denominado `ContentPlaceHolder1`**  -esse controle ContentPlaceHolder aparece dentro do formulário da Web e serve como a região para a interface do usuário da página de conteúdo.
- **Um servidor `<head>` elemento** - o `<head>` elemento tem o `runat="server"` atributo, tornando-o acessível por meio de código do lado do servidor. O `<head>` elemento é implementado dessa forma, para que o título da página e outros `<head>`-relacionados a marcação pode ser adicionada ou ajustada por meio de programação. Por exemplo, a definição de uma página do ASP.NET `Title` alterações de propriedade de `<title>` elemento renderizado pelo `<head>` controle de servidor.
- **Um controle de ContentPlaceHolder chamado `head`**  -esse controle ContentPlaceHolder aparece dentro a `<head>` servidor de controle e pode ser usado declarativamente adicionar conteúdo ao `<head>` elemento.

Essa marcação declarativa de página mestra padrão serve como um ponto de partida para criar suas próprias páginas mestras. Fique à vontade para editar o HTML ou para adicionar controles de Web adicionais ou ContentPlaceHolders para a página mestra.

> [!NOTE]
> Ao projetar uma página mestra Verifique se a página mestra contém um formulário da Web e pelo menos um controle ContentPlaceHolder aparece dentro desse formulário da Web.


### <a name="creating-a-simple-site-layout"></a>Criação de um Layout de Site simples

Vamos expandir `Site.master`da marcação declarativa de padrão para criar um layout de site em que todas as páginas compartilhem: um cabeçalho comum; uma coluna à esquerda com a navegação, notícias e outros tipos de conteúdo em todo o site; e um rodapé que exibe o ícone de "Alimentado pelo Microsoft ASP.NET". Figura 6 mostra o resultado final da página mestra, quando uma das suas páginas de conteúdo é exibida por meio de um navegador. A região dentro de um círculo vermelha na Figura 6 é específica para a página que está sendo visitada (`Default.aspx`); o outro conteúdo é definida na página mestre e, portanto, consistente em todas as páginas de conteúdo.


[![A página mestra define a marcação para a parte superior, esquerda e inferior partes](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Figura 06**: A página mestra define a marcação para a parte superior, esquerda e inferior partes ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))


Para obter o layout de site mostrado na Figura 6, comece atualizando o `Site.master` página mestra para que ele contenha a seguinte marcação declarativa:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

Layout da página mestra é definido usando uma série de `<div>` elementos HTML. O `topContent` `<div>` contém a marcação que aparece na parte superior de cada página, enquanto o `mainContent`, `leftContent`, e `footerContent` `<div>` s são usados para exibir o conteúdo da página, a coluna à esquerda e o "fornecida pela Microsoft Ícone do ASP.NET", respectivamente. Além de adicionar essas `<div>` elementos, também renomeei a `ID` propriedade do controle ContentPlaceHolder primário do `ContentPlaceHolder1` para `MainContent`.

As regras de formatação e layout para esses sortidas `<div>` elementos é descrito de forma a [folha de estilos em cascata (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) arquivo `Styles.css`, que é especificado por meio de um `<link>` elemento da página mestra `<head>`elemento. Essas várias regras definem a aparência de cada `<div>` elemento indicado acima. Por exemplo, o `topContent` `<div>` elemento, que exibe o texto "Tutoriais de páginas mestras" e o link, tem suas regras de formatação especificadas no `Styles.css` da seguinte maneira:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Se você estiver acompanhando em seu computador, você precisará baixar o código que acompanha este artigo deste tutorial e adicionar o `Styles.css` ao seu projeto. Da mesma forma, você também precisará criar uma pasta chamada `Images` e copiar o ícone "Alimentado pelo Microsoft ASP.NET" no site de demonstração baixado ao seu projeto.

> [!NOTE]
> Uma discussão sobre CSS e formatação de página da web está além do escopo deste artigo. Para obter mais informações sobre CSS, confira a [CSS tutoriais](http://www.w3schools.com/css/default.asp) na [W3Schools.com](http://www.w3schools.com/). Também recomendo que você baixe o código que acompanha este artigo deste tutorial e brincar com as configurações de CSS no `Styles.css` para ver os efeitos das regras de formatação diferentes.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Criando uma página mestra usando um modelo de projeto existente

Ao longo dos anos, eu criei um número de aplicativos web ASP.NET para empresas de pequeno a médio porte. Alguns dos meus clientes tinham um layout de site existente que quisessem usar; outras pessoas contrataram um designer de gráficos competente. Alguns confiados-me para criar o layout do site. Como você pode ver pela Figura 6, um programador para projetar o layout de um site de execução de tarefas normalmente, é tão inteligente como tendo seu contador executar open-heart surgery enquanto o médico tem seus impostos.

Felizmente, existem sites innumerous que oferece modelos de design HTML gratuitos - Google retornou mais de seis milhões de resultados para o termo de pesquisa "modelos de site gratuito". Um dos Meus Favoritos é [OpenDesigns.org](http://opendesigns.org/). Depois de encontrar um modelo de site que você deseja, adicione os arquivos CSS e imagens ao seu projeto de site e integrar o HTML do modelo em sua página mestra.

> [!NOTE]
> A Microsoft também oferece um número de [Liberar modelos de Kit de início do ASP.NET Design](https://msdn.microsoft.com/asp.net/aa336613.aspx) que se integram na caixa de diálogo Novo Site da Web no Visual Studio.


## <a name="step-2-creating-associated-content-pages"></a>Etapa 2: Criando páginas de conteúdo associadas

Com a página mestra foi criada, estamos prontos para começar a criar páginas ASP.NET que estão associadas à página mestra. Essas páginas são denominadas *páginas de conteúdo*.

Vamos adicionar uma nova página ASP.NET ao projeto e associá-lo para o `Site.master` página mestra. Clique com botão direito no nome do projeto no Gerenciador de soluções e escolha a opção de adicionar novo Item. Selecione o modelo de formulário da Web, digite o nome `About.aspx`e, em seguida, marque a caixa de seleção "Selecionar página mestra" conforme mostrado na Figura 7. Isso exibirá o selecione uma caixa de diálogo de página mestra caixa (veja a Figura 8) de onde você pode escolher a página mestra para usar.

> [!NOTE]
> Se você tiver criado seu site da Web ASP.NET usando o modelo de projeto de aplicativo Web em vez do modelo de projeto de Site, você não verá a caixa de seleção "Selecionar a página mestra" na caixa de diálogo Add New Item, mostrada na Figura 7. Para criar um conteúdo de página quando o projeto de aplicativo Web usando o modelo que você deve escolher o modelo de formulário de conteúdo da Web em vez do modelo de formulário da Web. Depois de selecionar o modelo de formulário de conteúdo da Web e clicando em Adicionar, o mesmo selecionar uma página mestra que será exibida a caixa de diálogo mostrada na Figura 8.


[![Adicione uma nova página de conteúdo](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Figura 07**: Adicione uma nova página de conteúdo ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![Selecione a página mestra do site](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Figura 08**: selecione o `Site.master` página mestra ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


Como mostra a seguinte marcação declarativa, uma página de conteúdo contém um `@Page` diretiva que aponta de volta para seu mestre página e um controle de conteúdo para cada um dos controles de ContentPlaceHolder da página mestra.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> Na seção "Criando um Layout de Site simples" na etapa 1, renomeei `ContentPlaceHolder1` para `MainContent`. Se você não renomear este controle de ContentPlaceHolder `ID` da mesma forma, marcação declarativa da página de conteúdo serão um pouco diferentes da marcação mostrada acima. Do ou seja, o segundo controle de conteúdo `ContentPlaceHolderID` refletirá a `ID` do ContentPlaceHolder correspondente de controle na sua página mestra.


Durante a renderização de uma página de conteúdo, o mecanismo do ASP.NET deve fuse da página de conteúdo controles com controles de ContentPlaceHolder da sua página mestra. O mecanismo do ASP.NET determina o conteúdo da página mestra do `@Page` da diretiva `MasterPageFile` atributo. Como mostra a marcação acima, esta página de conteúdo está associada a `~/Site.master`.

Como a página mestra tem dois controles de ContentPlaceHolder - `head` e `MainContent` -Visual Web Developer gerados dois controles de conteúdo. Cada controle de conteúdo faz referência a um determinado ContentPlaceHolder por meio de seu `ContentPlaceHolderID` propriedade.

Onde as páginas mestras brilham sobre técnicas de todo o site modelo anterior é com o suporte de tempo de design. A Figura 9 mostra o `About.aspx` página de conteúdo quando visualizado no modo de Design do Visual Web Developer. Observe que enquanto o conteúdo da página mestra estiver visível, ele fica esmaecido e não pode ser modificado. Os controles de conteúdo correspondente ContentPlaceHolders da página mestra são, no entanto, editáveis. E, assim como com qualquer outra página do ASP.NET, você pode criar interface da página de conteúdo com a adição de controles da Web através das exibições de origem ou de Design.


[![Modo de Design da página de conteúdo exibe a página mestre e de específico da página conteúdo](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Figura 09**: A página conteúdo Design View exibe tanto o específico da página e conteúdo da página mestra ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Adicionando marcação e controles da Web para a página de conteúdo

Reserve um tempo para criar algum conteúdo para o `About.aspx` página. Como você pode ver na Figura 10, que eu inseri um título de "Sobre o autor" e alguns parágrafos de texto, mas fique à vontade para adicionar controles de Web, também. Depois de criar essa interface, visite o `About.aspx` página por meio de um navegador.


[![Visite a página About através de um navegador](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Figura 10**: visite a `About.aspx` página por meio de um navegador ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


É importante entender que a página de conteúdo solicitada e a página mestra associada são combinados e renderizados como um todo inteiramente no servidor web. Navegador do usuário final, em seguida, é enviado para o HTML resultante, adição múltipla. Para verificar isso, exiba o HTML recebido pelo navegador indo até o menu Exibir e escolhendo o código-fonte. Observe que há nenhum quadro ou outras técnicas especializadas para exibir duas páginas da web diferentes em uma única janela.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Associando uma página mestra a uma página ASP.NET

Como vimos nesta etapa, a adição de que uma página de conteúdo para um aplicativo web ASP.NET é tão fácil quanto marcar a caixa de seleção "Selecionar a página mestra" e a página mestra de separação. Infelizmente, não é tão fácil converter uma página ASP.NET existente em uma página mestra.

Para associar uma página mestra a uma página ASP.NET, você precisa executar as seguintes etapas:

1. Adicione a `MasterPageFile` de atributo para a página ASP.NET `@Page` diretiva, apontá-lo para a página mestra apropriada.
2. Adicione controles de conteúdo para cada um do ContentPlaceHolders na página mestra.
3. Recortar e colar o conteúdo da página ASP.NET existente em controles de conteúdo apropriados seletivamente. Digo "seletivamente" aqui porque o ASP.NET página provavelmente contém marcação que já é expressa pela página mestre, como o `DOCTYPE`, o `<html>` elemento e o formulário da Web.

Para obter instruções passo a passo sobre esse processo, junto com capturas de tela, fazer check-out [Scott Guthrie](https://weblogs.asp.net/scottgu/)do [usando as páginas mestras e navegação no Site](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) tutorial. A "atualização `Default.aspx` e `DataSample.aspx` para usar a página mestra" seção fornece detalhes sobre essas etapas.

Como é muito mais fácil criar novas páginas de conteúdo que é converter as páginas ASP.NET existentes em páginas de conteúdo, recomendo que, sempre que você cria um novo site do ASP.NET adicionar uma página mestra ao site. Associe todas as páginas ASP.NET novas a essa página mestra. Não se preocupe se a página mestra inicial é muito simples ou simples; Você pode atualizar a página mestra mais tarde.

> [!NOTE]
> Ao criar um novo aplicativo ASP.NET, Visual Web Developer adiciona um `Default.aspx` página que não está associada a uma página mestra. Se você quiser praticar a conversão de uma página ASP.NET existente em uma página de conteúdo, vá em frente e fazê-lo com `Default.aspx`. Como alternativa, você pode excluir `Default.aspx` e, em seguida, adicione novamente, mas desta vez, marcando a caixa de seleção "Selecione página mestra".


## <a name="step-3-updating-the-master-pages-markup"></a>Etapa 3: Atualizar a marcação da página mestra

Um dos principais benefícios das páginas mestras é que uma única página mestra pode ser usada para definir o layout geral para várias páginas no site. Portanto, atualizar e a aparência do site exige a atualização de um único arquivo - a página mestra.

Para ilustrar esse comportamento, vamos atualizar nossa página mestre para incluir a data atual na parte superior da coluna à esquerda. Adicionar um rótulo denominado `DateDisplay` para o `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

Em seguida, crie um `Page_Load` manipulador de eventos para o mestre de página e adicione o seguinte código:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

O código acima define o rótulo `Text` formatada de propriedade para a data e hora atuais como o dia da semana, o nome do mês e dia de dois dígitos (veja a Figura 11). Com essa alteração, examine uma das suas páginas de conteúdo. Como mostra a Figura 11, a marcação resultante é imediatamente atualizada para incluir a alteração para a página mestra.


[![As alterações para a página mestra são refletidas quando exibindo a página de conteúdo](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Figura 11**: as alterações para a página mestra são refletidas quando exibindo a página de conteúdo ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> Como este exemplo ilustra, as páginas mestras podem conter controles do lado do servidor da Web, código e manipuladores de eventos.


## <a name="summary"></a>Resumo

Páginas mestras permitem que desenvolvedores ASP.NET criar um layout consistente em todo o site que é facilmente atualizável. Criar páginas mestras e suas páginas de conteúdo associadas é tão simple quanto criar páginas ASP.NET padrão, como o Visual Web Developer oferece suporte avançado do tempo de design.

O exemplo de página mestra criados neste tutorial tinha dois controles ContentPlaceHolder, head e MainContent. Especificamos apenas a marcação do controle MainContent ContentPlaceHolder na nossa página de conteúdo, no entanto. No próximo tutorial, vamos examinar usando o conteúdo de vários controles na página de conteúdo. Também vemos como definir padrão marcação para conteúdo controla dentro da página mestra, bem como para alternar entre usar o padrão marcação definido no mestre de página e fornecendo uma marcação personalizada da página de conteúdo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [ASP.NET para Designers: liberar modelos de Design e orientações sobre a criação de sites ASP.NET usando os padrões da Web](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Visão geral de páginas mestras do ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Tutoriais de (CSS) de folhas de estilo em cascata](http://www.w3schools.com/css/default.asp)
- [Definir dinamicamente o título da página](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Páginas mestras no ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Tutoriais de início rápido de páginas mestras](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 3.5 in 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](nested-master-pages-cs.md)
> [Próximo](multiple-contentplaceholders-and-default-content-vb.md)
