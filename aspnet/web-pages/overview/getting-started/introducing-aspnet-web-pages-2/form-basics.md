---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
title: Introdução ao ASP.NET Web Pages - Noções básicas do formulário HTML | Microsoft Docs
author: tfitzmac
description: Este tutorial mostra os fundamentos de como criar um formulário de entrada e como tratar a entrada do usuário quando você usar as páginas da Web do ASP.NET (Razor). E agora que você...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 81ed82bf-b940-44f1-b94a-555d0cb7cc98
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/form-basics
msc.type: authoredcontent
ms.openlocfilehash: 6f44f74774c2fa6338524987779e15f3940d1830
ms.sourcegitcommit: 9a35906446af7ffd4ccfc18daec38874b5abbef7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2018
ms.locfileid: "30898915"
---
<a name="introducing-aspnet-web-pages---html-form-basics"></a>Introdução a páginas da Web ASP.NET - Noções básicas do formulário HTML
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra os fundamentos de como criar um formulário de entrada e como tratar a entrada do usuário quando você usar as páginas da Web do ASP.NET (Razor). E agora que você tenha um banco de dados, você usará suas habilidades de formulário para permitir que os usuários a localizar filmes específicos no banco de dados. Ele pressupõe que você tenha concluído a série por meio de [Introdução a exibição de dados usando o ASP.NET Web Pages](/aspnet/web-pages/overview/getting-started/introducing-aspnet-web-pages-2/displaying-data).
> 
> O que você aprenderá:
> 
> - Como criar um formulário usando elementos HTML padrão.
> - Como ler o usuário de entrada em um formulário.
> - Como criar uma consulta SQL que seletivamente obtém os dados usando uma pesquisa de termos que o usuário fornece.
> - Como ter campos na página "lembrar" o que o usuário inseriu.
>   
> 
> Recursos/tecnologias abordadas:
> 
> - O objeto `Request`.
> - O SQL `Where` cláusula.


## <a name="what-youll-build"></a>O que você vai criar

No tutorial anterior, você criou um banco de dados, adicionar dados a ele e usado o `WebGrid` auxiliar para exibir os dados. Neste tutorial, você adicionará uma caixa de pesquisa que permite que você localize filmes de uma forma específica ou cujo título contém qualquer palavra que você inserir. (Por exemplo, você poderá encontrar todos os filmes cujo gênero é "Ação" ou cujo título contém "Harry" ou "Adventure.")

Quando você concluir este tutorial, você terá uma página como esta:

![Página de filmes com pesquisa de gênero e título](form-basics/_static/image1.png)

A parte de lista da página é o mesmo que o último tutorial &mdash; uma grade. A diferença será que a grade mostrará apenas os filmes que você pesquisou.

## <a name="about-html-forms"></a>Sobre formulários HTML

(Se você tem experiência com a criação de formulários HTML e a diferença entre `GET` e `POST`, você poderá ignorar esta seção.)

Um formulário tem os elementos de entrada de usuário &mdash; caixas de texto, botões, botões de opção, caixas de seleção, listas suspensas e assim por diante. Os usuários preenchem esses controles ou faça seleções e, em seguida, enviarem o formulário clicando em um botão.

A sintaxe básica de HTML de um formulário é ilustrada neste exemplo:

[!code-html[Main](form-basics/samples/sample1.html)]

Essa marcação é executada em uma página, ele cria um formulário simples que se parece com esta ilustração:

![Formato HTML básico como renderizado no navegador](form-basics/_static/image2.png)

O `<form>` elemento contém elementos HTML para serem enviados. (Um erro fácil fazer é adicionar elementos à página, mas esquecer de colocá-los dentro de um `<form>` elemento. Nesse caso, nada é enviado.) O `method` atributo informa ao navegador como enviar a entrada do usuário. Defina isso para `post` se você estiver executando uma atualização no servidor ou para `get` se você estiver buscando apenas dados do servidor.

<a id="GET,_POST,_and_HTTP_Verb_Safety"></a>

> [!TIP] 
> 
> **GET, POST e segurança de verbo HTTP**
> 
> HTTP, o protocolo usado por navegadores e servidores para trocar informações, é extremamente simple em suas operações básicas. Os navegadores usam apenas alguns verbos para fazer solicitações para servidores. Quando você escrever código para a web, é útil entender essas verbos e usá-los como o navegador e o servidor. Longe os verbos mais comumente usados são estes:
> 
> - `GET`. O navegador usa esse verbo para buscar algo do servidor. Por exemplo, quando você digita uma URL em seu navegador, o navegador executa um `GET` operação para solicitar a página que você deseja. Se a página inclui gráficos, o navegador executa adicionais `GET` operações para obter as imagens. Se o `GET` operação tem que passar informações para o servidor, as informações são passadas como parte da URL na cadeia de caracteres de consulta.
> - `POST`. O navegador envia um `POST` solicitação para enviar dados a serem adicionados ou alterados no servidor. Por exemplo, o `POST` verbo é usado para criar registros em um banco de dados ou alterar os existentes. Na maioria das vezes, quando você preenche um formulário e clique no botão Enviar, o navegador executa um `POST` operação. Em um `POST` operação, os dados que está sendo passados para o servidor estão no corpo da página.
> 
> Uma importante distinção entre esses verbos é que um `GET` operação não deve alterar nada no servidor — ou colocá-lo de forma um pouco mais abstrata, uma `GET` operação não resulta em uma alteração no estado do servidor. Você pode executar um `GET` operação nos mesmos recursos de quantas vezes desejar, e esses recursos não são alterados. (Um `GET` operação geralmente é chamada "segura", ou para usar um termo técnico, é *idempotente*.) Por outro lado, é claro, um `POST` solicitar alterações algo no servidor cada vez que você executar a operação.
> 
> Dois exemplos ajudarão a ilustrar essa distinção. Quando você executa uma pesquisa usando um mecanismo como o Bing ou o Google, preenchimento de um formulário que consiste em uma caixa de texto e, em seguida, clique no botão Pesquisar. O navegador executa um `GET` operação com o valor inserido na caixa passada como parte da URL. Usando um `GET` operação para esse tipo de formulário é bom, porque uma operação de pesquisa não altera todos os recursos no servidor, ele busca apenas informações.
> 
> Agora, considere o processo de ordenação algo on-line. Preencha os detalhes do pedido e, em seguida, clique no botão Enviar. Essa operação poderá ser um `POST` a solicitação, porque a operação resulta em alterações no servidor, como um novo registro de ordem, uma alteração em suas informações de conta e talvez muitas outras alterações. Diferentemente de `GET` operação, você não pode repetir o `POST` solicitação — se você fez, cada vez que reenviar a solicitação, você poderia gerar um novo pedido no servidor. (Em casos como esse, sites geralmente avisará que você não clique em um botão enviar mais de uma vez ou desabilitará o botão de envio, para que você não envie o formulário novamente acidentalmente.)
> 
> No decorrer deste tutorial, você vai usar um `GET` operação e um `POST` operação para trabalhar com formulários HTML. Explicaremos em cada caso porque o verbo que você usa é aquele apropriado.
> 
> (Para saber mais sobre os verbos HTTP, consulte o [definições de método](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artigo no site do W3C.)


A maioria dos elementos de entrada do usuário são HTML `<input>` elementos. Sua aparência `<input type="type" name="name">,` onde *tipo* indica o tipo de controle de entrada do usuário desejado. Esses elementos são mais comuns:

- Caixa de texto: `<input type="text">`
- Caixa de seleção: `<input type="check">`
- Botão de opção: `<input type="radio">`
- botão: `<input type="button">`
- Botão de envio: `<input type="submit">`

Você também pode usar o `<textarea>` elemento para criar uma caixa de texto de várias linhas e o `<select>` elemento para criar uma lista suspensa ou uma lista de rolagem. (Para mais informações sobre HTML formam elementos, consulte [formulários HTML e a entrada](http://www.w3schools.com/html/html_forms.asp) no site W3Schools.)

O `name` atributo é muito importante, porque o nome é como você obterá o valor do elemento posteriormente, como você verá em breve.

A parte interessante é que você, o desenvolvedor de página, fazer com a entrada do usuário. Não há nenhum comportamento interno associado a esses elementos. Em vez disso, você precisa obter os valores que o usuário tiver inserido ou selecionado e fazer algo com eles. É o que você aprenderá neste tutorial.

> [!TIP] 
> 
> **HTML5 e formulários de entrada**
> 
> Como você deve saber, HTML está em transição e a versão mais recente (HTML5) inclui suporte para formas mais intuitivas para os usuários insiram informações. Por exemplo, em HTML5, você (o desenvolvedor de página) pode determinar a página que você deseja que o usuário insira uma data. O navegador pode exibir automaticamente um calendário, em vez de exigir que o usuário insira uma data manualmente. No entanto, o HTML5 é novo e ainda não é suportado em todos os navegadores.
> 
> Páginas da Web ASP.NET dá suporte a HTML5 na medida em que a navegador do usuário faz de entrada. Para uma ideia dos novos atributos para o `<input>` elemento em HTML5, consulte [HTML &lt;entrada&gt; atributo de tipo](http://www.w3schools.com/html/html_form_input_types.asp) no site W3Schools.


## <a name="creating-the-form"></a>Criando o formulário

No WebMatrix, no **arquivos** espaço de trabalho, abra o *Movies.cshtml* página.

Após o fechamento `</h1>` marca e antes de abertura `<div>` marca das `grid.GetHtml` chamar, adicione a seguinte marcação:

[!code-html[Main](form-basics/samples/sample2.html)]

Essa marcação cria um formulário que tenha uma caixa de texto denominada `searchGenre` e um botão de envio. O texto da caixa e enviar botão são colocados em um `<form>` elemento cujo `method` atributo é definido como `get`. (Lembre-se de que se você não colocar a caixa de texto e enviar botão dentro de um `<form>` elemento, nada será enviado quando você clica no botão.) Você usa o `GET` verbo aqui porque você está criando um formulário que não faça alterações no servidor — apenas resulta em uma pesquisa. (No tutorial anterior, você usou um `post` método, que é como você enviar alterações para o servidor. Você verá que o tutorial Avançar novamente.)

Execute a página. Embora você não definiu nenhum comportamento para o formulário, você pode ver sua aparência:

![Página de filmes com a caixa de pesquisa para gênero](form-basics/_static/image3.png)

Insira um valor na caixa de texto, como "Comédia." Em seguida, clique em **pesquisa gênero**.

Anote a URL da página. Como você define o `<form>` do elemento `method` atributo `get`, o valor inserido é agora parte da cadeia de caracteres de consulta na URL, como este:

`http://localhost:45661/Movies.cshtml?searchGenre=Comedy`

## <a name="reading-form-values"></a>Valores de formulário de leitura

A página já contém um código que obtém dados do banco de dados e exibe os resultados em uma grade. Agora você precisa adicionar algum código que lê o valor da caixa de texto, você pode executar uma consulta SQL que inclui o termo de pesquisa.

Como definir o método do formulário `get`, você pode ler o valor inserido na caixa de texto usando código semelhante ao seguinte:

`var searchTerm = Request.QueryString["searchGenre"];`

O `Request.QueryString` objeto (o `QueryString` propriedade o `Request` objeto) inclui os valores dos elementos que foram enviados como parte do `GET` operação. O `Request.QueryString` propriedade contém um *coleção* (uma list) dos valores que são enviados no formulário. Para obter qualquer valor individual, especifique o nome do elemento que você deseja. É por isso que você precisa ter um `name` atributo no `<input>` elemento (`searchTerm`) que cria a caixa de texto. (Para obter mais informações sobre o `Request` de objeto, consulte o [barra lateral](#BKMK_TheRequestObject) posteriormente.)

É simple o suficiente para ler o valor da caixa de texto. Porém, se o usuário não insere nada em todos os na caixa de texto, mas clicou **pesquisa** mesmo assim, você pode ignorar esse clique, pois não há nada para pesquisar.

O código a seguir é um exemplo que mostra como implementar essas condições. (Você não precisa adicionar este código ainda; você fará isso mais tarde).

[!code-csharp[Main](form-basics/samples/sample3.cs)]

O teste divide desta forma:

- Obter o valor de `Request.QueryString["searchGenre"]`, ou seja, o valor que foi inserido no `<input>` elemento chamado `searchGenre`.
- Saber se ele está vazio usando o `IsEmpty` método. Esse método é o modo padrão para determinar se algo (por exemplo, um elemento de formulário) contém um valor. Mas realmente, você importa somente se ele tiver *não* vazia, portanto...
- Adicionar o `!` operador na frente do `IsEmpty` de teste. (O `!` operador significa não lógico).

Em inglês, todo o `if` condição traduz para o seguinte: *se o elemento de searchGenre do formulário não está vazio, em seguida,...*

Esse bloco define o estágio para criar uma consulta que usa o termo de pesquisa. Você vai fazer isso na próxima seção.

<a id="BKMK_TheRequestObject"></a>

> [!TIP] 
> 
> **O objeto de solicitação**
> 
> O `Request` objeto contém todas as informações que o navegador envia para seu aplicativo quando uma página for solicitada ou enviada. Este objeto inclui todas as informações fornecidas pelo usuário, como valores de caixa de texto ou um arquivo para carregar. Ele também inclui todos os tipos de informações adicionais, como cookies, os valores de cadeia de consulta de URL (se houver), o caminho do arquivo da página que está em execução, o tipo de navegador que o usuário está usando, a lista de idiomas que são definidas no navegador e muito mais.
> 
> O `Request` objeto é um *coleção* (lista) de valores. Você pode obter um valor individual fora da coleção especificando seu nome:
> 
> `var someValue = Request["name"];`
> 
> O `Request` objeto realmente expõe várias subconjuntos. Por exemplo:
> 
> - `Request.Form` fornece os valores dos elementos dentro de enviado `<form>` elemento se a solicitação for uma `POST` solicitação.
> - `Request.QueryString` fornece apenas os valores na cadeia de caracteres de consulta da URL. (Em uma URL como `http://mysite/myapp/page?searchGenre=action&page=2`, o `?searchGenre=action&page=2` seção da URL é a cadeia de caracteres de consulta.)
> - `Request.Cookies` coleção fornece acesso a cookies que o navegador enviou.
> 
> Para obter um valor que você sabe que está no formulário enviado, você pode usar `Request["name"]`. Como alternativa, você pode usar as versões mais específicas `Request.Form["name"]` (para `POST` solicitações) ou `Request.QueryString["name"]` (para `GET` solicitações). Obviamente, *nome* é o nome do item a ser obtido.
> 
> O nome do item que você deseja obter deve ser exclusivo dentro da coleção que você está usando. É por isso que o `Request` objeto fornece os subconjuntos como `Request.Form` e `Request.QueryString`. Suponha que sua página contém um elemento de formulário chamado `userName` e *também* contém um cookie chamado `userName`. Se você receber `Request["userName"]`, é ambíguo se desejar que o valor de formulário ou o cookie. No entanto, se você obtiver `Request.Form["userName"]` ou `Request.Cookie["userName"]`, você está sendo explícito sobre qual valor a ser obtido.
> 
> É uma boa prática para ser específico e usar o subconjunto de `Request` que você está interessado, como `Request.Form` ou `Request.QueryString`. Para as páginas simples que você está criando neste tutorial, ele provavelmente não faz qualquer diferença. No entanto, quando você cria páginas mais complexas, usando a versão de explícita `Request.Form` ou `Request.QueryString` pode ajudá-lo a evitar problemas que podem surgir quando a página contém um formulário (ou vários formulários), cookies, valores de cadeia de caracteres de consulta e assim por diante.


## <a name="creating-a-query-by-using-a-search-term"></a>Criando uma consulta usando um termo de pesquisa

Agora que você sabe como obter o termo de pesquisa que o usuário inseriu, você pode criar uma consulta que usa. Lembre-se de que, para obter todos os itens de filme fora do banco de dados, você está usando uma consulta SQL que se parece com esta instrução:

`SELECT * FROM Movies`

Para obter somente determinados filmes, você deve usar uma consulta que inclui um `Where` cláusula. Essa cláusula permite que você defina uma condição na qual as linhas são retornadas pela consulta. Veja um exemplo:

`SELECT * FROM Movies WHERE Genre = 'Action'`

O formato básico é `WHERE column = value`. Você pode usar operadores diferentes, além de apenas `=`, como `>` (maior que), `<` (menor que), `<>` (igual a), `<=` (menor ou igual a), etc., dependendo o que você está procurando.

Caso você esteja se perguntando, instruções SQL não diferenciam maiusculas de minúsculas &mdash; `SELECT` é o mesmo que `Select` (ou até mesmo `select`). No entanto, as pessoas geralmente aproveitar as palavras-chave em uma instrução SQL, como `SELECT` e `WHERE`, para facilitar a leitura.

### <a name="passing-the-search-term-as-a-parameter"></a>Passando o termo de pesquisa como um parâmetro

Procurando um gênero específico é muito fácil (`WHERE Genre = 'Action'`), mas você deseja ser capaz de procurar qualquer gênero inseridas pelo usuário. Para fazer isso, você cria como consulta SQL que inclua um espaço reservado para o valor a ser pesquisado. Ele se parecerá com este comando:

`SELECT * FROM Movies WHERE Genre = @0`

O espaço reservado é o `@` caractere seguido por zero. Como você pode imaginar, uma consulta pode conter vários espaços reservados e serão chamadas `@0`, `@1`, `@2`, etc.

Para configurar a consulta e, na verdade, ele passa o valor, você deve usar o código semelhante ao seguinte:

[!code-sql[Main](form-basics/samples/sample4.sql)]

Esse código é semelhante ao que você já fez para exibir dados na grade. As únicas diferenças são:

- A consulta contém um espaço reservado (`WHERE Genre = @0"`).
- A consulta é colocada em uma variável (`selectCommand`); antes, a consulta é passado diretamente para o `db.Query` método.
- Quando você chama o `db.Query` método, você passa a consulta e o valor a ser usado para o espaço reservado. (Se a consulta tiver vários espaços reservados, você passaria-los como valores separados para o método.)

Se você reunir todos esses elementos, você receberá o código a seguir:

[!code-csharp[Main](form-basics/samples/sample5.cs)]

> [!NOTE] 
> 
> **Importante!** Uso de espaços reservados (como `@0`) passar valores para um comando SQL é *extremamente importante* para segurança. A maneira como você vê-lo aqui, com espaços reservados para dados da variável, é a única maneira de você deve construir comandos SQL.
> 
> Nunca construa uma instrução SQL, reunindo texto literal (concatenação) e os valores que você obtém do usuário. Concatenação de entrada do usuário em uma instrução SQL abre o site para um *ataque de injeção de SQL* em que um usuário mal-intencionado envia valores para a página de hack seu banco de dados. (Você pode ler mais no artigo [injeção SQL](https://msdn.microsoft.com/library/ms161953.aspx) o site do MSDN.)


## <a name="updating-the-movies-page-with-search-code"></a>Atualizar a página de filmes com código de pesquisa

Agora você pode atualizar o código de *Movies.cshtml* arquivo. Para começar, substitua o código no bloco de código na parte superior da página com este código:

[!code-csharp[Main](form-basics/samples/sample6.cs)]

A diferença aqui é que você aplicou a consulta para o `selectCommand` variável, que você poderá passar para `db.Query` mais tarde. Colocar a instrução SQL em uma variável permite alterar a instrução, que é o que você vai fazer para executar a pesquisa.

Essas duas linhas, você poderá recolocar em posterior também tenha removido:

[!code-csharp[Main](form-basics/samples/sample7.cs)]

Você não deseja executar a consulta ainda (isto é, chamar `db.Query`) e você não quiser inicializar o `WebGrid` auxiliar ainda um. Depois que você tenha entendido qual instrução SQL deve ser executado, você fará as coisas.

Após esse bloco reconfigurado, você pode adicionar a nova lógica para lidar com a pesquisa. O código completo aparência a seguir. Atualize o código em sua página para que corresponda a este exemplo:

[!code-cshtml[Main](form-basics/samples/sample8.cshtml)]

A página agora funciona da seguinte maneira. Toda vez que a página é executada, o código abre o banco de dados e o `selectCommand` variável é definida como a instrução SQL que obtém todos os registros de `Movies` tabela. O código também inicializa o `searchTerm` variável.

No entanto, se a solicitação atual inclui um valor para o `searchGenre` elemento, o código define `selectCommand` para uma consulta diferente — ou seja, para um que inclui o `Where` cláusula para pesquisar um gênero. Ele também define `searchTerm` para tudo o que foi passado para a caixa de pesquisa (que pode ser nothing).

Independentemente de qual SQL instrução está em `selectCommand`, em seguida, chama o código `db.Query` para executar a consulta, passando qualquer é `searchTerm`. Se não houver nada `searchTerm`, não importa, pois o caso em que não há nenhum parâmetro para passar o valor para `selectCommand` mesmo assim.

Por fim, o código inicializa o `WebGrid` auxiliar usando os resultados da consulta, assim como antes.

Você pode ver que, colocando a instrução SQL e o termo de pesquisa em variáveis, você adicionou flexibilidade para o código. Como você verá posteriormente neste tutorial, você pode usar essa estrutura básica e continuar adicionando lógica para diferentes tipos de pesquisas.

## <a name="testing-the-search-by-genre-feature"></a>Testando o recurso de pesquisa por gênero

No WebMatrix, execute o *Movies.cshtml* página. Consulte a página com a caixa de texto para gênero.

Insira um gênero que você inseriu para um de seus registros de teste e clique em **pesquisa**. Neste momento, você verá uma lista de apenas de filmes que correspondem a esse gênero:

![Página de filmes listando depois procurar por gênero 'Comedies'](form-basics/_static/image4.png)

Insira um gênero diferente e pesquisar novamente. Tente inserir o gênero usando todas as letras em maiusculas ou minúsculas, de forma que você pode ver que a pesquisa não diferencia maiusculas de minúsculas.

## <a name="remembering-what-the-user-entered"></a>"Lembrar" o que o usuário inseriu

Você deve ter notado que depois que você inseriu um gênero e clicou **pesquisa gênero**, você viu uma listagem desse gênero. No entanto, a caixa de texto de pesquisa estava vazia &mdash; em outras palavras, ele não Lembre-se de que você tivesse digitado.

É importante entender por que esse comportamento ocorre. Quando você enviar uma página, o navegador envia uma solicitação ao servidor web. Quando ASP.NET recebe a solicitação, ele cria uma nova instância da página, executará o código e, em seguida, processa novamente a página para o navegador. Na verdade, no entanto, a página não sabe que você estava trabalhando com uma versão anterior de si mesma. Todos os sabe é que ele recebeu uma solicitação que tiveram alguns dados de formulário nele.

Toda vez que você solicita uma página &mdash; se pela primeira vez ou enviando- &mdash; você está obtendo uma nova página. O servidor web não tem memória de sua última solicitação. Não ASP.NET e não faz o navegador. A única conexão entre essas instâncias separadas da página é todos os dados transmitidos entre eles. Se você enviar uma página, por exemplo, a nova instância de página pode obter os dados de formulário que foi enviados pela instância anterior. (É outra maneira de passar dados entre páginas usar cookies.)

Uma maneira formal para descrever essa situação é dizer que páginas da web são *sem monitoração de estado*. Servidores Web e as próprias páginas e os elementos na página não precisam manter todas as informações sobre o estado anterior de uma página. A web foi projetada dessa maneira, como manter o estado de solicitações individuais seria rapidamente esgotar os recursos dos servidores web, que geralmente lidar com milhares, talvez até mesmo centenas de milhares de solicitações por segundo.

É por isso que a caixa de texto estava vazia. Depois que a página é enviada, o ASP.NET criada uma nova instância da página e foi executado por meio do código e a marcação. Não havia nada em que o código que disse ASP.NET para colocar um valor na caixa de texto. Portanto o ASP.NET não fez nada e a caixa de texto foi renderizada sem um valor.

Há, na verdade, uma maneira fácil de contornar esse problema. O gênero que você inseriu na caixa de texto *é* disponíveis no código &mdash; está em `Request.QueryString["searchGenre"]`.

Atualizar a marcação da caixa de texto para que o `value` atributo obtém seu valor de `searchTerm`, como neste exemplo:

[!code-html[Main](form-basics/samples/sample9.html?highlight=1)]

Nessa página, você pode ter também definir o `value` de atributo para o `searchTerm` variável, pois essa variável também contém o gênero inserido. Mas usando o `Request` objeto para definir o `value` atributo conforme mostrado aqui é o modo padrão para realizar essa tarefa. (Supondo que você deseja fazer isso mesmo &mdash; em algumas situações, você talvez queira renderizar a página *sem* valores nos campos. Tudo depende do que está acontecendo com seu aplicativo.)

> [!NOTE]
> Você não pode "lembrar" o valor de uma caixa de texto que é usado para senhas. Pode ser uma falha de segurança para permitir que as pessoas preencher um campo de senha por meio de código.


Execute novamente a página, insira um gênero e clique em **pesquisa gênero**. Desta vez não apenas ver os resultados da pesquisa, mas a caixa de texto se lembra de qual inseridos na última vez:

![Página mostrando que a caixa de texto tem 'lembradas' a entrada anterior](form-basics/_static/image5.png)

## <a name="searching-for-any-word-in-the-title"></a>Procurar qualquer palavra no título

Agora você pode pesquisar qualquer gênero, mas você também poderá procurar um título. É difícil de acertar um título exatamente quando você pesquisa, portanto em vez disso, que você pode procurar uma palavra que é exibido em qualquer lugar dentro de um título. Para fazer isso no SQL, você deve usar o `LIKE` operador e a sintaxe com o seguinte:

`SELECT * FROM Movies WHERE Title LIKE '%adventure%'`

Esse comando obtém todos os filmes cujos títulos contenham "adventure". Quando você usa o `LIKE` operador, você incluir o caractere curinga `%` como parte do termo de pesquisa. A pesquisa `LIKE 'adventure%'` significa "começando com 'adventure'". (Tecnicamente, isso significa "A cadeia de caracteres 'adventure' seguido de qualquer coisa.") Da mesma forma, o termo de pesquisa `LIKE '%adventure'` significa "nada seguido pela cadeia de caracteres 'adventure'", que é outra forma de dizer "termina com 'adventure'".

O termo de pesquisa `LIKE '%adventure%'` significa, portanto, "com"adventure' em qualquer lugar no título." (Tecnicamente, "nada no título, seguido por 'adventure', seguido de qualquer coisa.")

Dentro de `<form>` elemento, adicione a seguinte marcação imediatamente sob o fechamento `</div>` marca para a pesquisa de gênero (antes do fechamento `</form>` elemento):

[!code-html[Main](form-basics/samples/sample10.html)]

O código para lidar com essa pesquisa é semelhante para o código para a pesquisa de gênero, exceto que você deve montar o `LIKE` pesquisa. Dentro do bloco de código na parte superior da página, adicione `if` bloquear logo após o `if` bloco para a pesquisa de Gênero:

[!code-csharp[Main](form-basics/samples/sample11.cs)]

Esse código usa a mesma lógica que vimos anteriormente, exceto que a pesquisa usa um `LIKE` operador e coloca o código "`%`" antes e depois o termo de pesquisa.

Observe como é fácil adicionar outra pesquisa para a página. Tudo o que você precisava fazer era:

- Criar um `if` bloco testado para ver se a caixa de pesquisa relevante tinha um valor.
- Definir o `selectCommand` variável para uma nova instrução SQL.
- Definir o `searchTerm` variável como o valor para passar para a consulta.

Aqui está o bloco de código completo, que contém a nova lógica de uma pesquisa de título:

[!code-cshtml[Main](form-basics/samples/sample12.cshtml)]

Aqui está um resumo do que esse código faz:

- As variáveis `searchTerm` e `selectCommand` são inicializadas na parte superior. Você vai definir essas variáveis para o termo de pesquisa apropriada (se houver) e o comando SQL apropriado com base em que o usuário faz na página. A pesquisa padrão é o caso mais simples de obter todos os filmes do banco de dados.
- Nos testes para `searchGenre` e `searchTitle`, o código define `searchTerm` para o valor que você deseja pesquisar. Os blocos de código também definir `selectCommand` para um comando SQL apropriado para essa pesquisa.
- O `db.Query` método é chamado apenas uma vez, usando qualquer comando do SQL está em `selectedCommand` e qualquer valor que está em `searchTerm`. Se não houver nenhum termo de pesquisa (sem gênero e nenhuma palavra título), o valor de `searchTerm` é uma cadeia de caracteres vazia. No entanto, isso não importa, porque, nesse caso a consulta não requer um parâmetro.

## <a name="testing-the-title-search-feature"></a>Testando o recurso de pesquisa de título

Agora você pode testar sua página de pesquisa concluída. Executar *Movies.cshtml*.

Insira um gênero e clique em **pesquisa gênero**. A grade exibe filmes desse gênero, como antes.

Digite uma palavra de título e clique em **pesquisar título**. A grade exibe filmes com palavra no título.

![Página de filmes listando após procurando 'O' no título](form-basics/_static/image6.png)

Deixe em branco a ambas as caixas de texto e clique em um dos botões. A grade exibe todos os filmes.

## <a name="combining-the-queries"></a>Combinando as consultas

Você pode observar que as pesquisas que você pode executar são exclusivas. Você não pode pesquisar o título e o gênero ao mesmo tempo, mesmo se ambas as caixas de pesquisa possuem valores. Por exemplo, você não pode procurar todos os filmes de ação cujo título contém "Adventure". (A página é codificado agora, se você inserir valores para gênero e título, a pesquisa de título obtém precedência). Para criar uma pesquisa que combina as condições, você deve criar uma consulta SQL que tem sintaxe semelhante ao seguinte:

`SELECT * FROM Movies WHERE Genre = @0 AND Title LIKE @1`

E você terá de executar a consulta usando uma instrução semelhante à seguinte (em termos gerais):

`var selectedData = db.Query(selectCommand, searchGenre, searchTitle);`

Criando a lógica para permitir que muitas permutações de critérios de pesquisa pode obter um pouco envolvida, como você pode ver. Portanto, vamos parar aqui.

## <a name="coming-up-next"></a>Próximo

O seguinte tutorial, você criará uma página que usa um formulário para permitir que usuários adicionem filmes no banco de dados.

## <a name="complete-listing-for-movie-page-updated-with-search"></a>Listagem completa para a página de filme (atualizada com a pesquisa)

[!code-cshtml[Main](form-basics/samples/sample13.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Cláusula WHERE SQL](http://www.w3schools.com/sql/sql_where.asp) no site W3Schools
- [Definições de método](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) artigo no site do W3C

> [!div class="step-by-step"]
> [Anterior](displaying-data.md)
> [Próximo](entering-data.md)
