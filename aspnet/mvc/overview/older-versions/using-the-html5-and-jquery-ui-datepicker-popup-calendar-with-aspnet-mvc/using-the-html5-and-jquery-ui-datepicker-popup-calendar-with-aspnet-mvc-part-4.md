---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: Usando HTML5 e calendário jQuery UI Datepicker pop-up com o ASP.NET MVC – parte 4 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os fundamentos de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up jQuery UI datepicker em uma MV do ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 7ecd180b7608e82ea143575c6590574b92843dcf
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577490"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Usando HTML5 e calendário jQuery UI Datepicker pop-up com o ASP.NET MVC – parte 4
====================
por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> Este tutorial ensinará os fundamentos de como trabalhar com modelos do editor, modelos de exibição e calendário pop-up jQuery UI datepicker em um aplicativo Web ASP.NET MVC.


### <a name="adding-a-template-for-editing-dates"></a>Adicionando um modelo para edição de datas

Nesta seção, você criará um modelo para edição de datas que serão aplicadas quando o ASP.NET MVC exibe a interface do usuário para editar as propriedades de modelo que são marcadas com o **data** enumeração da [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo. O modelo irá renderizar apenas a data; tempo não será exibido. O modelo, você usará o [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) calendário pop-up para fornecer uma maneira para editar as datas.

Para começar, abra o *Movie.cs* arquivo e adicione o [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo com o **data** enumeração para o `ReleaseDate` propriedade, conforme mostrado no código a seguir:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Esse código faz com que o `ReleaseDate` campo a ser exibida sem o tempo em que ambos exibir modelos e editar modelos. Se seu aplicativo contém um *date.cshtml* modelo na *Views\Shared\EditorTemplates* pasta ou nos *Views\Movies\EditorTemplates* pasta, modelo será usado para renderizar qualquer `DateTime` propriedade durante a edição. Caso contrário, o sistema de modelos internos do ASP.NET exibirá a propriedade como uma data.

Pressione CTRL+F5 para executar o aplicativo. Selecione um link de edição para verificar que o campo de entrada para a data de lançamento está mostrando apenas a data.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

Na **Gerenciador de soluções**, expanda o *exibições* pasta, expanda o *compartilhado* pasta e, em seguida, clique com botão direito a *Views\Shared\EditorTemplates* pasta.

Clique em **Add**e, em seguida, clique em **exibição**. O **adicionar exibição** caixa de diálogo é exibida.

No **nome da exibição** , digite &quot;data&quot;.

Selecione o **criar como uma exibição parcial** caixa de seleção. Certifique-se de que o **usar uma layout ou página mestra** e **criar uma exibição fortemente tipada** caixas de seleção não estiver selecionadas.

Clique em **Adicionar**. O *Views\Shared\EditorTemplates\Date.cshtml* modelo é criado.

Adicione o seguinte código para o *Views\Shared\EditorTemplates\Date.cshtml* modelo.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

A primeira linha declara que o modelo a ser um `DateTime` tipo. Embora você não precisa declarar o tipo de modelo em Editar e exibir modelos, é uma prática recomendada para que você obtenha o tempo de compilação a verificação de modelo que está sendo passado para o modo de exibição. (Outro benefício é que você, em seguida, obtenha o IntelliSense para o modelo no modo de exibição no Visual Studio.) Se o tipo de modelo não for declarado, ASP.NET MVC considerará uma [dinâmico](https://msdn.microsoft.com/library/dd264741.aspx) de tipo e não há nenhum tempo de compilação verificação de tipo. Se você declarar o modelo a ser um `DateTime` tipo, ele se torna fortemente tipado.

A segunda linha é apenas literal marcação HTML que exibe &quot;usando o modelo de data&quot; antes de um campo de data. Você usará esta linha de temporariamente para verificar se esse modelo de data está sendo usado.

A próxima linha é um [HTML](https://msdn.microsoft.com/library/system.web.mvc.html.inputextensions.textbox.aspx) auxiliar que renderiza uma `input` campo que é uma caixa de texto. O terceiro parâmetro para o auxiliar usa um tipo anônimo para definir a classe para a caixa de texto `datefield` e o tipo a ser `date`. (Porque `class` é um reservada no c#, você precisará usar o `@` caractere de escape a `class` atributo no analisador c#.)

O `date` tipo é um tipo de entrada do HTML5 que permite que os navegadores com suporte a HTML5 renderizar um controle de calendário do HTML5. Posteriormente, você adicionará um JavaScript para ligar o datepicker do jQuery para o `Html.TextBox` elemento usando o `datefield` classe.

Pressione CTRL+F5 para executar o aplicativo. Você pode verificar se o `ReleaseDate` propriedade na exibição de edição é usando o modelo de edição porque o modelo exibe &quot;usando o modelo de data&quot; imediatamente antes o `ReleaseDate` caixa de entrada de texto conforme mostrado nesta imagem:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

Em seu navegador, exiba a origem da página. (Por exemplo, a página com o botão direito e selecione **Exibir código-fonte**.) O seguinte exemplo mostra algumas da marcação da página, ilustrando os `class` e `type` atributos no HTML renderizado.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Volte para o *Views\Shared\EditorTemplates\Date.cshtml* modelo e remover as &quot;usando o modelo de data&quot; marcação. Agora o modelo concluído esta aparência:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Adicionando um calendário jQuery UI Datepicker pop-up usando o NuGet

Nesta seção, você adicionará a [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendário pop-up para o modelo de edição de data. O [jQuery UI](http://jqueryui.com/) biblioteca fornece suporte para a animação, efeitos e widgets personalizáveis avançados. Ela é criada sobre a biblioteca JavaScript jQuery. O calendário pop-up datepicker torna fácil e natural para inserir datas usando um calendário em vez de inserir uma cadeia de caracteres. O calendário pop-up também limita os usuários a datas legais — entrada de texto comum para uma data permitiria que você digitar algo como `2/33/1999` (fevereiro 33rd, 1999), mas o [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendário pop-up não permitirá que.

Primeiro, você precisará instalar as bibliotecas de interface do usuário do jQuery. Para fazer isso, você usará o NuGet, que é um Gerenciador de pacote que está incluído nas versões do SP1 do Visual Studio 2010 e o Visual Web Developer.

No Visual Web Developer, do **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca** e, em seguida, selecione **Manage NuGet Packages**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Observação: Se o **ferramentas** menu não exibe o **Gerenciador de pacotes de biblioteca** de comando, você precisa instalar o NuGet, seguindo as instruções no [instalando o NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) página do o site do NuGet.   
  
Se você estiver usando o Visual Studio em vez do Visual Web Developer, do **ferramentas** menu, selecione **Gerenciador de pacotes de biblioteca** e, em seguida, selecione **adicionar referência de pacote de biblioteca**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

No **MVCMovie - gerenciar pacotes NuGet** caixa de diálogo, clique o **Online** guia à esquerda e, em seguida, insira &quot;jQuery.UI&quot; na caixa de pesquisa. Selecione j **WIdgets de interface do usuário de consulta: Datepicker**, em seguida, selecione o **instalar** botão.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

O NuGet adiciona essas versões de depuração e minificadas versões do jQuery Core de interface do usuário e o selecionador de data do jQuery UI ao seu projeto:

- *jquery.ui.core.js*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.js*
- *jquery.ui.datepicker.min.js*

Observação: As versões de depuração (os arquivos sem o *. Min* extensão) são úteis para depuração, mas em um site de produção, você incluiria apenas as versões minimizadas.

Para realmente usar o seletor de data do jQuery, você precisa criar um script de jQuery irá se conectar o widget de calendário para o modelo de edição. No **Gerenciador de soluções**, com o botão direito a *Scripts* pasta e selecione **Add**, em seguida, **Novo Item**e, em seguida, **JScript Arquivo**. Nomeie o arquivo *DatePickerReady.js*.

Adicione o seguinte código para o *DatePickerReady.js* arquivo:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Se você não estiver familiarizado com o jQuery, aqui está uma breve explicação do que isso faz: a primeira linha é o &quot;jQuery pronto&quot; função, que é chamada quando todos os elementos DOM em uma página tiveram carregado. A segunda linha seleciona todos os elementos de DOM que têm o nome da classe `datefield`, em seguida, invoca o `datepicker` função para cada um deles. (Lembre-se de que você adicionou o `datefield` de classe para o *Views\Shared\EditorTemplates\Date.cshtml* modelo anteriormente no tutorial.)

Em seguida, abra o *Views\Shared\\layout. cshtml* arquivo. Você precisará adicionar referências para os seguintes arquivos são necessários para que você possa usar o seletor de datas:

- *Content/themes/base/jquery.ui.core.css*
- *Content/themes/base/jquery.ui.datepicker.css*
- *Content/themes/base/jquery.ui.theme.css*
- *jquery.ui.core.min.js*
- *jquery.ui.datepicker.min.js*
- *DatePickerReady.js*

O exemplo a seguir mostra o código real que você deve adicionar na parte inferior a `head` elemento na *Views\Shared\\layout. cshtml* arquivo.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Completo `head` seção é mostrada aqui:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

O [auxiliar de URL conteúdo](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.content.aspx) método converte o caminho do recurso em um caminho absoluto. Você deve usar `@URL.Content` faça referência corretamente esses recursos quando o aplicativo está em execução no IIS.

Pressione CTRL+F5 para executar o aplicativo. Selecione um link de edição, e em seguida, coloque o ponto de inserção para o **ReleaseDate** campo. Calendário pop-up jQuery UI é exibido.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Como a maioria dos controles do jQuery, o datepicker permite personalizá-la amplamente. Para obter informações, consulte [personalização Visual: Criando um tema de interface do usuário do jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) sobre o [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) site.

### <a name="supporting-the-html5-date-input-control"></a>Suporte a controle de entrada de data HTML5

Conforme mais navegadores dão suporte a HTML5, você desejará usar o HTML5 nativo de entrada, como o `date` elemento de entrada e não usar o calendário de interface do usuário do jQuery. Você pode adicionar lógica ao seu aplicativo para usar controles HTML5 automaticamente se o navegador dá suporte a eles. Para fazer isso, substitua o conteúdo do *DatePickerReady.js* arquivo com o seguinte:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

A primeira linha desse script usa o Modernizr para verificar se a entrada de data HTML5 é compatível. Se não for compatível, o seletor de data do jQuery da interface do usuário está conectado em vez disso. ([Modernizr](http://www.modernizr.com/docs/) é uma biblioteca de JavaScript de código-fonte aberto que detecta a disponibilidade de implementações nativas de HTML5 e CSS3. O Modernizr é incluído em qualquer novos projetos do ASP.NET MVC que você criar.)

Após fazer essa alteração, você pode testá-la usando um navegador que ofereça suporte a HTML5, como 11 Opera. Execute o aplicativo usando um navegador compatível com o HTML5 e editar uma entrada de filme. O controle de data HTML5 é usado em vez de calendário pop-up jQuery UI:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Como as novas versões de navegadores estiver implementando HTML5 incrementalmente, uma boa abordagem para agora é adicionar código ao seu site que acomoda uma ampla variedade de suporte ao HTML5. Por exemplo, uma mais robusta *DatePickerReady.js* script é mostrado abaixo que permite seus navegadores de suporte do site que suportam apenas parcialmente o controle de data HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Esse script seleciona HTML5 `input` elementos do tipo `date` que não suportam totalmente o controle de data HTML5. Para esses elementos, conecta-se o calendário pop-up jQuery UI e, em seguida, altera a `type` de atributos de `date` para `text`. Alterando a `type` de atributos de `date` para `text`, suporte parcial de data HTML5 é eliminado. Ainda mais robusta *DatePickerReady.js* script pode ser encontrado em [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Adição de datas anuláveis para os modelos

Se você usa um dos modelos existentes de data e passar uma data de nula, você obterá um erro de tempo de execução. Para tornar os modelos de data mais robusto, você vai alterá-los para lidar com valores nulos. Para dar suporte a datas anuláveis, altere o código na *Views\Shared\DisplayTemplates\DateTime.cshtml* à seguinte:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

O código retorna uma cadeia de caracteres vazia quando o modelo está **nulo**.

Altere o código na *Views\Shared\EditorTemplates\Date.cshtml* arquivo para o seguinte:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Quando esse código é executado, se o modelo não for nulo, o modelo `DateTime` valor é usado. Se o modelo for nulo, a data atual será usada.

### <a name="wrapup"></a>Wrapup

Este tutorial abordou as Noções básicas de auxiliares com modelos do ASP.NET e mostra como usar o calendário de pop-up jQuery UI datepicker em um aplicativo ASP.NET MVC. Para obter mais informações, experimente estes recursos:

- Para obter informações sobre localização, consulte o blog do Rajeesh [JQueryUI Datepicker no ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Para obter informações sobre o jQuery UI, consulte [jQuery UI](http://docs.jquery.com/UI).
- Para obter informações sobre como localizar o controle datepicker, consulte [Datepicker/interface do usuário/localização](http://docs.jquery.com/UI/Datepicker/Localization).
- Para obter mais informações sobre os modelos do ASP.NET MVC, consulte a série do blog de Brad Wilson sobre [modelos do ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Embora a série foi escrita para o ASP.NET MVC 2, o material ainda se aplica para a versão atual do ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
