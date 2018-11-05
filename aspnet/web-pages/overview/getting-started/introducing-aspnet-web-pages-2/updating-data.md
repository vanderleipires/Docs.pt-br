---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Introdução ao ASP.NET Web Pages – atualizando o banco de dados | Microsoft Docs
author: Rick-Anderson
description: Este tutorial mostra como atualizar entrada (alterar) um banco de dados quando você usa o ASP.NET Web Pages (Razor). Ele pressupõe que você tenha concluído a série th...
ms.author: riande
ms.date: 01/02/2018
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: 206d7e209857aceb3eb92c2405bb73f7ff7dbaeb
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021424"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Introdução ao ASP.NET Web Pages – atualizando o banco de dados
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como atualizar entrada (alterar) um banco de dados quando você usa o ASP.NET Web Pages (Razor). Ele pressupõe que você tenha concluído a série por meio [inserindo dados por usando formulários usando páginas da Web ASP.NET](entering-data.md).
> 
> O que você aprenderá:
> 
> - Como selecionar um registro individual no `WebGrid` auxiliar.
> - Como ler um único registro de um banco de dados.
> - Como pré-carregar um formulário com os valores do registro de banco de dados.
> - Como atualizar um registro existente em um banco de dados.
> - Como armazenar informações na página sem exibi-la.
> - Como usar um campo oculto para armazenar informações.
>   
> 
> Recursos/tecnologias abordadas:
> 
> - O `WebGrid` auxiliar.
> - O SQL `Update` comando.
> - O método `Database.Execute`.
> - Campos ocultos (`<input type="hidden">`).


## <a name="what-youll-build"></a>O que você vai criar

No tutorial anterior, você aprendeu como adicionar um registro a um banco de dados. Aqui, você aprenderá a exibir um registro para edição. No *filmes* página, você atualizará o `WebGrid` auxiliar de modo que ele exibe um **editar** link ao lado de cada filme:

![O WebGrid exibir incluindo um link de 'Editar' para cada filme](updating-data/_static/image1.png)

Quando você clica o **editar** link, ele leva você até uma página diferente, onde as informações do filme já estão em um formulário:

![Editar página de filme mostrando o filme a ser editado](updating-data/_static/image2.png)

Você pode alterar qualquer um dos valores. Quando você envia as alterações, o código na página atualiza o banco de dados e leva você de volta para a listagem de filmes.

Esta parte do processo funciona quase exatamente como o *AddMovie.cshtml* página que você criou no tutorial anterior, portanto, grande parte deste tutorial será familiar.

Há várias maneiras que você pode implementar uma maneira de editar um filme individual. A abordagem mostrada foi escolhida porque é fácil de implementar e fácil de entender.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Adicionando um Link de edição para a listagem de filmes

Para começar, você irá atualizar o *filmes* página de modo que cada filme listando também contém uma **editar** link.

Abra o *Movies.cshtml* arquivo.

No corpo da página, altere o `WebGrid` marcação adicionando uma coluna. Aqui está a marcação modificada:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

A nova coluna é este:

[!code-html[Main](updating-data/samples/sample2.html)]

O objetivo desta coluna é mostrar um link (`<a>` elemento) cujo texto é "Editar". O que estamos procurando é criar um link que é semelhante ao seguinte quando a página é executada, com o `id` valor diferente para cada filme:

[!code-css[Main](updating-data/samples/sample3.css)]

Esse link invocará uma página chamada *EditMovie*, e ele passará a cadeia de caracteres de consulta `?id=7` para a página.

A sintaxe para a nova coluna pode parecer um pouco complexa, mas só porque ele reúne vários elementos. Cada elemento individual é simples. Se você se concentrar em apenas o `<a>` elemento, você verá essa marcação:

[!code-html[Main](updating-data/samples/sample4.html)]

Algumas informações básicas sobre o funcionamento da grade: a grade exibe linhas, uma para cada registro de banco de dados, e ele exibe colunas para cada campo no registro de banco de dados. Embora cada linha da grade está sendo construída, o `item` objeto contém o registro de banco de dados (item) para aquela linha. Além disso, ela oferece uma maneira no código para obter os dados para aquela linha. Isso é o que você vê aqui: a expressão `item.ID` está obtendo o valor da ID do item atual do banco de dados. Você pode obter qualquer um dos valores de banco de dados (título, gênero ou ano) da mesma forma, usando `item.Title`, `item.Genre`, ou `item.Year`.

A expressão `"~/EditMovie?id=@item.ID` combina a parte da URL de destino embutida (`~/EditMovie?id=`) com essa ID de dinamicamente derivada. (Você viu o `~` operador no tutorial anterior; é um operador do ASP.NET que representa a raiz do site atual.)

O resultado é que essa parte da marcação na coluna simplesmente produz algo parecido com a seguinte marcação em tempo de execução:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Naturalmente, o valor real do `id` será diferente para cada linha.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Criando uma exibição personalizada para uma coluna de grade

Agora, de volta para a coluna de grade. As três colunas você tinha originalmente nos grade exibido somente valores de dados (título, gênero e ano). Você especificou essa exibição, passando o nome da coluna de banco de dados &mdash; por exemplo, `grid.Column("Title")`.

Essa nova **editar** coluna de link é diferente. Em vez de especificar um nome de coluna, você está passando um `format` parâmetro. Esse parâmetro permite definir a marcação que o `WebGrid` auxiliar renderizará juntamente com o `item` valor para exibir os dados da coluna como negrito ou verde ou em qualquer formato que você deseja. Por exemplo, se você quisesse o título a ser exibido em negrito, você poderia criar uma coluna, como neste exemplo:

[!code-html[Main](updating-data/samples/sample6.html)]

(Os vários `@` caracteres do `format` propriedade marcar a transição entre a marcação e um valor de código.)

Depois que você sabe sobre o `format` propriedade, é mais fácil de entender como o novo **editar** coluna de link é reunida:

[!code-html[Main](updating-data/samples/sample7.html)]

A coluna consiste *apenas* da marcação que renderiza o link, além de algumas informações (a ID) que é extraído do registro de banco de dados para a linha.

> [!TIP]
> 
> **Parâmetros nomeados e posicionais parâmetros para um método**
> 
> Muitas vezes quando você chamou um método e passados parâmetros a ele, você simplesmente listou os valores de parâmetros separados por vírgulas. Veja alguns exemplos:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Não mencionamos o problema quando você primeiro viu esse código, mas em cada caso, você está passando parâmetros para os métodos em uma ordem específica &mdash; , ou seja, a ordem na qual os parâmetros são definidos nesse método. Para `db.Execute` e `Validation.RequireFields`, se misturar a ordem dos valores que você passe, você obterá uma mensagem de erro quando a página é executada, ou pelo menos alguns resultados estranhos. Sem dúvida, você precisa saber a ordem para passar os parâmetros no. (No WebMatrix, o IntelliSense pode ajudar você aprender a descobrir o nome, tipo e ordem dos parâmetros.)
> 
> Como uma alternativa para passar valores em ordem, você pode usar *parâmetros nomeados*. (Passando parâmetros na ordem é conhecida como uso *parâmetros posicionais*.) Para parâmetros nomeados, você incluir explicitamente o nome do parâmetro ao passar seu valor. Você usou parâmetros nomeados já um número de vezes nestes tutoriais. Por exemplo:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> e
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Parâmetros nomeados são úteis para algumas situações, especialmente quando um método usa muitos parâmetros. Um é quando você deseja passar apenas um ou dois parâmetros, mas os valores que você deseja passar não estão entre as posições primeiro na lista de parâmetros. Outra situação é quando você deseja tornar seu código mais legível, passando os parâmetros na ordem em que fizer mais sentido para você.
> 
> Obviamente, para usar parâmetros nomeados, você precisa saber os nomes dos parâmetros. WebMatrix IntelliSense pode *Mostrar* você os nomes, mas ele não é possível no momento, preencha-os para você.


## <a name="creating-the-edit-page"></a>Criando a página Editar

Agora você pode criar o *EditMovie* página. Quando os usuários clicarem o **editar** link, eles acabará nesta página.

Crie uma página chamada *EditMovie.cshtml* e substituir o que está no arquivo com a seguinte marcação:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Essa marcação e o código é semelhante ao que você tem na *AddMovie* página. Há uma pequena diferença no texto do botão Enviar. Assim como acontece com o *AddMovie* página, há um `Html.ValidationSummary` chamada que será exibir erros de validação, caso haja algum. Neste momento, está deixando de fora de chamadas para `Validation.Message`, uma vez que os erros serão exibidos no resumo de validação. Conforme observado no tutorial anterior, você pode usar o resumo de validação e as mensagens de erro individuais em várias combinações.

Observe novamente que o `method` atributo o `<form>` é definido como `post`. Assim como acontece com o *AddMovie.cshtml* página, essa página faz alterações no banco de dados. Portanto, esse formulário deve executar um `POST` operação. (Para obter mais informações sobre a diferença entre `GET` e `POST` operações, consulte o [GET, POST e segurança do verbo HTTP](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) barra lateral no tutorial em formulários HTML.)

Como você viu em um tutorial anterior, o `value` atributos das caixas de texto estão sendo definidos com o código do Razor para pré-carregá-los. Neste momento, no entanto, você está usando variáveis como `title` e `genre` para essa tarefa, em vez de `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Como antes, essa marcação será pré-carregar os valores de caixa de texto com os valores do filme. Você verá em breve por que é útil usar variáveis desta vez, em vez de usar o `Request` objeto.

Também há um `<input type="hidden">` elemento nesta página. Esse elemento armazena a ID de filme sem torná-la visível na página. A ID será inicialmente passada para a página por usando um valor de cadeia de caracteres de consulta (`?id=7` ou semelhantes na URL). Colocando o valor de ID em um campo oculto, você pode verificar que ele esteja disponível quando o formulário for enviado, mesmo se você não tiver acesso à URL original que a página foi chamada com.

Ao contrário do *AddMovie* página, o código para o *EditMovie* página tem duas funções distintas. A primeira função é que, quando a página é exibida pela primeira vez (e *apenas* , em seguida,), o código obtém a ID de filme da cadeia de consulta. Então o código usa a ID para ler o filme correspondente do banco de dados e exibir (pré-carregá-lo) nas caixas de texto.

A segunda função é que, quando o usuário clica o **enviar alterações** botão, o código tem de ler os valores das caixas de texto e validá-los. O código também deve atualizar o item do banco de dados com os novos valores. Essa técnica é semelhante a adicionar um registro, como você viu no *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Adicionando código para ler um único filme

Para executar a primeira função, adicione este código à parte superior da página:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

A maior parte desse código é dentro de um bloco que começa `if(!IsPost)`. O `!` operador significa "não", portanto, a expressão significa *se essa solicitação não for um envio de post*, que é uma maneira indireta de dizer *se essa solicitação for a primeira vez que esta página foi executada*. Conforme observado anteriormente, esse código deve ser executado *apenas* na primeira vez em que a página é executada. Se você não colocar o código em `if(!IsPost)`, ele seria executado sempre que a página é chamado, se a primeira vez ou em resposta a um botão, clique em.

Observe que o código inclui um `else` bloquear neste momento. Como dissemos quando apresentamos `if` blocos, às vezes você deseja executar um código alternativo, se a condição que você está testando não é verdadeira. Esse é o caso aqui. Se a condição for atendida (ou seja, se a ID passada para a página é okey), ler uma linha do banco de dados. No entanto, se a condição não passar, o `else` bloco é executado e o código define uma mensagem de erro.

## <a name="validating-a-value-passed-to-the-page"></a>Validar um valor passado para a página

O código usa `Request.QueryString["id"]` para obter a ID que é passada para a página. O código torna-se de que um valor, na verdade, foi passado para a ID. Se nenhum valor foi passado, o código define um erro de validação.

Este código mostra uma maneira diferente para validar as informações. No tutorial anterior, você trabalhou com o `Validation` auxiliar. Você registrou campos para validar e ASP.NET fez a validação automaticamente e exibidos erros usando `Html.ValidationMessage` e `Html.ValidationSummary`. Nesse caso, no entanto, você está na verdade, não Validando entrada do usuário. Em vez disso, você está validando um valor que foi passado para a página de qualquer outro lugar. O `Validation` auxiliar não faz isso para você.

Portanto, você verificar o valor por conta própria, testando-o com `if(!Request.QueryString["ID"].IsEmpty()`). Se houver um problema, você pode exibir o erro, usando `Html.ValidationSummary`, como você fez com o `Validation` auxiliar. Para fazer isso, você deve chamar `Validation.AddFormError` e passá-lo uma mensagem a ser exibida. `Validation.AddFormError` é um método interno que permite definir mensagens personalizadas que se integram o sistema de validação que você já conhece. (Posteriormente no tutorial, falaremos sobre como tornar esse processo de validação um pouco mais robusto.)

Após certificar-se de que há uma ID de filme, o código lê o banco de dados, procurando apenas um item único banco de dados. (Você provavelmente notou o padrão geral para operações de banco de dados: abrir o banco de dados, definir uma instrução SQL e execute a instrução.) Neste momento, o SQL `Select` instrução inclui `WHERE ID = @0`. Como a ID é exclusiva, apenas um registro pode ser retornado.

A consulta é executada por meio `db.QuerySingle` (não `db.Query`, conforme usado para a listagem de filme), e o código coloca o resultado no `row` variável. O nome `row` é arbitrário; você pode nomear as variáveis que desejar. As variáveis inicializadas na parte superior, em seguida, são preenchidas com os detalhes do filme, para que esses valores podem ser exibidos nas caixas de texto.

## <a name="testing-the-edit-page-so-far"></a>Testando a página de edição (até o momento)

Se você quiser testar a sua página, execute as *filmes* página agora e clique em um **editar** link ao lado de qualquer filme. Você verá a *EditMovie* preenchido página com os detalhes para o filme que você selecionou:

![Editar página de filme mostrando o filme a ser editado](updating-data/_static/image3.png)

Observe que a URL da página inclui algo como `?id=10` (ou algum outro número). Até agora você testou que **edite** vincula na *filme* página trabalho, que sua página está lendo a ID da cadeia de consulta, e que o banco de dados de consulta para obter um registro único de filme está funcionando.

Você pode alterar as informações do filme, mas nada acontece quando você clica **enviar alterações**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Adicionando código para atualizar o filme com as alterações do usuário

No *EditMovie.cshtml* de arquivos, para implementar a segunda função (Salvar alterações), adicione o seguinte código dentro da chave de fechamento do `@` bloco. (Se você não tiver certeza sobre onde colocar o código, você pode examinar os [completo listagem de código para a página Editar filme](#Complete_Page_Listing_for_EditMovie) que aparece no final deste tutorial.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Novamente, essa marcação e o código é semelhante ao código em *AddMovie*. O código está em um `if(IsPost)` bloquear, pois esse código é executado somente quando o usuário clica o **enviar alterações** botão &mdash; ou seja, quando (e somente quando) o formulário foi postado. Nesse caso, você não estiver usando um teste como `if(IsPost && Validation.IsValid())`— ou seja, você não está combinando os dois testes usando and. Nessa página, você primeiro determinar se há um envio de formulário (`if(IsPost)`) e só então registrar os campos para validação. Em seguida, você pode testar os resultados da validação (`if(Validation.IsValid()`). O fluxo é um pouco diferente na *AddMovie.cshtml* página, mas o efeito é o mesmo.

Obter os valores das caixas de texto usando `Request.Form["title"]` e um código semelhante para os outros `<input>` elementos. Observe que neste momento, o código obtém a ID de filme fora do campo oculto (`<input type="hidden">`). Quando a página foi executado na primeira vez, o código tem a ID de cadeia de caracteres de consulta. Você pode obter o valor do campo oculto para certificar-se de que você obtenha a ID do filme que originalmente foi exibido, no caso de alguma forma, a cadeia de caracteres de consulta foi alterada desde então.

A diferença muito importante entre o *AddMovie* código e esse código é que, nesse código você usa o SQL `Update` instrução em vez do `Insert Into` instrução. O exemplo a seguir mostra a sintaxe do SQL `Update` instrução:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Você pode especificar qualquer coluna em qualquer ordem, e você não precisa necessariamente atualizar todas as colunas durante um `Update` operação. (Você não pode atualizar a ID em si, porque em vigor que seria salvar o registro como um novo registro, e que não é permitido para um `Update` operação.)

> [!NOTE] 
> 
> **Importante** o `Where` cláusula com a ID é muito importante, pois é como o banco de dados sabe qual banco de dados registros que você deseja atualizar. Se você parou o `Where` cláusula, o banco de dados atualizaria *cada* registros no banco de dados. Na maioria dos casos, isso seria um desastre.


No código, para atualizar os valores são passados para a instrução SQL usando os espaços reservados. O que dissemos antes de repetir: por motivos de segurança *apenas* usar espaços reservados para passar valores para uma instrução SQL.

Depois que o código usa `db.Execute` para executar o `Update` instrução, ele redireciona de volta para a página de listagem, onde você pode ver as alterações.

> [!TIP] 
> 
> **Instruções de SQL diferentes, métodos diferentes**
> 
> Você deve ter notado que você use métodos ligeiramente diferentes para executar diferentes instruções de SQL. Para executar uma `Select` que potencialmente consulta retorna vários registros, você usar o `Query` método. Para executar uma `Select` consulta que você sabe que retornará apenas um item de banco de dados, você usar o `QuerySingle` método. Para executar comandos que fazer alterações, mas que não retornam os itens de banco de dados, você deve usar o `Execute` método.
> 
> Você precisa ter métodos diferentes porque cada um deles retorna resultados diferentes, como você já viu a diferença entre `Query` e `QuerySingle`. (O `Execute` método realmente retorna um valor também &mdash; , ou seja, o número de linhas do banco de dados que foram afetadas pelo comando &mdash; , mas você tiver sido ignorando que até o momento.)
> 
> É claro, o `Query` método pode retornar apenas uma linha do banco de dados. No entanto, o ASP.NET sempre trata os resultados do `Query` método como uma coleção. Mesmo que o método retorna apenas uma linha, você precisa extrair essa única linha da coleção. Portanto, em situações em que você *sabe* você receberá apenas uma linha, é um pouco mais conveniente usar `QuerySingle`.
> 
> Há alguns outros métodos que realizam a tipos específicos de operações de banco de dados. Você pode encontrar uma lista dos métodos de banco de dados do [referência rápida da API de páginas da Web de ASP.NET](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Tornando a validação para a ID mais robusto

Na primeira vez que a página é executada, você obter a ID de filme da cadeia de consulta para que você possa ir obter esse filme do banco de dados. Certificar-se que, na verdade, houve um valor para procurar, que você fez usando este código:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Você utilizou este código para certificar-se de que se um usuário para o *EditMovies* página sem primeiro selecionar um filme na *filmes* página, a página exibirá uma mensagem de erro amigável. (Caso contrário, os usuários veria um erro que provavelmente seria confundi-los.)

No entanto, essa validação não é muito consistente. A página também pode ser invocada com esses erros:

- A ID não é um número. Por exemplo, a página pode ser invocada com uma URL como `http://localhost:nnnnn/EditMovie?id=abc`.
- A ID é um número, mas faz referência a um filme que não existe (por exemplo, `http://localhost:nnnnn/EditMovie?id=100934`).

Se você estiver curioso para ver os erros resultantes dessas URLs, executadas as *filmes* página. Selecione para editar um filme e, em seguida, altere a URL do *EditMovie* página para uma URL que contém um alfabético ID ou a ID de um filme inexistente.

Portanto, o que você deve fazer? A primeira correção é certificar-se de que não só é uma ID passada para a página, mas a ID é um inteiro. Alterar o código para o `!IsPost` teste para se parecer com este exemplo:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Você adicionou uma segunda condição para o `IsEmpty` teste, vinculado com `&&` (AND lógico):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Você deve se lembrar do [Introdução à programação de páginas da Web do ASP.NET](../introducing-razor-syntax-c.md) tutorial que métodos como `AsBool` um `AsInt` converter uma cadeia de caracteres em algum outro tipo de dados. O `IsInt` método (e outros, como `IsBool` e `IsDateTime`) são semelhantes. No entanto, eles testam apenas se você *pode* converter a cadeia de caracteres, sem realmente executar a conversão. Aqui você está essencialmente dizendo *se o valor de cadeia de caracteres de consulta pode ser convertido em um inteiro...* .

O outro problema potencial está procurando um filme que não existe. O código para obter um filme se parece com este código:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Se você passar uma `movieId` de valor para o `QuerySingle` método que não corresponde a um filme real, nada será retornado e as instruções a seguir (por exemplo, `title=row.Title`) resultam em erros.

Novamente, há uma correção fácil. Se o `db.QuerySingle` método não retorna nenhum resultado, o `row` variável será nula. Portanto, você pode verificar se o `row` variável é nula antes de tentar obter valores dele. O código a seguir adiciona uma `if` bloco de instruções que obtenha os valores da `row` objeto:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Com esses dois testes de validação adicionais, a página se tornará mais prova. O código completo para o `!IsPost` branch agora se parece com este exemplo:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Podemos irá observar mais uma vez que essa tarefa é um bom uso para um `else` bloco. Se os testes não passarem, o `else` conjunto de blocos de mensagens de erro.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Adicionando um Link para retornar à página de filmes

Um detalhe final e útil é adicionar um link de volta para o *filmes* página. O fluxo comum de eventos, os usuários iniciará com a *filmes* página e clique em um **editar** link. Que leva para o *EditMovie* página, onde podem editar o filme e clique no botão. Depois que o código processa a alteração, ele redireciona de volta para o *filmes* página.

No entanto:

- O usuário pode decidir por não alterar nada.
- O usuário pode ter chegado a esta página sem primeiro clicar em um **edite** link na *filmes* página.

De qualquer forma, que você deseja tornar mais fácil para que eles possam retornar para a listagem principal. É fácil de corrigir &mdash; adicione a marcação a seguir logo após o fechamento `</form>` marca na marcação:

[!code-html[Main](updating-data/samples/sample19.html)]

Essa marcação usa a mesma sintaxe para um `<a>` elemento que você já viu em outro lugar. A URL inclui `~` para significar "" raiz"do site.

## <a name="testing-the-movie-update-process"></a>Testar o processo de atualização de filme

Agora você pode testar. Execute o *filmes* da página e clique em **editar** ao lado de um filme. Quando o *EditMovie* página for exibida, faça alterações no filme e clicar em **enviar alterações**. Quando a listagem do filme aparece, certifique-se de que suas alterações são mostradas.

Para certificar-se de que a validação está funcionando, clique em **editar** para outro filme. Quando chegar a *EditMovie* página, desmarque as **gênero** campo (ou **ano** campo, ou ambos) e tentar enviar suas alterações. Você verá um erro, como você poderia esperar:

![Editar página de filme mostrando erros de validação](updating-data/_static/image4.png)

Clique o **retornar para a listagem de filmes** link para abandonar suas alterações e retornar para o *filmes* página.

## <a name="coming-up-next"></a>Próximo

O próximo tutorial, você verá como excluir um registro de filme.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Listagem completa para a página de filme (atualizada com Links de edição)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>A listagem de página para página de edição do filme completo

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação Web do ASP.NET usando a sintaxe do Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Instrução de atualização SQL](http://www.w3schools.com/sql/sql_update.asp) no site W3Schools

> [!div class="step-by-step"]
> [Anterior](entering-data.md)
> [Próximo](deleting-data.md)
