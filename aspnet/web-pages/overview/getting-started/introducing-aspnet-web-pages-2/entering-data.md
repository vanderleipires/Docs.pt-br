---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
title: "Introdução a páginas da Web ASP.NET - inserindo dados de banco de dados usando formulários | Microsoft Docs"
author: tfitzmac
description: "Este tutorial mostra como criar um formulário de entrada e, em seguida, insira os dados que você obtém do formulário em uma tabela de banco de dados quando você usa o ASP.NET Web Pages (..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: d37c93fc-25fd-4e94-8671-0d437beef206
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/entering-data
msc.type: authoredcontent
ms.openlocfilehash: b74eecb16b2c4695bb417816b90f701f724cc9d0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---entering-database-data-by-using-forms"></a>Introdução a páginas da Web ASP.NET - inserindo dados de banco de dados usando formulários
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como criar um formulário de entrada e, em seguida, insira os dados que você obtém do formulário em uma tabela de banco de dados quando você usa páginas da Web do ASP.NET (Razor). Ele pressupõe que você tenha concluído a série por meio de [Noções básicas de formulários de HTML em páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251581).
> 
> O que você aprenderá:
> 
> - Mais informações sobre como processar formulários de entrada.
> - Como adicionar (inserir) dados em um banco de dados.
> - Como certificar-se de que os usuários tem inserido um valor obrigatório em um formulário (como validar a entrada do usuário).
> - Como exibir erros de validação.
> - Como saltar para outra página da página atual.
>   
> 
> Recursos/tecnologias abordadas:
> 
> - O método `Database.Execute`.
> - O SQL `Insert Into` instrução
> - O `Validation` auxiliar.
> - O método `Response.Redirect`.


## <a name="what-youll-build"></a>O que você vai criar

No tutorial anterior que mostrou como criar um banco de dados, você inseriu dados do banco de dados editando o banco de dados diretamente no WebMatrix, trabalhando no **banco de dados** espaço de trabalho. Na maioria dos aplicativos, que não é uma maneira prática de colocar dados no banco de dados, embora. Portanto, neste tutorial, você criará uma interface baseada na web que permite que você ou qualquer pessoa que inserir dados e salvá-lo no banco de dados.

Você criará uma página onde você pode inserir novos filmes. A página conterá um formulário de entrada que tem campos (caixas de texto) de onde você pode inserir um título do filme, gênero e ano. A página será semelhante a esta página:

!['Adicionar filme' page no navegador](entering-data/_static/image1.png)

As caixas de texto será HTML `<input>` elementos parecerá essa marcação, como:

`<input type="text" name="genre" value="" />`

## <a name="creating-the-basic-entry-form"></a>Criando o formulário de entrada básico

Crie uma página chamada *AddMovie.cshtml*.

Substitua o que está no arquivo com a seguinte marcação. Substituir tudo. Você adicionará um bloco de código na parte superior em breve.

[!code-cshtml[Main](entering-data/samples/sample1.cshtml)]

Este exemplo mostra o HTML típico para a criação de um formulário. Ele usa `<input>` elementos para as caixas de texto e no botão Enviar. As legendas para caixas de texto são criadas usando o padrão `<label>` elementos. O `<fieldset>` e `<legend>` elementos colocar uma caixa adequada ao redor do formulário.

Observe que nessa página, o `<form>` elemento usa `post` como o valor para o `method` atributo. No tutorial anterior, você criou um formulário que é usado o `get` método. Isso é correto, porque embora valores para o servidor de enviar o formulário, a solicitação não fez alterações. Tudo o que foi buscar dados de maneiras diferentes. No entanto, nesta página que você *será* fazer alterações, você vai adicionar novos registros de banco de dados. Portanto, este formulário deve usar o `post` método. (Para obter mais informações sobre a diferença entre `GET` e `POST` operações, consulte o[GET, POST e HTTP verbo segurança](https://go.microsoft.com/fwlink/?LinkId=251581#GET,_POST,_and_HTTP_Verb_Safety) lateral no tutorial anterior.)

Observe que cada caixa de texto tem uma `name` elemento (`title`, `genre`, `year`). Como você viu no tutorial anterior, esses nomes são importantes, porque você deve ter esses nomes para que você possa obter a entrada do usuário mais tarde. Você pode usar qualquer nome. É útil usar nomes significativos que ajudam você lembre-se os dados que você está trabalhando.

O `value` atributo de cada `<input>` elemento contém um trecho de código do Razor (por exemplo, `Request.Form["title"]`). Você aprendeu uma versão desse truque no tutorial anterior para preservar o valor inserido na caixa de texto (se houver) depois que o formulário é enviado.

## <a name="getting-the-form-values"></a>Obter os valores de formulário

Em seguida, você adiciona o código que processa o formulário. Na estrutura de tópicos, você fará o seguinte:

1. Verifique se a página está sendo lançada (enviado). Você deseja para seu código executar somente quando os usuários clicaram no botão, não quando a página é executada.
2. Obter os valores que o usuário inseriu nas caixas de texto. Nesse caso, porque o formulário está usando o `POST` verbo, você obtém os valores de formulário do `Request.Form` coleção.
3. Insira os valores como um novo registro no *filmes* tabela de banco de dados.

Na parte superior do arquivo, adicione o seguinte código:

[!code-cshtml[Main](entering-data/samples/sample2.cshtml)]

As primeiras linhas criam variáveis (`title`, `genre`, e `year`) para manter os valores de caixas de texto. A linha `if(IsPost)` certifica-se de que as variáveis são definidas *somente* quando os usuários clicarem o **Adicionar filme** botão — ou seja, quando o formulário foi lançado.

Como você viu um tutorial anterior, você obter o valor de uma caixa de texto usando uma expressão como `Request.Form["name"]`, onde *nome* é o nome do `<input>` elemento.

Os nomes das variáveis (`title`, `genre`, e `year`) são arbitrários. Como os nomes que você atribui a `<input>` elementos, você pode chamá-los que desejar. (Os nomes das variáveis não precisam coincidir com os atributos de nome de `<input>` elementos no formulário.) Mas, assim como acontece com o `<input>` elementos, é uma boa ideia usar nomes de variáveis que refletem os dados que eles contêm. Quando você escreve código, nomes consistentes torná-lo mais fácil de lembrar os dados que você está trabalhando.

## <a name="adding-data-to-the-database"></a>Adicionando dados ao banco de dados

No código do bloco você recém-adicionada, apenas *dentro de* a chave de fechamento ( `}` ) da `if` bloco (não apenas dentro do bloco de código), adicione o seguinte código:

[!code-csharp[Main](entering-data/samples/sample3.cs)]

Este exemplo é semelhante ao código usado em um tutorial anterior para busca e exibir os dados. A linha que começa com `db =` abre o banco de dados, como antes e a próxima linha define uma instrução SQL, novamente como você viu antes. No entanto, neste momento ele define um SQL `Insert Into` instrução. O exemplo a seguir mostra a sintaxe geral do `Insert Into` instrução:

`INSERT INTO table (column1, column2, column3, ...) VALUES (value1, value2, value3, ...)`

Em outras palavras, você especificar a tabela para inserir, lista as colunas para inserir e, em seguida, lista de valores a serem inseridos. (Conforme observado anteriormente, o SQL não diferencia maiusculas de minúsculas, mas algumas pessoas aproveitar as palavras-chave para facilitar a leitura do comando.)

As colunas que você está inserindo já estão listadas no comando — `(Title, Genre, Year)`. A parte interessante é como obter os valores de caixas de texto para o `VALUES` faz parte do comando. Em vez de valores reais, você verá `@0`, `@1`, e `@2`, é claro que são espaços reservados. Quando você executar o comando (no `db.Execute` linha), você passa os valores que você obteve as caixas de texto.

**Importante!** Lembre-se de que a única maneira de você nunca deve incluir dados inseridos on-line por um usuário em uma instrução SQL é usar espaços reservados, como você vê aqui (`VALUES(@0, @1, @2)`). Se você concatenar a entrada do usuário em uma instrução SQL, abrir-se a um ataque de injeção de SQL, conforme explicado em [Noções básicas de formulário em páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251581) (tutorial anterior).

Ainda dentro de `if` em bloco, adicione a seguinte linha após a `db.Execute` linha:

[!code-css[Main](entering-data/samples/sample4.css)]

Depois que o novo filme tenha sido inserido no banco de dados, essa linha é (redirecionamentos) salta para o *filmes* página para ver o filme que você acabou de digitar. O `~` operador significa "raiz do site". (O `~` operador geralmente funciona apenas em páginas do ASP.NET, não em HTML.)

O bloco de código completo é semelhante a este exemplo:

[!code-cshtml[Main](entering-data/samples/sample5.cshtml)]

## <a name="testing-the-insert-command-so-far"></a>Teste o comando Insert (até)

Você não ainda terminou, mas agora é um bom momento para testar.

Na exibição de árvore de arquivos no WebMatrix, clique com botão direito do *AddMovie.cshtml* página e, em seguida, clique em **iniciar no navegador**.

!['Adicionar filme' page no navegador](entering-data/_static/image2.png)

(Se você acabará com uma página diferente no navegador, certifique-se de que a URL é `http://localhost:nnnnn/AddMovie`), onde  *nnnnn*  é o número da porta que você está usando.)

Você obteve uma página de erro? Em caso afirmativo, leia com atenção e certifique-se de que o código é exatamente o que estava listado anteriormente.

Insira um filme no formato &mdash; , por exemplo, use "Cidadão Kane", "Drama" e "1941". (Ou qualquer outro.) Em seguida, clique em **Adicionar filme**.

Se tudo correr bem, você será redirecionado para a *filmes* página. Certifique-se de que seu novo filme está listado.

![Página de filmes mostrando recentemente adicionada filme](entering-data/_static/image3.png)

## <a name="validating-user-input"></a>Validando a entrada do usuário

Volte para o *AddMovie* página ou executá-lo novamente. Digite outro filme, mas desta vez, digite somente o título &mdash; por exemplo, digite "Entrando ' no chuva". Em seguida, clique em **Adicionar filme**.

Você será redirecionado para a *filmes* página novamente. Você pode encontrar o novo filme, mas está incompleto.

![Página de filmes mostrando novo filme que alguns valores está ausentes](entering-data/_static/image4.png)

Quando você criou o *filmes* tabela, você explicitamente disse que nenhum dos campos pode ser nulo. Aqui você pode ter um formulário de entrada de novos filmes e está saindo campos em branco. Que é um erro.

Nesse caso, o banco de dados, na verdade, não gerar (ou *gerar*) um erro. Você não forneceu um gênero ou ano, portanto o código a *AddMovie* página tratado esses valores como chamados *cadeias de caracteres vazias*. Quando o SQL `Insert Into` comando foi executado, os campos de gênero e ano não possuem dados úteis, mas eles não eram nulos.

Obviamente, você não deseja permitir que os usuários inserir informações de filme metade vazio no banco de dados. A solução é validar a entrada do usuário. Inicialmente, a validação simplesmente garantirá que o usuário inseriu um valor para todos os campos (ou seja, nenhuma delas contém uma cadeia de caracteres vazia).

> [!TIP] 
> 
> **Cadeias de caracteres nulas e vazias**
> 
> Em programação, há uma distinção entre Noções diferentes de "sem valor". Em geral, um valor é *nulo* se ela nunca foi definida ou inicializada de forma alguma. Em contraste, uma variável que espera dados de caractere (cadeias de caracteres) pode ser definida como um *cadeia de caracteres vazia*. Nesse caso, o valor não é nulo. ele é apenas foi explicitamente definido como uma cadeia de caracteres cujo tamanho é zero. Essas duas instruções mostram a diferença:
> 
> [!code-csharp[Main](entering-data/samples/sample6.cs)]
> 
> É um pouco mais complicado do que o, mas o ponto importante é que `null` representa um tipo de estado indeterminado.
> 
> Agora e, em seguida, é importante entender exatamente quando um valor é nulo e quando ele é apenas uma cadeia de caracteres vazia. No código para o *AddMovie* página, você obter os valores das caixas de texto usando `Request.Form["title"]` e assim por diante. Quando a página executa pela primeira vez (antes de você clicar no botão), o valor de `Request.Form["title"]` é nulo. Porém, quando você enviar o formulário, `Request.Form["title"]` obtém o valor da `title` caixa de texto. Não é óbvio, mas uma caixa de texto vazia não é nula. Ele apenas tem uma cadeia de caracteres vazia. Portanto, quando o código é executado em resposta ao botão clique, `Request.Form["title"]` exibe uma cadeia de caracteres vazia.
> 
> Por essa distinção é importante? Quando você criou o *filmes* tabela, você explicitamente disse que nenhum dos campos pode ser nulo. Mas aqui é ter um formulário de entrada de novos filmes e está saindo campos em branco. Você esperaria razoavelmente o banco de dados para reclamar quando você tentou salvar novos filmes que não têm valores de gênero ou ano. Mas isso é o ponto de &mdash; mesmo se você deixar as caixas de texto, os valores não nulos; elas estão cadeias de caracteres vazias. Como resultado, você é capaz de salvar novos filmes em banco de dados com essas colunas vazias &mdash; , mas não nulo! valores &mdash;. Portanto, você precisa certificar-se de que os usuários não enviarem uma cadeia de caracteres vazia, o que pode ser feito por meio da validação de entrada do usuário.


### <a name="the-validation-helper"></a>O auxiliar de validação

Páginas da Web ASP.NET inclui um auxiliar &mdash; o `Validation` auxiliar &mdash; que você pode usar para certificar-se de que os usuários insiram dados que atenda às suas necessidades. O `Validation` auxiliar é um dos auxiliares que é interno para o ASP.NET Web Pages, não é necessário instalá-lo como um pacote usando o NuGet, da maneira que você instalou o auxiliar Gravatar um tutorial anterior.

Para validar a entrada do usuário, você fará o seguinte:

- Use o código para especificar que você deseja exigir valores nas caixas de texto na página.
- Colocar um teste no código para que as informações de filme são adicionadas ao banco de dados somente se tudo for validado corretamente.
- Adicione código para a marcação para exibir mensagens de erro.

No bloco de código a *AddMovie* página, direita para cima na parte superior antes de declarações de variável, adicione o seguinte código:

[!code-csharp[Main](entering-data/samples/sample7.cs)]

Você chamar `Validation.RequireField` uma vez para cada campo (`<input>` elemento) onde você deseja exigir uma entrada. Você também pode adicionar uma mensagem de erro personalizada para cada chamada, como você vê aqui. (Nós variadas as mensagens apenas para mostrar que você pode colocar qualquer existe).

Se houver um problema, você deseja impedir que as novas informações de filme seja inserido no banco de dados. No `if(IsPost)` em bloco, use `&&` (AND lógico) para adicionar outra condição que testa `Validation.IsValid()`. Quando você terminar, todo o `if(IsPost)` bloco é parecido com este código:

[!code-csharp[Main](entering-data/samples/sample8.cs)]

Se houver um erro de validação com qualquer um dos campos que você registrou usando o `Validation` auxiliar, o `Validation.IsValid` método retornará false. E, nesse caso, nenhum código no bloco serão executados, portanto, não há entradas de filme inválido serão inseridas no banco de dados. E obviamente você não será redirecionado para a *filmes* página.

O bloco de código completo, incluindo o código de validação, agora se parece com este exemplo:

[!code-cshtml[Main](entering-data/samples/sample9.cshtml?highlight=10)]

## <a name="displaying-validation-errors"></a>Exibindo erros de validação

A última etapa é exibir as mensagens de erro. Você pode exibir as mensagens individuais para cada erro de validação, ou você pode exibir um resumo ou ambos. Para este tutorial, você realizará ambos para que você possa ver como ele funciona.

Ao lado de cada `<input>` elemento que você está validando, chame o `Html.ValidationMessage` método e passe o nome do `<input>` elemento sendo validada. Colocar o `Html.ValidationMessage` direita do método onde você deseja que a mensagem de erro. Quando a página é executada, o `Html.ValidationMessage` método renderiza um `<span>` elemento aonde o erro de validação. (Se não houver nenhum erro, o `<span>` elemento é processado, mas não há nenhum texto nele.)

Alterar a marcação na página de modo que ele inclui um `Html.ValidationMessage` método para cada um dos três `<input>` elementos na página, como neste exemplo:

[!code-cshtml[Main](entering-data/samples/sample10.cshtml?highlight=3,8,13)]

Para ver como funciona o resumo, também adicione a seguinte marcação e código logo após o `<h1>Add a Movie</h1>` elemento na página:

[!code-cshtml[Main](entering-data/samples/sample11.cshtml)]

Por padrão, o `Html.ValidationSummary` método exibe todas as mensagens de validação em uma lista (um `<ul>` elemento que está dentro de um `<div>` elemento). Assim como acontece com o `Html.ValidationMessage` método, a marcação para o resumo de validação é sempre renderizada; se não houver nenhum erro, nenhum item de lista é renderizados.

O resumo pode ser uma maneira alternativa de exibir mensagens de validação em vez de por meio de `Html.ValidationMessage` método para exibir cada erro de campo específicos. Ou você pode usar um resumo e os detalhes. Ou você pode usar o `Html.ValidationSummary` método para exibir um erro genérico e, em seguida, usar individuais `Html.ValidationMessage` chamadas para exibir os detalhes.

A página concluída agora se parece com este exemplo:

[!code-cshtml[Main](entering-data/samples/sample12.cshtml)]

É isso. Agora você pode testar a página pela adição de um filme, mas omitindo um ou mais campos. Quando você fizer isso, consulte o seguinte erro de exibição:

![Adicionar página de filme mostra mensagens de erro de validação](entering-data/_static/image5.png)

## <a name="styling-the-validation-error-messages"></a>As mensagens de erro de validação de estilo

Você pode ver que há mensagens de erro, mas eles não realmente destacam muito bem. Há uma maneira fácil para definir o estilo de mensagens de erro, embora.

Para definir o estilo de mensagens de erro individuais que são exibidas por `Html.ValidationMessage`, crie uma classe de estilo CSS denominada `field-validation-error`. Para definir a aparência para o resumo de validação, crie uma classe de estilo CSS denominada `validation-summary-errors`.

Para ver como essa técnica funciona, adicione um `<style>` elemento dentro do `<head>` seção da página. Em seguida, definir classes de estilo nomeadas `field-validation-error` e `validation-summary-errors` que contêm as seguintes regras:

[!code-cshtml[Main](entering-data/samples/sample13.cshtml?highlight=4-17)]

Normalmente você provavelmente seria colocar informações de estilo em uma separada *. CSS* arquivo, mas para simplificar você pode colocá-los na página agora. (Posteriormente neste tutorial conjunto, você moverá as regras de CSS para um separado *. CSS* arquivo.)

Se houver um erro de validação, o `Html.ValidationMessage` método renderiza um `<span>` elemento inclui `class="field-validation-error"`. Adicionando uma definição de estilo para a classe, você pode configurar a aparência de mensagem. Se houver erros, o `ValidationSummary` método renderiza da mesma forma dinamicamente o atributo `class="validation-summary-errors"`.

Execute novamente a página e deixe deliberadamente alguns dos campos. Agora, os erros são mais notáveis. (Na verdade, eles estiverem já a terá desobedecidos, mas isso é apenas para mostrar o que você pode fazer.)

![Adicionar página de filme mostrando erros de validação que tenham sido estilo](entering-data/_static/image6.png)

## <a name="adding-a-link-to-the-movies-page"></a>Adicionar um Link para a página de filmes

A etapa final é torná-lo conveniente para obter o *AddMovie* página da listagem de filme original.

Abra o *filmes* página novamente. Após o fechamento `</div>` marca que segue o `WebGrid` auxiliar, adicione a seguinte marcação:

[!code-cshtml[Main](entering-data/samples/sample14.cshtml)]

Como você viu antes, o ASP.NET interpreta o `~` operador como a raiz do site. Você não precisa usar o `~` operador; você pode usar a marcação `<a href="./AddMovie">Add a movie</a>` ou alguma outra maneira para definir o caminho que reconheça o HTML. Mas o `~` operador é uma boa abordagem geral quando você criar links para páginas Razor, pois ele torna o site mais flexível — se você mover a página atual para uma subpasta, o link ainda irá para o *AddMovie* página. (Lembre-se de que o `~` operador só funciona *. cshtml* páginas. Entenda ASP.NET, mas não é HTML padrão.)

Quando terminar, execute o *filmes* página. Ele será semelhante a esta página:

![Página de filmes com link para a página 'Adicionar filmes'](entering-data/_static/image7.png)

Clique o **adicionar um filme** link para certificar-se de que ele passa para o *AddMovie* página.

## <a name="coming-up-next"></a>Próximo

O seguinte tutorial, você aprenderá como permitir aos usuários editar dados que já está no banco de dados.

## <a name="complete-listing-for-addmovie-page"></a>Lista completa de página AddMovie

[!code-cshtml[Main](entering-data/samples/sample15.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [Inserir na instrução SQL](http://www.w3schools.com/sql/sql_insert.asp) no site W3Schools
- [Validando a entrada do usuário da Web do ASP.NET páginas Sites](https://go.microsoft.com/fwlink/?LinkId=253002). Para obter mais informações sobre como trabalhar com o `Validation` auxiliar.

>[!div class="step-by-step"]
[Anterior](form-basics.md)
[Próximo](updating-data.md)
