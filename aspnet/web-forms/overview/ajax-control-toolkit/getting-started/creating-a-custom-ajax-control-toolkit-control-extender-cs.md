---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Criando personalizado do AJAX controle de extensor Toolkit (c#) | Microsoft Docs
author: microsoft
description: Extensores personalizados permitem personalizar e estender os recursos dos controles do ASP.NET sem precisar criar novas classes.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 0dd243ec1709324174ff48b52e7dfb5185d3aaa2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390947"
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Criar um personalizado extensor AJAX Control Toolkit (c#)
====================
por [Microsoft](https://github.com/microsoft)

> Extensores personalizados permitem personalizar e estender os recursos dos controles do ASP.NET sem precisar criar novas classes.


Neste tutorial, você aprenderá como criar um extensor de controle personalizado do AJAX Control Toolkit. Podemos criar um simples, mas útil, novo extensor que altera o estado de um botão de desabilitado para habilitado quando você digita texto em uma caixa de texto. Depois de ler este tutorial, você poderá estender o ASP.NET AJAX Toolkit com seus próprio extensores de controle.

Você pode criar extensores de controle personalizado usando o Visual Studio ou Visual Web Developer (certifique-se de que você tenha a versão mais recente do Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Visão geral do extensor DisabledButton

O extensor DisabledButton é chamado de nosso novo extensor de controle. Desse extensor terá três propriedades:

- TargetControlID - caixa de texto que estende o controle.
- TargetButtonIID - o botão que é habilitado ou desabilitado.
- DisabledText - o texto que é exibido inicialmente no botão. Quando você começa a digitar, o botão exibe o valor da propriedade de texto do botão.

Você vincular o extensor DisabledButton a um controle TextBox e Button. Antes de digitar qualquer texto, o botão está desabilitado e a caixa de texto e o botão ter esta aparência:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))


Depois de começar a digitar texto, o botão está habilitado e a caixa de texto e o botão ter esta aparência:


[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))


Para criar nosso extensor de controle, é preciso criar três arquivos a seguir:

- DisabledButtonExtender.cs - esse arquivo é a classe de controle de servidor que gerenciará a criação de seu extensor e permitem que você defina as propriedades em tempo de design. Ele também define as propriedades que podem ser definidas em seu extensor. Essas propriedades são acessíveis por meio de código e em tempo de design e corresponde às propriedades definidas no arquivo DisableButtonBehavior.js.
- DisabledButtonBehavior.js – Esse arquivo é onde você adicionará todos de sua lógica de script de cliente.
- DisabledButtonDesigner.cs - essa classe habilita a funcionalidade de tempo de design. Se você quiser que o extensor do controle para funcionar corretamente com o Designer do Visual Studio/Visual Web Developer, você precisa dessa classe.

Portanto, um extensor de controle consiste em um controle de servidor, um comportamento do lado do cliente e uma classe de designer do lado do servidor. Você aprenderá a criar todos os três desses arquivos nas seções a seguir.

## <a name="creating-the-custom-extender-website-and-project"></a>Criando o projeto e o site do extensor personalizado

A primeira etapa é criar um site da Web e um projeto de biblioteca de classes no Visual Studio/Visual Web Developer. Podemos ll criar o extensor personalizado no projeto de biblioteca de classes e testar o extensor personalizado no site.

Permitir que o s comece com o site. Siga estas etapas para criar o site:

1. Selecione a opção de menu **arquivo, novo Site da Web**.
2. Selecione o **Site da Web ASP.NET** modelo.
3. Nomeie o novo site *Website1*.
4. Clique o **Okey** botão.

Em seguida, precisamos criar o projeto de biblioteca de classes que contém o código para o extensor do controle:

1. Selecione a opção de menu **novo projeto do arquivo, adicionar,**.
2. Selecione o **biblioteca de classes** modelo.
3. Nomeie a nova biblioteca de classes com o nome **CustomExtenders**.
4. Clique o **Okey** botão.

Depois de concluir essas etapas, a janela do Gerenciador de soluções deve ser semelhante a Figura 1.


[![Solução de projeto de biblioteca de classe e de site](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Figura 01**: a solução com o projeto de biblioteca de classe e de site ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))


Em seguida, você precisará adicionar todas as referências de assembly necessárias para o projeto de biblioteca de classes:

1. Clique com botão direito no projeto CustomExtenders e selecione a opção de menu **adicionar referência**.
2. Selecione a guia do .NET.
3. Adicione referências aos assemblies a seguir:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Selecione a guia Browse.
5. Adicione uma referência ao assembly AjaxControlToolkit. Esse assembly está localizado na pasta onde você baixou o AJAX Control Toolkit.

Depois de concluir essas etapas, sua pasta de referências de projeto de biblioteca de classes deve ser semelhante a Figura 2.


[![Pasta de referências com as referências necessárias](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Figura 02**: a pasta de referências com as referências necessárias ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Criando o extensor do controle personalizado

Agora que temos nossa biblioteca de classes, podemos começar a criação de nosso controle de extensor. Permitir que o s comece com o esqueleto de uma classe de controle de extensor personalizado (consulte a listagem 1).

**Listagem 1 - MyCustomExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Há várias coisas que você observar sobre a classe de controle de extensor na listagem 1. Primeiro, observe que a classe herda da classe base ExtenderControlBase. Todos os controles de extensor do AJAX Control Toolkit derivam dessa classe base. Por exemplo, a classe base inclui a propriedade TargetID que é uma propriedade obrigatória do extensor cada controle.

Em seguida, observe que a classe inclui os seguintes dois atributos relacionados ao script de cliente:

- WebResource - faz com que um arquivo a ser incluído como um recurso inserido em um assembly.
- ClientScriptResource - faz com que um recurso de script a ser recuperado de um assembly.

O atributo WebResource é usado para inserir o arquivo MyControlBehavior.js JavaScript no assembly quando o extensor personalizado é compilado. O atributo ClientScriptResource é usado para recuperar o script MyControlBehavior.js do assembly quando o extensor personalizado é usado em uma página da web.


Em ordem para os atributos WebResource e ClientScriptResource funcione, você deve compilar o arquivo JavaScript como um recurso inserido. Selecione o arquivo na janela do Gerenciador de soluções, abra a folha de propriedades e atribua o valor *Embedded Resource* para o **Build Action** propriedade.


Observe que o extensor do controle também inclui um atributo TargetControlType. Esse atributo é usado para especificar o tipo de controle que será estendido para o extensor do controle. No caso da listagem 1, o extensor do controle é usado para estender uma caixa de texto.

Finalmente, observe que o extensor personalizado inclui uma propriedade chamada MyProperty. A propriedade é marcada com o atributo ExtenderControlProperty. Os métodos GetPropertyValue() e SetPropertyValue() são usados para passar o valor da propriedade do extensor do controle do lado do servidor para o comportamento do lado do cliente.

Vá em frente e implementar o código para nosso extensor DisabledButton, permitem que s. O código para este extensor pode ser encontrado na listagem 2.

**Listagem 2 - DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

O extensor DisabledButton na listagem 2 tem duas propriedades chamadas TargetButtonID e DisabledText. O IDReferenceProperty aplicado à propriedade TargetButtonID impede que você atribuir qualquer coisa diferente da ID de um controle de botão para essa propriedade.

Os atributos WebResource e ClientScriptResource associar um comportamento do lado do cliente localizado em um arquivo chamado DisabledButtonBehavior.js com desse extensor. Discutiremos este arquivo JavaScript na próxima seção.

## <a name="creating-the-custom-extender-behavior"></a>Criando o comportamento de extensor personalizado

O componente do lado do cliente de um extensor de controle é chamado um comportamento. A lógica real para desabilitar e habilitar o botão está contida no comportamento DisabledButton. O código de JavaScript para o comportamento está incluído na listagem 3.

**Listagem 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

O arquivo JavaScript na listagem 3 contém uma classe de cliente chamada DisabledButtonBehavior. Essa classe, como seu gêmeo do lado do servidor, inclui duas propriedades chamadas TargetButtonID e DisabledText que você pode acessar usando obter\_TargetButtonID/set\_TargetButtonID e obtenha\_DisabledText/set\_ DisabledText.

O método Initialize () associa um manipulador de evento keyup com o elemento de destino para o comportamento. Cada vez que você digitar uma letra na caixa de texto associada a esse comportamento, o manipulador de keyup executa. O manipulador de keyup habilita ou desabilita o botão dependendo se a caixa de texto associada com o comportamento contém qualquer texto.

Lembre-se de que você deve compilar o arquivo JavaScript na listagem 3 como um recurso inserido. Selecione o arquivo na janela do Gerenciador de soluções, abra a folha de propriedades e atribua o valor *Embedded Resource* para o **Build Action** propriedade (veja a Figura 3). Essa opção está disponível no Visual Studio e Visual Web Developer.


[![Adicionando um arquivo JavaScript como um recurso inserido](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Figura 03**: adicionando um arquivo JavaScript como um recurso inserido ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Criando o Designer personalizado de extensão

Há uma última classe precisamos criar para concluir nosso extensor. Precisamos criar a classe de designer na listagem 4. Essa classe é necessária para tornar o extensor se comportar corretamente com o Designer do Visual Studio/Visual Web Developer.

**Listagem 4 - DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Você associa o designer na listagem 4 com o extensor DisabledButton com o atributo de Designer. Você precisa aplicar o atributo de Designer para a classe DisabledButtonExtender como este:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>Usar o extensor personalizado

Agora que podemos terminar de criar o extensor do controle DisabledButton, é hora de usá-lo em nosso site do ASP.NET. Primeiro, precisamos adicionar o extensor personalizado à caixa de ferramentas. Siga estas etapas:

1. Abra uma página ASP.NET clicando duas vezes a página na janela do Gerenciador de soluções.
2. A caixa de ferramentas com o botão direito e selecione a opção de menu **escolher itens**.
3. Na caixa de diálogo Escolher itens da caixa de ferramentas, navegue até o assembly CustomExtenders.dll.
4. Clique o **Okey** botão para fechar a caixa de diálogo.

Depois de concluir essas etapas, o extensor do controle DisabledButton deve aparecer na caixa de ferramentas (veja a Figura 4).


[![DisabledButton na caixa de ferramentas](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Figura 04**: DisabledButton na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))


Em seguida, precisamos criar uma nova página ASP.NET. Siga estas etapas:

1. Crie uma nova página do ASP.NET chamada ShowDisabledButton.aspx.
2. Arraste um ScriptManager à página.
3. Arraste um controle de caixa de texto para a página.
4. Arraste um controle de botão para a página.
5. Na janela Propriedades, altere a propriedade ID do botão para o valor <em>btnSave</em> e a propriedade de texto como o valor *salve\**.
  

Criamos uma página com um controle TextBox do ASP.NET e o botão padrão.

Em seguida, precisamos estender o controle de caixa de texto com o extensor DisabledButton:

1. Selecione o **adicionar extensor** opção para abrir a caixa de diálogo do Assistente de extensor de tarefa (consulte a Figura 5). Observe que a caixa de diálogo inclui nosso extensor DisabledButton personalizado.
2. Selecione o extensor DisabledButton e clique no **Okey** botão.


[![A caixa de diálogo do Assistente de extensor](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Figura 05**: caixa de diálogo Assistente de extensor a ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))


Por fim, podemos pode definir as propriedades do extensor DisabledButton. Você pode modificar as propriedades do extensor DisabledButton modificando as propriedades do controle de caixa de texto:

1. Selecione a caixa de texto no Designer.
2. Na janela Propriedades, expanda o nó de extensores (veja a Figura 6).
3. Atribua o valor *salve* para a propriedade DisabledText e o valor *btnSave* à propriedade TargetButtonID.


[![Definindo propriedades do extensor](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Figura 06**: definindo propriedades do extensor ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))


Quando você executa a página (pressionando F5), o controle de botão é inicialmente desabilitado. Assim que você começar a inserir texto na caixa de texto, o botão de controle está habilitado (veja a Figura 7).


[![O extensor DisabledButton em ação](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Figura 07**: DisabledButton o extensor em ação ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))


## <a name="summary"></a>Resumo

O objetivo deste tutorial foi explicar como você pode estender o AJAX Control Toolkit com controles do extensor personalizado. Neste tutorial, criamos um extensor de controle DisabledButton simple. Implementamos desse extensor, criando uma classe DisabledButtonExtender, um comportamento DisabledButtonBehavior JavaScript e uma classe DisabledButtonDesigner. Você seguir um conjunto semelhante de etapas sempre que você criar um extensor de controle personalizado.

> [!div class="step-by-step"]
> [Anterior](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [Próximo](get-started-with-the-ajax-control-toolkit-vb.md)
