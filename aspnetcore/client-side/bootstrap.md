---
title: "Criando sites lindos, respondendo com inicialização"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bd27832c-2877-4b7b-9337-e009361d845f
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/bootstrap
ms.openlocfilehash: f89ad584600c3f12a936599de27f931aff0cd4b5
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2017
---
# <a name="building-beautiful-responsive-sites-with-bootstrap"></a>Criando sites lindos, respondendo com inicialização

<a name="bootstrap-index"></a>

Por [Steve Smith](https://ardalis.com/)

Inicialização atualmente é a estrutura da web mais popular de desenvolvimento de aplicativos web responsivo. Ele oferece uma série de recursos e benefícios que podem melhorar a experiência dos usuários ao seu site, se você for um iniciante no front-end design e desenvolvimento ou de um especialista. Inicialização é implantada como um conjunto de arquivos CSS e JavaScript e foi projetada para ajudar a dimensionar seu site ou aplicativo com eficiência de telefones para tablets para áreas de trabalho.

## <a name="getting-started"></a>Introdução

Há várias maneiras para começar a inicialização. Se você estiver iniciando um novo aplicativo web no Visual Studio, você pode escolher o modelo de início padrão para o ASP.NET Core, no qual caso Bootstrap virão pré-instalados:

![Inicializar no modo de exibição de solução de modelo starter](bootstrap/_static/bootstrap-in-starter-template.png)

A adição de inicialização para um ASP.NET Core projeto é simplesmente uma questão de adicioná-la à *bower. JSON* como uma dependência:

[!code-json[Main](../common/samples/WebApplication1/bower.json?highlight=5)]

Essa é a maneira recomendada para adicionar a inicialização para um projeto do ASP.NET Core.

Você também pode instalar bootstrap usando um dos vários gerenciadores de pacotes, como Bower, npm ou NuGet. Em cada caso, o processo é essencialmente o mesmo:

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
> A maneira recomendada para instalar dependências de cliente como a inicialização no núcleo do ASP.NET é por meio de Bower (usando *bower. JSON*, conforme mostrado acima). O uso do npm/NuGet são mostrados para demonstrar como inicialização pode ser facilmente adicionada a outros tipos de aplicativos web, incluindo versões anteriores do ASP.NET.

Se você estiver fazendo referência a suas próprias versões locais de inicialização, você precisará fazer referência a eles em todas as páginas que irá usá-la. Em produção, você deve fazer referência bootstrap usando uma CDN. No modelo de site ASP.NET padrão, o *cshtml* arquivo assim como este:

[!code-html[Main](../common/samples/WebApplication1/Views/Shared/_Layout.cshtml?highlight=9,13,51,59)]

> [!NOTE]
> Se você estiver usando qualquer um dos plug-ins do Bootstrap jQuery, você também precisará fazer referência a jQuery.

## <a name="basic-templates-and-features"></a>Recursos e modelos básicos

O modelo de inicialização mais básico é muito parecido com o *cshtml* arquivo mostrado acima e simplesmente inclui um menu básico para navegação e um local para renderizar o restante da página.

### <a name="basic-navigation"></a>Navegação básica

O modelo padrão usa um conjunto de `<div>` elementos para processar uma barra de navegação superior e o corpo principal da página. Se você estiver usando HTML5, você pode substituir o primeiro `<div>` marca com um `<nav>` marca para obter o mesmo efeito, mas com semântica mais precisa.  Este primeiro em `<div>` você pode ver, há vários outros. Primeiro, um `<div>` com uma classe de "contêiner" e, em seguida, em que, mais de dois `<div>` elementos: "navbar-cabeçalho" e "navbar-recolher".  Cabeçalho navbar div inclui um botão que será exibido quando a tela estiver abaixo de uma determinada largura mínima, mostrando 3 linhas horizontais (uma chamada "ícone de hambúrguer"). O ícone é renderizado usando puro HTML e CSS; Nenhuma imagem é necessária. Este é o código que exibe o ícone, com cada o <span> marcas de renderização de uma das barras brancas:

```html
<button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
    <span class="icon-bar"></span>
</button>
```

Ele também inclui o nome do aplicativo, que aparece no canto superior esquerdo.  O menu de navegação principal é processado pelo `<ul>` elemento dentro do segundo div e inclui links para casa, aproximadamente e entre em contato com. Links adicionais para o registro e de logon são adicionados pela linha loginpartial em 29 de linha. Abaixo do painel de navegação, o corpo principal de cada página é renderizado em outro `<div>`, marcado com as classes de "contêiner" e "conteúdo do corpo". No arquivo layout simples padrão mostrado aqui, o conteúdo da página é renderizado pela exibição específica associada com a página e, em seguida, um simples `<footer>` é adicionada ao final do `<div>` elemento.  Você pode ver como o interno sobre página aparece usando este modelo:

![Sobre a página](bootstrap/_static/about-page-wide.png)

A barra de navegação recolhida, com o botão "hambúrguer" no canto superior direito, é exibido quando a janela cai abaixo de uma determinada largura:

![sobre a página com o menu de hambúrguer](bootstrap/_static/about-page-hamburger.png)

Clicando no ícone revela os itens de menu em uma gaveta vertical que slides para baixo da parte superior da página:

![sobre a página de hambúrguer menu expandido](bootstrap/_static/about-page-hamburger-open.png)

### <a name="typography-and-links"></a>Tipografia e links

Inicialização configura tipografia básico, cores e formatação em seu arquivo CSS de link do site. Esse arquivo CSS inclui estilos de padrão para tabelas, botões, elementos de formulário, imagens e muito mais ([mais](http://getbootstrap.com/css/)). Um recurso útil é o sistema de layout de grade, abordado em seguida.

### <a name="grids"></a>Grades

Um dos recursos mais populares de inicialização é o sistema de layout de grade. Aplicativos web modernos devem evitar usar o `<table>` marca de layout, em vez disso, restringir o uso desse elemento para dados de tabela reais. Em vez disso, colunas e linhas podem ser dispostas usando uma série de `<div>` elementos e as classes CSS apropriadas. Há várias vantagens dessa abordagem, incluindo a capacidade de ajustar o layout de grade para exibir verticalmente em estreitas telas, como em telefones.

[Sistema de layout de grade da inicialização](http://getbootstrap.com/css/#grid) é baseado em doze colunas. Esse número foi escolhido porque podem ser dividido uniformemente em 1, 2, 3 ou 4 colunas e larguras de coluna podem variar para dentro de 1/12 da largura da tela vertical. Para começar a usar o sistema de layout de grade, você deve começar com um contêiner `<div>` e, em seguida, adicione uma linha `<div>`, conforme mostrado aqui:

```html
<div class="container">
    <div class="row">
        ...
    </div>
</div>
```

Em seguida, adicione adicionais `<div>` elementos para cada coluna e especifique o número de colunas que `<div>` devem ocupar (fora de 12) como parte de uma classe CSS começando com "col - md-". Por exemplo, se você quiser simplesmente ter duas colunas de tamanho igual, você usaria uma classe de "col-md-6" para cada um. Nesse caso, "md" é a abreviação de "Médio" e se refere a tamanhos de exibição de tamanho padrão de computador desktop. Há quatro opções diferentes, que você pode escolher, e cada será usada para larguras superior, a menos que substituído (para que se você quiser que o layout para ser corrigido, independentemente da largura da tela, você pode especificar apenas as classes de xs).

Prefixo da classe CSS | Camada do dispositivo | Largura
:---: | :---: | :---:
col-xs - | Telefones | < 768px
col-sm - | Tablets | > = 768px
col-md - | Áreas de trabalho | > = 992px
col-lg - | Exibe maior de área de trabalho | > = 1200 px

Ao especificar duas colunas com "col-md-6" layout resultante será duas colunas com resoluções de área de trabalho, mas essas duas colunas serão empilhadas verticalmente quando renderizado em dispositivos menores (ou uma janela de navegador mais estreita em uma área de trabalho), permitindo aos usuários exibir facilmente conteúdo sem precisar rolar horizontalmente.

Inicialização padrão será sempre um layout de coluna única, portanto você precisa apenas especificar colunas quando você quiser mais de uma coluna. A única vez em que você deseja especificar explicitamente que um `<div>` ocupem todas as 12 colunas seria substituir o comportamento de um maior nível de dispositivo. Ao especificar várias classes de camada do dispositivo, talvez seja necessário redefinir o processamento de coluna em determinados pontos. Adicionar um div clearfix que só é visível dentro de um visor de determinados conseguir isso, conforme mostrado aqui:

![grade de visor largos e estreitos](bootstrap/_static/narrow-and-wide-viewport-grid.png)

No exemplo acima, um e dois compartilham uma linha no layout "md", enquanto dois e três compartilham uma linha no layout de "xs". Sem o clearfix `<div>`, dois e três não são exibidos corretamente no modo de exibição de "xs" (Observe que apenas um, quatro e cinco é mostrados):

![grade sem usar clearfix](bootstrap/_static/grid-without-clearfix.png)

Neste exemplo, apenas uma única linha `<div>` foi usado, e ainda Bootstrap principalmente fez o certo em relação o layout e a pilha de colunas. Normalmente, você deve especificar uma linha `<div>` para cada linha horizontal requer o layout e claro, você pode aninhar grades Bootstrap dentro de uma outra. Quando você fizer isso, cada grade aninhada ocupará 100% da largura do elemento no qual ele é colocado, que pode ser subdividida usando classes de coluna.

### <a name="jumbotron"></a>Jumbotron

Se você usou os modelos do ASP.NET MVC padrão no Visual Studio 2012 ou 2013, você provavelmente já viu Jumbotron em ação. Ela se refere a uma seção grande de largura inteira de uma página que pode ser usada para exibir uma imagem de plano de fundo grandes, uma chamada para ação, um AdRotator ou elementos semelhantes. Para adicionar um jumbotron para uma página, basta adicionar uma `<div>` e dê a ele uma classe de "jumbotron", em seguida, coloque um contêiner `<div>` dentro e adicionar o seu conteúdo.  Podemos facilmente ajustar o padrão de página para usar um jumbotron para os títulos principais exibe:

![exemplo de jumbotron](bootstrap/_static/jumbotron.png)

### <a name="buttons"></a>Botões

As classes de botão padrão e as cores são mostradas na figura abaixo.

![botões com tema](bootstrap/_static/theme-buttons.png)

### <a name="badges"></a>Notificações

Selos consultem textos explicativos pequenos, geralmente numéricos ao lado de um item de navegação. Eles podem indicar um número de mensagens ou notificações de espera, ou a presença de atualizações. Especificar essas notificações é tão simple quanto adicionando um <span> que contém o texto, com a classe de "notificação":

![notificações com tema](bootstrap/_static/theme-badges.png)

### <a name="alerts"></a>Alertas

Talvez seja necessário exibir algum tipo de notificação de alerta ou para usuários do seu aplicativo. Que é onde as classes de alerta padrão são úteis.  Há quatro níveis de severidade diferente com esquemas de cores associadas:

![alertas com tema](bootstrap/_static/theme-alerts.png)

### <a name="navbars-and-menus"></a>Menus e barras de navegação

Nosso layout já inclui uma barra de navegação padrão, mas o tema de inicialização dá suporte a opções de estilo adicionais. Podemos facilmente pode optar por exibir a barra de navegação verticalmente em vez de horizontalmente se que tem preferencial, bem como a adição de subnavegação itens nos menus de atalho. Menus de navegação simples, como as faixas guia baseiam-se na parte superior do <ul> elementos. Eles podem ser criados muito simples, apenas fornecendo a eles com as classes CSS "nav" e "nav-guias":

![tabstrips com tema](bootstrap/_static/theme-tabstrips.png)

Barras de navegação são criadas da mesma forma, mas são um pouco mais complexas.  Eles começar com um `<nav>` ou `<div>` com a classe de "barra de navegação", no qual um div de contêiner contém o restante dos elementos. Nossa página inclui uma barra de navegação em seu cabeçalho já – mostrado a seguir expande simplesmente neste, adicionando suporte para um menu suspenso:

![barras de navegação com tema](bootstrap/_static/theme-navbars.png)

### <a name="additional-elements"></a>Elementos adicionais

O tema padrão também pode ser usado para apresentar tabelas HTML em um estilo bem formatado, incluindo suporte para modos de exibição distribuídos. Há rótulos com estilos que são semelhantes dos botões. Você pode criar menus suspensos personalizados que oferecem suporte a opções de estilo adicionais além do HTML padrão `<select>` elemento, junto com as barras de navegação como o nosso site de início padrão já está usando. Se você precisar de uma barra de progresso, há vários estilos para escolha, bem como a lista de grupos e painéis que incluem um título e o conteúdo.  Explore as opções adicionais do tema de inicialização padrão aqui:

[http://getbootstrap.com/Examples/Theme/](http://getbootstrap.com/examples/theme/)

## <a name="more-themes"></a>Mais temas

Você pode estender o tema de inicialização padrão, substituindo alguns ou todos os seus CSS, ajustar as cores e estilos para atender às necessidades do seu próprio aplicativo. Se você deseja iniciar a partir de um tema pronto, há vários galerias de tema disponíveis online que especializados em temas de inicialização, como WrapBootstrap.com (que tem uma variedade de temas comerciais) e Bootswatch.com (que oferece temas livres).  Alguns dos modelos disponíveis pagos fornecem uma grande quantidade de funcionalidade sobre o tema de inicialização básica, como suporte avançado para administrativas menus e painéis avançados gráficos e medidores. Um exemplo de um modelo pago popular está Inspinia, para a venda de US $18, que inclui um modelo de ASP.NET MVC5 além AngularJS e versões HTML estáticas. Abaixo está uma captura de tela de exemplo.

![Exemplo tema inspinia](bootstrap/_static/theme-inspinia.png)

Se você quiser alterar o tema de inicialização, coloque o *bootstrap.css* arquivo para o tema que você deseja no **wwwroot/css** pasta e altere as referências no *cshtml* para apontá-lo.  Altere os links para todos os ambientes:

```html
<environment names="Development">
    <link rel="stylesheet" href="~/css/bootstrap.css" />
```

```html
<environment names="Staging,Production">
    <link rel="stylesheet" href="~/css/bootstrap.min.css" />
```

Se você deseja criar seu próprio painel, você pode iniciar do exemplo livre disponível aqui: [http://getbootstrap.com/examples/dashboard/](http://getbootstrap.com/examples/dashboard/).

## <a name="components"></a>Componentes

Além desses elementos já discutidos, inicialização inclui suporte para uma variedade de [componentes internos de interface do usuário](http://getbootstrap.com/components/).

### <a name="glyphicons"></a>Glyphicons

Inicialização inclui conjuntos de ícones de Glyphicons ([http://glyphicons.com](http://glyphicons.com)), com mais de 200 ícones disponíveis gratuitamente para uso dentro de seu aplicativo da web habilitado para inicialização. Aqui está a apenas uma pequena amostra:

![Glyphicons](bootstrap/_static/theme-glyphicons.png)

### <a name="input-groups"></a>grupos de entrada

Grupos de entrada permitem agrupamento de texto adicional ou botões com um elemento de entrada, fornecendo ao usuário uma experiência mais intuitiva com:

![grupos de entrada](bootstrap/_static/input-groups.png)

### <a name="breadcrumbs"></a>Trilha

Navegação estrutural é um componente de interface do usuário comum usado para mostrar ao usuário seu histórico recente ou a profundidade da hierarquia de navegação do site. Adicioná-los facilmente aplicando a classe "trilha" a qualquer `<ol>` elemento da lista. Inclui suporte interno para paginação usando a classe "paginação" em um `<ul>` elemento dentro de um `<nav>`. Adicionar apresentações de slides inseridas responsivos e vídeo usando `<iframe>`, `<embed>`, `<video>`, ou `<object>` elementos, que inicialização será estilo automaticamente. Especifique uma taxa de proporção específica usando classes específicas, como "Inserir-responsivo-16by9".

## <a name="javascript-support"></a>Suporte a JavaScript

Biblioteca de JavaScript do Bootstrap inclui suporte a API para os componentes incluídos, permitindo que você controle de seu comportamento programaticamente dentro de seu aplicativo. Além disso, *bootstrap.js* inclui uma dúzia jQuery personalizado plug-ins, fornecer recursos adicionais como transições, caixas de diálogo modais rolagem detecção (Atualizando estilos com base no qual o usuário tem rolado no documento), comportamento de recolher, carrosséis e fixar menus para a janela para que eles não aparecem na tela. Não há espaço suficiente para abranger todos os complementos JavaScript incorporados Bootstrap – para saber mais, visite [http://getbootstrap.com/javascript/](http://getbootstrap.com/javascript/).

## <a name="summary"></a>Resumo

Inicialização fornece uma estrutura de web que pode ser usada para formatar e uma ampla variedade de sites e aplicativos de estilo de forma rápida e produtiva. Seu tipografia básica e estilos fornecem uma agradável aparência que podem ser manipulada facilmente por meio do suporte de tema personalizado, que pode ser criado manualmente ou adquirido comercialmente. Ele dá suporte a um host de componentes da web que exigiria caros controles de terceiros para realizar e dar suporte a padrões da web modernos e abertos no passado.
