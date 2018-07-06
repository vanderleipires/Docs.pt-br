---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: Introdução ao ASP.NET Web Pages - inserindo o banco de dados usando formulários | Microsoft Docs
author: tfitzmac
description: Este tutorial mostra como criar um formulário de entrada e, em seguida, insira os dados que você obtém do formulário em uma tabela de banco de dados quando você usa o ASP.NET Web Pages (...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: 313fd6ff70af540d4dd749734f50e3fc24be0d29
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835755"
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Introdução ao ASP.NET Web Pages - inserindo o banco de dados usando formulários
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como criar um formulário de entrada e, em seguida, insira os dados que você obtém do formulário em uma tabela de banco de dados quando você usa o ASP.NET Web Pages (Razor). Ele pressupõe que você tenha concluído a série por meio [Noções básicas de formulários em HTML em páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> O que você aprenderá:
> 
> - Mais informações sobre como processar formulários de entrada.
> - Como adicionar dados (insert) em um banco de dados.
> - Como certificar-se de que os usuários tiverem inserido um valor obrigatório em um formulário (como validar a entrada do usuário).
> - Como exibir erros de validação.
> - Como alternar para outra página da página atual.
>   
> 
> Recursos/tecnologias abordadas:
> 
> - O método `Database.Execute`.
> - O SQL `Insert Into` instrução
> - O `Validation` auxiliar.
> - O método `Response.Redirect`.


## <a name="what-youll-build"></a>O que você vai criar

No tutorial anterior que mostrou como criar um banco de dados, você inseriu dados do banco de dados por meio da edição do banco de dados diretamente no WebMatrix, trabalhando na **banco de dados** espaço de trabalho. Na maioria dos aplicativos, que não é uma maneira prática para colocar dados no banco de dados, no entanto. Portanto, neste tutorial, você criará uma interface baseada na web que permite que você ou qualquer pessoa inserir dados e salvá-lo ao banco de dados.

Você criará uma página onde você pode inserir novos filmes. A página conterá um formulário de entrada que tem campos (caixas de texto) em que você pode inserir um título do filme, gênero e ano. A página será semelhante a esta página:

!['Adicionar filme' página no navegador](entering-data/_static/image1.png)

As caixas de texto será HTML `<input>` elementos que irá procurar como essa marcação:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Criando o formulário de entrada básico

Crie uma página chamada *AddMovie.cshtml*.

Substitua o que está no arquivo com a marcação a seguir. Substituir tudo. Você adicionará um bloco de código na parte superior em breve.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Este exemplo mostra o HTML típica para a criação de um formulário. Ele usa `<input>` elementos para as caixas de texto e para o botão Enviar. As legendas para as caixas de texto são criadas usando o padrão `<label>` elementos. O `<fieldset>` e `<legend>` elementos coloque uma caixa de BOM no formulário.

Observe que nessa página, o `<form>` usa o elemento `post` como o valor para o `method` atributo. No tutorial anterior, você criou um formulário que é usado o `get` método. Isso foi correto, porque embora o formulário enviado valores para o servidor, a solicitação não fez as alterações. Tudo o que fazia era buscar dados de maneiras diferentes. No entanto, nesta página você *serão* fazer alterações — você vai adicionar novos registros de banco de dados. Portanto, esse formulário deve usar o `post` método. (Para obter mais informações sobre a diferença entre `GET` e `POST` operações, consulte o[GET, POST e segurança do verbo HTTP](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) barra lateral no tutorial anterior.)

Observe que cada caixa de texto tem um `name` elemento (`title`, `genre`, `year`). Como você viu no tutorial anterior, esses nomes são importantes porque você deve ter esses nomes para que você possa obter a entrada do usuário mais tarde. Você pode usar qualquer nome. Vale a pena usar nomes significativos que ajudam você a se lembrar de quais dados você está trabalhando com.

O `value` atributo de cada `<input>` elemento contém um pouco de código do Razor (por exemplo, `Request.Form["title"]`). Uma versão desse truque você aprendeu no tutorial anterior para preservar o valor inserido na caixa de texto (se houver) depois que o formulário foi enviado.

## <a name="getting-the-form-values"></a>Obtendo os valores de formulário

Em seguida, você pode adicionar código que processa o formulário. Na estrutura de tópicos, você fará o seguinte:

1. Verifique se a página está sendo lançada (foi enviado). Você deseja para seu código ser executado somente quando os usuários clicaram no botão, não quando a página é executada.
2. Obter os valores que o usuário inseriu nas caixas de texto. Nesse caso, porque o formulário está usando o `POST` verbo, você obtém os valores de formulário do `Request.Form` coleção.
3. Inserir os valores como um novo registro na *filmes* tabela de banco de dados.

Na parte superior do arquivo, adicione o seguinte código:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

As primeiras linhas criam variáveis (`title`, `genre`, e `year`) para conter os valores das caixas de texto. A linha `if(IsPost)` torna-se de que as variáveis são definidas *apenas* quando os usuários clicarem o **Adicionar filme** botão — ou seja, quando o formulário foi postado.

Como você viu em um tutorial anterior, você obtém o valor de uma caixa de texto usando uma expressão como `Request.Form["name"]`, onde *nome* é o nome da `<input>` elemento.

Os nomes das variáveis (`title`, `genre`, e `year`) são arbitrárias. Como os nomes que você atribui a `<input>` elementos, você pode chamá-los que desejar. (Os nomes das variáveis não precisam corresponder os atributos de nome de `<input>` elementos no formulário.) Mas assim como acontece com o `<input>` elementos, é uma boa ideia usar nomes de variáveis que refletem os dados que eles contêm. Quando você escreve o código, nomes consistentes tornam mais fácil de lembrar quais dados você está trabalhando.

## <a name="adding-data-to-the-database"></a>Adicionando dados ao banco de dados

No código bloquear você recém-adicionada, basta *dentro de* a chave de fechamento ( `}` ) do `if` bloquear (não apenas dentro do bloco de código), adicione o seguinte código:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Este exemplo é semelhante ao código usado em um tutorial anterior para busca e exibir os dados. A linha que começa com `db =` abre o banco de dados, como antes e a próxima linha define uma instrução SQL, novamente como você viu antes. No entanto, desta vez ele define um SQL `Insert Into` instrução. O exemplo a seguir mostra a sintaxe geral do `Insert Into` instrução:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

Em outras palavras, você especificar a tabela a ser inserido, em seguida, liste as colunas a ser inserido e, em seguida, lista os valores a serem inseridos. (Conforme observado anteriormente, SQL, não diferencia maiusculas de minúsculas, mas algumas pessoas capitalizar as palavras-chave para torná-lo mais fácil de ler o comando.)

As colunas que você está inserindo já estão listadas no comando — `(Title, Genre, Year)`. A parte interessante é como obter os valores de caixas de texto para o `VALUES` faz parte do comando. Em vez de valores reais, você verá `@0`, `@1`, e `@2`, é claro, que são espaços reservados. Quando você executa o comando (no `db.Execute` linha), você passa os valores que você obteve as caixas de texto.

**Importante!** Lembre-se de que a única maneira de você nunca deve incluir os dados inseridos online por um usuário em uma instrução SQL é usar espaços reservados, como você pode ver aqui (`VALUES(@0, @1, @2)`). Se você concatenar a entrada do usuário em uma instrução SQL, você abre por conta própria a um ataque de injeção de SQL, conforme explicado em [Noções básicas de formulário em páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251581) (tutorial anterior).

Ainda dentro de `if` blocos, adicione a seguinte linha após o `db.Execute` linha:

[!code-css[Main](entering-data/samples/sample4.css)]

Depois que o novo filme foi inserido no banco de dados, esta linha você (redirecionamentos) salta para o *filmes* página para que você possa ver o filme que você acabou de digitar. O `~` operador significa "" raiz"do site. (O `~` operador geralmente funciona apenas em páginas ASP.NET, não em HTML.)

O bloco de código completo é semelhante a este exemplo:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Teste o comando de inserção (até o momento)

Você não ainda terminou, mas agora é um bom momento para testar.

Na exibição de árvore de arquivos no WebMatrix, clique com botão direito do *AddMovie.cshtml* da página e, em seguida, clique em **iniciar no navegador**.

!['Adicionar filme' página no navegador](entering-data/_static/image2.png)

(Se você acabará com uma página diferente no navegador, certifique-se de que a URL é `http://localhost:nnnnn/AddMovie`), onde *nnnnn* é o número da porta que você está usando.)

Você obteve uma página de erro? Nesse caso, leia com atenção e certifique-se de que o código parece exatamente o que estava listado anteriormente.

Insira um filme no formulário &mdash; por exemplo, use "Cidadão Kane", "Drama" e "1941". (Ou qualquer outro.) Em seguida, clique em **filme adicionar**.

Se tudo correr bem, você será redirecionado para o *filmes* página. Certifique-se de que o novo filme esteja listado.

![Página de filmes mostrando recentemente adicionada filme](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Validação de entrada do usuário

Volte para o *AddMovie* página ou executá-lo novamente. Insira outro filme, mas desta vez, insira somente o título &mdash; por exemplo, digite "Entrando ' no the Rain". Em seguida, clique em **filme adicionar**.

Você será redirecionado para o *filmes* página novamente. Você pode encontrar o novo filme, mas ele está incompleto.

![Página de filmes mostrando novo filme que alguns valores está ausentes](entering-data/_static/image4.png)

Quando você criou o *filmes* tabela, você explicitamente disse que nenhum dos campos pode ser nulo. Aqui, você tem um formulário de entrada para novos filmes e que está saindo campos em branco. Que é um erro.

Nesse caso, o banco de dados, na verdade, não geram (ou *lançar*) um erro. Você não forneceu um gênero ou um ano, portanto, o código na *AddMovie* página tratado esses valores como chamados *cadeias de caracteres vazias*. Quando o SQL `Insert Into` comando foi executado, os campos de gênero e ano não tinham dados úteis neles, mas eles não eram nulos.

Obviamente, você não deseja permitir que os usuários a inserir informações sobre filmes metade vazio no banco de dados. A solução é validar a entrada do usuário. Inicialmente, a validação simplesmente garantirá que o usuário inseriu um valor para todos os campos (ou seja, que nenhuma delas contém uma cadeia de caracteres vazia).

> [!TIP]
> 
> **Cadeias de caracteres vazias e nulas**
> 
> Em programação, há uma distinção entre Noções diferentes de "sem valor". Em geral, é um valor *nulo* se ele nunca foi definido ou inicializado de forma alguma. Em contraste, uma variável que espera dados de caracteres (cadeias de caracteres) pode ser definida como um *cadeia de caracteres vazia*. Nesse caso, o valor não é nulo. ele é apenas foi explicitamente definido como uma cadeia de caracteres cujo comprimento é zero. Essas duas instruções mostram a diferença:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> É um pouco mais complicado do que isso, mas o ponto importante é que `null` represente um tipo de estado indeterminado.
> 
> Agora e, em seguida, é importante compreender exatamente quando um valor é nulo e quando ele é apenas uma cadeia de caracteres vazia. No código para o *AddMovie* página, você obtém os valores das caixas de texto usando `Request.Form["title"]` e assim por diante. Quando a página executa pela primeira vez (antes de você clicar no botão), o valor de `Request.Form["title"]` é nulo. Mas quando você enviar o formulário `Request.Form["title"]` obtém o valor da `title` caixa de texto. Não é óbvio, mas uma caixa de texto vazia não é nula. Ele apenas tem uma cadeia de caracteres vazia. Portanto, quando o código é executado em resposta ao botão clique, `Request.Form["title"]` tem uma cadeia de caracteres vazia em si.
> 
> Por que essa distinção é importante? Quando você criou o *filmes* tabela, você explicitamente disse que nenhum dos campos pode ser nulo. Mas aqui, você tem um formulário de entrada para novos filmes e que está saindo campos em branco. Você seria razoável esperar o banco de dados reclamará quando você tentou salvar novos filmes que não têm valores para gênero ou ano. Mas esse é o ponto &mdash; mesmo se você deixar essas caixas de texto em branco, os valores não nulos; elas estão cadeias de caracteres vazias. Como resultado, você é capaz de salvar novos filmes no banco de dados com essas colunas vazias &mdash; , mas não nulo! valores &mdash;. Portanto, é necessário que certificar-se de que os usuários não enviar uma cadeia de caracteres vazia, o que pode ser feito por meio da validação de entrada do usuário.


### <a name="the-validation-helper"></a>O auxiliar de validação

Páginas da Web ASP.NET inclui um auxiliar &mdash; as `Validation` auxiliar &mdash; que você pode usar para certificar-se de que os usuários insiram dados que atenda às suas necessidades. O `Validation` auxiliar é um dos auxiliares que é interna para ASP.NET Web Pages, portanto, você não precisa instalá-lo como um pacote usando o NuGet, a maneira que você instalou o Gravatar auxiliar em um tutorial anterior.

Para validar a entrada do usuário, você fará o seguinte:

- Use o código para especificar que você deseja exigir valores nas caixas de texto na página.
- Coloque um teste no código para que as informações do filme seja adicionadas ao banco de dados somente se tudo for validado corretamente.
- Adicione o código na marcação para exibir mensagens de erro.

No bloco de código a *AddMovie* de página, direita para cima na parte superior antes das declarações de variável, adicione o seguinte código:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Você chama `Validation.RequireField` uma vez para cada campo (`<input>` elemento) onde você deseja exigir uma entrada. Você também pode adicionar uma mensagem de erro personalizada para cada chamada, como você vê aqui. (Estamos variadas as mensagens apenas para mostrar que você pode colocar qualquer coisa que você deseja lá).

Se houver um problema, você deseja impedir que as novas informações do filme que está sendo inserido no banco de dados. No `if(IsPost)` bloco, use `&&` (AND lógico) para adicionar outra condição que testa `Validation.IsValid()`. Quando você terminar, todo o `if(IsPost)` bloco se parece com este código:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Se não houver um erro de validação com qualquer um dos campos que você registrou usando o `Validation` auxiliar, o `Validation.IsValid` método retornará false. E, nesse caso, nenhum código daquele bloco será executado, portanto, nenhuma entrada inválida de filme será inserida no banco de dados. E, obviamente você não será redirecionado para o *filmes* página.

O bloco de código completo, incluindo o código de validação, agora se parece com este exemplo:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Exibindo erros de validação

A última etapa é exibir qualquer mensagem de erro. Você pode exibir as mensagens individuais para cada erro de validação, ou você pode exibir um resumo ou ambos. Para este tutorial, você fará ambos para que você possa ver como ele funciona.

Ao lado de cada `<input>` elemento que você está validando, a chamada a `Html.ValidationMessage` método e passe o nome da `<input>` elemento você está validando. Você coloca o `Html.ValidationMessage` à direita do método onde você deseja que a mensagem de erro seja exibida. Quando a página é executada, o `Html.ValidationMessage` método renderiza um `<span>` elemento aonde o erro de validação. (Se não houver nenhum erro, o `<span>` elemento é processado, mas não há nenhum texto nele.)

Altere a marcação na página de modo que ela inclua uma `Html.ValidationMessage` método para cada um dos três `<input>` elementos na página, como neste exemplo:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Para ver como funciona o resumo, também adicione a seguinte marcação e código logo após o `<h1>Add a Movie</h1>` elemento na página:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Por padrão, o `Html.ValidationSummary` método exibe todas as mensagens de validação em uma lista (uma `<ul>` elemento que está dentro de um `<div>` elemento). Assim como acontece com o `Html.ValidationMessage` método, a marcação para o resumo de validação é sempre renderizada; se não houver nenhum erro, nenhum item de lista é renderizados.

O resumo pode ser uma maneira alternativa de exibir mensagens de validação em vez de por meio de `Html.ValidationMessage` método para exibir cada erro de campo específicos. Ou você pode usar um resumo e detalhes. Ou você pode usar o `Html.ValidationSummary` método para exibir um erro genérico e, em seguida, usar individuais `Html.ValidationMessage` chamadas para exibir detalhes.

A página concluída agora se parece com este exemplo:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

Isso é tudo. Agora você pode testar a página pela adição de um filme, mas omitindo um ou mais dos campos. Quando você fizer isso, consulte o seguinte erro de exibição:

![Adicionar página de filme mostrando mensagens de erro de validação](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>As mensagens de erro de validação de definição de estilo

Você pode ver que há mensagens de erro, mas eles não realmente se destacam muito bem. Há uma maneira fácil de definir o estilo de mensagens de erro, no entanto.

Para definir o estilo de mensagens de erro individuais que são exibidas pelo `Html.ValidationMessage`, crie uma classe de estilo CSS denominada `field-validation-error`. Para definir a aparência para o resumo de validação, crie uma classe de estilo CSS chamada `validation-summary-errors`.

Para ver como essa técnica funciona, adicione uma `<style>` elemento dentro do `<head>` seção da página. Em seguida, definir classes de estilo nomeadas `field-validation-error` e `validation-summary-errors` que contêm as seguintes regras:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Normalmente você provavelmente seria colocar informações de estilo em um separado *. CSS* arquivo, mas para manter a simplicidade pode colocá-los na página por enquanto. (Posteriormente neste conjunto de tutoriais, você moverá as regras de CSS para um separado *. CSS* arquivo.)

Se não houver um erro de validação, o `Html.ValidationMessage` método renderiza um `<span>` elemento inclui `class="field-validation-error"`. Adicionando uma definição de estilo para essa classe, você pode configurar a aparência a mensagem. Se houver erros, o `ValidationSummary` método processa da mesma forma dinamicamente o atributo `class="validation-summary-errors"`.

Execute novamente a página e deliberadamente deixe alguns dos campos. Os erros agora são mais perceptíveis. (Na verdade, eles são houver abuso, mas que é apenas para mostrar o que você pode fazer.)

![Adicionar página de filme mostrando erros de validação que tem sido criado o estilo](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Adicionar um Link para a página de filmes

Uma etapa final é tornar fácil obter o *AddMovie* página da listagem de filmes original.

Abra o *filmes* página novamente. Após o fechamento `</div>` marca que segue o `WebGrid` auxiliar, adicione a seguinte marcação:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Como você viu antes, o ASP.NET interpreta o `~` operador como a raiz do site. Você não precisa usar o `~` operador; você pode usar a marcação `<a href="./AddMovie">Add a movie</a>` ou alguma outra maneira para definir o caminho que entende de HTML. Mas o `~` operador é uma boa abordagem geral quando você cria links para páginas do Razor, pois ele torna o site mais flexíveis — se você mover a página atual para uma subpasta, o link ainda irá para o *AddMovie* página. (Lembre-se de que o `~` operador funciona apenas *. cshtml* páginas. ASP.NET entende, mas não é HTML padrão.)

Quando terminar, execute as *filmes* página. Ele será semelhante a esta página:

![Página de filmes com link para a página 'Adicionar filmes'](entering-data/_static/image7.png)

Clique o **adicionar um filme** link para certificar-se de que ele vai para o *AddMovie* página.

## <a name="coming-up-next"></a>Próximo

No próximo tutorial, você aprenderá a permitir aos usuários editar dados que já estão no banco de dados.

## <a name="complete-listing-for-addmovie-page"></a>Listagem completa para a página AddMovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação Web do ASP.NET usando a sintaxe Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Inserir na instrução SQL](http://www.w3schools.com/sql/sql_insert.asp) no site W3Schools
- [Validação de entrada do usuário na Web ASP.NET de Sites de páginas](https://go.microsoft.com/fwlink/?LinkId=253002). Para obter mais informações sobre como trabalhar com o `Validation` auxiliar.

> [!div class="step-by-step"]
> [Anterior](form-basics.md)
> [Próximo](updating-data.md)
