---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Criar um Layout consistente na Web do ASP.NET (Razor) Sites de páginas | Microsoft Docs
author: tfitzmac
description: Para tornar mais eficiente criar páginas da web para seu site, você pode criar blocos reutilizáveis de conteúdo (como cabeçalhos e rodapés) para seu site e você c...
ms.author: aspnetcontent
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: d27cdc70417f380d596f4d07384a615586427643
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821208"
---
<a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Criar um Layout consistente nos Sites do ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo explica como você pode usar páginas de layout em um site de páginas da Web do ASP.NET (Razor) para criar blocos reutilizáveis de conteúdo (como cabeçalhos e rodapés) e para criar uma aparência consistente para todas as páginas no site.
> 
> **O que você aprenderá:** 
> 
> - Como criar blocos reutilizáveis de conteúdo como cabeçalhos e rodapés.
> - Como criar uma aparência consistente para todas as páginas no seu site usando um layout.
> - Como passar dados em tempo de execução para uma página de layout.
> 
> Estes são os recursos do ASP.NET introduzidos no artigo:
> 
> - Blocos de conteúdo, que são arquivos que contêm conteúdo formatado em HTML a ser inserido em várias páginas.
> - Páginas de layout, que são páginas que contêm conteúdo formatado em HTML que pode ser compartilhado por páginas do site.
> - O `RenderPage`, `RenderBody`, e `RenderSection` métodos, que informam ao ASP.NET onde inserir elementos de página.
> - O `PageData` dicionário que permite que você compartilhe dados entre os blocos de conteúdo e páginas de layout.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2.


## <a name="about-layout-pages"></a>Sobre páginas de Layout

Muitos sites têm conteúdo que é exibido em cada página, como um cabeçalho e rodapé ou uma caixa que informa aos usuários que ele está conectado. ASP.NET permite que você crie um arquivo separado com um bloco de conteúdo que pode conter texto, marcação e código, assim como uma página da web regulares. Em seguida, você pode inserir o bloco de conteúdo em outras páginas no site de onde você deseja que as informações sejam exibidas. Dessa forma, você não precisa copiar e colar o mesmo conteúdo em cada página. Criação de conteúdo comuns, como isso também torna mais fácil atualizar o site. Se você precisar alterar o conteúdo, você pode atualizar apenas um único arquivo e as alterações são refletidas em todos os lugares o conteúdo foi inserido.

O diagrama a seguir mostra como o conteúdo bloqueia o trabalho. Quando um navegador solicita uma página do servidor web, o ASP.NET insere os blocos de conteúdo no ponto em que o `RenderPage` método é chamado na página principal. A página (mesclada) concluído é enviada ao navegador.

![Diagrama conceitual mostrando como o método RenderPage insere uma página referenciada na página atual.](3-creating-a-consistent-look/_static/image1.jpg)

Neste procedimento, você criará uma página que faz referência a dois blocos de conteúdo (um cabeçalho e um rodapé) que estão localizados em arquivos separados. Você pode usar esses mesmos blocos de conteúdo em qualquer página no seu site. Quando você terminar, você obterá uma página como esta:

![Captura de tela mostrando uma página no navegador que resulta da execução de uma página que inclui chamadas ao método RenderPage.](3-creating-a-consistent-look/_static/image2.jpg)

1. Na pasta raiz do seu site, crie um arquivo chamado *index. cshtml*.
2. Substitua a marcação existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Na pasta raiz, crie uma pasta chamada *compartilhado*.

    > [!NOTE]
    > É prática comum para armazenar arquivos que são compartilhados entre páginas da web em uma pasta chamada *compartilhado*.
4. No *Shared* pasta, crie um arquivo chamado  *\_Header.cshtml*.
5. Substitua todo o conteúdo existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Observe que é o nome do arquivo  *\_Header.cshtml*, com um sublinhado (\_) como um prefixo. ASP.NET não enviará uma página no navegador, se seu nome começa com um sublinhado. Isso impede que as pessoas solicitando (inadvertidamente ou de outra forma) essas páginas diretamente. Ele é uma boa ideia usar um caractere de sublinhado para páginas de nome que tem blocos de conteúdo, como você realmente não quer que os usuários possam solicitar essas páginas &#8212; existirem estritamente sejam inseridos em outras páginas.
6. No *Shared* pasta, crie um arquivo chamado  *\_Footer.cshtml* e substitua o conteúdo a seguir:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. No *index. cshtml* página, adicione duas chamadas para o `RenderPage` método, conforme mostrado aqui:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Isso mostra como inserir um bloco de conteúdo em uma página da web. Você chama o `RenderPage` método e passe o nome do arquivo cujo conteúdo você deseja inserir nesse ponto. Aqui, você está inserindo o conteúdo do  *\_Header.cshtml* e  *\_Footer.cshtml* arquivos para o *index. cshtml* arquivo.
8. Execute o *index. cshtml* página em um navegador. (No WebMatrix, nos **arquivos** espaço de trabalho, o arquivo com o botão direito e, em seguida, selecione **iniciar no navegador**.)
9. No navegador, exiba a origem da página. (Por exemplo, no Internet Explorer, a página com o botão direito e, em seguida, clique em **Exibir código-fonte**.)

    Isso permite que você veja a marcação de página da web que é enviada ao navegador, que combina a marcação da página de índice com os blocos de conteúdo. O exemplo a seguir mostra a origem da página que é renderizada para *index. cshtml*. As chamadas para `RenderPage` que você inserida *index. cshtml* foram substituídos com o conteúdo real dos arquivos de cabeçalho e rodapé.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Criar uma aparência consistente usando páginas de Layout

Até agora, você viu que ele é fácil de incluir o mesmo conteúdo em várias páginas. Uma abordagem mais estruturada para criar uma aparência consistente para um site é usar páginas de layout. Uma página de layout define a estrutura de uma página da web, mas não contém qualquer conteúdo real. Depois de criar uma página de layout, você pode criar páginas da web que contêm o conteúdo e, em seguida, vinculá-los para a página de layout. Quando essas páginas são exibidas, eles serão formatados de acordo com a página de layout. (Nesse sentido, uma página de layout atua como um tipo de modelo para o conteúdo que é definido em outras páginas.)

A página de layout é como qualquer página HTML, exceto que ela contém uma chamada para o `RenderBody` método. A posição do `RenderBody` método na página de layout determina onde as informações da página de conteúdo será incluídas.

O diagrama a seguir mostra o conteúdo como páginas e páginas de layout são combinadas em tempo de execução para produzir a página da web concluída. O navegador solicita uma página de conteúdo. A página de conteúdo tem código que especifica a página de layout a ser usado para a estrutura da página. Na página de layout, o conteúdo é inserido no ponto em que o `RenderBody` método é chamado. Conteúdo de blocos também podem ser inseridos em página de layout, chamando o `RenderPage` método, da forma que fez na seção anterior. Quando a página da web for concluída, ela é enviada ao navegador.

![Captura de tela mostrando uma página no navegador que resulta da execução de uma página que inclui chamadas ao método RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

O procedimento a seguir mostra como criar um layout de página e link de páginas de conteúdo para ele.

1. No *Shared* pasta do seu site, crie um arquivo chamado  *\_Layout1.cshtml*.
2. Substitua todo o conteúdo existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Você usa o `RenderPage` método em uma página de layout para inserir blocos de conteúdo. Uma página de layout pode conter apenas uma chamada para o `RenderBody` método.
3. No *Shared* pasta, crie um arquivo chamado  *\_Header2.cshtml* e substitua todo o conteúdo existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Na pasta raiz, crie uma nova pasta e denomine *estilos*.
5. No *estilos* pasta, crie um arquivo chamado *CSS* e adicione as seguintes definições de estilo:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Essas definições de estilo aqui são apenas Mostrar como as folhas de estilos podem ser usadas com páginas de layout. Se você quiser, você pode definir seus próprios estilos para esses elementos.
6. Na pasta raiz, crie um arquivo chamado *Content1.cshtml* e substitua todo o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Essa é uma página que irá usar uma página de layout. O bloco de código na parte superior da página indica qual página de layout a ser usado para formatar esse conteúdo.
7. Execute *Content1.cshtml* em um navegador. A página renderizada usa o formato e a folha de estilos definidos no  *\_Layout1.cshtml* e o texto (conteúdo) definido na *Content1.cshtml*.

    ![[imagem]](3-creating-a-consistent-look/_static/image4.jpg)

    Você pode repetir a etapa 6 para criar páginas de conteúdo adicionais que podem compartilhar a mesma página de layout.

    > [!NOTE]
    > Você pode configurar seu site para que você pode usar a mesma página de layout automaticamente para todas as páginas de conteúdo em uma pasta. Para obter detalhes, consulte [Personalizando o comportamento de todo o Site do ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Criando páginas de Layout que têm várias seções de conteúdo

Uma página de conteúdo pode ter várias seções, que é útil se você quiser usar layouts que têm várias áreas com conteúdo substituível. Na página de conteúdo, você atribui cada seção um nome exclusivo. (A seção padrão é deixada sem nome.) Na página de layout, você adiciona um `RenderBody` método para especificar onde a seção (padrão) sem nome deverá aparecer. Em seguida, adicione separado `RenderSection` métodos para renderizar seções nomeadas individualmente.

O diagrama a seguir mostra como o ASP.NET gerencia o conteúdo que é dividido em várias seções. Cada seção nomeada está contida em um bloco de seção na página de conteúdo. (Eles são nomeados `Header` e `List` no exemplo.) O framework insere seção de conteúdo na página de layout no ponto em que o `RenderSection` método é chamado. A seção sem nome (padrão) é inserida no ponto em que o `RenderBody` método é chamado, como você viu anteriormente.

![Diagrama conceitual mostrando como o método RenderSection insere referências seções da página atual.](3-creating-a-consistent-look/_static/image5.jpg)

Este procedimento mostra como criar uma página de conteúdo que tem várias seções de conteúdo e como renderizá-lo usando uma página de layout que dá suporte a várias seções de conteúdo.

1. No *Shared* pasta, crie um arquivo chamado  *\_Layout2.cshtml*.
2. Substitua todo o conteúdo existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Você usa o `RenderSection` método para renderizar o cabeçalho e a lista de seções.
3. Na pasta raiz, crie um arquivo chamado *Content2.cshtml* e substitua todo o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Esta página de conteúdo contém um bloco de código na parte superior da página. Cada seção nomeada está contida em um bloco de seção. O restante da página contém a seção de conteúdo padrão (sem nome).
4. Execute *Content2.cshtml* em um navegador.

    ![Captura de tela mostrando uma página no navegador que resulta da execução de uma página que inclui chamadas ao método RenderSection.](3-creating-a-consistent-look/_static/image6.jpg)

## <a name="making-content-sections-optional"></a>Seções de conteúdo de tomada de opcional

Normalmente, as seções que você cria em uma página de conteúdo têm que corresponder as seções que são definidas na página de layout. Você pode obter erros se ocorrer qualquer uma das seguintes opções:

- A página de conteúdo contém uma seção com nenhuma seção correspondente na página de layout.
- A página de layout contém uma seção para a qual não há nenhum conteúdo.
- A página de layout inclui chamadas de método que tentam processar a mesma seção mais de uma vez.

No entanto, você pode substituir esse comportamento para uma seção nomeada, declarando a seção a ser opcional na página de layout. Isso permite que você defina várias páginas de conteúdo que podem compartilhar uma página de layout, mas que podem ou não pode ter o conteúdo de uma seção específica.

1. Abra *Content2.cshtml* e remover a seção a seguir:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Salve a página e, em seguida, executá-lo em um navegador. Uma mensagem de erro é exibida, porque a página de conteúdo não fornece conteúdo para uma seção definida na página de layout, ou seja, a seção de cabeçalho.

    ![Captura de tela que mostra o erro que ocorrerá se você executar uma página que chama o método RenderSection, mas a seção correspondente não for fornecida.](3-creating-a-consistent-look/_static/image7.jpg)
3. No *Shared* pasta, abra o  *\_Layout2.cshtml* página e substitua esta linha:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    com o código a seguir:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    Como alternativa, você poderia substituir a linha de código anterior com o seguinte bloco de código, que produz os mesmos resultados:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Execute o *Content2.cshtml* página em um navegador novamente. (Se você ainda tiver esta página abrir no navegador, você pode simplesmente atualizá-lo.) Neste momento a página é exibida sem erro, mesmo que ele não tem cabeçalho.

## <a name="passing-data-to-layout-pages"></a>Transmitindo dados para páginas de Layout

Você pode ter dados definidos na página de conteúdo que você precisa para referir-se em uma página de layout. Nesse caso, você precisará passar os dados da página de conteúdo para a página de layout. Por exemplo, você talvez queira exibir o status de logon de um usuário, ou você talvez queira mostrar ou ocultar as áreas de conteúdo com base na entrada do usuário.

Para passar dados de uma página de conteúdo para uma página de layout, você pode colocar valores no `PageData` propriedade da página de conteúdo. O `PageData` propriedade é uma coleção de pares nome/valor que contêm os dados que você deseja passar entre páginas. Na página de layout, você pode ler valores da `PageData` propriedade.

Aqui está outro diagrama. Este trecho mostra como o ASP.NET pode usar o `PageData` propriedade para passar valores de uma página de conteúdo para a página de layout. Quando o ASP.NET começa a criar a página da web, ele cria o `PageData` coleção. Na página de conteúdo, você escreve código para colocar dados no `PageData` coleção. Os valores no `PageData` coleção também pode ser acessada por outras seções na página de conteúdo ou blocos de conteúdo adicionais.

![Diagrama conceitual que mostra como uma página de conteúdo pode preencher um dicionário PageData e passar essa informação para a página de layout.](3-creating-a-consistent-look/_static/image8.jpg)

O procedimento a seguir mostra como passar dados de uma página de conteúdo para uma página de layout. Quando a página é executada, ele exibe um botão que permite que o usuário ocultar ou mostrar uma lista que é definida na página de layout. Quando os usuários clicam no botão, ele define um valor de verdadeiro/falso (booleano) no `PageData` propriedade. A página de layout lê esse valor e se for false, ocultará a lista. O valor também é usado na página de conteúdo para determinar se deve exibir o **lista ocultar** botão ou o **Mostrar lista** botão.

![[imagem]](3-creating-a-consistent-look/_static/image9.jpg)

1. Na pasta raiz, crie um arquivo chamado *Content3.cshtml* e substitua todo o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    O código armazena dados em duas a `PageData` propriedade &#8212; o título da página da web e true ou false para especificar se deseja exibir uma lista.

    Observe que o ASP.NET permite que você coloque a marcação HTML na página condicionalmente usando um bloco de código. Por exemplo, o `if/else` bloco no corpo da página determina qual formulário para exibir, dependendo se `PageData["ShowList"]` é definido como true.
2. No *Shared* pasta, crie um arquivo chamado  *\_Layout3.cshtml* e substitua todo o conteúdo existente pelo seguinte:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    A página de layout inclui uma expressão na `<title>` elemento que obtém o valor de título do `PageData` propriedade. Ele também usa o `ShowList` valor da `PageData` propriedade para determinar se deve exibir o bloco de conteúdo da lista.
3. No *Shared* pasta, crie um arquivo chamado  *\_List.cshtml* e substitua todo o conteúdo existente pelo seguinte:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Execute o *Content3.cshtml* página em um navegador. A página é exibida com a lista visível no lado esquerdo da página e um **Ocultar lista** botão na parte inferior.

    ![Captura de tela mostrando a página que inclui a lista e um botão que diz 'Ocultar lista'.](3-creating-a-consistent-look/_static/image10.jpg)
5. Clique em **Ocultar lista**. A lista desaparece e o botão muda para **Mostrar lista**.

    ![Captura de tela mostrando a página que não inclui a lista e um botão que diz 'Mostrar lista'.](3-creating-a-consistent-look/_static/image11.jpg)
6. Clique o **Mostrar lista** botão e a lista é exibida novamente.

## <a name="additional-resources"></a>Recursos adicionais


[Personalizando o comportamento de todo o Site para páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
