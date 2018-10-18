---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Usando o Visual Studio 2013 para criar uma página de formulários da Web básico ASP.NET 4.5
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: eb1a4632caf00097012bd1757da44016a076630f
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391278"
---
# <a name="using-visual-studio-2013-to-create-a-basic-aspnet-45-web-forms-page"></a>Usando o Visual Studio 2013 para criar uma página de formulários da Web básico ASP.NET 4.5

= = = por [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[](~/includes/rp.md)]

Este passo a passo fornece uma introdução ao ambiente de desenvolvimento da Web no [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) e, na [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Este passo a passo orienta você pela criação de uma página de Web Forms do ASP.NET simples e ilustra as técnicas básicas de criação de uma nova página, adicionando controles e escrever código.

As tarefas ilustradas neste passo a passo incluem:

- Criando um projeto de aplicativo de formulários da Web de sistema de arquivos.
- Familiarizar-se com o Visual Studio.
- Criando uma página ASP.NET.
- Adicionando controles.
- Adicionando manipuladores de eventos.
- Executar e testar uma página do Visual Studio.

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

Nesta parte do passo a passo, você criará um projeto de aplicativo Web e adicionar uma nova página a ele. Você também adicionar texto HTML e execute a página em seu navegador.

### <a name="to-create-a-web-application-project"></a>Para criar um projeto de aplicativo Web

1. Abra o Microsoft Visual Studio.
2. No menu **Arquivo**, selecione **Novo Projeto**.  
    ![Menu arquivo](creating-a-basic-web-forms-page/_static/image1.png)

    A caixa de diálogo **Novo Projeto** é exibida.
3. Selecione o **modelos**  - &gt; **Visual c#**  - &gt; **Web** grupo de modelos à esquerda.
4. Escolha o **aplicativo Web ASP.NET** modelo na coluna central.
5. Nomeie o projeto ***BasicWebApp*** e clique no **Okey** botão.   
![Caixa de diálogo Novo projeto](creating-a-basic-web-forms-page/_static/image2.png)
6. Em seguida, selecione a **Web Forms** modelo e clique no **Okey** botão para criar o projeto.  
![Caixa de diálogo Novo projeto ASP.NET](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio cria um novo projeto que inclui a funcionalidade predefinidas com base no modelo de formulários da Web. Ele fornece não só você com uma *home. aspx* página, um *About* página, uma *Contact* página, mas também inclui a funcionalidade de associação que registra os usuários e salva suas credenciais para que eles podem fazer logon em seu site. Quando uma nova página é criada, por padrão, o Visual Studio exibe a página no **origem** modo, onde você pode ver os elementos da página HTML. A ilustração a seguir mostra o que você veria no **fonte** exibir se você tiver criado uma nova página da Web chamada *BasicWebApp.aspx*.  
    ![Modo de Exibição de Fonte](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Um Tour pelo ambiente de desenvolvimento da Web do Visual Studio


Antes de prosseguir, modificando a página, é útil para se familiarizar com o ambiente de desenvolvimento do Visual Studio. A ilustração a seguir mostra as janelas e ferramentas que estão disponíveis no Visual Studio e Visual Studio Express para Web.

> [!NOTE] 
> 
> Este diagrama mostra o padrão do windows e os locais da janela. O **exibição** menu permite que você exibir janelas adicionais e reorganizar e redimensionar as janelas para atender às suas preferências. Se já tiverem sido feitas alterações para a disposição da janela, o que você vê não corresponderá a ilustração.


 Ambiente do Visual Studio

![Ambiente do Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Familiarize-se com o Web designer

A ilustração acima de examinar e corresponder o texto para a lista a seguir, que descreve com mais frequência windows e ferramentas usadas. (Nem todas as janelas e ferramentas que você vê estão listados aqui, somente aqueles marcados na ilustração anterior.)

- Barras de ferramentas. Fornecem comandos de formatação de texto, localização de texto e assim por diante. Algumas barras de ferramentas estão disponíveis somente quando você estiver trabalhando no **Design** modo de exibição.
- **Gerenciador de soluções** janela. Exibe os arquivos e pastas em seu aplicativo Web.
- Janela do documento. Exibe os documentos que você está trabalhando em janelas com guias. Você pode alternar entre documentos clicando nas guias.
- **Propriedades** janela. Permite que você altere as configurações para a página, elementos HTML, controles e outros objetos.
- Guias de exibição. Apresente exibições diferentes do mesmo documento. **Design** exibição é uma superfície de edição quase WYSIWYG. **Origem** modo é o editor de HTML para a página. **Divisão** modo de exibição exibe ambos o **Design** exibição e o **fonte** exibição para o documento. Você trabalhará com o **Design** e **origem** modos posteriormente neste passo a passo. Se você preferir abrir páginas da Web no **Design** exibir, no **ferramentas** menu, clique em **opções**, selecione o **HTML Designer** nó e altere o **Iniciar páginas em** opção.
- **Caixa de ferramentas**. Fornece controles e elementos HTML que você pode arrastar para sua página. **Caixa de ferramentas** elementos são agrupados por função em comum.
- S **ervidor Explorer**. Exibe as conexões de banco de dados. Se o Gerenciador de servidores não estiver visível, no menu Exibir, clique em Gerenciador de servidores.


### <a name="creating-a-new-aspnet-web-forms-page"></a>Criando uma nova página Web Forms do ASP.NET


Quando você cria um novo aplicativo do Web Forms usando o **aplicativo Web ASP.NET** modelo de projeto, o Visual Studio adiciona uma página do ASP.NET (página de Web Forms) chamada *default. aspx*, bem como vários outros arquivos e pastas. Você pode usar o *default. aspx* página como a home page do seu aplicativo Web. No entanto, para este passo a passo, você irá criar e trabalhar com uma nova página.

### <a name="to-add-a-page-to-the-web-application"></a>Para adicionar uma página ao aplicativo Web


1. Fechar o *default. aspx* página. Para fazer isso, clique na guia que exibe o nome do arquivo e, em seguida, clique na opção de fechar.
2. Na **Gerenciador de soluções**, clique no nome do aplicativo Web (neste tutorial é o nome do aplicativo **BasicWebSite**) e, em seguida, clique em **Add**  - &gt; **Novo Item**.   
A caixa de diálogo **Adicionar Novo Item** é exibida.
3. Selecione o **Visual c#**  - &gt; **Web** grupo de modelos à esquerda. Em seguida, selecione **Web Form** do meio lista e nomeie-o *FirstWebPage*.   
    ![Adicionar caixa de diálogo Novo Item](creating-a-basic-web-forms-page/_static/image6.png)
4. Clique em **adicionar** para adicionar a página da web ao seu projeto.  
Visual Studio cria a nova página e ele é aberto.


### <a name="adding-html-to-the-page"></a>Adicionando o HTML para a página


Nesta parte do passo a passo, você adicionará um texto estático para a página.

### <a name="to-add-text-to-the-page"></a>Para adicionar texto à página


1. Na parte inferior da janela do documento, clique o **Design** tab para mudar para **Design** modo de exibição.

    Modo Design exibe a página atual de uma forma de tipo WYSIWYG. Neste ponto, você não tem qualquer texto ou controles da página, portanto, a página está em branco, exceto para uma linha tracejada que descreve um retângulo. Esse retângulo representa uma **div** elemento na página.
2. Clique dentro do retângulo que é descrito por uma linha tracejada.
3. Tipo de **bem-vindo ao Visual Web Developer** e pressione **ENTER** duas vezes.

    A ilustração a seguir mostra o texto que você digitou na **Design** modo de exibição.

    ![Texto de boas vindas no modo de exibição de Design](creating-a-basic-web-forms-page/_static/image7.png "texto de boas vindas no modo de exibição de Design")
4. Alterne para **origem** modo de exibição.

    Você pode ver o HTML no **fonte** modo de exibição que você criou quando você digitou na **Design** modo de exibição.  
    ![Página da Web com texto estático](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>A execução da página


Antes de prosseguir com a adição de controles para a página, você pode executá-lo.

### <a name="to-run-the-page"></a>Para executar a página


1. Na **Gerenciador de soluções**, clique com botão direito *FirstWebPage* e selecione **Set as Start Page**.
2. Pressione **CTRL + F5** para executar a página.

    A página é exibida no navegador. Embora a página que você criou tem uma extensão de nome de arquivo de *. aspx*, atualmente é executado como qualquer página HTML.

    Para exibir uma página no navegador pode também clicar duas vezes na página **Gerenciador de soluções** e selecione **exibir no navegador**.
3. Feche o navegador para interromper o aplicativo Web.


## <a name="adding-and-programming-controls"></a>Adicionando e programando controles


<a id="sectionToggle1"></a>

Agora, você adicionará controles de servidor para a página. Controles de servidor, como botões, rótulos, caixas de texto e outros controles familiares, fornecem recursos de processamento de formulário típicos para suas páginas Web Forms. No entanto, você pode programar os controles com o código que é executado no servidor, em vez do cliente.

Você adicionará um [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controle, um [caixa de texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controle e uma [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) o controle para a página e escrever código para lidar com o [clique](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) eventos para o [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controle.

### <a name="to-add-controls-to-the-page"></a>Para adicionar controles à página


1. Clique o **Design** tab para mudar para **Design** modo de exibição.
2. Coloque o ponto de inserção no final dos **bem-vindo ao Visual Web Developer** texto e pressione **ENTER** cinco ou mais vezes para liberar algum espaço no **div** caixa do elemento.
3. No **caixa de ferramentas**, expanda o **Standard** grupo se ele não ainda estiver expandido.  
Observe que talvez seja necessário expandir a **caixa de ferramentas** janela à esquerda para exibi-lo.
4. Arraste uma [caixa de texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) até a página de controle e solte-o no meio do **div** caixa do elemento que tem **bem-vindo ao Visual Web Developer** na primeira linha.
5. Arraste uma [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) até a página de controle e solte-o à direita do [caixa de texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controle.
6. Arraste uma [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) até a página de controle e solte-o em uma linha separada abaixo de [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controle.
7. Coloque o ponto de inserção acima de [caixa de texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controlar e, em seguida, digite **insira seu nome:** .

    Esse texto HTML estático é a legenda para o [caixa de texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) controle. Você pode misturar HTML estático e controles de servidor na mesma página. A ilustração a seguir mostra como os três controles aparecem na **Design** modo de exibição.

    ![Três controles no modo de exibição de Design](creating-a-basic-web-forms-page/_static/image9.png "três controles no modo de exibição de Design")


### <a name="setting-control-properties"></a>Definindo propriedades de controle


Visual Studio oferece várias maneiras de definir as propriedades de controles na página. Nesta parte do passo a passo, você definirá as propriedades em ambos **Design** exibição e **origem** modo de exibição.

### <a name="to-set-control-properties"></a>Para definir as propriedades de controle


1. Primeiro, exiba os **propriedades** windows, selecionando a partir o **exibição** menu -&gt; **Other Windows**  - &gt; **Janela de dimensões**. Como alternativa, você pode selecionar **F4** para exibir o **propriedades** janela.
2. Selecione o [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controle e, em seguida, no **propriedades** janela, defina o valor de **texto** para **nome de exibição**. O texto que você inseriu é exibido no botão no designer, conforme mostrado na ilustração a seguir.

    ![Definir o texto do botão](creating-a-basic-web-forms-page/_static/image10.png "texto do botão definido")
3. Alterne para **origem** modo de exibição.

    **Origem** modo de exibição exibe o HTML da página, incluindo os elementos que o Visual Studio criou para os controles de servidor. Controles são declarados usando a sintaxe HTML, exceto as marcas que usam o prefixo **asp:** e inclua o atributo **runat =&quot;servidor&quot;**.

    As propriedades de controle são declaradas como atributos. Por exemplo, quando você define o [texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) propriedade para o [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controlar, na etapa 1, você realmente definiu o **texto** atributo na marcação do controle.

    > [!NOTE] 
    > 
    > Todos os controles estão dentro de um **formulário** elemento, que também tem o atributo **runat =&quot;servidor&quot;**. O **runat =&quot;server&quot;**  atributo e o **asp:** de prefixo para marcas de controle marcam os controles para que eles sejam processados pelo ASP.NET no servidor quando a página é executada. Código fora dos **&lt;formam runat =&quot;server&quot; &gt;** e **&lt;script runat =&quot;server&quot; &gt;** elementos são enviados sem alterações para o navegador, por isso, o código do ASP.NET deve estar dentro de um elemento cuja marca de abertura contém o **runat =&quot;server&quot;**  atributo.
4. Em seguida, você adicionará uma propriedade adicional para o [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controle. Coloque o ponto de inserção diretamente após **asp: Label** na **&lt;asp: Label&gt;** marcar e, em seguida, pressione **barra de espaços**.

    É exibida uma lista suspensa que exibe a lista de propriedades disponíveis, você pode definir para um [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controle. Esse recurso, conhecido como **IntelliSense**, ajudam na **origem** exibição com a sintaxe de controles de servidor, elementos HTML e outros itens na página. A ilustração a seguir mostra a **IntelliSense** na lista suspensa para o [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controle.

    ![Atributos do IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "atributos do IntelliSense")
5. Selecione **ForeColor** e, em seguida, digite um sinal de igual.

    O IntelliSense exibe uma lista de cores.

    > [!NOTE] 
    > 
    > Você pode exibir uma **IntelliSense** lista suspensa a qualquer momento pressionando **CTRL + J** ao exibir o código.
6. Selecione uma cor para o **[rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** texto do controle. Verifique se que você selecionar uma cor escura para ler em um plano de fundo branco.

    O **ForeColor** atributo é concluído com a cor que você selecionou, incluindo as aspas de fechamento.


### <a name="programming-the-button-control"></a>O controle de botão de programação


Para este passo a passo, você irá escrever código que lê o nome que o usuário digita na caixa de texto e, em seguida, exibe o nome na [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controle.

### <a name="add-a-default-button-event-handler"></a>Adicionar um manipulador de eventos do botão padrão


1. Alterne para **Design** modo de exibição.
2. Clique duas vezes o [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) controle.

    Por padrão, o Visual Studio alterna para um arquivo code-behind e cria um manipulador de eventos de esqueleto para o [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) evento padrão do controle, o [clique](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) eventos. O arquivo code-behind separa sua marcação da interface do usuário (como HTML) do seu código de servidor (como c#).   
   O cursor é posicionado à adicionou o código para este manipulador de eventos.

    > [!NOTE] 
    > 
    > Clicar duas vezes em um controle no **Design** exibição é apenas uma das várias maneiras que você pode criar manipuladores de eventos.
3. Dentro de **Button1\_clique em** manipulador de eventos, digite **Label1** seguido por um período (**.**).

    Quando você digita o período após o **ID** do rótulo (**Label1**), o Visual Studio exibe uma lista de membros disponíveis para o [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controle, conforme mostrado no exemplo a seguir ilustração. Um membro comumente uma propriedade, método ou evento.

    ![IntelliSense na exibição de código](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense na exibição de código")
4. Concluir a **clique** manipulador de eventos para o botão de modo que ele lê, conforme mostrado no exemplo de código a seguir.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Volte para a exibição a **fonte** modo de exibição de sua marcação HTML clicando com o *FirstWebPage* no **Gerenciador de soluções** e selecionando **exibição Marcação**.
6. Role até a **&lt;asp: Button&gt;** elemento. Observe que o **&lt;asp: Button&gt;** elemento agora tem o atributo **onclick =&quot;Button1\_clique&quot;**.

    Esse atributo associa o botão [clique](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) eventos para o método de manipulador que você codificou na etapa anterior.

    Métodos do manipulador de eventos podem ter qualquer nome; o nome exibido é o nome padrão criado pelo Visual Studio. O ponto importante é que o nome usado para o **OnClick** atributo no HTML deve corresponder ao nome de um método definido no code-behind.


### <a name="running-the-page"></a>A execução da página


Agora você pode testar os controles de servidor na página.

### <a name="to-run-the-page"></a>Para executar a página


1. Pressione **CTRL + F5** para executar a página no navegador. Se ocorrer um erro, verifique novamente as etapas acima.
2. Insira um nome na caixa de texto e clique no **nome de exibição** botão.

    O nome que você inseriu é exibido na [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controle. Observe que, quando você clica no botão, a página é postada para o servidor Web. ASP.NET, em seguida, recria a página, executa seu código (nesse caso, o [botão](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) do controle [clique](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) execuções de manipulador de eventos) e, em seguida, envia a nova página para o navegador. Se você observar a barra de status no navegador, você pode ver que a página está fazendo uma viagem de ida e volta ao servidor da Web sempre que você clicar no botão.
3. No navegador, exiba a origem da página que você está executando o botão direito do mouse na página e selecionando **Exibir código-fonte**.

    Na página de código fonte, consulte o HTML sem nenhum código de servidor. Especificamente, você não vir as **&lt;asp:&gt;** elementos que você estava trabalhando no **origem** modo de exibição. Quando a página é executada, o ASP.NET processa os controles de servidor e renderiza os elementos HTML para a página que executam as funções que representam o controle. Por exemplo, o **&lt;asp: Button&gt;** controle é renderizado como o HTML **&lt;tipo de entrada =&quot;enviar&quot; &gt;** elemento.
4. Feche o navegador.


## <a name="working-with-additional-controls"></a>Trabalhando com controles adicionais

<a id="sectionToggle2"></a>

Nesta parte do passo a passo, você trabalhará com o [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controle, que exibe as datas de um mês por vez. O [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controle é um controle mais complexo que o botão, a caixa de texto e o rótulo você tem trabalhado e ilustra alguns recursos adicionais de controles de servidor.

Nesta seção, você adicionará uma [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) o controle para a página e formatá-la.

### <a name="to-add-a-calendar-control"></a>Para adicionar um controle de calendário


1. No Visual Studio, mude para a **Design** modo de exibição.
2. Do **padrão** seção o **caixa de ferramentas**, arraste uma [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controlar até a página e solte-o abaixo o **div** elemento que contém outros controles.

    Painel de smart tag do calendário é exibido. O painel exibe os comandos que facilitam para você realizar as tarefas mais comuns para o controle selecionado. A ilustração a seguir mostra a [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controlar conforme renderizado em **Design** modo de exibição.

    ![Controle no modo de exibição de Design de calendário](creating-a-basic-web-forms-page/_static/image13.png "controle no modo de exibição de Design de calendário")
3. No painel de smart tag, escolha **AutoFormatação**.

    O **AutoFormatação** caixa de diálogo é exibida, que permite que você selecione um esquema de formatação para o calendário. A ilustração a seguir mostra a **AutoFormatação** caixa de diálogo para o [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controle.

    ![Caixa de diálogo Auto Format (controle de calendário)](creating-a-basic-web-forms-page/_static/image14.png "caixa de diálogo Auto Format (controle de calendário)")
4. Dos **selecione um esquema de** , selecione **simples** e, em seguida, clique em **Okey**.
5. Alterne para **origem** modo de exibição.

    Você pode ver os **&lt;asp: Calendar&gt;** elemento. Esse elemento é muito mais do que os elementos para os controles simples que você criou anteriormente. Ele também inclui subelementos, tais como  **&lt;WeekEndDayStyle&gt;**, que representam várias configurações de formatação. A ilustração a seguir mostra a [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) no controlar **origem** modo de exibição. (A marcação exata que você vê no **origem** exibição pode diferir ligeiramente das ilustração.)

    ![Controle no modo de exibição de fonte de calendário](creating-a-basic-web-forms-page/_static/image15.png "controle no modo de exibição de fonte de calendário")


### <a name="programming-the-calendar-control"></a>O controle de calendário de programação


Nesta seção, você programará o [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controle para exibir a data selecionada no momento.

### <a name="to-program-the-calendar-control"></a>Para programar o controle de calendário


1. Na **Design** exibir, clique duas vezes o [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controle.

    Um novo manipulador de eventos é criado e exibido no arquivo code-behind chamado *FirstWebPage.aspx.cs*.
2. Concluir a [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) manipulador de eventos com o código a seguir.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    O código acima define o texto do controle de rótulo para a data selecionada do controle de calendário.


### <a name="running-the-page"></a>A execução da página


Agora você pode testar o calendário.

### <a name="to-run-the-page"></a>Para executar a página


1. Pressione **CTRL + F5** para executar a página no navegador.
2. Clique em uma data no calendário.

    A data em que você clicou é exibida na [rótulo](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) controle.
3. No navegador, exiba o código-fonte para a página.

    Observe que o [calendário](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) controle foi renderizado para a página como um **tabela**, com cada dia como um **td** elemento.
4. Feche o navegador.


## <a name="next-steps"></a>Próximas etapas


<a id="nextStepsToggle"></a>

Este passo a passo ilustra os recursos básicos do designer de página do Visual Studio. Agora que você entende como criar e editar uma página de Web Forms no Visual Studio, você talvez queira explorar outros recursos. Por exemplo, você talvez queira fazer o seguinte:

- Saiba mais sobre o ASP.NET Web Forms, seguindo a série de tutoriais passo a passo [Introdução ao Web Forms do ASP.NET 4.5 e Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Saiba mais sobre folhas de estilos em cascata (CSS). Para obter detalhes, consulte [trabalhando com visão geral de CSS](https://msdn.microsoft.com/library/bb398931.aspx).
