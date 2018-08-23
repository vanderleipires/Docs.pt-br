---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Trabalhando com formulários HTML no ASP.NET Web Pages (Razor) Sites | Microsoft Docs
author: tfitzmac
description: Um formulário é uma seção de um documento HTML em que você colocar os controles de entrada do usuário, como listas suspensas, caixas de seleção, botões de opção e caixas de texto. Usar formulários qu...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 3f4effecb3b871f1bd7db1cd2a7aab6eeca80c50
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824986"
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Trabalhando com formulários HTML em Sites do ASP.NET Web Pages (Razor)
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
> Estes são os conceitos apresentados neste artigo de programação do ASP.NET:
> 
> - O objeto `Request`.
> - Validação de entrada.
> - A codificação HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2.


## <a name="creating-a-simple-html-form"></a>Criando um formulário HTML simples

1. Crie um novo site.
2. Na pasta raiz, crie uma página da web denominado *Form.cshtml* e insira a seguinte marcação:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Abrir a página no seu navegador. (No WebMatrix, nos **arquivos** espaço de trabalho, o arquivo com o botão direito e, em seguida, selecione **iniciar no navegador**.) Um formulário simples com três campos de entrada e uma **enviar** botão é exibido.

    ![Captura de tela de um formulário com três caixas de texto.](4-working-with-forms/_static/image1.jpg)

    Neste ponto, se você clicar na **enviar** botão, nada acontece. Para tornar o formulário útil, você precisa adicionar algum código que será executado no servidor.

## <a name="reading-user-input-from-the-form"></a>Ler a entrada do usuário do formulário

Para processar o formulário, você deve adicionar código que lê os valores de campo enviado e faz algo com eles. Este procedimento mostra como ler os campos e exibir a entrada do usuário na página. (Em um aplicativo de produção, você geralmente fazer coisas mais interessantes com a entrada do usuário. Você fará isso no artigo sobre como trabalhar com bancos de dados.)

1. Na parte superior do *Form.cshtml* de arquivo, insira o código a seguir:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Quando o usuário primeiro solicita a página, apenas o formulário vazio é exibido. O usuário (que será você) preenche o formulário e, em seguida, clica **enviar**. Isso envia (posta) a entrada do usuário para o servidor. Por padrão, a solicitação irá para a mesma página (ou seja, *Form.cshtml*).

    Quando você envia a página neste momento, os valores inseridos são exibidos apenas acima do formulário:

    ![Captura de tela que mostra os valores que você inseriu exibidos na página.](4-working-with-forms/_static/image2.jpg)

    Examinar o código para a página. Primeiro use o `IsPost` método para determinar se a página está sendo lançada &#8212; ou seja, se um usuário clicou o **enviar** botão. Quando se trata de uma postagem, `IsPost` retorna true. Essa é a maneira padrão em páginas da Web do ASP.NET para determinar se você estiver trabalhando com uma solicitação inicial (uma solicitação GET) ou um postback (uma solicitação POST). (Para obter mais informações sobre como GET e POST, consulte a barra lateral "HTTP GET e POST e a IsPost propriedade" em [Introdução ao ASP.NET páginas da Web de programação usando a sintaxe Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    Em seguida, você obterá os valores que o usuário preenchido a partir de `Request.Form` objeto e você os coloca em variáveis para uso posterior. O `Request.Form` objeto contém todos os valores que foram enviados com a página, cada uma identificada por uma chave. A chave é o equivalente a `name` atributo do campo de formulário que você deseja ler. Por exemplo, para ler o `companyname` campo (caixa de texto), você usa `Request.Form["companyname"]`.

    Valores de formulário são armazenados do `Request.Form` objeto como cadeias de caracteres. Portanto, quando você tem que trabalhar com um valor como um número ou uma data ou algum outro tipo, você precisa convertê-lo de uma cadeia de caracteres para esse tipo. No exemplo, o `AsInt` método da `Request.Form` é usada para converter o valor do campo de funcionários (que contém uma contagem de funcionários) para um número inteiro.
2. Abrir a página no seu navegador, preencha os campos de formulário e clique em **enviar**. A página exibe os valores inseridos.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Codificação para a aparência e a segurança HTML
> 
> HTML tem usos especiais para caracteres, como `<`, `>`, e `&`. Se esses caracteres especiais são exibidos em que eles não são esperados, eles podem estragar a aparência e funcionalidade da página da web. Por exemplo, o navegador interpreta os `<` caractere (a menos que ele é seguido por um espaço) como o início de um elemento HTML, como `<b>` ou `<input ...>`. Se o navegador não reconhecer o elemento, ele simplesmente descarta a cadeia de caracteres que começa com `<` até atingir algo que ele reconhece novamente. Obviamente, isso pode resultar em algum processamento estranho na página.
> 
> A codificação HTML substitui os caracteres reservados com um código que navegadores interpretam como o símbolo correto. Por exemplo, o `<` caractere será substituído por `&lt;` e o `>` caractere será substituído por `&gt;`. O navegador renderiza essas cadeias de caracteres de substituição como os caracteres que você deseja ver.
> 
> É uma boa ideia usar a qualquer momento que você exibe cadeias de caracteres de codificação HTML (entrada) que você obteve de um usuário. Se você não fizer isso, um usuário pode tentar obter sua página da web para executar um script mal-intencionado ou fazer outras coisas que compromete a segurança de seu site ou que é simplesmente não o que você pretende. (Isso é particularmente importante se você aceita entrada do usuário, armazená-lo em um local e, em seguida, exibi-lo mais tarde &#8212; por exemplo, como um comentário de blog, revisão de usuário ou algo assim.)
> 
> Para ajudar a evitar esses problemas, páginas da Web ASP.NET automaticamente codifica em HTML qualquer texto de conteúdo que você a partir do código de saída. Por exemplo, quando você exibe o conteúdo de uma variável ou uma expressão usando um código como `@MyVar`, páginas da Web ASP.NET codifica automaticamente a saída.


## <a name="validating-user-input"></a>Validação de entrada do usuário

Os usuários cometem erros. Peça para preencher um campo e esquecerem, ou peça para inserir o número de funcionários e eles digitem um nome em vez disso. Para certificar-se de que um formulário tiver sido preenchido corretamente antes de processá-lo, você deve validar a entrada do usuário.

Este procedimento mostra como validar todos os campos de formulário de três para garantir que o usuário não deixá-los em branco. Você também pode verificar o valor da contagem de funcionários é um número. Se houver erros, você exibirá um erro de mensagem que informa ao usuário que valores não passa na validação.

1. No *Form.cshtml* de arquivo, substitua o primeiro bloco de código com o código a seguir: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Para validar a entrada do usuário, você deve usar o `Validation` auxiliar. Registre os campos obrigatórios chamando `Validation.RequireField`. Registrar outros tipos de validação, chame `Validation.Add` e especificando o campo a ser validado e o tipo de validação a ser executado.

    Quando a página é executada, o ASP.NET faz a validação para você. Você pode verificar os resultados chamando `Validation.IsValid`, que retorna true se tudo o que é passado e false se qualquer campo Falha na validação. Normalmente, você chama `Validation.IsValid` antes de executar qualquer processamento em entrada do usuário.
2. Atualizar o `<body>` elemento adicionando três chamadas para o `Html.ValidationMessage` método, como este:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Para exibir mensagens de erro de validação, você pode chamar o Html.`ValidationMessage` e passe o nome do campo que você deseja que a mensagem para.
3. Execute a página. Deixe os campos em branco e clique em **enviar**. Ver mensagens de erro.

    ![Captura de tela que mostra mensagens de erro exibidas se a entrada do usuário não passa na validação.](4-working-with-forms/_static/image3.jpg)
4. Adicionar uma cadeia de caracteres (por exemplo, "ABC") para o **contagem de funcionários** campo e clique em **enviar** novamente. Neste momento, que você verá um erro indicando que a cadeia de caracteres não está no formato correto, ou seja, um número inteiro.

    ![Captura de tela que mostra mensagens de erro exibidas se os usuários inserem uma cadeia de caracteres para o campo de funcionários.](4-working-with-forms/_static/image4.jpg)

Páginas da Web do ASP.NET fornece mais opções de validação de entrada do usuário, incluindo a capacidade de realizar automaticamente a validação usando script de cliente, para que os usuários recebem feedback imediato no navegador. Consulte a [recursos adicionais](#Additional_Resources) posteriormente para obter mais informações.

## <a name="restoring-form-values-after-postbacks"></a>Restauração de valores de formulário após Postbacks

Quando você testou a página na seção anterior, você deve ter notado que se você tinha um erro de validação, tudo o que você inseriu (não apenas os dados inválidos) foi desapareceu e você tinha que insira novamente os valores para todos os campos. Isso ilustra um ponto importante: quando você envia uma página, processá-lo e, em seguida, renderiza a página novamente, a página é recriada do zero. Como você viu, isso significa que todos os valores que estavam na página quando ele foi enviado são perdidos.

Você pode corrigir isso facilmente, no entanto. Você tem acesso aos valores que foram enviadas (no `Request.Form` do objeto, portanto, você pode preencher esses valores de volta para os campos de formulário quando a página é renderizada.

1. No *Form.cshtml* do arquivo, substitua o `value` atributos do `<input>` elementos usando o `value` atributo.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    O `value` atributo o `<input>` elementos foi definido para ler dinamicamente o valor do campo do `Request.Form` objeto. Na primeira vez que a página é solicitada, os valores de `Request.Form` objeto estarão vazias. Isso é bom, porque dessa forma, o formulário está em branco.
2. Abrir a página no seu navegador, preencha os campos de formulário ou deixá-los em branco e clique em **enviar**. É exibida uma página que mostra os valores enviados.

    ![5 de formulários](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

- [1.001 maneiras de obter a entrada de usuários da Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Usando formulários e processamento de entrada do usuário](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Validação da entrada do usuário em sites de Páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Usando o preenchimento automático em formulários HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
