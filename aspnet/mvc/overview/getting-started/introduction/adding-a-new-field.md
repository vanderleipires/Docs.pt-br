---
uid: mvc/overview/getting-started/introduction/adding-a-new-field
title: Adicionar um novo campo | Microsoft Docs
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 4085de68-d243-4378-8a64-86236ea8d2da
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 453fbf68aa2f3a1d9ea708355c06c53d4f1eabd0
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/02/2018
---
<a name="adding-a-new-field"></a>Adicionando um Novo Campo
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

Nesta seção, você usará migrações do Entity Framework Code First para migrar algumas alterações para as classes de modelo para que a alteração será aplicada ao banco de dados.

Por padrão, ao usar o Entity Framework Code First para criar automaticamente um banco de dados, como você fez anteriormente neste tutorial, Code First adiciona uma tabela no banco de dados para ajudar a controlar se o esquema do banco de dados está em sincronizado com classes de modelo do que qual ela foi gerada. Se não estiverem sincronizados, o Entity Framework gerará um erro. Isso torna mais fácil rastrear os problemas em tempo de desenvolvimento que você pode caso contrário, apenas encontrar (por erros obscuros) em tempo de execução.

## <a name="setting-up-code-first-migrations-for-model-changes"></a>Configuração de migrações do Code First para alterações de modelo

Navegue até o Gerenciador de soluções. Clique com o botão direito no *Movies.mdf* de arquivo e selecione **excluir** para remover o banco de dados de filmes. Se você não vir o *Movies.mdf* de arquivo, clique no **Mostrar todos os arquivos** ícone mostrado abaixo na estrutura de tópicos vermelha.

![](adding-a-new-field/_static/image1.png)

O aplicativo para verificar se que não existem erros de compilação.

Do **ferramentas** menu, clique em **NuGet Package Manager** e **Package Manager Console**.

![Adicionar pacote Man](adding-a-new-field/_static/image2.png)

No **Package Manager Console** janela o `PM>` insira prompt

Enable-Migrations -ContextTypeName MvcMovie.Models.MovieDBContext

![](adding-a-new-field/_static/image3.png)

O **Enable-Migrations** comando (mostrado acima) cria um *Configuration.cs* arquivo em uma nova *migrações* pasta.

![](adding-a-new-field/_static/image4.png)

O Visual Studio abrirá o *Configuration.cs* arquivo. Substitua o `Seed` método o *Configuration.cs* arquivo com o código a seguir:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Passe o mouse sobre a linha curvada vermelha sob `Movie` e clique em `Show Potential Fixes` e, em seguida, clique em **usando** **MvcMovie.Models;**

![](adding-a-new-field/_static/image5.png)

Isso adiciona a seguinte instrução using:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

> [!NOTE] 
> 
> Code First Migrations chamadas a `Seed` método após a migração de todos (ou seja, chamando **Atualizar banco de dados** no Console do Gerenciador de pacotes), e este método atualiza linhas que já foram inseridas ou insere-los se eles não existem ainda.
> 
> O [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método no código a seguir executa uma operação "upsert":
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample3.cs)]
> 
> Porque o [semente](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) método é executado com cada migração, você não pode inserir apenas os dados, porque as linhas que você está tentando adicionar já estará lá após a migração primeiro que cria o banco de dados. O "[upsert](http://en.wikipedia.org/wiki/Upsert)" operação evita erros que acontecem se você tentar inserir uma linha que já existe, mas ela substitui quaisquer alterações nos dados que você fez ao testar o aplicativo. Com dados de teste em algumas tabelas talvez você não queira que isso aconteça: em alguns casos quando você altera dados ao testar deseja suas alterações para permanecer após as atualizações do banco de dados. Nesse caso você deseja fazer uma operação de inserção condicional: inserir uma linha apenas se ele ainda não existir.   
>   
> O primeiro parâmetro passado para o [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método Especifica a propriedade a ser usado para verificar se uma linha já existe. Para os dados do filme de teste que você fornecer, o `Title` propriedade pode ser usada para essa finalidade, pois cada título na lista é exclusivo:
> 
> [!code-csharp[Main](adding-a-new-field/samples/sample4.cs)]
> 
> Esse código supõe que os títulos sejam exclusivos. Se você adicionar manualmente um título duplicado, você receberá a seguinte exceção na próxima vez que você executar uma migração.   
>   
>  *A sequência contém mais de um elemento*  
>   
> Para obter mais informações sobre o [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) método, consulte [tome cuidado com EF 4.3 AddOrUpdate método](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/).


**Pressione CTRL-SHIFT-B para compilar o projeto.** (As etapas a seguir falhará se você não criar neste momento.)

A próxima etapa é criar um `DbMigration` classe para a migração inicial. Essa migração cria um novo banco de dados, que é por isso que você excluiu o *movie.mdf* arquivo em uma etapa anterior.

No **Package Manager Console** janela, digite o comando `add-migration Initial` para criar a migração inicial. O nome "Inicial" é arbitrário e é usado para nomear o arquivo de migração criado.

![](adding-a-new-field/_static/image6.png)

Migrações do Code First cria outro arquivo de classe no *migrações* pasta (com o nome *{DateStamp}\_Initial.cs* ), e essa classe contém o código que cria o esquema de banco de dados. O nome do arquivo de migração previamente é fixo com um carimbo de hora para ajudar com a ordenação. Examine o *{DateStamp}\_Initial.cs* arquivo, ele contém as instruções para criar o `Movies` tabela para o banco de dados do filme. Quando você atualiza o banco de dados nas instruções a seguir, isso *{DateStamp}\_Initial.cs* arquivo será executado e criar o esquema de banco de dados. Em seguida, o **semente** método serão executadas para popular o banco de dados com dados de teste.

No **Package Manager Console**, digite o comando `update-database` para criar o banco de dados e executar o `Seed` método.

![](adding-a-new-field/_static/image7.png)

Se você receber um erro que indica uma tabela já existe e não pode ser criada, é provável que o aplicativo é executado depois que você excluir o banco de dados e antes da execução `update-database`. Nesse caso, exclua o *Movies.mdf* novamente e repita a `update-database` comando. Se você ainda receber um erro, exclua a pasta migrations e o conteúdo e começar com as instruções na parte superior desta página (que é excluir o *Movies.mdf* de arquivo e, depois, para habilitar migrações). Se você ainda receber um erro, abra o Pesquisador de objetos do SQL Server e remover o banco de dados da lista.

Execute o aplicativo e navegue até o */Movies* URL. Os dados de semente são exibidos.

![](adding-a-new-field/_static/image8.png)

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adicionando uma propriedade de classificação ao modelo de filme

Comece adicionando um novo `Rating` propriedade existente `Movie` classe. Abra o *Models\Movie.cs* e adicione o `Rating` propriedade como esta:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

Completo `Movie` classe agora parece com o código a seguir:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs?highlight=12)]

Crie o aplicativo (Ctrl + Shift + B).

Como adicionar um novo campo para o `Movie` classe, você também precisa atualizar a associação *lista branca* para essa nova propriedade será incluída. Atualização de `bind` atributo `Create` e `Edit` métodos de ação para incluir o `Rating` propriedade:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs?highlight=1)]

Você também precisa atualizar os modelos de exibição para exibir, criar e editar a nova propriedade `Rating` na exibição do navegador.

Abra o *\Views\Movies\Index.cshtml* e adicione um `<th>Rating</th>` cabeçalho da coluna logo após o **preço** coluna. Em seguida, adicione um `<td>` coluna próximo ao final do modelo para renderizar o `@item.Rating` valor. Abaixo está o que a atualização *cshtml* aparência do modelo de exibição:

[!code-cshtml[Main](adding-a-new-field/samples/sample8.cshtml?highlight=31-33,52-54)]

Em seguida, abra o *\Views\Movies\Create.cshtml* e adicione o `Rating` campo com a seguinte marcação destacadas. Isso apresenta uma caixa de texto para que você pode especificar uma classificação quando um filme novo é criado.

[!code-cshtml[Main](adding-a-new-field/samples/sample9.cshtml?highlight=9-15)]

Agora que você atualizou o código do aplicativo para dar suporte aos novos `Rating` propriedade.

Execute o aplicativo e navegue até o */Movies* URL. Quando você fizer isso, no entanto, você verá um dos seguintes erros:

![](adding-a-new-field/_static/image9.png)  
  
O modelo fazendo o contexto de 'MovieDBContext' foi alterado desde que o banco de dados foi criado. Considere o uso de migrações do Code First para atualizar o banco de dados (https://go.microsoft.com/fwlink/?LinkId=238269).

![](adding-a-new-field/_static/image10.png)

Você está vendo este erro, porque a atualização `Movie` classe de modelo no aplicativo agora é diferente do esquema do `Movie` tabela do banco de dados existente. (Não há nenhuma coluna `Rating` na tabela de banco de dados.)


Existem algumas abordagens para resolver o erro:

1. Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo. Essa abordagem é muito conveniente no início do ciclo de desenvolvimento, quando você está fazendo o desenvolvimento ativo em um banco de dados de teste; ela permite que você desenvolva rapidamente o modelo e o esquema de banco de dados juntos. No entanto, a desvantagem é que você perca os dados existentes no banco de dados — para que você *não* para usar essa abordagem em um banco de dados de produção! Muitas vezes, o uso de um inicializador para propagar um banco de dados com os dados de teste automaticamente é uma maneira produtiva de desenvolver um aplicativo. Para obter mais informações sobre os inicializadores de banco de dados do Entity Framework, consulte [tutorial do ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
2. Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo. A vantagem dessa abordagem é que você mantém os dados. Faça essa alteração manualmente ou criando um script de alteração de banco de dados.
3. Use as Migrações do Code First para atualizar o esquema de banco de dados.


Para este tutorial, usaremos as Migrações do Code First.

Atualize o método de propagação para que ele forneça um valor para a nova coluna. Abra o arquivo Migrations\Configuration.cs e adicionar um campo de classificação para cada objeto de filme.

[!code-csharp[Main](adding-a-new-field/samples/sample10.cs?highlight=6)]

Compile a solução e, em seguida, abra o **Package Manager Console** janela e digite o seguinte comando:

`add-migration Rating`

O `add-migration` comando informa o framework de migração para examinar o modelo de filme atual com o esquema de banco de dados do filme atual e criar o código necessário para migrar o banco de dados para o novo modelo. O nome *classificação* é arbitrário e é usado para nomear o arquivo de migração. É útil usar um nome significativo para a etapa da migração.

Quando esse comando for concluído, o Visual Studio abrirá o arquivo de classe que define o novo `DbMigration` derivado da classe e no `Up` método, você pode ver o código que cria a nova coluna.

[!code-csharp[Main](adding-a-new-field/samples/sample11.cs)]

Compile a solução e, em seguida, insira o `update-database` do **Package Manager Console** janela.

A imagem a seguir mostra a saída a **Package Manager Console** janela (o carimbo de data acrescentando *classificação* será diferente.)

![](adding-a-new-field/_static/image11.png)

Execute novamente o aplicativo e navegue até a URL de /Movies. Você pode ver o novo campo de classificação.

![](adding-a-new-field/_static/image12.png)

Clique o **criar novo** link para adicionar um novo filme. Observe que você pode adicionar uma classificação.

![7_CreateRioII](adding-a-new-field/_static/image13.png)

Clique em **Criar**. Novo filme, incluindo classificação, agora é exibido nos filmes listando:

![7_ourNewMovie_SM](adding-a-new-field/_static/image14.png)

Agora que o projeto estiver usando migrações, você não será necessário remover o banco de dados quando você adiciona um novo campo ou caso contrário, atualizar o esquema. Na próxima seção, vamos fazer mais alterações de esquema e utilize as migrações para atualizar o banco de dados.

Você também deve adicionar o `Rating` campo para os modelos de exibição de edição, detalhes e Delete.

Você poderia inserir o comando "update-database" o **Package Manager Console** janela novamente e nenhum código de migração serão executado porque o esquema corresponde ao modelo. No entanto, em execução "update-database" executará o `Seed` método novamente, e se você alterou os dados de semente, as alterações serão perdidas porque o `Seed` dados upserts do método. Você pode ler mais sobre o `Seed` método de Tom Dykstra popular [tutorial do ASP.NET MVC/Entity Framework](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

Nesta seção, você viu como você pode modificar objetos de modelo e manter o banco de dados em sincronia com as alterações. Você também aprendeu uma maneira para popular um banco de dados recém-criado com dados de exemplo para que você pode experimentar os cenários. Isso foi apenas uma rápida introdução às Code First, consulte [criando um modelo de dados do Entity Framework para um aplicativo ASP.NET MVC](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) para obter um tutorial mais completo sobre o assunto. Em seguida, vamos dar uma olhada em como você pode adicionar lógica de validação mais rica para as classes de modelo e habilitar algumas regras de negócio a serem aplicadas.

>[!div class="step-by-step"]
[Anterior](adding-search.md)
[Próximo](adding-validation.md)
