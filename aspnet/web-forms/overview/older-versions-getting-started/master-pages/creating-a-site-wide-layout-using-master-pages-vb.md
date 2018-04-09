---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
title: Criar um Layout de todo o Site usando páginas mestras (VB) | Microsoft Docs
author: rick-anderson
description: Este tutorial mostrará as Noções básicas de página mestra. Isto é, quais são as páginas mestras, como faz uma criar uma página mestre, como quais são proprietários do conteúdo local, faz uma cr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 30945276-8ed9-4b27-8e50-4309244d3559
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: d18993af7159de552db0c622fbef58e814e36ebb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-site-wide-layout-using-master-pages-vb"></a>Criar um Layout de todo o Site usando páginas mestras (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_VB.pdf)

> Este tutorial mostrará as Noções básicas de página mestra. Isto é, quais são as páginas mestras, como uma criar uma página mestre, quais são proprietários do conteúdo local, como um cria uma página ASP.NET que usa uma página mestre, como modificar a página mestra automaticamente é refletida em suas páginas de conteúdo associadas e assim por diante.


## <a name="introduction"></a>Introdução

Um atributo de um site bem projetado é um layout de página consistente em todo o site. Veja o site www.asp.net, por exemplo. No momento da redação deste artigo, cada página tem o mesmo conteúdo na parte superior e inferior da página. Como mostra a Figura 1, o início de cada página exibe uma barra cinza com uma lista de Communities da Microsoft. Abaixo que é o logotipo do site, a lista de idiomas em que o site foi traduzido e as seções principais: Home, começar, saiba, Downloads e assim por diante. Da mesma forma, a parte inferior da página inclui informações sobre anúncios www.asp.net, uma declaração de direitos autorais e um link para a declaração de privacidade.


[![O site www.asp.net emprega uma aparência consistente em todas as páginas](creating-a-site-wide-layout-using-master-pages-vb/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image1.png)

<strong>Figura 01</strong>: O site www.asp.net emprega uma aparência consistente e sinta-se em todas as páginas ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image3.png))


Outro atributo de um site bem projetado é a facilidade com que a aparência do site pode ser alterada. A Figura 1 mostra a home page do www.asp.net a partir de março de 2008, mas entre agora e publicação neste tutorial, a aparência pode ter sido alterado. Talvez os itens de menu na parte superior se expandirá para incluir uma nova seção para a estrutura MVC. Ou talvez um radicalmente novo design com diferentes cores, fontes e layout lado surgem. Aplicar essas alterações para o site inteiro deve ser um processo rápido e simple que não requer modificação milhares de páginas da web que compõem o site.

Criar um modelo de página de todo o site no ASP.NET é possível por meio do uso de *páginas mestras*. Resumindo, uma página mestra é um tipo especial de página ASP.NET que define a marcação que é comum entre todos os *páginas de conteúdo* , bem como as regiões são personalizáveis em cada página de conteúdo por página de conteúdo. (Uma página de conteúdo é uma página ASP.NET que é vinculada à página mestre.) Sempre que uma página mestra layout ou formatação for alterado, todos de saída do conteúdo das suas páginas é da mesma forma atualizada imediatamente, que facilita a aplicação de alterações de aparência de todo o site tão fácil quanto a atualização e implantar um único arquivo (ou seja, a página mestra).

Este é o primeiro tutorial em uma série de tutoriais explorar usando páginas mestras. Ao longo da série tutorial é:

- Examine a criar páginas mestras e suas páginas de conteúdo associadas
- Discutir a uma variedade de dicas, truques e interceptações,
- Identificar armadilhas comuns de página mestra e explorar as soluções alternativas,
- Saber como acessar a página mestra de uma página de conteúdo e a-vice-versa,
- Saiba como especificar um conteúdo da página mestra em tempo de execução, e
- Tópicos de página mestra outros avançados.

Esses tutoriais são orientados para ser conciso e fornecem instruções passo a passo com capturas de tela suficiente para orientá-lo pelo processo visualmente. Cada tutorial está disponível em c# e Visual Basic versões e inclui um download do código completo usado.

Este tutorial inaugural inicia examinando os fundamentos de página mestra. Discuta como páginas mestras funcionam, examinar a criação de uma página mestra e páginas de conteúdo associadas usando o Visual Web Developer e ver como as alterações a uma página mestra são refletidas imediatamente em suas páginas de conteúdo. Vamos começar!

## <a name="understanding-how-master-pages-work"></a>Noções básicas sobre como páginas mestras funcionam

Criação de um site com um layout de página consistente em todo o site requer que cada página da web emitir comuns marcação formatação, além de seu conteúdo personalizado. Por exemplo, enquanto cada postagem tutorial ou fórum em www.asp.net tem seu próprio conteúdo exclusivo, cada uma dessas páginas também processar uma série de comuns `<div>` elementos que exibem os links da seção de nível superior: Home, começar, saiba mais e assim por diante.

Há uma variedade de técnicas para criar páginas da web com uma aparência consistente. Uma abordagem simples é simplesmente copie e cole a marcação de layout comuns em todas as páginas da web, mas essa abordagem tem várias desvantagens. Para começar, sempre que uma nova página for criada, você deverá se lembrar de copiar e colar o conteúdo compartilhado na página. Tais copiando e colando as operações são pronta para o erro, você poderá copiar acidentalmente apenas um subconjunto da marcação compartilhada em uma nova página. E para completar, essa abordagem torna substituindo a aparência de todo o site existente com um novo um real problemáticos porque todas as páginas no site único devem ser editadas para usar a nova aparência.

Antes do ASP.NET versão 2.0, página os desenvolvedores geralmente inseridos marcação comuns em [controles de usuário](https://msdn.microsoft.com/library/y6wb1a0e.aspx) e, em seguida, adicionar esses controles de usuário para cada página. Essa abordagem é necessário que o desenvolvedor de página não se esqueça de adicionar manualmente os controles de usuário para cada nova página, mas permitido para facilitar modificações de todo o site porque ao atualizar a marcação comuns somente os controles de usuário necessários para a ser modificado. Infelizmente, o Visual Studio .NET 2002 e 2003 - as versões do Visual Studio usado para criar aplicativos do ASP.NET 1. x - renderizados controles de usuário no modo de exibição de Design como caixas cinzas. Consequentemente, os desenvolvedores de página usando essa abordagem não gostou de um ambiente de tempo de design WYSIWYG.

As limitações do uso de controles de usuário foram resolvidas no ASP.NET versão 2.0 e o Visual Studio 2005 com a introdução de *páginas mestras*. Uma página mestra é um tipo especial de página ASP.NET que define a marcação de todo o site e o *regiões* onde associados *páginas de conteúdo* definir sua marcação personalizada. Como veremos na etapa 1, essas regiões são definidas por controles ContentPlaceHolder. O controle ContentPlaceHolder simplesmente denota uma posição na hierarquia de controle da página mestra onde o conteúdo personalizado pode ser inserido por uma página de conteúdo.

> [!NOTE]
> Os principais conceitos e a funcionalidade de páginas mestras não foi alterado desde o ASP.NET versão 2.0. No entanto, o Visual Studio 2008 oferece suporte de tempo de design para páginas mestras aninhadas, um recurso que não existia no Visual Studio 2005. Examinaremos usando páginas mestras aninhadas em um tutorial futuras.


A Figura 2 mostra a página mestra para www.asp.net aparência. Observe que a página mestra define o layout de todo o site - a marcação na parte superior, inferior e à direita de cada página - comuns, bem como um ContentPlaceHolder na parte intermediária esquerda, onde se encontra o conteúdo exclusivo para cada página da web.


![Uma página mestra define o Layout de todo o Site e as regiões editáveis em cada página de conteúdo por página conteúdo](creating-a-site-wide-layout-using-master-pages-vb/_static/image4.png)

**Figura 02**: uma página mestra define o Layout de todo o Site e as regiões editáveis em cada página de conteúdo por página conteúdo


Depois que uma página mestra foi definida pode ser associado às novas páginas ASP.NET por meio de escala de uma caixa de seleção. Essas páginas ASP.NET - chamadas páginas de conteúdo - incluem um controle de conteúdo para cada um dos controles ContentPlaceHolder da página mestra. Quando a página de conteúdo é visitada por meio de um navegador o mecanismo do ASP.NET cria a hierarquia de controle da página mestra e injeta a hierarquia de controle da página de conteúdo em locais apropriados. Essa hierarquia de controle combinado é renderizada e o HTML resultante é retornado ao navegador do usuário final. Consequentemente, a página de conteúdo emite a marcação comuns definidos em sua página mestra fora controles ContentPlaceHolder e a marcação de específico de página definido dentro de seu próprios controles de conteúdo. A Figura 3 ilustra esse conceito.


[![Marcação da página solicitada tem fusível para a página mestra](creating-a-site-wide-layout-using-master-pages-vb/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image5.png)

**Figura 03**: marcação da página solicitado o tem fusível para a página mestra ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image7.png))


Agora que já discutimos como páginas mestras funcionam, vamos dar uma olhada na criação de uma página mestra e páginas de conteúdo associadas usando o Visual Web Developer.

> [!NOTE]
> Para alcançar o público mais amplo possível, o site da Web ASP.NET criamos em toda a série de tutoriais será criado usando ASP.NET 3.5 com a versão gratuita da Microsoft do Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Se você ainda não foram atualizados para o ASP.NET 3.5, não se preocupe - os conceitos abordados trabalho esses tutoriais igualmente bem com o ASP.NET 2.0 e o Visual Studio 2005. No entanto, alguns aplicativos de demonstração podem usar recursos que são novos para o .NET Framework versão 3.5; Quando são usados recursos específicos de 3.5, posso incluir uma observação que explica como implementar uma funcionalidade semelhante na versão 2.0. Tenha em mente que os aplicativos de demonstração disponíveis para baixar o .NET Framework versão 3.5, que resulta em cada destino tutorial um `Web.config` arquivo que inclui os elementos de configuração específicas de 3.5. História, se você ainda precisa instalar o .NET 3.5 em seu computador, em seguida, o aplicativo web para download não funcionará sem primeiro remover a marcação específica do 3.5 de `Web.config`. Consulte [do dissecando ASP.NET versão 3.5 `Web.config` arquivo](http://www.4guysfromrolla.com/articles/121207-1.aspx) para obter mais informações sobre este tópico.


## <a name="step-1-creating-a-master-page"></a>Etapa 1: Criando uma página mestra

Antes de podemos explorar criando e usando páginas mestras e de conteúdo, é preciso primeiro um site ASP.NET. Comece criando um novo arquivo com base em sistema site da Web ASP.NET. Para fazer isso, inicie o Visual Web Developer e vá para o menu Arquivo e escolha o novo Site da Web, exibindo a caixa de diálogo do novo Site caixa (consulte a Figura 4). Escolha o modelo de Site da Web ASP.NET, definir a lista suspensa do local do sistema de arquivos, escolha uma pasta para colocar o site da web e definir a linguagem Visual Basic. Isso criará um novo site com um `Default.aspx` página ASP.NET, um `App_Data` pasta e um `Web.config` arquivo.

> [!NOTE]
> Visual Studio oferece suporte a dois modos de gerenciamento de projeto: os projetos de Site da Web e projetos de aplicativo Web. Projetos de Site não tem um arquivo de projeto, enquanto os projetos de aplicativo Web imitar a arquitetura de projeto no Visual Studio .NET 2002/2003 - eles incluem um arquivo de projeto e compilar o código-fonte do projeto em um único assembly, que é colocado no `/bin` pasta. O Visual Studio 2005 inicialmente apenas sites da Web com suporte de projetos, embora o modelo de projeto de aplicativo Web foi reintroduzido com Service Pack 1. O Visual Studio 2008 oferece os dois modelos de projeto. O Visual Web Developer 2005 e 2008 edições, no entanto, somente suportam a projetos de Site. Posso usar o modelo de projeto de Site da Web para meu demonstrações nesta série tutorial. Se você estiver usando uma edição não Express e deseja usar o [modelo de projeto de aplicativo Web](https://msdn.microsoft.com/library/aa730880(vs.80).aspx) em vez disso, fique à vontade para fazer isso, mas lembre-se de que pode haver algumas discrepâncias entre o que você vê na tela e as etapas que você deve tomar em relação a capturas de tela mostradas e instruções fornecidas nos tutoriais.


[![Criar um novo arquivo com base em sistema Site da Web](creating-a-site-wide-layout-using-master-pages-vb/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image8.png)

**Figura 04**: criar um Site New File System-Based ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image10.png))


Em seguida, adicione uma página mestra para o site no diretório raiz clicando duas vezes no nome do projeto, escolha Adicionar Novo Item e selecionando o modelo de página mestra. Observe que páginas mestras terminam com a extensão `.master`. Nomeie a nova página mestra `Site.master` e clique em Adicionar.


[![Adicionar uma página mestra chamado Site.master para o site](creating-a-site-wide-layout-using-master-pages-vb/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image11.png)

**Figura 05**: adicionar um nome de página mestra `Site.master` para o site ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image13.png))


Adicionar um novo arquivo de página mestre por meio do Visual Web Developer cria uma página mestra com a seguinte marcação declarativa:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample1.aspx)]

A primeira linha na marcação declarativa é o [ `@Master` diretiva](https://msdn.microsoft.com/library/ms228176.aspx). O `@Master` diretiva é semelhante do [ `@Page` diretiva](https://msdn.microsoft.com/library/ydy4x04a.aspx) que aparece nas páginas do ASP.NET. Define o idioma do servidor (VB) e informações sobre o local e a herança de classe de code-behind da página mestra.

O `DOCTYPE` e marcação declarativa da página aparece sob o `@Master` diretiva. A página inclui HTML estático juntamente com quatro controles do lado do servidor:

- **Um formulário da Web (o `<form runat="server">`)** - porque todas as páginas ASP.NET normalmente tem um formulário da Web - e como a página mestra pode incluir controles da Web que devem aparecer em um formulário da Web - Certifique-se de adicionar o formulário da Web para a página mestra (em vez de adicionar um formulário da Web e fique página de conteúdo).
- **Um controle ContentPlaceHolder chamado `ContentPlaceHolder1`**  -este controle ContentPlaceHolder aparece no formulário da Web e serve como a região para a interface do usuário da página de conteúdo.
- **Um lado do servidor `<head>` elemento** - o `<head>` elemento tem o `runat="server"` atributo, tornando-os acessíveis através do código do lado do servidor. O `<head>` elemento é implementado dessa maneira para que o título da página e outros `<head>`-relacionados a marcação pode ser adicionada ou ajustada por meio de programação. Por exemplo, a definição de uma página do ASP.NET `Title` alterações de propriedade de `<title>` elemento renderizado pelo `<head>` controle de servidor.
- **Um controle ContentPlaceHolder chamado `head`**  -este controle ContentPlaceHolder aparece dentro a `<head>` servidor controlar e pode ser usado para adicionar declarativamente conteúdo para o `<head>` elemento.

Essa marcação declarativa de página mestra padrão serve como um ponto de partida para criar suas próprias páginas mestras. Fique à vontade para editar o HTML ou para adicionar controles adicionais de Web ou ContentPlaceHolders para a página mestra.

> [!NOTE]
> Ao criar uma página mestra Verifique se a página mestra contém um formulário da Web e pelo menos um controle ContentPlaceHolder aparece dentro desse formulário da Web.


### <a name="creating-a-simple-site-layout"></a>Criar um Layout de Site simples

Vamos expandir `Site.master`da marcação declarativa de padrão para criar um layout de site em que todas as páginas compartilham: um cabeçalho comum; uma coluna à esquerda com navegação, notícias e outro conteúdo de todo o site; e um rodapé que exibe o ícone de "Alimentado por Microsoft ASP.NET". A Figura 6 mostra o resultado final da página mestra quando uma das suas páginas de conteúdo é exibida por meio de um navegador. A região dentro de um círculo vermelha na Figura 6 é específica para a página que está sendo visitada (`Default.aspx`); o outro conteúdo é definida na página mestre e, portanto, consistente em todas as páginas de conteúdo.


[![A página mestra define a marcação para a esquerda, superior e inferior partes](creating-a-site-wide-layout-using-master-pages-vb/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image14.png)

**Figura 06**: A página mestra define a marcação para a esquerda, superior e inferior partes ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image16.png))


Para obter o layout do site mostrado na Figura 6, inicie atualizando o `Site.master` página mestra para que ele contém a seguinte marcação declarativa:

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample2.aspx)]

Layout da página mestra é definido usando uma série de `<div>` elementos HTML. O `topContent` `<div>` contém a marcação que aparece na parte superior de cada página, enquanto o `mainContent`, `leftContent`, e `footerContent` `<div>` s são usados para exibir o conteúdo da página, na coluna esquerda e "alimentado por Microsoft Ícone do ASP.NET", respectivamente. Além de adicionar essas `<div>` elementos, também renomeei a `ID` propriedade do controle ContentPlaceHolder primário de `ContentPlaceHolder1` para `MainContent`.

As regras de formatação e layout essas sortidas `<div>` elementos é descrito de forma a [folha de estilos em cascata (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets) arquivo `Styles.css`, que é especificado por meio de um `<link>` elemento da página mestra `<head>`elemento. Essas regras vários definem a aparência de cada `<div>` elemento indicado acima. Por exemplo, o `topContent` `<div>` elemento, que exibe o texto "Mestre páginas tutoriais" e o link, tem suas regras de formatação especificadas no `Styles.css` da seguinte maneira:

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample3.css)]

Se você estiver acompanhando no seu computador, você precisará baixar o código que acompanha deste tutorial e adicione o `Styles.css` ao seu projeto. Da mesma forma, você também precisará criar uma pasta chamada `Images` e copie o ícone de "Alimentado por Microsoft ASP.NET" do site de demonstração baixado ao seu projeto.

> [!NOTE]
> Uma discussão de CSS e formatação de página da web está além do escopo deste artigo. Para obter mais informações sobre CSS, confira o [CSS tutoriais](http://www.w3schools.com/css/default.asp) em [W3Schools.com](http://www.w3schools.com/). Também recomendo que você baixar o código que acompanha deste tutorial e executar com as configurações de CSS em `Styles.css` para ver os efeitos das regras de formatação diferentes.


### <a name="creating-a-master-page-using-an-existing-design-template"></a>Criando uma página mestra usando um modelo de projeto existente

Ao longo dos anos, criei um número de aplicativos web ASP.NET para empresas de pequeno a médio porte. Alguns dos meus clientes tiveram um layout de site existente que quisessem usar; outros contrataram um designer gráfico competente. Alguns confiados-me para criar o layout do site. Como você pode ver pela Figura 6, execução de tarefas do programador para criar o layout do site é geralmente como prudente como tendo o contador executar open-heart surgery enquanto o doctor faz seus impostos.

Felizmente, há sites innumerous que oferecem modelos de design HTML livres - Google retornou mais de 6 milhões de resultados para o termo de pesquisa "modelos de site gratuito." Um dos Meus Favoritos é [OpenDesigns.org](http://opendesigns.org/). Depois de encontrar um modelo de site que desejar, adicione os arquivos CSS e imagens ao seu projeto de site e integrar HTML do modelo na sua página mestra.

> [!NOTE]
> A Microsoft também oferece um número de [ASP.NET Design iniciar Kit modelos gratuitos](https://msdn.microsoft.com/asp.net/aa336613.aspx) que integram a caixa de diálogo do novo Site da Web no Visual Studio.


## <a name="step-2-creating-associated-content-pages"></a>Etapa 2: Criando páginas de conteúdo associadas

Com a página mestra criada, você está pronto para começar a criar páginas ASP.NET que estão associadas com a página mestra. Essas páginas são chamadas de *páginas de conteúdo*.

Vamos adicionar uma nova página ASP.NET para o projeto e associá-lo para o `Site.master` página mestra. Clique com botão direito no nome do projeto no Gerenciador de soluções e escolha a opção de adicionar novo Item. Selecione o modelo de formulário da Web, digite o nome `About.aspx`e marque a caixa de seleção "Selecione página mestra", conforme mostrado na Figura 7. Isso exibirá o selecione uma caixa de diálogo de página mestra caixa (consulte a Figura 8) de onde você pode escolher a página mestra.

> [!NOTE]
> Se você tiver criado seu site da Web ASP.NET usando o modelo de projeto de aplicativo Web em vez do modelo de projeto de Site você não verá a caixa de seleção "Selecionar página mestra" na caixa de diálogo Adicionar Novo Item, mostrada na Figura 7. Para criar um conteúdo de página quando você usar o projeto de aplicativo Web do modelo deve escolher o modelo de formulário de conteúdo da Web em vez do modelo de formulário da Web. Depois de selecionar o modelo de formulário de conteúdo da Web e clicar em Adicionar, o mesmo selecionar uma página mestra que será exibida a caixa de diálogo mostrada na Figura 8.


[![Adicione uma nova página de conteúdo](creating-a-site-wide-layout-using-master-pages-vb/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image17.png)

**Figura 07**: adicionar uma nova página de conteúdo ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image19.png))


[![Selecione a página mestra Site.master](creating-a-site-wide-layout-using-master-pages-vb/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image20.png)

**Figura 08**: selecione o `Site.master` página mestra ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image22.png))


Como mostra a seguinte marcação declarativa, uma página de conteúdo contém um `@Page` diretiva que aponta de volta para o mestre de página e um controle de conteúdo para cada um dos controles ContentPlaceHolder da página mestra.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample4.aspx)]

> [!NOTE]
> Na seção "Criar um Layout de Site simples" na etapa 1 renomeei `ContentPlaceHolder1` para `MainContent`. Se você não o renomeou deste controle ContentPlaceHolder `ID` da mesma forma, marcação declarativa da página de conteúdo serão um pouco diferentes da marcação mostrada acima. Do ou seja, o segundo controle de conteúdo `ContentPlaceHolderID` refletirá o `ID` do ContentPlaceHolder correspondente de controle na sua página mestra.


Ao processar uma página de conteúdo, o mecanismo do ASP.NET deve unir a página conteúdo controles com ContentPlaceHolder da sua página mestra. O mecanismo do ASP.NET determina o conteúdo da página mestra do `@Page` da diretiva `MasterPageFile` atributo. Como mostra a marcação acima, esta página de conteúdo é associada a `~/Site.master`.

Porque a página mestra tiver dois controles ContentPlaceHolder - `head` e `MainContent` -Visual Web Developer gerados dois controles de conteúdo. Cada controle de conteúdo faz referência a um determinado ContentPlaceHolder por meio de seu `ContentPlaceHolderID` propriedade.

Onde as páginas mestras se destaque sobre técnicas de modelo de todo o site anterior é com o suporte de tempo de design. A Figura 9 mostra a `About.aspx` página de conteúdo quando visualizado no modo de Design do Visual Web Developer. Observe que enquanto o conteúdo da página mestra estiver visível, ela fica esmaecida e não pode ser modificada. Controles de conteúdo correspondente ContentPlaceHolders da página mestra são, no entanto, editáveis. E, assim como com qualquer outra página do ASP.NET, você pode criar interface da página de conteúdo com a adição de controles da Web por meio de exibições de fonte ou Design.


[![Modo de Design da página de conteúdo exibe o conteúdo de página mestra e de página](creating-a-site-wide-layout-using-master-pages-vb/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image23.png)

**Figura 09**: A página conteúdo Design exibição exibe os específico de página e conteúdo da página mestra ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>Adicionando controles da Web e marcação para a página de conteúdo

Reserve um tempo para criar algum conteúdo para o `About.aspx` página. Como você pode ver na Figura 10, que inseri um título "Sobre o autor" e alguns parágrafos de texto, mas fique à vontade para adicionar controles da Web, também. Depois de criar essa interface, visite o `About.aspx` página através de um navegador.


[![Visite a página About através de um navegador](creating-a-site-wide-layout-using-master-pages-vb/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image26.png)

**Figura 10**: visite o `About.aspx` página por meio de um navegador ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image28.png))


É importante entender que a página de conteúdo solicitada e a página mestra associada são fundidos e renderizados como um todo inteiramente no servidor web. Navegador do usuário final é enviado o HTML resultante, mesclado. Para verificar isso, exiba o HTML recebido pelo navegador no menu de exibição e escolhendo a origem. Observe que há sem quadros ou outras técnicas especializadas para exibir duas páginas da web diferentes em uma única janela.

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>Associação de uma página mestra para uma página ASP.NET existente

Como vimos nesta etapa, adicionando que uma nova página de conteúdo para um aplicativo web ASP.NET é tão fácil quanto marcar a caixa de seleção "Selecionar página mestra" e escolhendo a página mestra. Infelizmente, não é tão fácil converter uma página ASP.NET existente a uma página mestra.

Para vincular a uma página mestra para uma página ASP.NET existente, você precisa executar as seguintes etapas:

1. Adicionar o `MasterPageFile` de atributo para a página ASP.NET `@Page` diretiva, apontá-lo para a página mestra apropriada.
2. Adicione controles de conteúdo para cada um dos ContentPlaceHolders na página mestra.
3. Seletivamente recortar e colar o conteúdo existente da página ASP.NET em controles de conteúdo apropriados. Digamos que "seletivamente" aqui porque o ASP.NET provavelmente página contém marcação que já é expressa pela página mestre, como o `DOCTYPE`, o `<html>` elemento e o formulário da Web.

Para obter instruções passo a passo sobre esse processo junto com capturas de tela, check-out [Scott Guthrie](https://weblogs.asp.net/scottgu/)do [usando páginas de mestras e navegação de Site](http://webproject.scottgu.com/VisualBasic/MasterPages/MasterPages.aspx) tutorial. O "atualização `Default.aspx` e `DataSample.aspx` para usar a página mestra" seção detalha essas etapas.

Como é muito mais fácil de criar novas páginas de conteúdo que converter páginas ASP.NET existentes em páginas de conteúdo, é recomendável que, sempre que você criar um novo site ASP.NET adicionar uma página mestra para o site. Associe todas as páginas ASP.NET para esta página mestra. Não se preocupe se a página mestra inicial é muito simples ou criptografado; Você pode atualizar a página mestra mais tarde.

> [!NOTE]
> Ao criar um novo aplicativo ASP.NET, Visual Web Developer adiciona um `Default.aspx` página que não está associada a uma página mestra. Se você quiser prática para converter uma página ASP.NET existente em uma página de conteúdo, vá em frente e fazê-lo com `Default.aspx`. Como alternativa, você pode excluir `Default.aspx` e, em seguida, adicione novamente, mas desta vez verificando a caixa de seleção "Selecione página mestra".


## <a name="step-3-updating-the-master-pages-markup"></a>Etapa 3: Atualização de marcação da página mestra

Um dos principais benefícios de páginas mestras é que uma única página mestra pode ser usada para definir o layout geral para várias páginas no site. Portanto, atualizando a aparência do site exige a atualização de um único arquivo - a página mestra.

Para ilustrar esse comportamento, vamos atualizar nossos página mestra para incluir a data atual na parte superior da coluna à esquerda. Adicionar um rótulo denominado `DateDisplay` para o `leftContent` `<div>`.

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample5.aspx)]

Em seguida, crie um `Page_Load` manipulador de eventos para o mestre de página e adicione o seguinte código:

[!code-vb[Main](creating-a-site-wide-layout-using-master-pages-vb/samples/sample6.vb)]

O código acima define o rótulo `Text` propriedade para a data e hora atuais formatadas como o dia da semana, o nome do mês e o dia de dois dígitos (consulte a Figura 11). Com essa alteração, revisite uma das suas páginas de conteúdo. Como mostra a Figura 11, a marcação resultante é atualizada imediatamente para incluir a alteração para a página mestra.


[![As alterações para a página mestra são refletidas quando exibindo a uma página de conteúdo](creating-a-site-wide-layout-using-master-pages-vb/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-vb/_static/image29.png)

**Figura 11**: as alterações para a página mestra são refletidas quando exibindo a uma página de conteúdo ([clique para exibir a imagem em tamanho normal](creating-a-site-wide-layout-using-master-pages-vb/_static/image31.png))


> [!NOTE]
> Conforme ilustrado neste exemplo, páginas mestras podem conter controles do lado do servidor da Web, código e manipuladores de eventos.


## <a name="summary"></a>Resumo

Páginas mestras permitem que os desenvolvedores do ASP.NET criar um layout consistente de todo o site que é facilmente atualizável. Criando páginas mestras e suas páginas de conteúdo associadas é tão simple como criar as páginas padrão do ASP.NET, como Visual Web Developer oferece suporte avançado ao tempo de design.

O exemplo de página mestra criada neste tutorial tinha dois controles ContentPlaceHolder, cabeçalho e MainContent. Somente especificamos marcação para o controle MainContent ContentPlaceHolder em nossa página de conteúdo, no entanto. No tutorial Avançar observarmos usando o conteúdo de vários controles na página de conteúdo. Vemos também como definir padrão marcação para conteúdo controla dentro da página mestra, bem como para alternar entre usar o padrão marcação definido no mestre de página e fornecendo personalizada marcação da página de conteúdo.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [ASP.NET para Designers: livre modelos de Design e orientação sobre como criar sites ASP.NET usando padrões da Web](https://msdn.microsoft.com/asp.net/aa336602.aspx)
- [Visão geral de páginas mestras do ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx)
- [Tutoriais de folhas de estilo (CSS) em cascata](http://www.w3schools.com/css/default.asp)
- [Definir dinamicamente o título da página](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [Páginas mestras no ASP.NET](http://www.odetocode.com/articles/419.aspx)
- [Tutoriais de páginas mestras](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 3.5 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](nested-master-pages-cs.md)
> [Próximo](multiple-contentplaceholders-and-default-content-vb.md)
