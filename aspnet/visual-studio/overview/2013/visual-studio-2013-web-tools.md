---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratório prático: Visual Studio 2013 Web Tools | Microsoft Docs'
author: rick-anderson
description: Visual Studio é um ambiente de desenvolvimento excelente para. Windows com base em NET e projetos da web. Ele inclui um editor de texto avançada que pode ser facilmente usado para...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 82248efd767c1110b9a4067b7d0c0e2ecafcbef9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831042"
---
<a name="hands-on-lab-visual-studio-2013-web-tools"></a>Laboratório prático: Ferramentas de Web Visual Studio 2013
====================
por [Web Camps equipe](https://twitter.com/webcamps)

[Baixe o Kit de treinamento do Web Camps](http://aka.ms/webcamps-training-kit)

> Visual Studio é um ambiente de desenvolvimento excelente para. Windows com base em NET e projetos da web. Ele inclui um editor de texto avançada que pode ser facilmente usado para editar arquivos autônomos sem um projeto.
> 
> O Visual Studio mantém uma árvore de análise completa ao editar cada arquivo. Isso permite que o Visual Studio forneça inigualável automático e as ações do documento ao fazer a experiência de desenvolvimento muito mais rápida e mais agradável. Esses recursos são particularmente poderosos em documentos HTML e CSS.
> 
> Toda essa capacidade também está disponível para extensões, tornando mais fácil estender os editores com novos recursos poderosos para atender às suas necessidades. Web Essentials é uma coleção de (principalmente) aprimoramentos relacionados à web para o Visual Studio. Ele inclui vários novos preenchimentos do IntelliSense (especialmente para CSS), novos recursos de Link do navegador, automático de arquivos JSHint para JavaScript, novos avisos para HTML e CSS e muitos outros recursos que são essenciais para o desenvolvimento web moderno.
> 
> Todo o código de exemplo e trechos de código são incluídos no Web Camps treinamento Kit, disponível em [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Visão geral

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

Neste laboratório prático, você aprenderá como:

- Use os novos recursos do editor HTML incluídos no Web Essentials, como trechos de código avançados do HTML5 e o Zen de codificação
- Use os novos recursos do editor CSS incluídos no Web Essentials, como o seletor de cor e a dica de ferramenta de matriz de navegador
- Use os novos recursos do editor JavaScript incluídos no Web Essentials, como extrair para o arquivo e o IntelliSense para todos os elementos HTML
- Trocar dados entre o navegador e o Visual Studio usando o Link do navegador

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Pré-requisitos

O exemplo a seguir é necessário para concluir este laboratório prático:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) ou maior
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Configuração

Para executar os exercícios neste laboratório prático, você precisará configurar seu ambiente pela primeira vez.

1. Abra uma janela do Windows Explorer e navegue até o laboratório **origem** pasta.
2. Clique com botão direito **Setup. cmd** e selecione **executar como administrador** para iniciar o processo de instalação que irá configurar seu ambiente e instalem os trechos de código do Visual Studio para este laboratório.
3. Se a caixa de diálogo controle de conta de usuário for mostrada, confirme a ação para continuar.

> [!NOTE]
> Verifique se que você tiver marcado todas as dependências para este laboratório antes de executar a instalação.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usando os trechos de código

Em todo o documento de laboratório, você será instruído a inserir blocos de código. Para sua conveniência, a maioria desse código é fornecido como o Visual Studio trechos de código que pode ser acessada de dentro do Visual Studio 2013 para evitar ter que adicioná-lo manualmente.

> [!NOTE]
> Cada exercício é acompanhado por uma solução inicial localizada na **começar** pasta do exercício que permite que você siga cada exercício independentemente dos outros. Esteja ciente de que os trechos de código são adicionados durante um exercício estão ausentes desses iniciando soluções e podem não funcionar até concluir o exercício. Dentro do código-fonte para um exercício, você também encontrará uma **final** pasta que contém uma solução do Visual Studio com o código que é o resultado de concluir as etapas no exercício correspondente. Você pode usar essas soluções como uma diretriz se você precisar de ajuda adicional ao trabalhar com este laboratório prático.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Exercícios

Este laboratório prático inclui os seguintes exercícios:

1. [Trabalhando com o Web Essentials e o Link do navegador](#Exercise1)
2. [Aproveitando as vantagens de trechos de código e o IntelliSense](#Exercise2)

> [!NOTE]
> Quando você inicia o Visual Studio pela primeira vez, você deve selecionar uma das coleções de configurações predefinidas. Cada coleção predefinida foi projetada para corresponder a um estilo de desenvolvimento específico e determina o comportamento do editor, layouts de janela, trechos de código IntelliSense e opções da caixa de diálogo. Os procedimentos neste laboratório descrevem as ações necessárias para realizar uma determinada tarefa no Visual Studio ao usar o **configurações gerais de desenvolvimento** coleção. Se você escolher uma coleção de configurações diferentes para seu ambiente de desenvolvimento, pode haver diferenças nas etapas que você deve levar em conta.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Exercício 1: Trabalhar com o Link do navegador e o Web Essentials

**Web Essentials** é uma extensão do Visual Studio que adiciona uma variedade de recursos úteis para desenvolvimento web moderno, principalmente concentrada em tornar a experiência de desenvolvimento da web muito mais rápida e mais agradável. Você pode instalar o Web Essentials na Galeria de extensão no Visual Studio.

**Link do navegador** é um novo recurso incluído no Visual Studio 2013 que fornece um canal entre o IDE do Visual Studio e qualquer navegador aberto para trocar dados entre seu aplicativo web e o Visual Studio. Web Essentials estende o Link do navegador com as ferramentas para manipular o modelo de objeto do DOM e os estilos CSS de suas páginas da web diretamente do navegador.

Neste exercício, você irá explorar alguns dos recursos com suporte pelo **Web Essentials** e **Link do navegador** para aprimorar uma página de teste simples.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Tarefa 1: executar o projeto em vários navegadores

Nesta tarefa, você irá configurar seu aplicativo web para ser executado em vários navegadores, ao mesmo tempo, o que é útil para testes entre navegadores.

1. Abra **Microsoft Visual Studio**.
2. No **arquivo** menu, selecione **abrir | Projeto/solução...**  e navegue até **Ex1 WorkingwithBrowserLinkandWebEssentials\Begin** no **origem** pasta do laboratório (C:\WebCampsTK\HOL\VSWebTooling\Source). Selecione **Begin.sln** e clique em **abrir**.
3. Na barra de ferramentas do Visual Studio, expanda o menu de navegador e selecione **procurar com...** .

    ![Opção de menu procurar com](visual-studio-2013-web-tools/_static/image1.png "procurar com... no menu do navegador")

    *Opção de menu procurar com*
4. No **procurar com** caixa de diálogo, selecione **Google Chrome** e **Internet Explorer** segurando o **CTRL** da chave e clique em  **Definir como padrão**.

    ![Procurar com a caixa de diálogo](visual-studio-2013-web-tools/_static/image2.png "procurar com a caixa de diálogo")

    *Selecionar vários navegadores padrão*
5. Google Chrome e o Internet Explorer agora devem aparecer como os navegadores padrão. Clique em **Cancelar** para fechar a caixa de diálogo.

    ![Google Chrome e o Internet Explorer como navegadores padrão](visual-studio-2013-web-tools/_static/image3.png "navegadores de padrão do Internet Explorer e Google Chrome")

    *Google Chrome e o Internet Explorer como navegadores padrão*

    > [!NOTE]
    > Depois de configurar os navegadores padrão, o **vários navegadores** opção é selecionada no menu do navegador.
    > 
    > ![Vários navegadores](visual-studio-2013-web-tools/_static/image4.png "vários navegadores")
6. Pressione **CTRL** + **F5** para executar o aplicativo sem depuração.
7. Quando ambas as janelas do navegador se abre, coloque um deles acima do outro para ver as atualizações em ambos os navegadores simultaneamente. Os navegadores devem exibir uma página da web com um retângulo azul claro.

    ![Colocando um navegador acima do outro](visual-studio-2013-web-tools/_static/image5.png "colocando um navegador acima do outro")

    *Colocando um navegador acima do outro*
8. Não feche os navegadores. Você irá usá-los na próxima tarefa.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Tarefa 2 - Zen usando codificação para criar elementos HTML

**Codificação Zen** é um editor de codificação de plug-in para alta velocidade HTML, XML, XSL (ou qualquer outro formato de código estruturado) e de edição. O núcleo desse plug-in é um mecanismo de abreviação poderosa que permite que você expanda expressões - semelhantes para seletores de CSS - no código HTML. Codificação de Zen é uma maneira rápida de escrever a sintaxe do seletor de estilo HTML usando CSS.

Neste exercício, você usará o recurso Zen codificação fornecido pelo Web Essentials para gerar os botões HTML que representam as opções da pergunta.

1. Alterne para Visual Studio.
2. Abra o **index. cshtml** arquivo localizado na **exibições** | **Home** pasta.
3. Substitua os **&lt;! – TODO: Adicione aqui – opções&gt;** comentário com o seguinte código e pressione **guia**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. O código deve ser expandido para HTML.

    ![Expandido HTML](visual-studio-2013-web-tools/_static/image6.png "expandido HTML")

    *HTML expandido*

    > [!NOTE]
    > Para saber mais sobre Zen codificação sintaxe, consulte o seguinte [artigo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/).
5. Clique o **atualizar navegadores vinculados** botão atualizar ambos os navegadores.

    ![Atualizar navegadores vinculados](visual-studio-2013-web-tools/_static/image7.png "atualizar navegadores vinculados")

    *Atualizar navegadores vinculados*

    ![Internet Explorer - página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer - página atualizada com quatro botões")

    *Internet Explorer - página atualizada com quatro botões*

    ![Google Chrome - página atualizada com quatro botões](visual-studio-2013-web-tools/_static/image9.png "Google Chrome - página atualizada com quatro botões")

    *Google Chrome - página atualizada com quatro botões*
6. Alterne para Visual Studio.
7. Você adicionou os botões para a página, mas você ainda precisa adicionar uma pergunta de modelo. Para fazer isso, você usará um novo recurso na chamada do Web Essentials **Lorem Ipsum gerador**. Localize o **div** elemento com o **classe** atributo **front**.
8. Adicione o seguinte código como o primeiro elemento filho do **div**e pressione **guia**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. O código deve ser expandido para HTML.

    ![Gerado automaticamente do Lorem Ipsum](visual-studio-2013-web-tools/_static/image10.png "gerado automaticamente do Lorem Ipsum")

    *Gerado automaticamente do Lorem Ipsum*

    > [!NOTE]
    > Como parte da codificação Zen, agora você pode gerar o código do Lorem Ipsum diretamente no editor de HTML. Basta digitar **lorem** e clique em **guia** e um 30 word Lorem Ipsum o texto será inserido. Por exemplo, *lorem10* insere 10 palavras Lorem Ipsum.
10. Você irá adicionar um logotipo na parte superior da pergunta usando outro novo recurso na chamada do Web Essentials **gerador de Lorem Pixel**. Adicione o seguinte código como o primeiro elemento filho do **div** elemento com **contêiner** como **classe** valor e pressione **guia**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. O código deve se expandir para HTML.

    ![Gerado automaticamente de Lorem Pixel](visual-studio-2013-web-tools/_static/image11.png "gerado automaticamente de Lorem Pixel")

    *Gerado automaticamente de Lorem Pixel*

    > [!NOTE]
    > Como parte do Zen de codificação, você também pode gerar o código de Lorem Pixel diretamente no editor de HTML. Basta digitar **pix de 200 x 200 de animais** e clique em **guia** e um **img** marca com uma imagem de 200 x 200 de um animal será inserida. Para obter mais informações, consulte [Lorem Pixel](http://www.lorempixel.com).
12. Clique o **atualizar navegadores vinculados** botão atualizar ambos os navegadores.

    ![Internet Explorer - imagem gerada automaticamente e o texto](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer - imagem gerada automaticamente e texto")

    *Internet Explorer - imagem gerada automaticamente e texto*

    ![Google Chrome - imagem gerada automaticamente e o texto](visual-studio-2013-web-tools/_static/image13.png "Google Chrome - imagem gerada automaticamente e texto")

    *Google Chrome - imagem gerada automaticamente e texto*

    > [!NOTE]
    > Como a imagem é selecionada aleatoriamente ao adicionar o trecho de código, a imagem mostrada nos navegadores pode diferir.
13. Não feche os navegadores. Você irá usá-los na próxima tarefa.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Tarefa 3: atualizar uma propriedade de estilo

Nesta tarefa, você usará o Link do navegador **inspecionar modo** recurso para detectar o local exato em que o elemento DOM específico é gerado e, em seguida, atualize a propriedade de cor do elemento usando um seletor de cor fornecido pela Web Conceitos básicos.

1. No navegador Internet Explorer, pressione **CTRL** + **ALT** + **eu** para habilitar o modo de inspecionar.
2. Mova o ponteiro sobre a borda azul claro e clique em.

    ![Mover o ponteiro sobre a borda azul claro](visual-studio-2013-web-tools/_static/image14.png "movendo o ponteiro sobre a borda azul claro")

    *Mover o ponteiro sobre a borda azul claro*
3. Alterne para Visual Studio. Observe como o elemento HTML que você selecionou no navegador também está selecionado no editor de HTML do Visual Studio.

    ![Elemento HTML selecionado no editor de HTML do Visual Studio](visual-studio-2013-web-tools/_static/image15.png "elemento HTML selecionado no editor de HTML do Visual Studio")

    *Elemento HTML selecionado no editor de HTML do Visual Studio*
4. Agora você irá atualizar o **front** classe CSS para alterar o estilo do elemento selecionado. Para fazer isso, pressione **CTRL** + **,** para abrir o **navegar até** caixa de pesquisa. Tipo de **CSS** e pressione **ENTER** para abrir o arquivo.

    ![Abrir o arquivo CSS](visual-studio-2013-web-tools/_static/image16.png "abrir o arquivo CSS")

    *Abrir o arquivo CSS*
5. Pressione **CTRL** + **F** e digite **.front .flip contêiner** para localizar o seletor de CSS.
6. Clique no quadrado azul claro na propriedade da classe para abrir o seletor de cores da borda.

    ![Abrir o seletor de cores](visual-studio-2013-web-tools/_static/image17.png "abrindo o seletor de cores")

    *Abrir o seletor de cores*
7. Expanda o seletor de cores, clique no botão de divisa e selecione uma nova cor.

    ![Expandindo o seletor de cores](visual-studio-2013-web-tools/_static/image18.png "expandindo o seletor de cores")

    *Expandindo o seletor de cores*
8. Pressione **CTRL** + **ALT** + **ENTER** atualizar navegadores vinculados.
9. Alterne para o Internet Explorer e observe como a cor da borda foi alterada.

    ![Internet Explorer – a cor da borda atualizada](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer – a cor da borda atualizado")

    *Internet Explorer – a cor da borda atualizado*
10. Alterne para o Google Chrome e observe como a cor da borda foi alterada.

    ![Google Chrome - atualizado de cor de borda](visual-studio-2013-web-tools/_static/image20.png "Google Chrome - atualizado de cor de borda")

    *Google Chrome - atualizado de cor de borda*
11. Alterne para Visual Studio.
12. Vá até o final do **CSS** arquivo e pressione **CTRL** + **F** para localizar o **.btn** seletor.
13. Observe que o **- webkit-border-radius** propriedade está sublinhada em verde.

    ![propriedade - webkit-border-radius do seletor de btn](visual-studio-2013-web-tools/_static/image21.png "propriedade - webkit-border-radius do seletor de btn")

    *propriedade - webkit-border-radius do seletor de btn*
14. Coloque o cursor na **- webkit-border-radius** propriedade. Uma linha azul deve aparecer sob a primeira letra da primeira palavra da propriedade. Esse é o **marca inteligente**.
15. Pressione **CTRL** + **.** Para abrir o menu de sugestões e clique em **adicionar a propriedade padrão (raio de borda) ausente**.

    ![Adicionar ausente sugestão de propriedade padrão](visual-studio-2013-web-tools/_static/image22.png "adicionar ausente sugestão de propriedade padrão")

    *Adicionar ausentes sugestão de propriedade padrão*
16. O **raio de borda** propriedade é adicionada automaticamente à regra de CSS.

    ![Adicionada a propriedade padrão ausente](visual-studio-2013-web-tools/_static/image23.png "adicionada a propriedade padrão ausente")

    *Adicionada a propriedade padrão ausente*
17. Mova o ponteiro sobre o **raio de borda** propriedade para exibir o **dica de ferramenta do navegador matriz**. O **dica de ferramenta do navegador matriz** mostra a disponibilidade da propriedade em cada navegador.

    ![Dica de ferramenta do navegador matrix](visual-studio-2013-web-tools/_static/image24.png "dica de ferramenta de matriz de navegador")

    *Dica de ferramenta de matriz de navegador*
18. Observe que o valor de **raio de borda** propriedade está sublinhada ainda. Mova o ponteiro sobre o valor para ver a mensagem de aviso.

    ![Aviso de valor de propriedade de raio de borda](visual-studio-2013-web-tools/_static/image25.png "aviso de valor de propriedade de raio de borda")

    *Aviso de valor de propriedade de raio de borda*
19. Remova a unidade do **raio de borda** valor da propriedade conforme sugerido pela dica de ferramenta.
20. Como **raio de borda** é a propriedade padrão para a definição de borda como arredondada cantos são, você pode remover o **- webkit-border-radius** propriedade e o valor da regra de CSS.
21. Coloque o cursor na **quebra** propriedade e observe que a marca inteligente também é exibido abaixo.
22. Abra o menu e clique em **adicionar informações específicas de fornecedor ausente**.

    ![Adicione ausente sugestão de informações específicas de fornecedor](visual-studio-2013-web-tools/_static/image26.png "adicione ausente sugestão de informações específicas de fornecedor")

    *Adicione ausente sugestão de informações específicas de fornecedor*
23. O **ms quebra** propriedade é adicionada automaticamente à regra de CSS.

    ![Propriedade específica do fornecedor adicionada](visual-studio-2013-web-tools/_static/image27.png "adicionada de propriedade específica do fornecedor")

    *Propriedade específica do fornecedor adicionada*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Tarefa 4 - atualizando o código HTML no navegador

Nesta tarefa, você usará o Link do navegador **modo de Design** recurso para editar o objeto do DOM do navegador e transferir as alterações para o arquivo de código-fonte HTML no Visual Studio.

1. No Google Chrome, pressione **CTRL** + **ALT** + **1!d** para habilitar o modo de Design.
2. Mova o ponteiro sobre o **Lorem Ipsum dolor sit amet** de rótulo e clique em.

    ![Editando a pergunta](visual-studio-2013-web-tools/_static/image28.png "editando a pergunta")

    *Editando a pergunta*
3. Um cursor deve aparecer. Substitua o texto original com *o que ele se parece, como quando escrevo uma pergunta mais longa?* e, em seguida, pressione **ESC** para sair do modo de Design.

    ![Pergunta editada](visual-studio-2013-web-tools/_static/image29.png "pergunta editada")

    *Pergunta editada*
4. Alternar para o Visual Studio e abra **index. cshtml**, se ainda não estiver aberto. Observe que o texto interno do **&lt;p&gt;** elemento foi atualizado.

    ![Pergunta atualizadas na página HTML](visual-studio-2013-web-tools/_static/image30.png "pergunta atualizadas na página HTML")

    *Pergunta atualizada na página HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Tarefa 5 – revisão SEO relacionados avisos

**Otimização do mecanismo de pesquisa** (SEO) é o processo de criação de uma classificação de site mais alto na lista do mecanismo de pesquisa de resultados. Quanto maior o site classifica e mais consistentemente estiver listado, os visitantes mais o site obterá-se de que o mecanismo de pesquisa. Web Essentials incorpora uma ferramenta analítica que examina o HTML, relatórios de problemas encontrados e fornece assistência para corrigi-los.

1. Vá para o **modo de exibição** menu e clique em **lista de erros** para abrir o **lista de erros** janela.

    ![Menu de modo de exibição de lista de erros](visual-studio-2013-web-tools/_static/image31.png "lista de erros no menu Exibir")

    *Menu de modo de exibição de lista de erros*
2. Observe que há um aviso de SEO notificando que uma **&lt;meta&gt;** de marca para a descrição da página está ausente. Clique duas vezes na entrada de aviso de SEO para corrigi-lo.

    ![Janela lista de erros](visual-studio-2013-web-tools/_static/image32.png "janela lista de erros")

    *Janela Lista de Erros*
3. No **Web Essentials** caixa de diálogo, clique em **Yes** para inserir uma descrição &lt;meta&gt; marca.

    ![Caixa de diálogo do Web Essentials](visual-studio-2013-web-tools/_static/image33.png "caixa de diálogo do Web Essentials")

    *Caixa de diálogo do Web Essentials*
4. O editor para  **\_layout. cshtml** abre e a **&lt;meta&gt;** marca é adicionada automaticamente para o **head** seção o Arquivo HTML.

    ![Marca meta adicionado automaticamente à página layout](visual-studio-2013-web-tools/_static/image34.png "MetaTag adicionado automaticamente à página layout")

    *Marca meta adicionada automaticamente ao \_página de Layout*
5. Altere o valor da **conteúdo** atributo *GeekQuiz* e salve o arquivo.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Exercício 2: Aproveitando os trechos de código e IntelliSense

Com o Web Essentials, o editor de HTML foi estendido com funcionalidade adicional. Neste exercício, você verá alguns novos recursos que são úteis ao desenvolver aplicativos da web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Tarefa 1: usando o IntelliSense em documentos HTML

O primeiro recurso novo, você verá essa tarefa é chamado **IntelliSense dinâmico**. IntelliSense dinâmico lê outras marcas e atributos para inferir as possíveis ids que você usará.

Nesta tarefa, você criará um novo elemento de formulário HTML que contém um rótulo e um campo de entrada. Em seguida, você adicionará uma **para** de atributo para o rótulo para associá-lo à entrada, e você verá sugestões do IntelliSense com base nas ids das entradas no escopo.

1. Abra **Visual Studio Express 2013 para Web** e o **Begin.sln** solução localizada no **origem/o Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/início** pasta. Como alternativa, você pode continuar com a solução que você obteve no exercício anterior.
2. Na **Gerenciador de soluções**, abra o **index. cshtml** arquivo localizado no **modos de exibição** | **início** pasta.
3. Adicione o seguinte formulário dentro do **&lt;seção&gt;** elemento.

    (Código de trecho de código – *VisualStudio2013WebTooling* - *o Ex2* - *formulário*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. A marca de entrada deve ser precedida por um rótulo com uma descrição do campo. Adicione o seguinte rótulo antes da marca de entrada.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. O **para** atributo de uma **&lt;rótulo&gt;** Especifica qual elemento de formulário um rótulo está associado. O valor do atributo deve ser igual à id do elemento relacionado. Adicione a **para** atributo para o **&lt;rótulo&gt;** elemento. Conforme mostrado na figura a seguir, o &quot;nome&quot; valor aparece na caixa de IntelliSense, com base na id dos elementos dentro do mesmo escopo (circunscrição  **&lt;formulário&gt;**).

    ![Mostrando a id no IntelliSense](visual-studio-2013-web-tools/_static/image35.png "mostrando a id no IntelliSense")

    *Mostrando a id no IntelliSense*
6. Excluir recentemente adicionados **&lt;formulário&gt;** elemento e seu conteúdo.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Tarefa 2 - usando trechos de código HTML

O HTML5 introduziu mais de 25 novas marcas semânticas. Visual Studio já tinha suporte do IntelliSense para essas marcas, mas o Visual Studio 2013 tornam mais rápidos e mais fáceis de escrever a marcação com a adição de novos trechos de código. Embora essas marcas não sejam complicadas, elas vêm com algumas pequenas sutilezas, como a adição de fallbacks de codec corretos para o *áudio* marca. Nesta tarefa, você verá os trechos de código HTML para a marca de áudio.

1. No **index. cshtml** do arquivo, digite  **&lt;aud** dentro a **&lt;seção&gt;** elemento, conforme mostrado na figura a seguir.

    ![Inserção de um elemento de áudio](visual-studio-2013-web-tools/_static/image36.png "inserção de um elemento de áudio")

    *Inserção de um elemento de áudio*
2. Pressione **guia** duas vezes e observe como o código a seguir é adicionado na página e o cursor é colocado sobre o **src** atributo da primeira fonte.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Ao pressionar o **guia** chave duas vezes, o trecho de código é inserido. O trecho de áudio mostra o uso padrão do *áudio* marca, com dois arquivos de origem para o suporte aprimorado.
3. Excluir a segunda linha e atualizar a fonte da primeira linha com o link a seguir para mostrar o WebCampsTV Katana: [ http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3 ](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). O código resultante é mostrado abaixo.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > O arquivo de origem *KatanaProject.mp3* é usado como um exemplo. Se você preferir, você pode usar outra fonte.
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

Com o Web Essentials 2013, folhas de estilo e páginas HTML produzem uma lista de IDs e nomes de classe. Nesta tarefa, você aprenderá como essas listas de melhoram o suporte ao IntelliSense de JavaScript no Web Essentials 2013.

1. No **index. cshtml** do arquivo, adicione o código a seguir para definir um **script** marca para o código JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Adicione o seguinte código dentro de **script** marca para definir a função de retorno de chamada pronto.

    (Código de trecho de código – *VisualStudio2013WebTooling* - *o Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Coloque o cursor na **script** marca e pressione **CTRL** + **.** Para abrir o menu de sugestão.
4. Clique em **extrair arquivo**.

    ![Extraia o JavaScript para sugestão de arquivo](visual-studio-2013-web-tools/_static/image39.png "JavaScript extrair a sugestão de arquivo")

    *Extraia o JavaScript para sugestão de arquivo*
5. No **Salvar como** janela, selecione a **Scripts** pasta, nomeie o arquivo **init.js** e clique em **salvar**.

    ![Janela Salvar como](visual-studio-2013-web-tools/_static/image40.png "janela Salvar como")

    *Janela Salvar como*

    > [!NOTE]
    > O **init.js** arquivo é criado e o conteúdo do script é movido para o arquivo.
    > 
    > ![Arquivo init.js criado com o conteúdo incluído](visual-studio-2013-web-tools/_static/image41.png "arquivo Init.js criado com o conteúdo incluído")
    > 
    > *Arquivo init.js criado com o conteúdo incluído*
6. Abra o **index. cshtml** do arquivo e verifique que a marca de script foi substituída por uma referência para o **init.js** arquivo.

    ![Referência de html init.js](visual-studio-2013-web-tools/_static/image42.png "Init.js referência de html")

    *Referência de html init.js*
7. Vá para o **Gerenciador de soluções** e observe que o **init.js** arquivo foi incluído automaticamente na solução.

    ![Arquivo init.js incluído na solução](visual-studio-2013-web-tools/_static/image43.png "arquivo Init.js incluído na solução")

    *Arquivo init.js incluído na solução*
8. Volte para o **init.js** arquivo para atualizar o **pronto** retorno de chamada de função.
9. Dentro da definição de retorno de chamada de função que é passada para *pronto*, adicione o seguinte código para obter todos os elementos por um atributo de classe específica.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Pressione **CTRL** + **espaço** entre aspas dentro do **getElementsByClassName** chamada de função.

    ![Mostrando o IntelliSense para a função getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "mostrando o IntelliSense para a função getElementsByClassName")

    *Mostrando o IntelliSense para a função getElementsByClassName*

    > [!NOTE]
    > Observe que o IntelliSense mostra as classes definidas as folhas de estilo do projeto.
11. Substitua a linha que você criou com o código a seguir.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Posicione o cursor após **au** dentro de aspas na **getElementsByTagName** função e pressione **CTRL** + **espaço**.

    ![Mostrando o IntelliSense para o método getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "mostrando o IntelliSense para o método getElementByTagName")

    *Mostrando o IntelliSense para o método getElementsByTagName*
13. Selecione **&quot;áudio&quot;** na lista e pressione **ENTER**. O resultado é mostrado na figura a seguir.

    ![Recuperando elementos áudio](visual-studio-2013-web-tools/_static/image46.png "recuperando elementos de áudio")

    *Recuperação de elementos de áudio*
14. Na **Gerenciador de soluções**, com o botão direito a **init.js** de arquivo no **Scripts** pasta e selecione **JavaScript Minificar arquivos** do **Web Essentials** menu.

    ![Reduzir arquivo (s) de JavaScript](visual-studio-2013-web-tools/_static/image47.png "arquivos JavaScript Minificar")

    *Reduzir arquivo (s) de JavaScript*
15. Quando solicitado para habilitar a minificação automática quando as alterações de arquivo de origem clica **Sim**.

    ![Habilitar o aviso automático de minimização](visual-studio-2013-web-tools/_static/image48.png "habilitando aviso automático de minimização")

    *Habilitar o aviso automático de minimização*

    > [!NOTE]
    > O **init.min.js** é criado e adicionado como uma dependência da **init.js** arquivo.
    > 
    > ![Arquivo init.min.js criado](visual-studio-2013-web-tools/_static/image49.png "Init.min.js arquivo criado")
    > 
    > *Arquivo init.min.js criado*
16. Abra o **init.min.js** de arquivo e observe que o arquivo é minimizado.

    ![Conteúdo do arquivo init.min.js](visual-studio-2013-web-tools/_static/image50.png "Init.min.js o conteúdo do arquivo")

    *Conteúdo do arquivo init.min.js*
17. No **init.js** do arquivo, adicione o código a seguir abaixo de **getElementsByTagName** chamada de função para reproduzir todos os elementos de áudio.

    (Código de trecho de código – *VisualStudio2013WebTooling* - *o Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Clique em **CTRL** + **S** para salvar o arquivo. Uma vez que o arquivo reduzido já está aberto, você verá uma caixa de diálogo informando que o arquivo foi modificado fora do editor de código-fonte. Clique em **Sim**.

    ![Aviso do Microsoft Visual Studio](visual-studio-2013-web-tools/_static/image51.png "aviso do Microsoft Visual Studio")

    *Aviso do Microsoft Visual Studio*
19. Volte para o **init.min.js** arquivo para verificar que o arquivo foi atualizado com o novo código.

    ![Arquivo init.min.js atualizado](visual-studio-2013-web-tools/_static/image52.png "Init.min.js arquivo atualizado")

    *Arquivo init.min.js atualizado*
20. Clique o **Atualizar Link do navegador** botão.
21. Depois que ambos os navegadores são atualizados os players de áudio que você viu na tarefa anterior começará a ser reproduzido automaticamente.

    ![Player de áudio incluído na exibição](visual-studio-2013-web-tools/_static/image53.png "incluído na exibição de player de áudio")

    *Player de áudio incluído na exibição*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumo

Ao concluir este laboratório prático que você aprendeu como:

- Use os novos recursos do editor HTML incluídos no Web Essentials, como trechos de código avançados do HTML5 e o Zen de codificação
- Use os novos recursos do editor CSS incluídos no Web Essentials, como o seletor de cor e a dica de ferramenta de matriz de navegador
- Use os novos recursos do editor JavaScript incluídos no Web Essentials, como extrair para o arquivo e o IntelliSense para todos os elementos HTML
- Trocar dados entre o navegador e o Visual Studio usando o Link do navegador
