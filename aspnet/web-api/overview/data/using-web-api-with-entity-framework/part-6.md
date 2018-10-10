---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-6
title: Criar o cliente JavaScript | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 20360326-b123-4b1e-abae-1d350edf4ce4
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-6
msc.type: authoredcontent
ms.openlocfilehash: b5cb4d93c30ef80a48da48ffc51dd51411b1d0d0
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912639"
---
<a name="create-the-javascript-client"></a><span data-ttu-id="85dbb-102">Criar o cliente JavaScript</span><span class="sxs-lookup"><span data-stu-id="85dbb-102">Create the JavaScript Client</span></span>
====================
<span data-ttu-id="85dbb-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="85dbb-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="85dbb-104">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="85dbb-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="85dbb-105">Nesta seção, você irá criar o cliente para o aplicativo usando HTML, JavaScript e o [Knockout. js](http://knockoutjs.com/) biblioteca.</span><span class="sxs-lookup"><span data-stu-id="85dbb-105">In this section, you will create the client for the application, using HTML, JavaScript, and the [Knockout.js](http://knockoutjs.com/) library.</span></span> <span data-ttu-id="85dbb-106">Vamos criar o aplicativo cliente em estágios:</span><span class="sxs-lookup"><span data-stu-id="85dbb-106">We'll build the client app in stages:</span></span>

- <span data-ttu-id="85dbb-107">Mostrando uma lista de livros.</span><span class="sxs-lookup"><span data-stu-id="85dbb-107">Showing a list of books.</span></span>
- <span data-ttu-id="85dbb-108">Mostrando detalhes de um livro.</span><span class="sxs-lookup"><span data-stu-id="85dbb-108">Showing a book detail.</span></span>
- <span data-ttu-id="85dbb-109">Adicionando um novo catálogo.</span><span class="sxs-lookup"><span data-stu-id="85dbb-109">Adding a new book.</span></span>

<span data-ttu-id="85dbb-110">A biblioteca Knockout usa o padrão Model-View-ViewModel (MVVM):</span><span class="sxs-lookup"><span data-stu-id="85dbb-110">The Knockout library uses the Model-View-ViewModel (MVVM) pattern:</span></span>

- <span data-ttu-id="85dbb-111">O **modelo** é a representação do lado do servidor dos dados no domínio de negócios (no nosso caso, manuais e autores).</span><span class="sxs-lookup"><span data-stu-id="85dbb-111">The **model** is the server-side representation of the data in the business domain (in our case, books and authors).</span></span>
- <span data-ttu-id="85dbb-112">O **exibição** é a camada de apresentação (HTML).</span><span class="sxs-lookup"><span data-stu-id="85dbb-112">The **view** is the presentation layer (HTML).</span></span>
- <span data-ttu-id="85dbb-113">O **modelo de exibição** é um objeto JavaScript que contém os modelos.</span><span class="sxs-lookup"><span data-stu-id="85dbb-113">The **view model** is a JavaScript object that holds the models.</span></span> <span data-ttu-id="85dbb-114">O modelo de exibição é uma abstração de código da interface do usuário.</span><span class="sxs-lookup"><span data-stu-id="85dbb-114">The view model is a code abstraction of the UI.</span></span> <span data-ttu-id="85dbb-115">Ele não tem conhecimento da representação HTML.</span><span class="sxs-lookup"><span data-stu-id="85dbb-115">It has no knowledge of the HTML representation.</span></span> <span data-ttu-id="85dbb-116">Em vez disso, ele representa recursos abstratos da exibição, como &quot;uma lista de livros&quot;.</span><span class="sxs-lookup"><span data-stu-id="85dbb-116">Instead, it represents abstract features of the view, such as &quot;a list of books&quot;.</span></span>

<span data-ttu-id="85dbb-117">O modo de exibição associado a dados para o modelo de exibição.</span><span class="sxs-lookup"><span data-stu-id="85dbb-117">The view is data-bound to the view model.</span></span> <span data-ttu-id="85dbb-118">Atualizações para o modelo de exibição são refletidas automaticamente no modo de exibição.</span><span class="sxs-lookup"><span data-stu-id="85dbb-118">Updates to the view model are automatically reflected in the view.</span></span> <span data-ttu-id="85dbb-119">O modelo de exibição também obtém os eventos da exibição, como cliques de botão.</span><span class="sxs-lookup"><span data-stu-id="85dbb-119">The view model also gets events from the view, such as button clicks.</span></span>

![](part-6/_static/image1.png)

<span data-ttu-id="85dbb-120">Essa abordagem facilita a alterar o layout e interface do usuário do seu aplicativo, como você pode alterar as associações, sem reescrever nenhum código.</span><span class="sxs-lookup"><span data-stu-id="85dbb-120">This approach makes it easy to change the layout and UI of your app, because you can change the bindings, without rewriting any code.</span></span> <span data-ttu-id="85dbb-121">Por exemplo, você pode mostrar uma lista de itens como um `<ul>`, alterá-lo posteriormente a uma tabela.</span><span class="sxs-lookup"><span data-stu-id="85dbb-121">For example, you might show a list of items as a `<ul>`, then change it later to a table.</span></span>

## <a name="add-the-knockout-library"></a><span data-ttu-id="85dbb-122">Adicionar a biblioteca Knockout</span><span class="sxs-lookup"><span data-stu-id="85dbb-122">Add the Knockout Library</span></span>

<span data-ttu-id="85dbb-123">No Visual Studio, do **ferramentas** menu, selecione **Gerenciador de pacotes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="85dbb-123">In Visual Studio, from the **Tools** menu, select **NuGet Package Manager**.</span></span> <span data-ttu-id="85dbb-124">Em seguida, selecione **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="85dbb-124">Then select **Package Manager Console**.</span></span> <span data-ttu-id="85dbb-125">Na janela do Console do Gerenciador de pacotes, digite o seguinte comando:</span><span class="sxs-lookup"><span data-stu-id="85dbb-125">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](part-6/samples/sample1.cmd)]

<span data-ttu-id="85dbb-126">Este comando adiciona os arquivos do Knockout para a pasta Scripts.</span><span class="sxs-lookup"><span data-stu-id="85dbb-126">This command adds the Knockout files to the Scripts folder.</span></span>

## <a name="create-the-view-model"></a><span data-ttu-id="85dbb-127">Criar o modelo de exibição</span><span class="sxs-lookup"><span data-stu-id="85dbb-127">Create the View Model</span></span>

<span data-ttu-id="85dbb-128">Adicione um arquivo JavaScript chamado App. js para a pasta Scripts.</span><span class="sxs-lookup"><span data-stu-id="85dbb-128">Add a JavaScript file named app.js to the Scripts folder.</span></span> <span data-ttu-id="85dbb-129">(No Gerenciador de soluções, clique com botão direito na pasta de Scripts, selecione **Add**, em seguida, selecione **arquivo JavaScript**.) Cole o código a seguir:</span><span class="sxs-lookup"><span data-stu-id="85dbb-129">(In Solution Explorer, right-click the Scripts folder, select **Add**, then select **JavaScript File**.) Paste in the following code:</span></span>

[!code-javascript[Main](part-6/samples/sample2.js)]

<span data-ttu-id="85dbb-130">No Knockout, a `observable` classe permite que a associação de dados.</span><span class="sxs-lookup"><span data-stu-id="85dbb-130">In Knockout, the `observable` class enables data-binding.</span></span> <span data-ttu-id="85dbb-131">Quando altera o conteúdo de um observável, o observável notifica todos os controles ligados a dados, para que possam atualizar em si.</span><span class="sxs-lookup"><span data-stu-id="85dbb-131">When the contents of an observable change, the observable notifies all of the data-bound controls, so they can update themselves.</span></span> <span data-ttu-id="85dbb-132">(O `observableArray` classe é a versão de matriz de *observável*.) Para começar, nosso modelo de exibição tem dois observáveis:</span><span class="sxs-lookup"><span data-stu-id="85dbb-132">(The `observableArray` class is the array version of *observable*.) To start with, our view model has two observables:</span></span>

- <span data-ttu-id="85dbb-133">`books` contém a lista de livros.</span><span class="sxs-lookup"><span data-stu-id="85dbb-133">`books` holds the list of books.</span></span>
- <span data-ttu-id="85dbb-134">`error` contém uma mensagem de erro se uma chamada AJAX falhar.</span><span class="sxs-lookup"><span data-stu-id="85dbb-134">`error` contains an error message if an AJAX call fails.</span></span>

<span data-ttu-id="85dbb-135">O `getAllBooks` método faz uma chamada ao AJAX para obter a lista de livros.</span><span class="sxs-lookup"><span data-stu-id="85dbb-135">The `getAllBooks` method makes an AJAX call to get the list of books.</span></span> <span data-ttu-id="85dbb-136">Em seguida, ele envia o resultado para o `books` matriz.</span><span class="sxs-lookup"><span data-stu-id="85dbb-136">Then it pushes the result onto the `books` array.</span></span>

<span data-ttu-id="85dbb-137">O `ko.applyBindings` método faz parte da biblioteca do Knockout.</span><span class="sxs-lookup"><span data-stu-id="85dbb-137">The `ko.applyBindings` method is part of the Knockout library.</span></span> <span data-ttu-id="85dbb-138">Ele usa o modelo de exibição como um parâmetro e configura a vinculação de dados.</span><span class="sxs-lookup"><span data-stu-id="85dbb-138">It takes the view model as a parameter and sets up the data binding.</span></span>

## <a name="add-a-script-bundle"></a><span data-ttu-id="85dbb-139">Adicionar um pacote de Script</span><span class="sxs-lookup"><span data-stu-id="85dbb-139">Add a Script Bundle</span></span>

<span data-ttu-id="85dbb-140">O agrupamento é um recurso do ASP.NET 4.5 que torna mais fácil combinar ou agrupar vários arquivos em um único arquivo.</span><span class="sxs-lookup"><span data-stu-id="85dbb-140">Bundling is a feature in ASP.NET 4.5 that makes it easy to combine or bundle multiple files into a single file.</span></span> <span data-ttu-id="85dbb-141">Agrupamento reduz o número de solicitações para o servidor, o que pode melhorar o tempo de carregamento de página.</span><span class="sxs-lookup"><span data-stu-id="85dbb-141">Bundling reduces the number of requests to the server, which can improve page load time.</span></span>

<span data-ttu-id="85dbb-142">Abra o arquivo de aplicativo\_Start/BundleConfig.cs.</span><span class="sxs-lookup"><span data-stu-id="85dbb-142">Open the file App\_Start/BundleConfig.cs.</span></span> <span data-ttu-id="85dbb-143">Adicione o seguinte código ao método RegisterBundles.</span><span class="sxs-lookup"><span data-stu-id="85dbb-143">Add the following code to the RegisterBundles method.</span></span>

[!code-csharp[Main](part-6/samples/sample3.cs)]

> [!div class="step-by-step"]
> <span data-ttu-id="85dbb-144">[Anterior](part-5.md)
> [Próximo](part-7.md)</span><span class="sxs-lookup"><span data-stu-id="85dbb-144">[Previous](part-5.md)
[Next](part-7.md)</span></span>
