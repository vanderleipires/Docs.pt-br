---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
title: Usando HTML5 e jQuery UI Datepicker pop-up de calendário com ASP.NET MVC - parte 2 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de como trabalhar com modelos de editor, modelos de exibição e o calendário de pop-up jQuery UI datepicker em uma MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 21a178de-4c5a-4211-8a9c-74ec576c0f30
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2
msc.type: authoredcontent
ms.openlocfilehash: 84112316a9ace732cb7d75d7cbaeb071c72de822
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-2"></a>Usando HTML5 e jQuery UI Datepicker pop-up de calendário com ASP.NET MVC - parte 2
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial irá ensiná-lo os fundamentos de como trabalhar com modelos de editor, modelos de exibição e calendário pop-up jQuery UI datepicker em um aplicativo ASP.NET MVC.


## <a name="adding-an-automatic-datetime-template"></a>Adicionando um modelo de data e hora automáticas

A primeira parte deste tutorial, você viu como você pode adicionar atributos ao modelo para especificar explicitamente a formatação e como você pode especificar explicitamente o modelo que é usado para processar o modelo. Por exemplo, o [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo o explicitamente de código a seguir especifica a formatação de `ReleaseDate` propriedade.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample1.cs)]

No exemplo a seguir, o [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) de atributo, usando o `Date` enumeração, especifica que o modelo de data deve ser usado para renderizar o modelo. Se não houver nenhum modelo de data em seu projeto, o modelo de data interno é usado.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample2.cs)]

No entanto, ASP. MVC pode executar usando a convenção-over-configuração, procurando um modelo que corresponda ao nome de um tipo de correspondência de tipo. Isso permite que você crie um modelo que formata automaticamente os dados sem usar nenhum atributo ou o código em todos os. Para essa parte do tutorial, você criará um modelo que é aplicado automaticamente a propriedades de modelo do tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx). Você não precisará usar um atributo ou outra configuração para especificar que o modelo deve ser usado para processar todas as propriedades de modelo do tipo [DateTime](https://msdn.microsoft.com/library/system.datetime.aspx).

Você também aprenderá uma maneira de personalizar a exibição de campos ainda individuais ou propriedades individuais.

Para começar, vamos remover as informações de formatação e exibir datas completas no aplicativo.

Abra o *Movie.cs* de arquivo e comente a `DataType` atributo no `ReleaseDate` propriedade:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample3.cs)]

Pressione CTRL+F5 para executar o aplicativo.

Observe que o `ReleaseDate` propriedade agora exibe a data e a hora, porque esse é o padrão quando não há informações de formatação são fornecidas.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image1.png)

### <a name="adding-css-styles-for-testing-new-templates"></a>Adicionar estilos CSS para testar novos modelos

Antes de criar um modelo para a formatação de datas, você adicionará algumas regras de estilo CSS que você pode aplicar para os novos modelos. Eles ajudarão você a verificar se a página renderizada está usando o novo modelo.

Abra o *Content\Site.cs*s de arquivo e adicionar as seguintes regras CSS para o final do arquivo:

[!code-css[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample4.css)]

### <a name="adding-datetime-display-templates"></a>Adicionando modelos de exibição de data e hora

Agora você pode criar o novo modelo. No *Views\Movies* pasta, crie um *Modelosdeexibição* pasta.

No *exibições \ compartilhadas* pasta, crie um *Modelosdeexibição* pasta e um *Modelosdoeditor* pasta.

Os modelos de exibição no *\ compartilhadas \ modelosdeexibição* pasta será usada por todos os controladores. Os modelos de exibição no *Views\Movie\DisplayTemplates* pasta será usada somente pelo `Movie` controlador. (Se um modelo com o mesmo nome é exibido em ambas as pastas, o modelo no *Views\Movie\DisplayTemplates* pasta — ou seja, o modelo mais específico — tem precedência para exibições retornadas pelo `Movie` controlador.)

Em **Solution Explorer**, expanda o *exibições* pasta, expanda o *compartilhado* pasta e, em seguida, clique com botão direito a *\ compartilhadas \ modelosdeexibição* pasta.

Clique em **adicionar** e, em seguida, clique em **exibição**. O **adicionar exibição** caixa de diálogo é exibida.

No **nome de exibição** , digite `DateTime`. (Você deve usar esse nome para corresponder ao nome do tipo).

Selecione o **criar como uma exibição parcial** caixa de seleção. Verifique se o **usar uma layout ou página mestra** e **criar uma exibição fortemente tipada** caixas de seleção não estiver selecionadas.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image2.png)

Clique em **Adicionar**. Um *DateTime.cshtml* modelo for criado no *\ compartilhadas \ modelosdeexibição*.

A imagem a seguir mostra o *exibições* pasta **Gerenciador de soluções** depois que o `DateTime` exibição e o editor de modelos são criados.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image3.png)

Abra o *Views\Shared\DisplayTemplates\DateTime.cshtml* de arquivo e adicione a seguinte marcação, que usa o [Format](https://msdn.microsoft.com/library/system.string.format.aspx) método para formatar a propriedade como uma data sem o tempo. (O `{0:d}` formato Especifica o formato de data abreviada.)

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample5.cs)]

Repita essa etapa para criar um `DateTime` modelo o *Views\Movie\DisplayTemplates* pasta. Use o seguinte código no *Views\Movie\DisplayTemplates\DateTime.cshtml* arquivo.

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample6.cs)]

O `loud-1` classe CSS faz com que a data a ser exibido em negrito vermelho. Você adicionou o `loud-1` classe CSS apenas como uma medida temporária para você possa ver facilmente quando esse modelo específico está sendo usado.

O que fazer é criado e os modelos que o ASP.NET usará para exibir datas personalizados. O modelo mais geral (no *\ compartilhadas \ modelosdeexibição* pasta) exibe uma data abreviada simple. O modelo que é específico para o `Movie` controlador (no *Views\Movies\DisplayTemplates* pasta) exibe uma data abreviada que também é formatada como negrito vermelho.

Pressione CTRL+F5 para executar o aplicativo. O navegador renderiza a exibição do índice para o aplicativo.

O `ReleaseDate` propriedade agora exibe a data em uma fonte em negrito vermelha sem o tempo. Isso ajuda você a confirmar que o `DateTime` modelado auxiliar no *Views\Movies\DisplayTemplates* pasta está selecionada no `DateTime` modelado auxiliar na pasta compartilhada (*Views\Shared\ Modelosdeexibição*).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image4.png)

Agora renomear o *Views\Movies\DisplayTemplates\DateTime.cshtml* o arquivo para *Views\Movies\DisplayTemplates\LoudDateTime.cshtml*.

Pressione CTRL+F5 para executar o aplicativo.

Neste momento o `ReleaseDate` propriedade exibe uma data sem o tempo e sem o negrito vermelho. Isso ilustra que o tipo de um modelo que tenha o nome dos dados (nesse caso `DateTime`) é automaticamente usado para exibir todas as propriedades de modelo desse tipo. Depois que você renomeou o *DateTime.cshtml* o arquivo para *LoudDateTime.cshtml*, ASP.NET não encontrar um modelo no *Views\Movies\DisplayTemplates* pasta, para que ele usado o *DateTime.cshtml* modelo a partir de * Views\Movies\Shared\* pasta.

(O modelo de correspondência diferencia maiusculas de minúsculas, portanto, você poderia ter criado o nome do arquivo de modelo com qualquer uso de maiusculas e minúsculas. Por exemplo, *DATETIME.chstml, datetime.cshtml*, e *DaTeTiMe.cshtml* todos corresponderia a `DateTime` tipo.)

Para revisar: neste ponto, o `ReleaseDate` campo está sendo exibido usando o *Views\Movies\DisplayTemplates\DateTime.cshtml* modelo, que exibe os dados usando um formato de data abreviada, mas, caso contrário, não adiciona nenhuma formatação especial.

### <a name="using-uihint-to-specify-a-display-template"></a>Usando UIHint para especificar um modelo de exibição

Se seu aplicativo da web tem muitas `DateTime` campos e, por padrão, você deseja exibir todos ou a maioria no formato de data, o *DateTime.cshtml* modelo é uma boa abordagem. Mas e se você tem algumas datas em que você deseja exibir a data e hora completas? Sem problemas. Você pode criar um modelo adicional e use o [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo para especificar a formatação para a data e hora completas. Você pode aplicar seletivamente desse modelo. Você pode usar o [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo no nível do modelo, ou você pode especificar o modelo de dentro de um modo de exibição. Nesta seção, você verá como usar o `UIHint` atributo seletivamente alterar a formatação para algumas instâncias de campos de data e hora.

Abra o *Views\Movies\DisplayTemplates\LoudDateTime.cshtml* de arquivo e substitua o código existente pelo seguinte:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample7.cshtml)]

Isso faz com que a data completa e a hora a ser exibido e adiciona a classe CSS que faz com que o texto verde e grande.

Abra o *Movie.cs* e adicione o [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) de atributo para o `ReleaseDate` propriedade, conforme mostrado no exemplo a seguir:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample8.cs)]

Isso informa ao ASP.NET MVC que quando ele for exibido o `ReleaseDate` propriedade (especificamente e não qualquer `DateTime` objeto), ele deve usar o *LoudDateTime.cshtml* modelo.

Pressione CTRL+F5 para executar o aplicativo.

Observe que o `ReleaseDate` propriedade agora exibe a data e hora em uma fonte grande de verde.

Retornar para o `UIHint` atributo no *Movie.cs* de arquivo e comente-o para que o *LoudDateTime.cshtml* modelo não será usado. Execute o aplicativo novamente. A data de liberação não é exibida grande e verde. Isso verifica se o *Views\Shared\DisplayTemplates\DateTime.cshtml* modelo é usado nas exibições de detalhes e de índice.

Como mencionado anteriormente, você também pode aplicar um modelo em uma exibição, que permite que você aplicar o modelo a uma instância individual de alguns dados. Abra o *Views\Movies\Details.cshtml* exibição. Adicionar `"LoudDateTime"` como o segundo parâmetro do [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) pedir a `ReleaseDate` campo. O código completo tem esta aparência:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/samples/sample9.cshtml)]

Especifica que o `LoudDateTime` modelo deve ser usado para exibir a propriedade de modelo, independentemente de quais atributos são aplicados ao modelo.

Pressione CTRL+F5 para executar o aplicativo.

Verifique se a página de índice de filme é usando o *Views\Shared\DisplayTemplates\DateTime.cshtml* modelo (negrito vermelho) e o *Movie\Details* página está usando o *Views\Movies\ DisplayTemplates\LoudDateTime.cshtml* modelo (grande e verde).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2/_static/image5.png)

Na próxima seção, você criará um modelo para um tipo complexo.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1.md)
> [Próximo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3.md)
