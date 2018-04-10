---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
title: Criando um AJAX personalizado controle extensor de controle do Kit de ferramentas (VB) | Microsoft Docs
author: microsoft
description: Extensores personalizados permitem personalizar e estender os recursos dos controles do ASP.NET sem a necessidade de criar novas classes.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 18b29834-c991-4e0c-b533-44d358fbfc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 06950770bf788fff4a03e9d41fd448ea675a8bce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-custom-ajax-control-toolkit-control-extender-vb"></a>Criando um extensor de controle do Kit de ferramentas de controle AJAX personalizado (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Extensores personalizados permitem personalizar e estender os recursos dos controles do ASP.NET sem a necessidade de criar novas classes.


Neste tutorial, você aprenderá como criar um extensor de controle personalizado do AJAX Control Toolkit. Podemos criar um simples, mas útil, novo extensor que altera o estado de um botão de desabilitado para habilitado quando você digita texto em uma caixa de texto. Depois de ler este tutorial, você poderá estender o Kit de ferramentas ASP.NET AJAX com seus próprio extensores de controle.

Você pode criar os Extensores do controle personalizado usando o Visual Studio ou Visual Web Developer (certifique-se de que você tem a versão mais recente do Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Visão geral do extensor DisabledButton

Nosso novo extensor de controle é chamado de extensor DisabledButton. Este extensor tem três propriedades:

- TargetControlID - caixa de texto que o controle estende.
- TargetButtonIID - botão é habilitado ou desabilitado.
- DisabledText - o texto que é exibido inicialmente no botão. Quando você começa a digitar, o botão exibe o valor da propriedade de texto do botão.

Você conecta o extensor DisabledButton a um controle de botão e caixa de texto. Antes de digitar qualquer texto, o botão está desabilitado e a caixa de texto e o botão ter esta aparecem:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image1.png)

([Clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image3.png))


Depois que você começa a digitar o texto, o botão é habilitado e a caixa de texto e o botão ter esta aparecem:


[![](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image4.png)

([Clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image6.png))


Para criar nossa extensor de controle, precisamos criar três arquivos a seguir:

- DisabledButtonExtender.vb - este arquivo é a classe de controle de servidor que irá gerenciar criando extender e permitem que você defina as propriedades em tempo de design. Ele também define as propriedades que podem ser definidas no extender. Essas propriedades são acessíveis por meio de código e em tempo de design e correspondem a propriedades definidas no arquivo DisableButtonBehavior.js.
- DisabledButtonBehavior.js – Este arquivo é onde você adicionará todos sua lógica de script de cliente.
- DisabledButtonDesigner.vb - essa classe permite que a funcionalidade de tempo de design. Você precisa desta classe se você quiser que o extensor de controle para funcionar corretamente com o Designer do Visual Studio/Visual Web Developer.

Portanto um extensor de controle consiste em um controle de servidor, um comportamento do lado do cliente e uma classe do designer do lado do servidor. Você aprenderá a criar todos os três desses arquivos nas seções a seguir.

## <a name="creating-the-custom-extender-website-and-project"></a>Criando o projeto e o site da Web personalizado extensor

A primeira etapa é criar um projeto de biblioteca de classe e um site no Visual Studio/Visual Web Developer. Podemos ll criar o extensor personalizado no projeto de biblioteca de classe e testar o extensor personalizado no site.

Permitir que o s iniciar com o site. Siga estas etapas para criar o site:

1. Selecione a opção de menu **arquivo, o novo Site da Web**.
2. Selecione o **Site da Web ASP.NET** modelo.
3. Nomeie o novo site *Website1*.
4. Clique o **Okey** botão.

Em seguida, precisamos criar o projeto de biblioteca de classe que contém o código para o extensor de controle:

1. Selecione a opção de menu **arquivo, adicionar novo projeto**.
2. Selecione o **biblioteca de classes** modelo.
3. Nomear a nova biblioteca de classe com o nome **CustomExtenders**.
4. Clique o **Okey** botão.

Depois de concluir essas etapas, a janela do Gerenciador de soluções deve parecer com a Figura 1.


[![Solução de projeto de biblioteca de classe e de site](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image7.png)

**Figura 01**: solução com o projeto de biblioteca de classe e de site ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image9.png))


Em seguida, você precisa adicionar todas as referências de assembly necessárias para o projeto de biblioteca de classe:

1. O projeto CustomExtenders e selecione a opção de menu **adicionar referência**.
2. Selecione a guia .NET.
3. Adicione referências aos assemblies a seguir:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Selecione a guia do navegador.
5. Adicione uma referência ao assembly AjaxControlToolkit. Este assembly está localizado na pasta onde você baixou o Kit de ferramentas de controle AJAX.

Você pode verificar se você tiver adicionado todas as referências corretas clicando duas vezes o seu projeto, selecione Propriedades e clicando na guia referências (consulte a Figura 2).


[![Pasta de referências com as referências necessárias](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image10.png)

**Figura 02**: pasta de referências com as referências necessárias ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image12.png))


## <a name="creating-the-custom-control-extender"></a>Criando o extensor de controle personalizado

Agora que temos nossa biblioteca de classe, podemos começar a criação de nosso controle do extensor. Permitir que o s começar com o esqueleto de uma classe de controle do extensor personalizado (consulte a listagem 1).

**Listando 1 - MyCustomExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample1.vb)]

Há várias coisas a observar sobre a classe de extensor de controle na listagem 1. Primeiro, observe que a classe herda da classe base ExtenderControlBase. Todos os controles do extensor AJAX Control Toolkit derivam desta classe base. Por exemplo, a classe base inclui a propriedade TargetID que é uma propriedade necessária de cada extensor de controle.

Em seguida, observe que a classe inclui os seguintes dois atributos relacionados ao script de cliente:

- WebResource - faz com que um arquivo a ser incluído como um recurso inserido em um assembly.
- ClientScriptResource - faz com que um recurso de script a ser recuperado de um assembly.

O atributo WebResource é usado para inserir o arquivo MyControlBehavior.js JavaScript no assembly quando o extensor personalizado é compilado. O atributo ClientScriptResource é usado para recuperar o script MyControlBehavior.js do assembly quando o extensor personalizado for usado em uma página da web.


Para os atributos WebResource e ClientScriptResource trabalhar, você deve compilar o arquivo JavaScript como um recurso incorporado. Selecione o arquivo na janela do Gerenciador de soluções, abra a folha de propriedades e atribuir o valor *recurso incorporado* para o **ação de compilação** propriedade.


Observe que o extensor de controle também inclui um atributo TargetControlType. Este atributo é usado para especificar o tipo de controle que será estendido o extensor do controle. No caso de listagem 1, o extensor do controle é usado para estender uma caixa de texto.

Finalmente, observe que o extensor personalizado inclui uma propriedade chamada MyProperty. A propriedade é marcada com o atributo ExtenderControlProperty. Os métodos GetPropertyValue() e SetPropertyValue() são usados para passar o valor da propriedade de extensor de controle de servidor para o comportamento do lado do cliente.

Permitem s vá em frente e implementar o código para nosso extensor DisabledButton. O código para este extensor pode ser encontrado na lista 2.

**A listagem 2 - DisabledButtonExtender.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample2.vb)]

O extensor DisabledButton na listagem 2 tem duas propriedades chamadas TargetButtonID e DisabledText. O IDReferenceProperty aplicado à propriedade TargetButtonID impede que você atribuir qualquer coisa que não seja a ID de um controle de botão para essa propriedade.

Os atributos WebResource e ClientScriptResource associar um comportamento do cliente localizado em um arquivo chamado DisabledButtonBehavior.js com este extensor. Abordaremos esse arquivo JavaScript na próxima seção.

## <a name="creating-the-custom-extender-behavior"></a>Criando o comportamento de extensor personalizado

O componente do lado do cliente de um extensor de controle é chamado um comportamento. A lógica real para desabilitar e habilitar o botão está contida no comportamento DisabledButton. O código JavaScript para o comportamento está incluído na listagem 3.

**A listagem 3 - DisabledButton.js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample3.js)]

O arquivo JavaScript na listagem 3 contém uma classe de cliente chamada DisabledButtonBehavior. Essa classe, como duas seu lado do servidor, inclui duas propriedades chamadas TargetButtonID e DisabledText que você pode acessar usando obter\_TargetButtonID/set\_TargetButtonID e obter\_DisabledText/set\_ DisabledText.

O método Initialize associa o elemento de destino para o comportamento de um manipulador de evento keyup. Cada vez que você digitar uma letra na caixa de texto associada a esse comportamento, o manipulador keyup executa. O manipulador keyup habilita ou desabilita o botão dependendo se a caixa de texto associada ao comportamento contém qualquer texto.

Lembre-se de que você deve compilar o arquivo JavaScript na listagem 3 como um recurso inserido. Selecione o arquivo na janela do Gerenciador de soluções, abra a folha de propriedades e atribuir o valor *recurso incorporado* para o **ação de compilação** propriedade (consulte a Figura 3). Essa opção está disponível no Visual Studio e o Visual Web Developer.


[![Adicionando um arquivo JavaScript como um recurso inserido](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image13.png)

**Figura 03**: adicionando um arquivo JavaScript como um recurso incorporado ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image15.png))


## <a name="creating-the-custom-extender-designer"></a>Criando o Designer de extensor personalizado

Há uma classe última que precisamos criar para concluir nosso extensor. É preciso criar a classe do designer na listagem 4. Essa classe é necessária para tornar o extensor se comportem corretamente com o Designer do Visual Studio/Visual Web Developer.

**A listagem 4 - DisabledButtonDesigner.vb**

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample4.vb)]

Associe o designer na listagem 4 o extensor DisabledButton com o atributo do Designer. Você precisa aplicar o atributo de Designer para a classe DisabledButtonExtender como este:

[!code-vb[Main](creating-a-custom-ajax-control-toolkit-control-extender-vb/samples/sample5.vb)]

## <a name="using-the-custom-extender"></a>Usando o extensor personalizado

Agora que estamos terminar de criar o extensor do controle DisabledButton, é hora de usá-lo em nosso site da Web ASP.NET. Primeiro, precisamos adicionar o extensor personalizado à caixa de ferramentas. Siga estas etapas:

1. Abra uma página ASP.NET clicando duas vezes a página na janela do Gerenciador de soluções.
2. A caixa de ferramentas e selecione a opção de menu **escolher itens**.
3. Na caixa de diálogo Escolher itens da caixa de ferramentas, navegue até o assembly CustomExtenders.dll.
4. Clique o **Okey** para fechar a caixa de diálogo.

Depois de concluir essas etapas, o extensor do controle DisabledButton deve aparecer na caixa de ferramentas (consulte a Figura 4).


[![DisabledButton na caixa de ferramentas](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image16.png)

**Figura 04**: DisabledButton na caixa de ferramentas ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image18.png))


Em seguida, é preciso criar uma nova página ASP.NET. Siga estas etapas:

1. Crie uma nova página ASP.NET chamada ShowDisabledButton.aspx.
2. Arraste um ScriptManager na página.
3. Arraste um controle de caixa de texto para a página.
4. Arraste um controle de botão para a página.
5. Na janela Propriedades, altere a propriedade de ID de botões para o valor <em>btnSave</em> e a propriedade de texto como o valor *salvar\**.
  

Nós criamos uma página com um controle de caixa de texto do ASP.NET e o botão padrão.

Em seguida, é necessário estender o controle de caixa de texto com o extensor DisabledButton:

1. Selecione o **adicionar extensor** opção para abrir a caixa de diálogo do Assistente de extensor de tarefa (consulte a Figura 5). Observe que a caixa de diálogo inclui nosso extensor DisabledButton personalizado.
2. Selecione o extensor DisabledButton e clique no **Okey** botão.


[![A caixa de diálogo do Assistente de extensor](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image19.png)

**Figura 05**: caixa de diálogo do Assistente de extensor ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image21.png))


Por fim, podemos definir as propriedades do extensor DisabledButton. Você pode modificar as propriedades do extensor DisabledButton, modificando as propriedades do controle de caixa de texto:

1. Selecione a caixa de texto no Designer.
2. Na janela Propriedades, expanda o nó de extensores (veja a Figura 6).
3. Atribuir o valor *salvar* para a propriedade DisabledText e o valor *btnSave* à propriedade TargetButtonID.


[![Definindo propriedades estendidas](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image22.png)

**Figura 06**: definindo as propriedades estendidas ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image24.png))


Quando você executa a página (pressionando F5), o controle de botão é inicialmente desabilitado. Como começar a inserir o texto na caixa de texto, o botão de controle está habilitado (veja a Figura 7).


[![O extensor DisabledButton em ação](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image25.png)

**Figura 07**: DisabledButton o extensor em ação ([clique para exibir a imagem em tamanho normal](creating-a-custom-ajax-control-toolkit-control-extender-vb/_static/image27.png))


## <a name="summary"></a>Resumo

O objetivo deste tutorial era explicam como você pode estender o Kit de ferramentas de controle AJAX com controles do extensor personalizado. Neste tutorial, criamos um extensor de controle de DisabledButton simple. Implementamos desse extensor, criando uma classe DisabledButtonExtender, um comportamento DisabledButtonBehavior JavaScript e uma classe DisabledButtonDesigner. Você seguir um conjunto similar de etapas sempre que você criar um extensor de controle personalizado.

> [!div class="step-by-step"]
> [Anterior](using-ajax-control-toolkit-controls-and-control-extenders-vb.md)
