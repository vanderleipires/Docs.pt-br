---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Páginas mestras | Microsoft Docs
author: microsoft
description: Um dos principais componentes para um site da Web com êxito é uma aparência consistente. No ASP.NET 1. x, os desenvolvedores usado controles de usuário para replicar elem. de página comuns.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: f45dd9704f665244d2a48ec000326f6e98984e4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30885088"
---
<a name="master-pages"></a>Páginas mestras
====================
por [Microsoft](https://github.com/microsoft)

> Um dos principais componentes para um site da Web com êxito é uma aparência consistente. No ASP.NET 1. x, os desenvolvedores usado controles de usuário para replicar os elementos de página comuns em um aplicativo Web. Embora seja certamente uma solução viável, usando controles de usuário tem algumas desvantagens. Por exemplo, uma alteração na posição de um controle de usuário requer uma alteração em várias páginas em um site. Controles de usuário também não são processados no modo de Design depois de inserido em uma página.


Um dos principais componentes para um site da Web com êxito é uma aparência consistente. No ASP.NET 1. x, os desenvolvedores usado controles de usuário para replicar os elementos de página comuns em um aplicativo Web. Embora seja certamente uma solução viável, usando controles de usuário tem algumas desvantagens. Por exemplo, uma alteração na posição de um controle de usuário requer uma alteração em várias páginas em um site. Controles de usuário também não são processados no modo de Design depois de inserido em uma página.

O ASP.NET 2.0 apresenta mestre páginas como uma maneira de manter uma aparência consistente e você verá em breve, mestre páginas representam uma melhoria significativa em relação ao método de controle de usuário.

## <a name="why-master-pages"></a>Páginas mestras por que?

Você deve estar se perguntando por que as páginas mestras eram necessários no ASP.NET 2.0. Afinal, os desenvolvedores de sites já estiver usando controles de usuário no ASP.NET 1. x para compartilhar as áreas de conteúdo entre páginas. Há, na verdade, vários motivos, por que os controles de usuário são uma solução menos ideal para a criação de um layout comum.

Controles de usuário, na verdade, não definem o layout de página. Em vez disso, eles definem o layout e a funcionalidade de uma parte de uma página. A diferença entre esses dois é importante porque facilita o gerenciamento de uma solução de controle de usuário muito mais difícil. Por exemplo, quando você quiser alterar a posição de um controle de usuário na página, edite a página na qual o controle de usuário é exibida. Que problema se você tiver poucas páginas, mas em sites grandes, rapidamente se tornar um pesadelo de gerenciamento do site!

Outra desvantagem do uso de controles de usuário para definir um layout comum está enraizada na arquitetura do ASP.NET em si. Se nenhum membro público de um controle de usuário for alterado, ele exige que você recompile todas as páginas que usam o controle de usuário. Por sua vez, o ASP.NET será e repetição de JIT suas páginas quando são acessados. Isso, uma vez, produz uma arquitetura não escalonável e um problema de gerenciamento de site para sites de maior.

Bem, esses problemas (e muito mais) são endereçadas por páginas mestras no ASP.NET 2.0.

## <a name="how-master-pages-work"></a>Como páginas mestras funcionam

Uma página mestra é análoga a um modelo para outras páginas. Elementos da página que devem ser compartilhados com outras páginas (por exemplo, menus, bordas, etc.) são adicionados para a página mestra. Quando novas páginas são adicionadas ao site, você pode associá-los com uma página mestra. Uma página que está associada uma página mestra é chamada um **página de conteúdo**. Por padrão, uma página de conteúdo assume a aparência da página mestra. No entanto, quando você cria uma página mestre, você pode definir as partes da página que a página de conteúdo pode substituir seu próprio conteúdo. Essas partes são definidas usando um novo controle introduzido no ASP.NET 2.0; o **ContentPlaceHolder** controle.

Uma página mestra pode conter qualquer número de controles ContentPlaceHolder (ou nenhum deles.) Na página de conteúdo, o conteúdo dos controles ContentPlaceHolder aparece dentro de **conteúdo** controles, outro novo controle no ASP.NET 2.0. Por padrão, as páginas de conteúdo que controles de conteúdo estão vazias para que você pode fornecer seu próprio conteúdo. Se você quiser usar o conteúdo da página mestra dentro de controles de conteúdo, você pode fazer assim como você verá neste módulo. O controle de conteúdo é mapeado para o controle ContentPlaceHolder por meio do atributo ContentPlaceHolderID do controle de conteúdo. O código abaixo mapas um conteúdo de controle para um controle ContentPlaceHolder chamado mainBody em uma página mestra.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> Costumam ser pessoas descrevem páginas mestras como sendo uma classe base para outras páginas. Que realmente não é true. A relação entre páginas mestras e páginas de conteúdo não é um de herança.


**Figura 1** mostra uma página mestra e uma página de conteúdo associada que aparecem no Visual Studio 2005. Você pode ver o controle ContentPlaceHolder na página mestra e o correspondente controle na página de conteúdo de conteúdo. Observe que o conteúdo de páginas mestras que está fora do ContentPlaceHolder está visível, mas esmaecidas na página de conteúdo. Somente o conteúdo dentro do ContentPlaceHolder pode ser suplantado por página de conteúdo. Qualquer outro conteúdo que vem da página mestra é imutável.


![Uma página mestra e sua página de conteúdo associada](master-pages/_static/image1.jpg)

**Figura 1**: uma página mestra e sua página de conteúdo associada


## <a name="creating-a-master-page"></a>Criando uma página mestra

Para criar uma nova página mestra:

1. Abra o Visual Studio 2005 e crie um novo site.
2. Clique em arquivo, novo, de arquivos.
3. Escolha o arquivo mestre na caixa de diálogo Adicionar Novo Item, como mostrado na **Figura 2**.
4. Clique em Adicionar.


![Criar uma nova página mestra](master-pages/_static/image2.jpg)

**Figura 2**: Criando uma nova página mestra


Observe que a extensão de arquivo para uma página mestra é <em>. master</em>. Essa é uma das maneiras em que uma página mestra difere de uma página comum. A principal diferença é que lieu de um @Page diretiva, a página mestra contém um @Master diretiva. Alternar a exibição da fonte para o mestre de página que você acabou de criar e revisar o código.

Uma nova página mestra terá um controle ContentPlaceHolder por padrão. Na maioria dos casos, faz mais sentido para criar os elementos de página comuns primeiro e, em seguida, inserir controles ContentPlaceHolder onde o conteúdo personalizado é desejado. Nesses casos, os desenvolvedores vai querer excluir o controle padrão ContentPlaceHolder e inserir novos registros, como a página é desenvolvida. Controles ContentPlaceHolder não são redimensionáveis apesar do fato de que elas exibem as alças de dimensionamento. Os tamanhos de controle ContentPlaceHolder automaticamente com base no conteúdo que ele contém, com uma exceção; Se você colocar um controle ContentPlaceHolder dentro de um elemento de bloco, como uma célula de tabela, ele será dimensionado de acordo com o tamanho do elemento.

## <a name="lab-1-working-with-master-pages"></a>Laboratório 1 trabalhando com páginas mestras

Neste laboratório, você criará uma nova página mestra e definir três controles ContentPlaceHolder. Em seguida, você cria uma nova página de conteúdo e substituirá o conteúdo de pelo menos um dos controles ContentPlaceHolder.

1. Crie uma página mestra e inserir controles ContentPlaceHolder. 

    1. Crie uma nova página mestra conforme descrito acima.
    2. Exclua o controle padrão ContentPlaceHolder.
    3. Selecione o controle ContentPlaceHolder clicando sombreada borda superior do controle e, em seguida, excluí-la pressionando a tecla DEL no teclado.
    4. Inserir uma nova tabela usando o *cabeçalho e no lado do* modelo conforme mostrado na Figura 3. Altere a largura e altura em 90% cada para que a tabela inteira está visível no designer.


![](master-pages/_static/image3.jpg)

**Figura 3**


1. Coloque o cursor em cada célula da tabela e defina o *valign* propriedade *superior*.
2. Na caixa de ferramentas, inserir um controle ContentPlaceHolder na célula superior da tabela (a célula de cabeçalho).
3. Quando você insere esse controle ContentPlaceHolder, você observará que a altura da linha ocupará quase toda a página conforme mostrado na Figura 4. Se preocupar que neste momento.


![O espaço vazio é na mesma célula como o ContentPlaceHolder](master-pages/_static/image1.gif)

**Figura 4**: O espaço vazio é na mesma célula como o ContentPlaceHolder


1. Coloque um controle ContentPlaceHolder em duas células. Depois que os outros controles ContentPlaceHolder foram inseridos, o tamanho das células da tabela deve ser esperado. A página deve agora ser semelhante a página exibida no **Figura 5**.


![O mestre com todos os controles ContentPlaceHolder. Observe que a altura da célula para a célula de cabeçalho agora é o que deve ser](master-pages/_static/image2.gif)

**Figura 5**: A mestre com todos os controles ContentPlaceHolder. Observe que a altura da célula para a célula de cabeçalho agora é o que deve ser


1. Digite algum texto de sua escolha em cada um dos três controles ContentPlaceHolder.
2. Salve a página mestra como exercise1.master.
3. Criar um novo formulário da Web e associá-la com a página mestra exercise1.master.
4. Selecione o arquivo, novo, de arquivos no Visual Studio 2005.
5. Selecione **formulário da Web** na caixa de diálogo Adicionar Novo Item.
6. Certifique-se de que a caixa de seleção Selecionar página mestra esteja marcada como mostrado na Figura 6.


![Adicionar uma nova página de conteúdo](master-pages/_static/image3.gif)

**Figura 6**: adicionar uma nova página de conteúdo


1. Clique em Adicionar.
2. Selecione exercise1.master no, selecione uma caixa de diálogo de página mestra conforme mostrado na Figura 7.
3. Clique em Okey para adicionar a nova página de conteúdo.

Nova página de conteúdo é exibido no Visual Studio com um controle de conteúdo para cada controle ContentPlaceHolder na página mestra. Por padrão, os controles de conteúdo estão vazios para que você pode adicionar seu próprio conteúdo. Se youd como para usar o conteúdo do controle ContentPlaceHolder na página mestra, simplesmente clique no símbolo de marca inteligente (a pequena seta preto no canto superior direito do controle) e escolha *padrão mestres conteúdo* marca inteligente conforme mostrado na **Figura 8**. Quando você fizer isso, o item de menu é alterado para *criar conteúdo personalizado*. Clicando no momento remove o conteúdo da página mestra, permitindo que você defina o conteúdo personalizado para esse controle de conteúdo específico.


![Definir um controle de conteúdo padrão para o conteúdo de páginas mestras](master-pages/_static/image4.gif)

**Figura 7**: definir um controle de conteúdo padrão para o conteúdo de páginas mestras


## <a name="connecting-master-page-and-content-pages"></a>Conectar-se a página mestra e páginas de conteúdo

A associação entre uma página mestra e uma página de conteúdo pode ser configurada em uma das quatro maneiras diferentes:

- O <strong>MasterPageFile</strong> atributo da @Page diretiva
- Definindo o **Page.MasterPageFile** propriedade no código.
- O **&lt;páginas&gt;** elemento no arquivo de configuração do aplicativo (Web. config na pasta raiz do aplicativo)
- O **&lt;páginas&gt;** elemento em um arquivo de configuração de subpastas (Web. config em uma subpasta)

## <a name="masterpagefile-attribute"></a>Atributo MasterPageFile

O atributo MasterPageFile torna fácil aplicar uma página mestra para uma página específica do ASP.NET. Também é o método usado para aplicar a página mestra ao verificar o **selecionar a página mestra** caixa de seleção que você fez no Exercício 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Definindo Page.MasterPageFile no código

Definindo a propriedade MasterPageFile no código, você pode aplicar uma página mestra específica ao seu conteúdo em tempo de execução. Isso é útil em casos em que você pode precisar aplicar uma página mestra específica com base em uma função de usuários ou algum outro critério. A propriedade MasterPageFile deve ser definida no método PreInit. Se estiver definido após o método PreInit, InvalidOperationException será lançada. A página em que essa propriedade está sendo definida também deve ter um conteúdo de controle, como o controle de nível superior da página. Caso contrário, um HttpException será gerada quando a propriedade MasterPageFile está definida.

## <a name="using-the-ltpagesgt-element"></a>Usando o &lt;páginas&gt; elemento

Você pode configurar uma página mestra para as páginas, definindo o atributo masterPageFile no &lt;páginas&gt; elemento do arquivo Web. config. Ao usar esse método, tenha em mente que os arquivos Web. config inferiores na estrutura de aplicativo podem substituir essa configuração. Qualquer atributo MasterPageFile definido um @Page diretiva também ignorará esta configuração. Usando o &lt;páginas&gt; elemento torna simples para criar um <em>mestre</em> página mestra que pode ser substituída se necessário em determinadas pastas ou arquivos.

## <a name="properties-in-master-pages"></a>Propriedades em páginas mestras

Uma página mestre pode expor propriedades simplesmente fazendo as propriedades públicas dentro da página mestra. Por exemplo, o código a seguir define uma propriedade chamada SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Para acessar a propriedade SomeProperty da página de conteúdo, você precisará usar o mestre de propriedade da seguinte forma:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Páginas mestras de aninhamento

Páginas mestras são a solução ideal para garantir uma aparência comum em um aplicativo Web grande. No entanto, não é incomum ter algumas partes de um site grande compartilham uma interface comum, enquanto outras partes compartilham uma interface diferente. Para atender a essa necessidade, várias páginas mestras são a solução ideal. No entanto, que ainda não aborda o fato de que um aplicativo grande pode ter alguns componentes (por exemplo, um menu, por exemplo) que são compartilhadas entre todas as páginas e outros componentes que são compartilhados somente entre determinadas seções do site. Para esse tipo de situação, páginas mestras aninhadas preenchem perfeitamente a necessidade. Como vimos, uma página mestra normal consiste em uma página mestra e uma página de conteúdo. Em uma situação de página mestre aninhada, há duas páginas mestras. um mestre de pai e um mestre de filho. A página mestra filho também é uma página de conteúdo e seu mestre é a página mestra pai.

Aqui está o código para uma página mestra típico:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

Em um cenário mestre aninhado, isso seria o mestre de pai. Outra página mestra usaria essa página como sua página mestra e que o código teria esta aparência:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Observe que neste cenário, o mestre filho também é uma página de conteúdo para o mestre de pai. Todo o conteúdo do mestre filho é exibido dentro de um controle de conteúdo que obtém seu conteúdo do controle do ContentPlaceHolder do pai.

> [!NOTE]
> Suporte ao designer não está disponível para páginas mestras aninhadas. Quando você estiver desenvolvendo usando mestres aninhadas, você precisará usar a exibição da fonte.


Este vídeo mostra um passo a passo de como usar páginas mestras aninhadas.


![](master-pages/_static/image1.png)


[Abra vídeo de tela inteira](master-pages/_static/nested1.wmv)


![Selecionar uma página mestra](master-pages/_static/image4.jpg)

**Figura 8**: selecionar uma página mestra
