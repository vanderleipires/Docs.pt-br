---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Usando o Inspetor de página no Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: Neste laboratório prático, você vai descobrir uma nova ferramenta Localizar e corrigir problemas de página da web no Visual Studio – o Inspetor de página. O Page Inspector é uma nova ferramenta que b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 236b739abb8c9073535361040dd7d921da9dba6e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365718"
---
<a name="using-page-inspector-in-visual-studio-2012"></a>Usando o Inspetor de página no Visual Studio 2012
====================
por [Web Camps equipe](https://twitter.com/webcamps)

> Neste laboratório prático, você vai descobrir uma nova ferramenta Localizar e corrigir problemas de página da web no Visual Studio – o Inspetor de página.
> 
> O Page Inspector é uma nova ferramenta que traz as ferramentas de diagnóstico de navegador para o Visual Studio e fornece uma experiência integrada entre o navegador, o ASP.NET e o código-fonte. Ele renderiza uma página da web (HTML, Web Forms, ASP.NET MVC ou páginas da Web) diretamente no IDE do Visual Studio e permite que você examine o código-fonte e a saída resultante. Inspetor de página permite que você facilmente Decomponha um site, criar rapidamente páginas desde o e diagnostique problemas rapidamente.
> 
> Hoje em dia, temos um número de estruturas da Web que criar sites de flexíveis e escalonáveis de maneira oportuna, como ASP.NET MVC e formulários da Web. Por outro lado, ele fica mais difícil localizar problemas nas páginas porque o IDE não oferece suporte a exibição de designer em páginas com base em modelo e o conteúdo dinâmico. Portanto, esses sites possuem ser aberto em um navegador para ver como eles aparecem para um usuário.
> 
> Os desenvolvedores da Web usam ferramentas externas para localizar problemas que são executados regularmente no navegador. Em seguida, eles retornam ao IDE e iniciar a correção. Isso e voltar atividade entre o IDE, o navegador e as ferramentas de criação de perfil pode ser ineficiente e, às vezes, requer uma nova implantação e a cada vez que você deseja reproduzir um problema de limpeza do cache.
> 
> Inspetor de página preenche uma lacuna no desenvolvimento da Web entre o cliente (ferramentas do navegador) e o servidor (ASP.NET e código-fonte) porque reúne o melhor dos dois mundos com um conjunto combinado de recursos.
> 
> Usando o Inspetor de página, você pode ver quais elementos nos arquivos de origem (incluindo o código do lado do servidor) produziram a marcação HTML a ser renderizado no navegador. O Page Inspector também permite modificar as propriedades CSS e atributos do elemento DOM para ver as alterações refletidas imediatamente no navegador.
> 
> Neste laboratório prático irá orientá-lo por meio dos recursos do Page Inspector e mostram como você pode usá-los para corrigir problemas em aplicativos da Web. **Este laboratório contém dois exercícios usando fluxos semelhantes, mas diferentes tecnologias de direcionamento. Se você for um desenvolvedor de MVC do ASP.NET, siga o exercício um; Se você for um exercício de acompanhamento de desenvolvedor WebForms dois**.
> 
> Este laboratório orienta os aprimoramentos e novos recursos descritos anteriormente, aplicando alterações secundárias a um aplicativo Web de exemplo fornecido na pasta de origem.
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível em [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá como:

- Decomponha um Site usando o Inspetor de página
- Inspecionar e visualizar as alterações de estilos CSS com o Page Inspector
- Detectar e corrigir problemas em suas páginas da web usando o Inspetor de página

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

Você deve ter os seguintes itens para concluir este laboratório:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).
- Internet Explorer 9 ou superior

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Exercício 1: Usando o Inspetor de página em projetos do ASP.NET MVC](#Exercise1)
2. [Exercício 2: Usar o Inspetor de página em projetos de WebForms](#Exercise2)

> [!NOTE]
> Cada exercício é acompanhado por uma solução inicial, localizada na pasta de início do exercício, que permite que você siga cada exercício independentemente dos outros. Dentro do código-fonte para um exercício, você também encontrará uma pasta final que contém uma solução do Visual Studio com o código que é o resultado de concluir as etapas no exercício correspondente. Você pode usar essas soluções como uma diretriz se você precisar de ajuda adicional ao trabalhar com este laboratório prático.


Tempo estimado para concluir este laboratório: **30 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Exercício 1: Usando o Inspetor de página em projetos do ASP.NET MVC

Neste exercício, você aprenderá como visualizar e depurar um **ASP.NET MVC 4** solução usando **Page Inspector**. Primeiro, você executará uma breve volta a ferramenta para aprender os recursos que facilitam a processo de depuração da Web. Em seguida, você trabalhará em uma página da web que contém os problemas de estilo. Você aprenderá a usar o Inspetor de página para encontrar o código-fonte que gera o problema e corrigi-lo.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tarefa 1 - Explorando o Inspetor de página

Nesta tarefa, você aprenderá como usar o Inspetor de página no contexto de um projeto ASP.NET MVC 4 que mostra uma galeria de fotos.

1. Abra o **começar** solução localizado em **origem/Ex1-MVC4/início/** pasta.

   1. Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. No Solution Explorer, localize **index. cshtml** exibir sob o **/Views/Home** pasta do projeto, clique duas vezes e selecione **exibir no Page Inspector**.

    ![Selecionar um arquivo para visualizar no Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image1.png "selecionando um arquivo para visualizar no Inspetor de página")

    *Selecionar um arquivo para visualizar no Inspetor de página*
3. A janela do Inspetor de página mostrará as */Home/Index* URL mapeada para a fonte de exibição que você selecionou.

    ![ThefirstcontactwithPageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *O primeiro contato com o Page Inspector*

    A ferramenta de Inspetor de página é integrada em seu ambiente do Visual Studio. O Inspetor contiver um navegador incorporado, junto com um criador de perfil avançado do HTML. Observe que você não precisa executar a solução para ver a aparência de suas páginas.

    > [!NOTE]
    > Quando a largura do navegador Inspetor de página é menor que a largura da página aberta, você não verá a página corretamente. Se isso acontecer, ajuste a largura do Inspetor de página.
4. Clique o **arquivos** guia no Inspetor de página.

    Você verá todos os arquivos de origem que estiver compondo a página de índice. Esse recurso ajuda a identificar todos os elementos em um relance, especialmente quando você estiver trabalhando com exibições parciais e modelos. Observe que você também pode abrir cada um dos arquivos se você clicar nos links.

    ![A guia arquivos](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *A guia arquivos*
5. Clique o **ativar/desativar modo de inspeção** botão, localizado à esquerda das guias.

    Essa ferramenta permitirá que você selecione qualquer elemento da página e veja seu código HTML e Razor.

    ![Toggle-Inspection-Mode-button](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Botão de alternância de modo de inspeção*
6. No navegador Inspetor de página, mova o ponteiro do mouse sobre os elementos da página. Enquanto você move o ponteiro do mouse sobre qualquer parte da página renderizada, o tipo de elemento é exibido e o código ou a marcação de origem correspondente é realçado no editor do Visual Studio.

    ![Inspectionmodeinaction](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Modo de inspeção em ação*

    > [!NOTE]
    > Não é maximizar a janela do Inspetor de página ou não ser capaz de ver a guia de visualização que mostra o código-fonte. Se você clicar em um elemento no Page Inspector quando ela estiver maximizada, o código-fonte da seleção será exibida, mas ela ocultará a janela Inspetor de página.

    Se você preste atenção para o **index. cshtml** arquivo, você vai notar que a parte do código-fonte que gera o elemento selecionado é realçada. Esse recurso facilita a edição de arquivos de origem longo, fornecendo uma maneira rápida e direta para acessar o código.

    ![Inspectingelements](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Inspecionar elementos*
7. Clique o **ativar/desativar modo de inspeção** botão (![selecione a guia HTML para exibir o código HTML renderizado no navegador Inspetor de página.] (using-page-inspector-in-visual-studio-2012/_static/image7.png "Selecione a guia HTML para exibir o código HTML renderizado no navegador Inspetor de página.") ) para desabilitar o cursor.
8. Selecione o **HTML** guia para exibir o código HTML renderizado no navegador Inspetor de página.
9. Na marcação HTML, localize o item de lista com o link Koala e selecioná-lo.

    Observe que quando você seleciona o código, a saída correspondente é realçada automaticamente no navegador. Esse recurso é útil para ver como um bloco HTML é renderizado na página.

    ![Selecionar elemento HTML na página](using-page-inspector-in-visual-studio-2012/_static/image8.png "elemento selecionando HTML na página")

    *Selecionando um elemento HTML na página*
10. Clique o **ativar/desativar modo de inspeção** botão Habilitar *modo de inspeção* e clique na barra de navegação. À direita do código HTML, no painel Estilos, você verá uma lista com os estilos CSS aplicados ao elemento selecionado.

    > [!NOTE]
    > Como o cabeçalho é uma parte do layout do site, o Page Inspector também abrirá \_arquivo layout. cshtml e realce o segmento de código afetado.

    ![Discoveringstyles](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Descoberta de estilos e arquivos de origem de um elemento selecionado*
11. Com o ponteiro de inspeção de alternância habilitado, mova o ponteiro do mouse abaixo da barra azul em destaque e clique no círculo metade.

    ![Selecionando um elemento](using-page-inspector-in-visual-studio-2012/_static/image10.png "selecionando um elemento")

    *Selecionando um elemento*
12. No painel Estilos, localize o **imagem de plano de fundo** item sob o **conteúdo. main** grupo. **Desmarque a opção** as **imagem de plano de fundo** e ver o que acontece. Você observará que o navegador refletirá as alterações imediatamente e o círculo está oculto.

    > [!NOTE]
    > As alterações aplicadas na guia estilos de Inspetor de página não afetam a folha de estilos original. Você pode desmarcar estilos ou alterar seus valores quantas vezes quiser, mas eles serão restaurados depois de atualizar a página.

    ![Habilitando e desabilitando estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "habilitando e desabilitando estilos CSS")

    *Habilitando e desabilitando estilos CSS*
13. Agora, clique o '**seu logotipo aqui**' texto no cabeçalho usando o modo de inspeção.
14. No **estilos** guia, localize a **tamanho da fonte** CSS de atributo no **título .site** grupo. Clique duas vezes o valor do atributo e substitua o valor de 2,3 em com **em 3**, em seguida, pressione **ENTER**. Observe que o título parece maior.

    ![Alterando os valores CSS no Inspetor de página](using-page-inspector-in-visual-studio-2012/_static/image12.png "valores de alteração de CSS no Inspetor de página")

    *Alterando os valores CSS no Inspetor de página*
15. Clique o **rastrear estilos** guia, localizada no painel à direita de Inspetor de página. Isso é uma maneira alternativa para ver todos os estilos aplicados à seleção, ordenada pelo nome do atributo.

    ![CSSstylestracing](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Rastreamento de estilos CSS do elemento selecionado*
16. Outro recurso do Inspetor de página é o painel de Layout. Usando o modo de inspeção, selecione a barra de navegação e, em seguida, clique no **Layout** guia no painel à direita. Você verá o tamanho exato do elemento selecionado, bem como seu tamanho de deslocamento, margem, preenchimento e borda. Observe que você também pode modificar os valores dessa exibição.

    ![Layout do elemento no Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image14.png "layout do elemento no Page Inspector")

    *Layout do elemento no Page Inspector*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tarefa 2 – localizar e corrigir problemas de estilo em Galeria de fotos

Como diagnosticar problemas de páginas da Web com versões anteriores do Visual Studio? Você provavelmente está familiarizado com a web, ferramentas de depuração que são executados fora do IDE do Visual Studio, como as ferramentas de desenvolvedor do Internet Explorer ou o Firebug. Navegadores só entendem HTML, scripts e os estilos, embora uma estrutura subjacente gera o HTML que será renderizado. Por esse motivo, você geralmente precisará implantar todo o site para ver como as páginas da web.

Você provavelmente tinha seguiu estas etapas quando você quisesse detectar e corrigir um problema em seu site da web:

1. Executar a solução do Visual Studio, ou implantar a página no servidor web.
2. No navegador, abra as ferramentas de desenvolvedor, você usa, ou simplesmente abre o código-fonte e os estilos e tenta corresponder o problema. Para localizar os arquivos envolvidos, você teria que usar o &quot;pesquisa&quot; ou &quot;pesquisa nos arquivos&quot; recursos com o nome das classes de estilo.
3. Depois que o erro é detectado, pare o navegador da Web e o servidor.
4. Limpe o cache do navegador.
5. Retorne ao Visual Studio para aplicar uma correção. Repita todas as etapas para testar.

Como não há nenhum WYSIWYG real no ASP.NET MVC 4, a maioria dos problemas de estilo é detectada em um estágio posterior, depois de executar ou implantar o aplicativo web. Agora, com o Page Inspector, é possível visualizar qualquer página sem executar a solução.

Nesta tarefa, você irá usar o Inspetor de página e corrigir alguns problemas que o aplicativo de galeria de fotos.

1. Usando o Inspetor de página, localize o **registre** e o **faça logon no** links no lado esquerdo do cabeçalho.

    Observe que os links não são exibidos no local esperado na direita, e elas serão mostradas como uma lista com marcadores. Agora você alinha os links à direita e redefinir esse estilo-las adequadamente.

    ![Localizando o Log e registre-se nos links](using-page-inspector-in-visual-studio-2012/_static/image15.png "localizando o Log e registre-se nos links")

    *Localizando o Log e registre-se nos links*
2. Com o modo de inspeção de alternância selecionado, clique em próximo ao, mas não no, o link de registro para abrir seu código.

    Observe que o código-fonte dos links está localizado na  **\_Loginpartial** do arquivo, não o index. cshtml, nem o \_layout. cshtml, quais são os locais que você pode procurar em primeiro lugar. Você foram colocados diretamente no arquivo de código-fonte correto.
3. No **estilos** guia, localize e clique o **<section> #login</section>** item, que é o contêiner HTML para esses links.

    Observe que o **#login** estilo automaticamente está localizado em **CSS** depois de clicar em. Além disso, o código agora é realçado.

    ![Selecionando os estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "selecionando os estilos CSS")

    *Selecionando os estilos CSS*
4. Remova o **alinhamento de texto** do atributo no código realçado, removendo os caracteres de abertura e fechamento e salve o **CSS** arquivo.

    Inspetor de página está ciente de todos os diferentes arquivos que compõem a página atual, e ele pode detectar quando qualquer um desses arquivos são alterados. Ele alerta sempre que a página atual no navegador não está em sincronia com os arquivos de origem.
5. No navegador Inspetor de página, clique na barra localizada abaixo da barra de endereço para recarregar a página.

    ![Recarregar a página](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Recarregar a página*

    Os links agora estão à direita, mas elas ainda se pareçam com uma lista com marcadores. Agora, você removerá os marcadores e alinhar os links horizontalmente.

    ![Página atualizada](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Página atualizada*
6. Usando o modo de inspeção, selecione qualquer uma da **&lt;li&gt;** itens que contêm os &quot;registrar&quot; e &quot;entrar&quot; links. Em seguida, clique o  **&lt;seção&gt; #login** item para o acesso **Styles** código.

    ![Localizando o estilo](using-page-inspector-in-visual-studio-2012/_static/image19.png "localizando o estilo")

    *Localizando o estilo*
7. Na **Style. CSS**, remova o código para **#login li** itens. O estilo que você está adicionando será ocultar o marcador e exibir os itens horizontalmente.

    ![Reformular os links de logon](using-page-inspector-in-visual-studio-2012/_static/image20.png "estilização os links de logon")

    *Reformular os links de logon*
8. Salve **Style. CSS** de arquivo e clique uma vez na barra de localizada abaixo do endereço para recarregar a página. Observe que os links aparecem corretamente.

    ![Links alinhado à direita](using-page-inspector-in-visual-studio-2012/_static/image21.png "Links alinhado à direita")

    *Links alinhado à direita*
9. Por fim, você irá alterar o título do cabeçalho. Use o modo de inspeção para clicar **seu logotipo aqui** texto e get para o código-fonte que gera a ele.
10. Agora você está na  **\_layout. cshtml**, substitua '**seu logotipo aqui**'texto com'**da Galeria de fotos**'. Salve e atualize o navegador Inspetor de página.

    ![Atribuir um novo título](using-page-inspector-in-visual-studio-2012/_static/image22.png "atribuindo um novo título")

    *Atribuir um novo título*

    ![PhotoGallerypage](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Página da Galeria de fotos atualizada*
11. Por fim, selecione o **Galeriadefotos** projeto e pressione **F5** para executar o aplicativo. Fazer check-out de todas as alterações funcionem conforme o esperado.

* * *

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Exercício 2: Usar o Inspetor de página em projetos de WebForms

Neste exercício, você aprenderá como visualizar e depurar uma solução de formulários da Web usando o Inspetor de página. Primeiro, você executará uma breve volta a ferramenta para aprender os recursos do Page Inspector que facilitam a processo de depuração da Web. Em seguida, você trabalhará em uma página da web que contém os problemas de estilo. Você aprenderá a usar o Inspetor de página para encontrar o código-fonte que gera o problema e corrigi-lo.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tarefa 1 - Explorando o Inspetor de página

Nesta tarefa, você aprenderá como usar os recursos do Page Inspector no contexto de um projeto de formulários da Web que mostra uma galeria de fotos.

1. Abra o **começar** solução localizado em **origem/o Ex2-WebForms/início/** pasta.

   1. Você precisará baixar alguns pacotes do NuGet ausentes antes de continuar. Para fazer isso, clique o **Project** menu e selecione **Manage NuGet Packages**.
   2. No **gerenciar pacotes NuGet** caixa de diálogo, clique em **restaurar** para baixar os pacotes ausentes.
   3. Por fim, compile a solução clicando **construir** | **compilar solução**.

      > [!NOTE]
      > Uma das vantagens de usar o NuGet é que você não precisa enviar todas as bibliotecas em seu projeto, reduzindo o tamanho do projeto. Com o NuGet Power Tools, especificando as versões do pacote no arquivo Packages. config, você será capaz de baixar todas as bibliotecas necessárias na primeira vez em que você executar o projeto. É por isso você terá que executar essas etapas depois de abrir uma solução existente neste laboratório.
2. No Solution Explorer, localize **default. aspx** página, clique duas vezes e selecione **exibir no Page Inspector**.

    ![Abrindo default. aspx com o Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image24.png "abrindo default. aspx com o Page Inspector")

    *Abrindo default. aspx com o Page Inspector*
3. A janela do Inspetor de página mostrará o default. aspx.

    ![Exibição de Default. aspx no Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image25.png "exibindo default. aspx no Inspetor de página")

    *Exibição de Default. aspx no Inspetor de página*

    A ferramenta de Inspetor de página é integrada em seu ambiente do Visual Studio. O Inspetor contiver um navegador incorporado, junto com um criador de perfil avançado de HTML que mostrará o código selecionado. Observe que você não precisa executar a solução para ver a aparência de suas páginas.

    > [!NOTE]
    > Quando a largura do navegador Inspetor de página é menor que a largura da página aberta, você não verá a página corretamente. Se isso acontecer, ajuste a largura do Inspetor de página.
4. Clique o **arquivos** guia no Inspetor de página.

    Você verá todos os arquivos de origem que estiver compondo padrão página renderizada. Esse é um recurso útil para identificar todos os elementos em um relance, especialmente quando você estiver trabalhando com controles de usuário e páginas mestras. Observe que você também pode navegar até cada um dos arquivos.

    ![A guia arquivos](using-page-inspector-in-visual-studio-2012/_static/image26.png "guia a arquivos")

    *A guia arquivos*
5. Clique o **ativar/desativar modo de inspeção** botão, localizado à esquerda das guias.

    Essa ferramenta permitirá que você selecione qualquer elemento da página e veja seu código e. aspx código-fonte HTML.

    ![Botão de modo de inspeção de alternância](using-page-inspector-in-visual-studio-2012/_static/image27.png "botão Ativar/desativar modo de inspeção")

    *Botão de alternância de modo de inspeção*
6. No navegador Inspetor de página, mova o mouse sobre os elementos da página. Enquanto você move o ponteiro do mouse sobre qualquer parte da página renderizada, o tipo de elemento é exibido e o código ou a marcação de origem correspondente é realçado no editor do Visual Studio.

    ![Modo de inspeção em ação](using-page-inspector-in-visual-studio-2012/_static/image28.png "modo de inspeção em ação")

    *Modo de inspeção em ação*

    > [!NOTE]
    > Não é maximizar a janela do Inspetor de página ou não ser capaz de ver a guia de visualização que mostra o código-fonte. Se você clicar em um elemento no Page Inspector quando ela estiver maximizada, o código-fonte da seleção será exibida, mas ela ocultará a janela Inspetor de página.

    Se você preste atenção à **default. aspx** arquivo, você vai notar que a parte do código-fonte que gera o elemento selecionado é realçada. Esse recurso facilita a edição dos arquivos de origem longo, fornecendo uma maneira rápida e direta para acessar o código.

    ![Inspecionar elementos](using-page-inspector-in-visual-studio-2012/_static/image29.png "inspecionar elementos")

    *Inspecionar elementos*
7. Clique o **ativar/desativar modo de inspeção** botão (![Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.] (using-page-inspector-in-visual-studio-2012/_static/image30.png "Select-the-HTML-tab-to-display-the-HTML-code-rendered-in-the-Page-Inspector-browser.") ), localizadas em guias de Inspetor de página, para desabilitar o cursor.
8. Selecione o **HTML** guia para exibir o código HTML renderizado no navegador Inspetor de página.
9. No código HTML, localize o item de lista com o link Koala e selecioná-lo.

    Observe que, quando você seleciona o código, a saída correspondente é realçado automaticamente do navegador. Esse recurso é útil para ver como um bloco HTML é renderizado na página.

    ![Selecionando um elemento HTML na página](using-page-inspector-in-visual-studio-2012/_static/image31.png "selecionando um elemento HTML na página")

    *Selecionando um elemento HTML na página*
10. Clique o **ativar/desativar modo de inspeção** botão Habilitar *modo de inspeção* e clique na barra de navegação. À direita do código HTML, no painel Estilos, você verá uma lista com os estilos CSS aplicados ao elemento selecionado.

    > [!NOTE]
    > como o cabeçalho é uma parte do layout do site, o Inspetor de página também abrir o arquivo site e realce o segmento de código afetado.

    ![DiscoveringstylesWebForms](using-page-inspector-in-visual-studio-2012/_static/image32.png "descoberta estilos e arquivos de origem de um elemento selecionado")

    *Descoberta de estilos e arquivos de origem de um elemento selecionado*
11. Com o ponteiro de inspeção de alternância habilitado, mova o ponteiro do mouse abaixo da barra de menu e clique no círculo metade em branco.

    ![Selecionando um elemento](using-page-inspector-in-visual-studio-2012/_static/image33.png "selecionando um elemento")

    *Selecionando um elemento*
12. No painel Estilos, localize o **imagem de plano de fundo** item sob o **conteúdo. main** grupo. **Desmarque a opção** as **imagem de plano de fundo** e ver o que acontece. Você observará que o navegador refletirá as alterações imediatamente e o círculo está oculto.

    > [!NOTE]
    > As alterações aplicadas na guia estilos de Inspetor de página não afetam a folha de estilos original. Você pode desmarcar estilos ou alterar seus valores quantas vezes quiser, mas eles serão restaurados depois de atualizar a página.

    ![Habilitando e desabilitando CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "habilitando e desabilitando estilos CSS")

    *Habilitando e desabilitando estilos CSS*
13. Agora, clique o '**sua** **logotipo aqui '** texto no cabeçalho usando o modo de inspeção.
14. No **estilos** guia, localize a **tamanho da fonte** CSS de atributo no **título .site** grupo. Clique duas vezes o atributo de uma vez para editar seu valor. Substitua o 2.3em o valor pelo **3em**, e pressione ENTER. Observe que o título parece maior.

    ![Alterando os valores CSS no Page Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "valores de alteração de CSS no Inspetor de página")

    *Alterando os valores CSS no Inspetor de página*
15. Clique o **rastrear estilos** guia, localizada no painel à direita de Inspetor de página. Isso é uma maneira alternativa para ver todos os estilos aplicados à seleção, ordenada pelo nome do atributo.

    ![Rastreamento de estilos CSS do elemento selecionado](using-page-inspector-in-visual-studio-2012/_static/image36.png "rastreamento de estilos CSS do elemento selecionado")

    *Rastreamento de estilos CSS do elemento selecionado*
16. Outro recurso do Inspetor de página é o painel de Layout. Usando o modo de inspeção, selecione a barra de navegação e, em seguida, clique no **Layout** guia no painel à direita. Você verá o tamanho exato do elemento selecionado, bem como seu tamanho de deslocamento, margem, preenchimento e borda. Observe que você também pode modificar os valores dessa exibição.

    ![Layout do elemento no Page Inspector](using-page-inspector-in-visual-studio-2012/_static/image37.png "layout do elemento no Page Inspector")

    *Layout do elemento no Page Inspector*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tarefa 2 – localizar e corrigir problemas de estilo em Galeria de fotos

Como diagnosticar problemas de páginas da Web com versões anteriores do Visual Studio? Você provavelmente está familiarizado com a web, ferramentas de depuração que são executados fora do IDE do Visual Studio, como as ferramentas de desenvolvedor do Internet Explorer ou o Firebug. Navegadores só entendem HTML, scripts e os estilos, embora uma estrutura subjacente gera o HTML que será renderizado. Por esse motivo, você geralmente precisará implantar todo o site para ver como as páginas da web.

Você provavelmente tinha seguiu estas etapas quando você quisesse detectar e corrigir um problema em seu site da web:

1. Executar a solução do Visual Studio, ou implantar a página no servidor web.
2. No navegador, abra as ferramentas de desenvolvedor, você usa, ou simplesmente abre o código-fonte e os estilos e tenta corresponder o problema. Para localizar os arquivos envolvidos, você teria que usar o &quot;pesquisa&quot; ou &quot;pesquisa nos arquivos&quot; recursos com o nome das classes de estilo.
3. Depois que o erro é detectado, pare o navegador da Web e o servidor.
4. Limpe o cache do navegador.
5. Retorne ao Visual Studio para aplicar uma correção. Repita todas as etapas para testar.

Como é não real WYSIWYG no ASP.NET WebForms, alguns problemas de estilo são detectados em um estágio posterior, depois de executar ou implantar. Agora, com o Page Inspector, é possível visualizar qualquer página sem executar a solução.

Nesta tarefa, você usará o Inspetor de página para corrigir alguns problemas do aplicativo Galeria de fotos. As etapas a seguir, você detectará e corrigir rapidamente a algum problema de estilo simples no cabeçalho.

1. Usando a inspeção de página, localize o **registre** e o **Log em** links no lado esquerdo do cabeçalho.

    Observe que o link não é exibido no local esperado na direita. Agora você alinha o link para a direita e redefinir esse estilo adequadamente.

    ![Faça logon no link posicionado à esquerda](using-page-inspector-in-visual-studio-2012/_static/image38.png "faça logon no link posicionado à esquerda")

    *Link de log em posicionado à esquerda*
2. Com o modo de inspeção de alternância selecionado, selecione o link de Log em abrir seu código.

    Observe que o código de origem do link está localizado na **Master** arquivo, não na página Default. aspx, que é o lugar, você pode procurar em primeiro lugar; você foram colocadas diretamente no arquivo de origem correto.
3. No **estilos** guia, localize e clique o  **&lt;seção&gt; #login** item, que é o contêiner HTML para esses links.

    Observe que o **#login** estilo automaticamente está localizado em **CSS** depois de clicar em. Além disso, o código agora é realçado.

    ![Selecionando os estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "selecionando os estilos CSS")

    *Selecionando os estilos CSS*
4. Remova o **alinhamento de texto** do atributo no código realçado, removendo os caracteres de abertura e fechamento e salve o **CSS** arquivo.

    Inspetor de página está ciente de todos os diferentes arquivos que compõem a página atual, e ele pode detectar quando qualquer um desses arquivos são alterados. Ele alerta sempre que a página atual no navegador não está em sincronia com os arquivos de origem.
5. No navegador Inspetor de página, clique na barra localizada abaixo da barra de endereços para salvar as alterações e recarregue a página.

    ![Reloadingthepage](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Recarregar a página*

    Os links agora estão à direita, mas elas ainda se pareçam com uma lista com marcadores. Agora, você removerá os marcadores e alinhar os links horizontalmente.

    ![Página atualizada](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Página atualizada*
6. Usando o modo de inspeção, selecione qualquer uma da **&lt;li&gt;** itens que contêm os &quot;registrar&quot; e &quot;entrar&quot; links. Em seguida, clique o  **&lt;seção&gt; #login** item para o acesso **Styles** código.

    ![Localizando o estilo](using-page-inspector-in-visual-studio-2012/_static/image42.png "localizando o estilo")

    *Localizando o estilo*
7. Na **Style. CSS**, remova o código para **#login li** itens. O estilo que você está adicionando será ocultar o marcador e exibir os itens horizontalmente.

    ![Reformular os links de logon](using-page-inspector-in-visual-studio-2012/_static/image43.png "estilização os links de logon")

    *Reformular os links de logon*
8. Salve **Style. CSS** de arquivo e clique uma vez na barra de localizada abaixo do endereço para recarregar a página. Observe que os links aparecem corretamente.

    ![Links alinhado à direita](using-page-inspector-in-visual-studio-2012/_static/image44.png "Links alinhado à direita")

    *Links alinhado à direita*
9. Por fim, você irá alterar o título do cabeçalho. Em vez de buscar a '**seu logotipo aqui '** texto em todos os arquivos, use o modo de inspeção para clique no texto e obter o código-fonte que gera a ele.

    ![Localizando o título do site](using-page-inspector-in-visual-studio-2012/_static/image45.png "localizando o título do site")

    *Localizando o título do site*
10. Agora você está na **Master**, substitua o '**seu logotipo aqui**'texto com'**da Galeria de fotos**'. Salve e atualize o navegador Inspetor de página.

    ![Página da Galeria de fotos atualizada](using-page-inspector-in-visual-studio-2012/_static/image46.png "página Galeria de fotos atualizada")

    *Página da Galeria de fotos atualizada*
11. Por último, marque **F5** para executar o aplicativo o check-out de todas as alterações funcionem conforme o esperado.

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático, você tem aprendeu a usar o Inspetor de página para visualizar seu aplicativo da Web sem ter que recompilar e executar o site da Web em um navegador. Além disso, você tem adquiridos como rapidamente encontrar e corrigir bugs, acessando diretamente na saída renderizada no código-fonte.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalação do Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outra &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guia você pelas etapas necessárias para instalar *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-lo e pesquisar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar o Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "instalar o Visual Studio Express")

    *Instalar o Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação for concluído.

    ![Progresso da instalação](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Instalação concluída*
7. Clique em **Exit** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** de tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco da Web](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express para o bloco da Web*
