---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: URLs em páginas mestras (c#) | Microsoft Docs
author: rick-anderson
description: Aborda como URLs na página mestra podem ser dividido porque o arquivo página mestre em um diretório relativo diferente que a página de conteúdo. Examina a troca de base...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: dcbd066a55fed3dd6c79e8a091e61449f85531b0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398382"
---
<a name="urls-in-master-pages-c"></a>URLs em páginas mestras (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> Aborda como URLs na página mestra podem ser dividido porque o arquivo página mestre em um diretório relativo diferente que a página de conteúdo. Examina as URLs por meio da troca de base ~ na sintaxe declarativa e usando ResolveUrl e ResolveClientUrl programaticamente. (Consulte também


## <a name="introduction"></a>Introdução

Em todos os exemplos que vimos até agora, que as páginas mestras e de conteúdo tiveram sido localizadas na pasta mesma (pasta de raiz do site). Mas não há nenhum motivo por que as páginas mestras e de conteúdo devem estar na mesma pasta. Certamente, você pode criar páginas de conteúdo em subpastas. Da mesma forma, você pode criar um `~/MasterPages/` pasta onde você coloca páginas mestras do site.

Um possível problema com o posicionamento de páginas mestras e de conteúdo em pastas diferentes envolve URLs quebradas. Se a página mestra contém URLs relativas em hiperlinks, imagens ou outros elementos, o link serão inválido para páginas de conteúdo que residem em uma pasta diferente. Neste tutorial, vamos examinar a fonte desse problema, bem como soluções alternativas.

## <a name="the-problem-with-relative-urls"></a>O problema com URLs relativas

Uma URL em uma página da web deve ser um *URL relativa* se o local do recurso que ele aponta é relativo ao local da página da web na estrutura de pastas do site. Qualquer URL que não comece com uma barra à esquerda (`/`) ou um protocolo (como `http://`) é relativo porque ele é resolvido pelo navegador baseado no local da página da web que contém a URL.

Por exemplo, nosso site tem um `~/Images/` pasta com um único arquivo de imagem, `PoweredByASPNET.gif`. O arquivo de página mestra `Site.master` tem um `<img>` elemento o `footerContent` região com a seguinte marcação:


[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

O `src` valor no atributo o `<img>` elemento é uma URL relativa, porque não começa com `/` ou `http://`. Em resumo, o `src` valor de atributo informa ao navegador para examinar as `Images` subpasta para um arquivo chamado `PoweredByASPNET.gif`.

Ao visitar uma página de conteúdo, a marcação acima é enviada diretamente para o navegador. Reserve um tempo para visitar `About.aspx` e exibir o código-fonte HTML enviado ao navegador. Você descobrirá que a mesma marcação exata na página mestra foi enviada para o navegador.


[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

Se a página de conteúdo está na pasta raiz (como está `About.aspx`) tudo está funcionando conforme o esperado porque não há um `Images` subpasta relativa à pasta raiz. No entanto, as coisas dividir se a página de conteúdo estiver em uma pasta diferente que a página mestra. Para ilustrar isso, crie uma subpasta chamada `Admin`. Em seguida, adicione uma página de conteúdo denominada `Default.aspx` para o `Admin` pasta, certificando-se de associar a nova página para o `Site.master` página mestra.

> [!NOTE]
> No [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial, criamos uma classe de página de base personalizada chamada `BasePage` que definir o título da página de conteúdo automaticamente (se ele não foi explicitamente atribuído). Não se esqueça de ter a classe de code-behind da página recém-criada derivam `BasePage` para que ele possa utilizar essa funcionalidade.


Depois que você criou esta página de conteúdo, seu Gerenciador de soluções deve ser semelhante a Figura 1.


![Uma nova pasta e a página do ASP.NET foram adicionados ao projeto](urls-in-master-pages-cs/_static/image1.png)

**Figura 01**: uma nova pasta e a página do ASP.NET foram adicionados ao projeto


Em seguida, atualize o `Web.sitemap` arquivo para incluir um novo `<siteMapNode>` entrada para esta lição. O XML a seguir mostra todo `Web.sitemap` marcação, que agora inclui a adição de um terceiro `<siteMapNode>` elemento.


[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

Recém-criado `Default.aspx` página deve ter quatro controles de conteúdo correspondente para os quatro ContentPlaceHolders em `Site.master`. Adicione algum texto para a referência de controle de conteúdo a `MainContent` ContentPlaceHolder e, em seguida, visite a página por meio de um navegador. Como mostra a Figura 2, o navegador não é possível localizar o `PoweredByASPNET.gif` arquivo de imagem. O que está acontecendo aqui?

O `~/Admin/Default.aspx` página de conteúdo é enviada o mesmo HTML para o `footerContent` região como era o `About.aspx` página:


[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

Porque o `<img>` prvku `src` atributo é uma URL relativa, o navegador tenta procurar um `Images` pasta em relação ao local da pasta da página da web. Em outras palavras, o navegador está procurando o arquivo de imagem `Admin/Images/PoweredByASPNET.gif`.


[![Não é possível encontrar o arquivo de imagem PoweredByASPNET.gif](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**Figura 02**: O `PoweredByASPNET.gif` imagem do arquivo não pode ser encontrado ([clique para exibir a imagem em tamanho normal](urls-in-master-pages-cs/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Substituindo URLs relativas com URLs absolutas

O oposto de uma URL relativa é um *URL absoluta*, que é aquele que começa com uma barra invertida (`/`) ou um protocolo como `http://`. Como uma URL absoluta Especifica o local de um recurso de um ponto fixo conhecido, a mesma URL absoluta é válida em qualquer página da web, independentemente do local da página da web na estrutura de pastas do site.

Para corrigir a imagem quebrada, mostrada na Figura 2, precisamos atualizar o `<img>` do elemento `src` para que ele use uma URL absoluta, em vez de uma relativa do atributo. Para determinar a URL absoluta correta, visite uma das páginas da web em seu site e examine a barra de endereços. Como mostra a barra de endereços na Figura 2, o caminho totalmente qualificado para o aplicativo web é `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`. Portanto, é possível atualizar o `<img>` do elemento `src` de atributo para qualquer uma das duas URLs absolutas a seguir:

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

Reserve um tempo para atualizar o `<img>` do elemento `src` atributo para uma URL absoluta usando uma das formas mostradas acima e, em seguida, visite o `~/Admin/Default.aspx` página por meio de um navegador. Neste momento, o navegador será corretamente localizar e exibir o `PoweredByASPNET.gif` arquivo de imagem (veja a Figura 3).


[![A imagem de PoweredByASPNET.gif é exibido agora](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**Figura 03**: O `PoweredByASPNET.gif` imagem é agora exibido ([clique para exibir a imagem em tamanho normal](urls-in-master-pages-cs/_static/image7.png))


Enquanto o hard-coding na URL absoluto funciona, ele acople seu HTML para o servidor do site e local de pasta, que pode ser alterado. Usando uma URL absoluta do formulário `http://localhost:3908/...` é frágil porque o número da porta anterior `localhost` é selecionado automaticamente sempre que o servidor de Web de desenvolvimento de ASP.NET interno do Visual Studio é iniciado. Da mesma forma, o `http://localhost` parte só é válida durante o teste local. Depois que o código é implantado em um servidor de produção, a URL base será alterado para algo diferente, como `http://www.yourserver.com`. A URL absoluta no formulário `/ASPNET_MasterPages_Tutorial_04_CS/...` também é prejudicada fragilidade mesma porque muitas vezes, esse caminho de aplicativo é diferente entre os servidores de desenvolvimento e produção.

A boa notícia é que o ASP.NET oferece um método para gerar uma URL relativa válida no tempo de execução.

## <a name="usingandresolveclienturl"></a>Usando`~`e`ResolveClientUrl`

Em vez de codificar uma URL absoluta, ASP.NET permite que os desenvolvedores de páginas usar o til (`~`) para indicar a raiz do aplicativo web. Por exemplo, neste tutorial, usei a notação `~/Admin/Default.aspx` no texto para fazer referência a `Default.aspx` página no `Admin` pasta. O `~` indica que o `Admin` pasta é uma subpasta da raiz do aplicativo da web.

O `Control` da classe [ `ResolveClientUrl` método](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) usa uma URL e modifica-a uma URL relativa apropriada para a página da web no qual reside o controle. Por exemplo, chamar `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` partir `About.aspx` retorna `Images/PoweredByASPNET.gif`. Chamá-lo partir `~/Admin/Default.aspx`, no entanto, retorna `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Como todos os controles de servidor ASP.NET derivam a `Control` classe, todos os controles de servidor tenham acesso ao `ResolveClientUrl` método. Até mesmo a `Page` classe deriva de `Control` classe, que significa que você pode usar este método diretamente de classes code-behind de suas páginas ASP.NET.


### <a name="usingin-the-declarative-markup"></a>Usando`~`na marcação declarativa

Vários controles da Web do ASP.NET incluem propriedades relacionadas à URL: o controle de hiperlink tem um `NavigateUrl` propriedade; a imagem de controle tem um `ImageUrl` propriedade; e assim por diante. Quando renderizada, esses controles passam seus valores de propriedade relacionadas à URL para `ResolveClientUrl`. Consequentemente, se essas propriedades contêm uma `~` para indicar a raiz do aplicativo web, a URL será modificada para uma URL relativa válida.

Tenha em mente que somente os controles de servidor ASP.NET transform o `~` em suas propriedades relacionadas à URL. Se um `~` aparece na marcação HTML estática, como `<img src="~/Images/PoweredByASPNET.gif" />`, o mecanismo do ASP.NET envia a `~` no navegador junto com o restante do conteúdo HTML. O navegador supõe que o `~` faz parte da URL. Por exemplo, se o navegador recebe a marcação `<img src="~/Images/PoweredByASPNET.gif" />` pressupõe que há uma subpasta nomeada `~` com uma subpasta `Images` que contém o arquivo de imagem `PoweredByASPNET.gif`.

Para corrigir a marcação de imagem na `Site.master`, substitua a `<img>` elemento com um controle de Web de imagem do ASP.NET. Defina o controle de Web de imagem `ID` para `PoweredByImage`, sua `ImageUrl` propriedade `~/Images/PoweredByASPNET.gif`e seu `AlternateText` propriedade como "Funciona com o ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

Depois de fazer essa alteração para a página mestra, revisitar o `~/Admin/Default.aspx` página novamente. Desta vez o `PoweredByASPNET.gif` arquivo de imagem aparece na página (consulte a Figura 3). Quando a Web de imagem de controle é renderizado ele usa o `ResolveClientUrl` método para resolver seu `ImageUrl` valor da propriedade. Na `~/Admin/Default.aspx` o `ImageUrl` é convertida em uma URL relativa apropriado, como o seguinte trecho de HTML fonte mostra:


[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> Além de ser usado nas propriedades de controle de Web baseado em URL, o `~` também pode ser usado ao chamar o `Response.Redirect` e `Server.MapPath` métodos, entre outros. Além disso, o `ResolveClientUrl` método pode ser chamado diretamente de um ASP.NET ou marcação declarativa da página mestra, se necessário, consulte [Fritz Onion](https://www.pluralsight.com/blogs/fritz/)da entrada de blog [Using `ResolveClientUrl` na marcação](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Correção da página mestra restantes URLs relativas

Além de `<img>` elemento no `footerContent` que acabei de corrigir, a página mestra contém uma URL relativa mais que exige a nossa atenção. O `topContent` região inclui o link "Mestre páginas de tutoriais," que aponta para `Default.aspx`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

Porque essa URL for relativa, ele enviará ao usuário o `Default.aspx` página na pasta da página de conteúdo que estão sendo visitado. Para ter esse link sempre aponte para `Default.aspx` na pasta raiz, precisamos substituir o `<a>` elemento com um hiperlink Web controlam para que podemos usar o `~` notação.

Remover o `<a>` marcação de elemento e adicione um controle de hiperlink em seu lugar. Definir o HyperLink `ID` para `lnkHome`, sua `NavigateUrl` propriedade `~/Default.aspx`e seu `Text` propriedade como "Tutoriais de páginas mestras".


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

É só isso! Neste ponto, todas as URLs em nossa página mestre corretamente baseiam-se durante a renderização por uma página de conteúdo, independentemente de quais pastas, a página mestra e a página de conteúdo estão localizadas em.

### <a name="automatic-url-resolution-in-theheadsection"></a>Resolução automática de URL no`<head>`seção

No [ *criando um Layout de todo o Site usando páginas mestras* ](creating-a-site-wide-layout-using-master-pages-cs.md) tutorial adicionamos um `<link>` para o `Styles.css` arquivo no `<head>` região:


[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

Enquanto o `<link>` do elemento `href` atributo for relativo, ele é automaticamente convertido em um caminho apropriado em tempo de execução. Como discutimos na [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) tutorial, o `<head>` região é, na verdade, um controle do lado do servidor, que permite modificar a conteúdo de seus controles internos quando ele é renderizado.

Para verificar isso, examine o `~/Admin/Default.aspx` página e exibir o código-fonte HTML enviado ao navegador. Como o trecho a seguir ilustra, o `<link>` prvku `href` atributo tiver sido modificado automaticamente para uma URL relativa apropriada, `../Styles.css`.


[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>Resumo

Páginas mestras com muita frequência incluem links, imagens e outros recursos externos que devem ser especificados através de uma URL. Porque a página mestra e páginas de conteúdo podem não existir na mesma pasta, é importante abstain do uso de URLs relativas. Embora seja possível usar URLs absolutas embutidas, fazendo tão firmemente associa a URL absoluta para o aplicativo web. Se a URL absoluta for alterado – como geralmente faz ao mover ou implantar um aplicativo web – você terá que não se esqueça de voltar e atualizar as URLs absolutas.

A abordagem ideal é usar o til (`~`) para indicar a raiz do aplicativo. Mapeiam de controles da Web ASP.NET que contêm propriedades relacionadas à URL a `~` para a raiz do aplicativo em tempo de execução. Internamente, os controles da Web usam o `Control` da classe `ResolveClientUrl` método para gerar uma URL relativa válida. Esse método é público e está disponível de cada controle de servidor (incluindo o `Page` classe), portanto, você pode usá-lo por meio de programação de suas classes code-behind, se necessário.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Páginas mestras no ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Troca de base de URL em uma página mestra](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Usando `ResolveClientUrl` na marcação](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 3.5 in 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
> [Próximo](control-id-naming-in-content-pages-cs.md)
