---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Parte 3: Layout e Menu de categoria | Microsoft Docs'
author: JoeStagner
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks. Parte 3 aborda a adição de layout e um menu de categoria.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: f55b29a271dbdb72d3e2249ed74517b77d78cf5e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825592"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="721be-104">Parte 3: Layout e Menu de categoria</span><span class="sxs-lookup"><span data-stu-id="721be-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="721be-105">por [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="721be-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="721be-106">Tailspin Spyworks demonstra como incrivelmente simples é criar aplicativos avançados e escalonáveis para a plataforma .NET.</span><span class="sxs-lookup"><span data-stu-id="721be-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="721be-107">Ele mostra como usar os novos recursos no ASP.NET 4 para criar uma loja online, incluindo as compras, check-out e administração.</span><span class="sxs-lookup"><span data-stu-id="721be-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="721be-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo Tailspin Spyworks.</span><span class="sxs-lookup"><span data-stu-id="721be-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="721be-109">Parte 3 aborda a adição de layout e um menu de categoria.</span><span class="sxs-lookup"><span data-stu-id="721be-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="721be-110">Adicionando alguns Layout e um Menu de categoria</span><span class="sxs-lookup"><span data-stu-id="721be-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="721be-111">Em nossa página mestre do site, adicionaremos um div para a coluna do lado esquerdo que conterá nosso menu de categoria de produto.</span><span class="sxs-lookup"><span data-stu-id="721be-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="721be-112">Observe que o alinhamento desejado e outra formatação serão fornecidos pela classe CSS que adicionamos ao nosso arquivo Style CSS.</span><span class="sxs-lookup"><span data-stu-id="721be-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="721be-113">O menu de categoria de produto será ser criado dinamicamente em tempo de execução consultando o banco de dados do Commerce para categorias de produto e a criação dos itens de menu e existentes correspondente vincula.</span><span class="sxs-lookup"><span data-stu-id="721be-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="721be-114">Para fazer isso, que usaremos dois do ASP. Controles de dados avançada do NET.</span><span class="sxs-lookup"><span data-stu-id="721be-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="721be-115">O controle de "Entidade de fonte de dados" e o controle de "ListView".</span><span class="sxs-lookup"><span data-stu-id="721be-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="721be-116">Vamos alternar para o "Modo de Design" e usar os auxiliares para configurar os controles.</span><span class="sxs-lookup"><span data-stu-id="721be-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="721be-117">Vamos definir a propriedade ID EntityDataSource como EDS\_categoria\_Menu e clique em "Configurar a fonte de dados".</span><span class="sxs-lookup"><span data-stu-id="721be-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="721be-118">Selecione a Conexão CommerceEntities que foi criado para nós quando criamos o modelo de fonte de dados de entidade para nosso banco de dados de comércio e clique em "Avançar".</span><span class="sxs-lookup"><span data-stu-id="721be-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="721be-119">Selecione o nome do conjunto de entidade "Categorias" e deixe o restante das opções como padrão.</span><span class="sxs-lookup"><span data-stu-id="721be-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="721be-120">Clique em "Concluir".</span><span class="sxs-lookup"><span data-stu-id="721be-120">Click "Finish".</span></span>

<span data-ttu-id="721be-121">Agora vamos definir a propriedade de ID da instância do controle ListView que colocamos em nossa página de ListView\_ProductsMenu e ativar seu auxiliar.</span><span class="sxs-lookup"><span data-stu-id="721be-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="721be-122">No entanto, poderíamos usar opções de controle para formatar a exibição de item de dados e formatação, a criação de nosso menu necessitarão apenas de marcação simples para que inserimos o código na exibição da fonte.</span><span class="sxs-lookup"><span data-stu-id="721be-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="721be-123">Observe que a instrução "Eval": &lt;# Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="721be-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="721be-124">A sintaxe ASP.NET &lt;# %&gt; é uma convenção de taquigrafia que instrui o tempo de execução para executar tudo o que está contido e gerar os resultados "na linha".</span><span class="sxs-lookup"><span data-stu-id="721be-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="721be-125">A instrução Eval("CategoryName") instrui que, para a entrada atual na coleção associada de itens de dados, busca o valor dos nomes de item de modelo de entidade "CatagoryName".</span><span class="sxs-lookup"><span data-stu-id="721be-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="721be-126">Isso é uma sintaxe concisa para um recurso muito poderoso.</span><span class="sxs-lookup"><span data-stu-id="721be-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="721be-127">Vamos executar o aplicativo agora.</span><span class="sxs-lookup"><span data-stu-id="721be-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="721be-128">Observe que nosso menu de categoria de produto agora é exibido e quando focalizamos em um dos itens de menu categoria, podemos ver os pontos de link de item de menu a uma página é ainda preciso implementar chamado ProductsList.aspx e que criamos um argumento de cadeia de caracteres de consulta dinâmica que contém o  id da categoria.</span><span class="sxs-lookup"><span data-stu-id="721be-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="721be-129">[Anterior](tailspin-spyworks-part-2.md)
> [Próximo](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="721be-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
