---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
title: "Usando HTML5 e jQuery UI Datepicker pop-up de calendário com ASP.NET MVC - parte 4 | Microsoft Docs"
author: Rick-Anderson
description: "Este tutorial ensina as Noções básicas de como trabalhar com modelos de editor, modelos de exibição e o calendário de pop-up jQuery UI datepicker em uma MV ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 57666c69-2b0f-423a-a61d-be49547fa585
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4
msc.type: authoredcontent
ms.openlocfilehash: 2b76d2e449d491fd8d808343065b22ba267f1152
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-4"></a>Usando HTML5 e jQuery UI Datepicker pop-up de calendário com ASP.NET MVC - parte 4
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial irá ensiná-lo os fundamentos de como trabalhar com modelos de editor, modelos de exibição e calendário pop-up jQuery UI datepicker em um aplicativo ASP.NET MVC.


### <a name="adding-a-template-for-editing-dates"></a>Adicionando um modelo para edição de datas

Nesta seção, você criará um modelo para edição de datas que serão aplicadas ao ASP.NET MVC exibe a interface do usuário para editar as propriedades de modelo que são marcadas com o **data** enumeração do [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atributo. O modelo será renderizado somente a data; tempo não será exibido. O modelo, você usará o [jQuery UI Datepicker](http://jqueryui.com/demos/datepicker/) calendário pop-up para fornecer uma maneira para editar as datas.

Para começar, abra o *Movie.cs* e adicione o [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo com o **data** enumeração para o `ReleaseDate` propriedade, conforme mostrado no código a seguir:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample1.cs)]

Este código faz com que o `ReleaseDate` campo a ser exibido sem o tempo em ambos exibir modelos e editar modelos. Se seu aplicativo contém um *date.cshtml* modelo no *Views\Shared\EditorTemplates* pasta ou o *Views\Movies\EditorTemplates* pasta, modelo será usado para processar qualquer `DateTime` propriedade durante a edição. Caso contrário, o sistema de modelos interno do ASP.NET exibirá a propriedade como uma data.

Pressione CTRL+F5 para executar o aplicativo. Selecione um link de edição para verificar se o campo de entrada para a data de lançamento está mostrando apenas a data.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image1.png)

Em **Solution Explorer**, expanda o *exibições* pasta, expanda o *compartilhado* pasta e, em seguida, clique com botão direito a *Views\Shared\EditorTemplates* pasta.

Clique em **adicionar**e, em seguida, clique em **exibição**. O **adicionar exibição** caixa de diálogo é exibida.

No **nome de exibição** , digite &quot;data&quot;.

Selecione o **criar como uma exibição parcial** caixa de seleção. Verifique se o **usar uma layout ou página mestra** e **criar uma exibição fortemente tipada** caixas de seleção não estiver selecionadas.

Clique em **Adicionar**. O *Views\Shared\EditorTemplates\Date.cshtml* modelo é criado.

Adicione o seguinte código para o *Views\Shared\EditorTemplates\Date.cshtml* modelo.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample2.cshtml)]

A primeira linha declara o modelo a ser um `DateTime` tipo. Embora você não precisa declarar o tipo de modelo em edição e exibir modelos, é uma prática recomendada para que você obtenha o tempo de compilação a verificação do modelo que está sendo passado para o modo de exibição. (Outro benefício é que você obtenha, em seguida, o IntelliSense para o modelo no modo de exibição no Visual Studio.) Se o tipo de modelo não é declarado, ASP.NET MVC considera um [dinâmico](https://msdn.microsoft.com/en-us/library/dd264741.aspx) de tipo e não há nenhum tempo de compilação verificação de tipo. Se você declarar o modelo a ser um `DateTime` tipo, ele se torna fortemente tipado.

A segunda linha é apenas literal marcação HTML que exibe &quot;usando o modelo de data&quot; antes de um campo de data. Você usará essa linha de temporariamente para verificar se esse modelo de data está sendo usado.

A próxima linha é um [HTML. TextBox](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.inputextensions.textbox.aspx) auxiliar que renderiza uma `input` campo é uma caixa de texto. O terceiro parâmetro para o auxiliar usa um tipo anônimo para definir a classe de caixa de texto para `datefield` e o tipo para `date`. (Porque `class` é um reservado em c#, você precisa usar o `@` caractere de escape de `class` atributo no analisador de C#.)

O `date` é um tipo de entrada do HTML5 que permite que navegadores com suporte a HTML5 renderizar um controle de calendário do HTML5. Posteriormente, você adicionará um JavaScript para ligar o datepicker jQuery para o `Html.TextBox` elemento usando o `datefield` classe.

Pressione CTRL+F5 para executar o aplicativo. Você pode verificar se o `ReleaseDate` propriedade no modo de edição é usando o modelo de edição porque o modelo exibe &quot;usando o modelo de data&quot; antes de `ReleaseDate` caixa de entrada de texto conforme mostrado nesta imagem:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image2.png)

No navegador, exiba a origem da página. (Por exemplo, a página e selecione **Exibir código-fonte**.) O seguinte exemplo mostra algumas da marcação da página, que ilustra o `class` e `type` atributos no HTML renderizado.

[!code-html[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample3.html)]

Volte para o *Views\Shared\EditorTemplates\Date.cshtml* modelo e remover o &quot;usando o modelo de data&quot; marcação. Agora o modelo concluído esta aparência:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample4.cshtml)]

### <a name="adding-a-jquery-ui-datepicker-popup-calendar-using-nuget"></a>Adicionando um calendário jQuery UI Datepicker pop-up usando o NuGet

Nesta seção, você adicionará o [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendário pop-up para o modelo de edição de data. O [jQuery UI](http://jqueryui.com/) biblioteca fornece suporte para a animação, efeitos e widgets personalizáveis avançados. Ele é criado sobre a biblioteca JavaScript jQuery. O calendário pop-up de datepicker torna fácil e natural para inserir datas usando um calendário em vez de inserir uma cadeia de caracteres. O calendário pop-up também limita os usuários datas legais — entrada de texto comum para uma data permitem que você digitar algo como `2/33/1999` (fevereiro 33rd, 1999), mas o [jQuery UI datepicker](http://jqueryui.com/demos/datepicker/) calendário pop-up não permite que.

Primeiro, você precisa instalar as bibliotecas de interface do usuário da jQuery. Para fazer isso, você usará o NuGet, que é um Gerenciador de pacote que é incluído em versões do SP1 do Visual Studio 2010 e o Visual Web Developer.

No Visual Web Developer, do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote** e, em seguida, selecione **gerenciar pacotes NuGet**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image3.png)

Observação: Se o **ferramentas** menu não exibe o **Gerenciador de biblioteca de pacote** de comando, você precisa instalar o NuGet, seguindo as instruções [instalando NuGet](http://docs.nuget.org/docs/start-here/installing-nuget) página do o site do NuGet.   
  
Se estiver usando o Visual Studio em vez do Visual Web Developer, do **ferramentas** menu, selecione **Gerenciador de biblioteca de pacote** e, em seguida, selecione **adicionar referência de pacote de biblioteca**.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image4.png)

No **MVCMovie - gerenciar pacotes NuGet** caixa de diálogo, clique o **Online** guia à esquerda e, em seguida, digite &quot;jQuery.UI&quot; na caixa de pesquisa. Selecione j **WIdgets de interface do usuário de consulta: Datepicker**, em seguida, selecione o **instalar** botão.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image5.png)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image6.png)

NuGet adiciona essas versões de depuração e minimizada do jQuery da interface do usuário principal e o seletor de data do jQuery UI ao seu projeto:

- *jQuery.UI.Core.js*
- *jQuery.UI.Core.min.js*
- *jQuery.UI.DatePicker.js*
- *jQuery.UI.DatePicker.min.js*

Observação: As versões de depuração (os arquivos sem o *. min.js* extensão) são úteis para depuração, mas em um site de produção, você incluiria somente as versões minimizadas.

Para usar o seletor de data jQuery, você precisa criar um script de jQuery que irá se conectar o widget de calendário para o modelo de edição. No **Gerenciador de soluções**, com o botão direito do *Scripts* pasta e selecione **adicionar**, em seguida, **Novo Item**e, em seguida, **JScript Arquivo**. Nomeie o arquivo *DatePickerReady.js*.

Adicione o seguinte código para o *DatePickerReady.js* arquivo:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample5.js)]

Se você não estiver familiarizado com o jQuery, aqui está uma breve explicação do que isso faz: a primeira linha é o &quot;jQuery pronto&quot; função, que é chamada quando todos os elementos DOM em uma página carregou. A segunda linha seleciona todos os elementos DOM com o nome da classe `datefield`, em seguida, invoca o `datepicker` função para cada um deles. (Lembre-se de que você adicionou o `datefield` de classe para o *Views\Shared\EditorTemplates\Date.cshtml* modelo anteriormente no tutorial.)

Em seguida, abra o *exibições \ compartilhadas\\cshtml* arquivo. Você precisa adicionar referências para os seguintes arquivos são necessários para que você possa usar o seletor de data:

- *Content/Themes/base/jQuery.UI.Core.CSS*
- *Content/Themes/base/jQuery.UI.DatePicker.CSS*
- *Content/Themes/base/jQuery.UI.Theme.CSS*
- *jQuery.UI.Core.min.js*
- *jQuery.UI.DatePicker.min.js*
- *DatePickerReady.js*

O exemplo a seguir mostra o código real que você deve adicionar na parte inferior da `head` elemento o *exibições \ compartilhadas\\cshtml* arquivo.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample6.cshtml)]

Completo `head` seção é mostrada aqui:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample7.cshtml)]

O [auxiliar de URL conteúdo](https://msdn.microsoft.com/en-us/library/system.web.mvc.urlhelper.content.aspx) método converte o caminho do recurso em um caminho absoluto. Você deve usar `@URL.Content` para referenciar corretamente esses recursos quando o aplicativo está em execução no IIS.

Pressione CTRL+F5 para executar o aplicativo. Selecione um link de edição, coloque o ponto de inserção para o **ReleaseDate** campo. Calendário pop-up jQuery UI é exibido.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image7.png)

Como a maioria dos controles jQuery, de datepicker permite personalizá-la amplamente. Para obter informações, consulte [personalização Visual: Criando um tema de interface do usuário da jQuery](http://learn.jquery.com/jquery-ui/getting-started/#visual-customization-designing-a-jquery-ui-theme) no [jQuery UI](http://learn.jquery.com/jquery-ui/getting-started/) site.

### <a name="supporting-the-html5-date-input-control"></a>Suporte o controle de entrada de data HTML5

Como navegadores mais suportam a HTML5, você desejará usar o HTML5 nativo de entrada, como o `date` elemento de entrada e não usar o calendário de interface do usuário da jQuery. Você pode adicionar lógica ao seu aplicativo para usar controles HTML5 automaticamente se o navegador oferece suporte a eles. Para fazer isso, substitua o conteúdo do *DatePickerReady.js* arquivo com o seguinte:

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample8.js)]

A primeira linha do script usa Modernizr para verificar se a entrada de data HTML5 tem suporte. Se não for compatível, o seletor de data do jQuery da interface do usuário está conectado em vez disso. ([Modernizr](http://www.modernizr.com/docs/) é uma biblioteca de JavaScript do código-fonte aberto que detecta a disponibilidade de implementações nativas do HTML5 e CSS3. Modernizr é incluído em qualquer novos projetos do ASP.NET MVC que você criar.)

Depois de fazer essa alteração, você pode testá-lo usando um navegador que ofereça suporte a HTML5, como 11 Opera. Execute o aplicativo usando um navegador compatível com HTML5 e editar uma entrada de filme. O controle de data HTML5 é usado em vez de calendário pop-up jQuery UI:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/_static/image8.png)

Como novas versões de navegadores estiver implementando HTML5 incrementalmente, uma boa abordagem para agora é adicionar código ao seu site que acomoda uma ampla variedade de suporte a HTML5. Por exemplo, um mais robusto *DatePickerReady.js* script é mostrado abaixo que permite que os navegadores de suporte do site que suportam apenas parcialmente o controle de data HTML5.

[!code-javascript[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample9.js)]

Esse script seleciona HTML5 `input` elementos do tipo `date` que não suportam totalmente o controle de data HTML5. Para esses elementos, ele conecta calendário pop-up jQuery UI e, em seguida, altera o `type` de atributo de `date` para `text`. Alterando o `type` de atributo de `date` para `text`, suporte de data HTML5 parcial é eliminado. Ainda mais robustos *DatePickerReady.js* script pode ser encontrado em [JSFIDDLE](http://jsfiddle.net/XSTK8/15/).

### <a name="adding-nullable-dates-to-the-templates"></a>Adicionar datas permite valor nulas para os modelos

Se você usar um dos modelos de data existente e transmitir uma data nulo, você obterá um erro de tempo de execução. Para tornar os modelos de data mais robusta, você alterará para lidar com valores nulos. Para dar suporte a datas anuláveis, alterar o código de *Views\Shared\DisplayTemplates\DateTime.cshtml* para o seguinte:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample10.cshtml)]

O código retorna uma cadeia de caracteres vazia quando o modelo é **nulo**.

Alterar o código de *Views\Shared\EditorTemplates\Date.cshtml* arquivo para o seguinte:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4/samples/sample11.cshtml)]

Quando esse código é executado, se o modelo não for null, o modelo `DateTime` valor é usado. Se o modelo for nulo, a data atual será usada.

### <a name="wrapup"></a>Wrapup

Este tutorial abordou os fundamentos de auxiliares de modelo do ASP.NET e mostra como usar o calendário de pop-up jQuery UI datepicker em um aplicativo ASP.NET MVC. Para obter mais informações, tente estes recursos:

- Para obter informações sobre localização, consulte o blog do Rajeesh [JQueryUI Datepicker no ASP.NET MVC](http://www.rajeeshcv.com/2010/02/jqueryui-datepicker-in-asp-net-mvc/).
- Para obter informações sobre o jQuery UI, consulte [jQuery UI](http://docs.jquery.com/UI).
- Para obter informações sobre como localizar o controle de datepicker, consulte [Datepicker/interface de usuário/localização](http://docs.jquery.com/UI/Datepicker/Localization).
- Para obter mais informações sobre os modelos do ASP.NET MVC, consulte a série de blog de Brad Wilson em [modelos do ASP.NET MVC 2](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). Embora a série foi escrita para o ASP.NET MVC 2, o material ainda se aplicam para a versão atual do ASP.NET MVC.

>[!div class="step-by-step"]
[Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
