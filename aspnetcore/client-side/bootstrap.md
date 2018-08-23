---
title: Crie sites atraentes e dinâmicos com o Bootstrap e ASP.NET Core
author: ardalis
description: Saiba como usar a inicialização para o desenvolvimento de aplicativos web responsivo com ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: client-side/bootstrap
ms.openlocfilehash: 1ccf33b299739f5aa963a53feb70b44b290443ca
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832546"
---
# <a name="build-beautiful-responsive-sites-with-bootstrap-and-aspnet-core"></a>Crie sites atraentes e dinâmicos com o Bootstrap e ASP.NET Core

<a name="bootstrap-index"></a>

Por [Steve Smith](https://ardalis.com/)

Bootstrap atualmente é a estrutura da web mais popular de desenvolvimento de aplicativos web responsivo. Ele oferece uma série de recursos e benefícios que podem melhorar a experiência dos usuários ao seu site, se você for um iniciante no front-end design e desenvolvimento ou de um especialista. Bootstrap é implantado como um conjunto de arquivos CSS e JavaScript e foi projetada para ajudar a dimensionar seu site ou aplicativo com eficiência de telefones para tablets para áreas de trabalho.

## <a name="get-started"></a>Introdução

Há várias maneiras para começar a Bootstrap. Se você estiver iniciando um novo aplicativo web no Visual Studio, você pode escolher o modelo de início padrão para o ASP.NET Core, no qual caso Bootstrap virão pré-instalados:

![Bootstrap no modo de exibição de solução de modelo starter](bootstrap/_static/bootstrap-in-starter-template.png)

A adição de Bootstrap para um ASP.NET Core projeto é simplesmente uma questão de adicioná-la à *bower. JSON* como uma dependência:

[!code-json[](../common/samples/WebApplication1/bower.json?highlight=5)]

Essa é a maneira recomendada para adicionar o bootstrap para um projeto do ASP.NET Core.

Você também pode instalar o bootstrap usando um dos vários gerenciadores de pacotes, como Bower, npm ou NuGet. Em cada caso, o processo é essencialmente o mesmo:

### <a name="bower"></a>Bower

```console
bower install bootstrap
```

### <a name="npm"></a>npm

```console
npm install bootstrap
```

### <a name="nuget"></a>NuGet

```console
Install-Package bootstrap
```

> [!NOTE]
> A maneira recomendada para instalar dependências de cliente como o bootstrap no ASP.NET Core é por meio do Bower (usando *bower. JSON*, conforme mostrado acima). O uso do npm/NuGet são mostrados para demonstrar como Bootstrap pode ser facilmente adicionada a outros tipos de aplicativos web, incluindo versões anteriores do ASP.NET.

Se você estiver fazendo referência a suas próprias versões locais do bootstrap, você precisará fazer referência a eles em todas as páginas que irá usá-lo. Em produção, você deve fazer referência bootstrap usando uma CDN. No modelo de site do ASP.NET Core padrão, o *layout. cshtml* arquivo então, como este:

[!code-html[](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Se você pretende usar qualquer um dos plug-ins de jQuery do Bootstrap, você também precisará fazer referência a jQuery.

## <a name="basic-templates-and-features"></a>Recursos e modelos básicos

O modelo do bootstrap mais básico é muito parecido com o arquivo *cshtml* mostrado acima e simplesmente inclui um menu básico para navegação e um local para renderizar o restante da página.

### <a name="basic-navigation"></a>Navegação básica

O modelo padrão usa um conjunto de `<div>` elementos para processar uma barra de navegação superior e o corpo principal da página. Se você estiver usando HTML5, você pode substituir a primeira `<div>` marcar com um `<nav>` marca para obter o mesmo efeito, mas com semântica mais precisa. Este primeiro em `<div>` você pode ver, há várias outras. Primeiro, uma `<div>` com uma classe de "contêiner" e, em seguida, em que, mais dois `<div>` elementos: "navbar-header" e "navbar-collapse". A div de cabeçalho de barra de navegação inclui um botão que será exibido quando a tela estiver abaixo de uma determinada largura mínima, mostrando 3 linhas horizontais (uma assim chamada "ícone de hambúrguer"). O ícone é renderizado usando puro HTML e CSS; Nenhuma imagem é necessária. Este é o código que exibe o ícone, com cada um a <span> marcas de renderização de uma das barras em branco:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Ele também inclui o nome do aplicativo, que aparece na parte superior esquerda. O menu de navegação principal é processado pelo `<ul>` elemento dentro do div segundo e inclui links para a página inicial, sobre e contato. Abaixo do painel de navegação, o corpo principal de cada página é renderizado em outro `<div>`, marcado com as classes "contêiner" e "conteúdo do corpo". No padrão simple \_mostrado aqui, o conteúdo da página é renderizado pelo modo de exibição específico associado com a página e, em seguida, um simples do arquivo de Layout `<footer>` é adicionado ao final do `<div>` elemento. Você pode ver como o interno sobre a página aparece usando este modelo:

![Sobre a página](bootstrap/_static/about-page-wide.png)

Barra de navegação recolhida, com o botão "hambúrguer" no canto superior direito, será exibida quando a janela cai abaixo de uma determinada largura:

![página sobre com o menu de hambúrguer](bootstrap/_static/about-page-hamburger.png)

Clicando no ícone de revela os itens de menu em uma gaveta vertical que desliza para baixo da parte superior da página:

![sobre a página com um menu hamburger expandido](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Tipografia e links

Bootstrap configura tipografia básica, cores e formatação em seu arquivo CSS de link do site. Esse arquivo CSS inclui estilos de padrão para tabelas, botões, elementos de formulário, imagens e muito mais ([mais](http://getbootstrap.com/css/)). Um recurso útil é o sistema de layout de grade, abordado em seguida. Um recurso particularmente útil é o sistema de layout de grade, explicado a seguir.

### <a name="grids"></a>Grades

Um dos recursos mais populares de Bootstrap é o sistema de layout de grade. Aplicativos web modernos devem evitar usar o `<table>` marca de layout, em vez disso, restringir o uso desse elemento para dados de tabela reais. Em vez disso, colunas e linhas podem ser dispostas usando uma série de `<div>` elementos e as classes CSS apropriadas. Há várias vantagens dessa abordagem, incluindo a capacidade de ajustar o layout de grade para exibir verticalmente em estreitas telas, como em telefones.

[Sistema de layout de grade do bootstrap](http://getbootstrap.com/css/#grid) é baseado em doze colunas. Esse número foi escolhido porque pode ser dividido uniformemente em 1, 2, 3 ou 4 colunas e a largura das colunas pode variar de 1/12 da largura da tela vertical. Para começar a usar o sistema de layout de grade, você deve começar com um contêiner `<div>` e, em seguida, adicione uma linha `<div>`, conforme mostrado aqui:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Em seguida, adicionar mais `<div>` elementos para cada coluna e especifique o número de colunas que `<div>` deve ocupar (fora de 12) como parte de uma classe CSS começando com "col - md-". Por exemplo, se você quiser simplesmente ter duas colunas de tamanho igual, você usaria uma classe de "col-md-6" para cada uma delas. Nesse caso, "md" é a abreviação de "Médio" e se refere a tamanhos de exibição de tamanho padrão de computador desktop. Há quatro opções diferentes que podem ser escolhidos, e cada um será usada para larguras mais alto, a menos que substituído (portanto, se você quiser que o layout ser corrigido, independentemente da largura da tela, você pode especificar apenas classes xs).

Prefixo da classe CSS | Camada do dispositivo | Largura
:---: | :---: | :---:
col-xs- | Telefones | < 768px
col-sm- | Tablets | >= 768px
col-md - | Áreas de trabalho | >= 992px
col-lg - | Exibe da área de trabalho maiores | >= 1200px

Ao especificar duas colunas com "col-md-6" o layout resultante será duas colunas em resoluções da área de trabalho, mas essas duas colunas serão empilhados verticalmente quando renderizados em dispositivos menores (ou uma janela do navegador mais restrito em uma área de trabalho), permitindo que os usuários exibir com facilidade conteúdo sem a necessidade de rolar horizontalmente.

Bootstrap padrão será sempre um layout de coluna única, portanto você precisa apenas especificar colunas quando você quiser mais de uma coluna. A única vez em que você desejaria especificar explicitamente que um `<div>` seria fazer backup de todos os 12 colunas substituir o comportamento de uma camada do dispositivo maior. Ao especificar várias classes de camada do dispositivo, você precisa redefinir a renderização de coluna em determinados pontos. Adicionar um div clearfix que é visível somente dentro de um visor de determinados conseguir isso, conforme mostrado aqui:

![grade de visor estreitos e largos](bootstrap/_static/narrow-and-wide-viewport-grid.png)

No exemplo acima, um e dois compartilham uma linha no layout de "md", enquanto dois e três compartilham uma linha no layout de "xs". Sem o clearfix `<div>`, dois e três não são exibidas corretamente no modo de exibição de "xs" (Observe que apenas um, quatro e cinco é mostrados):

![grade sem usar clearfix](bootstrap/_static/grid-without-clearfix.png)

Neste exemplo, apenas uma única linha `<div>` tiver sido usado, e ainda Bootstrap principalmente fizeram a coisa certa com relação ao layout e empilhamento das colunas. Normalmente, você deve especificar uma linha `<div>` para cada linha horizontal requer seu layout e claro, você pode aninhar grades Bootstrap dentro uns dos outros. Quando você fizer isso, cada grade aninhada ocupará 100% da largura do elemento no qual ele é colocado, que, em seguida, pode ser subdividido usando classes de coluna.

### <a name="jumbotron"></a>Jumbotron

Se você já usou os modelos do ASP.NET Core MVC padrão no Visual Studio 2012 ou 2013, você provavelmente já viu o Jumbotron em ação. Ele se refere a uma seção grande de largura inteira de uma página que pode ser usada para exibir uma imagem de plano de fundo grande, uma chamada para ação, uma alternância ou elementos semelhantes. Para adicionar um jumbotron para uma página, basta adicionar uma `<div>` e dê a ele uma classe de "jumbotron" e, em seguida, colocar um contêiner `<div>` dentro e adicionar seu conteúdo. Podemos pode ajustar facilmente o padrão de página a ser usado um jumbotron para os títulos principais, que ele exibe:

![exemplo de jumbotron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Botões

As classes padrão do botão e suas cores são mostrados na figura abaixo.

![botões com temas](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Notificações

Notificações se referir a textos explicativos pequeno, geralmente numéricos ao lado de um item de navegação. Eles podem indicar um número de mensagens ou notificações de espera, ou a presença de atualizações. Especificar essas notificações é tão simple quanto adicionar um `<span>` que contém o texto, com a classe de "notificação":

![notificações com temas](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Alertas

Talvez você precise exibir algum tipo de notificação, alerta ou mensagem de erro para usuários do seu aplicativo. Isso é onde as classes padrão do alerta são úteis. Há quatro níveis de severidade diferente com esquemas de cores associadas:

![alertas com temas](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Menus e barras de navegação

Nosso layout já inclui uma barra de navegação padrão, mas o tema do bootstrap dá suporte a opções de estilo adicionais. Podemos facilmente optar por exibir a barra de navegação verticalmente em vez de horizontalmente se preferirmos, bem como adicionar itens de subnavegação aos menus de atalho. Menus de navegação simples, como o guia de faixas, baseiam-se na parte superior da `<ul>` elementos. Eles podem ser facilmente criados, apenas fornecendo-os com as classes CSS "nav" e "nav-guias":

![faixas de guias com temas](bootstrap/_static/theme-tabstrips.png)

Barras de navegação são criadas da mesma forma, mas são um pouco mais complexas. Eles começam com um `<nav>` ou `<div>` com a classe de "barra de navegação", no qual um div do contêiner mantém o restante dos elementos. Nossa página inclui uma barra de navegação em seu cabeçalho já – mostrado a seguir simplesmente se expande sobre isso, adicionando suporte para um menu suspenso:

![tema das barras de navegação](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Elementos adicionais

O tema padrão também pode ser usado para apresentar tabelas HTML em um estilo bem formatado, incluindo suporte para modos de exibição distribuídos. Há rótulos com estilos que são semelhantes dos botões. Você pode criar menus suspensos personalizados que dão suporte a opções de estilo adicionais além do padrão HTML `<select>` elemento, juntamente com barras de navegação como o nosso site de início padrão já está usando. Se você precisar de uma barra de progresso, há vários estilos à sua escolha, bem como a lista de grupos e painéis que incluem um título e o conteúdo. Explore as opções adicionais do tema de Bootstrap padrão aqui:

[http://getbootstrap.com/examples/theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Mais temas

Você pode estender o tema de Bootstrap padrão, substituindo alguns ou todos os seus CSS, ajustar as cores e estilos para atender às necessidades do seu próprio aplicativo. Se você deseja iniciar a partir de um tema pronto, há vários galerias de tema disponíveis online que especializados em temas de Bootstrap, como WrapBootstrap.com (que tem uma variedade de temas comerciais) e Bootswatch.com (que oferece temas livres). Alguns dos modelos disponíveis pagos fornecem uma grande quantidade de funcionalidade sobre o tema de Bootstrap básica, como suporte avançado para administrativas menus e painéis avançados gráficos e medidores. Um exemplo de um modelo pago popular é Inspinia, que é mostrado na seguinte captura de tela:

![Exemplo tema inspinia](bootstrap/_static/theme-inspinia.png)

Se você quiser alterar o tema do bootstrap, coloque o arquivo *bootstrap.css* para o tema que você deseja na pasta **wwwroot/css** e altere as referências no *cshtml* para apontá-lo. Altere os links para todos os ambientes:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Se você quiser criar seu próprio painel, você pode iniciar do exemplo gratuito disponível aqui: [ http://getbootstrap.com/examples/dashboard/ ](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Componentes

Além desses elementos já discutidos, bootstrap inclui suporte para uma variedade de [componentes internos de interface do usuário](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Bootstrap inclui conjuntos de ícones de Glyphicons ([http://glyphicons.com](http://glyphicons.com)), com mais de 200 ícones disponíveis gratuitamente para uso dentro de seu aplicativo da web habilitado para o bootstrap. Aqui está apenas uma pequena amostra: Aqui está a apenas uma pequena amostra:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>Grupos de entrada

Grupos de entrada permitem agrupamento de texto adicional ou botões com um elemento de entrada, oferecendo ao usuário uma experiência mais intuitiva:

![grupos de entrada](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Trilhas

Navegação estrutural é um componente de interface do usuário comum usado para mostrar ao usuário seu histórico recente ou a profundidade em uma hierarquia de navegação do site. Adicioná-los com facilidade, aplicando a classe "de trilha" para qualquer `<ol>` elemento da lista. Inclui suporte interno para a paginação usando a classe "paginação" em uma `<ul>` elemento dentro de um `<nav>`. Adicionar apresentações de slides inseridas responsivos e vídeo usando `<iframe>`, `<embed>`, `<video>`, ou `<object>` elementos, que Bootstrap será estilo automaticamente. Especifica uma taxa de proporção específica usando as classes específicas, como "Inserir-responsivo-16by9".

## <a name="javascript-support"></a>Suporte a JavaScript

Biblioteca de JavaScript do Bootstrap inclui suporte de API para os componentes incluídos, permitindo que você controle o comportamento de forma programática dentro de seu aplicativo. Além disso, *bootstrap. js* inclui mais de uma dúzia plug-ins personalizados do jQuery, fornecendo recursos adicionais, como faz a transição, caixas de diálogo modais, role para a detecção (Atualizando estilos com base em onde o usuário rolou no documento), comportamento de recolher, carrosséis e associar menus para a janela para que eles não aparecem na tela. Não há espaço suficiente para cobrir todos os complementos de JavaScript integrados do Bootstrap – para saber mais, visite [ http://getbootstrap.com/javascript/ ](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Resumo

Bootstrap fornece uma estrutura de web que pode ser usada para formatar e uma ampla variedade de sites e aplicativos de estilo de forma rápida e produtiva. Seu tipografia básica e estilos fornecem uma agradável aparência que pode ser facilmente manipulada por meio do suporte de tema personalizado, que pode ser artesanal ou adquirido comercialmente. Ele dá suporte a um host de componentes da web que seria tiver solicitado caros controles de terceiros para realizar e dar suporte a padrões da web modernos e abertos no passado.
