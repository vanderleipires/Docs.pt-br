---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Adicionar um novo campo para o modelo de filme e a tabela de banco de dados (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5927b7d977e375881fe618b4b844cbd708023ba1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>Adicionar um novo campo para o modelo de filme e a tabela de banco de dados (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial ensina as Noções básicas de criação de um aplicativo Web do ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução + ferramentas de suportam)
> 
> Se você estiver usando o Visual Studio 2010 em vez do Visual Web Developer 2010, instale os pré-requisitos clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com VB.NET código-fonte está disponível para acompanhar este tópico. [Baixe a versão VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir c#, alterne para o [versão c#](../cs/adding-a-new-field.md) deste tutorial.


Nesta seção, você fazer algumas alterações para as classes de modelo e saiba como você pode atualizar o esquema de banco de dados de acordo com as alterações do modelo.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adicionando uma propriedade de classificação ao modelo de filme

Comece adicionando um novo `Rating` propriedade existente `Movie` classe. Abra o *Movie.cs* e adicione o `Rating` propriedade como esta:

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

Completo `Movie` classe agora parece com o código a seguir:

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

Recompilar o aplicativo usando o **depurar** &gt; **criar filme** comando de menu.

Agora que você atualizou o `Model` classe, você também precisa atualizar o *\Views\Movies\Index.vbhtml* e *\Views\Movies\Create.vbhtml* exibir modelos para oferecer suporte a novos `Rating`propriedade.

Abra o<em>\Views\Movies\Index.vbhtml</em> e adicione um `<th>Rating</th>` cabeçalho da coluna logo após o <strong>preço</strong> coluna. Em seguida, adicione um `<td>` coluna próximo ao final do modelo para renderizar o `@item.Rating` valor. Abaixo está o que a atualização <em>Index.vbhtml</em> aparência do modelo de exibição:

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

Em seguida, abra o *\Views\Movies\Create.vbhtml* de arquivo e adicione a seguinte marcação próximo ao final do formulário. Isso apresenta uma caixa de texto para que você pode especificar uma classificação quando um filme novo é criado.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Gerenciamento de modelo e as diferenças de esquema de banco de dados

Agora que você atualizou o código do aplicativo para dar suporte aos novos `Rating` propriedade.

Agora execute o aplicativo e navegue até o */Movies* URL. Quando você fizer isso, no entanto, você verá o seguinte erro:

![](adding-a-new-field/_static/image1.png)

Você está vendo este erro, porque a atualização `Movie` classe de modelo no aplicativo agora é diferente do esquema do `Movie` tabela do banco de dados existente. (Não há nenhuma coluna `Rating` na tabela de banco de dados.)

Por padrão, ao usar o Entity Framework Code First para criar automaticamente um banco de dados, como você fez anteriormente neste tutorial, Code First adiciona uma tabela no banco de dados para ajudar a controlar se o esquema do banco de dados está em sincronizado com classes de modelo do que qual ela foi gerada. Se não estiverem sincronizados, o Entity Framework gerará um erro. Isso torna mais fácil rastrear os problemas em tempo de desenvolvimento que você pode caso contrário, apenas encontrar (por erros obscuros) em tempo de execução. O recurso de verificação de sincronização é o que faz com que a mensagem de erro a ser exibido que você acabou de ver.

Há duas abordagens para resolver o erro:

1. Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo. Essa abordagem é muito conveniente ao fazer o desenvolvimento ativo em um banco de dados de teste, porque ele permite que você rapidamente desenvolver o esquema de banco de dados e modelo juntos. No entanto, a desvantagem é que você perca os dados existentes no banco de dados — para que você *não* para usar essa abordagem em um banco de dados de produção!
2. Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo. A vantagem dessa abordagem é que você mantém os dados. Faça essa alteração manualmente ou criando um script de alteração de banco de dados.

Para este tutorial, vamos usar a primeira abordagem, você terá o Entity Framework Code First automaticamente recriar o banco de dados sempre que o modelo é alterado.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Recriando o banco de dados do modelo é alterado automaticamente

Vamos atualizar o aplicativo para que o Code First automaticamente descarta e recria o banco de dados sempre que você alterar o modelo para o aplicativo.

> [!NOTE] 
> 
> **Aviso** você deve habilitar essa abordagem de automaticamente descartar e recriar o banco de dados somente quando você estiver usando um banco de dados de desenvolvimento ou teste, e *nunca* em um banco de dados de produção que contém os dados reais. Usá-lo em um servidor de produção pode causar a perda de dados.


Em **Solution Explorer**, clique com botão direito do *modelos* pasta, selecione **adicionar**e, em seguida, selecione **classe**.

![](adding-a-new-field/_static/image2.png)

Nomeie a classe &quot;MovieInitializer&quot;. Atualização de `MovieInitializer` classe para conter o código a seguir:

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

O `MovieInitializer` classe especifica que o banco de dados usado pelo modelo deve ser descartado e automaticamente recriado se as classes de modelo mudar. O código inclui um `Seed` método para especificar que alguns dados padrão para adicionar automaticamente para o banco de dados de qualquer uma vez em que foi criado (ou recriado). Isso fornece uma maneira útil para preencher o banco de dados com alguns dados de exemplo, sem exigir que você popular manualmente cada vez que você alterar um modelo.

Agora que você definiu o `MovieInitializer` classe, você desejará conectá-lo para que cada vez que o aplicativo é executado, ele verifica se as classes de modelo são diferentes do esquema no banco de dados. Se estiverem, você pode executar o inicializador para recriar o banco de dados para coincidir com o modelo e, em seguida, preencher o banco de dados com os dados de exemplo.

Abra o *global. asax* arquivo que está na raiz do `MvcMovies` projeto:

O *global. asax* arquivo contém a classe que define o aplicativo inteiro para o projeto e contém um `Application_Start` manipulador de eventos que é executado quando o aplicativo é iniciado pela primeira vez.

Localizar o `Application_Start` método e adicionar uma chamada para `Database.SetInitializer` no início do método, conforme mostrado abaixo:

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

O `Database.SetInitializer` instrução que você acabou de adicionar indica que o banco de dados usado pela `MovieDBContext` instância deve ser automaticamente excluída e criada novamente se o esquema e o banco de dados não correspondem. E como você viu, ele preencherá o banco de dados com os dados de exemplo que são especificados no também o `MovieInitializer` classe.

Fechar o *global. asax* arquivo.

Execute novamente o aplicativo e navegue até o */Movies* URL. Quando o aplicativo for iniciado, ele detecta que a estrutura do modelo não coincide com o esquema de banco de dados. Ele automaticamente recria o banco de dados para coincidir com a nova estrutura de modelo e preenche o banco de dados com os filmes de exemplo:

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

Clique o **criar novo** link para adicionar um novo filme. Observe que você pode adicionar uma classificação.

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

Clique em **Criar**. Novo filme, incluindo classificação, agora é exibido nos filmes listando:

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

Nesta seção, você viu como você pode modificar objetos de modelo e manter o banco de dados em sincronia com as alterações. Você também aprendeu uma maneira para popular um banco de dados recém-criado com dados de exemplo para que você pode experimentar os cenários. Em seguida, vamos dar uma olhada em como você pode adicionar lógica de validação mais rica para as classes de modelo e habilitar algumas regras de negócio a serem aplicadas.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Próximo](adding-validation-to-the-model.md)
