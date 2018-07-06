---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: O que há de novo no ASP.NET e desenvolvimento da Web no Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: A nova versão do Visual Studio apresenta uma série de aprimoramentos e concentrados em melhorar a experiência e o desempenho ao trabalhar com tecnologias da Web...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 474df5c8e2cee820a3bdd80ba45e6504a025cdf1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823602"
---
<a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>O que há de novo no ASP.NET e desenvolvimento da Web no Visual Studio 2012
====================
por [Web Camps equipe](https://twitter.com/webcamps)

> A nova versão do Visual Studio apresenta uma série de aprimoramentos e concentrados em melhorar a experiência e o desempenho ao trabalhar com tecnologias da Web. Editores do Visual Studio para CSS, JavaScript e HTML foi completamente reformuladas para incluir muitas das ajudas de código mais acessado, como IntelliSense e recuo automático. Em relação ao desempenho, agrupamento e minificação agora estão integradas como o tempo de carregamento de recursos internos para reduzir facilmente a página.
> 
> Visual Studio permite que você trabalhe com as tecnologias mais recentes do site. Você pode usar trechos de código entre navegadores CSS3 para garantir que seu site funciona independentemente da plataforma cliente aproveitando também as vantagens dos novos recursos e elementos do HTML5.
> 
> Gravando e código JavaScript de criação de perfil devem ser mais fácil com esta versão do Visual Studio. Listas do IntelliSense, recursos de navegação e de documentação XML integrados agora estão disponíveis para o código JavaScript. Agora você tem o catálogo de JavaScript ao seu alcance. Além disso, você pode verificar a conformidade da ECMAScript5 com seus scripts e detectar erros de sintaxe em um estágio inicial.
> 
> Por último, mas não menos importante, essa versão do Visual Studio implementa interno de agrupamento e minificação. Arquivos de script e folhas de estilo serão compactadas e compactadas para que o site executa mais rápido.
> 
> Este laboratório orienta os aprimoramentos e novos recursos descritos anteriormente, aplicando alterações secundárias a um aplicativo Web de exemplo fornecido na pasta de origem.
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível em [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Este laboratório prático, você aprenderá como:

- Use os novos recursos e aprimoramentos no editor de CSS
- Use os novos recursos e aprimoramentos no editor de HTML
- Use os novos recursos e aprimoramentos no editor de JavaScript
- Configurar e usar o agrupamento e minificação

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) ou superior (leia [Apêndice A](#AppendixA) para obter instruções sobre como instalá-lo).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (para scripts de instalação - já instalados no Windows 8 e Windows Server 2008 R2)
- [O Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) - ou um navegador em conformidade com HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Exercício 1: O que há de novo no Editor de CSS](#Exercise1)
2. [Exercício 2: O que há de novo no Editor de HTML](#Exercise2)
3. [Exercício 3: O que há de novo no Editor de JavaScript](#Exercise3)
4. [Exercício 4: Agrupamento e Minificação](#Exercise4)

Tempo estimado para concluir este laboratório: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Exercício 1: O que há de novo no Editor de CSS

Os desenvolvedores da Web devem estar familiarizados com muitas das dificuldades relacionadas à edição de CSS. Um dos maiores problemas de estilo CSS é a compatibilidade de navegadores. Isso geralmente acontece que, depois de aplicar estilos a seu site, você perceber que ele tem uma aparência diferente se você abri-lo em outro navegador ou dispositivo. Portanto, você pode gastar um tempo considerável corrigir esses problemas visual para perceber que, quando você finalmente fazê-lo funcionar em um navegador, ele é dividido nas outras.

Visual Studio agora inclui recursos que ajudam os desenvolvedores de acesso, trabalhar e organizar as folhas de estilos CSS com eficiência. Ao longo deste exercício, você atenderá aos novos recursos para uma organização em vigor e edition, bem como os trechos de código CSS3 para compatibilidade de navegadores.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Tarefa 1 – novos recursos do Editor

Nesta tarefa, você descobrirá os novos recursos do Editor de CSS. Este novo editor ajudará você a aumentar sua produtividade, aproveitando o recuo inteligente novo, os comentários de código aprimorada e a lista do IntelliSense aprimorada.

1. Inicie **Visual Studio** e abra o **WhatsNewASPNET.sln** solução localizada no **Source\WhatsNewASPNET** pasta deste laboratório.
2. No Gerenciador de soluções, abra o **CSS** arquivo localizado sob a **estilos** pasta. Verifique se o **Editor de texto** ferramentas são visíveis na barra de ferramentas. Para fazer isso, selecione a **modo de exibição** | **barras de ferramentas** opção de menu e verifique se a **Editor de texto** opções. Você observará que, desde que essa nova versão, o **comentário** botão (![botão comentário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) ) e o **Remover comentário** botão (![Descomente-botão](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png)) também são habilitados para o editor de CSS.

    ![Habilitando o Editor e ferramentas CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "habilitando o Editor e ferramentas CSS")

    *Habilitando o Editor e ferramentas CSS*
3. Role o código e selecione qualquer definição de classe CSS. Clique o **comentário** (![botão comentário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png) ) botão para comentar as linhas selecionadas. Em seguida, clique o **Uncomment** (![botão Descomente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png) ) botão Desfazer as alterações.
4. Clique o **recolher** (![recolher](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png) ) e **expandir** (![expanda](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png) ) botões localizados na margem esquerda do texto. Observe que você agora pode ocultar os estilos que você não use para ter uma exibição mais clara.

    ![As classes CSS de recolhimento](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "classes CSS de recolhimento")

    *As classes CSS de recolhimento*
5. Certifique-se de que o recurso de recuo inteligente está habilitado. Selecione o **ferramentas** | **opções** opção de menu e, em seguida, selecione o **Editor de texto** | **CSS**  |  **Formatação** página no painel esquerdo da tela. Verifique as **recuo hierárquico** opção.

    ![Habilitando o recuo hierárquico](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "habilitando recuo hierárquico")

    *Habilitando o recuo hierárquico*
6. Localize a definição de classe principal. (Main) e acrescentar um estilo para elementos div. Você observará que o código alinha automaticamente, ajudando os usuários para localizar o pai de classes em um relance.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Alinhamento hierárquico no CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "alinhamento hierárquico no CSS")

    *Alinhamento hierárquico no CSS*
7. Dentro de **div. main** classe, localize o cursor no final da **borda: 0px;** e pressione **Enter** para exibir a lista do IntelliSense. Comece a digitar **superior** e observe como a lista é filtrada enquanto você digita. A lista exibirá os elementos que contêm **superior** em qualquer parte da palavra (nas versões anteriores do Visual Studio, a lista é filtrada pelos itens que *begin* com o termo).

    ![Melhorias do IntelliSense no CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "melhorias do IntelliSense no CSS")

    *Melhorias do IntelliSense no CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Tarefa 2 - o seletor de cores

Nesta tarefa, você descobrirá que o novo seletor de cor CSS integrado ao Visual Studio IntelliSense.

1. Na **CSS,** localize a definição de classe de cabeçalho (.header) e coloque o cursor ao lado **cor do plano de fundo** atributo entre os &quot;:&quot; e &quot; # &quot; caracteres nessa linha de código **.**

    ![Localizando o cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "localizando o cursor")

    *Localizando o cursor*
2. Excluir o **dois-pontos** (:) e gravá-lo novamente para exibir o seletor de cores. Observe que as cores primeiro, que você verá as cores mais frequentes do seu site. Se você clicar na cor branca, seu código de cor HTML (#fff) substituirá o código de cor atual na folha de estilos.

    ![Seletor de cores](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "selecionador de cores")

    *Seletor de cores*
3. Pressione a **Expand** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png) ) botão o seletor de cores para exibir o gradiente de cores e, em seguida, arraste o cursor de gradiente para selecionar uma cor diferente. Em seguida, clique o **conta-gotas** botão e selecione qualquer cor na tela. Observe que o valor de cor do plano de fundo muda dinamicamente enquanto você move o cursor.

    ![Gradiente de seletor de cores](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "gradiente do seletor de cores")

    *Gradiente de seletor de cores*
4. No **opacidade** controle deslizante, mova o seletor para o centro da barra de reduzir a opacidade. Observe que o valor de cor do plano de fundo agora altera sua escala para RGBA.

    ![Seletor de cor opacidade](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "opacidade do seletor de cores")

    *Opacidade do seletor de cores*

    > [!NOTE]
    > A definição de cor RGBA (vermelho, verde, azul, alfa) no CSS3 permite que você defina o valor de opacidade da cor de um único item. Diferentemente **opacidade -** um atributo CSS semelhante **-** cores RGBA também são compatíveis com navegadores mais recentes.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Tarefa 3: trechos de código compatível de CSS

Nesta tarefa, você aprenderá como usar trechos de CSS3 compatíveis de navegadores para implementar alguns recursos no seu site.

1. No **CSS** do arquivo, localize a **cabeçalho** CSS (.header) de definição de classe e coloque o cursor abaixo de **/ \*raio de borda\* /** espaço reservado para adicionar um novo trecho. Pressione **Enter** para exibir a lista do IntelliSense e o tipo **radius** para filtrar a lista. Selecione o **raio de borda** opção na lista com um clique duplo e, em seguida, pressione a **guia** tecla para inserir o trecho de código. Em seguida, digite um tamanho de radius em pixels e pressione **Enter**. Por exemplo, digite **15px**.

    Os atributos de CSS3 adicionados pelo trecho de renderizará bordas arredondadas na maioria dos navegadores de conformidade do HTML5, incluindo o Mozilla e navegadores de WebKit.

    ![Usando um trecho de raio de borda](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "usando um trecho de raio de borda")

    *Usando um trecho de raio de borda*
2. Aplicar a mesma **borda** trechos de código no estilo de página (.page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Pressione **F5** para executar a solução. Observe que cada página agora tem arredondados bordas.

    ![Cantos arredondados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "cantos arredondados")

    *Cantos arredondados*
4. Feche o navegador e retorne ao Visual Studio.
5. Abra o **Custom. CSS** arquivo localizado na **estilos** pasta e coloque o cursor dentro **div.images ul li img** definição de classe.
6. Pressione enter para exibir a lista do IntelliSense, tipo **caixa de sombra** e pressione a **guia** tecla duas vezes para inserir o trecho de código de sombra padrão dentro da definição de classe. Defina os valores de sombra para **10px 10px 5px #888**. Em seguida, digite **raio de borda** e inserir o trecho de código. Tipo de **15px** para definir o tamanho de radius e pressione **ENTER**.

    ![Arredondado cantos com sombra](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "arredondado cantos com sombra")

    *Cantos arredondados com sombra*

    > [!NOTE]
    > Neste momento, o atributo de sombra é inserido com o prefixo correspondente (moz, webkit, s) para dar suporte ao Mozilla e navegadores de Webkit (Chrome, Safari, Konkeror).
7. Crie uma nova classe **div.images ul li img:hover** abaixo de **div.images ul li img** definição de classe e coloque o cursor dentro dos colchetes **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Tipo de **transformar** e pressione a **guia** chave duas vezes para inserir o trecho de código de transformação. Em seguida, insira **rotate(-15deg)** para alterar o valor de ângulo de rotação quando as imagens são focalizadas.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Pressione **F5** para executar a solução e navegue até a página do CSS3. Observe que as imagens tem cantos arredondados e sombras de caixa. Passe o mouse sobre as imagens e assisti-los a girar.

    ![Girar uma imagem de trecho de código de transformação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "trecho de código de transformação girar uma imagem")

    *Transformar girar uma imagem de trecho de código*

    > [!NOTE]
    > Se você estiver usando o Internet Explorer 10 e não é possível ver as sombras, verifique se que o modo de documento é definido como padrões do IE10. Pressione **F12** para abrir as ferramentas de desenvolvedor do Internet Explorer e clique **modo de documento** para alterar para padrões do IE10.

    ![sobre-us](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Exercício 2: O que há de novo no Editor de HTML

O Visual Studio tem um editor de HTML aprimorado. Alguns dos aprimoramentos incluídos nesta versão são recuo inteligente em documentos HTML, trechos de código do HTML5, início HTML e correspondência de marca de fim e validação de HTML. Ao longo deste exercício, você verá como essas alterações melhorar sua fluência ao trabalhar na marcação de site.

Como o editor de CSS, editor de HTML também foi aprimorado. A maioria desses aprimoramentos é menores que tornam mais fácil a vida do desenvolvedor da Web. Coisas como mais trechos de código para HTML5, recuo inteligente, correspondência de marcas de início e término quando a edição e a validação direcionando o DOCTYPE do documento HTML são alguns desses aprimoramentos.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Tarefa 1 - validação de DOCTYPE aprimorado

O editor de HTML agora tem a capacidade de verificar o DOCTYPE da sua página, mesmo que a definição possa ser na página mestra. Dependendo do DOCTYPE da sua página, o editor de HTML será validada com o conjunto correto de regras e filtrará a lista do IntelliSense considerando os elementos DOCTYPE.

Nesta tarefa, você irá alterar o tipo de documento de uma página para ver como o comportamento do editor de HTML alterada de acordo.

1. Se ainda não estiver aberto, inicie **Visual Studio** e abra o **WhatsNewASPNET.sln** solução localizada no **Source\WhatsNewASPNET** pasta deste laboratório.
2. Abra o **Master** página.
3. Observe o esquema de destino para a barra de ferramentas de validação. O comportamento do editor de HTML (validação, IntelliSense, etc.) corretamente será alterado de acordo com o tipo de documento selecionado.

    ![Use o Doctype na barra de ferramentas de edição de código-fonte HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Doctype de uso na barra de ferramentas de edição de código-fonte HTML")

    *Use o Doctype na barra de ferramentas de edição de código-fonte HTML*
4. Altere o esquema de destino para HTML 4.01.

    ![Alterando o Doctype na barra de ferramentas de edição de código-fonte HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "alterando o tipo de documento na barra de ferramentas de edição de código-fonte HTML")

    *Alterando o Doctype na barra de ferramentas de edição de código-fonte HTML*
5. Coloque o cursor sob a **corpo** elemento e comece a digitar o nome de um elemento HTML5 (por exemplo, **vídeo**). Observe que o elemento não está disponível na lista do IntelliSense.

    ![Elementos do HTML5 não listados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "elementos do HTML5 não listados")

    *Elementos do HTML5 não listados*
6. Desfazer as alterações ao esquema de destino para a barra de ferramentas de validação, DOCTYPE de separação: XHTML5 na lista suspensa.

    ![Use o Doctype na barra de ferramentas de edição de código-fonte HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Doctype de uso na barra de ferramentas de edição de código-fonte HTML")

    *Redefinir o Doctype na barra de ferramentas de edição de código-fonte HTML*
7. Coloque o cursor sob a **corpo** elemento e comece a digitar um elemento HTML5 novamente (por exemplo, como **vídeo**). Observe que os elementos do HTML5 agora estão disponíveis na lista do IntelliSense.

    ![Elementos do HTML5 sejam listados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "elementos HTML5 sejam listados")

    *Elementos do HTML5 sejam listados*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Tarefa 2 - início/término de atualização automática de marcas

Agora, o Visual Studio atualiza o HTML de abertura e fechamento de marcas do elemento que você está editando para corresponder um ao outro. Esse novo recurso melhora a sua produtividade ao editar marcas HTML.

1. Sobre o **default. aspx** página, adicione um **H3** elemento com um título (por exemplo, o Visual Studio 2012 Rocks!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Alterar o **H3** marca e o tipo **H2** ou **H1.**

    Observe que a marca de fim é atualizada automaticamente. Você também pode modificar a marca de fim para ver que a marca de início também é atualizada adequadamente.

    ![Atualização automática da marca de fim](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "atualização automática da marca de fim")

    *Atualização automática da marca de fim*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Tarefa 3: novos trechos de código do HTML5

Agora, o Visual Studio inclui vários trechos de código do HTML5. Nesta tarefa, você usará alguns dos trechos de código.

1. Adicionar uma nova pasta chamada **áudio** na raiz da pasta do site da web. Abra o Windows Explorer e copiar qualquer arquivo de áudio para o **áudio** pasta da **WhatsNewASPNET.sln** solução.
2. No **default. aspx** , localize o cursor sob o Web11 Rocks!! Cabeçalho. Tipo de **áudio** e pressione a tecla TAB.

    O novo editor de HTML inclui trechos de código para o conteúdo do HTML5. Lembre-se de usar a definição de DOCTYPE adequada para habilitar os trechos de código do HTML5.

    ![Inserir trechos de código do HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "inserindo trechos de código do HTML5")

    *Inserir trechos de código do HTML5*
3. Atualize a fonte de áudio para apontar para um arquivo de áudio existente.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Você precisará adicionar o arquivo de áudio para a solução.
4. Pressione **F5** para executar o site e reproduzir o áudio.

    ![Executando controle de áudio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "executando o controle de áudio")

    *Executando controle de áudio*

    > [!NOTE]
    > Você também pode experimentar mais trechos de código incluídos no Visual Studio, como vídeo, figura, etc.
5. Agora, tente inserir um controle em alguma parte da página. Por exemplo, tente inserir uma **GridView** controle, mas em vez de digitar  **&lt;conforme gra,** comece a digitar  **&lt;GV**. Observe que a lista do IntelliSense mostra a **asp: GridView** controle.

    O IntelliSense no Editor de HTML agora fornece pesquisa de capitalização de título, bem como parcial correspondente (Recuperando todos os elementos que contém o termo).

    ![Inserindo um GridView com IntelliSense listas](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "inserindo um GridView com listas do IntelliSense")

    *Inserindo um GridView com listas do IntelliSense*

    Se você digitar  **&lt;grade** obterá todos os itens que correspondem ao termo, mas o Visual Studio irá sugerir a **gridview** controle:

    ![Inserindo um GridView com listas do IntelliSense e a correspondência parcial](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "inserindo um GridView com listas do IntelliSense e a correspondência parcial")

    *Inserindo um GridView com listas do IntelliSense e a correspondência parcial*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Tarefa 4 - marcas de inteligentes do Editor de HTML

Outra melhoria no Editor de HTML é o recurso de marcas inteligentes. Marcas inteligentes tornam fácil executar tarefas de desenvolvimento comuns ou repetitivas em uma base por controle. Esse recurso já estava disponível no Designer de HTML, mas não no Editor de HTML.

1. Abra **Master** e localize o **asp: Menu** elemento. Coloque o cursor na marca de início e observe que o glifo pequeno exibido na parte inferior do elemento - clicar nele para abrir o menu de tarefas inteligentes. Observe que você tem acesso rápido a algumas tarefas relacionadas ao controle de Menu.

    ![Inteligente tarefas para o controle de Menu](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "inteligente tarefas para o controle de Menu")

    *Tarefas inteligentes para o controle de Menu*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Tarefa 5 – recuo inteligente

Uma das melhores práticas em HTML é recuar os elementos aninhados para manter o código legível. No Visual Studio 2012, você observará que o editor recua automaticamente os elementos enquanto você estiver escrevendo o código.

> [!NOTE]
> Na versão anterior do Visual Studio, o recuo inteligente estava disponível no editor de XML, mas não no editor de HTML.


1. Certifique-se de que a configuração de recuo no Editor de HTML é definida como o recuo inteligente. Para fazer isso, selecione o **ferramentas | As opções** opção de menu e selecione o **Editor de texto | HTML | Guias** página no painel esquerdo da tela. Selecione a opção de recuo inteligente.

    ![Configurações do Editor de HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "configurações do Editor de HTML")

    *Configurações do Editor de HTML*
2. Sobre o **default. aspx** página, remova todo o conteúdo sob o elemento de áudio.
3. Coloque o cursor no final da abertura **áudio** elemento e pressione **ENTER**.

    Observe que a nova posição do cursor tem um nível de recuo adicional.

    ![Inteligente no Editor de HTML do recuo](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "inteligente recuo no Editor de HTML")

    *Recuo inteligente no Editor de HTML*
4. Restaurar a marcação de áudio com o conteúdo que você tenha removido ou feche **default. aspx** sem salvar as alterações.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Tarefa 6: Extrair para controle de usuário

As ferramentas de refatoração incluídas no Visual Studio, como extrair uma parte do código para uma função, são excelentes recursos que facilitam o aprimoramento e a refatoração do código existente. O equivalente para as páginas ASP.NET seria a extração de código HTML a um controle de usuário. Fazê-lo manualmente envolveria várias etapas, como criar um novo controle de usuário, movendo a seção de código para o controle de usuário, registrando um prefixo de marca do controle de usuário e, finalmente, instanciar o controle de usuário nas páginas. Agora, o novo *extrair para controle de usuário* ferramenta executa automaticamente todas essas etapas para você.

Nesta tarefa, você usará o novo extrair a operação de controle de usuário contextual para gerar um novo controle de usuário a partir do código selecionado.

1. Sobre o **default. aspx** página, selecione o **H2** e **áudio** elementos.
2. Clique com botão direito e selecione **extrair para controle de usuário**.

    ![Extrair a opção de menu de controle de usuário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "extrair a opção de menu de controle de usuário")

    *Extrair a opção de menu de controle de usuário*
3. Digite um nome para o novo controle de usuário. Por exemplo, **Jukebox.ascx**e, em seguida, clique em **Okey**.

    ![Salvando o controle de usuário extraído](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "salvando o controle de usuário extraído")

    *Salvando o controle de usuário extraído*
4. Observe que o código selecionado foi extraído para um controle de usuário e o local original do código selecionado foi substituído por uma instância do novo controle de usuário.

    ![Página é atualizada automaticamente para usar o novo controle de usuário](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "página é atualizada automaticamente para usar o novo controle de usuário")

    *Página é atualizada automaticamente para usar o novo controle de usuário*
5. Pressione **F5** para executar a página e verifique se o controle funciona.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Exercício 3: O que há de novo no Editor de JavaScript

Gravar ou editar o código JavaScript não é uma tarefa fácil, especialmente quando seu aplicativo começa a crescer em tamanho e você lidar com longos de arquivos e centenas de funções. Os desenvolvedores de script geralmente precisam realizar algum trabalho adicional para manter a legibilidade do código e navegar entre arquivos. Com a inclusão de bibliotecas JavaScript, como jQuery, navegação de script se tornou um desafio por causa do comprimento do código.

Visual Studio renovou o editor do JavaScript com a promessa de tornar o modo código organizado e acessível. Muitos recursos do Visual Studio que já existiam nos editores de c# ou VB agora são implementados no editor de JavaScript: Ir para definição, recuo automático, documentação e validação quando você está escrevendo. Com a lista do IntelliSense renovada, você terá o catálogo de função JavaScript ao seu alcance.

Neste exercício, você aprenderá a alguns dos novos recursos e aprimoramentos do editor de JavaScript. Você irá procurar arquivos de exemplo e descobrir cada uma das novas características que tornará sua programação JavaScript mais eficiente no Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Tarefa 1: novos recursos do Editor de JavaScript

Essa tarefa apresentará algumas dos novos recursos de editor do JavaScript, com foco em organizar seu código e colocar uma melhor experiência do usuário.

1. Se ainda não estiver aberto, inicie **Visual Studio** e abra o **WhatsNewASPNET.sln** solução localizada no **Source\WhatsNewASPNET** pasta deste laboratório.
2. Pressione **F5** para executar o aplicativo, em seguida, clique no link JavaScript na barra de navegação. Atualize a página várias vezes e verificar como os incrementos de contador.

    ![Contador de página](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "contador de página")

    *Contador de página*
3. Feche o navegador e volte para o Visual Studio.
4. Abra o **JavaScript.aspx** da página e localize o **&lt;script&gt;** bloco (mostrado abaixo).

    O código a seguir usa o armazenamento local do HTML5 para armazenar um *pageLoadCount* variável que armazena o número de vezes que a página foi visitada pelo usuário atual. Armazenamento local é um banco de dados do lado do cliente chave-valor introduzido com o HTML5 padrão. Os dados são salvos no computador local, dentro do navegador do usuário.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Certifique-se de que o DOCTYPE é definido para XHTML5 antes de continuar com as próximas etapas.
5. Editar o código e observe que o IntelliSense para JavaScript inclui recursos do HTML5, como o armazenamento local e seus métodos internos.

    ![Recursos do JavaScript HTML5 do JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "recursos JavaScript HTML5 do JavaScript")

    *Recursos do JavaScript do HTML5 no JavaScript*
6. Clique em qualquer colchete de abertura (**{**) do que o script de código e observe que os colchetes são realçados.

    ![Colchetes são realçados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "colchetes são realçados")

    *Colchetes são realçados*
7. Remova a função **testAutoAlign()** (selecione as três linhas, e você pode usar **CTRL** + **K**; **CTRL** + **U**) e localize o cursor dentro do código da função. Pressione enter para acrescentar uma segunda linha. Observe que agora o código está **alinhado** e **memorizado automaticamente**.

    ![O código JavaScript é automaticamente alinhado](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "código JavaScript é automaticamente alinhado")

    *O código JavaScript é automaticamente alinhado*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Tarefa 2 - validação de JavaScript

Nesta tarefa, você descobrirá que a nova validação de JavaScript para o padrão de ECMAScript5. Esse recurso o ajudará a escrevem código JavaScript em conformidade, evitando problemas de script antes da implantação do site.

> [!NOTE]
> Visual Studio 2010 implementou ECMAStript3 conformidade, enquanto o Visual Studio 2012 fornece conformidade ECMAScript5.


1. Abra **ECMA5script5.js** localizado sob a **Scripts\custom** pasta do projeto. Agora você irá testar validação ecmascript5 padrão.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Você pode fazer check-out a &quot; **estrito&lt;2}&lt;1** &quot; direção na primeira linha do arquivo, que permite a ECMAScript5 **modo estrito**. Esse modo consiste em um subconjunto da linguagem que esclarece as ambiguidades da última edição e adiciona alguns novos recursos, como getters e setters, suporte de biblioteca para JSON e reflexão mais completa sobre as propriedades do objeto.
2. Abra o **lista de erros** se ainda não estiver aberto (**exibição** menu | **Error List**). Observe que o **função** declaração está sublinhada. Isso ocorre porque no ECMA5 funções padrão não podem ser aninhadas dentro de estruturas de linguagem. O erro lista abaixo, você verá os detalhes do aviso.

    ![Mensagem de erro de validação de JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "mensagem de erro de validação de JavaScript")

    *Mensagem de erro de validação de JavaScript*
3. Comente a **&quot;estrito&lt;2}&lt;1&quot;** direção e observe que erros desaparecerão, mas permanecem os avisos.
4. Na última linha do arquivo, gravar qualquer cadeia de caracteres like **&quot;testar&quot;** (inclua as aspas para indicar que ele é como cadeia de caracteres). Gravar um período próximo a cadeia de caracteres para exibir a lista do IntelliSense e, em seguida, selecione a **trim** opção.

    No padrão de ECMAScript5, variáveis e valores de cadeia de caracteres também têm métodos de cadeia de caracteres definidos, como cortar, letras maiusculas, pesquisar e substituir.

    ![Lista do IntelliSense em JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "lista do IntelliSense em JavaScript")

    *Lista do IntelliSense em JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Tarefa 3 – documentação XML para JavaScript

Nesta tarefa, você irá explorar os recursos do Visual Studio para obter a documentação XML em JavaScript. Você verá que a lista do IntelliSense do JavaScript agora mostra a documentação XML de cada função. Além disso, você descobrirá que o recurso de navegação em JavaScript.

1. Abra **XMLDoc.js** arquivo localizado em **Scripts/personalizada** pasta do projeto. Esse arquivo contém a documentação XML em cada uma das funções de JavaScript.

    ![Documentação XML JavaScript integrado ao IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "integrado de documentação XML JavaScript IntelliSense")

    *Documentação XML JavaScript integrado ao IntelliSense*
2. Abaixo **adicione** funcionar **XMLDoc.js** arquivo, crie uma nova função chamada **testar**.
3. No **testar** de função, chame o **multiplicar** função que recebe dois parâmetros. Observe que a caixa de dica de ferramenta está mostrando o **multiplicar** documentação da função.

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Documentação XML para as funções JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "documentação XML para as funções JavaScript")

    *Documentação XML para as funções JavaScript*
4. Conclua a instrução de chamada de função e tipo de um *dot* para abrir a lista do IntelliSense no valor retornado. Observe que o Visual Studio está detectando as **retornar** valor na documentação, tratando o valor como um número.

    ![Documentação XML para tipos de retorno](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "documentação XML para tipos de retorno")

    *Documentação XML para tipos de retorno*
5. Agora, insira uma chamada para adicionar a função. Observe que o editor de JavaScript agora suporta sobrecargas de função. Quando você grava um nome de função, você poderá selecionar qualquer uma das sobrecargas disponíveis especificadas na documentação.

    ![Documentação XML para sobrecargas](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "documentação XML para sobrecargas")

    *Documentação XML para sobrecargas*
6. Abra **GotoDefinition.js** do arquivo e localize o **$().html()** chamada de função. Localize o cursor na **html**.
7. Pressione **F12** e navegue até a definição. Observe que agora você pode acessar e procurar seu código JavaScript sem usar o **localizar** ferramenta.
8. Localize o cursor sobre a instrução de jQuery antes do bloco de assinatura na parte inferior do arquivo de código. Pressione **F12**. Você navegará para o arquivo de biblioteca jQuery. Observe que você também pode navegar em todos os arquivos do jQuery usando **F12**.

    ![Navegar para as definições do jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "navegando até as definições do jQuery")

    *Navegar para as definições do jQuery*

> [!NOTE]
> Certifique-se de que GotoDefinition.js não tem nenhum erro de sintaxe antes de salvar o arquivo.


<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Exercício 4: Agrupamento e Minificação

Quantas vezes seus sites incluir mais de um JavaScript ou CSS arquivo? Isso é um cenário muito comum onde agrupamento e minificação podem ajudar a reduzir o tamanho do arquivo e fazer com que o site de um desempenho mais rápido. O novo recurso de agrupamento no ASP.NET 4.5 empacota um conjunto de arquivos JS ou CSS em um único elemento e reduz seu tamanho, a minificação o conteúdo (ou seja, removendo os espaços em branco não é necessários, remoção de comentários, reduzindo identificadores).

Agrupamento e minificação no ASP.NET 4.5 é executada no tempo de execução para que o processo possa identificar o agente do usuário (por exemplo, IE, Mozilla, etc) e, assim, melhorar a compactação direcionando o navegador do usuário (por exemplo, remover coisas que é específico do Mozilla Quando a solicitação vem de IE).

Neste exercício, você aprenderá como habilitar e usar os diferentes tipos de agrupamento e minificação no ASP.NET 4.5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Tarefa 1: instalar o agrupamento e Minificação de pacote do NuGet

1. Se ainda não estiver aberto, inicie **Visual Studio** e abra o **WhatsNewASPNET.sln** solução localizada no **Source\WhatsNewASPNET** pasta deste laboratório.
2. Abra o Console do Gerenciador de pacotes do NuGet. Para fazer isso, use o menu **modo de exibição** | **Other Windows** | **Package Manager Console**.

    ![Abrindo o pacote manager file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "abrindo o console do Gerenciador de pacotes")

    *Abrindo o console do Gerenciador de pacotes*
3. No **Package Manager Console** tipo **Install-Package Microsoft.Web.Optimization** e pressione **ENTER**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Tarefa 2 - grupos padrão

A maneira mais simples de usar agrupamento e minificação é habilitar os grupos padrão. Esse método usa as convenções para permitir que você fazer referência à versão agrupada e minificada para os arquivos CSS e JS em uma pasta.

Nesta tarefa, você aprenderá como habilitar e fazer referência aos arquivos CSS e JS agrupados e minificados e exibir a saída resultante.

1. Se ainda não estiver aberto, inicie **Visual Studio** e abra o **WhatsNewASPNET.sln** solução localizada no **Source\WhatsNewASPNET** pasta deste laboratório.
2. No **Gerenciador de soluções**, expanda o **estilos**, **Scripts\custom** e **Scripts\bundle** pastas.

    Observe que o aplicativo está usando o arquivo mais CSS e JS.

    ![Arquivos de várias folhas de estilo e o JavaScript no aplicativo](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "arquivos várias folhas de estilo e o JavaScript no aplicativo")

    *Vários arquivos de folhas de estilo e o JavaScript no aplicativo*
3. Abra o **Global.asax.cs** arquivo.

    Observe que o novo **Microsoft.Web.Optimization** namespace é comentado no início do arquivo. Remova o usando a diretiva para incluir os recursos de agrupamento e minificação.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Localize o **Application\_iniciar** método.

    Nesse método, remova a chamada EnableDefaultBundles, conforme mostrado no trecho a seguir. Isso nos permite fazer referência a uma coleção de pacote de arquivos CSS em uma pasta usando o caminho para essa pasta, mais as &quot;CSS&quot; ou o &quot;JS&quot; sufixo.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Abra o **Optimization.aspx** do arquivo e localize o controle de conteúdo para **HeadContent**.

    Observe que os arquivos CSS e JS têm uma única marca referenciada.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Esse código é para fins de demonstração. O ideal é que você irá referenciar os pacotes no arquivo master. Esse código de exemplo, você encontrará que alguns dos arquivos de pacote também estão sendo referenciadas pelo arquivo master, tornando esta última referência redundante.
6. Observe que os links estão usando as convenções de agrupamento de **href** atributo para obter os arquivos de todos os CSS ou JS dos estilos e Scripts\custom pasta, respectivamente.

    Você pode usar o caminho **personalizado/Scripts/JS** conforme mostrado abaixo para agrupar e minificar todos os arquivos JS dentro de uma **Scripts/personalizada** pasta. Isso é o comportamento padrão com os grupos padrão.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Abra o **Styles\Site.css** arquivo.

    Observe que o arquivo CSS original contém código recuado, espaços em branco e comentários que aumentar o arquivo. (Também o arquivo JavaScript contém comentários e espaços em branco).

    ![O CSS original dos arquivos na pasta de Scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "o CSS original dos arquivos na pasta de Scripts")

    *Um dos arquivos CSS originais na pasta de Scripts*
8. Pressione **F5** para executar o aplicativo e navegue até a **otimização** página.
9. Clique no **grupo CSS** link para baixar e abrir o arquivo.

    Confira o arquivo agrupado minimizado. Você observará que todos os espaços em branco, comentários e caracteres de recuo foram removidas, gerando um arquivo menor.

    ![Arquivos CSS empacotados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "arquivos CSS agrupado")

    *Arquivos CSS do pacote*
10. Agora, clique o **JS pacote** link para abrir o arquivo JavaScript agrupado. Você poderá desconsiderar o aviso de explorer. Observe os arquivos JavaScript sob o **personalizado** pasta também são agrupado e minificado.

    ![Arquivos JavaScript empacotados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "arquivos JavaScript agrupado")

    *Arquivos JavaScript do pacote*

    A habilitação da compactação de arquivos CSS ou JS era muito mais complicada na versão anterior do ASP.NET. Agora, como você viu, basta adicionar uma linha na *global. asax* para habilitar o agrupamento de arquivo e, em seguida, fazer referência os arquivos do pacote do seu site.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Tarefa 3: grupos estáticos

A abordagem de pacote estático permite que você personalize o conjunto de arquivos de pacote, a referência e o método de minimização que será usado.

Nesta tarefa, você irá configurar um pacote estático para definir um conjunto específico de arquivos para agrupar e minificar.

1. Feche o navegador.
2. Abra o **Global.asax.cs** do arquivo e localize o **aplicativo\_iniciar** método.
3. Descomente o código de pacote estático, conforme mostrado no código a seguir.

    Você está definindo um pacote estático que será referenciado com o &quot; **~/StaticBundle** &quot; caminho virtual e use **JsMinify** para minimização de todos os arquivos especificados com o **AddFile** método. Por fim, você está adicionando o pacote de estático para o **BundleTable** e habilitá-la.

    Observe que os arquivos não estão localizados no mesmo lugar; Isso é outra vantagem sobre o agrupamento padrão.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Abra o **Optimization.aspx** arquivo.

    Observe que o link para **pacote de JS estáticos** é usando o caminho que você tenha declarado quando você configurou o pacote estático no arquivo Global.asax.cs: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Pressione **F5** para executar o aplicativo e, em seguida, navegue até a **otimização** página.
6. Clique no **pacote estático de JS** link para abrir o arquivo.

    Observe que o minificado agrupado arquivo JavaScript é a saída para todos os arquivos JavaScript configurados no arquivo de pacote estático no caminho &quot;/StaticBundle&quot;.

    ![Pacote de arquivos estático JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "pacote de arquivos estáticos JavaScript")

    *Pacote de arquivos estático JavaScript*
7. Feche o navegador e retorne ao Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Tarefa 4 - pacotes de pasta dinâmico

Nesta tarefa, você aprenderá como configurar pacotes de pasta dinâmico. O poder de agrupamento dinâmico é que você pode incluir JavaScript estático, bem como outros arquivos em idiomas que é compilado em JavaScript e assim, exigem algum processamento antes do agrupamento é executada.

Neste exemplo, você aprenderá como usar o **DynamicFolderBundle** classe para criar um conjunto dinâmico para arquivos escritos em CofeeScript. CofeeScript é uma linguagem de programação que é compilada no JavaScript e fornece uma sintaxe mais simples para escrever código JavaScript, aprimora a brevidade do JavaScript e a legibilidade.

1. Abra o **Global.asax.cs** do arquivo e localize o **aplicativo\_iniciar** método.
2. Descomente o código de pacote dinâmico, conforme mostrado no código a seguir.

    Você está definindo um pacote da pasta dinâmico que usará o **CoffeeMinify** processador minificação personalizados que se aplicam somente a arquivos com o &quot; **. Coffee** &quot; (extensão Arquivos CoffeeScript). Observe que você pode usar um padrão de pesquisa para selecionar os arquivos de empacotamento dentro de uma pasta, como '\*. Coffee '.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Abra o Console do Gerenciador de pacotes do NuGet. Para fazer isso, use o menu **modo de exibição** | **Other Windows** | **Package Manager Console**.
4. No **Package Manager Console** tipo **Install-Package CoffeeSharp** e pressione **ENTER**.
5. Clique o **Show All Files** botão na **Gerenciador de soluções** janela

    ![Mostrando todos os arquivos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "mostrando todos os arquivos")

    *Mostrando todos os arquivos*
6. Clique com botão direito do **CoffeeMinify.cs** de arquivo na **Gerenciador de soluções** e selecione **incluir no projeto**

    ![Incluir o arquivo CoffeeMinify.cs no projeto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "incluir o arquivo CoffeeMinify.cs no projeto")

    *Incluir o arquivo CoffeeMinify.cs no projeto*
7. Abra o **CoffeeMinify.cs** arquivo.

    Essa classe herda de JsMinify para reduzir a saída de JavaScript resultante da compilação de código CoffeeScript. Ele chama o compilador CoffeeScript para gerar o código JavaScript pela primeira vez e, em seguida, ele envia para o método de JsMinify.Process para reduzir o código resultante.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Abra o **Script1.coffee** e **Script2.coffee** arquivos dos **Scripts/pacote** pasta.

    Esses arquivos incluirá o código de CoffeScript ser compilada ao realizar o agrupamento com a classe CoffeeMinify.

    Para fins de simplicidade, os arquivos CoffeeScript fornecidos são apenas incluindo código de exemplo do CoffeeScript. Os comentários são excluídos pelo processo de JsMinify.

    ![Arquivos CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "arquivos CoffeeScript")

    *Arquivos CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) fornece uma sintaxe mais simples para escrever código JavaScript, aprimora a legibilidade e a brevidade do JavaScript, bem como adicionar outros recursos, como a compreensão de matriz e a correspondência de padrões.
9. Abra o **Optimization.aspx** de arquivos e localize os links do pacote.

    Observe que o link para **pacote dinâmico do JS** está fazendo referência a **Scripts/pacote** pasta usando o **/café** sufixo que você configurou para o pacote da pasta dinâmico.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Pressione **F5** para executar o aplicativo e, em seguida, navegue até a **otimização** página.
11. Clique no **pacote de dinâmica JS** link para abrir o arquivo gerado.

    Observe que o conteúdo que foi incluído neste pacote contém apenas **. Coffee** arquivos. Você também pode ver que o código de CoffeeScript foi compilado para JavaScript e as linhas comentadas foi removido.

    ![Pacote de arquivos dinâmicos do JS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "pacote de arquivos JS dinâmico")

    *Pacote de arquivos JS dinâmico*

> [!NOTE]
> Além disso, você pode implantar esse aplicativo para Windows Azure Web Sites a seguir [apêndice b: publicando um aplicativo ASP.NET MVC 4 usando a implantação da Web](#AppendixB).


<a id="Summary"></a>
## <a name="summary"></a>Resumo

Este laboratório ajuda você a entender o que é novo no ASP.NET e desenvolvimento da Web no Visual Studio 2012 e como tirar proveito da variedade de aprimoramentos no Visual Studio 2012.

Ao concluir este laboratório prático, você tem aprendeu a usar os novos recursos e aprimoramentos no Visual Studio 2012 editores CSS, JavaScript e HTML. Além disso, você tem adquiridos como o Visual Studio 2012 implementa interno de agrupamento e minificação.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apêndice a: instalação do Visual Studio Express 2012 para Web

Você pode instalar **Microsoft Visual Studio Express 2012 para Web** ou outra &quot;Express&quot; versão usando o **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. As instruções a seguir guia você pelas etapas necessárias para instalar *Visual studio Express 2012 para Web* usando *Microsoft Web Platform Installer*.

1. Vá para [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, se você já tiver instalado o Web Platform Installer, você pode abri-lo e pesquisar o produto &quot; <em>Visual Studio Express 2012 para Web com o SDK do Windows Azure</em>&quot;.
2. Clique em **instalar agora**. Se você não tiver **Web Platform Installer** você será redirecionado para baixar e instalá-lo primeiro.
3. Uma vez **Web Platform Installer** é aberto, clique em **instalar** para iniciar a instalação.

    ![Instalar o Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "instalar o Visual Studio Express")

    *Instalar o Visual Studio Express*
4. Leia os termos e licenças de todos os produtos e clique em **aceito** para continuar.

    ![Aceitar os termos de licença](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Aceitar os termos de licença*
5. Aguarde até que o processo de download e instalação for concluído.

    ![Progresso da instalação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Progresso da instalação*
6. Quando a instalação for concluída, clique em **concluir**.

    ![Instalação concluída](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Instalação concluída*
7. Clique em **Exit** para fechar o Web Platform Installer.
8. Para abrir o Visual Studio Express para Web, vá para o **iniciar** de tela e comece a escrever &quot; **VS Express**&quot;, em seguida, clique no **VS Express para Web** lado a lado.

    ![VS Express para o bloco da Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express para o bloco da Web*

* * *

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apêndice b: publicando um aplicativo ASP.NET MVC 4 usando a implantação da Web

Este apêndice mostram como criar um novo site do Portal de gerenciamento do Windows Azure e publicar o aplicativo que você obteve seguindo o ambiente de laboratório, aproveitando o recurso de publicação de implantação da Web fornecido pelo Windows Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarefa 1 - criar um novo Site do Windows Azure Portal

1. Vá para o [Portal de gerenciamento do Windows Azure](https://manage.windowsazure.com/) e entre usando as credenciais da Microsoft associadas à sua assinatura.

    > [!NOTE]
    > Com o Windows Azure, você pode hospedar 10 Sites da Web ASP.NET gratuitamente e, em seguida, dimensione conforme seu tráfego aumenta. Você pode se inscrever [aqui](http://aka.ms/aspnet-hol-azure).

    ![Faça logon no portal do Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "faça logon no portal do Windows Azure")

    *Faça logon no Portal de gerenciamento do Azure do Windows*
2. Clique em **New** na barra de comandos.

    ![Criando um novo Site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "criando um novo Site")

    *Criando um novo Site*
3. Clique em **Compute** | **Site da Web**. Em seguida, selecione **criação rápida** opção. Forneça uma URL disponível para o novo site da web e clique em **Criar Site**.

    > [!NOTE]
    > Um Site do Windows Azure é o host para um aplicativo web em execução na nuvem que você pode controlar e gerenciar. A opção criação rápida permite que você implantar um aplicativo web completo para o Windows Azure Site de fora do portal. Ele não inclui as etapas para configurar um banco de dados.

    ![Criando um novo Site usando a criação rápida](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "criando um novo Site usando a criação rápida")

    *Criando um novo Site usando a criação rápida*
4. Aguarde até que o novo **Site da Web** é criado.
5. Depois que o Site da Web é criado, clique no link sob a **URL** coluna. Verifique se o novo Site da Web está funcionando.

    ![Navegando até o novo site](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "navegando até o novo site da web")

    *Navegando até o novo site da web*

    ![Site da Web em execução](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "site da Web em execução")

    *Site da Web em execução*
6. Volte para o portal e clique no nome do site da web sob o **nome** coluna para exibir as páginas de gerenciamento.

    ![Abrir as páginas de gerenciamento do site da web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "abrir as páginas de gerenciamento do site da web")

    *Abrir as páginas de gerenciamento do Site da Web*
7. No **Dashboard** página na **visão geral rápida** seção, clique no **baixar perfil de publicação** link.

    > [!NOTE]
    > O *perfil de publicação* contém todas as informações necessárias para publicar um aplicativo web para um site do Windows Azure para cada método de publicação habilitado. O perfil de publicação contém as URLs, as credenciais do usuário e cadeias de caracteres de banco de dados necessárias para conectar e autenticar em relação a cada um dos pontos de extremidade para o qual um método de publicação está habilitado. **Microsoft WebMatrix 2**, **Microsoft Visual Studio para Web Express** e **Microsoft Visual Studio 2012** oferecem suporte à leitura perfis de publicação para automatizar a configuração desses programas para publicação de aplicativos web nos sites do Windows Azure.

    ![Baixando o site da web de perfil de publicação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "baixando o site da web de perfil de publicação")

    *Baixando o Site da Web de perfil de publicação*
8. Baixe o arquivo de perfil de publicação em um local conhecido. Ainda mais neste exercício, você verá como usar esse arquivo para publicar um aplicativo web para um Windows Azure Web Sites do Visual Studio.

    ![Salvando o arquivo de perfil de publicação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "salvar o perfil de publicação")

    *Salvando o arquivo de perfil de publicação*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarefa 2 – Configurando o servidor de banco de dados

Se seu aplicativo faz uso do SQL Server você precisará criar um servidor de banco de dados SQL de bancos de dados. Se você quiser implantar um aplicativo simples que não usa o SQL Server, você pode ignorar esta tarefa.

1. Você precisará de um servidor de banco de dados SQL para armazenar o banco de dados do aplicativo. Você pode exibir os servidores de banco de dados SQL na sua assinatura no portal de gerenciamento do Windows Azure em **bancos de dados Sql** | **servidores** | **do servidor Painel**. Se você não tiver um servidor criado, você pode criar uma usando o **adicionar** botão na barra de comandos. Anote o **nome do servidor e o URL, o nome de logon de administrador e senha**, pois você usará nas próximas tarefas. Não crie o banco de dados ainda, ele será criado em um estágio posterior.

    ![Painel do servidor de banco de dados SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "painel de banco de dados do SQL Server")

    *Painel de banco de dados do SQL Server*
2. A próxima tarefa, você testará a conexão de banco de dados do Visual Studio, por esse motivo, você precisa incluir seu endereço IP local na lista do servidor **endereços IP permitidos**. Para fazer isso, clique em **configurar**, selecione o endereço IP do **endereço de IP do cliente atual** e cole-o na **endereço IP inicial** e **oendereçoIPfinal** caixas de texto. Insira um nome para a regra e clique no ![add-client-ip-address-ok-button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png) botão.

    ![Adicionar endereço IP do cliente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Adicionar endereço IP do cliente*
3. Uma vez a **endereço IP do cliente** é adicionado para endereços IP, clique em **salvar** para confirmar as alterações.

    ![Confirmar alterações](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Confirmar alterações*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarefa 3 – publicar um aplicativo ASP.NET MVC 4 usando a implantação da Web

1. Volte para a solução do ASP.NET MVC 4. No **Gerenciador de soluções**, clique com botão direito no projeto de site da web e selecione **publicar**.

    ![O aplicativo de publicação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "publicando o aplicativo")

    *Publicar o site da web*
2. Importe o perfil de publicação que você salvou na primeira tarefa.

    ![Importando o perfil de publicação](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "importando o perfil de publicação")

    *Importando o perfil de publicação*
3. Clique em **validar Conexão**. Depois que a validação estiver concluída, clique em **próxima**.

    > [!NOTE]
    > A validação é concluída quando você vir uma marca de seleção verde aparecer ao lado do botão Validar Conexão.

    ![Validar conexão](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "validar conexão")

    *Validando a conexão*
4. No **as configurações** página na **bancos de dados** seção, clique no botão ao lado da caixa de texto do sua conexão banco de dados (ou seja, **DefaultConnection**).

    ![Configuração de implantação da Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "configuração de implantação da Web")

    *Configuração de implantação da Web*
5. Configure a conexão de banco de dados da seguinte maneira:

   - No **nome do servidor** digite sua URL de servidor de banco de dados SQL usando o *tcp:* prefixo.
   - Na **nome de usuário** digite seu nome de logon de administrador do servidor.
   - Na **senha** digite sua senha de logon de administrador do servidor.
   - Digite um novo nome de banco de dados, por exemplo: *MVC4SampleDB*.

     ![Configurando a cadeia de caracteres de conexão de destino](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configurando a cadeia de caracteres de conexão de destino")

     *Configurando a cadeia de caracteres de conexão de destino*
6. Clique em **OK**. Quando for solicitado a criar o banco de dados, clique em **Sim**.

    ![Criando o banco de dados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "criando a cadeia de caracteres de banco de dados")

    *Criando o banco de dados*
7. A cadeia de caracteres de conexão que você usará para se conectar ao banco de dados SQL no Windows Azure é mostrada na caixa de texto de Conexão padrão. Clique em **Avançar**.

    ![Cadeia de caracteres de Conexão que aponta para o banco de dados SQL](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "cadeia de caracteres de Conexão que aponta para o banco de dados SQL")

    *Cadeia de caracteres de Conexão que aponta para o banco de dados SQL*
8. No **versão prévia** , clique em **publicar**.

    ![Publicando o aplicativo web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "publicando o aplicativo web")

    *Publicar o aplicativo da web*
9. Quando o processo de publicação for concluído, o navegador padrão abrirá o site publicado.

    ![Aplicativo publicado no Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "aplicativo publicado no Windows Azure")

    *Aplicativo publicado para o Windows Azure*
