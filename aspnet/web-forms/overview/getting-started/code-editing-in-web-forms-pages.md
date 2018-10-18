---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Edição de código Web Forms do ASP.NET no Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 670f81ca1ef9923575cb2fee1747f06f426963d8
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391213"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Código de edição Web Forms do ASP.NET no Visual Studio 2013
====================
por [Erik Reitan](https://github.com/Erikre)

Em muitas páginas de Web Form do ASP.NET, você escreve código em Visual Basic, c# ou outra linguagem. O editor de código no Visual Studio pode ajudá-lo a escrever código rapidamente, enquanto ajuda a evitar erros. Além disso, o editor fornece maneiras para você criar códigos reutilizáveis para ajudar a reduzir a quantidade de trabalho que você precisa fazer.

Este passo a passo ilustra vários recursos do editor de código do Visual Studio.

Durante este passo a passo, você aprenderá a:

- Corrija os erros de codificação embutida.
- Refatorar e renomear o código.
- Renomear variáveis e objetos.
- Inserir trechos de código.

## <a name="prerequisites"></a>Pré-requisitos


Para concluir este passo a passo, você precisará de:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). O .NET Framework é instalado automaticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 para Web serão ser conhecido como Visual Studio em toda esta série de tutoriais.  
    >   
    > Se você estiver usando o Visual Studio, este passo a passo pressupõe que você selecionou a **desenvolvimento Web** coleção de configurações na primeira vez que você iniciou o Visual Studio. Para obter mais informações, consulte [como: selecionar configurações de ambiente de desenvolvimento da Web](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Criando um projeto de aplicativo Web e uma página

<a id="sectionToggle0"></a>

Nesta parte do passo a passo, você criará um projeto de aplicativo Web e adicionar uma nova página a ele.

### <a name="to-create-a-web-application-project"></a>Para criar um projeto de aplicativo Web

1. Abra o Microsoft Visual Studio.
2. No menu **Arquivo**, selecione **Novo Projeto**.  
    ![Menu arquivo](code-editing-in-web-forms-pages/_static/image1.png)

    A caixa de diálogo **Novo Projeto** é exibida.
3. Selecione o **modelos**  - &gt; **Visual c#**  - &gt; **Web** grupo de modelos à esquerda.
4. Escolha o **aplicativo Web ASP.NET** modelo na coluna central.
5. Nomeie o projeto ***BasicWebApp*** e clique no **Okey** botão.   
![Caixa de diálogo Novo projeto](code-editing-in-web-forms-pages/_static/image2.png)
6. Em seguida, selecione a **Web Forms** modelo e clique no **Okey** botão para criar o projeto.  
![Caixa de diálogo Novo projeto ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio cria um novo projeto que inclui a funcionalidade predefinidas com base no modelo de formulários da Web.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Criando uma nova página Web Forms do ASP.NET


Quando você cria um novo aplicativo do Web Forms usando o **aplicativo Web ASP.NET** modelo de projeto, o Visual Studio adiciona uma página do ASP.NET (página de Web Forms) chamada *default. aspx*, bem como vários outros arquivos e pastas. Você pode usar o *default. aspx* página como a home page do seu aplicativo Web. No entanto, para este passo a passo, você irá criar e trabalhar com uma nova página.

### <a name="to-add-a-page-to-the-web-application"></a>Para adicionar uma página ao aplicativo Web


1. Na **Gerenciador de soluções**, clique no nome do aplicativo Web (neste tutorial é o nome do aplicativo **BasicWebSite**) e, em seguida, clique em **Add**  - &gt; **Novo Item**.   
A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o **Visual c#**  - &gt; **Web** grupo de modelos à esquerda. Em seguida, selecione **Web Form** do meio lista e nomeie-o *FirstWebPage*.   
    ![Adicionar caixa de diálogo Novo Item](code-editing-in-web-forms-pages/_static/image4.png)
3. Clique em **adicionar** para adicionar a página de Web Forms ao seu projeto.  
 Visual Studio cria a nova página e ele é aberto.
4. Em seguida, defina essa nova página como a página de inicialização padrão. Na **Gerenciador de soluções**, clique com botão direito a nova página denominada *FirstWebPage* e selecione **definir como página inicial**. Na próxima vez que você executar esse aplicativo para testar nosso progresso, você será automaticamente consulte essa nova página no navegador.


## <a name="correcting-inline-coding-errors"></a>Corrigindo embutido erros de codificação


O editor de código no Visual Studio ajuda você a evitar erros conforme você escreve o código e, se você tiver cometido um erro, o editor de código ajuda você a corrigir o erro. Nesta parte do passo a passo, você irá escrever uma linha de código que ilustram os recursos de correção de erro no editor.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Para corrigir erros de codificação simples no Visual Studio


1. Na **Design** exibir, clique duas vezes na página em branco para criar um manipulador para o **carga** evento para a página.   
   Você está usando o manipulador de eventos somente como um lugar para escrever um código.
2. Dentro do manipulador, digite a seguinte linha que contém um erro e pressione **ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Quando você pressiona **ENTER**, o editor de códigos coloca sublinhados verdes e vermelhos (normalmente chamam &quot;ondulada&quot; linhas) em áreas do código que apresentam problemas. Um sublinhado verde indica um aviso. Um sublinhado vermelho indica um erro que você precisa corrigir. 

    Mantenha o ponteiro do mouse sobre `myStr` para ver uma dica de ferramenta que informa você sobre o aviso. Além disso, mantenha o ponteiro do mouse sobre o sublinhado vermelho para ver a mensagem de erro.

    A imagem a seguir mostra o código com os sublinhados.

    ![Texto de boas vindas no modo de exibição de Design](code-editing-in-web-forms-pages/_static/image5.png "texto de boas vindas no modo de exibição de Design")  
   O erro deve ser corrigido pela adição de um ponto e vírgula `;` até o final da linha. O aviso simplesmente notifica que você não usou o `myStr` variável ainda.  

    > [!NOTE] 
    > 
    > Visualizar seu código atual formatação configurações no Visual Studio, selecionando **ferramentas**  - &gt; **opções**  - &gt; **fontes e Cores**.


## <a name="refactoring-and-renaming"></a>Refatoração e renomear

Refatoração é uma metodologia de software que envolve a reestruturação do código para torná-lo mais fácil de entender e manter, preservando sua funcionalidade. Um exemplo simples pode ser que você escreva código em um manipulador de eventos para obter dados de um banco de dados. À medida que você desenvolve sua página, você descobre que precisa acessar os dados de vários manipuladores diferentes. Portanto, você refatora o código da página Criando um método de acesso a dados na página e inserindo chamadas para o método nos manipuladores.

O editor de código inclui ferramentas para ajudá-lo a realizar várias tarefas de refatoração. Neste passo a passo, você trabalhará com duas técnicas de refatoração: Renomeando variáveis e métodos de extração. Outras opções de refatoração incluem encapsular campos, promovendo as variáveis locais para parâmetros de método e gerenciar os parâmetros de método. A disponibilidade dessas opções de refatoração depende do local no código.

### <a name="refactoring-code"></a>Código de refatoração

Um cenário de refatoração comum é criar (Extrair) um método do código que está dentro de outro membro, como um método. Isso reduz o tamanho do membro original e torna o código extraído reutilizável.

Nesta parte do passo a passo, você escrever um código simples e, em seguida, extrair um método dele. Refatoração é compatível com c#, portanto, você criará uma página que usa c# como sua linguagem de programação.

### <a name="to-extract-a-method-in-a-c-page"></a>Para extrair um método em uma página do c#

1. Alterne para **Design** modo de exibição.
2. No **caixa de ferramentas**, da **padrão** guia, arraste uma [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) até a página.
3. Clique duas vezes o **botão** controle para criar um manipulador para seu [clique](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento e, em seguida, adicione o seguinte código realçado:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   O código cria um **ArrayList** objeto, usa um loop para carregá-lo com valores e, em seguida, usa outro loop para exibir o conteúdo da **ArrayList** objeto.
4. Pressione **CTRL+F5** para executar a página e, em seguida, clique no **botão** para certificar-se de que você vê a seguinte saída:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Retornar ao editor de códigos e, em seguida, selecione as linhas a seguir no manipulador de eventos.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. A seleção com o botão direito, clique em **refatorar**e, em seguida, escolha **extrair método**. 

    O **extrair método** caixa de diálogo é exibida.
7. No **nome do novo método** , digite **DisplayArray**e, em seguida, clique em **Okey**. 

    O editor de código cria um novo método chamado `DisplayArray`e coloca uma chamada para o novo método em de **clique em** manipulador em que o loop foi originalmente.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Pressione **CTRL+F5** para executar a página novamente e, em seguida, clique no **botão**.

    A página funciona da mesma como fazia antes. O `DisplayArray` método agora pode ser chamada em qualquer lugar na classe de página.

## <a name="renaming-variables"></a>Renomeando variáveis

Quando você trabalha com variáveis, bem como objetos, você talvez queira renomeá-los depois que eles já estão referenciados em seu código. No entanto, a renomeação de variáveis e objetos pode causar quebra se você esquecer de renomear uma das referências do código. Portanto, você pode usar a refatoração para executar a renomeação.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Para usar a refatoração para renomear uma variável


1. No **clique** manipulador de eventos, localize a seguinte linha:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. O nome da variável com o botão direito `alist`, escolha **refatorar**e, em seguida, escolha **Renomear**.

    O **Renomear** caixa de diálogo é exibida.
3. No **novo nome** , digite **ArrayList1** e verifique se o **visualizar alterações de referência** caixa de seleção tiver sido selecionada. Clique em **OK**.

    O **visualizar alterações** caixa de diálogo aparece e exibe uma árvore que contém todas as referências para a variável que você está renomeando.
4. Clique em **Apply** para fechar o **visualizar alterações** caixa de diálogo.

    As variáveis que se refiram especificamente a instância que você selecionou são renomeadas. No entanto, observe que a variável `alist` na linha a seguir não é renomeada.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    A variável `alist` essa linha não é renomeada porque ele não representa o mesmo valor que a variável `alist` que você renomeou. A variável `alist` no `DisplayArray` declaração é uma variável local para o método. Isso ilustra que, usando a refatoração para renomear variáveis é diferente de simplesmente executar uma ação de localizar e substituir no editor; Refatoração renomeia variáveis com conhecimento da semântica da variável que ele está funcionando com.


## <a name="inserting-snippets"></a>Inserir trechos de código

Como há muitas tarefas de codificação que os desenvolvedores de Web Forms com frequência precisam executar, o editor de código fornece uma biblioteca de trechos de código, ou blocos de código pré-escritos. Você pode inserir esses trechos de código em sua página.

Cada linguagem que você usa no Visual Studio tem pequenas diferenças na maneira como inserir trechos de código. Para obter informações sobre como inserir trechos de código, consulte [trechos de código do Visual Basic IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx). Para obter informações sobre como inserir trechos de código no Visual c#, consulte [trechos de código do Visual c#](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Próximas etapas

Este passo a passo ilustra os recursos básicos do editor de código do Visual Studio 2010 para corrigir erros em seu código, refatoração de código, renomeando variáveis e inserindo trechos de código em seu código. Recursos adicionais no editor podem tornar o desenvolvimento de aplicativo rápido e fácil. Por exemplo, é possível:

- Saiba mais sobre os recursos do IntelliSense, como modificar opções do IntelliSense, gerenciar trechos de código e pesquisando trechos de código online. Para obter mais informações, veja [Usando o IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Aprenda a criar seus próprios trechos de código. Para obter mais informações, consulte [criando e usando trechos de código do IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- Saiba mais sobre os recursos específicos do Visual Basic de trechos de código do IntelliSense, como personalizar os trechos de código e solução de problemas. Para obter mais informações, consulte [trechos de código do Visual Basic IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Saiba mais sobre o c#-recursos específicos do IntelliSense, como trechos de código e refatoração. Para obter mais informações, consulte [Visual c# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
