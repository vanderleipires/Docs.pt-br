---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Com o Page Inspector no Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: "Neste laboratório prático, você vai descobrir uma nova ferramenta para encontrar e corrigir problemas de página da web no Visual Studio - o Inspetor de página. O Page Inspector é uma nova ferramenta que b..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 1a9e093faae2cea1c27c582e22aebc908f78addb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Com o Page Inspector no Visual Studio 2012
====================
por [Web Camps Team](https://twitter.com/webcamps)

> Neste laboratório prático, você vai descobrir uma nova ferramenta para encontrar e corrigir problemas de página da web no Visual Studio - o Inspetor de página.
> 
> O Page Inspector é uma nova ferramenta que traz as ferramentas de diagnóstico de navegador para o Visual Studio e fornece uma experiência integrada entre o navegador, o ASP.NET e o código-fonte. Ele renderiza uma página da web (HTML, formulários da Web, ASP.NET MVC ou páginas da Web) diretamente no IDE do Visual Studio e permite que você examine o código-fonte e a saída resultante. Inspetor de página permite que você facilmente Decomponha um site rapidamente criar páginas de ponta a ponta e diagnostique problemas rapidamente.
> 
> Hoje em dia, temos um número de estruturas da Web que criar sites flexíveis e escalonáveis de maneira oportuna, como ASP.NET MVC e formulários da Web. Por outro lado, obtém mais difícil localizar problemas nas páginas porque o IDE não suporta o modo de exibição designer em páginas com base em modelo e o conteúdo dinâmico. Portanto, esses sites devem ser abertas em um navegador para ver como eles são exibidos para um usuário.
> 
> Os desenvolvedores da Web usam ferramentas externas para localizar problemas regularmente executado no navegador. Em seguida, elas retornam ao IDE e iniciar a correção. Isso alternadas atividade entre o IDE, o navegador e as ferramentas de criação de perfil pode ser ineficiente e, às vezes, requer uma nova implantação e a cada vez que você deseja reproduzir um problema de limpeza do cache.
> 
> O Page Inspector preenche uma lacuna no desenvolvimento da Web entre o cliente (ferramentas do navegador) e o servidor (ASP.NET e código-fonte) porque reúne o melhor de ambos os mundos, com um conjunto combinado de recursos.
> 
> Com o Page Inspector, você pode ver quais elementos nos arquivos de origem (incluindo o código do lado do servidor) produziram a marcação HTML a ser renderizado no navegador. O Page Inspector também permite modificar as propriedades CSS e atributos do elemento DOM para ver as alterações refletidas imediatamente no navegador.
> 
> Este laboratório prático irá orientá-lo por meio dos recursos do Page Inspector e mostram como você pode usá-los para corrigir problemas em aplicativos da Web. **Este laboratório contém dois exercícios usando fluxos semelhantes mas direcionamento diferentes tecnologias. Se você for um desenvolvedor do ASP.NET MVC, siga o exercício um; Se você for um exercício de acompanhamento de desenvolvedor WebForms dois**.
> 
> Este laboratório orienta os aperfeiçoamentos e novos recursos descritos anteriormente, aplicando alterações secundárias a um aplicativo da Web de exemplo fornecido na pasta de origem.
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Este laboratório prático, você aprenderá como:

- Decompor um Site da Web com o Page Inspector
- Inspecionar e visualizar as alterações de estilos CSS com o Page Inspector
- Detectar e corrigir problemas em suas páginas da web com o Page Inspector

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).
- Internet Explorer 9 ou superior

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Exercício 1: Usando o Inspetor de página em projetos do ASP.NET MVC](#Exercise1)
2. [Exercício 2: Usar o Page Inspector em projetos de formulários da Web](#Exercise2)

> [!NOTE]
> Cada exercício é acompanhado por uma solução inicial, localizada na pasta inicial do exercício, que permite que você execute cada exercício independentemente de outras pessoas. Dentro do código-fonte para um exercício, você também encontrará uma pasta final que contém uma solução do Visual Studio com o código que é o resultado de concluir as etapas no exercício correspondente. Você pode usar essas soluções como orientação se você precisar de ajuda adicional usando este laboratório prático.


Tempo estimado para concluir este laboratório: **30 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Exercício 1: Usando o Inspetor de página em projetos do ASP.NET MVC

Neste exercício, você aprenderá como visualizar e depurar um **ASP.NET MVC 4** solução usando **Page Inspector**. Primeiro, você executará uma breve visão geral a ferramenta para aprender os recursos que facilitam a Web processo de depuração. Em seguida, você trabalhará em uma página da web que contém questões de estilo. Você aprenderá a usar o Page Inspector para localizar o código-fonte que gera o problema e corrigi-lo.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tarefa 1: Explorando o Page Inspector

Nesta tarefa, você aprenderá como usar o Inspetor de página no contexto de um projeto ASP.NET MVC 4 que mostra uma galeria de fotos.

1. Abra o **começar** solução localizado em **fonte/Ex1-MVC4/Begin/** pasta.

    1. Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. No Solution Explorer, localize **cshtml** exibir sob o **/exibições/inicial** pasta do projeto, clique duas vezes e selecione **exibir em Inspetor de página**.

    ![Selecionando um arquivo para visualizar o Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image1.png "selecionando um arquivo para visualização no Inspetor de página")

    *Selecionando um arquivo para visualização no Inspetor de página*
3. A janela do Inspetor de página mostrará o *inicial/índice* URL mapeada para a fonte de exibição selecionado.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *O primeiro contato com o Page Inspector*

    A ferramenta de Page Inspector é integrada em seu ambiente do Visual Studio. O Inspetor contiver um navegador incorporado, junto com um criador de perfil do HTML avançado. Observe que você não precisa executar a solução para ver a aparência de suas páginas.

    > [!NOTE]
    > Quando a largura do navegador do Inspetor de página for menor que a largura da página aberta, você não verá a página corretamente. Se isso acontecer, ajuste a largura do Inspetor de página.
4. Clique o **arquivos** guia no Inspetor de página.

    Você verá todos os arquivos de origem que são compor a página de índice. Esse recurso ajuda a identificar todos os elementos em um relance, especialmente quando você estiver trabalhando com exibições parciais e modelos. Observe que você também pode abrir cada um dos arquivos se você clicar nos links.

    ![A guia arquivos](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *A guia arquivos*
5. Clique o **alternar para o modo inspeção** botão, localizado no canto esquerdo das guias.

    Essa ferramenta permitirá que você selecionar qualquer elemento da página e ver o código HTML e Razor.

    ![Inspeção alternância-botão modo](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Botão de alternância modo de inspeção*
6. No navegador do Inspetor de página, mova o ponteiro do mouse sobre os elementos da página. Enquanto você move o ponteiro do mouse sobre qualquer parte da página renderizada, o tipo de elemento é exibido e a marcação de origem ou o código correspondente é realçado no editor do Visual Studio.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Modo de inspeção em ação*

    > [!NOTE]
    > Não é maximizar a janela do Inspetor de página ou você não poderá ver a guia de visualização mostra o código-fonte. Se você clicar em um elemento Page Inspector quando ela é maximizada, o código-fonte da seleção será exibida, mas ele será ocultar a janela do Inspetor de página.

    Se você prestar atenção para o **cshtml** arquivo, você vai notar que a parte do código-fonte que gera o elemento selecionado é realçada. Esse recurso facilita a edição de arquivos de origem longo, fornecendo uma maneira rápida e direta para acessar o código.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Inspecionando elementos*
7. Clique o **alternar para o modo inspeção** botão (![selecione a guia HTML para exibir o código HTML renderizado no navegador do Page Inspector.] (using-page-inspector-in-visual-studio-2012/_static/image7.png "Selecione a guia HTML para exibir o código HTML renderizado no navegador do Inspetor de página.") ) para desabilitar o cursor.
8. Selecione o **HTML** guia para exibir o código HTML renderizado no navegador do Inspetor de página.
9. Na marcação HTML, localize o item de lista com o link Koala e selecioná-lo.

    Observe que quando você seleciona o código, a saída correspondente é realçada automaticamente no navegador. Esse recurso é útil para ver como um bloco HTML é renderizado na página.

    ![Selecionar elemento HTML na página](using-page-inspector-in-visual-studio-2012/_static/image8.png "elemento de seleção HTML na página")

    *Selecionando um elemento HTML na página*
10. Clique o **alternar para o modo inspeção** botão Habilitar *inspeção modo* e clique na barra de navegação. À direita do código HTML, no painel Estilos, você verá uma lista com os estilos CSS aplicados ao elemento selecionado.

    > [!NOTE]
    > Como o cabeçalho é uma parte do layout do site, o Page Inspector também abrirá \_arquivo cshtml e realce o segmento de código afetado.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Descoberta de estilos e arquivos de origem de um elemento selecionado*
11. Com o ponteiro de inspeção de alternância habilitado, mova o ponteiro do mouse abaixo da barra de destaque azul e clique no círculo metade.

    ![Selecionando um elemento](using-page-inspector-in-visual-studio-2012/_static/image10.png "selecionando um elemento")

    *Selecionando um elemento*
12. No painel Estilos, localize o **imagem de plano de fundo** item sob o **. Main conteúdo** grupo. **Desmarque** o **imagem de plano de fundo** e ver o que acontece. Você observará que o navegador refletirá as alterações imediatamente e o círculo está oculto.

    > [!NOTE]
    > As alterações que você aplicar na guia estilos de Inspetor de página não afetam a folha de estilos original. Você pode desmarcar estilos ou alterar seus valores quantas vezes desejar, mas serão restaurados depois de atualizar a página.

    ![Habilitando e desabilitando estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "habilitando e desabilitando estilos CSS")

    *Habilitando e desabilitando estilos CSS*
13. Agora, clique o '**seu logotipo aqui**' texto no cabeçalho usando o modo de inspeção.
14. No **estilos** guia, localize o **tamanho da fonte** CSS atributo sob o **.site título** grupo. Clique duas vezes o valor do atributo e substitua o valor 2.3 em com **em 3**e, em seguida, pressione **ENTER**. Observe que o título é maior.

    ![Alterando os valores CSS no Inspetor de página](using-page-inspector-in-visual-studio-2012/_static/image12.png "valores CSS alterando no Inspetor de página")

    *Alterando os valores CSS no Inspetor de página*
15. Clique o **rastrear estilos** guia, localizado no painel à direita do Inspetor de página. Isso é uma maneira alternativa para ver todos os estilos aplicados à seleção, ordenada pelo nome do atributo.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Rastreamento de estilos CSS do elemento selecionado*
16. Outro recurso do Page Inspector é o painel de Layout. Usando o modo de inspeção, selecione a barra de navegação e, em seguida, clique no **Layout** guia no painel direito. Você verá o tamanho exato do elemento selecionado, bem como seu tamanho deslocamento, margem, preenchimento e borda. Observe que você também pode modificar os valores dessa exibição.

    ![Layout do elemento no Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image14.png "layout do elemento no Inspetor de página")

    *Layout do elemento no Inspetor de página*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tarefa 2 - localizar e corrigir problemas de estilo na Galeria de fotos

Como você faria diagnosticar problemas de páginas da Web com as versões anteriores do Visual Studio? Você provavelmente está familiarizado com as ferramentas que são executadas fora do IDE do Visual Studio, como ferramentas para desenvolvedores do Internet Explorer ou Firebug depuração de web. Navegadores entendem apenas HTML, scripts e estilos, enquanto uma estrutura subjacente gera o HTML que será renderizado. Por esse motivo, geralmente você precisa implantar o site inteiro para ver a aparência das páginas da web como.

Você provavelmente tinha seguir estas etapas para detectar e corrigir um problema em seu site da web:

1. Executar a solução do Visual Studio ou implantar a página no servidor web.
2. No navegador, abra as ferramentas de desenvolvedor usar, ou simplesmente abra o código-fonte e os estilos e tenta corresponder o problema. Para localizar os arquivos envolvidos, você teria que usar o &quot;pesquisa&quot; ou &quot;pesquisa nos arquivos&quot; recursos com o nome das classes de estilo.
3. Quando o erro for detectado, pare o navegador da Web e o servidor.
4. Limpe o cache do navegador.
5. Retorne ao Visual Studio para aplicar uma correção. Repita todas as etapas para testar.

Como não há nenhuma WYSIWYG real no ASP.NET MVC 4, a maioria dos problemas de estilo é detectada em um estágio posterior, depois de executar ou implantar o aplicativo web. Agora, com o Page Inspector, é possível visualizar qualquer página sem executar a solução.

Nesta tarefa, você irá usar o Inspetor de página e corrigir alguns problemas com o aplicativo de galeria de fotos.

1. Usando o Inspetor de página, localize o **registrar** e **login** links no lado esquerdo do cabeçalho.

    Observe que os links não são exibidos no local esperado à direita, e eles são mostrados como uma lista com marcadores. Agora você alinhe os links para a direita e usá-los de acordo.

    ![Localizar o registro e o Log em links](using-page-inspector-in-visual-studio-2012/_static/image15.png "localizar o registro e o Log em links")

    *Localizar o registro e o Log em links*
2. Com o modo de inspeção de alternância selecionado, clique em fechada, mas não no, o link de registro para abrir seu código.

    Observe que o código-fonte dos links está localizado no  **\_LoginPartial.cshtml** arquivo, não o cshtml nem o \_cshtml, que são locais para a qual você pode pesquisar em primeiro lugar. Foi adicionado diretamente no arquivo de origem correta.
3. No **estilos** guia, localize e clique no  **<section> #login</section>**  item, que é o contêiner HTML para esses links.

    Observe que o **#login** estilo automaticamente está localizado em **Site.css** depois de clicar em. Além disso, o código agora é realçado.

    ![Selecionando os estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "selecionando os estilos CSS")

    *Selecionando os estilos CSS*
4. Remova o **de alinhamento de texto** , removendo a abertura e fechamento de caracteres de atributo no código realçado e salve o **Site.css** arquivo.

    Inspetor de página está ciente de todos os arquivos diferentes que compõem a página atual, e ele pode detectar quando qualquer um dos arquivos alterados. Ele o alertará sempre que a página atual no navegador não está em sincronia com os arquivos de origem.
5. No navegador do Inspetor de página, clique na barra localizada abaixo da barra de endereço para recarregar a página.

    ![Recarregar a página](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Recarregar a página*

    Os links são agora à direita, mas eles ainda se parecer com uma lista com marcadores. Agora, você removerá os marcadores e alinhar os links horizontalmente.

    ![Página atualizada](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Página atualizada*
6. Usando o modo de inspeção, selecione qualquer uma da  **&lt;li&gt;**  itens que contêm o &quot;registrar&quot; e &quot;login&quot; links. Em seguida, clique no  **&lt;seção&gt; #login** item para acesso **Styles** código.

    ![Localizando o estilo](using-page-inspector-in-visual-studio-2012/_static/image19.png "localizando o estilo")

    *Localizando o estilo*
7. Em **Style.css**, descomente o código para **#login li** itens. O estilo que você está adicionando serão ocultar o marcador e exibir os itens na horizontal.

    ![Reformular os links de logon](using-page-inspector-in-visual-studio-2012/_static/image20.png "reformular os links de logon")

    *Reformular os links de logon*
8. Salvar **Style.css** de arquivo e clique uma vez na barra de localizada abaixo do endereço para recarregar a página. Observe que os links são exibidos corretamente.

    ![Links alinhado à direita](using-page-inspector-in-visual-studio-2012/_static/image21.png "Links alinhado à direita")

    *Links alinhado à direita*
9. Por fim, você irá alterar o título de cabeçalho. Use o modo de inspeção para clicar **seu logotipo aqui** texto e obter o código-fonte que gera.
10. Agora você está no  **\_cshtml**, substitua '**seu logotipo aqui**'text com'**galeria**'. Salve e atualize o navegador do Inspetor de página.

    ![Atribuir um novo título](using-page-inspector-in-visual-studio-2012/_static/image22.png "atribuir um novo título")

    *Atribuir um novo título*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Página de galeria de fotos atualizada*
11. Por fim, selecione o **Galeriadefotos** projeto e pressione **F5** para executar o aplicativo. Verifique se todas as alterações funcionam como esperado.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Exercício 2: Usar o Page Inspector em projetos de formulários da Web

Neste exercício, você aprenderá como visualizar e depura uma solução de formulários da Web usando o Inspetor de página. Primeiro, você executará uma breve visão geral a ferramenta para aprender sobre os recursos do Page Inspector que facilitam a Web processo de depuração. Em seguida, você trabalhará em uma página da web que contém questões de estilo. Você aprenderá a usar o Page Inspector para localizar o código-fonte que gera o problema e corrigi-lo.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tarefa 1: Explorando o Page Inspector

Nesta tarefa, você aprenderá como usar os recursos do Page Inspector no contexto de um projeto de formulários da Web que mostra uma galeria de fotos.

1. Abra o **começar** solução localizado em **fonte/o Ex2-WebForms/Begin/** pasta.

    1. Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **projeto** menu e selecione **gerenciar pacotes NuGet**.
    2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
    3. Por fim, compile a solução clicando **criar** | **compilar solução**.

    > [!NOTE]
    > Uma das vantagens de usar NuGet é que você não precisa enviar todas as bibliotecas no seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você poderá baixar todas as bibliotecas necessárias na primeira vez que você executar o projeto. É por isso você terá que executar estas etapas depois de abrir uma solução existente neste laboratório.
2. No Solution Explorer, localize **Default.aspx** página, clique duas vezes e selecione **exibir em Inspetor de página**.

    ![Abrir Default.aspx com o Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "abrir Default.aspx com o Page Inspector")

    *Abrir Default.aspx com o Page Inspector*
3. A janela do Inspetor de página mostrará Default.aspx.

    ![Exibindo Default.aspx no Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image25.png "exibindo Default.aspx no Inspetor de página")

    *Exibindo Default.aspx no Inspetor de página*

    A ferramenta de Page Inspector é integrada em seu ambiente do Visual Studio. O Inspetor contiver um navegador incorporado, junto com um criador de perfil do HTML avançado que mostra o código selecionado. Observe que você não precisa executar a solução para ver a aparência de suas páginas.

    > [!NOTE]
    > Quando a largura do navegador do Inspetor de página for menor que a largura da página aberta, você não verá a página corretamente. Se isso acontecer, ajuste a largura do Inspetor de página.
4. Clique o **arquivos** guia no Inspetor de página.

    Você verá todos os arquivos de origem que estiver compondo padrão página renderizada. Esse é um recurso útil para identificar todos os elementos em um relance, especialmente quando você estiver trabalhando com controles de usuário e páginas mestras. Observe que você também pode navegar para cada um dos arquivos.

    ![A guia arquivos](using-page-inspector-in-visual-studio-2012/_static/image26.png "guia a arquivos")

    *A guia arquivos*
5. Clique o **alternar para o modo inspeção** botão, localizado no canto esquerdo das guias.

    Essa ferramenta permitirá que você selecionar qualquer elemento da página e consulte seu código-fonte HTML. aspx e de código.

    ![Um botão de alternância modo de inspeção](using-page-inspector-in-visual-studio-2012/_static/image27.png "botão Ativar/desativar modo de inspeção")

    *Botão de alternância modo de inspeção*
6. No navegador do Inspetor de página, mova o mouse sobre os elementos da página. Enquanto você move o ponteiro do mouse sobre qualquer parte da página renderizada, o tipo de elemento é exibido e a marcação de origem ou o código correspondente é realçado no editor do Visual Studio.

    ![Modo de inspeção em ação](using-page-inspector-in-visual-studio-2012/_static/image28.png "modo de inspeção em ação")

    *Modo de inspeção em ação*

    > [!NOTE]
    > Não é maximizar a janela do Inspetor de página ou você não poderá ver a guia de visualização mostra o código-fonte. Se você clicar em um elemento Page Inspector quando ela é maximizada, o código-fonte da seleção será exibida, mas ele será ocultar a janela do Inspetor de página.

    Se você prestar atenção à **Default.aspx** arquivo, você vai notar que a parte do código-fonte que gera o elemento selecionado é realçada. Esse recurso facilita a edição dos arquivos de origem longo, fornecendo uma maneira rápida e direta para acessar o código.

    ![Inspecionando elementos](using-page-inspector-in-visual-studio-2012/_static/image29.png "inspecionando elementos")

    *Inspecionando elementos*
7. Clique o **alternar para o modo inspeção** botão (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.] (using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), localizado em guias do Page Inspector, para desabilitar o cursor.
8. Selecione o **HTML** guia para exibir o código HTML renderizado no navegador do Inspetor de página.
9. No código HTML, localize o item de lista com o link Koala e selecioná-lo.

    Observe que, quando você seleciona o código, a saída correspondente é realçado automaticamente do navegador. Esse recurso é útil para ver como um bloco HTML é renderizado na página.

    ![Selecionando um elemento HTML na página](using-page-inspector-in-visual-studio-2012/_static/image31.png "selecionando um elemento HTML na página")

    *Selecionando um elemento HTML na página*
10. Clique o **alternar para o modo inspeção** botão Habilitar *inspeção modo* e clique na barra de navegação. À direita do código HTML, no painel Estilos, você verá uma lista com os estilos CSS aplicados ao elemento selecionado.

    > [!NOTE]
    > como o cabeçalho é uma parte do layout do site, o Page Inspector também abrir arquivo Site.Master e realce o segmento de código afetado.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "descoberta estilos e arquivos de origem de um elemento selecionado")

    *Descoberta de estilos e arquivos de origem de um elemento selecionado*
11. Com o ponteiro de inspeção de alternância habilitado, mova o ponteiro do mouse abaixo da barra de menu e clique no círculo metade em branco.

    ![Selecionando um elemento](using-page-inspector-in-visual-studio-2012/_static/image33.png "selecionando um elemento")

    *Selecionando um elemento*
12. No painel Estilos, localize o **imagem de plano de fundo** item sob o **. Main conteúdo** grupo. **Desmarque** o **imagem de plano de fundo** e ver o que acontece. Você observará que o navegador refletirá as alterações imediatamente e o círculo está oculto.

    > [!NOTE]
    > As alterações que você aplicar na guia estilos de Inspetor de página não afetam a folha de estilos original. Você pode desmarcar estilos ou alterar seus valores quantas vezes desejar, mas serão restaurados depois de atualizar a página.

    ![Habilitando e desabilitando CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "habilitando e desabilitando estilos CSS")

    *Habilitando e desabilitando estilos CSS*
13. Agora, clique o '**sua** **logotipo aqui '** texto no cabeçalho usando o modo de inspeção.
14. No **estilos** guia, localize o **tamanho da fonte** CSS atributo sob o **.site título** grupo. Clique duas vezes o atributo de uma vez para editar seu valor. Valor de substituição de 2.3em com **3em**, e pressione ENTER. Observe que o título é maior.

    ![Alterar valores CSS no Page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "valores CSS alterando no Inspetor de página")

    *Alterando os valores CSS no Inspetor de página*
15. Clique o **rastrear estilos** guia, localizado no painel à direita do Inspetor de página. Isso é uma maneira alternativa para ver todos os estilos aplicados à seleção, ordenada pelo nome do atributo.

    ![Rastreamento de estilos CSS do elemento selecionado](using-page-inspector-in-visual-studio-2012/_static/image36.png "rastreamento de estilos CSS do elemento selecionado")

    *Rastreamento de estilos CSS do elemento selecionado*
16. Outro recurso do Page Inspector é o painel de Layout. Usando o modo de inspeção, selecione a barra de navegação e, em seguida, clique no **Layout** guia no painel direito. Você verá o tamanho exato do elemento selecionado, bem como seu tamanho deslocamento, margem, preenchimento e borda. Observe que você também pode modificar os valores dessa exibição.

    ![Layout do elemento no Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image37.png "layout do elemento no Inspetor de página")

    *Layout do elemento no Inspetor de página*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tarefa 2 - localizar e corrigir problemas de estilo na Galeria de fotos

Como você faria diagnosticar problemas de páginas da Web com as versões anteriores do Visual Studio? Você provavelmente está familiarizado com as ferramentas que são executadas fora do IDE do Visual Studio, como ferramentas para desenvolvedores do Internet Explorer ou Firebug depuração de web. Navegadores entendem apenas HTML, scripts e estilos, enquanto uma estrutura subjacente gera o HTML que será renderizado. Por esse motivo, geralmente você precisa implantar o site inteiro para ver a aparência das páginas da web como.

Você provavelmente tinha seguir estas etapas para detectar e corrigir um problema em seu site da web:

1. Executar a solução do Visual Studio ou implantar a página no servidor web.
2. No navegador, abra as ferramentas de desenvolvedor usar, ou simplesmente abra o código-fonte e os estilos e tenta corresponder o problema. Para localizar os arquivos envolvidos, você teria que usar o &quot;pesquisa&quot; ou &quot;pesquisa nos arquivos&quot; recursos com o nome das classes de estilo.
3. Quando o erro for detectado, pare o navegador da Web e o servidor.
4. Limpe o cache do navegador.
5. Retorne ao Visual Studio para aplicar uma correção. Repita todas as etapas para testar.

Como há não real WYSIWYG no ASP.NET WebForms, alguns problemas de estilo são detectados em um estágio posterior, após a execução ou implantação. Agora, com o Page Inspector, é possível visualizar qualquer página sem executar a solução.

Nesta tarefa, você usará o Inspetor de página para corrigir alguns problemas do aplicativo Galeria de fotos. As etapas a seguir, você detectará e corrigir rapidamente algum problema de estilo simples no cabeçalho.

1. Usando a inspeção de página, localize o **registrar** e **logon** links no lado esquerdo do cabeçalho.

    Observe que o link não é exibido no local esperado à direita. Agora você alinhe o link à direita e usar adequadamente.

    ![Faça logon no link posicionado à esquerda](using-page-inspector-in-visual-studio-2012/_static/image38.png "fazer logon no link posicionado à esquerda")

    *Link de logon posicionado à esquerda*
2. Com o modo de inspeção de alternância selecionado, selecione o link de logon para abrir seu código.

    Observe que o código de origem do link é localizado no **Site.Master** arquivo, não na página Default.aspx que é o local pode ser em primeiro lugar; foi adicionado diretamente no arquivo de origem correta.
3. No **estilos** guia, localize e clique no  **&lt;seção&gt; #login** item, que é o contêiner HTML para esses links.

    Observe que o **#login** estilo automaticamente está localizado em **Site.css** depois de clicar em. Além disso, o código agora é realçado.

    ![Selecionando os estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "selecionando os estilos CSS")

    *Selecionando os estilos CSS*
4. Remova o **de alinhamento de texto** , removendo a abertura e fechamento de caracteres de atributo no código realçado e salve o **Site.css** arquivo.

    Inspetor de página está ciente de todos os arquivos diferentes que compõem a página atual, e ele pode detectar quando qualquer um dos arquivos alterados. Ele o alertará sempre que a página atual no navegador não está em sincronia com os arquivos de origem.
5. No navegador do Inspetor de página, clique na barra localizada abaixo da barra de endereço para salvar as alterações e recarregar a página.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Recarregar a página*

    Os links são agora à direita, mas eles ainda se parecer com uma lista com marcadores. Agora, você removerá os marcadores e alinhar os links horizontalmente.

    ![Página atualizada](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Página atualizada*
6. Usando o modo de inspeção, selecione qualquer uma da  **&lt;li&gt;**  itens que contêm o &quot;registrar&quot; e &quot;login&quot; links. Em seguida, clique no  **&lt;seção&gt; #login** item para acesso **Styles** código.

    ![Localizando o estilo](using-page-inspector-in-visual-studio-2012/_static/image42.png "localizando o estilo")

    *Localizando o estilo*
7. Em **Style.css**, descomente o código para **#login li** itens. O estilo que você está adicionando serão ocultar o marcador e exibir os itens na horizontal.

    ![Reformular os links de logon](using-page-inspector-in-visual-studio-2012/_static/image43.png "reformular os links de logon")

    *Reformular os links de logon*
8. Salvar **Style.css** de arquivo e clique uma vez na barra de localizada abaixo do endereço para recarregar a página. Observe que os links são exibidos corretamente.

    ![Links alinhado à direita](using-page-inspector-in-visual-studio-2012/_static/image44.png "Links alinhado à direita")

    *Links alinhado à direita*
9. Por fim, você irá alterar o título de cabeçalho. Em vez de procurar o '**seu logotipo aqui '** texto em todos os arquivos, use o modo de inspeção para clique no texto e obter o código-fonte que gera.

    ![Localizando o título do site](using-page-inspector-in-visual-studio-2012/_static/image45.png "localizando o título do site")

    *Localizando o título do site*
10. Agora você está no **Site.Master**, substitua o '**seu logotipo aqui**'text com'**galeria**'. Salve e atualize o navegador do Inspetor de página.

    ![Página de galeria de fotos atualizada](using-page-inspector-in-visual-studio-2012/_static/image46.png "página Galeria de fotos atualizada")

    *Página de galeria de fotos atualizada*
11. Pressione finalmente **F5** para executar o aplicativo o check-out de todas as alterações funcionam como esperado.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você tem learnt como usar o Inspetor de página para visualizar seu aplicativo da Web sem precisar recompilar e executar o site da Web em um navegador. Além disso, você tem learnt como localizar e corrigir rapidamente os bugs acessando diretamente na saída renderizada para o código-fonte.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalar o Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; *Visual Studio Express 2012 para Web com o SDK do Windows Azure*&quot;.
2. Clique em **instalar agora**. Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluída.

    ![Progresso da instalação](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Instalação concluída*
7. Clique em **saída** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco de Web](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express para o bloco de Web*
