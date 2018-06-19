---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Adicionar um novo campo para o modelo de filme e a tabela | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples de seguir e demonstração...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: d8a42e9acdce687ab6e9742071dd2949f244622f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872784"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Adicionar um novo campo para o modelo de filme e tabela
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É muito mais simples a seguir, mais segura e demonstra mais recursos.


Nesta seção, você usará migrações do Entity Framework Code First para migrar algumas alterações para as classes de modelo para que a alteração será aplicada ao banco de dados.

Por padrão, ao usar o Entity Framework Code First para criar automaticamente um banco de dados, como você fez anteriormente neste tutorial, Code First adiciona uma tabela no banco de dados para ajudar a controlar se o esquema do banco de dados está em sincronizado com classes de modelo do que qual ela foi gerada. Se não estiverem sincronizados, o Entity Framework gerará um erro. Isso torna mais fácil rastrear os problemas em tempo de desenvolvimento que você pode caso contrário, apenas encontrar (por erros obscuros) em tempo de execução.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuração de migrações do Code First para alterações de modelo

Se você estiver usando o Visual Studio 2012, clique duas vezes o *Movies.mdf* arquivo no Gerenciador de soluções para abrir a ferramenta de banco de dados. Visual Studio Express para Web mostrará Pesquisador de objetos de banco de dados, o Visual Studio 2012 mostrará o Gerenciador de servidores. Se você estiver usando o Visual Studio 2010, use o Pesquisador de objetos do SQL Server.

Na ferramenta de banco de dados (Pesquisador de objetos de banco de dados, Gerenciador de servidores ou Pesquisador de objetos do SQL Server), clique com botão direito em `MovieDBContext` e selecione **excluir** para descartar o banco de dados de filmes.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Navegue de volta para o Gerenciador de soluções. Clique com o botão direito no *Movies.mdf* de arquivo e selecione **excluir** para remover o banco de dados de filmes.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

O aplicativo para verificar se que não existem erros de compilação.

Do **ferramentas** menu, clique em **Gerenciador de biblioteca de pacote** e **Package Manager Console**.

![Adicionar pacote Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

No **Package Manager Console** janela o `PM>` prompt insira "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

O **Enable-Migrations** comando (mostrado acima) cria um *Configuration.cs* arquivo em uma nova *migrações* pasta.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

O Visual Studio abrirá o *Configuration.cs* arquivo. Substitua o `Seed` método o *Configuration.cs* arquivo com o código a seguir:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Clique com o botão direito na linha curvada vermelha sob `Movie` e selecione **resolver** , em seguida, **usando** **MvcMovie.Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Isso adiciona a seguinte instrução using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First Migrations chamadas a `Seed` método após a migração de todos (ou seja, chamando **Atualizar banco de dados** no Console do Gerenciador de pacotes), e este método atualiza linhas que já foram inseridas ou insere-los se eles não existem ainda.


**Pressione CTRL-SHIFT-B para compilar o projeto.** (As etapas a seguir falhará se o não criar neste momento.)

A próxima etapa é criar um `DbMigration` classe para a migração inicial. A migração para cria um novo banco de dados, que é por isso que você excluiu o *movie.mdf* arquivo em uma etapa anterior.

No **Package Manager Console** janela, digite o comando "Adicionar-migração inicial" para criar a migração inicial. O nome "Inicial" é arbitrário e é usado para nomear o arquivo de migração criado.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migrações do Code First cria outro arquivo de classe no *migrações* pasta (com o nome *{DateStamp}\_Initial.cs* ), e essa classe contém o código que cria o esquema de banco de dados. O nome do arquivo de migração previamente é fixo com um carimbo de hora para ajudar com a ordenação. Examine o *{DateStamp}\_Initial.cs* arquivo, ele contém as instruções para criar a tabela de filmes para o banco de dados do filme. Quando você atualiza o banco de dados nas instruções a seguir, isso *{DateStamp}\_Initial.cs* arquivo será executado e criar o esquema de banco de dados. Em seguida, o **semente** método serão executadas para popular o banco de dados com dados de teste.

No **Package Manager Console**, digite o comando "update-database" para criar o banco de dados e executar o **semente** método.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Se você receber um erro que indica uma tabela já existe e não pode ser criada, é provável que o aplicativo é executado depois que você excluir o banco de dados e antes da execução `update-database`. Nesse caso, exclua o *Movies.mdf* novamente e repita a `update-database` comando. Se você ainda receber um erro, exclua a pasta migrations e o conteúdo e começar com as instruções na parte superior desta página (que é excluir o *Movies.mdf* de arquivo e, depois, para habilitar migrações).

Execute o aplicativo e navegue até o */Movies* URL. Os dados de semente são exibidos.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adicionando uma propriedade de classificação ao modelo de filme

Comece adicionando um novo `Rating` propriedade existente `Movie` classe. Abra o *Models\Movie.cs* e adicione o `Rating` propriedade como esta:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Completo `Movie` classe agora parece com o código a seguir:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Compile o aplicativo usando o **criar** &gt; **criar filme** menu de comando ou pressionando CTRL-SHIFT-B.

Agora que você atualizou o `Model` classe, você também precisa atualizar o *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* exibir modelos para exibir o novo `Rating`propriedade na exibição do navegador.

Abra o<em>\Views\Movies\Index.cshtml</em> e adicione um `<th>Rating</th>` cabeçalho da coluna logo após o <strong>preço</strong> coluna. Em seguida, adicione um `<td>` coluna próximo ao final do modelo para renderizar o `@item.Rating` valor. Abaixo está o que a atualização <em>cshtml</em> aparência do modelo de exibição:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Em seguida, abra o *\Views\Movies\Create.cshtml* de arquivo e adicione a seguinte marcação próximo ao final do formulário. Isso apresenta uma caixa de texto para que você pode especificar uma classificação quando um filme novo é criado.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Agora que você atualizou o código do aplicativo para dar suporte aos novos `Rating` propriedade.

Agora execute o aplicativo e navegue até o */Movies* URL. Quando você fizer isso, no entanto, você verá um dos seguintes erros:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Você está vendo este erro, porque a atualização `Movie` classe de modelo no aplicativo agora é diferente do esquema do `Movie` tabela do banco de dados existente. (Não há nenhuma coluna `Rating` na tabela de banco de dados.)


Existem algumas abordagens para resolver o erro:

1. Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo. Essa abordagem é muito conveniente ao fazer o desenvolvimento ativo em um banco de dados de teste; Ele permite que você rapidamente desenvolver o esquema de banco de dados e modelo juntos. No entanto, a desvantagem é que você perca os dados existentes no banco de dados — para que você *não* para usar essa abordagem em um banco de dados de produção! Usando um inicializador para um banco de dados com dados de teste de propagação automaticamente geralmente é uma maneira produtiva ao desenvolver um aplicativo. Para obter mais informações sobre os inicializadores de banco de dados do Entity Framework, consulte de Tom Dykstra [tutorial do ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo. A vantagem dessa abordagem é que você mantém os dados. Faça essa alteração manualmente ou criando um script de alteração de banco de dados.
3. Use as Migrações do Code First para atualizar o esquema de banco de dados.


Para este tutorial, usaremos as Migrações do Code First.

Atualize o método de propagação para que ele forneça um valor para a nova coluna. Abra o arquivo Migrations\Configuration.cs e adicionar um campo de classificação para cada objeto de filme.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Compile a solução e, em seguida, abra o **Package Manager Console** janela e digite o seguinte comando:

`add-migration AddRatingMig`

O `add-migration` comando informa o framework de migração para examinar o modelo de filme atual com o esquema de banco de dados do filme atual e criar o código necessário para migrar o banco de dados para o novo modelo. O AddRatingMig é arbitrário e é usado para nomear o arquivo de migração. É útil usar um nome significativo para a etapa da migração.

Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define o novo `DbMIgration` derivado da classe e no `Up` método, você pode ver o código que cria a nova coluna.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Compile a solução e, em seguida, digite o comando "update-database" o **Package Manager Console** janela.

A imagem a seguir mostra a saída de **Package Manager Console** janela (o carimbo de data acrescentando AddRatingMig será diferente.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Execute novamente o aplicativo e navegue até a URL de /Movies. Você pode ver o novo campo de classificação.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Clique o **criar novo** link para adicionar um novo filme. Observe que você pode adicionar uma classificação.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Clique em **Criar**. Novo filme, incluindo classificação, agora é exibido nos filmes listando:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Você também deve adicionar o `Rating` campo para a edição, detalhes e SearchIndex modelos de exibição.

Você poderia inserir o comando "update-database" o **Package Manager Console** janela novamente e nenhuma alteração teriam sido feitas, porque o esquema corresponde ao modelo.

Nesta seção, você viu como você pode modificar objetos de modelo e manter o banco de dados em sincronia com as alterações. Você também aprendeu uma maneira para popular um banco de dados recém-criado com dados de exemplo para que você pode experimentar os cenários. Em seguida, vamos dar uma olhada em como você pode adicionar lógica de validação mais rica para as classes de modelo e habilitar algumas regras de negócio a serem aplicadas.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Próximo](adding-validation-to-the-model.md)
