---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
title: Adicionando um novo campo para o modelo de filme e a tabela | Microsoft Docs
author: Rick-Anderson
description: 'Observação: Uma versão atualizada deste tutorial está disponível aqui que usa o ASP.NET MVC 5 e Visual Studio 2013. Ele é mais seguro e muito mais simples a seguir e demonstração...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 9ef2c4f1-a305-4e0a-9fb8-bfbd9ef331d9
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table
msc.type: authoredcontent
ms.openlocfilehash: 25a29e783f02e66e1713d8120eb526e1a02961a3
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366470"
---
<a name="adding-a-new-field-to-the-movie-model-and-table"></a>Adicionando um novo campo ao modelo de filme e à tabela
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples a seguir e apresenta mais recursos.


Nesta seção, você usará migrações do Entity Framework Code First para migrar algumas alterações para as classes de modelo para que a alteração será aplicada ao banco de dados.

Por padrão, quando você usa o Entity Framework Code First para criar automaticamente um banco de dados, como você fez neste tutorial, Code First adiciona uma tabela no banco de dados para ajudar a controlar se o esquema do banco de dados está em sincronizado com as classes de modelo, que ele foi gerado. Se não estiverem sincronizados, o Entity Framework gera um erro. Isso torna mais fácil rastrear problemas em tempo de desenvolvimento que caso contrário, apenas talvez (por erros obscuros) em tempo de execução.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuração de migrações do Code First para alterações do modelo

Se você estiver usando o Visual Studio 2012, clique duas vezes o *Movies.mdf* arquivo no Gerenciador de soluções para abrir a ferramenta de banco de dados. Visual Studio Express para Web mostrará o Gerenciador de banco de dados, o Visual Studio 2012 mostrará o Gerenciador de servidores. Se você estiver usando o Visual Studio 2010, use o Pesquisador de objetos do SQL Server.

Na ferramenta de banco de dados (Gerenciador de banco de dados, Gerenciador de servidores ou Pesquisador de objetos do SQL Server), clique com botão direito `MovieDBContext` e selecione **excluir** para descartar o banco de dados de filmes.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image1.png)

Navegue de volta para o Gerenciador de soluções. Clique com botão direito do *Movies.mdf* do arquivo e selecione **excluir** para remover o banco de dados de filmes.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image2.png)

Compile o aplicativo para garantir que não existem erros.

Dos **ferramentas** menu, clique em **Gerenciador de pacotes de biblioteca** e, em seguida, **Package Manager Console**.

![Adicionar pacote Man](adding-a-new-field-to-the-movie-model-and-table/_static/image3.png)

No **Package Manager Console** janela no `PM>` prompt insira "Enable-Migrations - ContextTypeName MvcMovie.Models.MovieDBContext".

![](adding-a-new-field-to-the-movie-model-and-table/_static/image4.png)

O **Enable-Migrations** comando (mostrado acima) cria um *Configuration.cs* arquivo em uma nova *migrações* pasta.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image5.png)

O Visual Studio abre o *Configuration.cs* arquivo. Substitua os `Seed` método na *Configuration.cs* arquivo pelo código a seguir:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample1.cs)]

Clique com botão direito na linha curvada vermelha sob `Movie` e selecione **resolver** , em seguida, **usando** **mvcmovie. Models;**

![](adding-a-new-field-to-the-movie-model-and-table/_static/image6.png)

Isso adiciona a seguinte instrução using:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First Migrations chamadas a `Seed` método após cada migração (ou seja, chamando **Atualizar banco de dados** no Console do Gerenciador de pacotes), e esse método atualiza as linhas que já foram inseridas ou insere-os se eles ainda não existem.


**Pressione CTRL-SHIFT-B para compilar o projeto.** (As etapas a seguir falhará se seu não crie neste momento.)

A próxima etapa é criar um `DbMigration` classe para a migração inicial. Essa migração para cria um novo banco de dados, é por isso que você excluiu o *movie.mdf* arquivo em uma etapa anterior.

No **Package Manager Console** janela, digite o comando "add-migration Initial" para criar a migração inicial. O nome "Inicial" é arbitrário e é usado para nomear o arquivo de migração criado.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image7.png)

Migrações do Code First cria outro arquivo de classe na *migrações* pasta (com o nome *{carimbo de data}\_Initial.cs* ), e essa classe contém código que cria o esquema de banco de dados. O nome do arquivo de migração é previamente corrigido com um carimbo de hora para ajudar com a ordenação. Examine os *{carimbo de data}\_Initial.cs* arquivo, ele contém as instruções para criar a tabela de filmes para o banco de dados do filme. Quando você atualiza o banco de dados nas instruções a seguir, isso *{carimbo de data}\_Initial.cs* arquivo será executado e criar o esquema de banco de dados. Em seguida, a **semente** método será executado para preencher o banco de dados com dados de teste.

No **Package Manager Console**, insira o comando "update-database" para criar o banco de dados e executar o **semente** método.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image8.png)

Se você receber um erro que indica uma tabela já existe e não pode ser criada, provavelmente é porque você executou o aplicativo depois que você excluiu o banco de dados e antes da execução `update-database`. Nesse caso, exclua o *Movies.mdf* novamente e repita a `update-database` comando. Se você ainda receber um erro, exclua a pasta migrações e o conteúdo, comece com as instruções na parte superior desta página (que é exclusão a *Movies.mdf* de arquivo e em seguida, vá para Enable-Migrations).

Execute o aplicativo e navegue até a */Movies* URL. Os dados de semente são exibidos.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image9.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adicionando uma propriedade de classificação ao modelo de filme

Comece adicionando um novo `Rating` propriedade à existente `Movie` classe. Abra o *Models\Movie.cs* arquivo e adicione o `Rating` propriedade parecida com esta:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample3.cs)]

Completo `Movie` classe agora parece o código a seguir:

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample4.cs?highlight=8)]

Compile o aplicativo usando o **construir** &gt; **Build filme** menu de comando ou pressionando CTRL-SHIFT-B.

Agora que você atualizou o `Model` classe, você também precisa atualizar o *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* exibir modelos para exibir o novo `Rating`propriedade na exibição do navegador.

Abra o<em>\Views\Movies\Index.cshtml</em> arquivo e adicione um `<th>Rating</th>` título de coluna logo após o <strong>preço</strong> coluna. Em seguida, adicione uma `<td>` coluna, próximo ao final do modelo para renderizar o `@item.Rating` valor. Abaixo está o que a atualização <em>index. cshtml</em> modelo de exibição se parece com:

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample5.cshtml?highlight=26-28,46-48)]

Em seguida, abra o *\Views\Movies\Create.cshtml* arquivo e adicione a seguinte marcação próximo ao final do formulário. Isso renderiza uma caixa de texto para que você possa especificar uma classificação quando um novo filme é criado.

[!code-cshtml[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample6.cshtml)]

Agora você atualizou o código do aplicativo para dar suporte a novos `Rating` propriedade.

Agora, execute o aplicativo e navegue até a */Movies* URL. Quando você fizer isso, no entanto, você verá um dos seguintes erros:

![](adding-a-new-field-to-the-movie-model-and-table/_static/image10.png)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image11.png)

Você está vendo esse erro porque atualizada `Movie` classe de modelo no aplicativo agora é diferente do esquema do `Movie` tabela do banco de dados existente. (Não há nenhuma coluna `Rating` na tabela de banco de dados.)


Existem algumas abordagens para resolver o erro:

1. Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo. Essa abordagem é muito conveniente ao fazer o desenvolvimento ativo em um banco de dados de teste; Ele permite que você desenvolva rapidamente o esquema de modelo e o banco de dados juntos. A desvantagem, no entanto, é que você perde os dados existentes no banco de dados — para que você *não* para usar essa abordagem em um banco de dados de produção! Geralmente é uma maneira produtiva de desenvolver um aplicativo usar um inicializador para propagar automaticamente um banco de dados com dados de teste. Para obter mais informações sobre inicializadores de banco de dados do Entity Framework, consulte de Tom Dykstra [tutorial do ASP.NET MVC/Entity Framework](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo. A vantagem dessa abordagem é que você mantém os dados. Faça essa alteração manualmente ou criando um script de alteração de banco de dados.
3. Use as Migrações do Code First para atualizar o esquema de banco de dados.


Para este tutorial, usaremos as Migrações do Code First.

Atualize o método de propagação para que ele forneça um valor para a nova coluna. Abra o arquivo Migrations\Configuration.cs e adicionar um campo de classificação para cada objeto de filme.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample7.cs?highlight=6)]

Compile a solução e, em seguida, abra o **Package Manager Console** janela e digite o seguinte comando:

`add-migration AddRatingMig`

O `add-migration` comando informa a estrutura de migração para examinar o modelo de filme atual com o esquema de banco de dados do filme atual e crie o código necessário para migrar o banco de dados para o novo modelo. O AddRatingMig é arbitrário e é usado para nomear o arquivo de migração. É útil usar um nome significativo para a etapa de migração.

Quando esse comando for concluído, o Visual Studio abre o arquivo de classe que define o novo `DbMIgration` classe, derivada e, no `Up` método, você pode ver o código que cria a nova coluna.

[!code-csharp[Main](adding-a-new-field-to-the-movie-model-and-table/samples/sample8.cs)]

Compile a solução e, em seguida, insira o comando "update-database" a **Package Manager Console** janela.

A imagem a seguir mostra a saída a **Package Manager Console** janela (o carimbo de data acrescentando AddRatingMig será diferente.)

![](adding-a-new-field-to-the-movie-model-and-table/_static/image12.png)

Execute novamente o aplicativo e navegue até a URL /Movies. Você pode ver o novo campo de classificação.

![](adding-a-new-field-to-the-movie-model-and-table/_static/image13.png)

Clique o **criar novo** link para adicionar um novo filme. Observe que você pode adicionar uma classificação.

![7_CreateRioII](adding-a-new-field-to-the-movie-model-and-table/_static/image14.png)

Clique em **Criar**. O novo filme, incluindo a classificação é exibido nos listagem de filmes:

![7_ourNewMovie_SM](adding-a-new-field-to-the-movie-model-and-table/_static/image15.png)

Você também deve adicionar o `Rating` modelos de exibição de campo para a edição, detalhes e SearchIndex.

Você poderia digitar o comando "update-database" a **Package Manager Console** janela novamente e nenhuma alteração seria feita, pois o esquema coincide com o modelo.

Nesta seção, você viu como você pode modificar objetos de modelo e manter o banco de dados em sincronia com as alterações. Você também aprendeu uma maneira de preencher um banco de dados recém-criado com dados de exemplo para que você pode experimentar os cenários. Em seguida, vamos dar uma olhada em como você pode adicionar lógica de validação mais avançada para as classes de modelo e habilitar algumas regras de negócio a serem impostos.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Próximo](adding-validation-to-the-model.md)
