---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: Introdução ao ASP.NET Web Pages – excluindo o banco de dados | Microsoft Docs
author: tfitzmac
description: Este tutorial mostra como excluir uma entrada de banco de dados individuais. Ele pressupõe que você tenha concluído a série por meio de atualização de banco de dados no PA da Web do ASP.NET....
ms.author: riande
ms.date: 01/02/2018
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: 3b759a5c88b066640005c823ce0cc3cc3ac89bc2
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834893"
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Introdução ao ASP.NET Web Pages – excluindo o banco de dados
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como excluir uma entrada de banco de dados individuais. Ele pressupõe que você tenha concluído a série por meio [atualização do banco de dados em páginas da Web do ASP.NET](updating-data.md).
> 
> O que você aprenderá:
> 
> - Como selecionar um registro individual de uma lista de registros.
> - Como excluir um único registro de um banco de dados.
> - Como verificar se um botão específico foi clicado em um formulário.
>   
> 
> Recursos/tecnologias abordadas:
> 
> - O `WebGrid` auxiliar.
> - O SQL `Delete` comando.
> - O `Database.Execute` método para executar um SQL `Delete` comando.


## <a name="what-youll-build"></a>O que você vai criar

No tutorial anterior, você aprendeu como atualizar um registro de banco de dados existente. Este tutorial é semelhante, exceto que em vez de atualizar o registro, você poderá excluí-lo. Os processos são muito parecidas, exceto que a exclusão é mais simples, portanto, este tutorial é curto.

No *filmes* página, você atualizará o `WebGrid` auxiliar de modo que ele exibe um **excluir** link ao lado de cada filme para acompanhar o **editar** link adicionado anteriormente.

![Página de filmes mostrando um link de Delete para cada filme](deleting-data/_static/image1.png)

Assim como acontece com a edição, quando você clica o **excluir** link, ele leva você até uma página diferente, onde as informações do filme já estão em um formulário:

![Excluir página de filmes com um filme exibido](deleting-data/_static/image2.png)

Clique no botão para excluí-lo permanentemente.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Adicionar um Link de exclusão para a listagem de filmes

Você começa pela adição de um **excluir** link para o `WebGrid` auxiliar. Esse link é semelhante para o **editar** link adicionado em um tutorial anterior.

Abra o *Movies.cshtml* arquivo.

Alterar o `WebGrid` marcação no corpo da página, adicionando uma coluna. Aqui está a marcação modificada:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

A nova coluna é este:

[!code-html[Main](deleting-data/samples/sample2.html)]

A maneira como a grade estiver configurada, o **editar** coluna é mais à esquerda na grade e o **excluir** coluna é mais à direita. (Há uma vírgula após o `Year` coluna agora, caso você não notei isso.) Não há nada de especial sobre onde ir essas colunas de link, e você pode colocá-los tão facilmente próximos uns dos outros. Nesse caso, eles são separados para torná-los mais difícil de se misturar.

![Página de filmes com links de edição e detalhes marcada para mostrar que não estão próximos uns dos outros](deleting-data/_static/image3.png)

A nova coluna mostra um link (`<a>` elemento) cujo texto é "Excluir". O destino do link (sua `href` atributo) é o código que é resolvida, por fim, para algo como essa URL com o `id` valor diferente para cada filme:

[!code-css[Main](deleting-data/samples/sample3.css)]

Esse link invocará uma página chamada *DeleteMovie* e passá-lo a ID do filme que você selecionou.

Este tutorial não entrarei em detalhes sobre como esse link é construído, porque ele é quase idêntico do **editar** link do tutorial anterior ([atualização do banco de dados em páginas da Web do ASP.NET](updating-data.md)).

## <a name="creating-the-delete-page"></a>Criando a página Excluir

Agora você pode criar a página que será o destino para o **excluir** link na grade.

> [!NOTE] 
> 
> **Importante** a técnica de primeiro selecionando um registro a ser excluído e, em seguida, usando uma página separada e um botão para confirmar o processo é extremamente importante para a segurança. Como você já leu nos tutoriais anteriores, tornando *qualquer* tipo de alteração para o seu site deve *sempre* ser feito usando um formulário &mdash; ou seja, usando uma operação HTTP POST. Se você agora é possível alterar o site apenas clicando em um link (ou seja, usando uma operação GET), as pessoas poderia fazer solicitações simples para seu site e excluir seus dados. Até mesmo um rastreador de mecanismo de pesquisa que seu site é a indexação inadvertidamente puder excluir dados apenas por links a seguir.
> 
> Quando seu aplicativo permite que as pessoas alterar um registro, você precisa apresentar o registro para o usuário para edição de qualquer forma. Mas você pode ficar tentado a ignorar esta etapa para exclusão de um registro. Não ignore essa etapa, no entanto. (Também é útil para os usuários ver o registro e confirmar que ele está excluindo o registro que eles se destinam.)
> 
> Um conjunto de tutoriais subsequente, você verá como adicionar funcionalidade de logon para que um usuário precise entrar antes de excluir um registro.


Crie uma página chamada *DeleteMovie.cshtml* e substituir o que está no arquivo com a seguinte marcação:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Essa marcação é como o *EditMovie* páginas, exceto que em vez de usar as caixas de texto (`<input type="text">`), a marcação inclui `<span>` elementos. Não há nada aqui para editar. Tudo o que você precisa fazer é exibir os detalhes do filme, para que os usuários podem ter certeza de que eles está excluindo o filme à direita.

A marcação já contém um link que permite que o usuário retornar à página de listagem de filmes.

Como mostra a *EditMovie* página, a ID do filme selecionado é armazenada em um campo oculto. (Ele é passado para a página inicialmente como um valor de cadeia de caracteres de consulta.) Há um `Html.ValidationSummary` chamada que exibirá os erros de validação. Nesse caso, o erro pode ser que não há ID de filme foi passado para a página ou a ID de filme é inválida. Essa situação pode ocorrer se alguém executou desta página sem primeiro selecionar um filme na *filmes* página.

É a legenda do botão **excluir filme**, e seu atributo name é definido como `buttonDelete`. O `name` atributo será usado no código para identificar o botão que enviou o formulário.

Você precisará escrever código para 1) Leia os detalhes do filme quando a página é exibida pela primeira vez e 2) exclui o filme quando o usuário clica no botão.

## <a name="adding-code-to-read-a-single-movie"></a>Adicionando código para ler um único filme

Na parte superior do *DeleteMovie.cshtml* página, adicione o seguinte bloco de código:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Essa marcação é o mesmo que o código correspondente na *EditMovie* página. Ele obtém a ID de filme fora a cadeia de caracteres de consulta e usa a ID para ler um registro do banco de dados. O código inclui o teste de validação (`IsInt()` e `row != null`) para certificar-se de que a ID de filme que está sendo passada para a página é válida.

Lembre-se de que esse código deve apenas ser executado na primeira vez em que a página é executada. Você não deseja ler novamente o registro de filme do banco de dados quando o usuário clica o **filme excluir** botão. Portanto, o código para ler o filme está dentro de um teste que diz `if(!IsPost)` &mdash; , ou seja, *se a solicitação não é uma operação post (envio do formulário)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Adicionando código para excluir o filme selecionado

Para excluir o filme quando o usuário clica no botão, adicione o seguinte código dentro da chave de fechamento do `@` bloco:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Esse código é semelhante ao código para atualizar um registro existente, mas é mais simples. Basicamente, o código executa um SQL `Delete` instrução.

 Como mostra a *EditMovie* página, o código está em um `if(IsPost)` bloco. Neste momento, o `if()` condição é um pouco mais complicada: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Há duas condições aqui. A primeira é que a página está sendo enviada, como você viu antes &mdash; `if(IsPost)`.

A segunda condição for `!Request["buttonDelete"].IsEmpty()`, que significa que a solicitação tem um objeto chamado `buttonDelete`. Sem dúvida, é uma maneira indireta de teste qual botão enviado o formulário. Se um formulário contiver vários botões de envio, apenas o nome do botão que foi clicado é exibido na solicitação. Portanto, logicamente, se o nome de um determinado botão aparece na solicitação &mdash; ou conforme declarado no código, se esse botão não estiver vazio &mdash; que é o botão que enviou o formulário.

O `&&` significa que o operador "e" (AND lógico). Portanto, todo o `if` condição é...

*Essa solicitação é uma postagem (não uma solicitação pela primeira vez)*  
  
 AND  
  
*O* `buttonDelete` *botão foi o botão que enviou o formulário.*

Este formulário (na verdade, essa página) contém apenas um botão, portanto, o teste adicional para `buttonDelete` tecnicamente não é necessária. Ainda assim, você está prestes a realizar uma operação que irá remover permanentemente os dados. Portanto, você queira Certifique-se como possível que você está executando a operação somente quando o usuário tiver solicitado explicitamente. Por exemplo, suponha que você expandido essa página posteriormente e adicionou outros botões para ele. Mesmo assim, o código que exclui o filme será executado somente se o `buttonDelete` botão foi clicado.

Como mostra a *EditMovie* página, obter a ID do campo oculto e, em seguida, execute o comando SQL. A sintaxe para o `Delete` instrução é:

`DELETE FROM table WHERE ID = value`

É vital para incluir o `WHERE` cláusula e a ID. Se você deixar a cláusula WHERE *todos os registros na tabela serão excluídos*. Como você viu, passar o valor da ID para o comando SQL usando um espaço reservado.

## <a name="testing-the-movie-delete-process"></a>Testar o processo de exclusão de filme

Agora você pode testar. Execute o *filmes* da página e clique em **excluir** ao lado de um filme. Quando o *DeleteMovie* página for exibida, clique em **filme excluir**.

![Excluir página de filmes com o botão Excluir filme realçado](deleting-data/_static/image4.png)

Quando você clica no botão, o código exclui os filmes e retorna para a listagem de filmes. Lá você pode procurar o filme excluído e confirme se ele foi excluído.

## <a name="coming-up-next"></a>Próximo

O próximo tutorial mostra como fornecer todas as páginas em seu site de uma aparência comum e o layout.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Listagem completa para a página de filme (atualizada com os Links excluir)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Listagem completa para a página DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação Web do ASP.NET usando a sintaxe do Razor](../introducing-razor-syntax-c.md)
- [Instrução DELETE do SQL](http://www.w3schools.com/sql/sql_delete.asp) no site W3Schools

> [!div class="step-by-step"]
> [Anterior](updating-data.md)
> [Próximo](layouts.md)
