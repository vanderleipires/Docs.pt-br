---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: "O que há de novo no ASP.NET e desenvolvimento da Web no Visual Studio 2012 | Microsoft Docs"
author: rick-anderson
description: "A nova versão do Visual Studio apresenta uma série de melhorias que se concentrou em melhorar a experiência e o desempenho ao trabalhar com tecnologias da Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: e57f1200aaa207c9109f2832cbf88629ed385bb5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>O que há de novo no ASP.NET e desenvolvimento da Web no Visual Studio 2012
====================
por [Web Camps Team](https://twitter.com/webcamps)

> A nova versão do Visual Studio apresenta uma série de melhorias que se concentrou em melhorar a experiência e o desempenho ao trabalhar com tecnologias da Web. Editores de CSS, JavaScript e HTML do Visual Studio foi completamente reformuladas para incluir muitas os auxílios de código mais acessado, como IntelliSense e recuo automático. Em relação ao desempenho, empacotamento e minimização agora estão integrados como tempo de carregamento de recursos internos para reduzir facilmente a página.
> 
> O Visual Studio permite que você trabalhe com as tecnologias mais recentes do site. Você pode usar vários navegadores CSS3 trechos para certificar-se de que seu site funciona independentemente da plataforma de cliente e também aproveitar os novos recursos e elementos de HTML5.
> 
> Gravação e criação de perfil de código JavaScript devem ser mais fácil com esta versão do Visual Studio. Listas do IntelliSense, recursos integrados de navegação e de documentação XML agora estão disponíveis para o código JavaScript. Agora você tem o catálogo de JavaScript ao seu alcance. Além disso, você pode verificar a conformidade da ECMAScript5 com seus scripts e detectar erros de sintaxe em uma etapa inicial.
> 
> Por último, mas não menos importante, esta versão do Visual Studio implementa internos de empacotamento e minimização. Arquivos de script e folhas de estilo serão compactadas e compactadas para que o site executa com mais rapidez.
> 
> Este laboratório orienta os aperfeiçoamentos e novos recursos descritos anteriormente, aplicando alterações secundárias a um aplicativo da Web de exemplo fornecido na pasta de origem.
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Este laboratório prático, você aprenderá como:

- Use os novos recursos e aprimoramentos no editor de CSS
- Use os novos recursos e aprimoramentos no editor de HTML
- Use os novos recursos e aprimoramentos no editor de JavaScript
- Configurar e usar o empacotamento e minimização

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leitura [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).
- [O Windows PowerShell](https://support.microsoft.com/kb/968930/) (para scripts de instalação - já instalados no Windows 8 e Windows Server 2008 R2)
- [O Internet Explorer 10](https://windows.microsoft.com/en-US/internet-explorer/products/ie/home) - ou um navegador compatível com HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Exercício 1: O que há de novo no Editor de CSS](#Exercise1)
2. [Exercício 2: O que há de novo no Editor de HTML](#Exercise2)
3. [Exercício 3: O que há de novo no Editor de JavaScript](#Exercise3)
4. [Exercício 4: Empacotamento e minimização](#Exercise4)

Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Exercício 1: O que há de novo no Editor de CSS

Os desenvolvedores da Web devem estar familiarizados com muitas das dificuldades relacionadas à edição de CSS. Uma das maiores questões de folhas de estilos é compatibilidade entre navegadores. Isso geralmente acontece que, depois de aplicar estilos a seu site, você notar que parece diferente se você abri-lo em outro dispositivo ou navegador. Portanto, você pode gastar um tempo considerável para corrigir esses problemas visual para perceber que, quando você finalmente utilizá-lo em um navegador, ela é quebrada em outros.

O Visual Studio agora inclui recursos que ajudam os desenvolvedores acessar, trabalhar e organizar folhas de estilos CSS de forma eficiente. Ao longo deste exercício, você atende os novos recursos para uma organização efetivada e a edição, bem como os trechos de código CSS3 para compatibilidade entre navegadores.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Tarefa 1 - novos recursos do Editor

Nesta tarefa, você descobrirá que os novos recursos do Editor de CSS. Este novo editor ajudará você a aumentar sua produtividade, aproveitando o recuo inteligente novo, os comentários de código aperfeiçoada e a lista aprimorada do IntelliSense.

1. Iniciar **Visual Studio** e abra o **WhatsNewASPNET.sln** solução localizada no **Source\WhatsNewASPNET** pasta deste laboratório.
2. No Solution Explorer, abra o **Site.css** arquivo localizado sob o **estilos** pasta. Verifique se o **Editor de texto** ferramentas são visíveis na barra de ferramentas. Para fazer isso, selecione o **exibição** | **barras de ferramentas** opção de menu e verifique o **Editor de texto** opções. Você observará que, desde que essa nova versão, o **comentário** botão (![botão comentário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) e o **Uncomment** botão (![Descomente-botão](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) também serão habilitados para o editor de CSS.

    ![Habilitando o Editor e ferramentas CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "habilitando o Editor e ferramentas CSS")

    *Habilitando o Editor e ferramentas CSS*
3. Role o código e selecione qualquer definição de classe CSS. Clique o **comentário** (![botão comentário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) botão para comentar as linhas selecionadas. Em seguida, clique no **Uncomment** (![botão Descomente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) botão Desfazer as alterações.
4. Clique o **recolher** (![recolher](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) e **expandir** (![expanda](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) botões localizados na margem esquerda do texto. Observe que agora você pode ocultar os estilos que você não usar para ter uma exibição de limpeza.

    ![Recolhendo classes CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "classes CSS recolhendo")

    *Recolhendo classes CSS*
5. Certifique-se de que o recurso de recuo inteligente está habilitado. Selecione o **ferramentas** | **opções** opção de menu e selecione o **Editor de texto** | **CSS**  |  **Formatação** página no painel esquerdo da tela. Verifique o **recuo hierárquico** opção.

    ![Habilitando o recuo hierárquico](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "habilitando recuo hierárquico")

    *Habilitando o recuo hierárquico*
6. Localize a definição de classe principal (. Main) e acrescente um estilo para elementos div. Você observará que o código automaticamente, alinha ajudar os usuários para localizar o pai classes em um relance.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Alinhamento hierárquico em CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "alinhamento hierárquico em CSS")

    *Alinhamento hierárquico em CSS*
7. Dentro de **. Main div** classe, localize o cursor no final do **border: 0px;** e pressione **Enter** para exibir a lista do IntelliSense. Comece a digitar **superior** e observe como a lista é filtrada conforme você digita. A lista exibirá os elementos que contêm **superior** em qualquer parte da palavra (nas versões anteriores do Visual Studio, a lista é filtrada pelos itens que *começar* com o termo).

    ![Aprimoramentos de IntelliSense no CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "aprimoramentos de IntelliSense no CSS")

    *Aprimoramentos de IntelliSense no CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Tarefa 2 - o seletor de cores

Nesta tarefa, você descobrirá que o novo seletor de cores CSS integrado ao Visual Studio IntelliSense.

1. Em **Site.css,** localize a definição de classe de cabeçalho (.header) e coloque o cursor ao lado **cor de plano de fundo** atributo entre o &quot;:&quot; e &quot; # &quot; caracteres na linha de código **.**

    ![Localizando o cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "localizando o cursor")

    *Localizando o cursor*
2. Excluir o **vírgula** (:) e gravá-la novamente para exibir o seletor de cores. Observe que as cores primeiro, que você verá as cores mais frequentes do seu site. Se você clicar na cor branca, seu código de cor HTML (#fff) substitui o código de cor atual na folha de estilos.

    ![Seletor de cores](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "seletor de cores")

    *Seletor de cores*
3. Pressione a **expandir** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) botão seletor de cores para exibir o gradiente de cores e, em seguida, arraste o cursor gradiente para selecionar uma cor diferente. Em seguida, clique o **conta-gotas** botão e selecione todas as cores da tela. Observe que o valor de cor do plano de fundo muda dinamicamente enquanto você move o cursor.

    ![Gradiente de seletor de cores](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "gradiente do seletor de cores")

    *Gradiente de seletor de cores*
4. No **opacidade** controle deslizante, mova o seletor para o centro da barra para reduzir a opacidade. Observe que o valor de cor de plano de fundo agora altera sua escala para RGBA.

    ![Seletor de cores opacidade](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "opacidade do seletor de cores")

    *Opacidade do seletor de cores*

    > [!NOTE]
    > A definição de cor RGBA (vermelho, verde, azul, Alpha) no CSS3 permite que você defina o valor da opacidade da cor de um único item. Ao contrário de **opacidade -** um atributo CSS semelhante  **-**  RGBA cores também são compatíveis com os navegadores mais recentes.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Tarefa 3 - trechos de código compatível de CSS

Nesta tarefa, você aprenderá como usar trechos de CSS3 compatíveis entre navegadores para implementar alguns recursos no seu site.

1. No **Site.css** de arquivo, localize o **cabeçalho** CSS (.header) de definição de classe e coloque o cursor abaixo o  **/ \*raio de borda\* /**  espaço reservado para adicionar um novo trecho de código. Pressione **Enter** para exibir a lista do IntelliSense e o tipo **radius** para filtrar a lista. Selecione o **border-radius** opção da lista com um clique duplo e, em seguida, pressione a **guia** chave para inserir o trecho de código. Em seguida, digite um tamanho de radius em pixels e pressione **Enter**. Por exemplo, digite **15px**.

    Os atributos de CSS3 adicionados pelo trecho de código serão renderizado bordas arredondadas na maioria dos navegadores de conformidade do HTML5, incluindo Mozilla e navegadores com base em WebKit.

    ![Usando um trecho de código border-radius](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "usando um trecho de código border-radius")

    *Usando um trecho de código border-radius*
2. Aplicar a mesma **borda** trechos de código no estilo de página (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Pressione **F5** para executar a solução. Observe que cada página agora tem arredondados bordas.

    ![Cantos arredondados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "cantos arredondados")

    *Cantos arredondados*
4. Feche o navegador e retorne ao Visual Studio.
5. Abrir o **Custom.css** arquivo localizado sob o **estilos** pasta e coloque o cursor dentro **img de li ul div.images** definição da classe.
6. Pressione enter para exibir a lista do IntelliSense, tipo **caixa sombra** e pressione a **guia** chave duas vezes para inserir o trecho de código de sombra padrão dentro da definição de classe. Defina os valores de sombra para **10px 10px 5px &#888;**. Em seguida, digite **border-radius** e inserir o trecho de código. Tipo **15px** para definir o tamanho de radius e pressione **ENTER**.

    ![Arredondar os cantos com sombra](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "arredondado cantos com sombra")

    *Cantos arredondados com sombra*

    > [!NOTE]
    > Neste momento, o atributo de sombra é inserido com o prefixo correspondente (moz webkit, s) para dar suporte ao Mozilla e navegadores Webkit (Chrome, Safari, Konkeror).
7. Criar uma nova classe **img:hover de li ul div.images** abaixo o **img de li ul div.images** definição da classe e coloque o cursor dentro dos colchetes **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Tipo **transformação** e pressione a **guia** chave duas vezes para inserir o trecho de código de transformação. Em seguida, insira **rotate(-15deg)** para alterar o valor do ângulo de rotação quando imagens são colocadas.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Pressione **F5** para executar a solução e navegue até a página CSS3. Observe que as imagens tem cantos arredondados e sombras de caixa. Passe o mouse sobre as imagens e assista a girar.

    ![Girar uma imagem de trecho de código de transformação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "transformação trecho girar uma imagem")

    *Transformar o trecho de rotação de uma imagem*

    > [!NOTE]
    > Se você estiver usando o Internet Explorer 10 e não pode ver as sombras, verifique se que o modo de documento é definido como padrões do IE10. Pressione **F12** para abrir as ferramentas de desenvolvedor do Internet Explorer e clique em **modo de documento** para alterar para padrões do IE10.

    ![sobre-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Exercício 2: O que há de novo no Editor de HTML

O Visual Studio tem um editor de HTML aprimorado. Algumas das melhorias incluídas nesta versão são recuo inteligente em documentos HTML, trechos de código do HTML5, início HTML e correspondência de marca de fim e validação de HTML. Ao longo deste exercício, você verá como essas alterações melhorar seu fluência ao trabalhar na marcação de site.

Como o editor de CSS, editor de HTML também foi aprimorado. A maioria dessas melhorias é pequenos que facilitam a vida do desenvolvedor da Web. Coisas como mais trechos de código para HTML5, recuo inteligente, correspondência de marcas de início e término quando edição e validação direcionando o documento HTML DOCTYPE são algumas dessas melhorias.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Tarefa 1 - validação de DOCTYPE aprimorado

O editor de HTML agora tem a capacidade de verificar o tipo de documento da página, mesmo que a definição possa ser na página mestra. Dependendo do DOCTYPE da página, o editor de HTML será validado com o conjunto correto de regras e filtra a lista de IntelliSense considerando os elementos de tipo de documento.

Nesta tarefa, você alterará o tipo de documento de uma página para ver como o comportamento do editor de HTML alterada de acordo.

1. Se ainda não estiver aberto, inicie **Visual Studio** e abra o **WhatsNewASPNET.sln** solução localizada no **Source\WhatsNewASPNET** pasta deste laboratório.
2. Abra o **Site.Master** página.
3. Observe o esquema de destino para a barra de ferramentas de validação. O comportamento do editor de HTML (validação, IntelliSense, etc.) será alterado corretamente de acordo com o tipo de documento selecionado.

    ![Use o Doctype na barra de ferramentas de edição de código HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Doctype Use na barra de ferramentas de edição de código HTML")

    *Use o Doctype na barra de ferramentas de edição de código HTML*
4. Altere o esquema de destino para HTML 4.01.

    ![Alterando tipo de documento na barra de ferramentas de edição de código HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "alterando o tipo de documento na barra de ferramentas de edição de código HTML")

    *Alterando tipo de documento na barra de ferramentas de edição de código HTML*
5. Posicione o cursor sob o **corpo** elemento e comece a digitar o nome de um elemento de HTML5 (por exemplo, **vídeo**). Observe que o elemento não está disponível na lista do IntelliSense.

    ![Elementos de HTML5 não listados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "elementos HTML5 não listados")

    *Elementos de HTML5 não listados*
6. Desfazer as alterações no esquema de destino para validação de ferramentas, escolhendo o tipo de documento: XHTML5 na lista suspensa.

    ![Use o Doctype na barra de ferramentas de edição de código HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Doctype Use na barra de ferramentas de edição de código HTML")

    *Redefinir Doctype na barra de ferramentas de edição de código HTML*
7. Colocar o cursor sob o **corpo** elemento e comece a digitar um elemento HTML5 novamente (por exemplo, como **vídeo**). Observe que os elementos do HTML5 agora estão disponíveis na lista do IntelliSense.

    ![Elementos de HTML5 sendo listados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "elementos HTML5 sendo listados")

    *Elementos de HTML5 sendo listados*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Tarefa 2 - início/término atualização automática de marcas

Agora, o Visual Studio atualiza o HTML abrir ou fechar marcas do elemento que você está editando para corresponder um ao outro. Esse novo recurso melhora a sua produtividade ao editar marcas HTML.

1. Sobre o **Default.aspx** página, adicione um **H3** elemento com um título (por exemplo, o Visual Studio 2012 Rocks!).


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Alterar o **H3** marca e o tipo **H2** ou **H1.**

    Observe que a marca de fim atualiza automaticamente. Você também pode modificar a marca de fim para ver se a marca de início atualiza muito.

    ![Atualização automática da marca de fim](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "atualização automática da marca de fim")

    *Atualização automática da marca de fim*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Tarefa 3 - novos trechos de código do HTML5

O Visual Studio agora inclui vários trechos de código do HTML5. Nesta tarefa, você irá usar alguns dos trechos de código.

1. Adicionar uma nova pasta chamada **áudio** para a raiz da pasta do site. Abra o Windows Explorer e copie qualquer arquivo de áudio para o **áudio** pasta o **WhatsNewASPNET.sln** solução.
2. No **Default.aspx** , localize o cursor sob o Rocks Web11!! Cabeçalho. Tipo **áudio** e pressione a tecla TAB.

    O novo editor de HTML inclui trechos de código para o conteúdo do HTML5. Lembre-se de usar a definição de DOCTYPE adequada para habilitar os trechos de código do HTML5.

    ![Inserir trechos de código do HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "inserir trechos de código do HTML5")

    *Inserir trechos de código do HTML5*
3. Atualize a fonte de áudio para apontar para um arquivo de áudio existente.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Você precisará adicionar o arquivo de áudio para a solução.
4. Pressione **F5** para executar o site e executar o áudio.

    ![Executar o controle de áudio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "executando o controle de áudio")

    *Executar o controle de áudio*

    > [!NOTE]
    > Você também pode tentar trechos de código mais incluídos no Visual Studio, como vídeo figura, etc.
5. Agora, tente inserir um controle em alguma parte da página. Por exemplo, tente inserir uma **GridView** controle, mas em vez de digitar  **&lt;Gri,** comece a digitar  **&lt;GV**. Observe que a lista de IntelliSense mostra o **asp: GridView** controle.

    Agora, o IntelliSense no Editor de HTML fornece pesquisa de título de maiusculas e minúsculas, bem como parcial correspondente (recuperação de todos os elementos que contém o termo).

    ![Inserindo um GridView com listas do IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "inserindo um GridView com listas do IntelliSense")

    *Inserindo um GridView com listas do IntelliSense*

    Se você digitar  **&lt;grade** você obterá todos os itens que correspondem ao termo, mas o Visual Studio irá sugerir o **gridview** controle:

    ![Inserindo um GridView com listas do IntelliSense e a correspondência parcial](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "inserindo um GridView com listas do IntelliSense e a correspondência parcial")

    *Inserindo um GridView com listas do IntelliSense e a correspondência parcial*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Tarefa 4 - marcas inteligentes de Editor de HTML

Outro aprimoramento no Editor de HTML é o recurso de marcas inteligentes. Marcas inteligentes tornam mais fácil executar tarefas de desenvolvimento comuns ou repetitivas em uma base por controle. Esse recurso já estava disponível no Designer de HTML, mas não no Editor de HTML.

1. Abra **Site.Master** e localize o **asp: Menu** elemento. Posicione o cursor na marca de início e observe que o glifo pequeno exibido na parte inferior do elemento - clique nele para abrir o menu de tarefas inteligentes. Observe que você tem acesso rápido a algumas tarefas relacionadas ao controle de Menu.

    ![Inteligentes de tarefas para o controle de Menu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "inteligentes de tarefas para o controle de Menu")

    *Tarefas inteligentes para o controle de Menu*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Tarefa 5: recuo inteligente

Uma das melhores práticas em HTML é recuo dos elementos aninhados para manter o código legível. No Visual Studio 2012, você observará que o editor recua automaticamente os elementos enquanto você estiver escrevendo o código.

> [!NOTE]
> Na versão anterior do Visual Studio, o recuo inteligente estava disponível no editor de XML, mas não no editor de HTML.


1. Certifique-se de que a configuração de recuo no Editor de HTML é definida como o recuo inteligente. Para fazer isso, selecione o **ferramentas | Opções de** opção de menu e selecione o **Editor de texto | HTML | Guias** página no painel esquerdo da tela. Selecione a opção de recuo inteligente.

    ![Configurações do Editor de HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "configurações do Editor de HTML")

    *Configurações do Editor de HTML*
2. Sobre o **Default.aspx** página, remova todo o conteúdo sob o elemento de áudio.
3. Posicione o cursor no final da abertura **áudio** elemento e pressione **ENTER**.

    Observe que a nova posição do cursor tem um nível de recuo adicional.

    ![Inteligente recuo no Editor de HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "inteligente recuo no Editor de HTML")

    *Recuo inteligente no Editor de HTML*
4. Restaurar a marca de áudio com o conteúdo que você tenha removido, ou feche **Default.aspx** sem salvar as alterações.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Tarefa 6 - extrair para controle de usuário

As ferramentas de refatoração incluídas no Visual Studio, como extrair uma parte do código para uma função, são ótimos recursos que facilitam o aprimoramento e a refatoração do código existente. O representante para páginas ASP.NET seria a extração de código HTML a um controle de usuário. Manualmente envolve várias etapas, como criar um novo controle de usuário, mover a seção de código para o controle de usuário, registrando um prefixo de marca do controle de usuário e, finalmente, criando o controle de usuário nas páginas. Agora, o novo *extrair para controle de usuário* ferramenta executa automaticamente todas as etapas para você.

Nesta tarefa, você usará o novo extrair a operação de controle de usuário contextual para gerar um novo controle de usuário do código selecionado.

1. Sobre o **Default.aspx** página, selecione o **H2** e **áudio** elementos.
2. Clique com o botão direito do mouse e selecione **extrair para controle de usuário**.

    ![Extrair a opção de menu de controle de usuário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "extrair a opção de menu de controle de usuário")

    *Extrair a opção de menu de controle de usuário*
3. Digite um nome para o novo controle de usuário. Por exemplo, **Jukebox.ascx**e, em seguida, clique em **Okey**.

    ![Salvando o controle de usuário extraídos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "salvando o controle de usuário extraídos")

    *Salvando o controle de usuário extraídos*
4. Observe que o código selecionado foi extraído para um controle de usuário e o local original do código selecionado foi substituído por uma instância do novo controle de usuário.

    ![Página é atualizada automaticamente para usar o novo controle de usuário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "página é atualizada automaticamente para usar o novo controle de usuário")

    *Página é atualizada automaticamente para usar o novo controle de usuário*
5. Pressione **F5** para executar a página e verifique se o controle funciona.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Exercício 3: O que há de novo no Editor de JavaScript

Gravar ou editar o código JavaScript não é uma tarefa fácil, especialmente quando o aplicativo começa a crescem em tamanho e você estiver lidando com longos de arquivos e centenas de funções. Os desenvolvedores de script normalmente precisam realizar algum trabalho adicional para manter a legibilidade do código e navegar em arquivos. Com a inclusão das bibliotecas JavaScript como jQuery, navegação de script se tornou um desafio devido o comprimento do código.

O Visual Studio renovou o editor do JavaScript com a promessa de tornar o modo código organizados e acessível. Muitos recursos do Visual Studio que já existiam no c# ou VB editores agora são implementados no editor de JavaScript: Ir para definição, recuo automático, documentação e validação quando você estiver escrevendo. Com a lista do IntelliSense renovada, você terá o catálogo de função JavaScript em suas mãos.

Neste exercício, você aprenderá alguns dos novos recursos e aprimoramentos do editor de JavaScript. Você irá procurar arquivos de exemplo e descobrir cada uma das novas características que tornam sua programação JavaScript mais eficiente no Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Tarefa 1: novos recursos do Editor de JavaScript

Essa tarefa apresentará os novos recursos de editor JavaScript, que se concentrar na organização do código e colocar uma melhor experiência do usuário.

1. Se ainda não estiver aberto, inicie **Visual Studio** e abra o **WhatsNewASPNET.sln** solução localizada no **Source\WhatsNewASPNET** pasta deste laboratório.
2. Pressione **F5** para executar o aplicativo, em seguida, clique no link de JavaScript, na barra de navegação. Atualize a página várias vezes e verificar como os incrementos de contador.

    ![Contador de página](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "contador de página")

    *Contador de página*
3. Feche o navegador e volte para o Visual Studio.
4. Abra o **JavaScript.aspx** página e localize o  **&lt;script&gt;**  bloco (mostrado abaixo).

    O código a seguir usa o armazenamento local do HTML5 para armazenar um *pageLoadCount* variável que armazena o número de vezes que a página foi visitada pelo usuário atual. Armazenamento local é um banco de dados de chave-valor do lado do cliente introduzido com o padrão de HTML5. Os dados são salvos no computador local, dentro do navegador do usuário.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Certifique-se de que o DOCTYPE é definido para XHTML5 antes de continuar com as próximas etapas.
5. Editar o código e observe que o IntelliSense para JavaScript inclui recursos de HTML5, como armazenamento local e seus métodos internos.

    ![Recursos de JavaScript HTML5 em JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "recursos JavaScript HTML5 em JavaScript")

    *Recursos de JavaScript HTML5 em JavaScript*
6. Clique em qualquer colchete de abertura (**{**) o script de código e observe que os colchetes são realçados.

    ![Colchetes são realçados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "colchetes são realçados")

    *Colchetes são realçados*
7. Remova a função **testAutoAlign()** (selecione as três linhas e você pode usar **CTRL** + **K**; **CTRL** + **U**) e localize o cursor dentro do código de função. Pressione enter para acrescentar uma segunda linha. Observe que o código é agora **alinhado** e **recuada automaticamente**.

    ![O código JavaScript é automaticamente alinhado](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "código JavaScript é automaticamente alinhado")

    *O código JavaScript é automaticamente alinhado*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Tarefa 2 - validação de JavaScript

Nesta tarefa, você descobrirá que a nova validação de JavaScript para o padrão de ECMAScript5. Esse recurso ajudará você a escrevem compatível com código JavaScript, evitando problemas de script antes da implantação do site.

> [!NOTE]
> Visual Studio 2010 implementado ECMAStript3 conformidade, enquanto o Visual Studio 2012 fornece ECMAScript5 conformidade.


1. Abra **ECMA5script5.js** localizado sob o **Scripts\custom** pasta do projeto. Agora, você testará a validação para ECMAScript5 padrão.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Você pode consultar o &quot; **uso estrito** &quot; direção na primeira linha do arquivo, que permite ECMAScript5 **modo estrito**. Esse modo consiste em um subconjunto da linguagem que esclarece ambiguidades da edição anterior e adiciona alguns novos recursos, como getters e setters, suporte de biblioteca para JSON e mais completa reflexão em Propriedades do objeto.
2. Abra o **lista de erros** se ainda não estiver aberto (**exibição** menu | **Lista de erros**). Observe o **função** declaração está sublinhada. Isso ocorre porque no ECMA5 funções padrão não podem ser aninhadas dentro de estruturas de idioma. O erro lista abaixo, você verá os detalhes de aviso.

    ![Mensagem de erro de validação de JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "mensagem de erro de validação de JavaScript")

    *Mensagem de erro de validação de JavaScript*
3. Comente o  **&quot;uso estrito&quot;**  direção e observe que desaparecem erros, mas permanecem os avisos.
4. Na última linha do arquivo, gravar qualquer cadeia de caracteres como  **&quot;teste&quot;**  (incluir as aspas para indicar que ela é como cadeia de caracteres). Gravar um período próximo a cadeia de caracteres para exibir a lista do IntelliSense e selecione o **trim** opção.

    No padrão de ECMAScript5, variáveis e valores de cadeia de caracteres também tem os métodos de cadeia de caracteres definidos, como cortar, letras maiusculas, pesquisar e substituir.

    ![Lista JavaScript IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "lista JavaScript IntelliSense")

    *Lista do IntelliSense em JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Tarefa 3 - documentação XML para JavaScript

Nesta tarefa, você irá explorar os recursos do Visual Studio para obter a documentação XML em JavaScript. Você verá que a lista de JavaScript IntelliSense agora mostra a documentação XML de cada função. Além disso, você descobrirá que o recurso de navegação em JavaScript.

1. Abra **XMLDoc.js** arquivo localizado em **Scripts/personalizada** pasta do projeto. Este arquivo contém a documentação XML em cada uma das funções JavaScript.

    ![Documentação XML JavaScript integrado ao IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "documentação XML JavaScript integrado ao IntelliSense")

    *Documentação XML JavaScript integrada ao IntelliSense*
2. Abaixo **adicionar** funcionar em **XMLDoc.js** de arquivo, crie uma nova função chamada **teste**.
3. No **teste** funcionar, chame o **multiplicar** função que recebe dois parâmetros. Observe que a caixa de dica de ferramenta mostra o **multiplicar** documentação de função.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Documentação XML para funções JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "documentação XML para funções JavaScript")

    *Documentação XML para funções JavaScript*
4. Conclua a instrução de chamada de função e digite um *ponto* para abrir a lista do IntelliSense no valor retornado. Observe que o Visual Studio está detectando a **retornar** valor na documentação, tratando o valor como um número.

    ![Documentação XML para tipos de retorno](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "documentação XML para tipos de retorno")

    *Documentação XML para tipos de retorno*
5. Agora, insira uma chamada para adicionar a função. Observe que o editor do JavaScript agora dá suporte a sobrecargas de função. Quando você escreve um nome de função, você poderá selecionar qualquer uma das sobrecargas disponíveis especificadas na documentação.

    ![Documentação XML para as sobrecargas](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "documentação XML para as sobrecargas")

    *Documentação XML para as sobrecargas*
6. Abra **GotoDefinition.js** de arquivo e localize o **$().html()** chamada de função. Localize o cursor em **html**.
7. Pressione **F12** e navegue até a definição. Observe que agora você pode acessar e procurar seu código JavaScript sem usar o **localizar** ferramenta.
8. Localize o cursor sobre a instrução jQuery antes do bloco de assinatura na parte inferior do arquivo de código. Pressione **F12**. Você navegará para o arquivo de biblioteca jQuery. Observe que você também pode navegar em todos os arquivos de jQuery usando **F12**.

    ![Navegar para jQuery definições](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "navegando até jQuery definições")

    *Navegar para jQuery definições*

> [!NOTE]
> Certifique-se de que GotoDefinition.js não tem nenhum erro de sintaxe antes de salvar o arquivo.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Exercício 4: Empacotamento e minimização

Quantas vezes os sites têm os CSS ou JavaScript mais de um arquivo? Este é um cenário bastante comum, onde o empacotamento e minimização podem ajudar a reduzir o tamanho do arquivo e fazer com que o site ser executado mais rapidamente. O novo recurso de agrupamento no ASP.NET 4.5 pacotes de um conjunto de arquivos JS ou CSS em um único elemento e reduz o tamanho, minimizando o conteúdo (por exemplo, remover espaços em branco não é necessários, removendo comentários, reduzindo identificadores).

Empacotamento e minimização no ASP.NET 4.5 é executada em tempo de execução, para que o processo pode identificar o agente do usuário (por exemplo, Internet Explorer, Mozilla, etc) e assim, melhorar a compactação direcionando o navegador do usuário (por exemplo, removendo objetos Mozilla específico Quando a solicitação chega do IE).

Neste exercício, você aprenderá como habilitar e usar os diferentes tipos de empacotamento e minimização no ASP.NET 4.5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Tarefa 1: instalar o empacotamento e minimização de pacote do NuGet

1. Se ainda não estiver aberto, inicie **Visual Studio** e abra o **WhatsNewASPNET.sln** solução localizada no **Source\WhatsNewASPNET** pasta deste laboratório.
2. Abra o NuGet Package Manager Console. Para fazer isso, use o menu **exibição** | **outras janelas** | **Package Manager Console**.

    ![Abrir o pacote manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "abrindo o console do Gerenciador de pacote")

    *Abrindo o console do Gerenciador de pacote*
3. No **Package Manager Console,** tipo **Install-Package Microsoft.Web.Optimization** e pressione **ENTER**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Tarefa 2 - pacotes padrão

A maneira mais simples de usar empacotamento e minimização é habilitar os pacotes padrão. Esse método usa as convenções para lhe permitem fazer referência à versão do pacote e minimizada para os arquivos JS e CSS em uma pasta.

Nesta tarefa, você aprenderá como ativar e fazer referência aos arquivos agrupados e minimizados JS e CSS e exibir a saída resultante.

1. Se ainda não estiver aberto, inicie **Visual Studio** e abra o **WhatsNewASPNET.sln** solução localizada no **Source\WhatsNewASPNET** pasta deste laboratório.
2. No **Solution Explorer**, expanda o **estilos**, **Scripts\custom** e **Scripts\bundle** pastas.

    Observe que o aplicativo está usando o arquivo de mais de uma CSS e JS.

    ![Arquivos de várias folhas de estilo e JavaScript no aplicativo](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "arquivos várias folhas de estilo e JavaScript no aplicativo")

    *Vários arquivos de folhas de estilo e JavaScript no aplicativo*
3. Abra o **Global.asax.cs** arquivo.

    Observe que o novo **Microsoft.Web.Optimization** namespace é comentado no início do arquivo. Remova o usando a diretiva para incluir os recursos de empacotamento e minimização.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Localize o **aplicativo\_iniciar** método.

    Nesse método, remova a chamada EnableDefaultBundles conforme mostrado no trecho a seguir. Isso nos permite fazer referência a uma coleção de pacote de arquivos CSS em uma pasta usando o caminho para a pasta, mais o &quot;CSS&quot; ou &quot;JS&quot; sufixo.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Abra o **Optimization.aspx** de arquivo e localize o controle de conteúdo de **HeadContent**.

    Observe os arquivos CSS e JS ter uma única marca referenciada.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Esse código é para fins de demonstração. Idealmente, você irá referenciar os pacotes no arquivo Site.Master. Nesse código de exemplo, você descobrirá que alguns dos arquivos de pacote também estão sendo referenciados pelo arquivo de Site.Master, tornando esta última referência redundantes.
6. Observe que os links são usando as convenções de agrupamento de **href** atributo para obter os arquivos de todos os CSS ou JS dos estilos e Scripts\custom pasta respectivamente.

    Você pode usar o caminho **personalizado/Scripts/JS** conforme mostrado abaixo, agrupar e minificada todos os arquivos JS dentro de um **Scripts/personalizada** pasta. Esse é o comportamento padrão com os pacotes padrão.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Abra o **Styles\Site.css** arquivo.

    Observe que o arquivo CSS original contém código recuado, espaços em branco e comentários que aumentam o arquivo. (Também o arquivo JavaScript contém comentários e espaços em branco).

    ![Uma folha de estilos original arquivos na pasta Scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "uma folha de estilos original arquivos na pasta Scripts")

    *Um dos arquivos CSS originais na pasta Scripts*
8. Pressione **F5** para executar o aplicativo e navegue até o **otimização** página.
9. Clique no **pacote CSS** link para baixar e abrir o arquivo.

    Check-out do arquivo de pacote minimizado. Você observará que todos os espaços em branco, comentários e caracteres de recuo foram removidas, gerando um arquivo menor.

    ![Arquivos CSS empacotados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "arquivos CSS agrupados")

    *Os arquivos CSS*
10. Agora clique o **JS pacote** link para abrir o arquivo de pacote de JavaScript. Você poderá desconsiderar o aviso do Pesquisador de objetos. Observe os arquivos JavaScript sob o **personalizado** pasta também são agrupadas e minimizada.

    ![Os arquivos JavaScript de pacote](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "arquivos JavaScript agrupado")

    *Os arquivos do JavaScript*

    A habilitação da compactação de arquivos CSS ou JS era muito mais complicada na versão anterior do ASP.NET. Agora, como você viu, basta adicionar uma linha no *global. asax* para habilitar o agrupamento de arquivo e, em seguida, fazer referência aos arquivos de pacotes do seu site.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Tarefa 3 - pacotes estáticos

A abordagem de pacote estático permite que você personalize o conjunto de arquivos de pacote, a referência e o método de minimização que será usado.

Nesta tarefa, você configurará um conjunto estático para definir um conjunto específico de arquivos para agrupar e minificada.

1. Feche o navegador.
2. Abra o **Global.asax.cs** de arquivo e localize o **aplicativo\_iniciar** método.
3. Descomente o código de pacote estáticos, conforme mostrado no código a seguir.

    Você está definindo um conjunto estático que será referenciado com o &quot; **~/StaticBundle** &quot; caminho virtual e use **JsMinify** para minimização de todos os arquivos especificados com o **AddFile** método. Por fim, você está adicionando o pacote estático para o **BundleTable** e habilitá-la.

    Observe que os arquivos não estão localizados no mesmo lugar; Essa é outra vantagem sobre o agrupamento padrão.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Abra o **Optimization.aspx** arquivo.

    Observe que o link para **conjunto estático de JS** está usando o caminho que você declarou quando você configurou o pacote estático no arquivo asax: **/StaticBundle**.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Pressione **F5** para executar o aplicativo e, em seguida, navegue até o **otimização** página.
6. Clique no **conjunto estático de JS** link para abrir o arquivo.

    Observe que o minimizada agrupados arquivo JavaScript é a saída para todos os arquivos do JavaScript são configurados no arquivo de pacote estáticos no caminho &quot;/StaticBundle&quot;.

    ![Pacote de arquivos estático JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "pacote de arquivos estáticos JavaScript")

    *Agrupar arquivos JavaScript estáticos*
7. Feche o navegador e retorne ao Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Tarefa 4 - pasta dinâmico pacotes

Nesta tarefa, você aprenderá como configurar pacotes de pasta dinâmico. O poder do empacotamento dinâmico é que você pode incluir JavaScript estático, bem como outros arquivos em idiomas que compila em JavaScript e assim, exigem algum processamento antes que o agrupamento é executada.

Neste exemplo, você aprenderá como usar o **DynamicFolderBundle** classe para criar um conjunto dinâmico para arquivos escritos em CofeeScript. CofeeScript é uma linguagem de programação que compila em JavaScript e fornece uma sintaxe mais simples para escrever código JavaScript, melhorando a legibilidade e resumir do JavaScript.

1. Abra o **Global.asax.cs** de arquivo e localize o **aplicativo\_iniciar** método.
2. Descomente o código de pacote dinâmico, conforme mostrado no código a seguir.

    Você está definindo um pacote da pasta dinâmico que usará o **CoffeeMinify** processador minimização personalizados que se aplicam somente a arquivos com o &quot; **.coffee** &quot; (de extensão Arquivos de CoffeeScript). Observe que você pode usar um padrão de pesquisa para selecionar os arquivos para agrupar dentro de uma pasta, como '\*.coffee'.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Abra o NuGet Package Manager Console. Para fazer isso, use o menu **exibição** | **outras janelas** | **Package Manager Console**.
4. No **Package Manager Console,** tipo **Install-Package CoffeeSharp** e pressione **ENTER**.
5. Clique o **Mostrar todos os arquivos** no botão de **Solution Explorer** janela

    ![Mostrando todos os arquivos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "mostrando todos os arquivos")

    *Mostrando todos os arquivos*
6. Clique com botão direito do **CoffeeMinify.cs** de arquivos no **Gerenciador de soluções** e selecione **incluir no projeto**

    ![Incluir o arquivo CoffeeMinify.cs no projeto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "incluem o arquivo CoffeeMinify.cs no projeto")

    *Incluir o arquivo CoffeeMinify.cs no projeto*
7. Abra o **CoffeeMinify.cs** arquivo.

    Essa classe herda do JsMinify ser minificada a saída de JavaScript resultante da compilação de código CoffeeScript. Ele chama o compilador CoffeeScript para gerar o código JavaScript primeiro e, em seguida, ele envia para o método de JsMinify.Process para minificada o código resultante.


    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Abra o **Script1.coffee** e **Script2.coffee** arquivos do **Scripts/pacote** pasta.

    Esses arquivos incluirá o código CoffeScript para ser compilada ao executar o agrupamento com a classe CoffeeMinify.

    Para fins de simplicidade, os arquivos de CoffeeScript fornecidos são apenas incluindo CoffeeScript código de exemplo. Os comentários são excluídos pelo processo de JsMinify.

    ![Arquivos de CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "CoffeeScript arquivos")

    *Arquivos do CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) fornece uma sintaxe mais simples para escrever código JavaScript, melhorando a legibilidade e resumir do JavaScript, bem como adicionar outros recursos, como a compreensão de matriz e a correspondência de padrões.
9. Abra o **Optimization.aspx** de arquivo e localize os links do pacote.

    Observe que o link para **pacote dinâmico de JS** faz referência a **Scripts/pacote** pasta usando o **/café** sufixo que você configurou para o pacote da pasta dinâmico.


    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Pressione **F5** para executar o aplicativo e, em seguida, navegue até o **otimização** página.
11. Clique no **pacote dinâmico de JS** link para abrir o arquivo gerado.

    Observe que só contém o conteúdo que foi incluído neste pacote **.coffee** arquivos. Você também pode ver que o código de CoffeeScript foi compilado para JavaScript e as linhas comentadas foi removido.

    ![Agrupar arquivos JS dinâmicos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "JS dinâmico pacote de arquivos")

    *Pacote de arquivos JS dinâmico*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice b: publicação um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Resumo

Este laboratório o ajudará a entender o que há de novo no ASP.NET e desenvolvimento da Web no Visual Studio 2012 e como aproveitar a variedade de aprimoramentos no Visual Studio 2012.

Ao concluir este laboratório prático, você tem learnt como usar os novos recursos e aprimoramentos no Visual Studio 2012 editores CSS, JavaScript e HTML. Além disso, você tem learnt como o Visual Studio 2012 implementa internos de empacotamento e minimização.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalar o Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outro &quot;Express&quot; versão usando o  **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)** . As instruções a seguir guiá-lo pelas etapas necessárias para instalar o *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-la e procure o produto &quot; *Visual Studio Express 2012 para Web com o SDK do Windows Azure*&quot;.
2. Clique em **instalar agora**. Se você não tem **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação seja concluída.

    ![Progresso da instalação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Instalação concluída*
7. Clique em **saída** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco de Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express para o bloco de Web*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice b: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

Este apêndice mostram como criar um novo site do Portal de gerenciamento do Windows Azure e publicar o aplicativo que você obteve seguindo o laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Windows Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarefa 1: criar um novo Site do Windows Azure Portal

1. Vá para o [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Windows Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego cresce. Você pode inscrever [aqui](http://aka.ms/aspnet-hol-azure).

    ![Faça logon no portal do Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "faça logon no portal do Windows Azure")

    *Faça logon no Portal de gerenciamento do Azure do Windows*
2. Clique em **novo** na barra de comandos.

    ![Criando um novo Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "criar um novo Site")

    *Criando um novo Site*
3. Clique em **de computação** | **Site da Web**. Em seguida, selecione **criação rápida** opção. Forneça uma URL disponível para o novo site e clique em **Criar Site**.

    > [!NOTE]
    > Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implante um aplicativo web concluído ao Site do Windows Azure de fora do portal. Ele não inclui etapas para configurar um banco de dados.

    ![Criando um novo Site usando a criação rápida](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "criar um novo Site usando a criação rápida")

    *Criando um novo Site usando a criação rápida*
4. Aguarde até que o novo **Site da Web** é criado.
5. Depois que o Site da Web é criado, clique no link sob a **URL** coluna. Verifique se o novo Site da Web está funcionando.

    ![Navegando para o novo site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "navegando para o novo site")

    *Navegando para o novo site*

    ![Site da Web em execução](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "site em execução")

    *Site da Web em execução*
6. Acesse o portal e clique no nome do site sob o **nome** coluna para exibir as páginas de gerenciamento.

    ![Abrir as páginas de gerenciamento do site da web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "abrir páginas de gerenciamento do site")

    *Abrir as páginas de gerenciamento do Site da Web*
7. No **painel** página no **visão rápida** seção, clique o **baixar perfil de publicação** link.

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para se conectar e autenticar cada um dos pontos de extremidade para o qual um método de publicação está habilitado. **Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web para sites do Windows Azure.

    ![Baixando o site da web de perfil de publicação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "baixando o site da web de perfil de publicação")

    *Baixando o Site da Web de perfil de publicação*
8. Baixe o arquivo de perfil de publicação para um local conhecido. Mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "salvar o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2 – Configurando o servidor de banco de dados

Se seu aplicativo utiliza o SQL Server bancos de dados que você precisará criar um servidor de banco de dados SQL. Se você deseja implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.

1. Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores de banco de dados SQL de sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**. Se você não tiver um servidor criado, você pode criar um usando o **adicionar** botão na barra de comandos. Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas. Não crie o banco de dados, como ele será criado em um estágio posterior.

    ![Painel do servidor de banco de dados SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "painel de banco de dados do SQL Server")

    *Painel de banco de dados do SQL Server*
2. A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor de **endereços IP permitidos**. Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço IP do cliente atual** e cole-o no **endereço IP inicial** e **oendereçoIPfinal** caixas de texto. Insira um nome para a regra e clique no ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) botão.

    ![Adicionar endereço IP do cliente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Adicionar endereço IP do cliente*
3. Uma vez o **endereço IP do cliente** é adicionado para os endereços IP permitidos, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Confirmar alterações*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3: publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

1. Volte para a solução do ASP.NET MVC 4. No **Solution Explorer**, com o botão direito no projeto de site da web e selecione **publicar**.

    ![Publicar o aplicativo](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "a publicação do aplicativo")

    *Publicar o site*
2. Importe o perfil de publicação que você salvou na primeira tarefa.

    ![Importar o perfil de publicação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "importar o perfil de publicação")

    *Importar o perfil de publicação*
3. Clique em **validar Conexão**. Quando a validação estiver concluída, clique em **próximo**.

    > [!NOTE]
    > A validação é concluída depois que aparecer uma marca de seleção verde ao lado do botão Validar Conexão.

    ![Validando a conexão](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "validação de conexão")

    *Validação de conexão*
4. No **configurações** página no **bancos de dados** seção, clique no botão ao lado da caixa de texto da sua conexão de banco de dados (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

    - No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.
    - Em **nome de usuário** digite seu nome de logon de administrador do servidor.
    - Em **senha** digite sua senha de logon de administrador do servidor.
    - Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.

    ![Configurando a cadeia de caracteres de conexão de destino](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configurando a cadeia de caracteres de conexão de destino")

    *Configurando a cadeia de caracteres de conexão de destino*
6. Clique em **OK**. Quando solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "criar a cadeia de caracteres do banco de dados")

    *Criando o banco de dados*
7. A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão. Clique em **Avançar**.

    ![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")

    *Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*
8. No **visualização** , clique em **publicar**.

    ![Publicar o aplicativo web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "publicar o aplicativo web")

    *Publicar o aplicativo web*
9. Quando termina o processo de publicação, o navegador padrão abrirá o site da web publicado.

    ![Aplicativo publicado no Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "aplicativo publicado no Windows Azure")

    *Aplicativos publicados no Windows Azure*
