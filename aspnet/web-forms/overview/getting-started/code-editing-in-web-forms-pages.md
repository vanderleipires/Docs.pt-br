---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Edição de código Web Forms do ASP.NET no Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 79b10df04432490d6338dadb8f7ddd36192beb3e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Código de edição Web Forms do ASP.NET no Visual Studio 2013
====================
por [Erik Reitan](https://github.com/Erikre)

Em muitas páginas de formulário Web do ASP.NET, você escreve código no Visual Basic, c# ou outro idioma. O editor de código no Visual Studio pode ajudá-lo a escrever código rapidamente ao ajudá-lo a evitar erros. Além disso, o editor fornece maneiras para você criar códigos reutilizáveis para ajudar a reduzir a quantidade de trabalho que você precisa fazer.

Este passo a passo ilustra vários recursos do editor de código do Visual Studio.

Durante este passo a passo, você aprenderá a:

- Corrija os erros de codificação embutido.
- Refatorar e renomear código.
- Renomear variáveis e objetos.
- Inserir trechos de código.

## <a name="prerequisites"></a>Pré-requisitos


Para concluir este passo a passo, você precisará de:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). O .NET Framework é instalado automaticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 para Web serão ser conhecida como Visual Studio em toda a série de tutoriais.  
    >   
    > Se você estiver usando o Visual Studio, este passo a passo pressupõe que você selecionou o **desenvolvimento Web** conjunto de configurações na primeira vez que você iniciar o Visual Studio. Para obter mais informações, consulte [como: selecionar configurações de ambiente de desenvolvimento da Web](https://msdn.microsoft.com/library/ff521558.aspx).

  Para obter uma introdução ao Visual Studio e ASP.NET, consulte [criando uma página de Web Forms do ASP.NET 4.5 básica no Visual Studio 2013](creating-a-basic-web-forms-page.md).   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Criando um projeto de aplicativo Web e uma página

<a id="sectionToggle0"></a>

Nesta parte do passo a passo, você cria um projeto de aplicativo Web e adicionar uma nova página a ele.

### <a name="to-create-a-web-application-project"></a>Para criar um projeto de aplicativo Web

1. Abra o Microsoft Visual Studio.
2. No menu **Arquivo**, selecione **Novo Projeto**.  
    ![Menu arquivo](code-editing-in-web-forms-pages/_static/image1.png)

    A caixa de diálogo **Novo Projeto** é exibida.
3. Selecione o **modelos**  - &gt; **Visual C#**  - &gt; **Web** grupo de modelos, à esquerda.
4. Escolha o **aplicativo Web ASP.NET** modelo na coluna central.
5. Nomeie o projeto ***BasicWebApp*** e clique no **Okey** botão.   
![Caixa de diálogo Novo projeto](code-editing-in-web-forms-pages/_static/image2.png)
6. Em seguida, selecione o **Web Forms** modelo e clique no **Okey** botão para criar o projeto.  
![Caixa de diálogo Novo projeto ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio cria um novo projeto que inclui a funcionalidade predefinida com base no modelo de Web Forms.


## <a name="creating-a-new-aspnet-web-forms-page"></a>Criar uma nova página Web Forms do ASP.NET


Quando você cria um novo aplicativo do Web Forms usando o **aplicativo Web ASP.NET** modelo de projeto, o Visual Studio adiciona uma página do ASP.NET (página Web Forms) chamada *Default.aspx*, bem como vários outros arquivos e pastas. Você pode usar o *Default.aspx* página como a home page do seu aplicativo da Web. No entanto, para este passo a passo, você irá criar e trabalhar com uma nova página.

### <a name="to-add-a-page-to-the-web-application"></a>Para adicionar uma página para o aplicativo Web


1. Em **Solution Explorer**, clique no nome do aplicativo Web (neste tutorial é o nome do aplicativo **BasicWebSite**) e, em seguida, clique em **adicionar**  - &gt; **Novo Item**.   
A caixa de diálogo **Adicionar Novo Item** é exibida.
2. Selecione o **Visual C#**  - &gt; **Web** grupo de modelos, à esquerda. Em seguida, selecione **formulário da Web** do meio lista e nomeie-o *FirstWebPage*.   
    ![Adicionar caixa de diálogo Novo Item](code-editing-in-web-forms-pages/_static/image4.png)
3. Clique em **adicionar** para adicionar a página de Web Forms ao seu projeto.  
 Visual Studio cria a nova página e abre-o.
4. Em seguida, defina essa nova página como a página de inicialização padrão. Em **Solution Explorer**, clique com botão direito a nova página chamada *FirstWebPage* e selecione **definir como página inicial**. Na próxima vez que você executa esse aplicativo para testar o nosso progresso automaticamente verá essa nova página no navegador.


## <a name="correcting-inline-coding-errors"></a>Corrigindo embutido erros de codificação


O editor de código no Visual Studio ajuda a evitar erros conforme você escrever o código e se você fez um erro, o editor de códigos ajuda você a corrigir o erro. Nesta parte do passo a passo, você irá escrever uma linha de código que ilustram os recursos de correção de erro no editor.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Para corrigir erros de código simples no Visual Studio


1. Em **Design** exibir, clique duas vezes na página em branco para criar um manipulador para o **carga** evento para a página.   
   Você está usando o manipulador de eventos somente como um local para escrever um código.
2. Dentro do manipulador, digite a seguinte linha que contém um erro e pressione **ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Quando você pressiona **ENTER**, o editor de código coloca um sublinhado vermelho e verde (normalmente chamadas &quot;curvadas&quot; linhas) em áreas do código que apresentam problemas. Um sublinhado verde indica um aviso. Um sublinhado vermelho indica um erro que você deve corrigir. 

    Mantenha o ponteiro do mouse sobre `myStr` para ver uma dica de ferramenta que informa você sobre o aviso. Além disso, mantenha o ponteiro do mouse sobre o sublinhado vermelho para ver a mensagem de erro.

    A imagem a seguir mostra o código com os sublinhados.

    ![Bem-vindo texto no modo de Design](code-editing-in-web-forms-pages/_static/image5.png "bem-vindo texto no modo de Design")  
   O erro deve ser corrigido adicionando um ponto e vírgula `;` ao final da linha. O aviso simplesmente notifica que você não tiver usado o `myStr` variável ainda.  

    > [!NOTE] 
    > 
    > Exibir o código atual configurações no Visual Studio de formatação selecionando **ferramentas**  - &gt; **opções**  - &gt; **fontes e Cores**.


## <a name="refactoring-and-renaming"></a>Refatoração e renomear

Refatoração é uma metodologia de software que envolve reestruturação do seu código para tornar mais fácil de entender e manter, preservando sua funcionalidade. Um exemplo simples pode ser que você escreva o código em um manipulador de eventos para obter dados de um banco de dados. À medida que desenvolve sua página, você descobre que você precisa acessar os dados de vários manipuladores diferentes. Portanto, você refatorar o código da página Criando um método de acesso a dados na página e inserindo chamadas para o método nos manipuladores.

O editor de código inclui ferramentas para ajudá-lo a executar várias tarefas de refatoração. Neste passo a passo, você trabalhará com duas técnicas de refatoração: renomear variáveis e métodos de extração. Outras opções de refatoração incluem encapsular campos, promover variáveis locais para parâmetros de método e gerenciar parâmetros de método. A disponibilidade dessas opções de refatoração depende do local no código.

### <a name="refactoring-code"></a>Refatoração do código

Um cenário de refatoração comum é criar (Extrair) um método do código que está dentro de outro membro, como um método. Isso reduz o tamanho do membro original e torna o código extraído reutilizável.

Nesta parte do passo a passo, você escrever códigos simples e, em seguida, extrair um método dele. Refatoração é suportada para c#, então você irá criar uma página que usa c# como sua linguagem de programação.

### <a name="to-extract-a-method-in-a-c-page"></a>Para extrair um método em uma página c#

1. Alternar para **Design** exibição.
2. No **caixa de ferramentas**, do **padrão** guia, arraste um [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) até a página.
3. Clique duas vezes o **botão** controle para criar um manipulador para seu [clique](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) evento e, em seguida, adicione o seguinte código:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   O código cria um **ArrayList** objeto, usa um loop para carregá-lo com valores e então usa outro loop para exibir o conteúdo do **ArrayList** objeto.
4. Pressione **CTRL + F5** para executar a página e, em seguida, clique no **botão** para certificar-se de que você verá a seguinte saída:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Retornar ao editor de códigos e, em seguida, selecione as linhas seguintes no manipulador de eventos.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. A seleção, clique **refatorar**e, em seguida, escolha **extrair método**. 

    O **extrair método** caixa de diálogo é exibida.
7. No **nome do novo método** , digite **DisplayArray**e, em seguida, clique em **Okey**. 

    O editor de código cria um novo método chamado `DisplayArray`e coloca uma chamada para o novo método no **clique** manipulador onde o loop estava originalmente.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Pressione **CTRL + F5** para executar a página novamente e, em seguida, clique no **botão**.

    A página funciona como antes. O `DisplayArray` método agora pode ser chamada em qualquer lugar na classe de página.

## <a name="renaming-variables"></a>Renomeando variáveis

Ao trabalhar com variáveis, bem como objetos, convém renomeá-los depois que eles já estão referenciados em seu código. No entanto, a renomeação de variáveis e objetos pode causar quebra se você esquecer de renomear uma das referências do código. Portanto, você pode usar a refatoração para executar a renomeação.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Para usar a refatoração Renomear uma variável


1. No **clique** manipulador de eventos, localize a seguinte linha:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Clique no nome de variável `alist`, escolha **refatorar**e, em seguida, escolha **Renomear**.

    O **Renomear** caixa de diálogo é exibida.
3. No **novo nome** , digite **ArrayList1** e verifique se o **visualizar alterações de referência** caixa de seleção foi selecionada. Clique em **OK**.

    O **visualizar alterações** caixa de diálogo aparece e exibe uma árvore que contém todas as referências para a variável que você está renomeando.
4. Clique em **aplicar** para fechar o **visualizar alterações** caixa de diálogo.

    As variáveis que fazem referência especificamente a instância que você selecionou são renomeadas. No entanto, observe que a variável `alist` na linha a seguir não é renomeada.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    A variável `alist` essa linha não é renomeada porque ela não representa o mesmo valor que a variável `alist` que você renomeou. A variável `alist` no `DisplayArray` declaração é uma variável local para esse método. Isso ilustra que usando refatoração Renomear variáveis é diferente de simplesmente executar uma ação de localização e substituição no editor; Refatoração renomeia variáveis com conhecimento da semântica da variável que ele está funcionando com.


## <a name="inserting-snippets"></a>Inserindo trechos

Como há muitas tarefas de codificação que os desenvolvedores de Web Forms frequentemente precisam executar, o editor de código fornece uma biblioteca de trechos de código ou blocos de código boa. Você pode inserir trechos de código em sua página.

Cada linguagem que você usa no Visual Studio tem pequenas diferenças na maneira como inserir trechos de código. Para obter informações sobre como inserir trechos de código, consulte [trechos de código do Visual Basic IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx). Para obter informações sobre como inserir trechos de código no Visual c#, consulte [trechos de código do Visual c#](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Próximas etapas

Este passo a passo ilustra os recursos básicos do editor de códigos do Visual Studio 2010 para corrigir erros em seu código, refatoração de código, renomeando variáveis e inserindo trechos de código em seu código. Recursos adicionais no editor podem tornar o desenvolvimento de aplicativo rápido e fácil. Por exemplo, é possível:

- Saiba mais sobre os recursos do IntelliSense, como modificar opções do IntelliSense, gerenciar trechos de código e a pesquisa de trechos de código online. Para obter mais informações, veja [Usando o IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Aprenda a criar seus próprios trechos de código. Para obter mais informações, consulte [criando e usando trechos de código do IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- Saiba mais sobre os recursos específicos do Visual Basic de trechos de código do IntelliSense, como personalizar os trechos de código e solução de problemas. Para obter mais informações, consulte [trechos de código do Visual Basic IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Saiba mais sobre o c#-recursos específicos do IntelliSense, como refatoração e trechos de código. Para obter mais informações, consulte [Visual c# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
