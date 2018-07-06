---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: Introdução à programação Web do ASP.NET usando a sintaxe do Razor (Visual Basic) | Microsoft Docs
author: tfitzmac
description: Este apêndice fornece uma visão geral da programação com páginas da Web ASP.NET no Visual Basic, usando a sintaxe do Razor.
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 72f995e62141df4e8f4cd082b4873d82067af8c1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816542"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a>Introdução à programação Web do ASP.NET usando a sintaxe do Razor (Visual Basic)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artigo fornece uma visão geral da programação com páginas da Web do ASP.NET usando a sintaxe do Razor e o Visual Basic. O ASP.NET é uma tecnologia da Microsoft para a execução de páginas da web dinâmicas em servidores web.
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


A maioria dos exemplos de como usar páginas da Web ASP.NET com sintaxe do Razor usar c#. Mas a sintaxe do Razor também suporta Visual Basic. Para programar uma página da web ASP.NET no Visual Basic, você cria uma página da web com um *. vbhtml* extensão de nome de arquivo e, em seguida, adicione o código do Visual Basic. Este artigo fornece uma visão geral de como trabalhar com a linguagem Visual Basic e a sintaxe para criar páginas da Web ASP.NET.

> [!NOTE]
> Os modelos de site padrão para o Microsoft WebMatrix (**padaria**, **Galeria de fotos**, e **Starter Site**, etc.) estão disponíveis nas versões c# e Visual Basic. Você pode instalar os modelos do Visual Basic, como pacotes NuGet. Modelos de site são instalados na pasta raiz do seu site em uma pasta chamada *Microsoft Templates*.


## <a name="the-top-8-programming-tips"></a>Principais dicas de programação 8

Esta seção lista algumas dicas que com certeza, você precisa saber como começar a escrever código de servidor ASP.NET usando a sintaxe Razor.

### <a name="1-you-add-code-to-a-page-using-the--character"></a>1. Adicione o código para uma página usando o caractere @

O `@` caractere inicia expressões embutidas, os blocos de instrução única e blocos de várias instruções:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

O resultado exibido em um navegador:

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> **A codificação HTML**
> 
> Quando você exibe o conteúdo em uma página usando o `@` de caracteres, como nos exemplos anteriores, ASP.NET codifica em HTML a saída. Isso substitui os caracteres reservados de HTML (tal como `<` e `>` e `&`) com os códigos que permitem que os caracteres a serem exibidos como caracteres em uma página da web, em vez de sejam interpretados como marcações HTML ou entidades. Sem codificação HTML, a saída do seu código de servidor não seja exibido corretamente e pode expor uma página a riscos de segurança.
> 
> Se sua meta é uma marcação HTML que processa marcas como marcação de saída (por exemplo `<p></p>` para um parágrafo ou `<em></em>` para enfatizar o texto), consulte a seção [combinação de texto, marcação e código em blocos de código](#BM_CombiningTextMarkupAndCode) mais adiante neste artigo.
> 
> Você pode ler mais sobre a codificação HTML no [trabalhando com formulários HTML em Sites de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202892).


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a>2. Coloque blocos de código com o código... Código de fim

Um bloco de código inclui uma ou mais instruções de código e está entre as palavras-chave `Code` e `End Code`. Coloque a abertura `Code` palavra-chave imediatamente após o `@` caractere &#8212; não pode haver espaço em branco entre eles.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

O resultado exibido em um navegador:

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a>3. Dentro de um bloco, você encerrar cada instrução de código com uma quebra de linha

Em um bloco de código do Visual Basic, cada instrução termina com uma quebra de linha. (Posteriormente neste artigo você verá uma maneira para encapsular uma instrução de código longo em várias linhas, se necessário.)

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a>4. Usar variáveis para armazenar valores

Você pode armazenar valores em uma *variável*, incluindo cadeias de caracteres, números e datas, etc. Você cria uma nova variável usando o `Dim` palavra-chave. Você pode inserir valores de variáveis diretamente em uma página usando `@`.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

O resultado exibido em um navegador:

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a>5. Coloque os valores de cadeia de caracteres literal entre aspas duplas

Um *cadeia de caracteres* é uma sequência de caracteres que são tratados como texto. Para especificar uma cadeia de caracteres, coloque-o entre aspas duplas:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

Para inserir aspas duplas dentro de um valor de cadeia de caracteres, insira dois caracteres de aspas duplas. Se você quiser que o caractere de aspas duplas para aparecer uma vez em que a saída da página, insira-o como `""` o entre aspas dentro de cadeia de caracteres e se você deseja que ele apareça duas vezes, insira-o como `""""` na cadeia de caracteres entre aspas.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

O resultado exibido em um navegador:

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a>6. Código do Visual Basic não diferencia maiusculas de minúsculas

A linguagem Visual Basic, não diferencia maiusculas de minúsculas. Palavras-chave de programação (como `Dim`, `If`, e `True`) e nomes de variáveis (como `myString`, ou `subTotal`) podem ser gravados em qualquer caso.

As seguintes linhas de código de atribuir um valor à variável `lastname` usando em letras minúsculas nomear e saída, em seguida, o valor da variável para a página usando um nome de letras maiusculas.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

O resultado exibido em um navegador:

![vb-syntax-5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a>7. Grande parte da sua codificação envolve o trabalho com objetos

Um objeto representa uma coisa que você pode programar com o &#8212; uma página, uma caixa de texto, um arquivo, uma imagem, uma solicitação da web, uma mensagem de email, um registro de cliente (linha de banco de dados), etc. Objetos têm propriedades que descrevem suas características de &#8212; um objeto de caixa de texto tem uma `Text` propriedade, um objeto de solicitação tem um `Url` propriedade, uma mensagem de email tem um `From` propriedade e um objeto do cliente tem um `FirstName` propriedade. Objetos também têm métodos que são as &quot;verbos&quot; eles podem executar. Exemplos incluem um objeto de arquivo `Save` método, um objeto de imagem `Rotate` método e um objeto de email `Send` método.

Geralmente, você trabalhará com o `Request` campos de objeto, que fornece informações como os valores de formulário na página (caixas de texto, etc.), o tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc. Este exemplo mostra como acessar propriedades do `Request` objeto e como chamar o `MapPath` método o `Request` objeto, que fornece o caminho absoluto da página no servidor:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

O resultado exibido em um navegador:

![Razor-Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a>8. Você pode escrever código que toma decisões

Um recurso importante de páginas da web dinâmicas é que você pode determinar o que fazer com base em condições. A maneira mais comum para isso é com o `If` instrução (e opcional `Else` instrução).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

A instrução `If IsPost` é uma forma abreviada da gravação `If IsPost = True`. Juntamente com `If` instruções, há uma variedade de maneiras de testar condições, repetidas blocos de código, e assim por diante, que são descritos neste artigo.

O resultado exibido em um navegador (depois de clicar em **enviar**):

![Razor-Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> **HTTP GET e POST métodos e a propriedade IsPost**
> 
> O protocolo usado para páginas da web (HTTP) dá suporte a um número muito limitado de métodos (&quot;verbos&quot;) que são usados para fazer solicitações ao servidor. Dois os mais comuns são GET, que é usado para ler uma página, e o POST, o que é usado para enviar uma página. Em geral, na primeira vez que um usuário solicita uma página, a página é solicitada usando GET. Se o usuário preenche um formulário e, em seguida, clica **enviar**, o navegador faz uma solicitação POST para o servidor.
> 
> Na programação da web, muitas vezes é útil saber se uma página está sendo solicitada como um GET ou como uma POSTAGEM para que você saiba como processar a página. Em páginas de Web do ASP.NET, você pode usar o `IsPost` propriedade para ver se uma solicitação é um GET ou uma POSTAGEM. Se a solicitação for uma POSTAGEM, o `IsPost` propriedade retornará true, e você pode fazer coisas como ler os valores das caixas de texto em um formulário. Muitos exemplos, você verá mostram como processar a página de forma diferente dependendo do valor de `IsPost`.


## <a name="a-simple-code-example"></a>Um exemplo de código simples

Este procedimento mostra como criar uma página que ilustra as técnicas de programação básicas. No exemplo, você cria uma página que permite aos usuários inserir dois números, em seguida, adiciona-os e exibe o resultado.

1. Em seu editor, crie um novo arquivo e nomeie- *AddNumbers.vbhtml*.
2. Copie o seguinte código e marcação para a página, substituindo qualquer coisa já na página.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    Aqui estão algumas coisas a observar:

    - O `@` caractere inicia o primeiro bloco de código na página, e precede o `totalMessage` variável inserido na parte inferior.
    - O bloco na parte superior da página é colocado entre `Code...End Code`.
    - As variáveis `total`, `num1`, `num2`, e `totalMessage` armazenar vários números e uma cadeia de caracteres.
    - O valor de cadeia de caracteres literal atribuído para o `totalMessage` variável está entre aspas duplas.
    - Porque o código do Visual Basic não diferencia maiusculas de minúsculas, quando o `totalMessage` variável é usada na parte inferior da página, seu nome somente precisa corresponder a ortografia de declaração da variável na parte superior da página. O uso de maiusculas e minúsculas não importa.
    - A expressão `num1.AsInt()`  +  `num2.AsInt()` mostra como trabalhar com objetos e métodos. O `AsInt` método em cada variável converte a cadeia de caracteres inserida por um usuário em um número inteiro (um inteiro) que pode ser adicionado.
    - O `<form>` marca inclui um `method="post"` atributo. Isso especifica que quando o usuário clica **adicionar**, a página será enviada ao servidor usando o método HTTP POST. Quando a página é enviada, o código `If IsPost` é avaliada como true e a condicional código é executado, exibindo o resultado da adição de números.
3. Salve a página e executá-lo em um navegador. (Certifique-se de que a página está selecionada na **arquivos** espaço de trabalho antes de executá-lo.) Insira dois números inteiros e, em seguida, clique no **adicionar** botão.

    ![Razor-Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a>Sintaxe e a linguagem Visual Basic

Anteriormente, você viu um exemplo básico de como criar uma página da web do ASP.NET e como você pode adicionar código de servidor a marcação HTML. Aqui você aprenderá as Noções básicas de como usar o Visual Basic para escrever código de servidor ASP.NET usando a sintaxe do Razor &#8212; ou seja, as regras linguagem de programação.

Se você tiver experiência com programação (especialmente se você já usou o C, C++, c#, Visual Basic ou JavaScript), grande parte o que você leia aqui será familiar. Você provavelmente precisará familiarizar-se apenas com como o código do WebMatrix é adicionado a marcação na *. vbhtml* arquivos.

### <a id="BM_CombiningTextMarkupAndCode"></a>  A combinação de texto, marcação e código em blocos de código

Em blocos de código do servidor, você geralmente deseja texto e marcação para a página de saída. Se um bloco de código de servidor contiver o texto que não é código e que em vez disso, deve ser renderizado como está, o ASP.NET precisa ser capaz de distinguir que o texto do código. Há várias maneiras de fazer isso.

- Colocar o texto em um elemento de bloco HTML, como `<p></p>` ou `<em></em>`:

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    O elemento HTML pode incluir texto, elementos HTML adicionais e expressões de código do servidor. Quando o ASP.NET vê a marca HTML de abertura (por exemplo, `<p>`), tudo o que renderiza o elemento e seu conteúdo como para o navegador (e resolve as expressões de código do servidor).

- Use o `@:` operador ou o `<text>` elemento. O `@:` gera uma única linha de conteúdo que contém o texto sem formatação ou marcas HTML sem correspondência; o `<text>` elemento abrange várias linhas de saída. Essas opções são úteis quando você não deseja renderizar um elemento HTML como parte da saída.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    O exemplo a seguir repete o exemplo anterior, mas usa um único par de `<text>` marcas para delimitar o texto a ser renderizado.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    No exemplo a seguir, o `<text>` e `</text>` marcas Coloque três linhas, todas com parte do texto dependente e marcas HTML não correspondentes (`<br />`), junto com código de servidor e as marcas HTML correspondentes. Novamente, você também pode preceder cada linha individualmente com a `@:` operador; ambas funcionam de forma.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > Quando você gerar o texto conforme mostrado nesta seção &#8212; usando um elemento HTML, o `@:` operador, ou o `<text>` elemento &#8212; ASP.NET não codificar com HTML a saída. (Conforme observado anteriormente, o ASP.NET codificar a saída de expressões de código do servidor e os blocos de código do servidor que são precedidos por `@`, exceto nos casos especiais observados nesta seção.)

### <a name="whitespace"></a>Whitespace

Os espaços extras em uma instrução (e fora de uma cadeia de caracteres literal) não afetam a instrução:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a>Dividir instruções longas em várias linhas

É possível dividir uma instrução de código longo em várias linhas usando o caractere de sublinhado `_` (que no Visual Basic é chamado de *caractere de continuação*) após cada linha de código. Para interromper uma instrução para a próxima linha, no final da linha, adicione um espaço e, em seguida, o caractere de continuação. A instrução continue na próxima linha. Você pode encapsular declarações em tantas linhas conforme necessário melhorar a legibilidade. As instruções a seguir são os mesmos:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

No entanto, você não pode inserir uma linha no meio de um literal de cadeia de caracteres. O exemplo a seguir não funciona:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

Para combinar uma cadeia de caracteres longa é quebrada para várias linhas, como o código acima, você precisaria usar o *operador de concatenação* (`&`), que você verá neste artigo.

### <a name="code-comments"></a>Comentários de código

Comentários permitem que você deixar observações para você ou terceiros. Comentários de sintaxe do Razor são prefixados com `@*` e terminar com `*@`.

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

Dentro de blocos de código, você pode usar os comentários de sintaxe do Razor, ou você pode usar o caractere de comentário Visual Basic comum, que é uma aspa simples (`'`) prefixado para cada linha.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a>Variáveis

Uma variável é um objeto nomeado que você pode usar para armazenar dados. Você pode nomear variáveis, mas o nome deve começar com um caractere alfabético e não pode conter espaço em branco ou caracteres reservados. No Visual Basic, como você viu anteriormente, não importa o caso das letras do nome de uma variável.

### <a name="variables-and-data-types"></a>Variáveis e tipos de dados

Uma variável pode ter um tipo de dados específico, que indica o tipo de dados é armazenado na variável. Você pode ter variáveis de cadeia de caracteres que armazenam valores de cadeia de caracteres (como &quot;Olá, mundo&quot;), variáveis de inteiro que armazenam valores de número inteiro (como 3 ou 79) e variáveis de data que armazenam valores de data em uma variedade de formatos (como 12/4/2012 ou de março de 2009 ). E há muitos outros tipos de dados que você pode usar.

No entanto, você não precisa especificar um tipo para uma variável. Na maioria dos casos ASP.NET pode descobrir o tipo com base em como os dados na variável está sendo usados. (Ocasionalmente, você deve especificar um tipo; você verá exemplos em que isso é verdadeiro).

Para declarar uma variável sem especificar um tipo, use `Dim` além do nome de variável (por exemplo, `Dim myVar`). Para declarar uma variável com um tipo, use `Dim` mais o nome da variável, seguido por `As` e, em seguida, o nome do tipo (por exemplo, `Dim myVar As String`).

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

O exemplo a seguir mostra algumas expressões embutidas que usam as variáveis em uma página da web.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

O resultado exibido em um navegador:

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a>Convertendo e tipos de dados de teste

Embora o ASP.NET geralmente pode determinar automaticamente um tipo de dados, às vezes, ele não é possível. Portanto, você pode precisar ajudar ASP.NET executando uma conversão explícita. Mesmo se você não precisa converter os tipos, às vezes é útil testar para ver qual tipo de dados você pode estar trabalhando com.

O caso mais comum é que você precisa converter uma cadeia de caracteres para outro tipo, como em um inteiro ou uma data. O exemplo a seguir mostra um caso típico onde você deve converter uma cadeia de caracteres em um número.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

Como uma regra de entrada do usuário são enviados a você como cadeias de caracteres. Mesmo se você tiver solicitado o usuário insira um número, e mesmo se eles inseriu um dígito, quando a entrada do usuário é enviada e lê-lo no código, os dados estão no formato de cadeia de caracteres. Portanto, você deve converter a cadeia de caracteres em um número. No exemplo, se você tentar executar cálculos nos valores sem convertê-los, o erro a seguir resulta, porque o ASP.NET não é possível adicionar duas cadeias de caracteres:

`Cannot implicitly convert type 'string' to 'int'.`

Para converter os valores inteiros, você deve chamar o `AsInt` método. Se a conversão for bem-sucedida, você pode adicionar os números.

A tabela a seguir lista alguns métodos comuns de conversão e teste para variáveis.


Iniciando linha:::::: coluna::: <strong>método</strong> ::: coluna final:::::: coluna::: <strong>descrição</strong> ::: coluna final:::::: coluna::: <strong>exemplo</strong> ::: coluna final:::::: final de linha:::
* * *
::: linha:::::: coluna 0x%2 `AsInt(), IsInt()` ::: coluna final:::::: coluna::: converte uma cadeia de caracteres que representa um número inteiro (como &quot;593&quot;) em um inteiro.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `AsBool(), IsBool()` ::: coluna final:::::: coluna::: converte uma cadeia de caracteres, como &quot;verdadeiro&quot; ou &quot;false&quot; para um tipo booleano.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    ::: end coluna:::::: final de linha:::
* * *
::: linha:::::: coluna 0x%2 `AsFloat(), IsFloat()` ::: coluna final:::::: coluna::: converte uma cadeia de caracteres que tem um valor decimal como &quot;1.3&quot; ou &quot;7.439&quot; um número de ponto flutuante.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    ::: end coluna:::::: final de linha:::
* * *
::: linha:::::: coluna 0x%2 `AsDecimal(), IsDecimal()` ::: coluna final:::::: coluna::: converte uma cadeia de caracteres que tem um valor decimal como &quot;1.3&quot; ou &quot;7.439&quot; em um número decimal. (No ASP.NET, um número decimal é mais preciso do que um número de ponto flutuante.) ::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    ::: end coluna:::::: final de linha:::
* * *
::: linha:::::: coluna 0x%2 `AsDateTime(), IsDateTime()` ::: coluna final:::::: coluna::: converte uma cadeia de caracteres que representa um valor de data e hora para o ASP.NET `DateTime` tipo.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `ToString()` ::: coluna final:::::: coluna::: converte qualquer tipo de dados em uma cadeia de caracteres.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    ::: end coluna:::::: final de linha:::


## <a name="operators"></a>Operadores

Um operador é uma palavra-chave ou um caractere que informa ao ASP.NET que tipo de comando para executar em uma expressão. Visual Basic oferece suporte a muitos operadores, mas você só precisa reconhecer algumas para começar a desenvolver páginas da web ASP.NET. A tabela a seguir resume os operadores mais comuns.


Iniciando linha:::::: coluna::: <strong>operador</strong> ::: coluna final:::::: coluna::: <strong>descrição</strong> ::: coluna final:::::: coluna::: <strong>exemplos</strong> ::: coluna final:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `+ - * /` ::: coluna final:::::: coluna::: operadores de cálculo usados em expressões numéricas.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `=` ::: end coluna:::::: coluna::: atribuição e igualdade. Dependendo do contexto, atribui o valor no lado direito de uma instrução para o objeto no lado esquerdo ou verifica os valores para igualdade.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `<>` ::: end coluna:::::: coluna::: desigualdade. Retorna `True` se os valores não forem iguais.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `< > <= >=` ::: coluna final:::::: coluna::: menor, maior, menor ou igual e maior ou igual.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `&` ::: coluna final:::::: coluna::: concatenação, que é usada para unir cadeias de caracteres.
::: end coluna:::::: coluna::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `+= -=` ::: coluna final:::::: coluna::: os operadores de incremento e decremento, que adicionar e subtrair 1 (respectivamente) a partir de uma variável.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `.` ::: coluna final:::::: coluna::: ponto. Usado para distinguir os objetos e suas propriedades e métodos.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `()` ::: end coluna:::::: coluna::: parênteses. Usado para agrupar expressões, para passar parâmetros para métodos e para acessar membros de coleções e matrizes.
::: end coluna:::::: coluna::: [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `Not` ::: end coluna:::::: coluna::: não. Inverte um valor true para false e vice-versa. Normalmente usado como uma forma abreviada para testar `False` (ou seja, para não `True`).
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    ::: end coluna:::::: final de linha:::
* * *
Iniciando linha:::::: coluna::: `AndAlso OrElse` ::: coluna final:::::: coluna::: lógica e e, que são usados para vincular condições ou juntos.
::: end coluna:::::: coluna::: [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    ::: end coluna:::::: final de linha:::

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

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a>Fazendo referência a raiz virtual: o ~ operador e o método de Href

Em um *. cshtml* ou *. vbhtml* arquivo, você pode referenciar o caminho virtual raiz usando o `~` operador. Isso é muito útil porque você pode mover páginas em um site, e todos os links para outras páginas contêm não será interrompidos. Também é útil caso você nunca move seu site para um local diferente. Estes são alguns exemplos:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

Se o site `http://myserver/myapp`, aqui está como ASP.NET irá tratar esses caminhos quando a página é executada:

- `myImagesFolder`: `http://myserver/myapp/images`
- `myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`

(Na verdade, você não verá esses caminhos como os valores da variável, mas ASP.NET tratará os caminhos como se isso é o que eles foram).

Você pode usar o `~` operador no código do servidor (como acima) e na marcação, como este:

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

Na marcação, você deve usar o `~` operador para criar caminhos a recursos como arquivos de imagem, outras páginas da web e arquivos CSS. Quando a página é executada, o ASP.NET procura por meio da página (código e marcação) e resolve todos os `~` referências para o caminho apropriado.

## <a name="conditional-logic-and-loops"></a>Loops e lógica condicional

Código de servidor do ASP.NET permite que você execute tarefas com base em condições e escreva o código que repete instruções que um número específico de vezes ou seja, código que executa um loop).

### <a name="testing-conditions"></a>Condições de testes

Para testar uma condição simple é usar o `If...Then` instrução, que retorna `True` ou `False` com base em um teste que você especificar:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

O `If` palavra-chave inicia um bloco. O teste real (condição) segue o `If` palavra-chave e retorna true ou false. O `If` instrução termina com `Then`. As instruções que serão executado se o teste seja verdadeiro são delimitadas por `If` e `End If`. Uma `If` instrução pode incluir um `Else` bloco que especifica as instruções a serem executadas se a condição for falsa:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

Se um `If` instrução se inicia um bloco de código, você não precisa usar o normal `Code...End Code` instruções para incluir os blocos. Você pode adicionar apenas `@` para o bloco, e ela funcionará. Essa abordagem funciona com `If` , bem como outro palavras-chave que são seguidas por blocos de código, incluindo de programação do Visual Basic `For`, `For Each`, `Do While`, etc.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

Você pode adicionar várias condições usando um ou mais `ElseIf` blocos:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

Neste exemplo, se a primeira condição na `If` bloco não for true, o `ElseIf` condição está marcada. Se essa condição for atendida, as instruções no `ElseIf` bloco são executados. Se nenhuma das condições forem atendidas, as instruções no `Else` bloco são executados. Você pode adicionar qualquer número de `ElseIf` bloqueia e, em seguida, fechar com uma `Else` bloquear como o &quot;todo o resto&quot; condição.

Para testar um grande número de condições, use um `Select Case` bloco:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

O valor a ser testado está entre parênteses (no exemplo, a variável de dia da semana). Cada teste individual usa um `Case` instrução que um valor de lista. Se o valor de uma `Case` instrução corresponde ao valor de teste, o código em que `Case` bloco é executado.

O resultado dos dois últimos blocos condicionais exibido em um navegador:

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a>Código de loop

Geralmente, você precisará executar as mesmas instruções repetidas vezes. Você pode fazer isso por meio de loops. Por exemplo, você sempre execute as mesmas instruções para cada item em uma coleção de dados. Se você souber exatamente quantas vezes você deseja executar um loop, você pode usar um `For` loop. Esse tipo de loop é especialmente útil para de contagem ou contagem regressiva:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

O loop começa com o `For` palavra-chave, seguido de três elementos:

- Imediatamente após o `For` a instrução, você declara uma variável de contador (você não precisa usar `Dim`) e, em seguida, indique o intervalo, como em `i = 10 to 20`. Isso significa que a variável `i` começará a contagem em 10 e continuar até que ele atinja 20 (inclusivo).
- Entre os `For` e `Next` instruções é o conteúdo do bloco. Isso pode conter um ou mais instruções de código que executarem com cada loop.
- O `Next i` instrução termina o loop. Ele incrementa o contador e começa a próxima iteração do loop.

A linha de código entre o `For` e `Next` linhas contém o código que é executado para cada iteração do loop. A marcação cria um novo parágrafo (`<p>` elemento) cada vez e adiciona uma linha na saída, exibindo o valor de i (o contador). Quando você executa essa página, o exemplo cria 11 linhas exibindo a saída, com o texto em cada linha que indica o número de item.

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

Se você estiver trabalhando com uma coleção ou matriz, você geralmente usa um `For Each` loop. Uma coleção é um grupo de objetos semelhantes e o `For Each` loop permite que você executar uma tarefa em cada item na coleção. Esse tipo de loop é conveniente para coleções, pois ao contrário de um `For` loop, você não precisa incrementar o contador ou definir um limite. Em vez disso, o `For Each` loop código simplesmente ocorrerá por meio da coleção, até que ela seja concluída.

Este exemplo retorna os itens a `Request.ServerVariables` coleção (que contém informações sobre seu servidor web). Ele usa um `For Each` loop para exibir o nome de cada item, criando um novo `<li>` elemento em uma lista com marcadores de HTML.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

O `For Each` palavra-chave é seguido por uma variável que representa um único item na coleção (no exemplo, `myItem`), seguido pelo `In` palavra-chave, seguido pela coleção que você deseja fazer o loop. No corpo do `For Each` loop, você pode acessar o item atual usando a variável declarada anteriormente por você.

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

Para criar um loop de propósito mais geral, use o `Do While` instrução:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

Esse loop começa com o `Do While` palavra-chave, seguido por uma condição, seguido pelo bloco repetir. Loops normalmente incrementam (Adicionar ao) ou decrementar (subtrair de) uma variável ou um objeto usado para contagem. No exemplo, o `+=` operador adiciona 1 ao valor de uma variável de cada vez que o loop é executado. (Para diminuir a uma variável em um loop de contagem regressiva, você usaria o operador de decremento `-=`.)

## <a name="objects-and-collections"></a>Objetos e coleções

Quase tudo em um site ASP.NET é um objeto, incluindo a própria página da web. Esta seção discute alguns objetos importantes que você trabalhará com frequência em seu código.

### <a name="page-objects"></a>Objetos de página

O objeto mais básico no ASP.NET é a página. Você pode acessar as propriedades do objeto page diretamente sem qualquer objeto qualificado. O código a seguir obtém o caminho do arquivo da página, usando o `Request` objeto da página:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

Você pode usar propriedades do `Page` objeto do qual obter muitas informações, tais como:

- `Request`. Como você já viu, esta é uma coleção de informações sobre a solicitação atual, incluindo o tipo de navegador fez a solicitação, a URL da página, a identidade do usuário, etc.
- `Response`. Esta é uma coleção de informações sobre a resposta (página) que será enviada ao navegador quando o código de servidor concluiu a execução. Por exemplo, você pode usar essa propriedade para gravar informações na resposta.

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a>Objetos de coleção (matrizes e dicionários)

Uma coleção é um grupo de objetos do mesmo tipo, como uma coleção de `Customer` objetos de banco de dados. ASP.NET contém várias coleções internas, como o `Request.Files` coleção.

Geralmente, você trabalhará com dados em coleções. Dois tipos de coleção comuns são as *array* e o *dicionário*. Uma matriz é útil quando você deseja armazenar uma coleção de itens semelhantes, mas não quiser criar uma variável separada para manter cada item:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

Com matrizes, você declara um tipo de dados específico, como `String`, `Integer`, ou `DateTime`. Para indicar que a variável pode conter uma matriz, adicione parênteses para o nome da variável na declaração (como `Dim myVar() As String`). Você pode acessar itens em uma matriz usando sua posição (índice) ou usando o `For Each` instrução. Índices de matriz são baseados em zero &#8212; ou seja, o primeiro item está na posição 0, o segundo item estiver na posição 1 e assim por diante.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

Você pode determinar o número de itens em uma matriz obtendo sua `Length` propriedade. Para obter a posição de um item específico na matriz (ou seja, para pesquisar a matriz), use o `Array.IndexOf` método. Você também pode fazer coisas como reverter o conteúdo de uma matriz (o `Array.Reverse` método) ou classificar o conteúdo (o `Array.Sort` método).

A saída do código de matriz de cadeia de caracteres exibido em um navegador:

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

Um dicionário é uma coleção de pares chave/valor, em que você fornece a chave (ou nome) para definir ou recuperar o valor correspondente:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

Para criar um dicionário, você deve usar o `New` palavra-chave para indicar que você está criando um novo `Dictionary` objeto. Você pode atribuir um dicionário para uma variável usando o `Dim` palavra-chave. Você indica os tipos de dados dos itens no dicionário usando parênteses ( `( )` ). No final da declaração, você deve adicionar outro par de parênteses, porque isso é, na verdade, um método que cria um novo dicionário.

Para adicionar itens ao dicionário, você pode chamar o `Add` método da variável de dicionário (`myScores` nesse caso) e, em seguida, especifique uma chave e um valor. Como alternativa, você pode usar parênteses para indicar a chave e fazer uma simples atribuição, como no exemplo a seguir:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

Para obter um valor do dicionário, você pode especificar a chave entre parênteses:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a>Chamar métodos com parâmetros

Como você viu neste artigo, os objetos que você programou com têm métodos. Por exemplo, uma `Database` objeto pode ter um `Database.Connect` método. Muitos métodos também tem um ou mais parâmetros. Um *parâmetro* é um valor que você passa para um método para habilitar o método concluir a tarefa. Por exemplo, examinar uma declaração para o `Request.MapPath` método, que usa três parâmetros:

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

Esse método retorna o caminho físico no servidor que corresponde a um caminho virtual especificado. Os três parâmetros para o método são `virtualPath`, `baseVirtualDir`, e `allowCrossAppMapping`. (Observe que na declaração, os parâmetros são listados com os tipos de dados dos dados que ele aceitará.) Quando você chama esse método, você deve fornecer valores para todos os três parâmetros.

Quando você estiver usando o Visual Basic com a sintaxe do Razor, você tem duas opções para passar parâmetros para um método: *parâmetros posicionais* ou *parâmetros nomeados*. Para chamar um método usando parâmetros posicionais, você pode passar os parâmetros em uma ordem estrita que é especificada na declaração de método. (Você normalmente saberia nesta ordem, lendo a documentação do método.) Você deve seguir a ordem, e você não pode ignorar qualquer um dos parâmetros &#8212; se necessário, você passa uma cadeia de caracteres vazia (`""`) ou para um parâmetro posicional que você não tiver um valor para null.

O exemplo a seguir pressupõe que você tem uma pasta chamada *scripts* em seu site. O código chama o `Request.MapPath` e passa valores para os três parâmetros na ordem correta. Ele, em seguida, exibe o caminho de mapeada resultante.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

Quando há muitos parâmetros para um método, você pode manter seu código mais limpo e legível usando parâmetros nomeados. Para chamar um método usando parâmetros nomeados, especifique o nome do parâmetro seguido por `:=` e, em seguida, forneça o valor. Uma vantagem dos parâmetros nomeados é que você pode adicioná-los em qualquer ordem desejada. (Uma desvantagem é que a chamada de método não é mais compacta.)

O exemplo a seguir chama o método acima, mas usa parâmetros para fornecer os valores nomeados:

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

Como você pode ver, os parâmetros são passados em uma ordem diferente. No entanto, se você executar o exemplo anterior e este exemplo, eles retornar o mesmo valor.

## <a name="handling-errors"></a>Manipulando erros

### <a name="try-catch-statements"></a>Instruções Try-Catch

Geralmente, você terá as instruções em seu código que pode falhar por razões de fora do seu controle. Por exemplo:

- Se seu código tenta abrir, criar, ler ou gravar um arquivo, todos os tipos de erros podem ocorrer. O arquivo que você deseja que pode não existir, ele pode ter sido bloqueado, o código pode não ter permissões e assim por diante.
- Da mesma forma, se seu código tenta atualizar registros em um banco de dados, pode haver problemas de permissões, a conexão ao banco de dados pode ser descartado, os dados para salvar podem estar inválido e assim por diante.

Em termos de programação, essas situações são chamadas *exceções*. Se seu código encontrar uma exceção, ele gera (gera) uma mensagem de erro que é, na melhor das hipóteses, irritantes aos usuários.

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

Em situações em que seu código poderá encontrar exceções e para evitar mensagens de erro desse tipo, você pode usar `Try/Catch` instruções. No `Try` instrução, que você executa o código que você está verificando. Em uma ou mais `Catch` instruções, você pode procurar específicos de erros (tipos específicos de exceções) que possam ter ocorrido. Você pode incluir tantos `Catch` instruções que você precisam procurar por erros que você está prevendo.

> [!NOTE]
> É recomendável que você evite usar o `Response.Redirect` método no `Try/Catch` instruções, porque isso pode causar uma exceção em sua página.


O exemplo a seguir mostra uma página que cria um arquivo de texto na primeira solicitação e, em seguida, exibe um botão que permite que o usuário abrir o arquivo. O exemplo deliberadamente usa um nome de arquivo incorreto para que ela fará com que uma exceção. O código inclui `Catch` instruções para duas exceções possíveis: `FileNotFoundException`, que ocorre se o nome do arquivo é ruim, e `DirectoryNotFoundException`, que ocorre se o ASP.NET ainda não é possível localizar a pasta. (Você pode remover o comentário uma instrução no exemplo a fim de ver como ele é executado quando tudo está funcionando corretamente.)

Se seu código não trata a exceção, você verá uma página de erro, como a captura de tela anterior. No entanto, o `Try/Catch` seção ajuda a impedir que o usuário ver esses tipos de erros.

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a>Recursos adicionais

### <a name="reference-documentation"></a>Documentação de referência

- [ASP.NET](https://msdn.microsoft.com/library/ee532866.aspx)
- [Linguagem Visual Basic](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
