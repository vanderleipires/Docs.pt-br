---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: Usando as anotações de dados para validação de modelo | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC. Parte 6 abrange usando as anotações de dados modelo V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="part-6-using-data-annotations-for-model-validation"></a><span data-ttu-id="5cbf5-104">Parte 6: Usando as anotações de dados para a validação de modelo</span><span class="sxs-lookup"><span data-stu-id="5cbf5-104">Part 6: Using Data Annotations for Model Validation</span></span>
====================
<span data-ttu-id="5cbf5-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="5cbf5-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="5cbf5-106">O repositório de música MVC é um aplicativo tutorial que apresenta e explica passo a passo sobre como usar o ASP.NET MVC e o Visual Studio para desenvolvimento na web.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="5cbf5-107">O repositório de música MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e funcionalidade do carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="5cbf5-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo do repositório de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="5cbf5-109">Parte 6 abrange o uso de anotações de dados para a validação do modelo.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-109">Part 6 covers Using Data Annotations for Model Validation.</span></span>


<span data-ttu-id="5cbf5-110">Temos um grande problema com nosso formas de criar e editar: não fazem nenhuma validação.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-110">We have a major issue with our Create and Edit forms: they're not doing any validation.</span></span> <span data-ttu-id="5cbf5-111">Podemos fazer coisas como deixar em branco os campos obrigatórios ou letras de tipo no campo de, e é o primeiro erro que veremos do banco de dados.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-111">We can do things like leave required fields blank or type letters in the Price field, and the first error we'll see is from the database.</span></span>

<span data-ttu-id="5cbf5-112">Podemos facilmente adicionar validação para o nosso aplicativo adicionando anotações de dados para nosso classes de modelo.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-112">We can easily add validation to our application by adding Data Annotations to our model classes.</span></span> <span data-ttu-id="5cbf5-113">As anotações de dados que descrevem as regras que queremos aplicadas às nossas propriedades de modelo e ASP.NET MVC cuidará de aplicá-las e exibir as mensagens apropriadas para os usuários.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-113">Data Annotations allow us to describe the rules we want applied to our model properties, and ASP.NET MVC will take care of enforcing them and displaying appropriate messages to our users.</span></span>

## <a name="adding-validation-to-our-album-forms"></a><span data-ttu-id="5cbf5-114">Adicionando validação a nossos formulários álbum</span><span class="sxs-lookup"><span data-stu-id="5cbf5-114">Adding Validation to our Album Forms</span></span>

<span data-ttu-id="5cbf5-115">Vamos usar os seguintes atributos de anotação de dados:</span><span class="sxs-lookup"><span data-stu-id="5cbf5-115">We'll use the following Data Annotation attributes:</span></span>

- <span data-ttu-id="5cbf5-116">**Necessário** – indica que a propriedade é um campo obrigatório</span><span class="sxs-lookup"><span data-stu-id="5cbf5-116">**Required** – Indicates that the property is a required field</span></span>
- <span data-ttu-id="5cbf5-117">**DisplayName** – define o texto que desejamos usados nos campos de formulário e mensagens de validação</span><span class="sxs-lookup"><span data-stu-id="5cbf5-117">**DisplayName** – Defines the text we want used on form fields and validation messages</span></span>
- <span data-ttu-id="5cbf5-118">**StringLength** – define um comprimento máximo de um campo de cadeia de caracteres</span><span class="sxs-lookup"><span data-stu-id="5cbf5-118">**StringLength** – Defines a maximum length for a string field</span></span>
- <span data-ttu-id="5cbf5-119">**Intervalo** – fornece um valor mínimo e máximo para um campo numérico</span><span class="sxs-lookup"><span data-stu-id="5cbf5-119">**Range** – Gives a maximum and minimum value for a numeric field</span></span>
- <span data-ttu-id="5cbf5-120">**Associar** – lista de campos para excluir ou incluir ao associar valores de parâmetro ou formulário a propriedades de modelo</span><span class="sxs-lookup"><span data-stu-id="5cbf5-120">**Bind** – Lists fields to exclude or include when binding parameter or form values to model properties</span></span>
- <span data-ttu-id="5cbf5-121">**ScaffoldColumn** – permite ocultar campos de formulários do editor</span><span class="sxs-lookup"><span data-stu-id="5cbf5-121">**ScaffoldColumn** – Allows hiding fields from editor forms</span></span>

<span data-ttu-id="5cbf5-122">*Observação: Para obter mais informações sobre a validação de modelo usando atributos de anotação de dados, consulte a documentação do MSDN em*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span><span class="sxs-lookup"><span data-stu-id="5cbf5-122">*Note: For more information on Model Validation using Data Annotation attributes, see the MSDN documentation at*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)</span></span>

<span data-ttu-id="5cbf5-123">Abra a classe álbum e adicione o seguinte *usando* instruções para a parte superior.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-123">Open the Album class and add the following *using* statements to the top.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

<span data-ttu-id="5cbf5-124">Em seguida, atualize as propriedades para adicionar atributos de exibição e a validação, conforme mostrado abaixo.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-124">Next, update the properties to add display and validation attributes as shown below.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

<span data-ttu-id="5cbf5-125">Enquanto estiver lá, nós também alteramos o gênero e artista para propriedades virtuais.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-125">While we're there, we've also changed the Genre and Artist to virtual properties.</span></span> <span data-ttu-id="5cbf5-126">Isso permite que o Entity Framework lento-carga-los conforme necessário.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-126">This allows Entity Framework to lazy-load them as necessary.</span></span>

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

<span data-ttu-id="5cbf5-127">Após ter adicionado a esses atributos ao nosso modelo álbum, nossa tela criar e editar imediatamente começa a validar campos e usando os nomes de exibição que escolhemos (por exemplo, álbum de arte Url em vez de AlbumArtUrl).</span><span class="sxs-lookup"><span data-stu-id="5cbf5-127">After having added these attributes to our Album model, our Create and Edit screen immediately begin validating fields and using the Display Names we've chosen (e.g. Album Art Url instead of AlbumArtUrl).</span></span> <span data-ttu-id="5cbf5-128">Execute o aplicativo e navegue até /StoreManager/Create.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-128">Run the application and browse to /StoreManager/Create.</span></span>

![](mvc-music-store-part-6/_static/image1.png)

<span data-ttu-id="5cbf5-129">Em seguida, vamos quebrar algumas regras de validação.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-129">Next, we'll break some validation rules.</span></span> <span data-ttu-id="5cbf5-130">Insira um preço de 0 e deixe o título em branco.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-130">Enter a price of 0 and leave the Title blank.</span></span> <span data-ttu-id="5cbf5-131">Quando é clicar no botão Criar, vemos o formulário exibido com mensagens de erro de validação mostrando os campos que não atendeu as regras de validação definimos.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-131">When we click on the Create button, we will see the form displayed with validation error messages showing which fields did not meet the validation rules we have defined.</span></span>

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a><span data-ttu-id="5cbf5-132">Testando a validação do lado do cliente</span><span class="sxs-lookup"><span data-stu-id="5cbf5-132">Testing the Client-Side Validation</span></span>

<span data-ttu-id="5cbf5-133">Validação do lado do servidor é muito importante da perspectiva do aplicativo, porque os usuários podem contornar a validação do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-133">Server-side validation is very important from an application perspective, because users can circumvent client-side validation.</span></span> <span data-ttu-id="5cbf5-134">No entanto, os formulários de página da Web que implementam a validação do lado do servidor só exibem três problemas significativos.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-134">However, webpage forms which only implement server-side validation exhibit three significant problems.</span></span>

1. <span data-ttu-id="5cbf5-135">O usuário deve aguardar para o formulário a ser lançada, validados no servidor e para a resposta a ser enviada ao navegador.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-135">The user has to wait for the form to be posted, validated on the server, and for the response to be sent to their browser.</span></span>
2. <span data-ttu-id="5cbf5-136">O usuário não obter feedback imediato quando eles corrigir um campo para que ele seja aprovado agora as regras de validação.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-136">The user doesn't get immediate feedback when they correct a field so that it now passes the validation rules.</span></span>
3. <span data-ttu-id="5cbf5-137">Nós são desperdício de recursos de servidor para executar a lógica de validação em vez de utilizar o navegador do usuário.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-137">We are wasting server resources to perform validation logic instead of leveraging the user's browser.</span></span>

<span data-ttu-id="5cbf5-138">Felizmente, os modelos de scaffold do ASP.NET MVC 3 têm validação do lado do cliente interna, exigindo sem trabalho adicional de qualquer tipo.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-138">Fortunately, the ASP.NET MVC 3 scaffold templates have client-side validation built in, requiring no additional work whatsoever.</span></span>

<span data-ttu-id="5cbf5-139">Digitar uma única letra no campo título satisfaz os requisitos de validação, portanto, a mensagem de validação é removida imediatamente.</span><span class="sxs-lookup"><span data-stu-id="5cbf5-139">Typing a single letter in the Title field satisfies the validation requirements, so the validation message is immediately removed.</span></span>

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> <span data-ttu-id="5cbf5-140">[Anterior](mvc-music-store-part-5.md)
> [Próximo](mvc-music-store-part-7.md)</span><span class="sxs-lookup"><span data-stu-id="5cbf5-140">[Previous](mvc-music-store-part-5.md)
[Next](mvc-music-store-part-7.md)</span></span>
