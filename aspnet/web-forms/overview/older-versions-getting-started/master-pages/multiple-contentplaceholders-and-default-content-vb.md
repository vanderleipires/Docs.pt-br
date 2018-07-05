---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: Vários ContentPlaceHolders e conteúdo padrão (VB) | Microsoft Docs
author: rick-anderson
description: Examina como adicionar vários espaços reservados conteúdo a uma página mestra, bem como especificar o conteúdo padrão em que o conteúdo de espaços reservados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: 5425d59ac1ccec46617400601655ae72f860cab1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395108"
---
<a name="multiple-contentplaceholders-and-default-content-vb"></a>Vários ContentPlaceHolders e conteúdo padrão (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> Examina como adicionar vários espaços reservados conteúdo a uma página mestra, bem como especificar o conteúdo padrão em que o conteúdo de espaços reservados.


## <a name="introduction"></a>Introdução

No tutorial anterior, examinamos como mestre páginas habilitar os desenvolvedores do ASP.NET para criar um layout consistente de todo o site. Páginas mestras definem a marcação que é comum a todas as suas páginas de conteúdo e regiões que são personalizáveis em uma base de página por página. No tutorial anterior, criamos uma página mestra simple (`Site.master`) e duas páginas de conteúdo (`Default.aspx` e `About.aspx`). Nossa página mestre consistiu em dois ContentPlaceHolders denominados `head` e `MainContent`, que estavam no `<head>` elemento e o formulário da Web, respectivamente. Enquanto as páginas de conteúdo cada tinham dois controles de conteúdo, especificamos apenas uma marcação para aquele correspondente ao `MainContent`.

Conforme evidenciado pelos dois controles ContentPlaceHolder `Site.master`, uma página mestra pode conter vários ContentPlaceHolders. Além disso, a página mestra pode especificar uma marcação padrão para os controles de ContentPlaceHolder. Uma página de conteúdo, em seguida, pode opcionalmente especificar sua própria marcação ou usar a marcação padrão. Neste tutorial é examinar o uso de vários controles de conteúdo na página mestra e veja como definir a marcação padrão nos controles ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Etapa 1: Adicionando controles ContentPlaceHolder adicionais para a página mestra

Muitos designs de site contêm várias áreas na tela que são personalizadas em uma base de página por página. `Site.master`, a página mestra que criamos no tutorial anterior, contém um único ContentPlaceHolder dentro do formulário da Web denominado `MainContent`. Especificamente, este ContentPlaceHolder está localizado dentro de `mainContent` `<div>` elemento.

A Figura 1 mostra `Default.aspx` quando visualizado por meio de um navegador. A região circulada em vermelho é a marcação de específico da página correspondente ao `MainContent`.


[![A região dentro de um círculo mostra a área atualmente personalizáveis em uma base de página por página](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**Figura 01**: A região circulados mostra a área atualmente personalizáveis em uma base de página por página ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))


Imagine que, além da região mostrada na Figura 1, também precisamos adicionar itens de específico da página para a coluna esquerda abaixo as lições e notícias seções. Para fazer isso, adicionamos outro controle ContentPlaceHolder para a página mestra. Para acompanhar, abra o `Site.master` página no Visual Web Developer mestra e, em seguida, arraste um controle de ContentPlaceHolder da caixa de ferramentas para o designer após a seção notícias. Defina o ContentPlaceHolder `ID` para `LeftColumnContent`.


[![Adicionar um controle de ContentPlaceHolder a coluna da esquerda da página mestra](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**Figura 02**: adicionar um controle de ContentPlaceHolder a coluna da esquerda da página mestra ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))


Com a adição do `LeftColumnContent` ContentPlaceHolder para a página mestra, podemos definir o conteúdo para esta região em uma base de página por página, incluindo o conteúdo de um controle na página cuja `ContentPlaceHolderID` é definido como `LeftColumnContent`. Vamos examinar esse processo na etapa 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Etapa 2: Definir o conteúdo para o novo ContentPlaceHolder nas páginas de conteúdo

Ao adicionar uma nova página de conteúdo para o site, o Visual Web Developer cria automaticamente um conteúdo de controle na página para cada ContentPlaceHolder na página mestra selecionada. Depois de adicionar um a `LeftColumnContent` ContentPlaceHolder à nossa página mestre na etapa 1, o novo ASP.NET páginas serão agora tem três controles de conteúdo.

Para ilustrar isso, adicione uma nova página de conteúdo para o diretório raiz chamado `MultipleContentPlaceHolders.aspx` que está associado a `Site.master` página mestra. O Visual Web Developer cria esta página com a seguinte marcação declarativa:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

Insira algum conteúdo para a referência de controle de conteúdo a `MainContent` ContentPlaceHolders (`Content2`). Em seguida, adicione a seguinte marcação para o `Content3` controle de conteúdo (que faz referência a `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

Depois de adicionar essa marcação, visite a página por meio de um navegador. Como mostra a Figura 3, a marcação é colocado no `Content3` controle de conteúdo é exibido na coluna à esquerda, abaixo da seção de notícias (circulada em vermelho). A marcação é colocado no `Content2` é exibido na parte direita da página (circulada em azul).


[![A coluna à esquerda agora inclui conteúdo específico da página abaixo da seção de notícias](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**Figura 03**: O esquerda coluna agora inclui específico da página conteúdo abaixo a seção notícias ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Como definir o conteúdo em páginas de conteúdo existentes

Criando uma nova página de conteúdo automaticamente incorpora o controle ContentPlaceHolder que adicionamos na etapa 1. Mas nossas duas páginas de conteúdo existentes - `About.aspx` e `Default.aspx` -não tem um controle de conteúdo para o `LeftColumnContent` ContentPlaceHolder. Para especificar o conteúdo para este ContentPlaceHolder nessas duas páginas existentes, precisamos adicionar um controle de conteúdo sozinhos.

Ao contrário da maioria dos controles da Web do ASP.NET, as ferramentas do Visual Web Developer não inclui um item de controle de conteúdo. É possível digitar manualmente na marcação declarativa de conteúdo do controle no modo de exibição de fonte, mas uma abordagem mais fácil e rápida é usar o modo de exibição de Design. Abra o `About.aspx` página e alternar para o modo de exibição de Design. Como ilustra a Figura 4, o `LeftColumnContent` ContentPlaceHolder aparece na exibição de Design; se você passar o mouse sobre ele, o título exibido lê: "LeftColumnContent (mestre)". A inclusão de "Master" no título indica que não há nenhum controle de conteúdo definido na página para este ContentPlaceHolder. Se houver um controle de conteúdo para ContentPlaceHolder, como no caso de `MainContent`, lerá o título: "*ContentPlaceHolderID* (personalizada)."

Para adicionar um controle de conteúdo para o `LeftColumnContent` ContentPlaceHolder para `About.aspx`, expanda a marca inteligente do ContentPlaceHolder e clique no link criar conteúdo personalizado.


[![Modo de Design para About mostra o LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**Figura 04**: O modo de Design para `About.aspx` mostra o `LeftColumnContent` ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))


Ao clicar no link criar conteúdo personalizado gera o necessário controle na página e conjuntos de conteúdo seu `ContentPlaceHolderID` propriedade para o ContentPlaceHolder `ID`. Por exemplo, clicar no link criar conteúdo personalizado para `LeftColumnContent` região no `About.aspx` adiciona a seguinte marcação declarativa à página:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>A omissão de controles de conteúdo

ASP.NET não exige que todas as páginas de conteúdo incluem controles de conteúdo para cada ContentPlaceHolder definido na página mestra. Se um controle de conteúdo for omitido, o mecanismo do ASP.NET usa a marcação definida dentro de ContentPlaceHolder na página mestra. Essa marcação é conhecida como o ContentPlaceHolder *conteúdo padrão* e é útil em cenários em que o conteúdo para alguma região é comum entre a maioria das páginas, mas precisa ser personalizado para um pequeno número de páginas. Etapa 3 explora especificando conteúdo padrão na página mestra.

No momento, `Default.aspx` contém dois controles de conteúdo para o `head` e `MainContent` ContentPlaceHolders; ele não tem um controle de conteúdo para `LeftColumnContent`. Consequentemente, quando `Default.aspx` é renderizado o `LeftColumnContent` conteúdo de padrão do ContentPlaceHolder é usado. Como temos ainda definir qualquer conteúdo padrão para esse ContentPlaceHolder, o efeito líquido é que nenhuma marcação é emitida para esta região. Para verificar esse comportamento, visite `Default.aspx` através de um navegador. Como mostra a Figura 5, nenhuma marcação é emitida na coluna à esquerda, abaixo da seção de notícias.


[![Nenhum conteúdo é renderizado para o LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**Figura 05**: sem conteúdo é renderizado para o `LeftColumnContent` ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Etapa 3: Especificar o conteúdo padrão na página mestra

Alguns designs de site incluem uma região cujo conteúdo é o mesmo para todas as páginas no site, exceto para uma ou duas exceções. Considere um site que oferece suporte a contas de usuário. Um site requer uma página de logon onde visitantes podem inserir suas credenciais para entrar no site. Para agilizar o processo de entrada, os designers de site podem incluir caixas de texto nome de usuário e senha no canto superior esquerdo de cada página para permitir que usuários se conectem sem precisar explicitamente, visite a página de logon. Essas caixas de texto nome de usuário e senha são úteis para a maioria das páginas, eles são redundantes na página de logon, que já contém caixas de texto para as credenciais do usuário.

Para implementar esse design, você pode criar um controle ContentPlaceHolder no canto superior esquerdo da página mestra. Cada página que o necessário para exibir as caixas de texto nome de usuário e senha no seu canto superior esquerdo seria criar um controle de conteúdo para esse ContentPlaceHolder e adicionar a interface necessária. A página de logon, por outro lado, seria ou omitir a adição de um controle de conteúdo para esse ContentPlaceHolder ou criaria um conteúdo de controle com nenhuma marcação definida. A desvantagem dessa abordagem é que temos lembrar-se de adicionar as caixas de texto nome de usuário e senha em cada página que adicionamos ao site (exceto para a página de logon). Isso é querer problemas. É provável que se esqueça de adicionar essas caixas de texto a uma página ou dois ou, pior ainda, talvez não implementamos a interface corretamente (talvez adicionar apenas uma caixa de texto em vez de dois).

Uma solução melhor é definir as caixas de texto nome de usuário e senha como conteúdo de padrão do ContentPlaceHolder. Ao fazer isso, só precisamos substituir esse conteúdo padrão em algumas páginas que não são exibidos para as caixas de texto nome de usuário e senha (o página de logon, por exemplo). Para ilustrar especificando conteúdo padrão para um controle ContentPlaceHolder, vamos implementar o cenário que acabamos de discutir.

> [!NOTE]
> O restante deste tutorial atualiza nosso site para incluir uma interface de logon na coluna à esquerda para todas as páginas, mas a página de logon. No entanto, este tutorial examina como configurar o site para dar suporte a contas de usuário. Para obter mais informações sobre esse tópico, consulte a minha [formulários de autenticação, autorização, contas de usuário e funções](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) tutoriais.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Adicionando um ContentPlaceHolder e especificando seu conteúdo padrão

Abra o `Site.master` página mestra e adicione a seguinte marcação para a coluna esquerda entre a `DateDisplay` seção rótulo e lições:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

Depois de adicionar essa marcação de exibição de Design da sua página mestre deve ser semelhante a Figura 6.


[![A página mestra inclui um controle de logon](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**Figura 06**: A página mestra inclui um controle de logon ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))


Este ContentPlaceHolder `QuickLoginUI`, tem um controle de Web de logon como seu conteúdo padrão. O controle de logon exibe uma interface do usuário que solicita ao usuário seu nome de usuário e senha juntamente com um botão de Log. Ao clicar no botão logon, o controle de logon internamente valida as credenciais do usuário em relação a API Membership. Para usar esse controle de logon na prática, em seguida, você precisará configurar seu site para usar a associação. Este tópico está além do escopo deste tutorial. Consulte a minha [formulários de autenticação, autorização, contas de usuário e funções](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) tutoriais para obter mais informações sobre a criação de um aplicativo web que dá suporte a contas de usuário.

Fique à vontade personalizar o comportamento ou a aparência do controle de logon. Defini duas de suas propriedades: `TitleText` e `FailureAction`. O `TitleText` valor da propriedade, cujo padrão é "Fazer logon", é exibida na parte superior da interface do usuário do controle. Posso ter definir essa propriedade para que ele exibe o texto "Entrar" como um `<h3>` elemento. O `FailureAction` propriedade indica o que fazer se as credenciais do usuário são inválidas. O padrão é um valor de `Refresh`, que deixa o usuário na mesma página e exibe uma mensagem de falha dentro do controle de logon. Alterei para `RedirectToLoginPage`, que envia o usuário para a página de logon no caso de credenciais inválidas. Eu prefiro enviar o usuário para a página de logon, quando um usuário tenta fazer logon de alguma outra página, mas falhar, porque a página de logon pode conter instruções adicionais e opções que não se ajusta facilmente para a coluna à esquerda. Por exemplo, a página de logon pode incluir opções para recuperar uma senha esquecida ou criar uma nova conta.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Criando a página de logon e substituindo o conteúdo padrão

Com a página mestre concluída, nossa próxima etapa é criar a página de logon. Adicionar uma página ASP.NET para o diretório de raiz do seu site chamado `Login.aspx`, associando-o para o `Site.master` página mestra. Isso criará uma página com quatro controles de conteúdo, um para cada o ContentPlaceHolders definido no `Site.master`.

Adicionar um controle de logon para o `MainContent` controle de conteúdo. Da mesma forma, fique à vontade adicionar qualquer conteúdo para o `LeftColumnContent` região. No entanto, certifique-se de deixar o controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder vazia. Isso irá assegurar que o logon do controle não será exibido na coluna à esquerda da página de logon.

Depois de definir o conteúdo para o `MainContent` e `LeftColumnContent` regiões, marcação declarativa da sua página de logon deve ser semelhante ao seguinte:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

Figura 7 mostra essa página quando visualizado por meio de um navegador. Como esta página especifica um controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder, ele substitui o conteúdo padrão especificado na página mestra. O efeito líquido é que o controle de logon é exibido no Design da página mestra exibição (veja a Figura 6) é renderizada não nesta página.


[![A página de logon Represses conteúdo de padrão de QuickLoginUI ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**Figura 07**: A página de logon Represses a `QuickLoginUI` o conteúdo de padrão do ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Usando o conteúdo padrão em novas páginas

Queremos mostrar o controle de logon na coluna à esquerda para todas as páginas, exceto a página de logon. Para fazer isso, todas as páginas de conteúdo, exceto para a página de logon deverá omitir um controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder. Omitindo um controle de conteúdo, o conteúdo de padrão do ContentPlaceHolder será usado em vez disso.

Nossas páginas de conteúdo existentes - `Default.aspx`, `About.aspx`, e `MultipleContentPlaceHolders.aspx` -não incluem um controle de conteúdo para `QuickLoginUI` porque eles foram criados antes de adicionarmos desse controle ContentPlaceHolder para a página mestra. Portanto, essas páginas existentes não precisa ser atualizado. No entanto, novas páginas adicionadas ao site incluem um controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder, por padrão. Portanto, temos que não se esqueça de remover esses controles de conteúdo de cada vez que podemos adicionar uma nova página de conteúdo (a menos que queremos substituir o conteúdo de padrão do ContentPlaceHolder, como no caso da página de logon).

Para remover o controle de conteúdo, você pode manualmente excluir sua marcação declarativa da exibição da fonte ou, na exibição de Design, escolha o padrão para o link de conteúdo do mestre de sua marca inteligente. Qualquer uma das abordagens remove o controle de conteúdo da página e produz o mesmo efeito de rede.

A Figura 8 mostra `Default.aspx` quando visualizado por meio de um navegador. Lembre-se de que `Default.aspx` tem apenas dois controles de conteúdo especificados na sua marcação declarativa - um para `head` e outra para `MainContent`. Como resultado, o conteúdo para o padrão de `LeftColumnContent` e `QuickLoginUI` ContentPlaceHolders são exibidos.


[![O conteúdo padrão para o LeftColumnContent e QuickLoginUI ContentPlaceHolders são exibidos](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**Figura 08**: O padrão conteúdo para o `LeftColumnContent` e `QuickLoginUI` ContentPlaceHolders são exibidas ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))


## <a name="summary"></a>Resumo

O modelo de página mestra do ASP.NET permite que um número arbitrário de ContentPlaceHolders na página mestra. O que é mais, ContentPlaceHolders incluir conteúdo padrão, o que é emitido no caso em que não há nenhum correspondente controle na página de conteúdo de conteúdo. Neste tutorial vimos como incluir controles ContentPlaceHolder adicionais na página mestra e como definir controles de conteúdo para esses novos ContentPlaceHolders nas páginas do ASP.NET novas e existentes. Também examinamos especificando padrão conteúdos em um ContentPlaceHolder, que é útil em cenários em que apenas uma minoria das necessidades de páginas para personalizar o caso contrário padronizado conteúdo dentro de uma determinada região.

O próximo tutorial, examinaremos o `head` ContentPlaceHolder mais detalhadamente, vendo como declarativamente e programaticamente definir o título, marcas meta e outros cabeçalhos de HTML em uma base de página por página.

Boa programação!

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla.com, trabalha com tecnologias Web Microsoft desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 3.5 in 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog em [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Revisor de avanço para este tutorial foi Suchi Banerjee. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-a-site-wide-layout-using-master-pages-vb.md)
> [Próximo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
