---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
title: URLs em páginas mestras (VB) | Microsoft Docs
author: rick-anderson
description: Aborda como URLs na página mestra podem quebrar porque o arquivo página mestre em um diretório relativo diferente que a página de conteúdo. Examina a alteração da base...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2008
ms.topic: article
ms.assetid: 43d1e83c-0092-4dcf-977c-e709c4dce7c3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: e1d4b2d66bedfb5f3d7d8c61265944a82605e77e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="urls-in-master-pages-vb"></a>URLs em páginas mestras (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_VB.pdf)

> Aborda como URLs na página mestra podem quebrar porque o arquivo página mestre em um diretório relativo diferente que a página de conteúdo. Analisa URLs por meio de alteração ~ na sintaxe declarativa e usando ResolveUrl e ResolveClientUrl programaticamente. (Consulte também


## <a name="introduction"></a>Introdução

Em todos os exemplos que vimos até agora, que as páginas mestras e de conteúdo tenham sido colocadas na mesma pasta (pasta de raiz do site). Mas não há nenhum motivo por que as páginas mestras e de conteúdo devem estar na mesma pasta. Certamente você pode criar páginas de conteúdo em subpastas. Da mesma forma, você pode criar um `~/MasterPages/` pasta onde você coloca páginas mestras do site.

Um possível problema com a colocação de páginas mestras e de conteúdo em pastas diferentes envolve URLs quebradas. Se a página mestra contém URLs relativas em hiperlinks, imagens ou outros elementos, o vínculo será inválido para páginas de conteúdo que residem em uma pasta diferente. Neste tutorial, podemos examinar a origem desse problema, bem como soluções alternativas.

## <a name="the-problem-with-relative-urls"></a>O problema com URLs relativas

Uma URL em uma página da web é considerada um *URL relativa* se o local do recurso aponta para é relativo ao local da página da web na estrutura de pasta do site. Qualquer URL que não começa com uma barra à esquerda (`/`) ou um protocolo (como `http://`) é relativo porque ele está resolvido pelo navegador com base no local da página da web que contém a URL.

Por exemplo, o site tem um `~/Images/` pasta com um único arquivo de imagem, `PoweredByASPNET.gif`. O arquivo de página mestra `Site.master` tem um `<img>` elemento o `footerContent` região com a seguinte marcação:


[!code-html[Main](urls-in-master-pages-vb/samples/sample1.html)]

O `src` valor no atributo de `<img>` elemento é uma URL relativa porque não começa com `/` ou `http://`. Em resumo, o `src` o valor do atributo informa ao navegador para examinar o `Images` subpasta para um arquivo denominado `PoweredByASPNET.gif`.

Quando visita uma página de conteúdo, a marcação acima é enviada diretamente para o navegador. Reserve um tempo para visitar `About.aspx` e exibir o código-fonte HTML enviada para o navegador. Você descobrirá que a mesma marcação exata na página mestra foi enviada para o navegador.


[!code-html[Main](urls-in-master-pages-vb/samples/sample2.html)]

Se a página de conteúdo está na pasta raiz (como é `About.aspx`) tudo funciona conforme o esperado porque não há um `Images` subpasta relativo à pasta raiz. No entanto, as coisas dividir se a página de conteúdo está em uma pasta diferente da página mestra. Para ilustrar isso, crie uma subpasta chamada `Admin`. Em seguida, adicione uma página de conteúdo chamada `Default.aspx` para o `Admin` pasta, certificando-se de associar a nova página para o `Site.master` página mestra.

> [!NOTE]
> No [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial criamos uma classe de página de base personalizada chamada `BasePage` que definir o título da página de conteúdo automaticamente (se ele não foi explicitamente atribuído). Não se esqueça de ter a classe de code-behind da página recém-criado derivam `BasePage` para que ele pode usar essa funcionalidade.


Depois que você criou esta página de conteúdo, o Gerenciador de soluções deve ser semelhante à Figura 1.


![Uma nova pasta e a página ASP.NET foram adicionados ao projeto](urls-in-master-pages-vb/_static/image1.png)

**Figura 01**: uma nova pasta e a página ASP.NET foram adicionados ao projeto


Em seguida, atualize o `Web.sitemap` arquivo para incluir um novo `<siteMapNode>` entrada desta lição. O seguinte XML mostra completo `Web.sitemap` marcação, que agora inclui a adição de um terceiro `<siteMapNode>` elemento.


[!code-xml[Main](urls-in-master-pages-vb/samples/sample3.xml)]

Recém-criado `Default.aspx` página deve ter quatro controles de conteúdo correspondente as quatro ContentPlaceHolders em `Site.master`. Adicione um texto para o controle de conteúdo referenciando o `MainContent` ContentPlaceHolder e, em seguida, visite a página por meio de um navegador. Como mostra a Figura 2, o navegador não é possível localizar o `PoweredByASPNET.gif` arquivo de imagem. O que está acontecendo aqui?

O `~/Admin/Default.aspx` página de conteúdo é enviada mesmo HTML para o `footerContent` região, como era o `About.aspx` página:


[!code-html[Main](urls-in-master-pages-vb/samples/sample4.html)]

Porque o `<img>` do elemento `src` atributo é uma URL relativa, o navegador tenta procurar um `Images` pasta relativo ao local da pasta da página da web. Em outras palavras, o navegador está procurando o arquivo de imagem `Admin/Images/PoweredByASPNET.gif`.


[![Não é possível encontrar o arquivo de imagem PoweredByASPNET.gif](urls-in-master-pages-vb/_static/image3.png)](urls-in-master-pages-vb/_static/image2.png)

**Figura 02**: O `PoweredByASPNET.gif` imagem do arquivo não pode ser encontrado ([clique para exibir a imagem em tamanho normal](urls-in-master-pages-vb/_static/image4.png))


### <a name="replacing-relative-urls-with-absolute-urls"></a>Substituindo URLs relativas com URLs absolutas

É o oposto de uma URL relativa um *URL absoluta*, que é um que começa com uma barra invertida (`/`) ou um protocolo, como `http://`. Como uma URL absoluta Especifica o local de um recurso de um ponto fixo conhecido, a mesma URL absoluta é válida em qualquer página da web, independentemente do local da página da web na estrutura de pasta do site.

Para corrigir a imagem quebrada mostrada na Figura 2, precisamos atualizar o `<img>` do elemento `src` para que ele usa uma URL absoluta, em vez de uma relação de atributo. Para determinar a URL absoluta correta, visite uma das páginas da web no seu site e examine a barra de endereços. Como mostra a barra de endereços na Figura 2, o caminho totalmente qualificado para o aplicativo web é `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/`. Portanto, podemos pode atualizar o `<img>` do elemento `src` atributo para qualquer uma das duas URLs absoluta a seguir:

- `/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_VB/Images/PoweredByASPNET.gif`

Reserve um tempo para atualizar o `<img>` do elemento `src` de atributo para uma URL absoluta usando uma das formas mostradas acima e, em seguida, visite o `~/Admin/Default.aspx` página através de um navegador. Neste momento, o navegador será corretamente localizar e exibir o `PoweredByASPNET.gif` arquivo de imagem (consulte a Figura 3).


[![A imagem de PoweredByASPNET.gif é exibido agora](urls-in-master-pages-vb/_static/image6.png)](urls-in-master-pages-vb/_static/image5.png)

**Figura 03**: O `PoweredByASPNET.gif` imagem é exibida agora ([clique para exibir a imagem em tamanho normal](urls-in-master-pages-vb/_static/image7.png))


Enquanto a codificação de URL absoluto funciona, agrupa rigidamente HTML para o servidor do site e local de pasta, que pode ser alterado. Usando uma URL absoluta do formulário `http://localhost:3908/...` é frágil porque o número de porta anterior localhost é selecionado automaticamente sempre que o servidor de Web de desenvolvimento de ASP.NET interno do Visual Studio é iniciado. Da mesma forma, o `http://localhost` parte é válida apenas quando testes localmente. Quando o código é implantado em um servidor de produção, a URL base será alterado para algo diferente, como `http://www.yourserver.com`. A URL absoluta no formulário `/ASPNET_MasterPages_Tutorial_04_VB/...` também é prejudicada a mesmo fragilidade porque muitas vezes, esse caminho de aplicativo é diferente entre os servidores de desenvolvimento e produção.

A boa notícia é que o ASP.NET oferece um método para gerar uma URL relativa válida no tempo de execução.

## <a name="usingandresolveclienturl"></a>Usando`~`e`ResolveClientUrl`

Em vez de codificar uma URL absoluta, ASP.NET permite aos desenvolvedores de página usar o til (`~`) para indicar a raiz do aplicativo web. Por exemplo, anteriormente neste tutorial, usei a notação `~/Admin/Default.aspx` no texto para referir-se a `Default.aspx` página o `Admin` pasta. O `~` indica que o `Admin` pasta é uma subpasta da raiz do aplicativo da web.

O `Control` da classe [ `ResolveClientUrl` método](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) usa uma URL e modifica a uma URL relativa apropriada para a página da web no qual reside o controle. Por exemplo, chamar `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` de `About.aspx` retorna `Images/PoweredByASPNET.gif`. Chamando-o de `~/Admin/Default.aspx`, no entanto, retorna `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Como todos os controles de servidor ASP.NET derivam do `Control` classe, todos os controles de servidor tem acesso para o `ResolveClientUrl` método. Até mesmo o `Page` classe deriva de `Control` classe, que significa que você pode usar este método diretamente da classe code-behind de suas páginas ASP.NET.


### <a name="usingin-the-declarative-markup"></a>Usando`~`na marcação declarativa

Vários controles da Web do ASP.NET incluem propriedades relacionadas a URL: o controle de hiperlink tem um `NavigateUrl` propriedade; a imagem de controle tem um `ImageUrl` propriedade; e assim por diante. Quando renderizados, esses controles transmitir seus valores de propriedade relacionados à URL para `ResolveClientUrl`. Consequentemente, se essas propriedades contêm um `~` para indicar a raiz do aplicativo web, a URL será modificada para uma URL relativa válida.

Tenha em mente que somente os controles de servidor ASP.NET transform o `~` em suas propriedades de URL. Se um `~` aparece na marcação HTML estática, como `<img src="~/Images/PoweredByASPNET.gif" />`, o mecanismo do ASP.NET envia o `~` no navegador junto com o restante do conteúdo HTML. O navegador supõe que o `~` faz parte da URL. Por exemplo, se o navegador recebe a marcação `<img src="~/Images/PoweredByASPNET.gif" />` ele assume que há uma subpasta chamada `~` com uma subpasta `Images` que contém o arquivo de imagem `PoweredByASPNET.gif`.

Para corrigir a marcação de imagem em `Site.master`, substituir o `<img>` elemento com um controle da Web do ASP.NET imagem. Definir o controle de imagem Web `ID` para `PoweredByImage`, sua `ImageUrl` propriedade `~/Images/PoweredByASPNET.gif`e sua `AlternateText` propriedade como "Funciona com o ASP.NET!"


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample5.aspx)]

Depois de fazer essa alteração para a página mestra, revisitar o `~/Admin/Default.aspx` página novamente. Neste momento o `PoweredByASPNET.gif` arquivo de imagem aparece na página (consulte a Figura 3). Quando Web imagem do controle é processado, usa o `ResolveClientUrl` método para resolver seu `ImageUrl` o valor da propriedade. Em `~/Admin/Default.aspx` o `ImageUrl` é convertido em uma URL relativa apropriado, como o seguinte trecho de HTML fonte mostra:


[!code-html[Main](urls-in-master-pages-vb/samples/sample6.html)]

> [!NOTE]
> Além de ser usado nas propriedades de controle da Web baseado em URL, o `~` também pode ser usado ao chamar o `Response.Redirect` e `Server.MapPath` métodos, entre outros. Além disso, o `ResolveClientUrl` método pode ser chamado diretamente de uma ASP.NET ou declarativo da página mestra, se necessário, consulte [Fritz Onion](https://www.pluralsight.com/blogs/fritz/)da entrada de blog [usando `ResolveClientUrl` na marcação](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).


## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Correção da página mestra restantes URLs relativas

Além de `<img>` elemento o `footerContent` que corrigimos apenas, a página mestra contém uma URL relativa mais que exige a atenção do nosso. O `topContent` região inclui o link "Mestre páginas tutoriais," que aponta para `Default.aspx`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample7.html)]

Como essa URL é relativa, ele enviará o usuário o `Default.aspx` página na pasta visitando a página de conteúdo. Para que este link sempre apontar para `Default.aspx` na pasta raiz, é preciso substituir o `<a>` elemento com um hiperlink Web controlar a forma que podemos usar o `~` notação.

Remover o `<a>` marcação de elemento e adicionar um controle de hiperlink em seu lugar. Definir o hiperlink `ID` para `lnkHome`, sua `NavigateUrl` propriedade `~/Default.aspx`e sua `Text` propriedade como "Tutoriais de páginas mestra".


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample8.aspx)]

É só isso! Neste ponto, todos os URLs em nossa página mestre são baseados corretamente durante a renderização por uma página de conteúdo, independentemente de quais pastas, a página mestra e a página de conteúdo estão localizados em.

### <a name="automatic-url-resolution-in-theheadsection"></a>Resolução automática de URL a`<head>`seção

No [ *criando um Layout de todo o Site usando páginas mestras* ](creating-a-site-wide-layout-using-master-pages-vb.md) tutorial adicionamos um `<link>` para o `Styles.css` arquivo o `<head>` região:


[!code-aspx[Main](urls-in-master-pages-vb/samples/sample9.aspx)]

Enquanto o `<link>` do elemento `href` atributo for relativo, ele é automaticamente convertido em um caminho correto em tempo de execução. Conforme abordado no [ *especificando o título, marcas Meta e outros cabeçalhos de HTML na página mestra* ](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial, o `<head>` região é, na verdade, um controle de servidor, que permite que você modifique o conteúdo de seus controles internos quando ele for renderizado.

Para verificar isso, revisitar o `~/Admin/Default.aspx` página e exibir o código-fonte HTML enviada para o navegador. Como mostra o trecho a seguir, o `<link>` do elemento `href` atributo foi modificado automaticamente para uma URL relativa apropriado, `../Styles.css`.


[!code-html[Main](urls-in-master-pages-vb/samples/sample10.html)]

## <a name="summary"></a>Resumo

Páginas mestras frequentemente incluem links, imagens e outros recursos externos que devem ser especificados por meio de uma URL. Porque a página mestra e páginas de conteúdo podem não existir na mesma pasta, é importante abstain de usar URLs relativas. Embora seja possível usar URLs absolutas codificadas, fazendo assim rigidamente associa a URL absoluta para o aplicativo web. Se a URL absoluta altera - como geralmente quando mover ou implantar um aplicativo web - você terá de lembrar para voltar e atualizar as URLs absolutas.

A abordagem ideal é usar o til (`~`) para indicar a raiz do aplicativo. Mapeiam de controles da Web do ASP.NET que contêm propriedades relacionadas a URL de `~` para a raiz do aplicativo em tempo de execução. Internamente, os controles da Web usam o `Control` da classe `ResolveClientUrl` método para gerar uma URL relativa válida. Esse método é público e disponível de cada controle de servidor (incluindo o `Page` classe), portanto, você pode usá-lo por meio de programação de sua classe code-behind, se necessário.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Páginas mestras no ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Alteração da URL base em uma página mestra](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Usando `ResolveClientUrl` na marcação](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 3.5 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
> [Próximo](control-id-naming-in-content-pages-vb.md)
