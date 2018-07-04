---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: Introdução à programação Web do ASP.NET usando a sintaxe do Razor (c#) | Microsoft Docs
author: tfitzmac
description: Este capítulo fornece uma visão geral da programação com páginas da Web do ASP.NET usando a sintaxe do Razor. O ASP.NET é uma tecnologia da Microsoft para executar o pa web dinâmico...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 6e0af63ffab5ce1a4d582cbe1e9456da20df2b64
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363397"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a>Introdução à programação Web do ASP.NET usando a sintaxe do Razor (c#)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo fornece uma visão geral da programação com páginas da Web do ASP.NET usando a sintaxe Razor. O ASP.NET é uma tecnologia da Microsoft para a execução de páginas da web dinâmicas em servidores web. Este artigo concentra-se em usando a linguagem de programação c#.
> 
> **O que você vai aprender**:
> 
> - Os 8 principais dicas para começar a programação de páginas Web ASP.NET usando a sintaxe do Razor de programação.
> - Conceitos básicos de programação, será necessário.
> - Qual código de servidor ASP.NET e a sintaxe do Razor é tudo sobre.
>   
> 
> ## <a name="software-versions"></a>Versões de software
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
>   
> 
> Este tutorial também funciona com ASP.NET Web Pages 2.


## <a name="the-top-8-programming-tips"></a>Principais dicas de programação 8

Esta seção lista algumas dicas que com certeza, você precisa saber como começar a escrever código de servidor ASP.NET usando a sintaxe Razor.

> [!NOTE]
> A sintaxe Razor é baseada na linguagem de programação c#, e que é o idioma que é usado com mais frequência com páginas da Web do ASP.NET. No entanto, a sintaxe do Razor também oferece suporte a linguagem Visual Basic e tudo o que você vê que você também pode fazer no Visual Basic. Para obter detalhes, consulte o Apêndice [linguagem Visual Basic e a sintaxe](https://go.microsoft.com/fwlink/?LinkId=202908).


Você pode encontrar mais detalhes sobre a maioria dessas técnicas de programação posteriormente neste artigo.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Adicione o código para uma página usando o caractere @

O `@` caractere inicia expressões embutidas, os blocos de instrução única e blocos de várias instruções:

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

Isso é que essas instruções aparência quando a página é executada em um navegador:

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> **A codificação HTML**
> 
> Quando você exibe o conteúdo em uma página usando o `@` de caracteres, como nos exemplos anteriores, ASP.NET codifica em HTML a saída. Isso substitui os caracteres reservados de HTML (tal como `<` e `>` e `&`) com os códigos que permitem que os caracteres a serem exibidos como caracteres em uma página da web, em vez de sejam interpretados como marcações HTML ou entidades. Sem codificação HTML, a saída do seu código de servidor não seja exibido corretamente e pode expor uma página a riscos de segurança.
> 
> Se sua meta é uma marcação HTML que processa marcas como marcação de saída (por exemplo `<p></p>` para um parágrafo ou `<em></em>` para enfatizar o texto), consulte a seção [combinação de texto, marcação e código em blocos de código](#BM_CombiningTextMarkupAndCode) mais adiante neste artigo.
> 
> Você pode ler mais sobre a codificação HTML no [trabalhando com formulários](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-in-braces"></a>2. Coloque blocos de código entre chaves

Um *bloco de código* inclui uma ou mais instruções de código e é colocado entre chaves.

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

O resultado exibido em um navegador:

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a>3. Dentro de um bloco, você encerrar cada instrução de código com um ponto e vírgula

Dentro de um bloco de código, cada instrução de código completo deve terminar com um ponto e vírgula. Expressões embutidas não terminam com ponto e vírgula.

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a>4. Usar variáveis para armazenar valores

Você pode armazenar valores em uma *variável*, incluindo cadeias de caracteres, números e datas, etc. Você cria uma nova variável usando o `var` palavra-chave. Você pode inserir valores de variáveis diretamente em uma página usando `@`.

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

O resultado exibido em um navegador:

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Coloque os valores de cadeia de caracteres literal entre aspas duplas

Um *cadeia de caracteres* é uma sequência de caracteres que são tratados como texto. Para especificar uma cadeia de caracteres, coloque-o entre aspas duplas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

Se a cadeia de caracteres que você deseja exibir contém um caractere de barra invertida ( `\` ) ou aspas duplas ( `"` ), use uma *literal de cadeia de caracteres textual* que é prefixado com o `@` operador. (No c#, a \ caractere tem um significado especial, a menos que você use uma cadeia de caracteres textual literal.)

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

Para inserir marcas de aspas duplas, use uma cadeia de caracteres textual literal e repita as aspas:

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

Aqui está o resultado do uso de ambos os exemplos em uma página:

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> Observe que o `@` caractere é usado para marcar os literais de cadeia de caracteres textual em c# e para marcar o código em páginas ASP.NET.


### <a name="6-code-is-case-sensitive"></a>6. Código diferencia maiusculas de minúsculas

No c#, palavras-chave (como `var`, `true`, e `if`) e nomes de variáveis diferenciam maiusculas de minúsculas. As seguintes linhas de código criam duas variáveis diferentes, `lastName` e `LastName.`

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

Se você declarar uma variável como `var lastName = "Smith";` e se você tentar referenciar essa variável em sua página como `@LastName`, ocorrerá um erro porque `LastName` não será reconhecida.

> [!NOTE]
> No Visual Basic, variáveis e palavras-chave são *não* diferencia maiusculas de minúsculas.


### <a name="7-much-of-your-coding-involves-objects"></a>7. Grande parte da sua codificação envolve objetos

Uma *objeto* representa uma coisa que você pode programar com &#8212; uma página, uma caixa de texto, um arquivo, uma imagem, uma solicitação da web, uma mensagem de email, um registro de cliente (linha de banco de dados), etc. Objetos têm propriedades que descrevem suas características e que você pode ler ou modificar &#8212; um objeto de caixa de texto tem uma `Text` propriedade (entre outros), um objeto de solicitação tem um `Url` propriedade, uma mensagem de email tem um `From` propriedade, e um objeto do cliente tem um `FirstName` propriedade. Objetos também têm métodos que são as &quot;verbos&quot; eles podem executar. Exemplos incluem um objeto de arquivo `Save` método, um objeto de imagem `Rotate` método e um objeto de email `Send` método.

Geralmente, você trabalhará com o `Request` do objeto, que oferece informações como os valores das caixas de texto (campos de formulário) na página, o tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc. O exemplo a seguir mostra como acessar propriedades do `Request` objeto e como chamar o `MapPath` método o `Request` objeto, que fornece o caminho absoluto da página no servidor:

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

O resultado exibido em um navegador:

![Razor-Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Você pode escrever código que toma decisões

Um recurso importante de páginas da web dinâmicas é que você pode determinar o que fazer com base em condições. A maneira mais comum para isso é com o `if` instrução (e opcional `else` instrução).

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

A instrução `if(IsPost)` é uma forma abreviada da gravação `if(IsPost == true)`. Juntamente com `if` instruções, há uma variedade de maneiras de testar condições, repetidas blocos de código, e assim por diante, que são descritos neste artigo.

O resultado exibido em um navegador (depois de clicar em **enviar**):

![Razor-Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a>HTTP GET e POST métodos e a propriedade IsPost
> 
> O protocolo usado para páginas da web (HTTP) dá suporte a um número muito limitado de métodos (verbos) que são usados para fazer solicitações ao servidor. Dois os mais comuns são GET, que é usado para ler uma página, e o POST, o que é usado para enviar uma página. Em geral, na primeira vez que um usuário solicita uma página, a página é solicitada usando GET. Se o usuário preenche um formulário e, em seguida, clica no botão Enviar, o navegador faz uma solicitação POST para o servidor.
> 
> Na programação da web, muitas vezes é útil saber se uma página está sendo solicitada como um GET ou como uma POSTAGEM para que você saiba como processar a página. Em páginas de Web do ASP.NET, você pode usar o `IsPost` propriedade para ver se uma solicitação é um GET ou uma POSTAGEM. Se a solicitação for uma POSTAGEM, o `IsPost` propriedade retornará true, e você pode fazer coisas como ler os valores das caixas de texto em um formulário. Muitos exemplos, você verá mostram como processar a página de forma diferente dependendo do valor de `IsPost`.


## <a name="a-simple-code-example"></a>Um exemplo de código simples

Este procedimento mostra como criar uma página que ilustra as técnicas de programação básicas. No exemplo, você cria uma página que permite aos usuários inserir dois números, em seguida, adiciona-os e exibe o resultado.

1. Em seu editor, crie um novo arquivo e nomeie- *AddNumbers.cshtml*.
2. Copie o seguinte código e marcação para a página, substituindo qualquer coisa já na página.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    Aqui estão algumas coisas a observar:

    - O `@` caractere inicia o primeiro bloco de código na página, e precede o `totalMessage` variável que é inserido na parte inferior da página.
    - O bloco na parte superior da página é colocado entre chaves.
    - No bloco na parte superior, todas as linhas terminam com um ponto e vírgula.
    - As variáveis `total`, `num1`, `num2`, e `totalMessage` armazenar vários números e uma cadeia de caracteres.
    - O valor de cadeia de caracteres literal atribuído para o `totalMessage` variável está entre aspas duplas.
    - Porque o código é diferencia maiusculas de minúsculas, quando o `totalMessage` variável é usada na parte inferior da página, seu nome deve corresponder exatamente a variável na parte superior.
    - A expressão `num1.AsInt() + num2.AsInt()` mostra como trabalhar com objetos e métodos. O `AsInt` método em cada variável converte a cadeia de caracteres inserida por um usuário em um número (um inteiro) para que você possa realizar aritmética nele.
    - O `<form>` marca inclui um `method="post"` atributo. Isso especifica que quando o usuário clica **adicionar**, a página será enviada ao servidor usando o método HTTP POST. Quando a página é enviada, o `if(IsPost)` teste for avaliado como true e a condicional código é executado, exibindo o resultado da adição de números.
3. Salve a página e executá-lo em um navegador. (Certifique-se de que a página está selecionada na **arquivos** espaço de trabalho antes de executá-lo.) Insira dois números inteiros e, em seguida, clique no **adicionar** botão. 

    ![Razor-Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a>Conceitos básicos de programação

Este artigo fornece uma visão geral da programação de web do ASP.NET. Não é um exame exaustivo, apenas um tour rápido por meio dos conceitos de programação que você usará com mais frequência. Mesmo assim, ele aborda a quase tudo o que você precisará começar com páginas da Web do ASP.NET.

Mas primeiro, uma pouco técnicas básicas.

### <a name="the-razor-syntax-server-code-and-aspnet"></a>A sintaxe do Razor, o código do servidor e o ASP.NET

Sintaxe do Razor é uma sintaxe de programação simple para inserir código baseado em servidor em uma página da web. Em uma página da web que usa a sintaxe do Razor, há dois tipos de conteúdo: código de conteúdo e o servidor do cliente. Conteúdo do cliente é as coisas que você está acostumado em páginas da web: marcação HTML (elementos), informações como o CSS, de estilo talvez alguns scripts de cliente, como JavaScript e texto sem formatação.

Sintaxe do Razor permite adicionar código de servidor para esse conteúdo de cliente. Se não houver código de servidor na página, o servidor executa o código em primeiro lugar, antes de enviar a página no navegador. Executando no servidor, o código pode executar tarefas que podem ser muito mais complexas de fazer usando o conteúdo do cliente usado sozinho, como acessar os bancos de dados baseados em servidor. Mais importante, código de servidor pode criar dinamicamente o conteúdo de cliente &#8212; ele pode gerar uma marcação HTML ou outro conteúdo em tempo real e, em seguida, enviá-lo no navegador junto com qualquer HTML estático que a página pode conter. Da perspectiva do navegador, cliente o conteúdo que é gerado pelo seu código de servidor não é diferente de qualquer outro conteúdo do cliente. Como você já viu, o código de servidor que é necessário é bastante simples.

Páginas da web ASP.NET que incluem a sintaxe do Razor têm uma extensão de arquivo especial (*. cshtml* ou *. vbhtml*). O servidor reconhece essas extensões, executa o código que é marcado com a sintaxe do Razor e, em seguida, envia a página no navegador.

### <a name="where-does-aspnet-fit-in"></a>Em que o ASP.NET se encaixa?

Sintaxe do Razor baseia-se em uma tecnologia da Microsoft chamada ASP.NET, que por sua vez, baseia-se no Microsoft .NET Framework. O.NET Framework é uma estrutura de programação intensa e abrangente da Microsoft para o desenvolvimento de praticamente qualquer tipo de aplicativo do computador. O ASP.NET é a parte do .NET Framework que é projetado especificamente para criação de aplicativos web. Os desenvolvedores usaram o ASP.NET para criar muitos dos maiores e mais alto tráfego sites do mundo. (Qualquer momento você ver a extensão de nome de arquivo *. aspx* como parte da URL em um site, você saberá que o site foi escrito usando o ASP.NET.)

A sintaxe do Razor oferece toda a potência do ASP.NET, mas usando uma sintaxe simplificada que é mais fácil de saber se você for um iniciante e o que torna você mais produtivo se você for um especialista. Mesmo que essa sintaxe é simple de usar, significa que sua família relação com o ASP.NET e o .NET Framework como seus sites se tornarem mais sofisticados, você tem o poder das estruturas maiores disponíveis para você.

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> **Classes e instâncias**
> 
> Código de servidor ASP.NET usa objetos, que por sua vez, são criados na ideia de classes. A classe é a definição ou modelo para um objeto. Por exemplo, um aplicativo pode conter um `Customer` classe que define as propriedades e métodos que precisa de qualquer objeto de cliente.
> 
> Quando o aplicativo precisa trabalhar com informações de cliente real, ele cria uma instância do (ou *instancia*) um objeto do cliente. Cada cliente individual é uma instância separada do `Customer` classe. Cada instância suporta as mesmas propriedades e métodos, mas os valores de propriedade para cada instância são normalmente é diferentes, pois cada objeto customer é exclusivo. No objeto de um cliente, o `LastName` propriedade poderia ser "Smith"; em outro objeto de cliente, o `LastName` propriedade poderia ser "Dias".
> 
> Da mesma forma, qualquer página da web individuais em seu site é um `Page` que é uma instância do objeto a `Page` classe. Um botão na página é uma `Button` objeto que é uma instância do `Button` classe e assim por diante. Cada instância tem suas próprias características, mas todos eles baseiam-se no que é especificado na definição de classe do objeto.


## <a name="basic-syntax"></a>Sintaxe básica

Anteriormente, você viu um exemplo básico de como criar uma página da Web do ASP.NET e como você pode adicionar código de servidor a marcação HTML. Aqui você aprenderá os conceitos básicos de escrever código de servidor ASP.NET usando a sintaxe do Razor &#8212; ou seja, as regras linguagem de programação.

Se você tiver experiência com programação (especialmente se você já usou o C, C++, c#, Visual Basic ou JavaScript), grande parte o que você leia aqui será familiar. Você provavelmente precisará familiarizar-se apenas com como o código do servidor é adicionado à marcação na *. cshtml* arquivos.

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a>A combinação de texto, marcação e código em blocos de código

Em blocos de código do servidor, você geralmente deseja saída texto ou marcação (ou ambos) para a página. Se um bloco de código de servidor contiver o texto que não é código e que em vez disso, deve ser renderizado como está, o ASP.NET precisa ser capaz de distinguir que o texto do código. Há várias maneiras de fazer isso.

- Colocar o texto em um elemento HTML, como `<p></p>` ou `<em></em>`:   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    O elemento HTML pode incluir texto, elementos HTML adicionais e expressões de código do servidor. Quando o ASP.NET vê a marca HTML de abertura (por exemplo, `<p>`), ele processa tudo, incluindo o elemento e seu conteúdo como é para o navegador, resolução de expressões de código do servidor quando ele passa.
- Use o `@:` operador ou o `<text>` elemento. O `@:` gera uma única linha de conteúdo que contém o texto sem formatação ou marcas HTML sem correspondência; o `<text>` elemento abrange várias linhas de saída. Essas opções são úteis quando você não deseja renderizar um elemento HTML como parte da saída.  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    Se você quiser várias linhas de texto ou marcas HTML não correspondentes de saída, você pode preceder cada linha com `@:`, ou você pode colocar a linha em um `<text>` elemento. Como o `@:` operador,`<text>` marcas são usadas pelo ASP.NET para identificar o conteúdo de texto e nunca são renderizadas na saída de página.

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    O primeiro exemplo repete o exemplo anterior, mas usa um único par de `<text>` marcas para delimitar o texto a ser renderizado. No segundo exemplo, o `<text>` e `</text>` marcas Coloque três linhas, todas com parte do texto dependente e marcas HTML não correspondentes (`<br />`), junto com código de servidor e as marcas HTML correspondentes. Novamente, você também pode preceder cada linha individualmente com a `@:` operador; ambas funcionam de forma.

    > [!NOTE]
    > Quando você gerar o texto conforme mostrado nesta seção &#8212; usando um elemento HTML, o `@:` operador, ou o `<text>` elemento &#8212; ASP.NET não codificar com HTML a saída. (Conforme observado anteriormente, o ASP.NET codificar a saída de expressões de código do servidor e os blocos de código do servidor que são precedidos por `@`, exceto nos casos especiais observados nesta seção.)

### <a name="whitespace"></a>Whitespace

Os espaços extras em uma instrução (e fora de uma cadeia de caracteres literal) não afetam a instrução:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

Uma quebra de linha em uma instrução não tem nenhum efeito sobre a instrução, e você pode encapsular instruções para facilitar a leitura. As instruções a seguir são os mesmos:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

No entanto, você não pode inserir uma linha no meio de um literal de cadeia de caracteres. O exemplo a seguir não funciona:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

Para combinar uma cadeia de caracteres longa é quebrada para várias linhas, como o código acima, há duas opções. Você pode usar o operador de concatenação (`+`), que você verá neste artigo. Você também pode usar o `@` caracteres para criar uma cadeia de caracteres textual literal, como você viu neste artigo. Você pode dividir literais de cadeia de caracteres textual em linhas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a>Código (e a marcação) comentários

Comentários permitem que você deixar observações para você ou terceiros. Eles também permitem que você desabilite (*comente*) uma seção de código ou marcação que você não deseja executar, mas deseja manter em sua página por enquanto.

Há diferentes de sintaxe para código do Razor em marcação HTML de comentário. Assim como acontece com todos os códigos do Razor, Razor comentários são processados (e, em seguida, removidos) no servidor antes que a página é enviada ao navegador. Portanto, a sintaxe de comentário do Razor permite colocar comentários no código (ou até mesmo na marcação) que você pode ver quando você editar o arquivo, mas que os usuários não veem, até mesmo na fonte de página.

Para comentários do Razor do ASP.NET, você iniciar o comentário com `@*` e encerrá-lo com `*@`. O comentário pode estar em uma linha ou várias linhas:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

Aqui está um comentário dentro de um bloco de código:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

Aqui é o mesmo bloco de código, com a linha de código comentada para que ele não será executado:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

Dentro de um bloco de código, como uma alternativa ao uso da sintaxe de comentário do Razor, você pode usar a sintaxe de comentário da linguagem de programação que você está usando, como c#:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

No c#, os comentários de linha única são precedidos pela `//` caracteres e comentários de várias linhas começam com `/*` e terminar com `*/`. (Assim como acontece com comentários em Razor, c# comentários não são renderizados no navegador.)

Para marcação, como você provavelmente sabe, você pode criar um comentário HTML:

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

Os comentários do HTML começam com `<!--` caracteres e terminam com `-->`. Você pode usar comentários HTML ao redor não apenas texto, mas também qualquer marcação HTML que você talvez queira manter na página, mas não deseja renderizar. Este comentário HTML ocultará a todo o conteúdo de marcas e o texto que contêm:

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

Ao contrário de comentários, comentários HTML do Razor *são* renderizado para a página e o usuário poderá vê-los ao exibir a origem da página.

O Razor tem limitações em blocos aninhados da linguagem c#. Para obter mais informações, consulte [variáveis c# chamado e aninhados blocos gerar quebrado código](http://aspnetwebstack.codeplex.com/workitem/1914)

## <a name="variables"></a>Variáveis

Uma variável é um objeto nomeado que você pode usar para armazenar dados. Você pode nomear variáveis, mas o nome deve começar com um caractere alfabético e não pode conter espaço em branco ou caracteres reservados.

### <a name="variables-and-data-types"></a>Variáveis e tipos de dados

Uma variável pode ter um tipo de dados específico, que indica o tipo de dados é armazenado na variável. Você pode ter variáveis de cadeia de caracteres que armazenam valores de cadeia de caracteres (como &quot;Olá, mundo&quot;), variáveis de inteiro que armazenam valores de número inteiro (como 3 ou 79) e variáveis de data que armazenam valores de data em uma variedade de formatos (como 12/4/2012 ou de março de 2009 ). E há muitos outros tipos de dados que você pode usar.

No entanto, você geralmente não precisa especificar um tipo para uma variável. Na maioria das vezes, ASP.NET pode descobrir o tipo com base em como os dados na variável está sendo usados. (Ocasionalmente, você deve especificar um tipo; você verá exemplos em que isso é verdadeiro).

Você declara uma variável usando o `var` palavra-chave (se você não quiser especificar um tipo) ou usando o nome do tipo:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

O exemplo a seguir mostra alguns usos típicos de variáveis em uma página da web:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

Se você combinar os exemplos anteriores em uma página, você verá que eles são exibidos em um navegador:

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Convertendo e tipos de dados de teste

Embora o ASP.NET geralmente pode determinar automaticamente um tipo de dados, às vezes, ele não é possível. Portanto, você pode precisar ajudar ASP.NET executando uma conversão explícita. Mesmo se você não precisa converter os tipos, às vezes é útil testar para ver qual tipo de dados você pode estar trabalhando com.

O caso mais comum é que você precisa converter uma cadeia de caracteres para outro tipo, como em um inteiro ou uma data. O exemplo a seguir mostra um caso típico onde você deve converter uma cadeia de caracteres em um número.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

Como uma regra de entrada do usuário são enviados a você como cadeias de caracteres. Mesmo se você tiver solicitado que os usuários insiram um número, e mesmo se eles inseriu um dígito, quando a entrada do usuário é enviada e lê-lo no código, os dados estão no formato de cadeia de caracteres. Portanto, você deve converter a cadeia de caracteres em um número. No exemplo, se você tentar executar cálculos nos valores sem convertê-los, o erro a seguir resulta, porque o ASP.NET não é possível adicionar duas cadeias de caracteres:

*Não é possível converter implicitamente o tipo 'string' para 'int'.*

Para converter os valores inteiros, você deve chamar o `AsInt` método. Se a conversão for bem-sucedida, você pode adicionar os números.

A tabela a seguir lista alguns métodos comuns de conversão e teste para variáveis.

Iniciando linha:::::: coluna::: <strong>método</strong> ::: coluna final:::::: coluna::: <strong>descrição</strong> ::: coluna final:::::: coluna::: <strong>exemplo</strong> ::: coluna final:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `AsInt(), IsInt()` ::: coluna final:::::: coluna::: converte uma cadeia de caracteres que representa um número inteiro (como "593") em um inteiro.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `AsBool(), IsBool()` ::: coluna final:::::: coluna::: converte uma cadeia de caracteres, como &quot;verdadeiro&quot; ou &quot;false&quot; para um tipo booleano.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]
    ::: end coluna:::::: final de linha:::
* * *
::: linha:::::: coluna 0x%2 `AsFloat(), IsFloat()` ::: coluna final:::::: coluna::: converte uma cadeia de caracteres que tem um valor decimal como &quot;1.3&quot; ou &quot;7.439&quot; um número de ponto flutuante.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]
    ::: end coluna:::::: final de linha:::
* * *
::: linha:::::: coluna 0x%2 `AsDecimal(), IsDecimal()` ::: coluna final:::::: coluna::: converte uma cadeia de caracteres que tem um valor decimal como &quot;1.3&quot; ou &quot;7.439&quot; em um número decimal. (No ASP.NET, um número decimal é mais preciso do que um número de ponto flutuante.) ::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]
    ::: end coluna:::::: final de linha:::
* * *
::: linha:::::: coluna 0x%2 `AsDateTime(), IsDateTime()` ::: coluna final:::::: coluna::: converte uma cadeia de caracteres que representa um valor de data e hora para o ASP.NET `DateTime` tipo.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `ToString()` ::: coluna final:::::: coluna::: converte qualquer tipo de dados em uma cadeia de caracteres.
::: end coluna:::::: coluna::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]
    ::: end coluna:::::: final de linha:::

## <a name="operators"></a>Operadores

Um operador é uma palavra-chave ou um caractere que informa ao ASP.NET que tipo de comando para executar em uma expressão. A linguagem c# (e a sintaxe do Razor que se baseia) dá suporte a muitos operadores, mas você só precisa reconhecer algumas para começar a usar. A tabela a seguir resume os operadores mais comuns.


Iniciando linha:::::: coluna::: <strong>operador</strong> ::: coluna final:::::: coluna::: <strong>descrição</strong> ::: coluna final:::::: coluna::: <strong>exemplos</strong> ::: coluna final:::::: final de linha:::
* * *
Iniciando linha:::::: coluna iniciando `+` `-` `*` `/` ::: coluna final:::::: coluna::: operadores matemáticos usados em expressões numéricas.
::: end coluna:::::: coluna::: [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `=` ::: coluna final:::::: coluna::: atribuição. Atribui o valor no lado direito de uma instrução para o objeto no lado esquerdo.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `==` ::: end coluna:::::: coluna::: igualdade. Retorna `true` se os valores forem iguais. (Observe a diferença entre o `=` operador e o `==` operador.)::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `!=` ::: end coluna:::::: coluna::: desigualdade. Retorna `true` se os valores não forem iguais.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `< > <= >=` ::: coluna final:::::: coluna::: menor-que, maior-que less-than-or-equal e maior-than-or-equal.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `+` ::: coluna final:::::: coluna::: concatenação, que é usada para unir cadeias de caracteres. ASP.NET sabe a diferença entre esse operador e o operador de adição com base no tipo de dados da expressão.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna iniciando `+=` `-=` ::: coluna final:::::: coluna::: os operadores de incremento e decremento, que adicionar e subtrair 1 (respectivamente) a partir de uma variável.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `.` ::: coluna final:::::: coluna::: ponto. Usado para distinguir os objetos e suas propriedades e métodos.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `()` ::: end coluna:::::: coluna::: parênteses. Usado para agrupar expressões e passar parâmetros para métodos.
::: end coluna:::::: coluna::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `[]` ::: end coluna:::::: coluna::: colchetes. Usado para acessar valores em matrizes ou coleções.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `!` ::: end coluna:::::: coluna::: não. Inverte uma `true` valor `false` e vice-versa. Normalmente usado como uma forma abreviada para testar `false` (ou seja, para não `true`).
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `&&` <code>&#124;&#124;</code> ::: coluna final:::::: coluna::: lógica e e, que são usados para vincular condições ou juntos.
::: end coluna:::::: coluna::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]
    ::: end coluna:::::: final de linha:::

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a>Trabalhando com arquivos e caminhos de pasta no código

Geralmente, você trabalhará com caminhos de arquivo e pasta em seu código. Aqui está um exemplo da estrutura de pasta física para um site, como pode aparecer em seu computador de desenvolvimento:

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

Aqui estão alguns detalhes essenciais sobre URLs e caminhos:

- Uma URL começa com qualquer um de um nome de domínio (`http://www.example.com`) ou um nome de servidor (`http://localhost`, `http://mycomputer`).
- Uma URL corresponde a um caminho físico em um computador host. Por exemplo, `http://myserver` podem corresponder à pasta *C:\websites\mywebsite* no servidor.
- Um caminho virtual é uma abreviação para representar caminhos no código sem ter que especificar o caminho completo. Ele inclui a parte de uma URL que segue o nome de domínio ou servidor. Quando você usa caminhos virtuais, você pode mover seu código para um domínio diferente ou um servidor sem ter que atualizar os caminhos.

Aqui está um exemplo para ajudá-lo a entender as diferenças:

| URL completa | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| Nome do servidor | *mycompanyserver* |
| Caminho virtual | */humanresources/CompanyPolicy.htm* |
| Caminho físico | *C:\mywebsites\humanresources\CompanyPolicy.htm* |

É a raiz virtual /, assim como a raiz da unidade c: unidade é \. (Os caminhos de pasta virtual sempre usam barras "/"). O caminho virtual de uma pasta não precisa ter o mesmo nome que a pasta física; ele pode ser um alias. (Em servidores de produção, o caminho virtual raramente corresponde a um caminho físico exato.)

Quando você trabalha com arquivos e pastas no código, às vezes, você precisará referenciar o caminho físico e, às vezes, um caminho virtual, dependendo de quais objetos você está trabalhando com. ASP.NET fornece essas ferramentas para trabalhar com caminhos de arquivo e pasta no código: o `Server.MapPath` método e o `~` operador e `Href` método.

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a>Conversão de caminhos virtuais físicos: o método Server. MapPath

O `Server.MapPath` método converte um caminho virtual (como */default.cshtml*) em um caminho físico absoluto (como *C:\WebSites\MyWebSiteFolder\default.cshtml*). Você usar esse método sempre que precisar de um caminho físico completo. Um exemplo típico é quando você estiver lendo ou gravando um arquivo de texto ou arquivo de imagem no servidor web.

Normalmente você não souber o caminho físico absoluto do seu site no servidor de hospedagem do site, para que esse método possa converter o caminho que você sabe, o caminho virtual — para o caminho correspondente no servidor para você. Passe o caminho virtual para um arquivo ou pasta para o método e retorna o caminho físico:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Fazendo referência a raiz virtual: o ~ operador e o método de Href

Em um *. cshtml* ou *. vbhtml* arquivo, você pode referenciar o caminho virtual raiz usando o `~` operador. Isso é muito útil porque você pode mover páginas em um site, e todos os links para outras páginas contêm não será interrompidos. Também é útil caso você nunca move seu site para um local diferente. Estes são alguns exemplos:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

Se o site `http://myserver/myapp`, aqui está como ASP.NET irá tratar esses caminhos quando a página é executada:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Na verdade, você não verá esses caminhos como os valores da variável, mas ASP.NET tratará os caminhos como se isso é o que eles foram).

Você pode usar o `~` operador no código do servidor (como acima) e na marcação, como este:

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

Na marcação, você deve usar o `~` operador para criar caminhos a recursos como arquivos de imagem, outras páginas da web e arquivos CSS. Quando a página é executada, o ASP.NET procura por meio da página (código e marcação) e resolve todos os `~` referências para o caminho apropriado.

## <a name="conditional-logic-and-loops"></a>Loops e lógica condicional

Código de servidor do ASP.NET permite que você execute tarefas com base nas condições e escrever código que repete instruções um número específico de vezes (ou seja, código que executa um loop).

### <a name="testing-conditions"></a>Condições de testes

Para testar uma condição simple é usar o `if` instrução, que retorna true ou false com base em um teste que você especificar:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

O `if` palavra-chave inicia um bloco. O teste real (condição) está entre parênteses e retorna true ou false. As instruções que são executados se o teste seja verdadeiro são colocadas entre chaves. Uma `if` instrução pode incluir um `else` bloco que especifica as instruções a serem executadas se a condição for falsa:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

Você pode adicionar várias condições usando um `else if` bloco:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

Neste exemplo, se a primeira condição na se bloco não for true, o `else if` condição está marcada. Se essa condição for atendida, as instruções no `else if` bloco são executados. Se nenhuma das condições forem atendidas, as instruções no `else` bloco são executados. Você pode adicionar qualquer número de else if bloqueia e, em seguida, fechar com uma `else` bloquear como o &quot;todo o resto&quot; condição.

Para testar um grande número de condições, use um `switch` bloco:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

O valor a ser testado está entre parênteses (no exemplo, o `weekday` variável). Cada teste individual usa um `case` instrução que termina com dois-pontos (:). Se o valor de um `case` instrução corresponde ao valor de teste, o código daquele bloco case é executado. Fechar cada instrução case com uma `break` instrução. (Se você se esquecer de incluir quebra em cada `case` bloquear, o código do próximo `case` instrução serão executados também.) Um `switch` bloco geralmente tem uma `default` instrução como o último caso de uma &quot;todo o resto&quot; opção que é executado se nenhum dos outros casos forem verdadeiras.

O resultado dos dois últimos blocos condicionais exibido em um navegador:

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a>Código de loop

Geralmente, você precisará executar as mesmas instruções repetidas vezes. Você pode fazer isso por meio de loops. Por exemplo, você sempre execute as mesmas instruções para cada item em uma coleção de dados. Se você souber exatamente quantas vezes você deseja executar um loop, você pode usar um `for` loop. Esse tipo de loop é especialmente útil para de contagem ou contagem regressiva:

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

O loop começa com o `for` palavra-chave, seguido de três instruções entre parênteses, cada encerrada com ponto e vírgula.

- Dentro dos parênteses, a primeira instrução (`var i=10;`) cria um contador e o inicializa para 10. Não é necessário nomear o contador `i` &#8212; você pode usar qualquer variável. Quando o `for` loop é executado, o contador é incrementado automaticamente.
- A segunda instrução (`i < 21;`) define a condição para até onde você deseja contar. Nesse caso, você deseja que ele vá até um máximo de 20 (ou seja, continue enquanto o contador for menor que 21).
- A terceira instrução (`i++` ) usa um operador de incremento, que só especifica que o contador deve ter 1 adicionado a cada vez que o loop é executado.

Dentro das chaves é o código que será executado em cada iteração do loop. A marcação cria um novo parágrafo (`<p>` elemento) cada vez e adiciona uma linha na saída, exibindo o valor de `i` (o contador). Quando você executa essa página, o exemplo cria 11 linhas exibindo a saída, com o texto em cada linha que indica o número de item.

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

Se você estiver trabalhando com uma coleção ou matriz, você geralmente usa um `foreach` loop. Uma coleção é um grupo de objetos semelhantes e o `foreach` loop permite que você executar uma tarefa em cada item na coleção. Esse tipo de loop é conveniente para coleções, pois ao contrário de um `for` loop, você não precisa incrementar o contador ou definir um limite. Em vez disso, o `foreach` loop código simplesmente ocorrerá por meio da coleção, até que ela seja concluída.

Por exemplo, o código a seguir retorna os itens a `Request.ServerVariables` coleção, que é um objeto que contém informações sobre seu servidor web. Ele usa um `foreac` loop h para exibir o nome de cada item, criando um novo `<li>` elemento em uma lista com marcadores de HTML.

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

O `foreach` palavra-chave é seguido por parênteses, em que você declare uma variável que representa um único item na coleção (no exemplo, `var item`), seguido pelo `in` palavra-chave, seguido pela coleção que você deseja fazer o loop. No corpo do `foreach` loop, você pode acessar o item atual usando a variável declarada anteriormente por você.

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

Para criar um loop de propósito mais geral, use o `while` instrução:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

Um `while` loop começa com o `while` palavra-chave, seguido por parênteses, onde você especifica por quanto tempo o loop continua (aqui, para desde que `countNum` é menor que 50), em seguida, o bloco de repetir. Loops normalmente incrementam (Adicionar ao) ou decrementar (subtrair de) uma variável ou um objeto usado para contagem. No exemplo, o `+=` operador adiciona 1 ao `countNum` cada vez que o loop é executado. (Para diminuir a uma variável em um loop de contagem regressiva, você usaria o operador de decremento `-=`).

## <a name="objects-and-collections"></a>Objetos e coleções

Quase tudo em um site ASP.NET é um objeto, incluindo a própria página da web. Esta seção discute alguns objetos importantes que você trabalhará com frequência em seu código.

### <a name="page-objects"></a>Objetos de página

O objeto mais básico no ASP.NET é a página. Você pode acessar as propriedades do objeto page diretamente sem qualquer objeto qualificado. O código a seguir obtém o caminho do arquivo da página, usando o `Request` objeto da página:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

Para torná-lo claro que você está fazendo referência a propriedades e métodos no objeto da página atual, você pode usar a palavra-chave `this` para representar o objeto de página em seu código. Aqui está o exemplo de código anterior, com `this` adicionado para representar a página:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

Você pode usar propriedades do `Page` objeto do qual obter muitas informações, tais como:

- `Request`. Como você já viu, esta é uma coleção de informações sobre a solicitação atual, incluindo o tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc.
- `Response`. Esta é uma coleção de informações sobre a resposta (página) que será enviada ao navegador quando o código de servidor concluiu a execução. Por exemplo, você pode usar essa propriedade para gravar informações na resposta. 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a>Objetos de coleção (matrizes e dicionários)

Um *coleta* é um grupo de objetos do mesmo tipo, como uma coleção de `Customer` objetos de banco de dados. ASP.NET contém várias coleções internas, como o `Request.Files` coleção.

Geralmente, você trabalhará com dados em coleções. Dois tipos de coleção comuns são as *array* e o *dicionário*. Uma matriz é útil quando você deseja armazenar uma coleção de itens semelhantes, mas não quiser criar uma variável separada para manter cada item:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

Com matrizes, você declara um tipo de dados específico, como `string`, `int`, ou `DateTime`. Para indicar que a variável pode conter uma matriz, adicione parênteses para a declaração (como `string[]` ou `int[]`). Você pode acessar itens em uma matriz usando sua posição (índice) ou usando o `foreach` instrução. Índices de matriz são baseados em zero &#8212; ou seja, o primeiro item está na posição 0, o segundo item estiver na posição 1 e assim por diante.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

Você pode determinar o número de itens em uma matriz obtendo sua `Length` propriedade. Para obter a posição de um item específico na matriz (para pesquisar a matriz), use o `Array.IndexOf` método. Você também pode fazer coisas como reverter o conteúdo de uma matriz (o `Array.Reverse` método) ou classificar o conteúdo (o `Array.Sort` método).

A saída do código de matriz de cadeia de caracteres exibido em um navegador:

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

Um dicionário é uma coleção de pares chave/valor, em que você fornece a chave (ou nome) para definir ou recuperar o valor correspondente:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

Para criar um dicionário, você deve usar o `new` palavra-chave para indicar que você está criando um novo objeto de dicionário. Você pode atribuir um dicionário para uma variável usando o `var` palavra-chave. Você indica os tipos de dados dos itens no dicionário usando colchetes angulares ( `< >` ). No final da declaração, você deve adicionar um par de parênteses, porque isso é, na verdade, um método que cria um novo dicionário.

Para adicionar itens ao dicionário, você pode chamar o `Add` método da variável de dicionário (`myScores` nesse caso) e, em seguida, especifique uma chave e um valor. Como alternativa, você pode usar colchetes para indicar a chave e fazer uma simples atribuição, como no exemplo a seguir:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

Para obter um valor do dicionário, você pode especificar a chave entre colchetes:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a>Chamar métodos com parâmetros

Ao ler no início deste artigo, os objetos que você programa com podem ter métodos. Por exemplo, uma `Database` objeto pode ter um `Database.Connect` método. Muitos métodos também tem um ou mais parâmetros. Um *parâmetro* é um valor que você passa para um método para habilitar o método concluir a tarefa. Por exemplo, examinar uma declaração para o `Request.MapPath` método, que usa três parâmetros:

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

(A linha foi encapsulada para torná-lo mais legível. Lembre-se de que você pode colocar quebras de linha quase qualquer lugar exceto inside cadeias de caracteres que são colocados entre aspas.)

Esse método retorna o caminho físico no servidor que corresponde a um caminho virtual especificado. Os três parâmetros para o método são `virtualPath`, `baseVirtualDir`, e `allowCrossAppMapping`. (Observe que na declaração, os parâmetros são listados com os tipos de dados dos dados que ele aceitará.) Quando você chama esse método, você deve fornecer valores para todos os três parâmetros.

A sintaxe do Razor oferece duas opções para passar parâmetros para um método: *parâmetros posicionais* e *parâmetros nomeados*. Para chamar um método usando parâmetros posicionais, você pode passar os parâmetros em uma ordem estrita que é especificada na declaração de método. (Você normalmente saberia nesta ordem, lendo a documentação do método.) Você deve seguir a ordem, e você não pode ignorar qualquer um dos parâmetros &#8212; se necessário, você passa uma cadeia de caracteres vazia (`""`) ou `null` para um parâmetro posicional que você não tiver um valor para.

O exemplo a seguir pressupõe que você tem uma pasta chamada *scripts* em seu site. O código chama o `Request.MapPath` e passa valores para os três parâmetros na ordem correta. Ele, em seguida, exibe o caminho de mapeada resultante.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

Quando um método tem muitos parâmetros, você pode manter seu código mais legível usando parâmetros nomeados. Para chamar um método usando parâmetros nomeados, você especifica o nome do parâmetro seguido por dois-pontos (:) e, em seguida, o valor. A vantagem dos parâmetros nomeados é que você pode passá-los em qualquer ordem desejada. (Uma desvantagem é que a chamada de método não é mais compacta.)

O exemplo a seguir chama o método acima, mas usa parâmetros para fornecer os valores nomeados:

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

Como você pode ver, os parâmetros são passados em uma ordem diferente. No entanto, se você executar o exemplo anterior e este exemplo, eles retornar o mesmo valor.

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a>Manipulando erros

### <a name="try-catch-statements"></a>Instruções Try-Catch

Geralmente, você terá as instruções em seu código que pode falhar por razões de fora do seu controle. Por exemplo:

- Se seu código tentar criar ou acessar um arquivo, todos os tipos de erros podem ocorrer. O arquivo que você deseja que pode não existir, ele pode ter sido bloqueado, o código pode não ter permissões e assim por diante.
- Da mesma forma, se seu código tenta atualizar registros em um banco de dados, pode haver problemas de permissões, a conexão ao banco de dados pode ser descartado, os dados para salvar podem estar inválido e assim por diante.

Em termos de programação, essas situações são chamadas *exceções*. Se seu código encontrar uma exceção, ele gera (gera) uma mensagem de erro que está, na melhor das hipóteses, irritantes aos usuários:

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

Em situações em que seu código poderá encontrar exceções e para evitar mensagens de erro desse tipo, você pode usar `try/catch` instruções. No `try` instrução, que você executa o código que você está verificando. Em uma ou mais `catch` instruções, você pode procurar específicos de erros (tipos específicos de exceções) que possam ter ocorrido. Você pode incluir tantos `catch` instruções que você precisam procurar por erros que você está prevendo.

> [!NOTE]
> É recomendável que você evite usar o `Response.Redirect` método no `try/catch` instruções, porque isso pode causar uma exceção em sua página.


O exemplo a seguir mostra uma página que cria um arquivo de texto na primeira solicitação e, em seguida, exibe um botão que permite que o usuário abrir o arquivo. O exemplo deliberadamente usa um nome de arquivo incorreto para que ela fará com que uma exceção. O código inclui `catch` instruções para duas exceções possíveis: `FileNotFoundException`, que ocorre se o nome do arquivo é ruim, e `DirectoryNotFoundException`, que ocorre se o ASP.NET ainda não é possível localizar a pasta. (Você pode remover o comentário uma instrução no exemplo a fim de ver como ele é executado quando tudo está funcionando corretamente.)

Se seu código não trata a exceção, você verá uma página de erro, como a captura de tela anterior. No entanto, o `try/catch` seção ajuda a impedir que o usuário ver esses tipos de erros.

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

**Programação com o Visual Basic**


[Apêndice: linguagem Visual Basic e a sintaxe](https://go.microsoft.com/fwlink/?LinkId=202908)


**Documentação de referência**


[ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)

[Linguagem c#](https://msdn.microsoft.com/library/kx37x362.aspx)
