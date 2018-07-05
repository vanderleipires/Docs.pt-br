---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Usando HTML5 e calendário jQuery UI Datepicker pop-up com o ASP.NET MVC – parte 3 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os fundamentos de como trabalhar com modelos do editor, modelos de exibição e o calendário de pop-up jQuery UI datepicker em uma MV do ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: 11f9e0a8602e9ee1feda9d0e7d0d319add7c440c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400765"
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Usando HTML5 e calendário jQuery UI Datepicker pop-up com o ASP.NET MVC – parte 3
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial ensinará os fundamentos de como trabalhar com modelos do editor, modelos de exibição e calendário pop-up jQuery UI datepicker em um aplicativo Web ASP.NET MVC.


## <a name="working-with-complex-types"></a>Trabalhando com tipos complexos

Nesta seção você criará uma classe de endereço e saiba como criar um modelo para exibi-lo.

No *modelos* pasta, crie um novo arquivo de classe chamado *Person.cs* onde você colocará dois tipos: uma `Person` classe e um `Address` classe. O `Person` classe conterá uma propriedade que é digitada como `Address`. O `Address` tipo é um tipo complexo, que significa que ele não é um dos tipos internos, como `int`, `string`, ou `double`. Em vez disso, ele tem várias propriedades. O código para as novas classes tem esta aparência:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

No `Movie` controlador, adicione o seguinte `PersonDetail` ação para exibir uma instância de pessoa:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Em seguida, adicione o seguinte código para o `Movie` controlador para preencher o `Person` modelo com alguns dados de exemplo:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Abra o *Views\Movies\PersonDetail.cshtml* arquivo e adicione a seguinte marcação para o `PersonDetail` modo de exibição.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Pressione Ctrl + F5 para executar o aplicativo e navegue até *filmes/PersonDetail*.

O `PersonDetail` modo de exibição não contiver o `Address` tipo complexo, como você pode ver nessa captura de tela. (Nenhum endereço é mostrado).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

O `Address` dados de modelo não são exibidos porque ele é um tipo complexo. Para exibir as informações de endereço, abra o *Views\Movies\PersonDetail.cshtml* novamente e adicione a seguinte marcação.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

A marcação completa para o `PersonDetail` agora exibição tenha esta aparência:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Executar o aplicativo novamente e exibir o `PersonDetail` modo de exibição. As informações de endereço agora são exibidas:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Criar um modelo para um tipo complexo

Nesta seção, você criará um modelo que será usado para renderizar o `Address` tipo complexo. Quando você cria um modelo para o `Address` tipo, ASP.NET MVC pode automaticamente ser usado para formatar um modelo de endereço em qualquer lugar no aplicativo. Isso lhe dá uma maneira de controlar o processamento do `Address` tipo de um só lugar no aplicativo.

No *\ compartilhadas \ modelosdeexibição* pasta, crie uma exibição parcial com rigidez de tipos nomeada **endereço**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Clique em **Add**e, em seguida, abra o novo *Views\Shared\DisplayTemplates\Address.cshtml* arquivo. O novo modo de exibição contém a marcação gerada a seguir:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Executar o aplicativo e exibir o `PersonDetail` modo de exibição. Neste momento, o `Address` modelo que você acabou de criar é usado para exibir o `Address` tipo complexo, portanto, a exibição é semelhante ao seguinte:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Resumo: Maneiras de especificar o formato de exibição do modelo e o modelo

Você já viu que você pode especificar o formato ou o modelo para uma propriedade de modelo usando as seguintes abordagens:

- Aplicando o `DisplayFormat` de atributo a uma propriedade no modelo. Por exemplo, o código a seguir faz com que a data a ser exibida sem o tempo:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Aplicação de um [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo a uma propriedade no modelo e especificar o tipo de dados. Por exemplo, o código a seguir faz com que a data a ser exibida sem o tempo.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Se o aplicativo contém um *date.cshtml* modelo na *\ compartilhadas \ modelosdeexibição* pasta ou o *Views\Movies\DisplayTemplates* pasta, modelo será usado para renderizar o `DateTime` propriedade. Caso contrário, o sistema de modelos internos do ASP.NET exibe a propriedade como uma data.
- Criando um modelo de exibição na *\ compartilhadas \ modelosdeexibição* pasta ou o *Views\Movies\DisplayTemplates* pasta cujo nome corresponde ao tipo de dados que você deseja formatar. Por exemplo, você viu que o *Views\Shared\DisplayTemplates\DateTime.cshtml* foi usado para renderizar `DateTime` propriedades em um modelo, sem adicionar um atributo para o modelo e sem adicionar qualquer marcação para modos de exibição.
- Usando o [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo no modelo para especificar o modelo para exibir a propriedade de modelo.
- Adicionar explicitamente o nome do modelo de exibição para o [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) chamar em um modo de exibição.

A abordagem usada depende do que você precisa fazer em seu aplicativo. Não é incomum combinar essas abordagens para obter exatamente o tipo de formatação que você precisa.

Na próxima seção, você vai mudar a marcha um pouco e mover de personalizar como os dados são exibidos para personalizar como está inserido. Vou ligar o datepicker do jQuery para os modos de exibição de edição no aplicativo para fornecer uma ótima maneira de especificar as datas.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Próximo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
