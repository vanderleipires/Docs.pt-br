---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: Usando anotações de dados para validação de modelo | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 6 aborda o uso de anotações de dados para o modelo V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: ca0ac2ca909f838f1c91e6cc01b8aafa90c0b193
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37397646"
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="6b6fe-104">Parte 6: Usando Data Annotations para validação de modelo</span><span class="sxs-lookup"><span data-stu-id="6b6fe-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="6b6fe-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="6b6fe-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="6b6fe-106">A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="6b6fe-107">A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="6b6fe-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="6b6fe-109">Parte 6 aborda o uso de anotações de dados para a validação de modelo.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="6b6fe-110">Temos um grande problema com nossos formulários de criar e editar: eles não estão fazendo nenhuma validação.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="6b6fe-111">Podemos fazer coisas como deixar os campos obrigatórios em branco ou tipo letras no campo de preço e o primeiro erro que veremos é do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="6b6fe-112">Podemos facilmente adicionar validação ao nosso aplicativo com a adição de anotações de dados para nossas classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="6b6fe-113">Anotações de dados nos permitem descrever as regras que queremos que sejam aplicadas a nossas propriedades de modelo e o ASP.NET MVC se encarregará de impô-las e exibir as mensagens apropriadas para nossos usuários.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="6b6fe-114">Adicionando validação a nossos formulários de álbum</span><span class="sxs-lookup"><span data-stu-id="6b6fe-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="6b6fe-115">Vamos usar os seguintes atributos de anotação de dados:</span><span class="sxs-lookup"><span data-stu-id="6b6fe-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="6b6fe-116">**Necessário** – indica que a propriedade é um campo obrigatório</span><span class="sxs-lookup"><span data-stu-id="6b6fe-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="6b6fe-117">**DisplayName** – define o texto que desejamos usados em campos de formulário e mensagens de validação</span><span class="sxs-lookup"><span data-stu-id="6b6fe-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="6b6fe-118">**StringLength** – define um comprimento máximo para um campo de cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="6b6fe-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="6b6fe-119">**Intervalo** – fornece um valor mínimo e máximo para um campo numérico</span><span class="sxs-lookup"><span data-stu-id="6b6fe-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="6b6fe-120">**Associar** – lista de campos para excluir ou incluir ao associar os valores de parâmetro ou o formulário a propriedades de modelo</span><span class="sxs-lookup"><span data-stu-id="6b6fe-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="6b6fe-121">**ScaffoldColumn** – permite ocultar campos de formulários do editor</span><span class="sxs-lookup"><span data-stu-id="6b6fe-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="6b6fe-122">*Observação: Para obter mais informações sobre validação de modelo usando atributos de anotação de dados, consulte a documentação do MSDN em*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="6b6fe-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="6b6fe-123">Abra a classe de álbum e adicione o seguinte *usando* instruções na parte superior.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="6b6fe-124">Em seguida, atualize as propriedades para adicionar atributos de validação e exibição, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="6b6fe-125">Enquanto estamos fazendo isso, nós também alteramos o gênero e o artista para propriedades virtuais.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="6b6fe-126">Isso permite que o Entity Framework carregamento lento-los conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="6b6fe-127">Depois de ter adicionado a esses atributos ao nosso modelo de álbum, nossa tela de criar e editar imediatamente começa a validar campos e usando os nomes de exibição, escolhemos (por exemplo, álbum arte Url em vez de AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="6b6fe-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="6b6fe-128">Execute o aplicativo e navegue até /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="6b6fe-129">Em seguida, dividiremos algumas regras de validação.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="6b6fe-130">Insira um preço de 0 e deixe o título em branco.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="6b6fe-131">Quando clicamos no botão Criar, vemos o formulário exibido com mensagens de erro de validação que mostra quais campos não atendeu as regras de validação definimos.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="6b6fe-132">Testando a validação do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="6b6fe-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="6b6fe-133">Validação do lado do servidor é muito importante da perspectiva do aplicativo, porque os usuários podem ignorar a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="6b6fe-134">No entanto, os formulários de página da Web que implementam a validação do lado do servidor só exibem três problemas consideráveis.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="6b6fe-135">O usuário precisa esperar para o formulário seja postado, validados no servidor e para a resposta seja enviada ao seu navegador.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="6b6fe-136">O usuário não obtém comentários imediatos quando eles corrigem um campo para que ele agora passa as regras de validação.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="6b6fe-137">Nós são desperdício de recursos de servidor para executar a lógica de validação em vez de aproveitar o navegador do usuário.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="6b6fe-138">Felizmente, os modelos do scaffold ASP.NET MVC 3 tem validação do lado do cliente interna, não exigindo nenhum trabalho adicional para qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="6b6fe-139">Digitar uma única letra no campo título satisfaz os requisitos de validação, para que a mensagem de validação é removida imediatamente.</span><span class="sxs-lookup"><span data-stu-id="6b6fe-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="6b6fe-140">[Anterior](mvc-music-store-part-5.md)
> [Próximo](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="6b6fe-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
