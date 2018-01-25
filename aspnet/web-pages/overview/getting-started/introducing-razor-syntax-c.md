---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: "Introdução à programação da Web do ASP.NET usando a sintaxe do Razor (c#) | Microsoft Docs"
author: tfitzmac
description: "Este capítulo fornece uma visão geral de programação com páginas da Web do ASP.NET usando a sintaxe do Razor. O ASP.NET é uma tecnologia da Microsoft para executar o pa web dinâmicos..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 32cdd8d524d783d7ccc3ab076de636ce4a868132
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Introdução à programação da Web do ASP.NET usando a sintaxe do Razor (c#)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo fornece uma visão geral de programação com páginas da Web do ASP.NET usando a sintaxe do Razor. O ASP.NET é uma tecnologia da Microsoft para a execução de páginas da web dinâmicas em servidores web. Este artigo enfoca usando a linguagem de programação c#.
> 
> **Você aprenderá**:
> 
> - Os 8 principais dicas para começar a trabalhar com o ASP.NET Web Pages com sintaxe Razor de programação de programação.
> - Conceitos de programação, será necessário.
> - O código de servidor do ASP.NET e a sintaxe do Razor é tudo sobre.
>   
> 
> ## <a name="software-versions"></a>Versões de software
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com 2 de páginas da Web do ASP.NET.


## <a name="the-top-8-programming-tips"></a>As dicas de programação 8 superior

Esta seção lista algumas dicas que é absolutamente necessário saber como começar a escrever código de servidor ASP.NET usando a sintaxe do Razor.

> [!NOTE]
> A sintaxe do Razor é baseada na linguagem de programação c#, e que é a linguagem que é geralmente usada com páginas da Web do ASP.NET. No entanto, a sintaxe do Razor também oferece suporte a linguagem Visual Basic e tudo o que você vê que você também pode fazer no Visual Basic. Para obter detalhes, consulte o Apêndice [linguagem Visual Basic e a sintaxe](https://go.microsoft.com/fwlink/?LinkId=202908).


Você pode encontrar mais detalhes sobre a maioria dessas técnicas de programação posteriormente neste artigo.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Adicione código para uma página usando o caractere @

O `@` caractere inicia expressões internas, blocos de instrução única e blocos de várias instruções:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Isso é que essas instruções aparência quando a página é executada em um navegador:

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **Codificação HTML**
> 
> Quando você exibe o conteúdo em uma página usando o `@` caractere, como nos exemplos anteriores, ASP.NET HTML codifica a saída. Isso substitui caracteres reservados de HTML (como `<` e `>` e `&`) com códigos que permitem que os caracteres a serem exibidos como caracteres em uma página da web em vez de ser interpretado como marcas HTML ou entidades. Sem codificação HTML, a saída do seu código de servidor não seja exibido corretamente e pode expor uma página a riscos de segurança.
> 
> Se sua meta é uma marcação HTML que renderiza marcações como marcação de saída (por exemplo `<p></p>` para um parágrafo ou `<em></em>` para enfatizar o texto), consulte a seção [combinação de texto, marcação e código em blocos de código](#BM_CombiningTextMarkupAndCode) posteriormente neste artigo.
> 
> Você pode ler mais sobre a codificação de HTML no [trabalhar com formulários](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Coloque os blocos de código entre chaves

Um *bloco de código* inclui uma ou mais instruções de código e fica entre chaves.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

O resultado exibido em um navegador:

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. Dentro de um bloco, você encerrar a cada instrução de código com um ponto e vírgula

Dentro de um bloco de código, cada instrução de código completo deve terminar com um ponto e vírgula. Expressões internas não terminam com um ponto e vírgula.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Usar variáveis para armazenar valores

Você pode armazenar valores em uma *variável*, incluindo cadeias de caracteres, números e datas, etc. Criar uma nova variável usando o `var` palavra-chave. Você pode inserir valores de variáveis diretamente em uma página usando `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

O resultado exibido em um navegador:

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Coloque os valores de cadeia de caracteres literal entre aspas duplas

Um *cadeia de caracteres* é uma sequência de caracteres que são tratados como texto. Para especificar uma cadeia de caracteres, coloque-o entre aspas duplas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Se a cadeia de caracteres que você deseja exibir contém um caractere de barra invertida ( `\` ) ou aspas duplas ( `"` ), use um *literal de cadeia de caracteres textual* que é prefixado com o `@` operador. (Em c#, o \ caractere tem um significado especial, a menos que você use uma cadeia de caracteres textual literal.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Inserir aspas duplas, use uma cadeia de caracteres textual literal e repita as aspas:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Aqui está o resultado do uso de ambos os exemplos em uma página:

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Observe que o `@` caractere é usado para marcar os literais de cadeia de caracteres textuais em c# e marcar o código em páginas ASP.NET.


### <a name="6-code-is-case-sensitive"></a>6. Código diferencia maiusculas de minúsculas

No c#, palavras-chave (como `var`, `true`, e `if`) e nomes de variáveis diferenciam maiusculas de minúsculas. Linhas de código a seguir criam duas variáveis diferentes, `lastName` e`LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Se você declarar uma variável como `var lastName = "Smith";` e se você tentar fazer referência a essa variável em sua página como `@LastName`, ocorrerá um erro porque `LastName` não será reconhecida.

> [!NOTE]
> No Visual Basic, variáveis e palavras-chave são *não* diferencia maiusculas de minúsculas.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Grande parte da sua codificação envolve objetos

Um *objeto* representa algo que você pode programar com &#8212; uma página, uma caixa de texto, um arquivo, uma imagem, uma solicitação da web, uma mensagem de email, um registro de cliente (linha de banco de dados), etc. Os objetos têm propriedades que descrevem suas características e que você pode ler ou alterar &#8212; um objeto de caixa de texto tem uma `Text` propriedade (entre outros), um objeto de solicitação tem uma `Url` propriedade, uma mensagem de email tem um `From` propriedade e um objeto de cliente tem um `FirstName` propriedade. Objetos também têm métodos que são o &quot;verbos&quot; eles podem executar. Os exemplos incluem um objeto de arquivo `Save` método, um objeto de imagem `Rotate` método e um objeto de email `Send` método.

Geralmente, você trabalhará com o `Request` do objeto, que fornece informações como os valores de caixas de texto (campos de formulário) na página, o tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc. O exemplo a seguir mostra como acessar as propriedades do `Request` objeto e como chamar o `MapPath` método o `Request` object, que fornece o caminho absoluto da página no servidor:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

O resultado exibido em um navegador:

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Você pode escrever código que toma decisões

Um recurso importante de páginas da web dinâmicas é que você pode determinar o que fazer com base nas condições. A maneira mais comum para isso é com o `if` instrução (e opcionais `else` instrução).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

A instrução `if(IsPost)` é uma forma abreviada de gravar `if(IsPost == true)`. Juntamente com `if` instruções, há uma variedade de maneiras de testar condições, repetição blocos de código, e assim por diante, que são descrito neste artigo.

O resultado exibido em um navegador (depois de clicar em **enviar**):

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET e POST métodos e a propriedade IsPost
> 
> O protocolo usado para páginas da web (HTTP) oferece suporte a um número muito limitado de métodos (verbos) que são usados para fazer solicitações para o servidor. Duas as mais comuns são GET, que é usado para ler uma página, e o POST, que é usado para enviar uma página. Em geral, a primeira vez que um usuário solicita uma página, a página é solicitada usando GET. Se o usuário preenche um formulário e, em seguida, clica no botão Enviar, o navegador faz uma solicitação POST para o servidor.
> 
> Programação da web, geralmente é útil saber se uma página está sendo solicitada como GET ou como um POST para que você sabe como processar a página. Em páginas de Web do ASP.NET, você pode usar o `IsPost` propriedade para ver se uma solicitação é um GET ou um POST. Se a solicitação for uma POSTAGEM, o `IsPost` propriedade retornará true, e você pode fazer coisas como ler os valores das caixas de texto em um formulário. Muitos exemplos, você verá mostram como processar a página de forma diferente dependendo do valor de `IsPost`.


## <a name="a-simple-code-example"></a>Um exemplo de código simples

Este procedimento mostra como criar uma página que ilustra as técnicas básicas de programação. No exemplo, você deve criar uma página que permite aos usuários inserir dois números, em seguida, adiciona-os e exibe o resultado.

1. Em seu editor, crie um novo arquivo e nomeie- *AddNumbers.cshtml*.
2. Copie o seguinte código e marcação para a página, substituindo qualquer coisa já na página.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Aqui estão algumas coisas a observar:

    - O `@` caractere inicia o primeiro bloco de código na página, e ela precede o `totalMessage` variável que é inserido na parte inferior da página.
    - O bloco na parte superior da página é colocado entre chaves.
    - No bloco na parte superior, todas as linhas terminam com um ponto e vírgula.
    - As variáveis `total`, `num1`, `num2`, e `totalMessage` armazenar vários números e uma cadeia de caracteres.
    - O valor de cadeia de caracteres literal atribuído para o `totalMessage` variável está entre aspas duplas.
    - Porque o código diferencia maiusculas de minúsculas, quando o `totalMessage` variável é usada na parte inferior da página, o nome deve corresponder exatamente a variável na parte superior.
    - A expressão `num1.AsInt() + num2.AsInt()` mostra como trabalhar com objetos e métodos. O `AsInt` método em cada variável converte a cadeia de caracteres inserida por um usuário para um número (um inteiro) para que você possa realizar aritmética nele.
    - O `<form>` marca inclui um `method="post"` atributo. Isso especifica que quando o usuário clica **adicionar**, a página será enviada ao servidor usando o método HTTP POST. Quando a página é enviada, o `if(IsPost)` teste seja avaliada como true e a condicional código é executado, exibindo o resultado da adição de números.
3. Salve a página e executá-lo em um navegador. (Verifique se a página está selecionada no **arquivos** espaço de trabalho antes de você executá-lo.) Insira dois números inteiros e, em seguida, clique no **adicionar** botão. 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Conceitos básicos de programação

Este artigo fornece uma visão geral de programação da web ASP.NET. Não é uma análise completa, um rápido tour por meio de conceitos de programação que você usará com mais frequência. Mesmo assim, ele aborda quase tudo o que você precisará começar com páginas da Web do ASP.NET.

Mas primeiro, uma pouco técnicas básicas.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>A sintaxe do Razor, o código do servidor e o ASP.NET

A sintaxe do Razor é uma sintaxe de programação simple para inserir o código de servidor em uma página da web. Em uma página da web que usa a sintaxe do Razor, há dois tipos de conteúdo: código de conteúdo e o servidor do cliente. Conteúdo do cliente é coisas que você está acostumado a em páginas da web: a marcação HTML (elementos), estilo informações como CSS, talvez alguns scripts de cliente, como JavaScript e texto sem formatação.

A sintaxe do Razor permite adicionar código de servidor para esse conteúdo de cliente. Se houver código de servidor na página, o servidor executa código primeiro, antes de enviar a página para o navegador. Executando no servidor, o código pode executar tarefas que podem ser muito mais complexas fazer usando o conteúdo do cliente usado sozinho, como acessar bancos de dados baseados em servidor. Mais importante, o código de servidor pode criar dinamicamente cliente conteúdo &#8212; ele pode gerar uma marcação HTML ou outro conteúdo em tempo real e, em seguida, enviá-lo para o navegador junto com qualquer HTML estático que a página pode conter. Da perspectiva do navegador, conteúdo de cliente que é gerado pelo seu código de servidor não é diferente de qualquer outro conteúdo do cliente. Como já vimos, o código de servidor necessária é muito simple.

Páginas da web ASP.NET que inclua a sintaxe do Razor têm uma extensão de arquivo especial (*. cshtml* ou *. vbhtml*). O servidor reconhece essas extensões, executa o código que é marcado com a sintaxe do Razor e, em seguida, envia a página para o navegador.

### <a name="where-does-aspnet-fit-in"></a>Onde entra a ASP.NET?

A sintaxe do Razor baseia-se em uma tecnologia da Microsoft chamada de ASP.NET, que por sua vez, baseia-se no Microsoft .NET Framework. O.NET Framework é uma estrutura de programação grande e abrangente da Microsoft para o desenvolvimento de praticamente qualquer tipo de aplicativo de computador. O ASP.NET é parte do .NET Framework que é projetado especificamente para a criação de aplicativos web. Os desenvolvedores usaram ASP.NET para criar muitos sites maiores e mais alto tráfego no mundo. (Qualquer momento você ver a extensão de nome de arquivo *. aspx* como parte da URL em um site, você saberá que o site foi escrito usando o ASP.NET.)

A sintaxe do Razor fornece todo o poder do ASP.NET, mas usando uma sintaxe simplificada, que é mais fácil saber se você for iniciante e que torna mais produtivo, se você é um especialista. Embora essa sintaxe é simple de usar, sua relação família com ASP.NET e o .NET Framework significa como seus sites se tornam mais sofisticados, você tem a capacidade do maior frameworks disponíveis para você.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Classes e instâncias**
> 
> Código de servidor ASP.NET usa objetos, que por sua vez são criados em conceito de classes. A classe é a definição ou modelo para um objeto. Por exemplo, um aplicativo pode conter um `Customer` classe que define as propriedades e métodos que precisa de qualquer objeto do cliente.
> 
> Quando o aplicativo precisa para trabalhar com as informações do cliente real, ele cria uma instância do (ou *instancia*) um objeto de cliente. Cada cliente individual é uma instância separada do `Customer` classe. Cada instância suporta as mesmas propriedades e métodos, mas os valores de propriedade para cada instância são normalmente diferentes, porque cada objeto de cliente é exclusivo. No objeto de um cliente, o `LastName` propriedade pode ser "Smith"; em outro objeto de cliente, o `LastName` propriedade pode ser "Jones".
> 
> Da mesma forma, qualquer página da web individuais em seu site é um `Page` que é uma instância do objeto de `Page` classe. Um botão na página é uma `Button` que é uma instância do objeto o `Button` classe e assim por diante. Cada instância tem características próprias, mas todos eles são com base no que é especificado na definição de classe do objeto.


## <a name="basic-syntax"></a>Sintaxe básica

Anteriormente, você viu um exemplo básico de como criar uma página da Web do ASP.NET e como você pode adicionar o código do servidor para marcação HTML. Aqui, você aprenderá os conceitos básicos de escrever código de servidor ASP.NET usando a sintaxe do Razor &#8212; ou seja, as regras linguagem de programação.

Se você estiver familiarizado com a programação (especialmente se você tiver usado o C, C++, c#, Visual Basic ou JavaScript), muitas das quais você ler aqui serão familiares. Você provavelmente precisará familiarizar-se apenas com como o código do servidor é adicionado à marcação em *. cshtml* arquivos.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>A combinação de texto, marcação e código em blocos de código

Em blocos de código do servidor, você geralmente deseja saída texto ou marcação (ou ambos) para a página. Se um bloco de código do servidor contém texto que não é código e que em vez disso, deve ser renderizado como é, o ASP.NET precisa ser capaz de distinguir o texto do código. Há várias maneiras de fazer isso.

- Coloque o texto em um elemento HTML como `<p></p>` ou `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    O elemento HTML pode incluir texto, elementos HTML adicionais e expressões de código do servidor. Quando o ASP.NET vê a marca HTML de abertura (por exemplo, `<p>`), ele processa tudo, incluindo o elemento e seu conteúdo como para o navegador, resolvendo expressões de código do servidor quando ele passa.
- Use o `@:` operador ou a `<text>` elemento. O `@:` gera uma única linha de conteúdo que contém o texto sem formatação ou marcas HTML não correspondentes; o `<text>` elemento abrange várias linhas de saída. Essas opções são úteis quando você não deseja renderizar um elemento HTML como parte da saída.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Se você deseja produzir várias linhas de texto ou marcas HTML não correspondentes, você pode preceder cada linha com `@:`, ou você pode colocar a linha em uma `<text>` elemento. Como o `@:` operador,`<text>` marcas são usadas pelo ASP.NET para identificar o conteúdo de texto e nunca são renderizadas na saída da página.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    O primeiro exemplo repete o exemplo anterior, mas usa um único par de `<text>` marcas para incluir o texto para processar. No segundo exemplo, o `<text>` e `</text>` marcas Coloque três linhas, que têm algum texto contido e marcas HTML não correspondentes (`<br />`), junto com o código do servidor e as marcas HTML correspondentes. Novamente, você também pode preceder cada linha individualmente com a `@:` operador; qualquer funciona de maneira.

    > [!NOTE]
    > Quando você saída texto conforme mostrado nesta seção &#8212; usando um elemento HTML, o `@:` operador, ou o `<text>` elemento &#8212; ASP.NET não codificação HTML a saída. (Como observado anteriormente, o ASP.NET codificar a saída de expressões de código do servidor e blocos de código de servidor que são precedidos por `@`, exceto nos casos especiais observados nesta seção.)

### <a name="whitespace"></a>Whitespace

A instrução não afetam os espaços adicionais em uma instrução (e fora de uma literal de cadeia de caracteres):

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Uma quebra de linha em uma instrução não tem nenhum efeito na instrução, e você pode encapsular instruções para facilitar a leitura. As instruções a seguir são os mesmos:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

No entanto, você não pode encapsular uma linha no meio de uma cadeia de caracteres literal. O exemplo a seguir não funciona:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Para combinar uma cadeia de caracteres longa é quebrada em várias linhas com o código acima, há duas opções. Você pode usar o operador de concatenação (`+`), que você verá neste artigo. Você também pode usar o `@` caractere para criar uma cadeia de caracteres textual literal, como você viu neste artigo. Você pode dividir a literais de cadeia de caracteres textuais em linhas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Código (e a marcação) comentários

Comentários permitem que você deixe anotações para você ou outras pessoas. Eles também permitem que você desabilite (*comentar*) uma seção de código ou marcação que você não deseja executar, mas deseja manter em sua página por enquanto.

Há diferentes comentando sintaxe código Razor para marcação HTML. Assim como acontece com todo o código Razor, Razor comentários são processados (e, em seguida, removidos) no servidor antes da página é enviada para o navegador. Portanto, a sintaxe de comentário do Razor permite que você coloque comentários no código (ou até mesmo para a marcação) que você pode ver quando você editar o arquivo, mas que os usuários não veem, mesmo na fonte de página.

Para comentários de ASP.NET Razor, iniciar o comentário com `@*` e terminar com `*@`. O comentário pode estar em uma ou várias linhas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Aqui está um comentário dentro de um bloco de código:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Aqui está o mesmo bloco de código, com a linha de código comentado para que ele não será executado:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

Dentro de um bloco de código, como uma alternativa ao uso da sintaxe de comentário do Razor, você pode usar a sintaxe de comentário da linguagem de programação que você está usando, como c#:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

No c#, comentários de linha única são precedidos pelo `//` caracteres e comentários de várias linhas que começam com `/*` e terminar com `*/`. (Assim como acontece com comentários Razor, c# comentários não são processados no navegador.)

Para marcação, como você deve saber, você pode criar um comentário HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Comentários HTML iniciar com `<!--` caracteres e terminar com `-->`. Você pode usar comentários HTML ao redor não apenas texto, mas também qualquer marcação HTML que você talvez queira manter na página, mas não deseja processar. Este comentário HTML ocultará todo o conteúdo de marcas e o texto que contêm:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Ao contrário de comentários do Razor, comentários HTML *são* processado para a página e o usuário pode vê-los ao exibir a origem da página.

Razor tem limitações em blocos aninhados do c#. Para obter mais informações, consulte [chamado c# variáveis e aninhados blocos gerar desfeitos código](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Variáveis

Uma variável é um objeto nomeado que você usa para armazenar dados. Você pode nomear variáveis, mas o nome deve começar com um caractere alfabético e não pode conter espaço em branco ou caracteres reservados.

### <a name="variables-and-data-types"></a>Tipos de dados e variáveis

Uma variável pode ter um tipo de dados específico, que indica o tipo de dados é armazenado na variável. Você pode ter variáveis de cadeia de caracteres que armazenam valores de cadeia de caracteres (como &quot;Olá, mundo&quot;), variáveis de inteiro que armazenam valores de número inteiro (como 3 ou 79) e variáveis de data que armazenam valores de data em uma variedade de formatos (como 12/4/2012 ou de março de 2009 ). E há muitos outros tipos de dados que você pode usar.

No entanto, você geralmente não precisa especificar um tipo para uma variável. Na maioria das vezes, ASP.NET pode descobrir o tipo com base em como os dados na variável está sendo usados. (Ocasionalmente, você deve especificar um tipo, você verá exemplos em que isso é verdadeiro.)

Você declara uma variável usando o `var` palavra-chave (se não desejar especificar um tipo) ou usando o nome do tipo:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

O exemplo a seguir mostra alguns usos típicos de variáveis em uma página da web:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Se você combinar os exemplos anteriores em uma página, você verá exibidas em um navegador:

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Convertendo e tipos de dados de teste

Embora o ASP.NET geralmente pode determinar automaticamente um tipo de dados, às vezes, não pode ser. Portanto, você precisará ajudar ASP.NET executando uma conversão explícita. Mesmo se você não precisa converter os tipos de, às vezes é útil testar para ver o tipo de dados você pode estar trabalhando com.

O caso mais comum é que você precisa converter uma cadeia de caracteres para outro tipo, como em uma data ou um número inteiro. O exemplo a seguir mostra um caso comum em que você deve converter uma cadeia de caracteres em um número.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Como regra, entrada do usuário é fornecida a você como cadeias de caracteres. Mesmo que você tenha solicitado aos usuários inserir um número, e mesmo que foi inserido um dígito, quando a entrada do usuário é enviada e lê-lo no código, os dados estão em formato de cadeia de caracteres. Portanto, você deve converter a cadeia de caracteres em um número. No exemplo, se você tentar executar cálculos nos valores sem convertê-los, o seguinte erro resulta, porque o ASP.NET não é possível adicionar duas cadeias de caracteres:

*Não é possível converter implicitamente o tipo 'string' para 'int'.*

Para converter os valores inteiros, você deve chamar o `AsInt` método. Se a conversão for bem-sucedida, você pode adicionar os números.

A tabela a seguir lista alguns métodos comuns de conversão e teste para variáveis.

| **Método** | **Descrição** | **Exemplo** |
| --- | --- | --- |
| `AsInt(), IsInt()` | Converte uma cadeia de caracteres que representa um número inteiro (como "593") para um número inteiro. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)] |
| `AsBool(), IsBool()` | Converte uma cadeia de caracteres como &quot;true&quot; ou &quot;false&quot; para um tipo Boolean. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)] |
| `AsFloat(), IsFloat()` | Converte uma cadeia de caracteres que tem um valor decimal como &quot;1.3&quot; ou &quot;7.439&quot; para um número de ponto flutuante. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)] |
| `AsDecimal(), IsDecimal()` | Converte uma cadeia de caracteres que tem um valor decimal como &quot;1.3&quot; ou &quot;7.439&quot; para um número decimal. (No ASP.NET, um número decimal é mais preciso do que um número de ponto flutuante.) | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)] |
| `AsDateTime(), IsDateTime()` | Converte uma cadeia de caracteres que representa um valor de data e hora para o ASP.NET `DateTime` tipo. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)] |
| `ToString()` | Converte qualquer tipo de dados em uma cadeia de caracteres. | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)] |

## <a name="operators"></a>Operadores

Um operador é uma palavra-chave ou um caractere que diz ao ASP.NET que tipo de comando para executar em uma expressão. A linguagem c# (e a sintaxe do Razor que se baseia) dá suporte a muitos operadores, mas você precisa apenas reconhecer alguns para começar. A tabela a seguir resume os operadores mais comuns.

| **Operador** | **Descrição** | **Exemplos** |
| --- | --- | --- |
| `+` `-` `*` `/` | Operadores matemáticos usados em expressões numéricas. | [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)] |
| `=` | Atribuição. Atribui o valor à direita de uma instrução para o objeto no lado esquerdo. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)] |
| `==` | Igualdade. Retorna `true` se os valores são iguais. (Observe a diferença entre o `=` operador e o `==` operador.) | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)] |
| `!=` | Desigualdade. Retorna `true` se os valores não forem iguais. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)] |
| `< > <= >=` | Menos-que, maior-que less-than-or-equal e maior-than-or-equal. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)] |
| `+` | Concatenação, que é usada para unir cadeias de caracteres. ASP.NET sabe a diferença entre esse operador e o operador de adição com base no tipo de dados da expressão. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)] |
| `+=` `-=` | Os operadores de incremento e de decremento, adicionam e subtrair 1 (respectivamente) de uma variável. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)] |
| `.` | Ponto. Usado para distinguir os objetos e suas propriedades e métodos. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)] |
| `()` | Parênteses. Usado para agrupar expressões e passar parâmetros para métodos. | [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)] |
| `[]` | Colchetes. Usado para acessar valores em matrizes ou coleções. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)] |
| `!` | Não. Inverte uma `true` valor `false` e vice-versa. Normalmente usado como uma forma abreviada para testar o `false` (ou seja, para não `true`). | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)] |
| `&&` <code>&#124;&#124;</code> | AND lógico e, que são usados para vincular condições ou juntos. | [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)] |

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Trabalhar com caminhos de arquivo e pasta no código

Geralmente, você trabalhará com os caminhos de arquivo e pasta no seu código. Aqui está um exemplo da estrutura de pasta física para um site, como pode aparecer no computador de desenvolvimento:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Aqui estão alguns detalhes essenciais sobre URLs e caminhos:

- Uma URL começa com a um nome de domínio (`http://www.example.com`) ou um nome de servidor (`http://localhost`, `http://mycomputer`).
- Uma URL corresponde a um caminho físico em um computador host. Por exemplo, `http://myserver` deve corresponder à pasta *C:\websites\mywebsite* no servidor.
- Um caminho virtual é abreviado para representar os caminhos de código sem a necessidade de especificar o caminho completo. Ele inclui a parte de uma URL que segue o nome de domínio ou servidor. Quando você usa caminhos virtuais, você pode mover seu código para um domínio diferente ou servidor sem ter que atualizar os caminhos.

Aqui está um exemplo para ajudá-lo a entender as diferenças:

| URL completa | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nome do servidor | *mycompanyserver* |
| Caminho virtual | */humanresources/CompanyPolicy.htm* |
| Caminho físico | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

É a raiz virtual /, assim como a raiz da unidade c: unidade é \. (Os caminhos de pasta virtual sempre usam barras "/"). O caminho virtual de uma pasta não precisa ter o mesmo nome como a pasta física; é um alias. (Em servidores de produção, o caminho virtual raramente corresponde a um caminho físico exato.)

Quando você trabalha com arquivos e pastas no código, às vezes você precisa referenciar o caminho físico e, às vezes, um caminho virtual, dependendo de quais objetos você está trabalhando. ASP.NET oferece essas ferramentas para trabalhar com caminhos de arquivo e pasta no código: o `Server.MapPath` método e o `~` operador e `Href` método.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Conversão de caminhos virtuais físicos: o método MapPath

O `Server.MapPath` método converte um caminho virtual (como */default.cshtml*) em um caminho físico absoluto (como *C:\WebSites\MyWebSiteFolder\default.cshtml*). Você usar esse método sempre que você precisa de um caminho físico completo. Um exemplo típico é ao ler ou gravar um arquivo de texto ou imagem no servidor web.

Normalmente você não souber o caminho físico absoluto do seu site no servidor de hospedagem do site, para que esse método pode converter o caminho você souber — o caminho virtual — para o caminho correspondente no servidor para você. Passe o caminho virtual para um arquivo ou pasta para o método e retorna o caminho físico:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Fazendo referência a raiz virtual: o ~ operador e o método de Href

Em um *. cshtml* ou *. vbhtml* arquivo, você pode referenciar o caminho raiz virtual usando o `~` operador. Isso é muito útil porque você pode se mover páginas em um site, e os links para outras páginas contêm não será interrompidos. Também é útil no caso de você nunca mover seu site para um local diferente. Estes são alguns exemplos:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Se o site for `http://myserver/myapp`, aqui está como ASP.NET tratará esses caminhos quando a página é executada:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Esses caminhos, na verdade, não verá como os valores da variável, mas o ASP.NET tratará os caminhos como se o que é o que eles foram).

Você pode usar o `~` operador no código do servidor (como acima) e na marcação, como este:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Na marcação, você deve usar o `~` operador para criar caminhos para recursos, como arquivos CSS, outras páginas da web e arquivos de imagem. Quando a página é executada, o ASP.NET procura por meio da página (código e marcação) e resolve todos os `~` referências ao caminho adequado.

## <a name="conditional-logic-and-loops"></a>Loops e lógica condicional

Código de servidor do ASP.NET permite executar tarefas com base nas condições e escrever código que se repete instruções um número específico de vezes (ou seja, código que executa um loop).

### <a name="testing-conditions"></a>Condições de teste

Para testar uma condição simple é usar o `if` instrução, que retorna true ou false com base em um teste que você especificar:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

O `if` palavra-chave inicia um bloco. O teste real (condição) está entre parênteses e retorna true ou false. As instruções que são executados quando o teste é true são colocadas entre chaves. Um `if` instrução pode incluir um `else` bloco que especifica as instruções a serem executadas se a condição for false:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Você pode adicionar várias condições usando um `else if` bloco:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

Neste exemplo, se a primeira condição no se bloco não for true, o `else if` condição é verificada. Se essa condição for atendida, as instruções de `else if` bloco são executados. Se nenhuma das condições forem atendidas, as instruções de `else` bloco são executados. Você pode adicionar qualquer número de senão se bloqueia e, em seguida, feche com um `else` bloquear como o &quot;tudo&quot; condição.

Para testar um grande número de condições, use um `switch` bloco:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

O valor a ser testado está entre parênteses (no exemplo, o `weekday` variável). Cada teste individual usa um `case` instrução que termina com dois pontos (:). Se o valor de um `case` instrução corresponde ao valor de teste, o código no bloco caso é executado. Feche cada instrução case com um `break` instrução. (Se você se esquecer de incluir quebra em cada `case` bloquear, o código do próximo `case` instrução será executada também.) Um `switch` bloco geralmente tem um `default` instrução como o último caso para um &quot;tudo&quot; opção que é executado se nenhum dos outros casos é true.

O resultado dos últimos dois blocos condicionais exibido em um navegador:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Código de loop

Muitas vezes, é preciso executar as mesmas instruções repetidamente. Para fazer isso, um loop. Por exemplo, você sempre execute as mesmas instruções para cada item em uma coleção de dados. Se você souber exatamente quantas vezes desejar executar um loop, você pode usar um `for` loop. Esse tipo de loop é especialmente útil para de contagem ou contagem regressiva:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

O loop começa com o `for` palavra-chave, seguido de três instruções entre parênteses, cada um foi encerrado com um ponto e vírgula.

- Dentro dos parênteses, a primeira instrução (`var i=10;`) cria um contador e inicializa a 10. Você não tem o nome do contador `i` &#8212; você pode usar qualquer variável. Quando o `for` loop é executado, o contador é incrementado automaticamente.
- A segunda instrução (`i < 21;`) define a condição para a distância em que você deseja contar. Nesse caso, você quiser ir para um máximo de 20 (ou seja, continuar enquanto o contador é menos de 21).
- A terceira instrução (`i++` ) usa um operador de incremento, que só especifica que o contador deve ter 1 adicionado a cada vez que o loop é executado.

Dentro das chaves é o código que será executado em cada iteração do loop. A marcação cria um novo parágrafo (`<p>` elemento) cada vez e adiciona uma linha para a saída, exibindo o valor de `i` (o contador). Quando você executa esta página, o exemplo cria 11 linhas exibindo a saída, com o texto de cada linha que indica o número de item.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Se você estiver trabalhando com uma coleção ou matriz, você geralmente usa um `foreach` loop. Uma coleção é um grupo de objetos semelhantes e o `foreach` loop permite a você executa uma tarefa em cada item na coleção. Esse tipo de loop é conveniente para coleções, porque Diferentemente de uma `for` loop, você não precisa incrementar o contador ou definir um limite. Em vez disso, o `foreach` loop código simplesmente passa a coleção até que ela seja concluída.

Por exemplo, o código a seguir retorna os itens a `Request.ServerVariables` coleção, que é um objeto que contém informações sobre seu servidor web. Ele usa um `foreac` h loop para exibir o nome de cada item, criando um novo `<li>` elemento em uma lista com marcadores de HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

O `foreach` palavra-chave é seguido por parênteses, onde você pode declarar uma variável que representa um único item na coleção (no exemplo, `var item`), seguido de `in` palavra-chave, seguido de coleção que você deseja fazer o loop. No corpo do `foreach` loop, você pode acessar o item atual usando a variável que é declarado anteriormente.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Para criar um loop mais geral, use o `while` instrução:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Um `while` loop começa com o `while` palavra-chave, seguido de parênteses em que você especificar quanto tempo o loop continua (aqui, para desde que `countNum` é menor que 50), em seguida, o bloco para repetir. Loops normalmente incrementam (Adicionar a) ou de decremento (subtrair de) uma variável ou o objeto usado para contagem. No exemplo, o `+=` operador adiciona 1 à `countNum` cada vez que o loop é executado. (Para diminuir a uma variável em um loop que conta para baixo, você usaria o operador de decremento `-=`).

## <a name="objects-and-collections"></a>Objetos e coleções

Quase tudo em um site ASP.NET é um objeto, incluindo a própria página da web. Esta seção discute alguns objetos importantes que você trabalhará com frequência em seu código.

### <a name="page-objects"></a>Objetos de página

O objeto mais básico no ASP.NET é a página. Você pode acessar as propriedades do objeto page diretamente sem nenhum objeto qualificado. O código a seguir obtém o caminho do arquivo da página, usando o `Request` objeto da página:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Para tornar claro que você está fazendo referência a propriedades e métodos no objeto da página atual, você também pode usar a palavra-chave `this` para representar o objeto de página em seu código. Aqui está o exemplo de código anterior, com `this` adicionado para representar a página:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Você pode usar propriedades do `Page` objeto para obter muitas informações, como:

- `Request`. Como você já viu, esta é uma coleção de informações sobre a solicitação atual, incluindo o tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc.
- `Response`. Esta é uma coleção de informações sobre a resposta (página) que será enviada ao navegador quando o código de servidor concluiu a execução. Por exemplo, você pode usar essa propriedade para gravar informações na resposta. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Objetos de coleção (matrizes e dicionários)

Um *coleção* é um grupo de objetos do mesmo tipo, como uma coleção de `Customer` objetos de banco de dados. O ASP.NET contém várias coleções internas, como o `Request.Files` coleção.

Geralmente, você trabalhará com dados em coleções. Dois tipos de coleção comuns a *matriz* e *dicionário*. Uma matriz é útil quando você deseja armazenar uma coleção de itens semelhantes, mas não deseja criar uma variável separada para conter cada item:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Com matrizes, você declara um tipo de dados específico, como `string`, `int`, ou `DateTime`. Para indicar que a variável pode conter uma matriz, adicione parênteses à declaração (como `string[]` ou `int[]`). Você pode acessar itens em uma matriz usando sua posição (índice) ou usando o `foreach` instrução. Matriz de índices são com base em zero &#8212; Isto é, o primeiro item está na posição 0, o segundo item está na posição 1 e assim por diante.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Você pode determinar o número de itens em uma matriz obtendo seus `Length` propriedade. Para obter a posição de um item específico na matriz (para a matriz de pesquisa), use o `Array.IndexOf` método. Você também pode fazer coisas como reverter o conteúdo de uma matriz (a `Array.Reverse` método) ou classificar o conteúdo (o `Array.Sort` método).

A saída do código de matriz de cadeia de caracteres exibido em um navegador:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Um dicionário é uma coleção de pares chave/valor, em que você fornecer a chave (ou nome) para definir ou recuperar o valor correspondente:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Para criar um dicionário, use o `new` palavra-chave para indicar que você está criando um novo objeto de dicionário. Você pode atribuir um dicionário para uma variável usando o `var` palavra-chave. Indicar os tipos de dados dos itens no dicionário usando colchetes angulares ( `< >` ). No final da declaração, você deve adicionar um par de parênteses, porque isso é, na verdade, um método que cria um novo dicionário.

Para adicionar itens ao dicionário, você pode chamar o `Add` método da variável de dicionário (`myScores` nesse caso) e, em seguida, especifique uma chave e um valor. Como alternativa, você pode usar colchetes para indicar a chave e fazer uma atribuição simple, como no exemplo a seguir:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Para obter um valor do dicionário, você pode especificar a chave entre colchetes:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Chamando métodos com parâmetros

Durante a leitura deste artigo, os objetos que você programar com podem ter métodos. Por exemplo, um `Database` objeto pode ter um `Database.Connect` método. Muitos métodos também tem um ou mais parâmetros. Um *parâmetro* é um valor que você passa para um método para habilitar o método concluir a tarefa. Por exemplo, veja uma declaração para o `Request.MapPath` método, que usa três parâmetros:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(A linha foi quebrada para torná-lo mais legível. Lembre-se de que você pode colocar as quebras de linha quase qualquer local, exceto interna cadeias de caracteres são colocados entre aspas.)

Esse método retorna o caminho físico no servidor que corresponde a um caminho virtual especificado. Os três parâmetros para o método são `virtualPath`, `baseVirtualDir`, e `allowCrossAppMapping`. (Observe que na declaração, os parâmetros são listados com os tipos de dados dos dados que eles aceitará). Quando você chamar esse método, você deve fornecer valores para todos os três parâmetros.

A sintaxe do Razor fornece duas opções para passar parâmetros para um método: *parâmetros posicionais* e *parâmetros nomeados*. Para chamar um método usando parâmetros posicionais, você pode passar os parâmetros em uma ordem estrita que é especificada na declaração de método. (Você normalmente saberia nesta ordem lendo a documentação do método.) Você deve seguir a ordem, e você não pode ignorar qualquer um dos parâmetros &#8212; Se necessário, você passar uma cadeia de caracteres vazia (`""`) ou `null` para um parâmetro posicional que você não tem um valor.

O exemplo a seguir supõe que você tem uma pasta chamada *scripts* no seu site. O código chama o `Request.MapPath` método e passa valores para os três parâmetros na ordem correta. Em seguida, ele exibe o caminho de mapeada resultante.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Quando um método tem muitos parâmetros, você pode manter seu código mais legível usando parâmetros nomeados. Para chamar um método usando parâmetros nomeados, você especificar o nome do parâmetro seguido por dois-pontos (:) e, em seguida, o valor. A vantagem de parâmetros nomeados é que você pode passá-los em qualquer ordem desejada. (Uma desvantagem é que a chamada do método não é tão compacta).

O exemplo a seguir chama o método mesmo que acima, mas usa a parâmetros para fornecer os valores nomeados:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Como você pode ver, os parâmetros são passados em uma ordem diferente. No entanto, se você executar o exemplo anterior e neste exemplo, eles irá retornar o mesmo valor.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Manipulando erros

### <a name="try-catch-statements"></a>Instruções Try-Catch

Geralmente, você terá instruções no seu código que pode falhar por razões de fora de seu controle. Por exemplo:

- Se seu código tentar criar ou acessar um arquivo, todos os tipos de erros podem ocorrer. O arquivo que você deseja pode não existir, ele pode ser bloqueado, o código não pode ter permissões e assim por diante.
- Da mesma forma, se seu código tentar atualizar registros em um banco de dados, pode haver problemas de permissões, a conexão ao banco de dados pode ser descartado, os dados ao salvar podem ser inválida e assim por diante.

Em termos de programação, essas situações são chamadas *exceções*. Se seu código encontra uma exceção, ele gera (lança) uma mensagem de erro que 's, na melhor das hipóteses, irritantes aos usuários:

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

Em situações em que o seu código pode encontrar exceções e para evitar esse tipo de mensagem de erro, você pode usar `try/catch` instruções. No `try` instrução, você executa o código que você está verificando. Em um ou mais `catch` instruções, você pode procurar específico erros (tipos específicos de exceções) que possam ter ocorrido. Você pode incluir tantos `catch` instruções de como você precisam procurar erros antecipando a você.

> [!NOTE]
> É recomendável que você evite usar o `Response.Redirect` método `try/catch` instruções, porque ele pode causar uma exceção em sua página.


O exemplo a seguir mostra uma página que cria um arquivo de texto na primeira solicitação e, em seguida, exibe um botão que permite que o usuário abra o arquivo. O exemplo deliberadamente usa um nome de arquivo inválido para que ela fará com que uma exceção. Inclui o código `catch` instruções para duas exceções possíveis: `FileNotFoundException`, que ocorre se o nome de arquivo for inválido, e `DirectoryNotFoundException`, que ocorre se o ASP.NET ainda não é possível localizar a pasta. (Você pode remover o comentário uma instrução no exemplo para ver como ele é executado quando tudo está funcionando corretamente.)

Se seu código não lidar com a exceção, você verá uma página de erro como a captura de tela anterior. No entanto, o `try/catch` seção ajuda a impedir que o usuário ver esses tipos de erros.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

**Programação com o Visual Basic**


[Apêndice: linguagem Visual Basic e a sintaxe](https://go.microsoft.com/fwlink/?LinkId=202908)


**Documentação de referência**


[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[Linguagem c#](https://msdn.microsoft.com/library/kx37x362.aspx)
