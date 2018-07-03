---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Usando HTML5 e calendário jQuery UI Datepicker pop-up com o ASP.NET MVC – parte 2 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os fundamentos de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up jQuery UI datepicker em uma MV do ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 2818e69f912509a6392333bad8f5a1afa55884f6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375283"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Usando HTML5 e calendário jQuery UI Datepicker pop-up com o ASP.NET MVC – parte 2
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial ensinará os fundamentos de como trabalhar com modelos do editor, modelos de exibição e calendário pop-up jQuery UI datepicker em um aplicativo Web ASP.NET MVC.


## <a name="adding-an-automatic-datetime-template"></a>Adicionando um modelo de data e hora automática

Na primeira parte deste tutorial, você viu como você pode adicionar atributos ao modelo para especificar explicitamente a formatação, e como você pode especificar explicitamente o modelo que é usado para renderizar o modelo. Por exemplo, o [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo no explicitamente de código a seguir especifica a formatação para o `ReleaseDate` propriedade.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

No exemplo a seguir, o [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) de atributo, usando o `Date` enumeração, especifica que o modelo de data deve ser usado para renderizar o modelo. Se não houver nenhum modelo de data em seu projeto, o modelo de data interna é usado.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

No entanto, ASP. MVC pode executar usando a convenção em detrimento da configuração, procurando por um modelo que corresponda ao nome de um tipo de correspondência de tipo. Isso permite que você crie um modelo que formata automaticamente os dados sem usar qualquer atributo ou o código em todos os. Para essa parte do tutorial, você criará um modelo que é aplicado automaticamente às propriedades de modelo do tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Você não precisará usar um atributo ou outra configuração para especificar que o modelo deve ser usado para processar todas as propriedades de modelo do tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Você também aprenderá uma maneira de personalizar a exibição de propriedades individuais ou campos até mesmo individuais.

Para começar, vamos remover as informações de formatação e exibir datas completas no aplicativo.

Abra o *Movie.cs* de arquivo e comente a `DataType` atributo o `ReleaseDate` propriedade:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Pressione CTRL+F5 para executar o aplicativo.

Observe que o `ReleaseDate` propriedade agora exibe a data e a hora, porque esse é o padrão quando nenhuma informação de formatação é fornecida.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Adicionar estilos CSS para testar novos modelos

Antes de criar um modelo para a formatação de datas, você adicionará algumas regras de estilo CSS que você pode aplicar aos novos modelos. Eles ajudarão a verificar se a página renderizada está usando o novo modelo.

Abra o *Content\Site.cs*s de arquivo e adicione as seguintes regras CSS na parte inferior do arquivo:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Adicionando modelos de exibição de data e hora

Agora você pode criar o novo modelo. No *exibições \ filmes* pasta, crie um *Modelosdeexibição* pasta.

No *Views\Shared* pasta, crie um *Modelosdeexibição* pasta e um *Modelosdoeditor* pasta.

Os modelos de exibição na *\ compartilhadas \ modelosdeexibição* pasta será usada por todos os controladores. Os modelos de exibição na *Views\Movie\DisplayTemplates* pasta será usado somente pelo `Movie` controlador. (Se um modelo com o mesmo nome aparece em ambas as pastas, o modelo na *Views\Movie\DisplayTemplates* pasta — ou seja, o modelo mais específico — tem precedência para exibições retornado pelo `Movie` controlador.)

Na **Gerenciador de soluções**, expanda o *exibições* pasta, expanda o *compartilhado* pasta e, em seguida, clique com botão direito a *\ compartilhadas \ modelosdeexibição* pasta.

Clique em **Add** e, em seguida, clique em **exibição**. O **adicionar exibição** caixa de diálogo é exibida.

No **nome da exibição** , digite `DateTime`. (Você deve usar esse nome para corresponder ao nome do tipo).

Selecione o **criar como uma exibição parcial** caixa de seleção. Certifique-se de que o **usar uma layout ou página mestra** e **criar uma exibição fortemente tipada** caixas de seleção não estiver selecionadas.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Clique em **Adicionar**. Um *DateTime* modelo for criado na *\ compartilhadas \ modelosdeexibição*.

A imagem a seguir mostra o *modos de exibição* pasta **Gerenciador de soluções** depois que o `DateTime` exibição e o editor de modelos são criados.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Abra o *Views\Shared\DisplayTemplates\DateTime.cshtml* arquivo e adicione a seguinte marcação, que usa o [String. Format](https://msdn.microsoft.com/library/system.string.format.aspx) método para formatar a propriedade como uma data sem o tempo. (O `{0:d}` formato Especifica o formato de data abreviada.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Repita essa etapa para criar uma `DateTime` modelo de *Views\Movie\DisplayTemplates* pasta. Use o seguinte código na *Views\Movie\DisplayTemplates\DateTime.cshtml* arquivo.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

O `loud-1` classe CSS faz com que a data a ser exibida no texto em negrito vermelho. Você adicionou o `loud-1` classe CSS apenas como uma medida temporária para que você possa ver facilmente quando é usado neste modelo específico.

O que foi feito é criado e os modelos ASP.NET a ser usado para exibir datas personalizados. O modelo mais geral (na *\ compartilhadas \ modelosdeexibição* pasta) exibe uma simple data abreviada. O modelo que é especificamente para o `Movie` controlador (em de *Views\Movies\DisplayTemplates* pasta) exibe uma data abreviada que também está formatada como texto em negrito vermelho.

Pressione CTRL+F5 para executar o aplicativo. O navegador renderiza a exibição de índice para o aplicativo.

O `ReleaseDate` propriedade agora exibe a data em uma fonte em negrito vermelha sem o tempo. Isso ajuda você a confirmar que o `DateTime` auxiliar de modelo na *Views\Movies\DisplayTemplates* pasta tem preferência sobre o `DateTime` auxiliar de modelo na pasta compartilhada (*Views\Shared\ Modelosdeexibição*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Renomear agora o *Views\Movies\DisplayTemplates\DateTime.cshtml* arquivo *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Pressione CTRL+F5 para executar o aplicativo.

Desta vez o `ReleaseDate` propriedade exibe uma data sem o tempo e sem a fonte vermelha em negrito. Isso ilustra que um modelo que tem o nome dos dados de tipo (nesse caso, `DateTime`) é usada automaticamente para exibir todas as propriedades de modelo desse tipo. Depois que você renomeou o *DateTime* arquivo *LoudDateTime.cshtml*, ASP.NET não será mais encontrado um modelo no *Views\Movies\DisplayTemplates* pasta, para que ele usado o *DateTime* modelo a partir de * Views\Movies\Shared\* pasta.

(O modelo correspondente é diferencia maiusculas de minúsculas, portanto, você poderia ter criado o nome do arquivo de modelo com qualquer uso de maiusculas e minúsculas. Por exemplo, *DATETIME.chstml, DateTime*, e *DateTime* tudo corresponderia a `DateTime` tipo.)

Para examinar: neste ponto, o `ReleaseDate` campo está sendo exibido usando o *Views\Movies\DisplayTemplates\DateTime.cshtml* modelo, que exibe os dados usando um formato de data abreviada, mas caso contrário, não adiciona nenhum formato especial.

### <a name="using-uihint-to-specify-a-display-template"></a>Usando o UIHint para especificar um modelo de exibição

Se seu aplicativo web tem muitas `DateTime` campos e, por padrão, você deseja exibir todos ou a maioria no formato de data, o *DateTime* modelo é uma boa abordagem. Mas e se você tiver algumas datas em que você deseja exibir a data e hora completas? Sem problemas. Você pode criar um modelo adicional e usar o [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo para especificar a formatação para a data e hora completas. Em seguida, você poderá aplicar seletivamente esse modelo. Você pode usar o [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo no nível do modelo ou você pode especificar o modelo dentro de um modo de exibição. Nesta seção, você verá como usar o `UIHint` atributo alterar seletivamente a formatação de algumas instâncias de campos de data e hora.

Abra o *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* de arquivo e substitua o código existente pelo seguinte:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Isso faz com que a data completa e a hora a ser exibido e adiciona a classe CSS que faz com que o texto verde e grandes.

Abra o *Movie.cs* arquivo e adicione o [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo para o `ReleaseDate` propriedade, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Isso informa ao ASP.NET MVC que quando ela for exibida a `ReleaseDate` propriedade (especificamente e não apenas a qualquer `DateTime` objeto), ele deve usar o *LoudDateTime.cshtml* modelo.

Pressione CTRL+F5 para executar o aplicativo.

Observe que o `ReleaseDate` propriedade agora exibe a data e hora em uma grande fonte verde.

Volte para o `UIHint` de atributo na *Movie.cs* do arquivo e comente-o para que o *LoudDateTime.cshtml* modelo não será usado. Execute o aplicativo novamente. A data de lançamento não é exibida, grandes e verde. Isso verifica se o *Views\Shared\DisplayTemplates\DateTime.cshtml* modelo é usado nos modos de exibição Index e Details.

Como mencionado anteriormente, você também pode aplicar um modelo em uma exibição, que permite que você aplique o modelo a uma instância individual de alguns dados. Abra o *Views\Movies\Details.cshtml* exibição. Adicione `"LoudDateTime"` como o segundo parâmetro do [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) chamar para o `ReleaseDate` campo. O código completo tem esta aparência:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Isso especifica que o `LoudDateTime` modelo deve ser usado para exibir a propriedade de modelo, independentemente de quais atributos são aplicados ao modelo.

Pressione CTRL+F5 para executar o aplicativo.

Verifique se a página de índice de filme é usando o *Views\Shared\DisplayTemplates\DateTime.cshtml* modelo (em negrito vermelho) e o *Movie\Details* página está usando o *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* modelo (grande e verde).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Na próxima seção, você criará um modelo para um tipo complexo.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Próximo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
