---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
title: Introdução a páginas da Web ASP.NET - atualização do banco de dados | Microsoft Docs
author: tfitzmac
description: Este tutorial mostra como atualizar entrada (alterar) um banco de dados quando você usa páginas da Web do ASP.NET (Razor). Ele pressupõe que você tenha concluído a série th...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/02/2018
ms.topic: article
ms.assetid: ac86ec9c-6b69-485b-b9e0-8b9127b13e6b
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/updating-data
msc.type: authoredcontent
ms.openlocfilehash: e889cd27e2267a08f7b6ea708c92e35edbdd7a1a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30898529"
---
<a name="introducing-aspnet-web-pages---updating-database-data"></a>Introdução a páginas da Web ASP.NET - atualização do banco de dados
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como atualizar entrada (alterar) um banco de dados quando você usa páginas da Web do ASP.NET (Razor). Ele pressupõe que você tenha concluído a série por meio de [inserindo dados pelo uso de formulários usando páginas da Web ASP.NET](entering-data.md).
> 
> O que você aprenderá:
> 
> - Como selecionar um registro individual no `WebGrid` auxiliar.
> - Como ler um único registro de um banco de dados.
> - Como pré-carregar um formulário com valores do registro do banco de dados.
> - Como atualizar um registro existente em um banco de dados.
> - Como armazenar informações na página sem exibi-lo.
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

No tutorial anterior, você aprendeu como adicionar um registro de um banco de dados. Aqui, você aprenderá como exibir um registro para edição. No *filmes* página, você poderá atualizar o `WebGrid` auxiliar para que ele exibe um **editar** link ao lado de cada filme:

![WebGrid exibir incluindo um link de 'Editar' para cada filme](updating-data/_static/image1.png)

Quando você clica o **editar** link, ele leva a uma página diferente, onde as informações de filme já estão em um formulário:

![Editar página de filme mostrando filme a ser editado](updating-data/_static/image2.png)

Você pode alterar qualquer um dos valores. Quando você envia as alterações, o código na página atualiza o banco de dados e levará de volta para a listagem do filme.

Esta parte do processo funciona quase exatamente como o *AddMovie.cshtml* página que você criou no tutorial anterior, para grande parte deste tutorial será familiar.

Há várias maneiras que você pode implementar uma maneira para editar um filme individual. A abordagem mostrada foi escolhida porque é fácil de implementar e fácil de entender.

## <a name="adding-an-edit-link-to-the-movie-listing"></a>Adicionando um Link de edição para a listagem de filme

Para começar, você atualizará o *filmes* página de forma que cada filme listando também contém um **editar** link.

Abra o *Movies.cshtml* arquivo.

No corpo da página, altere o `WebGrid` marcação adicionando uma coluna. Aqui está a marcação modificada:

[!code-html[Main](updating-data/samples/sample1.html?highlight=6)]

A nova coluna é este:

[!code-html[Main](updating-data/samples/sample2.html)]

O ponto dessa coluna é para mostrar um link (`<a>` elemento) cujo texto diz "Editar". O que estamos procurando é criar um link que é semelhante ao seguinte quando a página é executada, com o `id` valor diferente para cada filme:

[!code-css[Main](updating-data/samples/sample3.css)]

Este link invocará uma página chamada *EditMovie*, e passe a cadeia de caracteres de consulta `?id=7` para a página.

A sintaxe para a nova coluna pode parecer um pouco complexa, mas só porque ela reúne vários elementos. Cada elemento individual é simples. Se você se concentrar em apenas o `<a>` elemento, você verá essa marcação:

[!code-html[Main](updating-data/samples/sample4.html)]

Algumas informações básicas sobre como funciona a grade: a grade exibe linhas, uma para cada registro de banco de dados, e exibe colunas para cada campo no registro do banco de dados. Enquanto está sendo construída cada linha de grade, o `item` objeto contém o registro do banco de dados (item) para aquela linha. Além disso, ela oferece uma maneira em código para obter os dados para aquela linha. É o que você vê aqui: a expressão `item.ID` está obtendo o valor da ID do item atual do banco de dados. Você pode obter os valores do banco de dados (título, gênero ou ano) da mesma forma, usando `item.Title`, `item.Genre`, ou `item.Year`.

A expressão `"~/EditMovie?id=@item.ID` combina a parte da URL de destino embutido (`~/EditMovie?id=`) com esta ID de dinamicamente derivada. (Você viu o `~` operador no tutorial anterior; é um operador do ASP.NET que representa a raiz do site atual.)

O resultado é que essa parte da marcação na coluna simplesmente produz algo parecido com a seguinte marcação em tempo de execução:

[!code-xml[Main](updating-data/samples/sample5.xml)]

Naturalmente, o valor real do `id` será diferente para cada linha.

## <a name="creating-a-custom-display-for-a-grid-column"></a>Criando uma exibição personalizada para uma coluna de grade

Agora, de volta para a coluna de grade. As três colunas você tinha originalmente nos grade exibido somente valores de dados (título, gênero e ano). Você especificou essa exibição, passando o nome da coluna de banco de dados &mdash; por exemplo, `grid.Column("Title")`.

Essa nova **editar** link coluna é diferente. Em vez de especificar um nome de coluna, você está passando um `format` parâmetro. Esse parâmetro permite que você defina marcação que o `WebGrid` auxiliar processará junto com o `item` valor para exibir os dados da coluna como negrito ou verde ou em qualquer formato que você deseja. Por exemplo, se você quiser que o título a ser exibido em negrito, pode criar uma coluna como neste exemplo:

[!code-html[Main](updating-data/samples/sample6.html)]

(Os vários `@` caracteres do `format` propriedade marcar a transição entre a marcação e um valor de código.)

Depois que você conheça o `format` propriedade, é mais fácil entender como o novo **editar** coluna de link é reunida:

[!code-html[Main](updating-data/samples/sample7.html)]

A coluna consiste em *somente* da marcação que renderiza o link, além de algumas informações (a ID) que é extraído do registro do banco de dados para a linha.

> [!TIP]
> 
> **Parâmetros nomeados e posicionais parâmetros para um método**
> 
> Muitas vezes quando você chamou um método e passados parâmetros para ele, simplesmente listados os valores de parâmetros separados por vírgulas. Veja alguns exemplos:
> 
> `db.Execute(insertCommand, title, genre, year)`
> 
> `Validation.RequireField("title", "You must enter a title")`
> 
> Não mencionamos o problema quando você viu primeiro esse código, mas em cada caso, você está passando parâmetros para os métodos em uma ordem específica &mdash; , ou seja, a ordem na qual os parâmetros são definidos no método. Para `db.Execute` e `Validation.RequireFields`, se você misto a ordem dos valores que você passa, você receberia uma mensagem de erro quando a página é executada, ou pelo menos alguns resultados estranhos. Obviamente, você precisa saber a ordem para passar os parâmetros. (No WebMatrix, IntelliSense pode ajudá-lo a aprender descobrir o nome, tipo e ordem dos parâmetros.)
> 
> Como alternativa para passar valores em ordem, você pode usar *parâmetros nomeados*. (Passando parâmetros na ordem é conhecido como usando *parâmetros posicionais*.) Parâmetros nomeados, você explicitamente incluir o nome do parâmetro ao passar seu valor. Você usou parâmetros nomeados já um número de vezes nesses tutoriais. Por exemplo:
> 
> [!code-csharp[Main](updating-data/samples/sample8.cs)]
> 
> e
> 
> [!code-css[Main](updating-data/samples/sample9.css)]
> 
> Parâmetros nomeados são úteis para algumas situações, especialmente quando um método usa muitos parâmetros. Um é quando você deseja passar apenas um ou dois parâmetros, mas não são os valores que você deseja passar entre as posições primeiro na lista de parâmetros. Outra situação é quando você deseja tornar seu código mais legível, passando os parâmetros na ordem em que faz mais sentido para você.
> 
> Obviamente, para usar parâmetros nomeados, você precisa saber os nomes dos parâmetros. WebMatrix IntelliSense pode *Mostrar* você os nomes, mas ele não pode atualmente preenchê-los para você.


## <a name="creating-the-edit-page"></a>Criar página de edição

Agora você pode criar o *EditMovie* página. Quando os usuários clicarem o **editar** link, ele acabará nesta página.

Crie uma página chamada *EditMovie.cshtml* e substitua o que está no arquivo com a seguinte marcação:

[!code-cshtml[Main](updating-data/samples/sample10.cshtml)]

Essa marcação e o código é semelhante ao que você tem *AddMovie* página. Há uma pequena diferença no texto do botão de envio. Assim como acontece com o *AddMovie* página, não há um `Html.ValidationSummary` chamada que irá exibir erros de validação, se houver. Neste momento, está saindo chamadas para `Validation.Message`, uma vez que os erros serão exibidos no resumo de validação. Conforme observado no tutorial anterior, você pode usar o resumo de validação e mensagens de erro individuais em várias combinações.

Observe novamente que o `method` atributo o `<form>` é definido como `post`. Assim como acontece com o *AddMovie.cshtml* página, esta página faz alterações no banco de dados. Portanto, este formulário deve executar um `POST` operação. (Para obter mais informações sobre a diferença entre `GET` e `POST` operações, consulte o [GET, POST e HTTP verbo segurança](form-basics.md#GET,_POST,_and_HTTP_Verb_Safety) barra lateral do tutorial em formulários HTML.)

Como você viu um tutorial anterior, o `value` atributos das caixas de texto estão sendo definidos com o código do Razor para pré-carregá-los. Neste momento, porém, você está usando variáveis como `title` e `genre` para essa tarefa, em vez de `Request.Form["title"]`:

`<input type="text" name="title" value="@title" />`

Como antes, esta marcação será pré-carregar os valores da caixa de texto com os valores do filme. Você verá em breve por que é útil para usar variáveis neste momento, em vez de usar o `Request` objeto.

Também há um `<input type="hidden">` elemento nesta página. Esse elemento armazena a ID de filme sem torná-lo visível na página. A ID é inicialmente passada para a página, usando um valor de cadeia de caracteres de consulta (`?id=7` ou semelhante na URL). Colocando o valor de ID em um campo oculto, você pode garantir que ele esteja disponível quando o formulário é enviado, mesmo se você não tiver acesso à URL original que a página foi chamada com.

Ao contrário de *AddMovie* página, o código para o *EditMovie* página tem duas funções diferentes. A primeira função é que, quando a página é exibida pela primeira vez (e *somente* , em seguida,), o código obtém a ID de filme da cadeia de consulta. O código, em seguida, usa o ID para ler o filme correspondente do banco de dados e exibir (pré-carregar)-lo nas caixas de texto.

A segunda função é que, quando o usuário clica o **enviar alterações** botão, o código precisa ler os valores das caixas de texto e validá-los. O código também deve atualizar o item de banco de dados com os novos valores. Essa técnica é semelhante à adição de um registro, como você viu na *AddMovie*.

## <a name="adding-code-to-read-a-single-movie"></a>Adicionando código para ler um filme único

Para executar a primeira função, adicione este código para a parte superior da página:

[!code-cshtml[Main](updating-data/samples/sample11.cshtml)]

A maioria do código está dentro de um bloco que inicia `if(!IsPost)`. O `!` operador significa "não", portanto, a expressão significa *se esta solicitação não for um envio post*, que é uma maneira indireta de dizer *se essa solicitação é a primeira vez que esta página foi executada*. Conforme observado anteriormente, esse código deve ser executado *somente* na primeira vez em que a página é executada. Se você não incluir o código em `if(!IsPost)`, ele seria executado sempre que a página é chamado, se a primeira vez ou em resposta a um botão, clique em.

Observe que o código inclui um `else` bloquear neste momento. Como foi dito quando apresentamos `if` blocos, às vezes você deseja executar código alternativo, se a condição que você está testando não for true. Esse é o caso aqui. Se a condição for aprovada (ou seja, se a ID passada para a página é okey), você ler uma linha do banco de dados. No entanto, se a condição não for aprovada, o `else` bloco é executado e o código define uma mensagem de erro.

## <a name="validating-a-value-passed-to-the-page"></a>Validar um valor passado para a página

O código usa `Request.QueryString["id"]` para obter a ID que é passada para a página. O código torna-se de que, na verdade, foi passado um valor para a ID. Se nenhum valor foi passado, o código define um erro de validação.

Esse código mostra uma maneira diferente de validar informações. No tutorial anterior, você trabalhou com o `Validation` auxiliar. Você registrou campos para validar e ASP.NET automaticamente fez a validação e exibidos erros usando `Html.ValidationMessage` e `Html.ValidationSummary`. Nesse caso, no entanto, você está realmente não Validando entrada do usuário. Em vez disso, você está validando um valor que foi passado para a página de qualquer outro lugar. O `Validation` auxiliar não faz isso para você.

Portanto, é verificar o valor por conta própria, testando-o com `if(!Request.QueryString["ID"].IsEmpty()`). Se houver um problema, você pode exibir o erro usando `Html.ValidationSummary`, como você fez com o `Validation` auxiliar. Para fazer isso, você deve chamar `Validation.AddFormError` e passá-lo em uma mensagem a ser exibida. `Validation.AddFormError` é um método interno que permite que você defina as mensagens personalizadas empatarem com o sistema de validação que você já estiver familiarizado com. (Posteriormente neste tutorial falaremos sobre como tornar o processo de validação um pouco mais robusto.)

Após certificar-se de que há uma ID de filme, o código lê o banco de dados, procurando por apenas um item único banco de dados. (Você provavelmente notou o padrão geral para operações de banco de dados: Abra o banco de dados, definir uma instrução SQL e execute a instrução.) Neste momento, o SQL `Select` instrução inclui `WHERE ID = @0`. Porque a ID é exclusiva, apenas um registro pode ser retornado.

A consulta é executada usando `db.QuerySingle` (não `db.Query`, conforme usado para a listagem de filme), e o código coloca o resultado para o `row` variável. O nome `row` é arbitrário; você pode nomear as variáveis que desejar. As variáveis inicializadas na parte superior, em seguida, são preenchidas com os detalhes do filme para que esses valores podem ser exibidos nas caixas de texto.

## <a name="testing-the-edit-page-so-far"></a>Testando a página Editar (até)

Se você gostaria de sua página de teste, execute o *filmes* página agora e clique em uma **editar** link ao lado de qualquer filme. Você verá o *EditMovie* página com os detalhes preenchidos para o filme selecionado:

![Editar página de filme mostrando filme a ser editado](updating-data/_static/image3.png)

Observe que a URL da página inclui algo como `?id=10` (ou algum outro número). Até você testou que **editar** links no *filme* página trabalho, se a página está lendo a ID da cadeia de consulta, e que o banco de dados de consulta para obter um registro único filme está funcionando.

Você pode alterar as informações de filme, mas nada acontece quando você clica em **enviar alterações**.

## <a name="adding-code-to-update-the-movie-with-the-users-changes"></a>Adicionando código para atualizar o filme com as alterações do usuário

No *EditMovie.cshtml* de arquivos, para implementar a segunda função (Salvar alterações), adicione o seguinte código dentro da chave de fechamento do `@` bloco. (Se você não tiver certeza sobre onde colocar o código, você pode examinar o [completo listagem de código para a página Editar filme](#Complete_Page_Listing_for_EditMovie) que aparece no fim deste tutorial.)

[!code-csharp[Main](updating-data/samples/sample12.cs)]

Novamente, esta marcação e o código é semelhante ao código *AddMovie*. O código está em um `if(IsPost)` bloquear, porque esse código é executado somente quando o usuário clica o **enviar alterações** botão &mdash; ou seja, quando (e apenas quando) o formulário foi lançado. Nesse caso, você não estiver usando um teste como `if(IsPost && Validation.IsValid())`— ou seja, você não está combinando os dois testes usando and. Nessa página, você primeiro determinar se há uma submissão de formulário (`if(IsPost)`) e só então registrar os campos para validação. Em seguida, você pode testar os resultados de validação (`if(Validation.IsValid()`). O fluxo é um pouco diferente no *AddMovie.cshtml* página, mas o efeito é o mesmo.

Obter os valores das caixas de texto usando `Request.Form["title"]` e um código semelhante para os outros `<input>` elementos. Observe que esse tempo, o código obtém a ID de filme fora do campo oculto (`<input type="hidden">`). Quando a página foi executado na primeira vez, o código tem a ID de cadeia de caracteres de consulta. Você pode obter o valor do campo oculto para certificar-se de que você está obtendo a ID do filme que foi originalmente exibida, no caso de alguma forma, a cadeia de caracteres de consulta foi alterada desde então.

A diferença muito importante entre o *AddMovie* código e esse código é nesse código usar o SQL `Update` instrução, em vez do `Insert Into` instrução. O exemplo a seguir mostra a sintaxe do SQL `Update` instrução:

`UPDATE table SET col1="value", col2="value", col3="value" ... WHERE ID = value`

Você pode especificar qualquer coluna em qualquer ordem, e não necessariamente é necessário atualizar todas as colunas durante um `Update` operação. (Você não pode atualizar a ID em si, porque em vigor que seria salvá-lo como um novo registro, e que não é permitido para um `Update` operação.)

> [!NOTE] 
> 
> **Importante** de `Where` cláusula com a ID é muito importante, porque é como o banco de dados sabe qual banco de dados de registro que você deseja atualizar. Se você parou o `Where` cláusula, atualizará o banco de dados *cada* registros no banco de dados. Na maioria dos casos, isso seria um desastre.


No código, para atualizar os valores são passados para a instrução SQL usando espaços reservados. O que dissemos antes de repetir: por razões de segurança, *somente* usar espaços reservados para passar valores para uma instrução SQL.

Depois que o código usa `db.Execute` para executar o `Update` instrução, ele redireciona de volta para a página de listagem, onde você pode ver as alterações.

> [!TIP] 
> 
> **Instruções SQL diferente, métodos diferentes**
> 
> Você deve ter notado que usa métodos ligeiramente diferentes para executar instruções SQL diferentes. Para executar um `Select` que potencialmente consulta retorna vários registros, use o `Query` método. Para executar um `Select` a consulta que você sabe que irá retornar apenas um item de banco de dados, use o `QuerySingle` método. Para executar comandos que fazer alterações, mas que não retornam os itens de banco de dados, você deve usar o `Execute` método.
> 
> Você precisa ter diferentes métodos porque cada um deles retorna resultados diferentes, como você já viu a diferença entre `Query` e `QuerySingle`. (O `Execute` método realmente retorna um valor também &mdash; , ou seja, o número de linhas do banco de dados que foram afetadas pelo comando &mdash; , mas você tiver sido ignorando que até o momento.)
> 
> Naturalmente, o `Query` método pode retornar apenas uma linha de banco de dados. No entanto, o ASP.NET sempre trata os resultados do `Query` método como uma coleção. Mesmo que o método retorna apenas uma linha, você precisa extrair aquela única linha da coleção. Portanto, em situações em que você *saber* você terá apenas uma linha, é um pouco mais conveniente usar `QuerySingle`.
> 
> Há alguns outros métodos que realizam a tipos específicos de operações de banco de dados. Você pode encontrar uma lista de métodos de banco de dados no [referência rápida de API de páginas da Web do ASP.NET](../../api-reference/asp-net-web-pages-api-reference.md#Data).


## <a name="making-validation-for-the-id-more-robust"></a>Fazer a validação de ID mais robusta

Na primeira vez que a página é executada, você obtém a ID de filme de cadeia de caracteres de consulta para que você possa ir obter esse filme do banco de dados. Certificar-se que, na verdade, não havia um valor para procurar, que você fez usando este código:

[!code-csharp[Main](updating-data/samples/sample13.cs)]

Você usou esse código para garantir que se um usuário obtém para o *EditMovies* página sem primeiro selecionar um filme no *filmes* página, a página será exibida uma mensagem de erro amigável. (Caso contrário, os usuários verá um erro que seria provavelmente apenas confundem-las.)

No entanto, essa validação não é muito eficiente. A página também pode ser chamada com estes erros:

- A ID não é um número. Por exemplo, a página pode ser chamada com uma URL como `http://localhost:nnnnn/EditMovie?id=abc`.
- A ID é um número, mas faz referência a um filme que não existe (por exemplo, `http://localhost:nnnnn/EditMovie?id=100934`).

Se você estiver curioso para ver os erros resultantes dessas URLs, executadas o *filmes* página. Selecione um filme para editar e, em seguida, altere a URL do *EditMovie* página para uma URL que contém um alfabético ID ou a ID de um filme inexistente.

Assim que você deve fazer? A primeira correção é certificar-se de que não só é uma ID passada para a página, mas a ID é um inteiro. Altere o código para o `!IsPost` teste para se parecer com este exemplo:

[!code-csharp[Main](updating-data/samples/sample14.cs)]

Você adicionou uma segunda condição para a `IsEmpty` teste, vinculado com `&&` (AND lógico):

[!code-csharp[Main](updating-data/samples/sample15.cs)]

Você deve se lembrar do [Introdução à programação de páginas da Web do ASP.NET](../introducing-razor-syntax-c.md) tutorial métodos como `AsBool` um `AsInt` converter uma cadeia de caracteres para outro tipo de dados. O `IsInt` método (e outros, como `IsBool` e `IsDateTime`) são semelhantes. No entanto, eles testam somente se você *pode* converter a cadeia de caracteres, sem realmente executar a conversão. Aqui você está dizendo essencialmente *se o valor de cadeia de caracteres de consulta pode ser convertido em um número inteiro...* .

O outro problema está procurando um filme que não existe. O código para obter um filme parece com este código:

[!code-csharp[Main](updating-data/samples/sample16.cs)]

Se você passar um `movieId` o valor para o `QuerySingle` método que não corresponde a um filme real, nada será retornado e as instruções a seguir (por exemplo, `title=row.Title`) resultar em erros.

Novamente, há uma solução fácil. Se o `db.QuerySingle` método não retorna nenhum resultado, o `row` variável será nula. Por isso, você pode verificar se o `row` variável é nula antes de tentar obter valores dele. O código a seguir adiciona uma `if` bloco de instruções que obtêm valores da `row` objeto:

[!code-csharp[Main](updating-data/samples/sample17.cs)]

Com esses dois testes de validação adicionais, a página se torna mais prova. O código completo para o `!IsPost` ramificação agora parece semelhante a este exemplo:

[!code-csharp[Main](updating-data/samples/sample18.cs)]

Podemos observará mais uma vez que essa tarefa é um bom uso para um `else` bloco. Se não passarem nos testes, a `else` conjunto de blocos de mensagens de erro.

## <a name="adding-a-link-to-return-to-the-movies-page"></a>Adicionar um Link para retornar à página de filmes

Um detalhe final e útil é adicionar um link para o *filmes* página. O fluxo normal de eventos, os usuários começarão ao *filmes* página e clique em uma **editar** link. Isso traz-los o *EditMovie* página, onde podem editar o filme e clique no botão. Depois que o código processa a alteração, ele redireciona de volta para o *filmes* página.

No entanto:

- O usuário pode decidir por não altera nada.
- O usuário pode ter chegado desta página sem primeiro clicar em um **editar** link no *filmes* página.

De qualquer forma, que você deseja que tornam mais fácil retornar à lista principal. É uma solução fácil &mdash; adicione a seguinte marcação logo após o fechamento `</form>` marca na marcação:

[!code-html[Main](updating-data/samples/sample19.html)]

Essa marcação usa a mesma sintaxe para um `<a>` elemento que você viu em outro lugar. A URL inclui `~` significando "raiz do site".

## <a name="testing-the-movie-update-process"></a>Testar o processo de atualização de filme

Agora você pode testar. Execute o *filmes* página e, em seguida, clique em **editar** ao lado de um filme. Quando o *EditMovie* página aparecer, fazer alterações de filme e clique em **enviar alterações**. Quando a listagem do filme aparece, certifique-se de que as alterações são mostradas.

Para certificar-se de que a validação está funcionando, clique em **editar** para outro filme. Quando chegar a *EditMovie* página, desmarque o **gênero** campo (ou **ano** campo, ou ambos) e tentar enviar suas alterações. Você verá um erro, conforme o esperado:

![Editar página de filme mostrando erros de validação](updating-data/_static/image4.png)

Clique o **retornar à lista de filme** link para abandonar suas alterações e retornar para o *filmes* página.

## <a name="coming-up-next"></a>Próximo

O seguinte tutorial, você verá como excluir um registro de filme.

## <a name="complete-listing-for-movie-page-updated-with-edit-links"></a>Listagem completa para a página de filme (atualizada com Links de edição)

[!code-cshtml[Main](updating-data/samples/sample20.cshtml)]

<a id="Complete_Page_Listing_for_EditMovie"></a>
## <a name="complete-page-listing-for-edit-movie-page"></a>Listagem de página para a página de edição de filme completo

[!code-cshtml[Main](updating-data/samples/sample21.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação da Web do ASP.NET usando a sintaxe do Razor](../../getting-started/introducing-razor-syntax-c.md)
- [Instrução de atualização SQL](http://www.w3schools.com/sql/sql_update.asp) no site W3Schools

> [!div class="step-by-step"]
> [Anterior](entering-data.md)
> [Próximo](deleting-data.md)
