---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Introdução ao ASP.NET Web Pages - Noções básicas de formulário HTML | Microsoft Docs
author: tfitzmac
description: Este tutorial mostra os fundamentos de como criar um formulário de entrada e como lidar com a entrada do usuário quando você usa o ASP.NET Web Pages (Razor). E agora que você...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: f5f7c9813041c443675f4e443822a81c1c508c20
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391985"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Introdução ao ASP.NET Web Pages - Noções básicas de formulário HTML
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra os fundamentos de como criar um formulário de entrada e como lidar com a entrada do usuário quando você usa o ASP.NET Web Pages (Razor). E agora que você tem um banco de dados, você usará o suas habilidades de formulário para permitir que os usuários a localizar filmes específicos no banco de dados. Ele pressupõe que você tenha concluído a série por meio [Introduction para exibir dados usando o ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> O que você aprenderá:
> 
> - Como criar um formulário usando elementos HTML padrão.
> - Como ler o usuário do entrada em um formulário.
> - Como criar uma consulta SQL que seletivamente obtém os dados usando uma pesquisa de termos que o usuário fornece.
> - Como fazer com que os campos na página "lembrar" o que o usuário inseriu.
>   
> 
> Recursos/tecnologias abordadas:
> 
> - O objeto `Request`.
> - O SQL `Where` cláusula.


## <a name="what-youll-build"></a>O que você vai criar

No tutorial anterior, você criou um banco de dados, adicionar dados a ele e usado, em seguida, o `WebGrid` auxiliar para exibir os dados. Neste tutorial, você adicionará uma caixa de pesquisa que permite que você encontre os filmes de um gênero específico ou cujo título contém qualquer palavra que você inserir. (Por exemplo, você poderá encontrar todos os filmes cujo gênero é "Ação" ou cujo título contém "Harry" ou "Adventure.")

Quando você concluir este tutorial, você terá uma página como esta:

![Página de filmes com pesquisa de título e gênero](form-basics/_static/image1.png)

A parte de listagem da página é o mesmo do último tutorial &mdash; uma grade. A diferença será que a grade mostrará apenas os filmes que você pesquisou.

## <a name="about-html-forms"></a>Sobre os formulários HTML

(Se você tem experiência com a criação de formulários HTML e com a diferença entre `GET` e `POST`, você poderá ignorar esta seção.)

Um formulário tem elementos de entrada do usuário &mdash; caixas de texto, botões, botões de opção, caixas de seleção, listas suspensas e assim por diante. Os usuários preenchem esses controles ou faça seleções e, em seguida, enviarem o formulário clicando em um botão.

A sintaxe básica de HTML de um formulário é ilustrada por este exemplo:

[!code-html[Main](form-basics/samples/sample1.html)]

Quando essa marcação é executado em uma página, ele cria um formulário simples que se parece com esta ilustração:

![Formulário básico de HTML como renderizado no navegador](form-basics/_static/image2.png)

O `<form>` elemento abrange elementos HTML para serem enviadas. (Um erro fácil fazer é adicionar elementos à página, mas esquecer de colocá-los dentro de um `<form>` elemento. Nesse caso, nada será enviado.) O `method` atributo informa ao navegador como enviar a entrada do usuário. Defina isso como `post` se você estiver executando uma atualização no servidor ou para `get` se você apenas estiver buscando dados do servidor.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST e segurança do verbo HTTP**
> 
> HTTP, o protocolo usado por navegadores e servidores para trocar informações, é surpreendentemente simple em suas operações básicas. Os navegadores usam apenas alguns verbos para fazer solicitações para servidores. Quando você escreve o código para a web, é útil entender esses verbos e usá-los como o navegador e o servidor. Longe os verbos mais comumente usados são estes:
> 
> - `GET`. O navegador usará esse verbo para buscar algo no servidor. Por exemplo, quando você digita uma URL no seu navegador, o navegador executa um `GET` operação para solicitar a página desejada. Se a página inclui elementos gráficos, o navegador executa adicionais `GET` operações para obter as imagens. Se o `GET` operação tem que passar informações para o servidor, as informações são passadas como parte da URL na cadeia de caracteres de consulta.
> - `POST`. O navegador envia um `POST` solicitação a fim de enviar dados a serem adicionadas ou alteradas no servidor. Por exemplo, o `POST` verbo é usado para criar registros em um banco de dados ou alterar os existentes. Na maioria das vezes, quando você preenche um formulário e clique no botão Enviar, o navegador executa um `POST` operação. Em um `POST` operação, os dados que está sendo passados para o servidor estão no corpo da página.
> 
> Uma importante distinção entre esses verbos é que um `GET` operação não deve alterar nada no servidor — ou colocá-lo de forma um pouco mais abstrata, um `GET` operação não resultará em uma alteração de estado no servidor. Você pode executar um `GET` operação nos mesmos recursos de quantas vezes desejar, e esses recursos não são alterados. (Um `GET` operação geralmente é dito "seguro", ou para usar um termo técnico, está *idempotente*.) Por outro lado, é claro, um `POST` solicitar alterações algo no servidor cada vez que você executa a operação.
> 
> Dois exemplos ajudarão a ilustrar essa distinção. Quando você executa uma pesquisa usando um mecanismo como o Bing ou o Google, você preenche um formulário que consiste em uma caixa de texto e, em seguida, clique no botão Pesquisar. O navegador executa um `GET` operação com o valor inserido na caixa de passada como parte da URL. Usando um `GET` operação para esse tipo de formulário é bom, porque uma operação de pesquisa não altera todos os recursos no servidor, ele simplesmente busca informações.
> 
> Agora, considere o processo de ordenação algo on-line. Preencha os detalhes do pedido e, em seguida, clique no botão Enviar. Esta operação será um `POST` solicitar, porque a operação resultará em alterações no servidor, como um novo registro de ordem, uma alteração nas informações de conta e talvez muitas outras alterações. Ao contrário de `GET` operação, você não pode se repetir seu `POST` solicitação — se você fez, cada vez que você reenviar a solicitação, geraria um novo pedido no servidor. (Em casos como esse, sites geralmente avisará que você não clique em um botão enviar mais de uma vez ou desabilitará o botão Enviar para que você não envie o formulário novamente acidentalmente.)
> 
> No decorrer deste tutorial, você vai usar um `GET` operação e um `POST` operação para trabalhar com formulários HTML. Vamos explicar cada motivo caso o verbo que você usa é aquele apropriado.
> 
> (Para saber mais sobre verbos HTTP, consulte o [definições de método](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artigo no site do W3C.)


A maioria dos elementos de entrada do usuário são HTML `<input>` elementos. Elas se pareçam com `<input type="type" name="name">,` onde *tipo* indica o tipo de controle de entrada do usuário desejado. Esses elementos são mais comuns:

- Caixa de texto: `<input type="text">`
- Caixa de seleção: `<input type="check">`
- Botão de opção: `<input type="radio">`
- Botão: `<input type="button">`
- Botão de envio: `<input type="submit">`

Você também pode usar o `<textarea>` elemento para criar uma caixa de texto de várias linhas e o `<select>` elemento para criar uma lista suspensa ou uma lista rolável. (Para mais informações sobre HTML formarem elementos, consulte [formulários HTML e a entrada](http://www.w3schools.com/html/html_forms.asp) no site W3Schools.)

O `name` atributo é muito importante, porque o nome é como você obterá o valor do elemento mais tarde, como você verá em breve.

A parte interessante é que você, o desenvolvedor da página, fazer com a entrada do usuário. Não há nenhum comportamento interno associado a esses elementos. Em vez disso, você precisa obter os valores que o usuário tiver inserido ou selecionado e fazer algo com eles. Esse é o que você aprenderá neste tutorial.

> [!TIP] 
> 
> **HTML5 e formulários de entrada**
> 
> Como você deve saber, HTML está em transição; a versão mais recente (HTML5) inclui suporte para as formas mais intuitivas para que os usuários insiram informações. Por exemplo, em HTML5, você (o desenvolvedor da página) pode informar a página que você deseja que o usuário insira uma data. O navegador pode exibir automaticamente um calendário, em vez de exigir que o usuário insira uma data manualmente. No entanto, o HTML5 é novo e ainda não é suportado em todos os navegadores.
> 
> Páginas da Web do ASP.NET oferece suporte a HTML5 na medida em que faz o navegador do usuário de entrada. Para ter uma ideia dos novos atributos para o `<input>` elemento em HTML5, consulte [HTML &lt;entrada&gt; atributo de tipo](http://www.w3schools.com/html/html_form_input_types.asp) no site W3Schools.


## <a name="creating-the-form"></a>Criando o formulário

No WebMatrix, nos **arquivos** espaço de trabalho, abra o *Movies.cshtml* página.

Após o fechamento `</h1>` marca e antes da abertura `<div>` marca do `grid.GetHtml` chamar, adicione a seguinte marcação:

[!code-html[Main](form-basics/samples/sample2.html)]

Essa marcação cria um formulário que tenha uma caixa de texto denominada `searchGenre` e um botão Enviar. O texto da caixa e enviar o botão são incluídos em uma `<form>` elemento cujo `method` atributo é definido como `get`. (Lembre-se de que se você não colocar a caixa de texto e botão dentro de enviar um `<form>` elemento, nada será enviado quando você clica no botão.) Você usa o `GET` verbo aqui porque você está criando um formulário que não faz qualquer alteração no servidor — ele apenas resulta em uma pesquisa. (No tutorial anterior, você usou um `post` método, que é como você envia alterações para o servidor. Você verá que no próximo tutorial novamente.)

Execute a página. Embora você ainda não definiu nenhum comportamento para o formulário, você pode ver sua aparência:

![Página de filmes com caixa de pesquisa para gênero](form-basics/_static/image3.png)

Insira um valor na caixa de texto, como "Comédia." Em seguida, clique em **gênero pesquisa**.

Anote a URL da página. Porque você definiu a `<form>` do elemento `method` atributo `get`, o valor inserido é agora parte da cadeia de caracteres de consulta na URL, como este:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Valores de formulário de leitura

A página já contém um código que obtém dados do banco de dados e exibe os resultados em uma grade. Agora você precisa adicionar um código que lê o valor da caixa de texto para que você possa executar uma consulta SQL que inclui o termo de pesquisa.

Porque você definiu o método do formulário como `get`, você pode ler o valor digitado na caixa de texto usando código semelhante ao seguinte:

`var searchTerm = Request.QueryString["searchGenre"];`

O `Request.QueryString` objeto (o `QueryString` propriedade da `Request` objeto) inclui os valores dos elementos que foram enviados como parte do `GET` operação. O `Request.QueryString` propriedade contém um *coleção* (uma lista) dos valores que são enviados no formulário. Para obter qualquer valor individual, você deve especificar o nome do elemento que você deseja. É por isso que você precisa ter uma `name` atributo o `<input>` elemento (`searchTerm`) que cria a caixa de texto. (Para obter mais informações sobre o `Request` do objeto, consulte a [barra lateral](#BKMK_TheRequestObject) posteriormente.)

É simple o suficiente para ler o valor da caixa de texto. Porém, se o usuário não inserir nada em todos os na caixa de texto, mas clicou **pesquisa** de qualquer forma, você pode ignorar esse clique, já que não há nada para pesquisar.

O código a seguir é um exemplo que mostra como implementar essas condições. (Você não precisa adicionar esse código ainda; você terá de fazer isso em instantes).

[!code-csharp[Main](form-basics/samples/sample3.cs)]

O teste é dividida desta forma:

- Obter o valor de `Request.QueryString["searchGenre"]`, ou seja, o que foi inserido no valor de `<input>` elemento chamado `searchGenre`.
- Descobrir se ele está vazio usando o `IsEmpty` método. Esse método é o modo padrão para determinar se algo (por exemplo, um elemento de formulário) contém um valor. Mas, você se importa realmente somente se ele tiver *não* vazio, portanto...
- Adicione a `!` operador na frente do `IsEmpty` de teste. (O `!` significa que operador NOT lógico).

Em inglês sem formatação, todo o `if` condição traduz da seguinte maneira: *se o elemento de searchGenre do formulário não estiver vazio, então...*

Este bloco prepara o terreno para a criação de uma consulta que usa o termo de pesquisa. Você terá de fazer isso na próxima seção.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **O objeto de solicitação**
> 
> O `Request` objeto contém todas as informações que o navegador envia para seu aplicativo quando uma página é solicitada ou enviada. Este objeto inclui todas as informações fornecidas pelo usuário, como valores de caixa de texto ou um arquivo para carregar. Ele também inclui todos os tipos de informações adicionais, assim como os cookies, valores de cadeia de consulta de URL (se houver), o caminho do arquivo da página que está em execução, o tipo de navegador que o usuário está usando, a lista de idiomas que são definidas no navegador e muito mais.
> 
> O `Request` objeto é um *coleção* (lista) de valores. Você obtém um valor individual da coleção especificando seu nome:
> 
> `var someValue = Request["name"];`
> 
> O `Request` objeto realmente expõe várias subconjuntos. Por exemplo:
> 
> - `Request.Form` fornece valores de elementos dentro de enviado `<form>` elemento se a solicitação for uma `POST` solicitação.
> - `Request.QueryString` fornece apenas os valores na cadeia de caracteres de consulta da URL. (Em uma URL como `http://mysite/myapp/page?searchGenre=action&page=2`, o `?searchGenre=action&page=2` seção da URL é a cadeia de caracteres de consulta.)
> - `Request.Cookies` coleção fornece acesso aos cookies que o navegador enviou.
> 
> Para obter um valor que você sabe que está no formulário enviado, você pode usar `Request["name"]`. Como alternativa, você pode usar as versões mais específicas `Request.Form["name"]` (para `POST` solicitações) ou `Request.QueryString["name"]` (para `GET` solicitações). É claro *nome* é o nome do item a ser obtido.
> 
> O nome do item que você deseja obter deve ser exclusivo dentro da coleção que você está usando. É por isso que o `Request` objeto fornece, como os subconjuntos `Request.Form` e `Request.QueryString`. Suponha que sua página contém um elemento de formulário chamado `userName` e *também* contém um cookie chamado `userName`. Se você receber `Request["userName"]`, é ambíguo se deseja que o valor de formulário ou o cookie. No entanto, se você obtiver `Request.Form["userName"]` ou `Request.Cookie["userName"]`, você está sendo explícito sobre qual valor a ser obtido.
> 
> Ele é uma boa prática para ser específico e usar o subconjunto de `Request` que você está interessado, como `Request.Form` ou `Request.QueryString`. Para as páginas simples que você está criando neste tutorial, provavelmente não realmente faz alguma diferença. No entanto, quando você cria páginas mais complexas, usando a versão explícita `Request.Form` ou `Request.QueryString` pode ajudá-lo a evitar problemas que podem surgir quando a página contém um formulário (ou vários formulários), cookies, valores de cadeia de caracteres de consulta e assim por diante.


## <a name="creating-a-query-by-using-a-search-term"></a>Criando uma consulta usando um termo de pesquisa

Agora que você sabe como obter o termo de pesquisa que o usuário inseriu, você pode criar uma consulta que usa a ele. Lembre-se de que, para obter todos os itens de filme do banco de dados, você está usando uma consulta SQL que se parece com esta instrução:

`SELECT * FROM Movies`

Para obter somente determinados filmes, você precisa usar uma consulta que inclui um `Where` cláusula. Essa cláusula permite definir uma condição em que as linhas são retornadas pela consulta. Veja um exemplo:

`SELECT * FROM Movies WHERE Genre = 'Action'`

O formato básico é `WHERE column = value`. Você pode usar operadores diferentes, além de apenas `=`, como `>` (maior que), `<` (menor que), `<>` (igual a), `<=` (menor ou igual a), etc., dependendo o que você está procurando.

Caso você esteja se perguntando, instruções SQL não diferenciam maiusculas de minúsculas &mdash; `SELECT` é o mesmo que `Select` (ou até mesmo `select`). No entanto, as pessoas geralmente capitalizar as palavras-chave em uma instrução SQL, como `SELECT` e `WHERE`, para facilitar a leitura.

### <a name="passing-the-search-term-as-a-parameter"></a>Passando o termo de pesquisa como um parâmetro

Ao procurar por um gênero específico é muito fácil (`WHERE Genre = 'Action'`), mas você deseja ser capaz de procurar qualquer gênero inseridas pelo usuário. Para fazer isso, você cria como a consulta SQL que inclui um espaço reservado para o valor para pesquisar. Ele se parecerá com este comando:

`SELECT * FROM Movies WHERE Genre = @0`

O espaço reservado é a `@` caracteres seguidos por zero. Como você pode imaginar, uma consulta pode conter vários espaços reservados e seria nomeados `@0`, `@1`, `@2`, etc.

Para configurar a consulta e passar para ele, na verdade, o valor, você deve usar o código semelhante ao seguinte:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Esse código é semelhante a o que já foi feito para exibir dados na grade. As únicas diferenças são:

- A consulta contém um espaço reservado (`WHERE Genre = @0"`).
- A consulta é colocada em uma variável (`selectCommand`); antes, a consulta é passado diretamente para o `db.Query` método.
- Quando você chama o `db.Query` método, você passa a consulta e o valor a ser usado para o espaço reservado. (Se a consulta tiver vários espaços reservados, você deve passá-los como valores separados para o método.)

Se você reunir todos esses elementos, você obterá o código a seguir:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Importante!** Uso de espaços reservados (como `@0`) passar valores para um comando SQL está *extremamente importante* para segurança. A maneira que você vê-la aqui, com espaços reservados para dados da variável, é a única maneira de você deve construir comandos SQL.
> 
> Nunca construa uma instrução SQL reunindo valores que obter do usuário e texto literal (concatenar). Concatenação de entrada do usuário em uma instrução SQL é aberta no seu site para um *ataque de injeção de SQL* em que um usuário mal-intencionado envia valores para sua página que hack seu banco de dados. (Você pode ler mais no artigo [injeção de SQL](https://msdn.microsoft.com/library/ms161953.aspx) site do MSDN.)


## <a name="updating-the-movies-page-with-search-code"></a>Atualizar a página de filmes com código de pesquisa

Agora você pode atualizar o código na *Movies.cshtml* arquivo. Para começar, substitua o código no bloco de código na parte superior da página com este código:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

A diferença aqui é que você os colocou a consulta para o `selectCommand` variável, que você passará para `db.Query` mais tarde. Colocando a instrução SQL em uma variável permite alterar a instrução, que é o que você fará para executar a pesquisa.

Também removeu estas duas linhas, você vai colocar de volta no posteriormente:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Você não quiser executar a consulta ainda (ou seja, chame `db.Query`) e você não deseja inicializar o `WebGrid` auxiliar ainda qualquer um. Depois que você descobriu quais instrução SQL deve ser executado, você fará essas coisas.

Após esse bloco reconfigurado, você pode adicionar a nova lógica para lidar com a pesquisa. O código completo será a seguinte aparência. Atualize o código em sua página para que coincida com este exemplo:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

A página agora funciona da seguinte maneira. Sempre que a página é executada, o código abre o banco de dados e o `selectCommand` variável é definida como a instrução SQL que obtém todos os registros do `Movies` tabela. O código também inicializa o `searchTerm` variável.

No entanto, se a solicitação atual inclui um valor para o `searchGenre` elemento, o código define `selectCommand` para uma consulta diferente — ou seja, para um que inclua o `Where` cláusula para procurar um gênero. Ele também define `searchTerm` tudo o que foi passado para a caixa de pesquisa (que pode ser nada).

Independentemente de qual SQL instrução está em `selectCommand`, o código, em seguida, chama `db.Query` para executar a consulta, passando a ele qualquer é `searchTerm`. Se não houver nada `searchTerm`, não importa, porque nesse caso, não há nenhum parâmetro para passar o valor para `selectCommand` mesmo assim.

Por fim, o código inicializa o `WebGrid` auxiliar usando os resultados da consulta, como antes.

Você pode ver que, colocando a instrução SQL e o termo de pesquisa em variáveis, você adicionou flexibilidade ao código. Como você verá posteriormente no tutorial, você pode usar essa estrutura básica e Continue adicionando a lógica para diferentes tipos de pesquisas.

## <a name="testing-the-search-by-genre-feature"></a>Testar o recurso de pesquisa por gênero

No WebMatrix, execute as *Movies.cshtml* página. Você verá a página com a caixa de texto do gênero.

Insira um gênero que você inseriu para um dos seus registros de teste, clique **pesquisa**. Neste momento, você verá uma listagem dos apenas filmes que correspondem daquele gênero:

![Página de filmes listando depois de pesquisar por gênero 'Comedies'](form-basics/_static/image4.png)

Insira um gênero diferente e pesquise novamente. Tente inserir o gênero usando todas as letras em maiusculas ou minúsculas para que você possa ver que a pesquisa não diferencia maiusculas de minúsculas.

## <a name="remembering-what-the-user-entered"></a>"Lembrar" o que o usuário inseriu

Você deve ter notado que depois que você inseriu um gênero e clicado **gênero pesquisa**, você viu uma listagem para daquele gênero. No entanto, a caixa de texto de pesquisa estava vazia &mdash; em outras palavras, ele não Lembre-se você tivesse inserido.

É importante entender por que esse comportamento ocorre. Quando você enviar uma página, o navegador envia uma solicitação ao servidor web. Quando o ASP.NET obtém a solicitação, ele cria uma instância totalmente nova da página, executará o código e, em seguida, renderiza a página para o navegador novamente. Na verdade, no entanto, a página não sabe que você estava trabalhando apenas com uma versão anterior de si mesmo. Tudo o que ele sabe é que ele recebeu uma solicitação que tinha alguns dados de formulário nele.

Sempre que você solicita uma página &mdash; pela primeira vez ou enviando-o &mdash; você está obtendo uma nova página. O servidor web não tem memória de sua última solicitação. Também não o ASP.NET, e também não o navegador. A única conexão entre essas instâncias separadas da página é uma todos os dados transmitidos entre eles. Se você enviar uma página, por exemplo, a nova instância de página pode obter os dados de formulário que foi enviados pela instância anterior. (É outra maneira de passar dados entre páginas use cookies.)

Uma maneira formal para descrever essa situação é dizer que as páginas da web estão *sem monitoração de estado*. Servidores Web e as próprias páginas e os elementos na página não mantêm todas as informações sobre o estado anterior de uma página. A web foi projetada dessa forma porque a manutenção do estado de solicitações individuais seria rapidamente esgotar os recursos dos servidores web, que geralmente manipulam milhares, talvez até mesmo centenas de milhares de solicitações por segundo.

É por isso que a caixa de texto estava vazia. Depois que a página é enviada, o ASP.NET criado uma nova instância da página e foi executado por meio do código e marcação. Não havia nada em que o código que disse ASP.NET para colocar um valor na caixa de texto. Portanto, o ASP.NET não fez nada e a caixa de texto foi renderizada sem um valor nele.

Há, na verdade, uma maneira fácil de contornar esse problema. O gênero que você inseriu na caixa de texto *está* disponíveis para você no código &mdash; está em `Request.QueryString["searchGenre"]`.

Atualize a marcação da caixa de texto, de modo que o `value` atributo obtém seu valor de `searchTerm`, como neste exemplo:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Nessa página, você pode ter também definir a `value` de atributo para o `searchTerm` variável, uma vez que essa variável também contém o gênero que você inseriu. Mas o uso de `Request` objeto para definir o `value` atributo conforme mostrado aqui é o modo padrão para realizar essa tarefa. (Supondo que você deseja fazer isso até mesmo &mdash; em algumas situações, você talvez queira processar a página *sem* valores nos campos. Tudo depende o que está acontecendo com seu aplicativo.)

> [!NOTE]
> Você não pode "lembrar" o valor de uma caixa de texto que é usado para senhas. Seria uma brecha de segurança para permitir que pessoas preencher um campo de senha por meio de código.


Execute novamente a página, insira um gênero e clique em **gênero pesquisa**. Desta vez não só você ver os resultados da pesquisa, mas a caixa de texto se lembra de que você inseriu última vez:

![Página mostrando que a caixa de texto tem 'lembradas' a entrada anterior](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Procurando por qualquer palavra no título

Agora você pode pesquisar qualquer gênero, mas você também poderá pesquisar um título. É difícil de acertar um título exatamente quando você pesquisa, portanto, em vez disso, que você pode pesquisar uma palavra que aparece em qualquer lugar dentro de um título. Para fazer isso no SQL, você deve usar o `LIKE` operador e a sintaxe semelhante ao seguinte:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Esse comando obtém todos os filmes cujos títulos contêm "adventure". Quando você usa o `LIKE` operador, você incluir o caractere curinga `%` como parte do termo de pesquisa. A pesquisa `LIKE 'adventure%'` significa "começando com 'adventure'". (Tecnicamente, isso significa "A cadeia de caracteres 'adventure', seguida por qualquer coisa.") Da mesma forma, o termo de pesquisa `LIKE '%adventure'` significa que "nada seguido pela cadeia de caracteres 'adventure'", que é outra maneira de dizer "termina com 'adventure'".

O termo de pesquisa `LIKE '%adventure%'` , portanto, significa "com a"adventure' em qualquer lugar no título." (Tecnicamente, "qualquer coisa no título, seguido de 'adventure', seguida por qualquer coisa".)

Dentro de `<form>` elemento, adicione a seguinte marcação imediatamente sob o fechamento `</div>` marca para a pesquisa de gênero (antes do fechamento `</form>` elemento):

[!code-html[Main](form-basics/samples/sample10.html)]

O código para lidar com essa pesquisa é semelhante ao código para a pesquisa de gênero, exceto que você precisa que montar o `LIKE` pesquisa. Dentro do bloco de código na parte superior da página, adicione `if` bloquear logo após o `if` bloco para a pesquisa de Gênero:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Esse código usa a mesma lógica que você viu anteriormente, exceto que a pesquisa usa um `LIKE` operador e coloca o código "`%`" antes e depois o termo de pesquisa.

Observe como ele foi fácil de adicionar outra pesquisa para a página. Tudo o que você precisava fazer era:

- Criar um `if` bloco testado para ver se a caixa de pesquisa relevantes tinha um valor.
- Defina o `selectCommand` variável para uma nova instrução SQL.
- Defina o `searchTerm` variável como o valor para passar para a consulta.

Aqui está o bloco de código completo, que contém a nova lógica de uma pesquisa de título:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Aqui está um resumo do que esse código faz:

- As variáveis `searchTerm` e `selectCommand` são inicializados na parte superior. Você vai definir essas variáveis para o termo de pesquisa apropriado (se houver) e o comando SQL apropriado com base em que o usuário faz na página. A pesquisa padrão é o caso mais simples de obter todos os filmes de banco de dados.
- Nos testes para `searchGenre` e `searchTitle`, o código define `searchTerm` para o valor que você deseja pesquisar. Os blocos de código também definir `selectCommand` para um comando SQL apropriado para que a pesquisa.
- O `db.Query` método é invocado apenas uma vez, usando qualquer outro comando SQL está em `selectedCommand` e o valor que está em `searchTerm`. Se não houver nenhum termo de pesquisa (sem gênero e nenhuma palavra título), o valor de `searchTerm` é uma cadeia de caracteres vazia. No entanto, isso não importa, porque nesse caso, a consulta não requer um parâmetro.

## <a name="testing-the-title-search-feature"></a>Testar o recurso de pesquisa de título

Agora você pode testar sua página de pesquisa concluída. Execute *Movies.cshtml*.

Insira um gênero e clique em **gênero pesquisa**. A grade exibe os filmes daquele gênero, como antes.

Insira uma palavra de título e clique em **pesquisar título**. A grade exibe filmes com essa palavra no título.

![Listagem depois de pesquisar por 'O' no título de página de filmes](form-basics/_static/image6.png)

Deixe as caixas de texto em branco e clique em um dos botões. A grade exibe todos os filmes.

## <a name="combining-the-queries"></a>Combinando as consultas

Você pode observar que as pesquisas que você pode executar são exclusivas. É possível pesquisar o título e o gênero ao mesmo tempo, mesmo que ambas as caixas de pesquisa têm valores neles. Por exemplo, você não pode procurar todos os filmes de ação cujo título contém "Adventure". (A página é codificado agora, se você inserir valores para o gênero e o título, a pesquisa de título obtém precedência). Para criar uma pesquisa que combina as condições, você precisaria criar uma consulta SQL que tem uma sintaxe semelhante ao seguinte:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

E você teria de executar a consulta usando uma instrução semelhante ao seguinte (em termos gerais):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Criar a lógica para permitir que muitas permutações de critérios de pesquisa pode obter um pouco envolvido, como você pode ver. Portanto, vamos parar aqui.

## <a name="coming-up-next"></a>Próximo

O próximo tutorial, você criará uma página que usa um formulário para permitir que usuários adicionem filmes no banco de dados.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Listagem completa para a página de filme (atualizada com a pesquisa)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação Web do ASP.NET usando a sintaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Cláusula WHERE SQL](http://www.w3schools.com/sql/sql_where.asp) no site W3Schools
- [As definições de método](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artigo no site do W3C

> [!div class="step-by-step"]
> [Anterior](displaying-data.md)
> [Próximo](entering-data.md)
