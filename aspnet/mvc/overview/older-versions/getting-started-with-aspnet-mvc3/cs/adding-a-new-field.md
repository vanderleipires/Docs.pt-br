---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Adicionar um novo campo ao modelo de filme e tabela (c#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 3408afe6665d353e9570acc2740b5f1b7ba916da
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367151"
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Adicionar um novo campo ao modelo de filme e tabela (c#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Uma versão atualizada deste tutorial está disponível [aqui](../../../getting-started/introduction/getting-started.md) que usa o ASP.NET MVC 5 e Visual Studio 2013. É mais seguro e muito mais simples a seguir e apresenta mais recursos.
> 
> 
> Este tutorial ensinará os conceitos básicos da criação de um aplicativo Web ASP.NET MVC usando o Microsoft Visual Web Developer 2010 Express Service Pack 1, que é uma versão gratuita do Microsoft Visual Studio. Antes de começar, verifique se que você instalou os pré-requisitos listados abaixo. Você pode instalar todos eles clicando no link a seguir: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, você pode instalar individualmente os pré-requisitos usando os links a seguir:
> 
> - [Pré-requisitos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Atualização de ferramentas do ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tempo de execução de ferramentas de suporte +)
> 
> Se você estiver usando o Visual Studio 2010, em vez do Visual Web Developer 2010, instale os pré-requisitos, clicando no link a seguir: [pré-requisitos do Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Um projeto do Visual Web Developer com código-fonte c# está disponível para acompanhar este tópico. [Baixe a versão c#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se você preferir o Visual Basic, alterne para o [versão do Visual Basic](../vb/intro-to-aspnet-mvc-3.md) deste tutorial.


Nesta seção, você vai fazer algumas alterações para as classes de modelo e saiba como você pode atualizar o esquema de banco de dados de acordo com as alterações do modelo.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adicionando uma propriedade de classificação ao modelo de filme

Comece adicionando um novo `Rating` propriedade à existente `Movie` classe. Abra o *Movie.cs* arquivo e adicione o `Rating` propriedade parecida com esta:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

Completo `Movie` classe agora parece o código a seguir:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Recompilar o aplicativo usando o **Debug** &gt; **Build filme** comando de menu.

Agora que você atualizou o `Model` classe, você também precisa atualizar o *\Views\Movies\Index.cshtml* e *\Views\Movies\Create.cshtml* exibir modelos para dar suporte a novos `Rating`propriedade.

Abra o *\Views\Movies\Index.cshtml* arquivo e adicione um `<th>Rating</th>` título de coluna logo após o **preço** coluna. Em seguida, adicione uma `<td>` coluna, próximo ao final do modelo para renderizar o `@item.Rating` valor. Abaixo está o que a atualização *index. cshtml* modelo de exibição se parece com:

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

Em seguida, abra o *\Views\Movies\Create.cshtml* arquivo e adicione a seguinte marcação próximo ao final do formulário. Isso renderiza uma caixa de texto para que você possa especificar uma classificação quando um novo filme é criado.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Gerenciamento de modelo e diferenças de esquema de banco de dados

Agora você atualizou o código do aplicativo para dar suporte a novos `Rating` propriedade.

Agora, execute o aplicativo e navegue até a */Movies* URL. Quando você fizer isso, no entanto, você verá o seguinte erro:

![](adding-a-new-field/_static/image1.png)

Você está vendo esse erro porque atualizada `Movie` classe de modelo no aplicativo agora é diferente do esquema do `Movie` tabela do banco de dados existente. (Não há nenhuma coluna `Rating` na tabela de banco de dados.)

Por padrão, quando você usa o Entity Framework Code First para criar automaticamente um banco de dados, como você fez neste tutorial, Code First adiciona uma tabela no banco de dados para ajudar a controlar se o esquema do banco de dados está em sincronizado com as classes de modelo, que ele foi gerado. Se não estiverem sincronizados, o Entity Framework gera um erro. Isso torna mais fácil rastrear problemas em tempo de desenvolvimento que caso contrário, apenas talvez (por erros obscuros) em tempo de execução. O recurso de verificação de sincronização é o que faz com que a mensagem de erro a ser exibido que você acabou de ver.

Há duas abordagens para resolver o erro:

1. Faça com que o Entity Framework remova automaticamente e recrie o banco de dados com base no novo esquema de classe de modelo. Essa abordagem é muito conveniente ao fazer o desenvolvimento ativo em um banco de dados de teste, porque ele permite que você desenvolva rapidamente o esquema de modelo e o banco de dados juntos. A desvantagem, no entanto, é que você perde os dados existentes no banco de dados — para que você *não* para usar essa abordagem em um banco de dados de produção!
2. Modifique explicitamente o esquema do banco de dados existente para que ele corresponda às classes de modelo. A vantagem dessa abordagem é que você mantém os dados. Faça essa alteração manualmente ou criando um script de alteração de banco de dados.

Para este tutorial, vamos usar a primeira abordagem, você terá o Entity Framework Code First automaticamente recriar o banco de dados a qualquer momento que o modelo é alterado.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Recriar automaticamente o banco de dados de alterações do modelo

Vamos atualizar o aplicativo para que o Code First automaticamente descarta e recria o banco de dados a qualquer momento que você alterar o modelo para o aplicativo.

> [!NOTE] 
> 
> **Aviso** você deve habilitar essa abordagem de automaticamente descartar e recriar o banco de dados somente quando você estiver usando um banco de dados de desenvolvimento ou teste, e *nunca* em um banco de dados de produção que contém dados reais. Usá-lo em um servidor de produção pode levar à perda de dados.


Na **Gerenciador de soluções**, clique com botão direito do *modelos* pasta, selecione **Add**e, em seguida, selecione **classe**.

![](adding-a-new-field/_static/image2.png)

Nomeie a classe "MovieInitializer". Atualização de `MovieInitializer` classe para conter o código a seguir:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

O `MovieInitializer` classe especifica que o banco de dados usado pelo modelo deve ser descartado e novamente criado automaticamente se as classes de modelo mudem. O código inclui um `Seed` método para especificar alguns dados padrão para adicionar automaticamente para o banco de dados de qualquer uma vez que ele foi criado (ou recriado). Isso fornece uma maneira útil para popular o banco de dados com alguns dados de exemplo, sem a necessidade de você preencher manualmente cada vez que você alterar um modelo.

Agora que você definiu o `MovieInitializer` classe, você vai querer fazer a conexão para que cada vez que o aplicativo é executado, ele verifica se as classes de modelo são diferentes do esquema no banco de dados. Se elas forem, você pode executar o inicializador para recriar o banco de dados para corresponder ao modelo e, em seguida, preencher o banco de dados com os dados de exemplo.

Abra o *global. asax* que está na raiz do arquivo a `MvcMovies` projeto:

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

O *global. asax* arquivo contém a classe que define todo o aplicativo para o projeto e contém um `Application_Start` manipulador de eventos que é executado quando o aplicativo é iniciado pela primeira vez.

Vamos adicionar dois usando instruções na parte superior do arquivo. O primeiro faz referência ao namespace de Entity Framework e o segundo faz referência ao namespace onde nosso `MovieInitializer` vidas de classe:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

Em seguida, localize o `Application_Start` método e adicione uma chamada para `Database.SetInitializer` no início do método, conforme mostrado abaixo:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

O `Database.SetInitializer` instrução que você acabou de adicionar indica que o banco de dados usado pelo `MovieDBContext` instância deverá ser automaticamente excluída e recriada se o esquema e o banco de dados não corresponderem. E como você viu, ele preencherá o banco de dados com os dados de exemplo que são especificados no também o `MovieInitializer` classe.

Fechar o *global. asax* arquivo.

Executar o aplicativo novamente e navegue até a */Movies* URL. Quando o aplicativo é iniciado, ele detecta que a estrutura do modelo não coincide mais com o esquema de banco de dados. Ele automaticamente recria o banco de dados de acordo com a nova estrutura de modelo e preenche o banco de dados com os filmes de exemplo:

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Clique o **criar novo** link para adicionar um novo filme. Observe que você pode adicionar uma classificação.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

Clique em **Criar**. O novo filme, incluindo a classificação é exibido nos listagem de filmes:

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

Nesta seção, você viu como você pode modificar objetos de modelo e manter o banco de dados em sincronia com as alterações. Você também aprendeu uma maneira de preencher um banco de dados recém-criado com dados de exemplo para que você pode experimentar os cenários. Em seguida, vamos dar uma olhada em como você pode adicionar lógica de validação mais avançada para as classes de modelo e habilitar algumas regras de negócio a serem impostos.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Próximo](adding-validation-to-the-model.md)
