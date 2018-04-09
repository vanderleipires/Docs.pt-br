---
uid: web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
title: Vários ContentPlaceHolders e conteúdo padrão (VB) | Microsoft Docs
author: rick-anderson
description: Examina Adicionar vários conteúdo espaços reservados para uma página mestre, bem como especificar o conteúdo padrão em que o conteúdo de espaços reservados.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 866a7177-6884-451e-88f4-c934b1dd1af5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/multiple-contentplaceholders-and-default-content-vb
msc.type: authoredcontent
ms.openlocfilehash: fcd1d8f34dba52a04c0d9f6a1961df7b97405b42
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="multiple-contentplaceholders-and-default-content-vb"></a>Vários ContentPlaceHolders e conteúdo padrão (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_02_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_02_VB.pdf)

> Examina Adicionar vários conteúdo espaços reservados para uma página mestre, bem como especificar o conteúdo padrão em que o conteúdo de espaços reservados.


## <a name="introduction"></a>Introdução

No tutorial anterior examinamos como mestre páginas habilitar os desenvolvedores do ASP.NET para criar um layout consistente de todo o site. Páginas mestras definem marcação que é comuns a todas as suas páginas de conteúdo e regiões que podem ser personalizados em uma base de página por página. No tutorial anterior, criamos uma página mestra simples (`Site.master`) e duas páginas de conteúdo (`Default.aspx` e `About.aspx`). Nossa página mestre consistia em duas ContentPlaceHolders chamados `head` e `MainContent`, que foram localizado no `<head>` elemento e o formulário da Web, respectivamente. Enquanto as páginas conteúdas tinham dois controles de conteúdo, especificamos somente marcação para o correspondente `MainContent`.

Conforme evidenciado por dois controles ContentPlaceHolder `Site.master`, uma página mestra pode conter vários ContentPlaceHolders. Além disso, a página mestra pode especificar marcação padrão para os controles ContentPlaceHolder. Uma página de conteúdo, em seguida, pode opcionalmente especificar sua própria marcação ou usar a marcação padrão. Neste tutorial, examinar usando vários controles de conteúdo na página mestra e ver como definir marcação padrão em controles ContentPlaceHolder.

## <a name="step-1-adding-additional-contentplaceholder-controls-to-the-master-page"></a>Etapa 1: Adicionando controles ContentPlaceHolder adicionais para a página mestra

Muitos projetos de site contêm várias áreas que são personalizadas em uma base de página por página na tela. `Site.master`, a página mestra são criadas no tutorial anterior, contém um único ContentPlaceHolder dentro do formulário da Web denominado `MainContent`. Especificamente, este ContentPlaceHolder está localizado dentro de `mainContent` `<div>` elemento.

A Figura 1 mostra `Default.aspx` quando visualizada através de um navegador. A região dentro de um círculo em vermelho é a marcação de página específico correspondente ao `MainContent`.


[![A região dentro de um círculo mostra a área atualmente personalizável em uma base de página por página](multiple-contentplaceholders-and-default-content-vb/_static/image2.png)](multiple-contentplaceholders-and-default-content-vb/_static/image1.png)

**Figura 01**: A região circulados mostra a área atualmente personalizável em uma base de página por página ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image3.png))


Imagine que além da região mostrada na Figura 1, é também necessário adicionar itens específicos de páginas para a coluna esquerda sob as lições e notícias seções. Para fazer isso, adicionamos outro controle ContentPlaceHolder para a página mestra. Para acompanhar, abra o `Site.master` página no Visual Web Developer mestra e, em seguida, arraste um controle ContentPlaceHolder da caixa de ferramentas para o designer após a seção de notícias. Definir o ContentPlaceHolder `ID` para `LeftColumnContent`.


[![Adicionar um controle ContentPlaceHolder a coluna à esquerda da página mestra](multiple-contentplaceholders-and-default-content-vb/_static/image5.png)](multiple-contentplaceholders-and-default-content-vb/_static/image4.png)

**Figura 02**: adicionar um controle ContentPlaceHolder a coluna da esquerda da página mestra ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image6.png))


Com a adição do `LeftColumnContent` ContentPlaceHolder para a página mestra, podemos definir conteúdo para esta região em uma base de página por página, incluindo um conteúdo de controle na página cuja `ContentPlaceHolderID` é definido como `LeftColumnContent`. Vamos examinar esse processo na etapa 2.

## <a name="step-2-defining-content-for-the-new-contentplaceholder-in-the-content-pages"></a>Etapa 2: Definir o conteúdo para o novo ContentPlaceHolder nas páginas de conteúdo

Ao adicionar uma nova página de conteúdo para o site, o Visual Web Developer cria automaticamente um conteúdo de controle na página para cada ContentPlaceHolder na página mestra selecionada. Após adicionar um a `LeftColumnContent` ContentPlaceHolder à nossa página mestre na etapa 1, novo ASP.NET páginas serão agora tem três controles de conteúdo.

Para ilustrar isso, adicione uma nova página de conteúdo para o diretório raiz chamado `MultipleContentPlaceHolders.aspx` que é associado ao `Site.master` página mestra. O Visual Web Developer cria esta página com a seguinte marcação declarativa:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample1.aspx)]

Insira algum conteúdo para o controle de conteúdo de referência a `MainContent` ContentPlaceHolders (`Content2`). Em seguida, adicione a seguinte marcação para o `Content3` controle de conteúdo (que referencia o `LeftColumnContent` ContentPlaceHolder):

[!code-html[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample2.html)]

Depois de adicionar essa marcação, visite a página por meio de um navegador. Como mostra a Figura 3, a marcação é colocado no `Content3` controle de conteúdo é exibido na coluna à esquerda, abaixo da seção de notícias (marcada em vermelho). A marcação é colocado no `Content2` é exibido na parte direita da página (dentro de um círculo em azul).


[![A coluna da esquerda agora inclui o conteúdo específico de página abaixo da seção de notícias](multiple-contentplaceholders-and-default-content-vb/_static/image8.png)](multiple-contentplaceholders-and-default-content-vb/_static/image7.png)

**Figura 03**: O esquerda agora inclui específico de página conteúdo abaixo o notícias seção da coluna ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image9.png))


### <a name="defining-content-in-existing-content-pages"></a>Definir o conteúdo em páginas de conteúdo existentes

Criar uma nova página de conteúdo automaticamente incorpora o controle ContentPlaceHolder que adicionamos na etapa 1. Mas nossas duas páginas de conteúdo existentes - `About.aspx` e `Default.aspx` -não tem um controle de conteúdo para o `LeftColumnContent` ContentPlaceHolder. Para especificar o conteúdo para este ContentPlaceHolder nessas duas páginas existentes, é preciso adicionar nós um controle de conteúdo.

Ao contrário da maioria dos controles da Web do ASP.NET, ferramentas do Visual Web Developer não inclui um item de controle de conteúdo. É possível digitar manualmente na marcação declarativa do controle de conteúdo no modo de exibição de fonte, mas uma abordagem mais fácil e rápida é usar o modo de exibição de Design. Abra o `About.aspx` página e alterne para o modo de Design. Como ilustra a Figura 4, o `LeftColumnContent` ContentPlaceHolder aparece na exibição de Design; se você passar o mouse sobre ele, lê o título exibido: "LeftColumnContent (Master)". A inclusão de "Mestre" no título indica que não há nenhum controle de conteúdo definido na página para este ContentPlaceHolder. Se houver um controle de conteúdo para ContentPlaceHolder, como no caso de `MainContent`, lerá o título: "*ContentPlaceHolderID* (personalizado)."

Para adicionar um controle de conteúdo para o `LeftColumnContent` ContentPlaceHolder para `About.aspx`, expanda a marca inteligente do ContentPlaceHolder e clique no link criar conteúdo personalizado.


[![O modo de exibição de Design para About mostra o LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image11.png)](multiple-contentplaceholders-and-default-content-vb/_static/image10.png)

**Figura 04**: O modo de exibição de Design para `About.aspx` mostra o `LeftColumnContent` ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image12.png))


Clique no link criar conteúdo personalizado gera necessários controle na página e conjuntos de conteúdo seu `ContentPlaceHolderID` propriedade para o ContentPlaceHolder `ID`. Por exemplo, clique no link criar conteúdo personalizado para `LeftColumnContent` região em `About.aspx` adiciona a seguinte marcação declarativa à página:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample3.aspx)]

### <a name="omitting-content-controls"></a>A omissão de controles de conteúdo

ASP.NET não exige que todas as páginas de conteúdo incluem controles de conteúdo para cada ContentPlaceHolder definido na página mestra. Se um controle de conteúdo for omitido, o mecanismo do ASP.NET usa a marcação definida no ContentPlaceHolder na página mestra. Essa marcação é conhecida como o ContentPlaceHolder *conteúdo padrão* e é útil em cenários onde o conteúdo para alguns região é comum entre a maioria das páginas, mas precisa ser personalizado para um pequeno número de páginas. Etapa 3 explora especificando conteúdo padrão da página mestra.

Atualmente, `Default.aspx` contém dois controles de conteúdo para o `head` e `MainContent` ContentPlaceHolders; ele não tem um controle de conteúdo para `LeftColumnContent`. Consequentemente, quando `Default.aspx` é renderizado o `LeftColumnContent` conteúdo de padrão do ContentPlaceHolder é usado. Como temos ainda definir qualquer conteúdo padrão para este ContentPlaceHolder, o resultado é que nenhuma marcação é emitida para esta região. Para verificar se esse comportamento, visite `Default.aspx` através de um navegador. Como mostra a Figura 5, nenhuma marcação é emitida na coluna à esquerda, abaixo da seção de notícias.


[![Nenhum conteúdo é renderizado para o LeftColumnContent ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image14.png)](multiple-contentplaceholders-and-default-content-vb/_static/image13.png)

**Figura 05**: nenhum conteúdo é renderizado para o `LeftColumnContent` ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image15.png))


## <a name="step-3-specifying-default-content-in-the-master-page"></a>Etapa 3: Especificar conteúdo padrão na página mestra

Alguns designs de site incluem uma região cujo conteúdo é o mesmo para todas as páginas no site, exceto uma ou duas exceções. Considere a possibilidade de um site que oferece suporte a contas de usuário. Um site requer uma página de logon onde os visitantes podem inserir suas credenciais para entrar no site. Para agilizar o processo de inscrição, os designers de site podem incluir caixas de texto nome de usuário e senha no canto superior esquerdo de cada página para permitir que os usuários façam logon sem precisar explicitamente, visite a página de logon. Essas caixas de texto nome de usuário e senha são úteis para a maioria das páginas, eles são redundantes na página de logon, que já contém caixas de texto para as credenciais do usuário.

Para implementar esse design, você pode criar um controle ContentPlaceHolder no canto superior esquerdo da página mestra. Cada página que é necessária para exibir caixas de texto nome de usuário e senha no seu canto superior esquerdo deve criar um controle de conteúdo para este ContentPlaceHolder e adicionar a interface necessária. A página de logon, por outro lado, seria omitir ou adicionando um controle de conteúdo para este ContentPlaceHolder ou criaria um conteúdo de controle com nenhuma marcação definida. A desvantagem dessa abordagem é que temos de lembrar para adicionar caixas de texto nome de usuário e senha para cada página que podemos adicionar ao site (exceto para a página de logon). Isso está solicitando de problemas. Estamos provavelmente esqueceu de adicionar essas caixas de texto para uma página ou dois, ou pior, pode não implementa a interface corretamente (talvez adicionando apenas uma caixa de texto em vez de dois).

A melhor solução é definir as caixas de texto nome de usuário e senha como conteúdo de padrão do ContentPlaceHolder. Ao fazer isso, precisamos apenas substituir esse conteúdo padrão em algumas páginas que não exibirá caixas de texto nome de usuário e senha (logon de página, por exemplo). Para ilustrar especificando conteúdo padrão para um controle ContentPlaceHolder, vamos implementar o cenário que acabamos de discutir.

> [!NOTE]
> O restante deste tutorial atualiza o site para incluir uma interface de logon na coluna à esquerda para todas as páginas, mas a página de logon. No entanto, este tutorial não examina como configurar o site para oferecer suporte a contas de usuário. Para obter mais informações sobre este tópico, consulte o meu [formulários de autenticação, autorização, contas de usuário e funções](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) tutoriais.


### <a name="adding-a-contentplaceholder-and-specifying-its-default-content"></a>Adicionar um ContentPlaceHolder e especificar seu conteúdo padrão

Abra o `Site.master` página mestra e adicione a seguinte marcação para a coluna esquerda entre a `DateDisplay` seção rótulo e lições:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample4.aspx)]

Depois de adicionar essa marcação de modo de Design da sua página mestra deve ser semelhante à Figura 6.


[![A página mestra inclui um controle de logon](multiple-contentplaceholders-and-default-content-vb/_static/image17.png)](multiple-contentplaceholders-and-default-content-vb/_static/image16.png)

**Figura 06**: A página mestra inclui um controle de logon ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image18.png))


Este ContentPlaceHolder `QuickLoginUI`, tem um controle de Web de logon como seu conteúdo padrão. O controle de logon exibe uma interface do usuário que solicita ao usuário seu nome de usuário e senha juntamente com um botão de logon. Ao clicar no botão logon, o controle de logon internamente valida as credenciais do usuário em relação a API de associação. Para usar esse controle de logon na prática, em seguida, você precisa configurar seu site para usar a associação. Este tópico está além do escopo deste tutorial; Consulte o meu [formulários de autenticação, autorização, contas de usuário e funções](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) tutoriais para obter mais informações sobre a criação de um aplicativo web que oferece suporte a contas de usuário.

Fique à vontade para personalizar o comportamento ou a aparência do controle de logon. Eu defini duas das suas propriedades: `TitleText` e `FailureAction`. O `TitleText` valor da propriedade, cujo padrão é "Log em", é exibida na parte superior da interface do usuário do controle. Eu defini essa propriedade para que ele exibe o texto "Entrar" como um `<h3>` elemento. O `FailureAction` propriedade indica o que fazer se as credenciais do usuário são inválidas. O padrão é um valor de `Refresh`, que deixa o usuário na mesma página e exibe uma mensagem de falha dentro do controle de logon. Alterei a `RedirectToLoginPage`, que envia o usuário para a página de logon no caso de credenciais inválidas. Prefiro enviar o usuário para a página de logon quando um usuário tenta fazer logon em alguma outra página, mas falhar, porque a página de logon pode conter instruções adicionais e opções que não se ajusta facilmente a coluna à esquerda. Por exemplo, a página de logon pode incluir opções para recuperar uma senha esquecida ou para criar uma nova conta.

### <a name="creating-the-login-page-and-overriding-the-default-content"></a>Criando a página de logon e substituindo o conteúdo padrão

Com a página mestra completa, nossa próxima etapa é criar a página de logon. Adicionar uma página ASP.NET para o diretório raiz de seu site chamado `Login.aspx`, associando-a para o `Site.master` página mestra. Isso criará uma página com quatro controles de conteúdo, uma para cada o ContentPlaceHolders definido no `Site.master`.

Adicionar um controle de logon para o `MainContent` controle de conteúdo. Da mesma forma, fique à vontade para adicionar o conteúdo para o `LeftColumnContent` região. No entanto, certifique-se de deixar o controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder vazio. Isso irá garantir que o controle não aparece na coluna à esquerda da página de logon de logon.

Depois de definir o conteúdo para o `MainContent` e `LeftColumnContent` regiões, marcação declarativa da sua página de logon deve ser semelhante ao seguinte:

[!code-aspx[Main](multiple-contentplaceholders-and-default-content-vb/samples/sample5.aspx)]

A Figura 7 mostra essa página quando visualizada através de um navegador. Como esta página especifica um controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder, ela substitui o conteúdo padrão especificado na página mestra. O efeito líquido é que o controle de logon é exibido no Design da página mestra exibição (consulte a Figura 6) não é processada nesta página.


[![A página de logon Represses padrão conteúdo de QuickLoginUI ContentPlaceHolder](multiple-contentplaceholders-and-default-content-vb/_static/image20.png)](multiple-contentplaceholders-and-default-content-vb/_static/image19.png)

**Figura 07**: A página de logon Represses o `QuickLoginUI` padrão conteúdo do ContentPlaceHolder ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image21.png))


### <a name="using-the-default-content-in-new-pages"></a>Usando o conteúdo padrão nas novas páginas

Nós queremos mostrar o controle de logon na coluna à esquerda para todas as páginas, exceto a página de logon. Para fazer isso, todas as páginas de conteúdo, exceto a página de logon devem omitir um controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder. Omitindo um controle de conteúdo, padrão conteúdo do ContentPlaceHolder que será usado em vez disso.

Nossas páginas de conteúdo existentes - `Default.aspx`, `About.aspx`, e `MultipleContentPlaceHolders.aspx` -não incluem um controle de conteúdo para `QuickLoginUI` porque eles foram criados antes que adicionou o controle ContentPlaceHolder para a página mestra. Portanto, essas páginas existentes não precisam ser atualizados. No entanto, as novas páginas adicionadas ao site incluem um controle de conteúdo para o `QuickLoginUI` ContentPlaceHolder, por padrão. Portanto, temos de lembrar para remover esses controles de conteúdo de cada vez, adicione uma nova página de conteúdo (a menos que desejamos substituir o conteúdo do padrão do ContentPlaceHolder, como no caso da página de logon).

Para remover o controle de conteúdo, você pode manualmente excluir sua marcação declarativa da exibição da fonte ou, na exibição de Design, escolha o padrão para o link de conteúdo do mestre de sua marca inteligente. Qualquer abordagem remove o controle de conteúdo da página e produz o mesmo efeito de rede.

A Figura 8 mostra `Default.aspx` quando visualizada através de um navegador. Lembre-se de que `Default.aspx` tem apenas dois controles de conteúdo especificados na sua marcação declarativa - uma para `head` e outra para `MainContent`. Como resultado, o conteúdo para o padrão de `LeftColumnContent` e `QuickLoginUI` ContentPlaceHolders são exibidos.


[![O conteúdo padrão para o LeftColumnContent e QuickLoginUI ContentPlaceHolders são exibidos](multiple-contentplaceholders-and-default-content-vb/_static/image23.png)](multiple-contentplaceholders-and-default-content-vb/_static/image22.png)

**Figura 08**: O padrão conteúdo para o `LeftColumnContent` e `QuickLoginUI` ContentPlaceHolders são exibidas ([clique para exibir a imagem em tamanho normal](multiple-contentplaceholders-and-default-content-vb/_static/image24.png))


## <a name="summary"></a>Resumo

O modelo de página mestra ASP.NET permite um número arbitrário de ContentPlaceHolders na página mestra. O que é mais, ContentPlaceHolders incluir conteúdo padrão, o que é emitido no caso em que não há nenhum correspondente controle na página de conteúdo de conteúdo. Neste tutorial, vimos como incluir controles ContentPlaceHolder adicionais na página mestra e como definir os controles de conteúdo para esses novos ContentPlaceHolders em páginas do ASP.NET novas e existentes. Também vimos especificar padrão de conteúdo em um ContentPlaceHolder, que é útil em cenários em que apenas uma minoria dos necessidades de páginas para personalizar a padronizados conteúdo dentro de uma determinada região.

O seguinte tutorial, examinaremos a `head` ContentPlaceHolder em mais detalhes, veja como declarativamente e programaticamente definir o título, marcas meta e outros cabeçalhos HTML em uma base de página por página.

Boa programação!

### <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de vários livros sobre ASP/ASP.NET e fundador da 4GuysFromRolla. com, trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 3.5 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott pode ser contatado pelo [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com) ou em seu blog [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisor levar para este tutorial foi Suchi Banerjee. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-a-site-wide-layout-using-master-pages-vb.md)
> [Próximo](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)
