---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
title: Usando HTML5 e jQuery UI Datepicker pop-up de calendário com ASP.NET MVC - parte 3 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de como trabalhar com modelos de editor, modelos de exibição e o calendário de pop-up jQuery UI datepicker em uma MV ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/29/2011
ms.topic: article
ms.assetid: 8f5f91ae-12d7-4cf3-ac09-4bb53d07ee60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3
msc.type: authoredcontent
ms.openlocfilehash: fd1ae746f4f134b779c7eee50cf6c840bbb7068e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-3"></a>Usando HTML5 e jQuery UI Datepicker pop-up de calendário com ASP.NET MVC - parte 3
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial irá ensiná-lo os fundamentos de como trabalhar com modelos de editor, modelos de exibição e calendário pop-up jQuery UI datepicker em um aplicativo ASP.NET MVC.


## <a name="working-with-complex-types"></a>Trabalhando com tipos complexos

Nesta seção você criará uma classe de endereço e saiba como criar um modelo para exibi-la.

No *modelos* pasta, crie um novo arquivo de classe chamado *Person.cs* onde você colocará dois tipos: um `Person` classe e um `Address` classe. O `Person` classe conterá uma propriedade é digitada como `Address`. O `Address` é um tipo complexo, que significa que não é um dos tipos internos como `int`, `string`, ou `double`. Em vez disso, ele tem várias propriedades. O código para as novas classes tem esta aparência:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample1.cs)]

No `Movie` controlador, adicione o seguinte `PersonDetail` ação para exibir uma instância de pessoa:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample2.cs)]

Em seguida, adicione o seguinte código para o `Movie` controlador para preencher o `Person` modelo com alguns dados de exemplo:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample3.cs)]

Abra o *Views\Movies\PersonDetail.cshtml* de arquivo e adicione a seguinte marcação para o `PersonDetail` exibição.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample4.cshtml)]

Pressione Ctrl + F5 para executar o aplicativo e navegue até *filmes/PersonDetail*.

O `PersonDetail` exibição não contiver o `Address` tipo complexo, como você pode ver nesta captura de tela. (Nenhum endereço é exibido.)

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image1.png)

O `Address` dados de modelo não são exibidos porque ele é um tipo complexo. Para exibir as informações de endereço, abra o *Views\Movies\PersonDetail.cshtml* novamente e adicione a seguinte marcação.

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample5.cshtml)]

A marcação concluída para o `PersonDetail` agora exibição tem esta aparência:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample6.cshtml)]

Execute novamente o aplicativo e exibir o `PersonDetail` exibição. As informações de endereço agora são exibidas:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image2.png)

### <a name="creating-a-template-for-a-complex-type"></a>Criando um modelo para um tipo complexo

Nesta seção, você criará um modelo que será usado para renderizar o `Address` tipo complexo. Quando você cria um modelo para o `Address` tipo, ASP.NET MVC pode automaticamente ser usado para formatar um modelo de endereço em qualquer lugar no aplicativo. Isso fornece uma maneira de controlar o processamento do `Address` tipo de um só lugar no aplicativo.

No *\ compartilhadas \ modelosdeexibição* pasta, criar uma exibição parcial fortemente tipada denominada **endereço**:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image3.png)

Clique em **adicionar**e, em seguida, abra o novo *Views\Shared\DisplayTemplates\Address.cshtml* arquivo. O novo modo de exibição contém a seguinte marcação gerada:

[!code-cshtml[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample7.cshtml)]

Execute o aplicativo e exibir o `PersonDetail` exibição. Neste momento, o `Address` modelo que você acabou de criar é usado para exibir o `Address` tipo complexo, para que a exibição é semelhante ao seguinte:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/_static/image4.png)

### <a name="summary-ways-to-specify-the-model-display-format-and-template"></a>Resumo: Maneiras para especificar o formato de exibição do modelo e o modelo

Você viu que você pode especificar o formato ou o modelo para uma propriedade de modelo usando as seguintes abordagens:

- Aplicar o `DisplayFormat` atributo a uma propriedade no modelo. Por exemplo, o código a seguir faz com que a data a ser exibida sem o tempo:

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample8.cs)]
- Aplicando um [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) atributo a uma propriedade no modelo e especificar o tipo de dados. Por exemplo, o código a seguir faz com que a data a ser exibida sem o tempo.

    [!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-3/samples/sample9.cs)]

    Se o aplicativo contém um *date.cshtml* modelo o *\ compartilhadas \ modelosdeexibição* pasta ou o *Views\Movies\DisplayTemplates* pasta, modelo será usado para renderizar o `DateTime` propriedade. Caso contrário, o sistema de modelos interno do ASP.NET exibe a propriedade como uma data.
- Criando um modelo de exibição no *\ compartilhadas \ modelosdeexibição* pasta ou o *Views\Movies\DisplayTemplates* pasta cujo nome corresponde ao tipo de dados que você deseja formatar. Por exemplo, você viu que o *Views\Shared\DisplayTemplates\DateTime.cshtml* foi usado para renderizar `DateTime` propriedades em um modelo, sem adicionar um atributo para o modelo e sem adicionar qualquer marcação a modos de exibição.
- Usando o [UIHint](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.uihintattribute.uihint.aspx) atributo no modelo para especificar o modelo para exibir a propriedade de modelo.
- Adicionar explicitamente o nome do modelo de exibição para o [Html.DisplayFor](https://msdn.microsoft.com/library/ee407420.aspx) chamada em um modo de exibição.

A abordagem usada depende do que você precisa fazer em seu aplicativo. Não é incomum combinar essas abordagens para obter exatamente o tipo de formatação que você precisa.

Na próxima seção, você alternará marcha um pouco e mover-se de personalizar como os dados são exibidos para personalizar como ele é inserido. Você vai conectar de datepicker jQuery para os modos de exibição de edição no aplicativo para fornecer uma ótima maneira de especificar as datas.

> [!div class="step-by-step"]
> [Anterior](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
> [Próximo](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-4.md)
