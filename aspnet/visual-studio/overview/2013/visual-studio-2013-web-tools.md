---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratório prático: Visual Studio 2013 Web Tools | Microsoft Docs'
author: rick-anderson
description: O Visual Studio é um ambiente de desenvolvimento excelente para. Janelas baseadas em rede e projetos da web. Ele inclui um editor de texto avançado que pode ser facilmente usado para...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: ef8ab82f9043ef9da3a3e6a146a97f083149534d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
ms.locfileid: "26507125"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Laboratório prático: Ferramentas de Web Visual Studio 2013
====================
Por [Web Camps Team](https://twitter.com/webcamps)

[Baixar o Kit de treinamento de Camps de Web](http://aka.ms/webcamps-training-kit)

> O Visual Studio é um ambiente de desenvolvimento excelente para. Janelas baseadas em rede e projetos da web. Ele inclui um editor de texto avançado que pode ser facilmente usado para editar arquivos autônomos sem um projeto.
> 
> O Visual Studio mantém uma árvore de análise completa ao editar cada arquivo. Isso permite que o Visual Studio fornecer preenchimento automático incomparável e ações com base em documento durante a experiência de desenvolvimento muito mais rápido e mais agradável. Esses recursos são especialmente eficientes em documentos HTML e CSS.
> 
> Toda essa capacidade também está disponível para as extensões, tornando mais fácil estender os editores com novos recursos poderosos para atender às suas necessidades. Web Essentials é uma coleção de (principalmente) relacionados à web aprimoramentos para o Visual Studio. Ele inclui vários novos conclusões IntelliSense (especialmente para CSS), novos recursos de Link do navegador, automático JSHint para JavaScript arquivos, novos avisos para HTML, CSS e muitos outros recursos que são essenciais para o desenvolvimento na web moderna.
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponíveis em [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Visão geral

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Este laboratório prático, você aprenderá como:

- Usar os novos recursos do editor HTML incluídos no Web Essentials como ricos trechos de código do HTML5 e Zen codificação
- Use os novos recursos de editor de CSS incluídos no Essentials Web como o seletor de cores e uma dica de ferramenta de matriz de navegador
- Usar novos recursos do editor JavaScript incluídos no Web Essentials como extrair para o arquivo e o IntelliSense para todos os elementos HTML
- Trocar dados entre o navegador e o Visual Studio usando o Link do navegador

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

O exemplo a seguir é necessário para concluir este laboratório prático:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) ou maior
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Configuração

Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente primeiro.

1. Abra uma janela do Windows Explorer e navegue até o laboratório **fonte** pasta.
2. Clique com botão direito **Setup.cmd** e selecione **executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instale os trechos de código do Visual Studio para este laboratório.
3. Se a caixa de diálogo controle de conta de usuário for exibida, confirme a ação para continuar.

> [!NOTE]
> Verifique se que você selecionou todas as dependências para este laboratório antes de executar a instalação.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usando os trechos de código

Em todo o documento de laboratório, você será instruído a inserir blocos de código. Para sua conveniência, a maioria do código é fornecido como o Visual Studio trechos de código que pode ser acessada de dentro do Visual Studio 2013 para evitar a necessidade para adicioná-lo manualmente.

> [!NOTE]
> Cada exercício é acompanhado por uma solução inicial localizada no **começar** pasta do exercício que permite que você execute cada exercício independentemente de outras pessoas. Esteja ciente de que os trechos de código que são adicionados durante um exercício estão ausentes dos seguintes iniciando soluções e podem não funcionar até que você concluiu o exercício. Dentro do código-fonte para um exercício, você também encontrará um **final** pasta que contém uma solução do Visual Studio com o código que é o resultado de concluir as etapas no exercício correspondente. Você pode usar essas soluções como orientação se você precisar de ajuda adicional usando este laboratório prático.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Trabalhando com os conceitos básicos de Web e de Link do navegador](#Exercise1)
2. [Aproveitando as vantagens de trechos de código e o IntelliSense](#Exercise2)

> [!NOTE]
> Quando você inicia o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas. Cada coleção predefinida foi projetada para coincidir com um estilo de desenvolvimento específico e determina o comportamento do editor, layouts de janela, trechos de código IntelliSense e opções da caixa de diálogo. Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma tarefa específica no Visual Studio ao usar o **configurações gerais de desenvolvimento** coleção. Se você escolher uma coleção de configurações diferentes para o seu ambiente de desenvolvimento, pode haver diferenças nas etapas que você deve levar em conta.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Exercício 1: Trabalhando com os conceitos básicos de Web e de Link do navegador

**Web Essentials** é uma extensão do Visual Studio que adiciona uma variedade de recursos úteis para o desenvolvimento de web modernos, principalmente se concentra em tornar a experiência de desenvolvimento da web muito mais rápida e mais agradável. Você pode instalar o Web Essentials na Galeria de extensão no Visual Studio.

**Link do navegador** é um novo recurso incluído no Visual Studio 2013 que fornece um canal entre o Visual Studio IDE e qualquer navegador aberto para trocar dados entre seu aplicativo web e o Visual Studio. Web Essentials estende o Link do navegador com ferramentas para manipular o modelo de objeto DOM e os estilos CSS de suas páginas da web diretamente do navegador.

Neste exercício, você irá explorar alguns dos recursos com suporte **Web Essentials** e **Link do navegador** para aprimorar uma página de teste simples.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Tarefa 1: executar o projeto em vários navegadores

Nesta tarefa, você irá configurar seu aplicativo da web para ser executado em vários navegadores de uma vez, que é útil para testes entre navegadores.

1. Abra **Microsoft Visual Studio**.
2. No **arquivo** menu, selecione **abrir | Projeto/solução...**  e navegue até **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** no **fonte** pasta do laboratório (C:\WebCampsTK\HOL\VSWebTooling\Source). Selecione **Begin.sln** e clique em **abrir**.
3. Na barra de ferramentas do Visual Studio, expanda o menu do navegador e selecione **procurar com...** .

    ![Opção de menu procurar com](visual-studio-2013-web-tools/_static/image1.png "procurar com... no menu do navegador")

    *Procurar com opção de menu*
4. No **procurar com** caixa de diálogo, selecione **Google Chrome** e **Internet Explorer** segurando o **CTRL** chave e clique em  **Definir como padrão**.

    ![Procurar a caixa de diálogo](visual-studio-2013-web-tools/_static/image2.png "procurar a caixa de diálogo")

    *Selecionando vários navegadores padrão*
5. Google Chrome e o Internet Explorer agora devem aparecer como os navegadores padrão. Clique em **Cancelar** para fechar a caixa de diálogo.

    ![Google Chrome e o Internet Explorer como navegadores padrão](visual-studio-2013-web-tools/_static/image3.png "navegadores de padrão do Internet Explorer e Google Chrome")

    *Google Chrome e o Internet Explorer como navegadores padrão*

    > [!NOTE]
    > Depois de configurar os navegadores padrão, o **vários navegadores** opção é selecionada no menu do navegador.
    > 
    > ![Vários navegadores](visual-studio-2013-web-tools/_static/image4.png "vários navegadores")
6. Pressione **CTRL** + **F5** para executar o aplicativo sem depuração.
7. Quando ambas as janelas do navegador se abre, coloque um deles acima do outro para ver as atualizações em ambos os navegadores simultaneamente. Os navegadores devem exibir uma página da web com um retângulo azul-claro.

    ![Colocando um navegador acima do outro](visual-studio-2013-web-tools/_static/image5.png "colocando um navegador acima do outro")

    *Colocando um navegador acima do outro*
8. Não feche os navegadores. Você irá usá-los na próxima tarefa.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Tarefa 2 - Zen usando codificação para criar elementos HTML

**Codificação Zen** é um editor de codificação de plug-in de alta velocidade HTML, XML, XSL (ou qualquer outro formato de código estruturado) e edição. O núcleo deste plug-in é um mecanismo de abreviação poderoso que permite que você expanda expressões - semelhantes para seletores CSS - em código HTML. Codificação Zen é uma maneira rápida de escrever a sintaxe do seletor de estilo HTML usando uma CSS.

Neste exercício, você usará o recurso Zen codificação fornecido pelo Web Essentials para gerar os botões HTML que representam as opções da pergunta.

1. Alterne para o Visual Studio.
2. Abra o **cshtml** arquivo localizado no **exibições** | **início** pasta.
3. Substitua o **&lt;! – TODO: Adicione aqui – opções&gt;** comentário com o seguinte código e pressione **guia**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. O código deve ser expandido para HTML.

    ![Expandido HTML](visual-studio-2013-web-tools/_static/image6.png "expandido HTML")

    *HTML expandido*

    > [!NOTE]
    > Para saber mais sobre a sintaxe Zen de codificação, consulte o seguinte [artigo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Clique o **atualizar navegadores vinculados** botão atualizar ambos os navegadores.

    ![Atualizar navegadores vinculados](visual-studio-2013-web-tools/_static/image7.png "atualizar navegadores vinculados")

    *Atualizar navegadores vinculados*

    ![Internet Explorer - página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - página atualizada com quatro botões")

    *Internet Explorer - página atualizada com quatro botões*

    ![Google Chrome - página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - página atualizada com quatro botões")

    *Google Chrome - página atualizada com quatro botões*
6. Alterne para o Visual Studio.
7. Adicionar os botões para a página, mas você ainda precisa adicionar uma pergunta de modelo. Para fazer isso, você usará um novo recurso no Essentials Web chamado **Lorem Ipsum gerador**. Localize o **div** elemento com o **classe** atributo **frontal**.
8. Adicione o seguinte código como o primeiro elemento filho do **div**e pressione **guia**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. O código deve ser expandido para HTML.

    ![Gerado automaticamente do Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "gerado Lorem Ipsum automaticamente")

    *Gerado Lorem Ipsum automaticamente*

    > [!NOTE]
    > Como parte da codificação Zen, agora é possível gerar código Lorem Ipsum diretamente no editor de HTML. Basta digitar **lorem** e clique em **guia** e um 30 palavras Lorem Ipsum o texto será inserido. Por exemplo, *lorem10* insere 10 Lorem Ipsum palavras.
10. Você irá adicionar um logotipo na parte superior da pergunta usando outro novo recurso no Essentials Web chamado **gerador de Lorem Pixel**. Adicione o seguinte código como o primeiro elemento filho do **div** elemento com **contêiner** como **classe** valor e pressione **guia**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. O código deve expandir para HTML.

    ![Gerado automaticamente de Lorem Pixel](visual-studio-2013-web-tools/_static/image11.png "gerados automaticamente de Lorem Pixel")

    *Gerado automaticamente de Lorem Pixel*

    > [!NOTE]
    > Como parte da codificação Zen, você também pode gerar o código de Lorem Pixel diretamente no editor de HTML. Basta digitar **pix-200 x 200-animais** e clique em **guia** e um **img** marca com uma imagem de um animal de 200 x 200 será inserida. Para obter mais informações, consulte [Lorem Pixel](http://www.lorempixel.com).
12. Clique o **atualizar navegadores vinculados** botão atualizar ambos os navegadores.

    ![Internet Explorer - imagem gerada automaticamente e texto](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - imagem gerada automaticamente e texto")

    *Internet Explorer - imagem gerada automaticamente e texto*

    ![Google Chrome - imagem gerada automaticamente e texto](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - imagem gerada automaticamente e texto")

    *Google Chrome - imagem gerada automaticamente e texto*

    > [!NOTE]
    > Como a imagem é selecionada aleatoriamente ao adicionar o trecho de código, a imagem mostrada os navegadores pode diferir.
13. Não feche os navegadores. Você irá usá-los na próxima tarefa.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Tarefa 3 - atualizando uma propriedade de estilo

Nesta tarefa, você usará o Link de navegador **modo inspecionar** recurso para detectar o local exato em que o elemento DOM específico é gerado e, em seguida, atualize a propriedade de cor do elemento usando um seletor de cores fornecido pelo Web Conceitos básicos.

1. No navegador Internet Explorer, pressione **CTRL** + **ALT** + **I** para habilitar o modo de inspecionar.
2. Mova o ponteiro sobre a borda azul clara e clique em.

    ![Mover o ponteiro sobre a borda azul clara](visual-studio-2013-web-tools/_static/image14.png "mover o ponteiro sobre a borda azul clara")

    *Mover o ponteiro sobre a borda azul clara*
3. Alterne para o Visual Studio. Observe como o elemento HTML que você selecionou no navegador também é selecionado no editor de HTML do Visual Studio.

    ![Elemento HTML selecionado no editor de HTML do Visual Studio](visual-studio-2013-web-tools/_static/image15.png "elemento HTML selecionado no editor de HTML do Visual Studio")

    *Elemento HTML selecionado no editor de HTML do Visual Studio*
4. Agora você atualizará o **frontal** classe CSS para alterar o estilo do elemento selecionado. Para fazer isso, pressione **CTRL** + **,** para abrir o **navegar para** caixa de pesquisa. Tipo **site.css** e pressione **ENTER** para abrir o arquivo.

    ![Abrir arquivo Site.css](visual-studio-2013-web-tools/_static/image16.png "Site.css ao abrir arquivo")

    *Abrir o arquivo Site.css*
5. Pressione **CTRL** + **F** e tipo **.front .flip contêiner** para localizar o seletor de CSS.
6. Clique no quadrado azul claro na propriedade da classe para abrir o seletor de cor de borda.

    ![Abrir o seletor de cores](visual-studio-2013-web-tools/_static/image17.png "abrir o seletor de cores")

    *Abrir o seletor de cores*
7. Expanda o seletor de cores, clicando no botão de seta e selecione uma cor.

    ![Expandindo o seletor de cores](visual-studio-2013-web-tools/_static/image18.png "expandindo o seletor de cores")

    *Expandindo o seletor de cores*
8. Pressione **CTRL** + **ALT** + **ENTER** atualizar navegadores vinculados.
9. Alterne para o Internet Explorer e observe como a cor da borda foi alterado.

    ![Internet Explorer - atualizado de cor de borda](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer - atualizado de cor de borda")

    *Internet Explorer - atualizado de cor de borda*
10. Alterne para o Google Chrome e observe como a cor da borda foi alterado.

    ![Google Chrome - atualizado de cor de borda](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - atualizado de cor de borda")

    *Google Chrome - atualizado de cor de borda*
11. Alterne para o Visual Studio.
12. Vá até o final do **Site.css** e pressione **CTRL** + **F** para localizar o **.btn** seletor.
13. Observe que o **- webkit-border-radius** propriedade está sublinhada em verde.

    ![propriedade - webkit-border-radius do seletor btn](visual-studio-2013-web-tools/_static/image21.png "- webkit-border-radius propriedade do seletor btn")

    *propriedade - webkit-border-radius do seletor btn*
14. Coloque o cursor no **- webkit-border-radius** propriedade. Uma linha azul deve aparecer sob a primeira letra da primeira palavra da propriedade. Este é o **marca inteligente**.
15. Pressione **CTRL** + **.** Para abrir o menu de sugestões e clique em **adicionar (border-radius) de propriedade padrão ausente**.

    ![Adicionar ausente sugestão de propriedade padrão](visual-studio-2013-web-tools/_static/image22.png "adicionar ausente sugestão de propriedade padrão")

    *Adicionar ausentes sugestão de propriedade padrão*
16. O **border-radius** propriedade é adicionada automaticamente à regra de CSS.

    ![Não tem propriedade padrão adicionada](visual-studio-2013-web-tools/_static/image23.png "adicionada de propriedade padrão ausente")

    *Propriedade padrão adicionada ausente*
17. Mova o ponteiro sobre o **border-radius** propriedade para exibir o **dica de ferramenta do navegador matriz**. O **dica de ferramenta do navegador matriz** mostra a disponibilidade da propriedade em cada navegador.

    ![Dica de ferramenta do navegador matriz](visual-studio-2013-web-tools/_static/image24.png "dica de ferramenta de matriz de navegador")

    *Dica de ferramenta de matriz de navegador*
18. Observe que o valor de **border-radius** propriedade está sublinhada ainda. Mova o ponteiro sobre o valor para ver a mensagem de aviso.

    ![Aviso de valor de propriedade Border-radius](visual-studio-2013-web-tools/_static/image25.png "aviso de valor de propriedade Border-radius")

    *Aviso de valor de propriedade Border-radius*
19. Remova a unidade da **border-radius** valor da propriedade conforme sugerido pela dica de ferramenta.
20. Como **border-radius** é a propriedade padrão para a definição de borda como arredondada cantos estiverem, você pode remover o **- webkit-border-radius** propriedade e o valor da regra de CSS.
21. Coloque o cursor no **quebra** propriedade e observe que a marca inteligente também aparece abaixo.
22. Abra o menu e clique em **adicionar informações específicas de fornecedor ausente**.

    ![Adicionar ausente sugestão de informações específicas de fornecedor](visual-studio-2013-web-tools/_static/image26.png "adicionar ausente sugestão de informações específicas de fornecedor")

    *Adicionar ausente sugestão de informações específicas de fornecedor*
23. O **-ms-word-wrap** propriedade é adicionada automaticamente à regra de CSS.

    ![Propriedade específica do fornecedor adicionada](visual-studio-2013-web-tools/_static/image27.png "adicionada de propriedade específica do fornecedor")

    *Propriedade específica do fornecedor adicionada*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Tarefa 4 - atualizando o código HTML a partir do navegador

Nesta tarefa, você usará o Link de navegador **modo de Design** recurso para editar o objeto DOM do navegador e transferir as alterações no arquivo de origem HTML no Visual Studio.

1. No Google Chrome, pressione **CTRL** + **ALT** + **D** para habilitar o modo de Design.
2. Mova o ponteiro sobre o **Lorem Ipsum dolor sit amet** rótulo e clique em.

    ![Editando a pergunta](visual-studio-2013-web-tools/_static/image28.png "editando a pergunta")

    *Editar a pergunta*
3. Um cursor deve aparecer. Substitua o texto original por *o que é ele a aparência quando escrever uma pergunta mais?* e, em seguida, pressione **ESC** para sair do modo de Design.

    ![Pergunta editada](visual-studio-2013-web-tools/_static/image29.png "pergunta editada")

    *Pergunta editada*
4. Alternar para o Visual Studio e abra **cshtml**, se ainda não estiver aberto. Observe que o texto interno do **&lt;p&gt;** elemento foi atualizado.

    ![Pergunta atualizadas na página HTML](visual-studio-2013-web-tools/_static/image30.png "pergunta atualizadas na página HTML")

    *Pergunta atualizada na página HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Tarefa 5: SEO revisão relacionada avisos

**Otimização do mecanismo de pesquisa** (SEO) é o processo de criação de uma classificação de site superior na lista do mecanismo de pesquisa de resultados. Quanto maior o site classifica e mais consistentemente estiver listado, mais visitantes do site obterá desse mecanismo de pesquisa. Web Essentials incorpora uma ferramenta de análise que examina o HTML, relatórios de problemas encontrados e fornece assistência para corrigi-los.

1. Vá para o **exibição** menu e clique em **lista de erros** para abrir o **lista de erros** janela.

    ![Menu de modo de exibição de lista de erros](visual-studio-2013-web-tools/_static/image31.png "lista de erros no menu Exibir")

    *Menu de modo de exibição de lista de erros*
2. Observe que há um aviso de SEO notificando que um **&lt;meta&gt;** marca para a descrição da página está ausente. Clique duas vezes na entrada de aviso de SEO para corrigi-lo.

    ![Janela lista de erros](visual-studio-2013-web-tools/_static/image32.png "janela lista de erros")

    *Janela lista de erros*
3. No **Web Essentials** caixa de diálogo, clique em **Sim** para inserir uma descrição &lt;meta&gt; marca.

    ![Caixa de diálogo do Web Essentials](visual-studio-2013-web-tools/_static/image33.png "caixa de diálogo Web Essentials")

    *Caixa de diálogo do Web Essentials*
4. O editor de  **\_cshtml** abre e **&lt;meta&gt;** marca será adicionada automaticamente ao **head** seção o Arquivo HTML.

    ![Marca meta adicionada automaticamente na página layout](visual-studio-2013-web-tools/_static/image34.png "MetaTag adicionado automaticamente no layout página")

    *Marca meta adicionada automaticamente à \_página de Layout*
5. Alterar o valor da **conteúdo** atributo *GeekQuiz* e salve o arquivo.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Exercício 2: Aproveitando as vantagens de trechos de código e o IntelliSense

Com o Web Essentials, o editor de HTML foi estendido com funcionalidade adicional. Neste exercício, você verá alguns novos recursos que são úteis ao desenvolver aplicativos da web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Tarefa 1: usando o IntelliSense em documentos HTML

O novo recurso primeiro, você verá essa tarefa é chamado **IntelliSense dinâmico**. IntelliSense dinâmico lê outras marcas e atributos para deduzir as possíveis ids que você usará.

Nesta tarefa, você criará um novo elemento de formulário HTML que contém um rótulo e um campo de entrada. Em seguida, você adicionará um **para** de atributo para o rótulo para associá-lo para a entrada, e você verá o IntelliSense sugestões com base nas ids das entradas no escopo.

1. Abra **Visual Studio Express 2013 para Web** e **Begin.sln** solução localizada no **fonte/o Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/início** pasta. Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.
2. Em **Solution Explorer**, abra o **cshtml** arquivo localizado no **exibições** | **início** pasta.
3. Adicione o seguinte formato dentro de **&lt;seção&gt;** elemento.

    (Código de trecho - *VisualStudio2013WebTooling* - *o Ex2* - *formulário*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. A marca de entrada deve ser precedida por um rótulo com uma descrição do campo. Adicione o seguinte rótulo antes da marca de entrada.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. O **para** atributo de um **&lt;rótulo&gt;** Especifica qual elemento de formulário de um rótulo está associado. O valor do atributo deve ser igual à id do elemento relacionado. Adicionar o **para** de atributo para o **&lt;rótulo&gt;** elemento. Conforme mostrado na figura a seguir, o &quot;nome&quot; valor exibido na caixa do IntelliSense, com base na id dos elementos dentro do mesmo escopo (circunscrição  **&lt;formulário&gt;**).

    ![Mostrando a id do IntelliSense](visual-studio-2013-web-tools/_static/image35.png "mostrando a id do IntelliSense")

    *Mostrando a id do IntelliSense*
6. Excluir adicionado recentemente **&lt;formulário&gt;** elemento e seu conteúdo.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Tarefa 2: usando trechos de código HTML

O HTML5 introduziu mais de 25 novas marcas semânticas. Visual Studio já tinha suporte do IntelliSense para essas marcas, mas o Visual Studio 2013 torna mais rápido e mais fácil de escrever marcação adicionando novos trechos de código. Embora essas marcas não sejam complicadas, elas apresentam algumas pequenas sutilezas, como a adição de fallbacks de codec corretos para o *áudio* marca. Nesta tarefa, você verá os trechos de código HTML para a marca de áudio.

1. No **cshtml** de arquivos, digite  **&lt;aud** dentro de **&lt;seção&gt;** elemento conforme mostrado na figura a seguir.

    ![Inserindo um elemento audio](visual-studio-2013-web-tools/_static/image36.png "inserindo um elemento de áudio")

    *Inserindo um elemento de áudio*
2. Pressione **guia** duas vezes e observe como o código a seguir é adicionado na página e o cursor é colocado no **src** atributo da primeira fonte.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Pressionando o **guia** chave duas vezes, o trecho de código é inserido. O trecho de áudio mostra o uso padrão da *áudio* marca, com dois arquivos de origem para o suporte aprimorado.
3. Exclua a segunda linha e atualizar a fonte da primeira linha com o seguinte link para a apresentação de WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). O código resultante é mostrado abaixo.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > O arquivo de origem *KatanaProject.mp3* é usado como um exemplo. Se preferir, você pode usar outra fonte.
4. Pressione **CTRL** + **S** para salvar o arquivo.
5. Pressione **CTRL** + **F5** para iniciar o aplicativo.
6. Observe que um player de áudio foi adicionado ao aplicativo.

    ![Player de áudio no Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "player de áudio no Internet Explorer")

    *Player de áudio no Internet Explorer*

    ![Player de áudio no Google Chrome](visual-studio-2013-web-tools/_static/image38.png "player de áudio no Google Chrome")

    *Player de áudio no Google Chrome*
7. Não feche os navegadores. Você irá usá-los na próxima tarefa.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Tarefa 3: usando o IntelliSense em documentos de JavaScript

Com o Web Essentials 2013, folhas de estilo e páginas HTML produzem uma lista de IDs e nomes de classe. Nesta tarefa, você aprenderá como essas listas de melhoram o suporte a JavaScript IntelliSense na Web Essentials 2013.

1. No **cshtml** de arquivo, adicione o código a seguir para definir um **script** marca para o código JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Adicione o seguinte código dentro de **script** tag para definir a função de retorno de chamada pronto.

    (Código de trecho - *VisualStudio2013WebTooling* - *o Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Coloque o cursor no **script** marca e pressione **CTRL** + **.** Para abrir o menu de sugestão.
4. Clique em **extrair para o arquivo**.

    ![Extraia o JavaScript para sugestão de arquivo](visual-studio-2013-web-tools/_static/image39.png "JavaScript extrair a sugestão de arquivo")

    *Extraia o JavaScript para sugestão de arquivo*
5. No **Salvar como** janela, selecione o **Scripts** pasta, nomeie o arquivo **init.js** e clique em **salvar**.

    ![Salvar como janela](visual-studio-2013-web-tools/_static/image40.png "janela Salvar como")

    *Janela Salvar como*

    > [!NOTE]
    > O **init.js** arquivo é criado e o conteúdo do script é movido para o arquivo.
    > 
    > ![Arquivo init.js criado com o conteúdo incluído](visual-studio-2013-web-tools/_static/image41.png "Init.js arquivo criado com o conteúdo incluído")
    > 
    > *Arquivo init.js criado com o conteúdo incluído*
6. Abra o **cshtml** de arquivo e verifique que a marca de script foi substituída por uma referência para o **init.js** arquivo.

    ![Referência de html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js referência de html")

    *Referência de html init.js*
7. Vá para o **Solution Explorer** e observe que o **init.js** arquivo foi incluído automaticamente na solução.

    ![Arquivo init.js incluído na solução](visual-studio-2013-web-tools/_static/image43.png "Init.js arquivos incluídos na solução")

    *Arquivo init.js incluído na solução*
8. Volte para o **init.js** arquivo para atualizar o **pronto** retorno de chamada de função.
9. Dentro da definição de retorno de chamada de função que é passada para *pronto*, adicione o seguinte código para obter todos os elementos por um atributo de classe específica.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Pressione **CTRL** + **espaço** entre as aspas dentro de **getElementsByClassName** chamada de função.

    ![Mostrando o IntelliSense para a função getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "mostrando o IntelliSense para a função getElementsByClassName")

    *Mostrando o IntelliSense para a função getElementsByClassName*

    > [!NOTE]
    > Observe que o IntelliSense mostra as classes definidas nas folhas de estilo de projeto.
11. Substitua a linha que você criou com o código a seguir.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Posicione o cursor depois **Austrália** dentro de aspas no **getElementsByTagName** função e pressione **CTRL** + **espaço**.

    ![Mostrando o IntelliSense para o método getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "mostrando o IntelliSense para o método getElementByTagName")

    *Mostrando o IntelliSense para o método getElementsByTagName*
13. Selecione **&quot;áudio&quot;** na lista e pressione **ENTER**. O resultado é mostrado na figura a seguir.

    ![Recuperando elementos áudio](visual-studio-2013-web-tools/_static/image46.png "recuperar os elementos de áudio")

    *Recuperação de elementos de áudio*
14. Em **Solution Explorer**, com o botão direito a **init.js** do arquivo no **Scripts** pasta e selecione **arquivos JavaScript Minificada** da **Web Essentials** menu.

    ![Minificada arquivos JavaScript](visual-studio-2013-web-tools/_static/image47.png "arquivos JavaScript Minificada")

    *Minificada arquivos JavaScript*
15. Quando solicitado a habilitar minimização automática quando as alterações de arquivo de origem em **Sim**.

    ![Habilitar o aviso de minimização automático](visual-studio-2013-web-tools/_static/image48.png "habilitando aviso minimização automática")

    *Habilitar o aviso de minimização automática*

    > [!NOTE]
    > O **init.min.js** é criado e é adicionado como uma dependência da **init.js** arquivo.
    > 
    > ![Arquivo init.min.js criado](visual-studio-2013-web-tools/_static/image49.png "Init.min.js arquivo criado")
    > 
    > *Arquivo init.min.js criado*
16. Abra o **init.min.js** de arquivo e observe que o arquivo é minimizado.

    ![O conteúdo do arquivo init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js o conteúdo do arquivo")

    *Conteúdo do arquivo init.min.js*
17. No **init.js** arquivo, adicione o seguinte código abaixo o **getElementsByTagName** chamada para executar todos os elementos de áudio de função.

    (Código de trecho - *VisualStudio2013WebTooling* - *o Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Clique em **CTRL** + **S** para salvar o arquivo. Desde que o arquivo minimizado já está aberto, você verá uma caixa de diálogo informando que o arquivo foi modificado fora do editor de origem. Clique em **Sim**.

    ![Aviso do Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "aviso do Microsoft Visual Studio")

    *Aviso do Microsoft Visual Studio*
19. Volte para o **init.min.js** arquivo para verificar se o arquivo foi atualizado com o novo código.

    ![Arquivo init.min.js atualizado](visual-studio-2013-web-tools/_static/image52.png "Init.min.js arquivo atualizado")

    *Arquivo init.min.js atualizado*
20. Clique o **atualização de Link do navegador** botão.
21. Depois que ambos os navegadores são atualizados os players de áudio que na tarefa anterior, você viu execução serão iniciada automaticamente.

    ![Player de áudio incluído na exibição](visual-studio-2013-web-tools/_static/image53.png "incluído na exibição de player de áudio")

    *Player de áudio incluído no modo de exibição*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático você aprendeu como:

- Usar os novos recursos do editor HTML incluídos no Web Essentials como ricos trechos de código do HTML5 e Zen codificação
- Use os novos recursos de editor de CSS incluídos no Essentials Web como o seletor de cores e uma dica de ferramenta de matriz de navegador
- Usar novos recursos do editor JavaScript incluídos no Web Essentials como extrair para o arquivo e o IntelliSense para todos os elementos HTML
- Trocar dados entre o navegador e o Visual Studio usando o Link do navegador
