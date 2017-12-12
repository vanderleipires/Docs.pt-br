---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
title: "Introdução a páginas da Web ASP.NET - excluir dados do banco de dados | Microsoft Docs"
author: tfitzmac
description: "Este tutorial mostra como excluir uma entrada de banco de dados individuais. Ele pressupõe que você tenha concluído a série por meio de atualização de banco de dados no PA da Web do ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 75b5c1cf-84bd-434f-8a86-85c568eb5b09
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/deleting-data
msc.type: authoredcontent
ms.openlocfilehash: aef31b6170cc3bba2421eb8c2c41e83aadc129c5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="introducing-aspnet-web-pages---deleting-database-data"></a>Introdução a páginas da Web ASP.NET - excluir dados do banco de dados
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial mostra como excluir uma entrada de banco de dados individuais. Ele pressupõe que você tenha concluído a série por meio de [atualização do banco de dados em páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251583).
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

No tutorial anterior, você aprendeu como atualizar um registro de banco de dados existente. Este tutorial é semelhante, exceto que, em vez de atualizar o registro, você poderá excluí-lo. Os processos são de modo semelhante, exceto que a exclusão é mais simples, para que seja curto neste tutorial.

No *filmes* página, você poderá atualizar o `WebGrid` auxiliar para que ele exibe um **excluir** link ao lado de cada filme para acompanhar o **editar** link adicionado anteriormente.

![Página de filmes mostrando um link de exclusão para cada filme](deleting-data/_static/image1.png)

Com edição, quando você clica o **excluir** link, ele leva a uma página diferente, onde as informações de filme já estão em um formulário:

![Excluir a página de filme com um filme exibido](deleting-data/_static/image2.png)

Você pode, em seguida, clique no botão para excluí-lo permanentemente.

## <a name="adding-a-delete-link-to-the-movie-listing"></a>Adicionar um Link de exclusão para a listagem de filme

Comece adicionando um **excluir** link para o `WebGrid` auxiliar. Este link é semelhante do **editar** link adicionado em um tutorial anterior.

Abra o *Movies.cshtml* arquivo.

Alteração de `WebGrid` marcação no corpo da página, adicionando uma coluna. Aqui está a marcação modificada:

[!code-html[Main](deleting-data/samples/sample1.html?highlight=9-10)]

A nova coluna é este:

[!code-html[Main](deleting-data/samples/sample2.html)]

A maneira como a grade é configurada, o **editar** coluna é mais à esquerda na grade e **excluir** coluna é mais à direita. (Há uma vírgula após o `Year` coluna agora, no caso de você nem notou que.) Não há nada especial sobre onde ir essas colunas de link, e você pode colocá-los facilmente próximos um do outro. Nesse caso, eles são separados para torná-los mais difícil obter misturados.

![Página de filmes com links de edição e detalhes marcada para mostrar que não estão próximos um do outro](deleting-data/_static/image3.png)

A nova coluna mostra um link (`<a>` elemento) cujo texto é "Excluir". O destino do link (seu `href` atributo) é o código que, por fim, resolve para algo como essa URL com o `id` valor diferente para cada filme:

[!code-css[Main](deleting-data/samples/sample3.css)]

Este link invocará uma página chamada *DeleteMovie* e passe a ID do filme que você selecionou.

Este tutorial não entrarei em detalhes sobre como esse link é construído, porque ele é quase idêntico de **editar** link do tutorial anterior ([atualização do banco de dados em páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251583)).

## <a name="creating-the-delete-page"></a>Criando a página de exclusão

Agora você pode criar a página que será o destino para o **excluir** link na grade.

> [!NOTE] 
> 
> **Importante** a técnica de primeiro selecionar um registro para excluir e, em seguida, usando uma página separada e o botão para confirmar o processo é extremamente importante para a segurança. Conforme você leu nos tutoriais anteriores, tornando *qualquer* deve do tipo de alteração para seu site *sempre* ser feito usando um formulário &mdash; ou seja, usando uma operação HTTP POST. Se você fez possível alterar o site bastando clicar em um link (ou seja, usando uma operação GET), as pessoas podem fazer solicitações simples para seu site e excluir seus dados. Até mesmo um rastreador de mecanismo de pesquisa que é a indexação de seu site pode inadvertidamente excluir dados apenas links a seguir.
> 
> Quando seu aplicativo permite que pessoas alterar um registro, você precisa apresentar o registro para o usuário para edição mesmo assim. Mas você poderá ficar tentado para ignorar esta etapa para exclusão de um registro. Não ignore essa etapa, embora. (Também é útil para os usuários vejam o registro e confirmar que ele está excluindo o registro que eles se destinam.)
> 
> Um tutorial subsequente, você verá como adicionar a funcionalidade de logon para que um usuário precisa entrar antes de excluir um registro.


Crie uma página chamada *DeleteMovie.cshtml* e substitua o que está no arquivo com a seguinte marcação:

[!code-cshtml[Main](deleting-data/samples/sample4.cshtml)]

Essa marcação é como o *EditMovie* páginas, exceto que, em vez de usar as caixas de texto (`<input type="text">`), a marcação inclui `<span>` elementos. Não há nada aqui para editar. Tudo o que você precisa fazer é exibir os detalhes do filme, para que os usuários podem Certifique-se de que eles está excluindo o filme à direita.

A marcação já contém um link que permite que o usuário retornar para a página de listagem do filme.

Como no *EditMovie* página, a ID do filme selecionado é armazenada em um campo oculto. (Ele é passado para a página da primeira vez como um valor de cadeia de caracteres de consulta.) Há um `Html.ValidationSummary` chamada com erros de validação. Nesse caso, o erro pode ser que nenhuma ID de filme foi passado para a página ou que a ID de filme é inválida. Essa situação pode ocorrer se alguém tiver executado essa página sem primeiro selecionar um filme no *filmes* página.

A legenda do botão é **excluir filme**, e seu nome de atributo é definido como `buttonDelete`. O `name` atributo será usado no código para identificar o botão que enviou o formulário.

Você precisará escrever código para 1) Leia os detalhes do filme quando a página é exibida pela primeira vez e 2) realmente excluir o filme quando o usuário clica no botão.

## <a name="adding-code-to-read-a-single-movie"></a>Adicionando código para ler um filme único

Na parte superior do *DeleteMovie.cshtml* página, adicione o seguinte bloco de código:

[!code-cshtml[Main](deleting-data/samples/sample5.cshtml)]

Essa marcação é o mesmo que o código correspondente no *EditMovie* página. Ele obtém a ID de filme fora a cadeia de caracteres de consulta e usa a ID para ler um registro do banco de dados. O código inclui o teste de validação (`IsInt()` e `row != null`) para certificar-se de que a ID de filme que está sendo passada para a página é válida.

Lembre-se de que esse código deve apenas ser executado na primeira vez em que a página é executada. Você não deseja ler novamente o registro de filme do banco de dados quando o usuário clica o **excluir filme** botão. Portanto, de código para ler o filme está dentro de um teste que diz `if(!IsPost)` &mdash; ou seja, *se a solicitação não é uma operação post (envio)*.

## <a name="adding-code-to-delete-the-selected-movie"></a>Adicionando código para excluir o filme selecionado

Para excluir o filme quando o usuário clica no botão, adicione o seguinte código dentro da chave de fechamento do `@` bloco:

[!code-csharp[Main](deleting-data/samples/sample6.cs)]

Esse código é semelhante ao código para atualizar um registro existente, mas é mais simples. Basicamente, o código executa um SQL `Delete` instrução.

 Como no *EditMovie* página, o código está em um `if(IsPost)` bloco. Neste momento, o `if()` condição é um pouco mais complicada: 

[!code-csharp[Main](deleting-data/samples/sample7.cs)]

Há duas condições aqui. A primeira é que a página está sendo enviada, como você viu antes &mdash; `if(IsPost)`.

A segunda condição for `!Request["buttonDelete"].IsEmpty()`, que significa que a solicitação tem um objeto chamado `buttonDelete`. Sem dúvida, é uma maneira indireta de teste que botão enviado o formulário. Se um formulário contém vários botões de envio, somente o nome do botão que foi clicado aparece na solicitação. Portanto, logicamente, se o nome de um determinado botão aparece na solicitação &mdash; ou conforme indicado no código, se esse botão não estiver vazio &mdash; que é o botão que enviou o formulário.

O `&&` operador significa "e" (AND lógico). Portanto, toda a `if` condição for...

*Essa solicitação é post (não é uma solicitação pela primeira vez)*  
  
 AND  
  
*O* `buttonDelete` *botão foi o botão que enviou o formulário.*

Este formulário (na verdade, essa página) contém apenas um botão, portanto o teste adicional para `buttonDelete` tecnicamente não é necessária. Ainda assim, você está prestes a executar uma operação que irá remover permanentemente os dados. Portanto, você queira Certifique-se como possíveis que você estiver executando a operação somente quando o usuário tiver solicitado explicitamente. Por exemplo, suponha que você expandido esta página mais tarde e adicionadas outros botões. Mesmo assim, o código que exclui o filme será executado somente se o `buttonDelete` botão foi clicado.

Como no *EditMovie* página, obter a ID do campo oculto e, em seguida, execute o comando SQL. A sintaxe para a `Delete` instrução é:

`DELETE FROM table WHERE ID = value`

É essencial para incluir o `WHERE` cláusula e a ID. Se você omitir a cláusula WHERE, *todos os registros na tabela serão excluídos*. Como você viu, você passar o valor de ID para o comando SQL usando um espaço reservado.

## <a name="testing-the-movie-delete-process"></a>Teste o processo de exclusão de filme

Agora você pode testar. Execute o *filmes* página e, em seguida, clique em **excluir** ao lado de um filme. Quando o *DeleteMovie* página aparece, clique em **excluir filme**.

![Excluir a página de filme com o botão Excluir filme realçado](deleting-data/_static/image4.png)

Quando você clica no botão, o código exclui filmes e retorna para a listagem do filme. Lá você pode procurar o filme excluído e confirme se ele foi excluído.

## <a name="coming-up-next"></a>Próximo

O seguinte tutorial mostra como fornecer todas as páginas em seu site uma aparência e layout comuns.

## <a name="complete-listing-for-movie-page-updated-with-delete-links"></a>Listagem completa para a página de filme (atualizada com Links de exclusão)

[!code-cshtml[Main](deleting-data/samples/sample8.cshtml)]

## <a name="complete-listing-for-deletemovie-page"></a>Lista completa de página DeleteMovie

[!code-cshtml[Main](deleting-data/samples/sample9.cshtml)]

## <a name="additional-resources"></a>Recursos adicionais

- [Introdução à programação da Web do ASP.NET usando a sintaxe do Razor](https://go.microsoft.com/fwlink/?LinkID=202890)
- [A instrução DELETE do SQL](http://www.w3schools.com/sql/sql_delete.asp) no site W3Schools

>[!div class="step-by-step"]
[Anterior](updating-data.md)
[Próximo](layouts.md)
