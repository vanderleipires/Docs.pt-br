---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Validação do modelo na API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 75474d5597ba11e09f788269b9a2a278559c63a9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393090"
---
<a name="model-validation-in-aspnet-web-api"></a><span data-ttu-id="77245-102">Validação do modelo na API Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="77245-102">Model Validation in ASP.NET Web API</span></span>
====================
<span data-ttu-id="77245-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="77245-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="77245-104">Quando um cliente envia dados para sua API da web, muitas vezes você deseja validar os dados antes de fazer qualquer processamento.</span><span class="sxs-lookup"><span data-stu-id="77245-104">When a client sends data to your web API, often you want to validate the data before doing any processing.</span></span> <span data-ttu-id="77245-105">Este artigo mostra como anotar seus modelos, use as anotações para validação de dados e tratar erros de validação em sua API da web.</span><span class="sxs-lookup"><span data-stu-id="77245-105">This article shows how to annotate your models, use the annotations for data validation, and handle validation errors in your web API.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="77245-106">Anotações de dados</span><span class="sxs-lookup"><span data-stu-id="77245-106">Data Annotations</span></span>

<span data-ttu-id="77245-107">Na API Web ASP.NET, você pode usar atributos do [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace para definir regras de validação para propriedades em seu modelo.</span><span class="sxs-lookup"><span data-stu-id="77245-107">In ASP.NET Web API, you can use attributes from the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace to set validation rules for properties on your model.</span></span> <span data-ttu-id="77245-108">Considere o seguinte modelo:</span><span class="sxs-lookup"><span data-stu-id="77245-108">Consider the following model:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="77245-109">Se você tiver usado a validação do modelo no ASP.NET MVC, isso deve ser familiar.</span><span class="sxs-lookup"><span data-stu-id="77245-109">If you have used model validation in ASP.NET MVC, this should look familiar.</span></span> <span data-ttu-id="77245-110">O **necessária** atributo diz que o `Name` propriedade não pode ser nula.</span><span class="sxs-lookup"><span data-stu-id="77245-110">The **Required** attribute says that the `Name` property must not be null.</span></span> <span data-ttu-id="77245-111">O **intervalo** atributo diz que `Weight` deve estar entre zero e 999.</span><span class="sxs-lookup"><span data-stu-id="77245-111">The **Range** attribute says that `Weight` must be between zero and 999.</span></span>

<span data-ttu-id="77245-112">Suponha que um cliente envia uma solicitação POST com a seguinte representação JSON:</span><span class="sxs-lookup"><span data-stu-id="77245-112">Suppose that a client sends a POST request with the following JSON representation:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

<span data-ttu-id="77245-113">Você pode ver que o cliente não incluiu o `Name` propriedade, que é marcada como obrigatório.</span><span class="sxs-lookup"><span data-stu-id="77245-113">You can see that the client did not include the `Name` property, which is marked as required.</span></span> <span data-ttu-id="77245-114">Quando a API da Web converte o JSON em uma `Product` instância, ele valida o `Product` contra os atributos de validação.</span><span class="sxs-lookup"><span data-stu-id="77245-114">When Web API converts the JSON into a `Product` instance, it validates the `Product` against the validation attributes.</span></span> <span data-ttu-id="77245-115">A ação de controlador, você pode verificar se o modelo é válido:</span><span class="sxs-lookup"><span data-stu-id="77245-115">In your controller action, you can check whether the model is valid:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="77245-116">Validação de modelo não garante que os dados de cliente estão seguros.</span><span class="sxs-lookup"><span data-stu-id="77245-116">Model validation does not guarantee that client data is safe.</span></span> <span data-ttu-id="77245-117">Validação adicional pode ser necessário em outras camadas do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="77245-117">Additional validation might be needed in other layers of the application.</span></span> <span data-ttu-id="77245-118">(Por exemplo, a camada de dados pode impor restrições de chave estrangeira.) O tutorial [usando a API da Web com o Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explora alguns desses problemas.</span><span class="sxs-lookup"><span data-stu-id="77245-118">(For example, the data layer might enforce foreign key constraints.) The tutorial [Using Web API with Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) explores some of these issues.</span></span>

<span data-ttu-id="77245-119">**"Corrigiremos lançamento"**: lançamento corrigiremos acontece quando o cliente omite algumas propriedades.</span><span class="sxs-lookup"><span data-stu-id="77245-119">**"Under-Posting"**: Under-posting happens when the client leaves out some properties.</span></span> <span data-ttu-id="77245-120">Por exemplo, suponha que o cliente envia o seguinte:</span><span class="sxs-lookup"><span data-stu-id="77245-120">For example, suppose the client sends the following:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

<span data-ttu-id="77245-121">Aqui, o cliente não especificou valores para `Price` ou `Weight`.</span><span class="sxs-lookup"><span data-stu-id="77245-121">Here, the client did not specify values for `Price` or `Weight`.</span></span> <span data-ttu-id="77245-122">O formatador JSON atribui um valor padrão de zero para as propriedades ausentes.</span><span class="sxs-lookup"><span data-stu-id="77245-122">The JSON formatter assigns a default value of zero to the missing properties.</span></span>

![](model-validation-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="77245-123">O estado do modelo é válido, porque o zero é um valor válido para essas propriedades.</span><span class="sxs-lookup"><span data-stu-id="77245-123">The model state is valid, because zero is a valid value for these properties.</span></span> <span data-ttu-id="77245-124">Se esse é um problema depende do seu cenário.</span><span class="sxs-lookup"><span data-stu-id="77245-124">Whether this is a problem depends on your scenario.</span></span> <span data-ttu-id="77245-125">Por exemplo, em uma operação de atualização, convém distinguir entre "zero" e "não definido".</span><span class="sxs-lookup"><span data-stu-id="77245-125">For example, in an update operation, you might want to distinguish between "zero" and "not set."</span></span> <span data-ttu-id="77245-126">Para forçar os clientes para definir um valor, verifique a propriedade anulável e defina as **necessário** atributo:</span><span class="sxs-lookup"><span data-stu-id="77245-126">To force clients to set a value, make the property nullable and set the **Required** attribute:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

<span data-ttu-id="77245-127">**"Excesso de postagem"**: um cliente também pode enviar *mais* dados que o esperado.</span><span class="sxs-lookup"><span data-stu-id="77245-127">**"Over-Posting"**: A client can also send *more* data than you expected.</span></span> <span data-ttu-id="77245-128">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="77245-128">For example:</span></span>

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

<span data-ttu-id="77245-129">Aqui, o JSON inclui uma propriedade ("Color") que não existe no `Product` modelo.</span><span class="sxs-lookup"><span data-stu-id="77245-129">Here, the JSON includes a property ("Color") that does not exist in the `Product` model.</span></span> <span data-ttu-id="77245-130">Nesse caso, o formatador JSON simplesmente ignora esse valor.</span><span class="sxs-lookup"><span data-stu-id="77245-130">In this case, the JSON formatter simply ignores this value.</span></span> <span data-ttu-id="77245-131">(O formatador XML faz o mesmo.) Excesso de postagem causa problemas se seu modelo tem propriedades que devem ser somente leitura.</span><span class="sxs-lookup"><span data-stu-id="77245-131">(The XML formatter does the same.) Over-posting causes problems if your model has properties that you intended to be read-only.</span></span> <span data-ttu-id="77245-132">Por exemplo:</span><span class="sxs-lookup"><span data-stu-id="77245-132">For example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

<span data-ttu-id="77245-133">Você não deseja que os usuários atualizem o `IsAdmin` propriedade e elevar a mesmos para os administradores!</span><span class="sxs-lookup"><span data-stu-id="77245-133">You don't want users to update the `IsAdmin` property and elevate themselves to administrators!</span></span> <span data-ttu-id="77245-134">A estratégia mais segura é usar uma classe de modelo que corresponda exatamente o que o cliente tem permissão para enviar:</span><span class="sxs-lookup"><span data-stu-id="77245-134">The safest strategy is to use a model class that exactly matches what the client is allowed to send:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> <span data-ttu-id="77245-135">Postagem de blog de Brad Wilson "[vs de validação de entrada. Modelo de validação no ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"tem uma boa discussão do lançamento insuficiente e excesso de postagem.</span><span class="sxs-lookup"><span data-stu-id="77245-135">Brad Wilson's blog post "[Input Validation vs. Model Validation in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" has a good discussion of under-posting and over-posting.</span></span> <span data-ttu-id="77245-136">Embora a postagem é sobre o ASP.NET MVC 2, os problemas ainda são relevantes para a API Web.</span><span class="sxs-lookup"><span data-stu-id="77245-136">Although the post is about ASP.NET MVC 2, the issues are still relevant to Web API.</span></span>


## <a name="handling-validation-errors"></a><span data-ttu-id="77245-137">Tratamento de erros de validação</span><span class="sxs-lookup"><span data-stu-id="77245-137">Handling Validation Errors</span></span>

<span data-ttu-id="77245-138">API da Web não automaticamente retornar um erro ao cliente quando a validação falha.</span><span class="sxs-lookup"><span data-stu-id="77245-138">Web API does not automatically return an error to the client when validation fails.</span></span> <span data-ttu-id="77245-139">Cabe a ação do controlador para verificar o estado do modelo e responder adequadamente.</span><span class="sxs-lookup"><span data-stu-id="77245-139">It is up to the controller action to check the model state and respond appropriately.</span></span>

<span data-ttu-id="77245-140">Você também pode criar um filtro de ação para verificar o estado do modelo antes que a ação do controlador seja invocada.</span><span class="sxs-lookup"><span data-stu-id="77245-140">You can also create an action filter to check the model state before the controller action is invoked.</span></span> <span data-ttu-id="77245-141">O código a seguir mostra um exemplo:</span><span class="sxs-lookup"><span data-stu-id="77245-141">The following code shows an example:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

<span data-ttu-id="77245-142">Se a validação do modelo falhar, esse filtro retorna uma resposta HTTP que contém os erros de validação.</span><span class="sxs-lookup"><span data-stu-id="77245-142">If model validation fails, this filter returns an HTTP response that contains the validation errors.</span></span> <span data-ttu-id="77245-143">Nesse caso, a ação do controlador não é invocada.</span><span class="sxs-lookup"><span data-stu-id="77245-143">In that case, the controller action is not invoked.</span></span>

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

<span data-ttu-id="77245-144">Para aplicar esse filtro para todos os controladores de API da Web, adicione uma instância do filtro para o **HttpConfiguration.Filters** coleção durante a configuração:</span><span class="sxs-lookup"><span data-stu-id="77245-144">To apply this filter to all Web API controllers, add an instance of the filter to the **HttpConfiguration.Filters** collection during configuration:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

<span data-ttu-id="77245-145">Outra opção é definir o filtro como um atributo em controladores ou ações do controlador individuais:</span><span class="sxs-lookup"><span data-stu-id="77245-145">Another option is to set the filter as an attribute on individual controllers or controller actions:</span></span>

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
