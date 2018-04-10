---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Trabalhando com formulários HTML em Sites do ASP.NET páginas da Web (Razor) | Microsoft Docs
author: tfitzmac
description: Um formulário é uma seção de um documento HTML em que você pode colocar controles de entrada do usuário, como caixas de texto, caixas de seleção, botões e listas suspensas. Usar formulários de pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/10/2018
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Trabalhando com formulários HTML em Sites de páginas (Razor) da Web do ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo descreve como processar um formulário HTML (com caixas de texto e botões) quando você estiver trabalhando em um site de páginas da Web do ASP.NET (Razor).
> 
> **O que você aprenderá:** 
> 
> - Como criar um formulário HTML.
> - Como ler a entrada do usuário do formulário.
> - Como validar a entrada do usuário.
> - Como restaurar os valores de formulário depois que a página é enviada.
> 
> Esses são o ASP.NET conceitos introduzidos no artigo de programação:
> 
> - O objeto `Request`.
> - Validação de entrada.
> - Codificação HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com 2 de páginas da Web do ASP.NET.


## <a name="creating-a-simple-html-form"></a>Criando um formulário HTML simples

1. Crie um novo site.
2. Na pasta raiz, crie uma página da web denominado *Form.cshtml* e digite a seguinte marcação:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Abrir a página no seu navegador. (No WebMatrix, no **arquivos** espaço de trabalho, clique no arquivo e, em seguida, selecione **iniciar no navegador**.) Uma forma simples com três campos de entrada e um **enviar** botão é exibido.

    ![Captura de tela de um formulário com três caixas de texto.](4-working-with-forms/_static/image1.jpg)

    Neste ponto, se você clicar no **enviar** botão, nada acontece. Para tornar o formulário útil, você precisa adicionar algum código que será executado no servidor.

## <a name="reading-user-input-from-the-form"></a>Ler a entrada do usuário do formulário

Para processar o formulário, você adiciona o código que lê os valores de campo enviado e faça algo com eles. Este procedimento mostra como ler os campos e exibir a entrada do usuário na página. (Em um aplicativo de produção, você geralmente fazer coisas mais interessantes com a entrada do usuário. Você vai fazer isso no artigo sobre como trabalhar com bancos de dados.)

1. Na parte superior do *Form.cshtml* de arquivos, digite o seguinte código:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Quando o usuário solicita a página pela primeira vez, apenas o formulário vazio é exibido. O usuário (que será você) preenche o formulário e, em seguida, clica **enviar**. Isso envia (posta) a entrada do usuário para o servidor. Por padrão, a solicitação irá para a mesma página (ou seja, *Form.cshtml*).

    Quando você enviar a página neste momento, os valores inseridos são exibidos acima do formulário:

    ![Captura de tela que mostra os valores que você inseriu exibidos na página.](4-working-with-forms/_static/image2.jpg)

    Examine o código para a página. Você primeiro usar o `IsPost` método para determinar se a página está sendo lançada &#8212; ou seja, se um usuário clicou o **enviar** botão. Se esta for uma postagem, `IsPost` retorna true. Este é o modo padrão em páginas da Web do ASP.NET para determinar se você estiver trabalhando com uma solicitação inicial (uma solicitação GET) ou um postback (uma solicitação POST). (Para obter mais informações sobre o GET e POST, consulte a barra lateral "HTTP GET e POST e a IsPost propriedade" [Introdução ao ASP.NET páginas da Web de programação usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Em seguida, você pode obter os valores que o usuário preenchido a partir de `Request.Form` objeto e colocá-los em variáveis para mais tarde. O `Request.Form` objeto contém todos os valores que foram enviados com a página, cada uma identificada por uma chave. A chave é o equivalente para o `name` atributo do campo de formulário que você deseja ler. Por exemplo, para ler o `companyname` campo (caixa de texto), você usa `Request.Form["companyname"]`.

    Valores de formulário são armazenados no `Request.Form` objeto como cadeias de caracteres. Portanto, quando você precisar trabalhar com um valor como um número ou uma data ou algum outro tipo, você precisa convertê-lo de uma cadeia de caracteres desse tipo. No exemplo, o `AsInt` método o `Request.Form` é usado para converter o valor do campo de funcionários (que contém uma contagem de funcionários) para um número inteiro.
2. Abrir a página no seu navegador, preencha os campos do formulário e clique em **enviar**. A página exibe os valores que você inseriu.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Codificação aparência e a segurança de HTML
> 
> HTML tem usos especiais de caracteres como `<`, `>`, e `&`. Se esses caracteres especiais aparecem em que eles não são esperados, eles podem estragar a aparência e a funcionalidade da página da web. Por exemplo, o navegador interpreta o `<` de caracteres (a menos que ele seja seguido por um espaço) como o início de um elemento HTML, como `<b>` ou `<input ...>`. Se o navegador não reconhecer o elemento, simplesmente descarta a cadeia de caracteres que começa com `<` até atingir algo que ele reconheça novamente. Obviamente, isso pode resultar em alguns renderização estranha na página.
> 
> Codificação HTML substitui esses caracteres reservados com um código de navegadores interpretam como o símbolo correto. Por exemplo, o `<` caractere será substituído por `&lt;` e `>` caractere será substituído por `&gt;`. O navegador processa essas cadeias de caracteres de substituição, como os caracteres que você deseja ver.
> 
> É recomendável usar a qualquer momento que você exibir cadeias de caracteres de codificação HTML (entrada) que você obteve de um usuário. Se você não fizer isso, um usuário pode tentar obter a página da web para executar um script mal-intencionado ou fazer outras coisas que compromete a segurança de seu site ou simplesmente não pretende. (Isso é particularmente importante se você se entrada do usuário, armazene-o em algum ponto e, em seguida, exibi-lo mais tarde &#8212; por exemplo, como um comentário de blog, revisão de usuário ou algo assim.)
> 
> Para ajudar a evitar esses problemas, páginas da Web ASP.NET automaticamente codifica o HTML qualquer texto de conteúdo que você no seu código de saída. Por exemplo, quando você exibe o conteúdo de uma variável ou uma expressão usando código como `@MyVar`, páginas da Web ASP.NET automaticamente codifica a saída.


## <a name="validating-user-input"></a>Validando a entrada do usuário

Os usuários cometem erros. Peça para preencher um campo e esquecerem, ou solicite a inserção do número de funcionários e eles digitem um nome em vez disso. Para certificar-se de que um formulário tem sido preenchido corretamente antes de você processá-la, você deve validar a entrada do usuário.

Este procedimento mostra como validar todos os campos de formulário de três para certificar-se de que o usuário não deixá-los em branco. Você também pode verificar o valor de contagem de funcionários é um número. Se houver erros, você exibirá um erro de mensagem que informa ao usuário os valores que não passou na validação.

1. No *Form.cshtml* de arquivo, substitua o primeiro bloco de código com o código a seguir: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Para validar a entrada do usuário, você deve usar o `Validation` auxiliar. Registre os campos obrigatórios chamando `Validation.RequireField`. Registre outros tipos de validação chamando `Validation.Add` e especificando o campo a ser validado e o tipo de validação a ser executado.

    Quando a página é executada, o ASP.NET não validação para você. Você pode verificar os resultados chamando `Validation.IsValid`, que retorna true se tudo passado e FALSO se qualquer campo Falha na validação. Normalmente, você chamar `Validation.IsValid` antes de executar qualquer processamento em entrada do usuário.
2. Atualização de `<body>` elemento adicionando três chamadas para o `Html.ValidationMessage` método, como este:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Para exibir mensagens de erro de validação, você pode chamar o Html.`ValidationMessage` e passe o nome do campo que você deseja que a mensagem.
3. Execute a página. Deixe os campos em branco e clique em **enviar**. Ver mensagens de erro.

    ![Captura de tela que mostra as mensagens de erro exibidas se a entrada do usuário não passa na validação.](4-working-with-forms/_static/image3.jpg)
4. Adicionar uma cadeia de caracteres (por exemplo, "ABC") para o **contagem de funcionários** campo e clique em **enviar** novamente. Neste momento que você vir um erro que indica que a cadeia de caracteres não está no formato correto, ou seja, um número inteiro.

    ![Captura de tela que mostra as mensagens de erro exibidas se os usuários inserir uma cadeia de caracteres para o campo de funcionários.](4-working-with-forms/_static/image4.jpg)

Páginas da Web ASP.NET fornece mais opções para validar a entrada do usuário, incluindo a capacidade de executar a validação usando script de cliente automaticamente para que os usuários obtêm um retorno imediato no navegador. Consulte o [recursos adicionais](#Additional_Resources) posteriormente para obter mais informações.

## <a name="restoring-form-values-after-postbacks"></a>Restaurando valores de formulário após Postbacks

Depois de testar a página na seção anterior, você deve ter notado que se você tinha um erro de validação, tudo o que você inseriu (não apenas os dados inválidos) foi excluído e era necessário novamente, insira valores para todos os campos. Isso ilustra um ponto importante: quando você envia uma página, processá-la e, em seguida, renderizar a página novamente, a página é recriada do zero. Como você viu, isso significa que quaisquer valores que estavam na página quando ela foi enviada são perdidos.

Você pode corrigir isso facilmente, no entanto. Você tem acesso aos valores que foram enviadas (no `Request.Form` do objeto, para que você pode preencher os valores de volta para os campos de formulário quando a página é renderizada.

1. No *Form.cshtml* de arquivo, substitua o `value` atributos do `<input>` elementos usando o `value` atributo.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    O `value` atributo do `<input>` elementos foi definido para ler dinamicamente o valor do campo do `Request.Form` objeto. Na primeira vez que a página é solicitada, os valores de `Request.Form` objeto estejam vazias. Isso é bom, porque assim o formulário está em branco.
2. Abrir a página no seu navegador, preencha os campos de formulário ou deixá-los em branco e clique em **enviar**. É exibida uma página que mostra os valores enviados.

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [1.001 maneiras de obter a entrada de usuários da Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Uso de formulários e processamento de entrada do usuário](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Validação da entrada do usuário em sites de Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Usando o preenchimento automático em formulários HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
