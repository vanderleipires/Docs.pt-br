---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Validando com a Interface IDataErrorInfo (c#) | Microsoft Docs
author: StephenWalther
description: "Stephen Walther mostra como exibir mensagens de erro de validação personalizado implementando a interface IDataErrorInfo em uma classe de modelo."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: c04088c576481e4a91676d7e6962c03b56e7a8a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a><span data-ttu-id="edcdc-103">Validando com a Interface IDataErrorInfo (c#)</span><span class="sxs-lookup"><span data-stu-id="edcdc-103">Validating with the IDataErrorInfo Interface (C#)</span></span>
====================
<span data-ttu-id="edcdc-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="edcdc-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="edcdc-105">Stephen Walther mostra como exibir mensagens de erro de validação personalizado implementando a interface IDataErrorInfo em uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="edcdc-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="edcdc-106">O objetivo deste tutorial é explicar um método para executar a validação em um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="edcdc-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="edcdc-107">Você aprenderá a impedir que alguém enviar um formulário HTML sem fornecer valores para os campos obrigatórios do formulário.</span><span class="sxs-lookup"><span data-stu-id="edcdc-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="edcdc-108">Neste tutorial, você aprenderá a executar a validação usando a interface IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="edcdc-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="edcdc-109">Suposições</span><span class="sxs-lookup"><span data-stu-id="edcdc-109">Assumptions</span></span>

<span data-ttu-id="edcdc-110">Neste tutorial, usarei o banco de dados MoviesDB e a tabela de banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="edcdc-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="edcdc-111">Esta tabela tem as seguintes colunas:</span><span class="sxs-lookup"><span data-stu-id="edcdc-111">This table has the following columns:</span></span>

<a id="0.5_table01"></a>


| <span data-ttu-id="edcdc-112">**Nome da coluna**</span><span class="sxs-lookup"><span data-stu-id="edcdc-112">**Column Name**</span></span> | <span data-ttu-id="edcdc-113">**Tipo de dados**</span><span class="sxs-lookup"><span data-stu-id="edcdc-113">**Data Type**</span></span> | <span data-ttu-id="edcdc-114">**Permitir nulos**</span><span class="sxs-lookup"><span data-stu-id="edcdc-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="edcdc-115">Id</span><span class="sxs-lookup"><span data-stu-id="edcdc-115">Id</span></span> | <span data-ttu-id="edcdc-116">int</span><span class="sxs-lookup"><span data-stu-id="edcdc-116">Int</span></span> | <span data-ttu-id="edcdc-117">False</span><span class="sxs-lookup"><span data-stu-id="edcdc-117">False</span></span> |
| <span data-ttu-id="edcdc-118">Título</span><span class="sxs-lookup"><span data-stu-id="edcdc-118">Title</span></span> | <span data-ttu-id="edcdc-119">nvarchar (100)</span><span class="sxs-lookup"><span data-stu-id="edcdc-119">Nvarchar(100)</span></span> | <span data-ttu-id="edcdc-120">False</span><span class="sxs-lookup"><span data-stu-id="edcdc-120">False</span></span> |
| <span data-ttu-id="edcdc-121">Diretor</span><span class="sxs-lookup"><span data-stu-id="edcdc-121">Director</span></span> | <span data-ttu-id="edcdc-122">nvarchar (100)</span><span class="sxs-lookup"><span data-stu-id="edcdc-122">Nvarchar(100)</span></span> | <span data-ttu-id="edcdc-123">False</span><span class="sxs-lookup"><span data-stu-id="edcdc-123">False</span></span> |
| <span data-ttu-id="edcdc-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="edcdc-124">DateReleased</span></span> | <span data-ttu-id="edcdc-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="edcdc-125">DateTime</span></span> | <span data-ttu-id="edcdc-126">False</span><span class="sxs-lookup"><span data-stu-id="edcdc-126">False</span></span> |


<span data-ttu-id="edcdc-127">Neste tutorial, posso usar o Microsoft Entity Framework para gerar classes de modelo meu banco de dados.</span><span class="sxs-lookup"><span data-stu-id="edcdc-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="edcdc-128">A classe de filme gerada pelo Entity Framework é exibida na Figura 1.</span><span class="sxs-lookup"><span data-stu-id="edcdc-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="edcdc-129">[![A entidade de filme](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="edcdc-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)</span></span>

<span data-ttu-id="edcdc-130">**Figura 01**: entidade o filme ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="edcdc-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="edcdc-131">Para saber mais sobre como usar o Entity Framework para gerar as classes de modelo de banco de dados, consulte o tutorial Criando Classes de modelo com o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="edcdc-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="edcdc-132">A classe do controlador</span><span class="sxs-lookup"><span data-stu-id="edcdc-132">The Controller Class</span></span>

<span data-ttu-id="edcdc-133">Podemos usar o controlador Home filmes de lista e criar novos filmes.</span><span class="sxs-lookup"><span data-stu-id="edcdc-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="edcdc-134">O código para essa classe está contido na listagem 1.</span><span class="sxs-lookup"><span data-stu-id="edcdc-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="edcdc-135">**Listando 1 - Controllers\HomeController.cs**</span><span class="sxs-lookup"><span data-stu-id="edcdc-135">**Listing 1 - Controllers\HomeController.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

<span data-ttu-id="edcdc-136">A classe do controlador Home na listagem 1 contém duas ações Create ().</span><span class="sxs-lookup"><span data-stu-id="edcdc-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="edcdc-137">A primeira ação exibe o formulário HTML para criar um novo filme.</span><span class="sxs-lookup"><span data-stu-id="edcdc-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="edcdc-138">A segunda ação Create () executa a inserção real do novo filme no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="edcdc-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="edcdc-139">A segunda ação Create () é chamada quando o formulário exibido, a primeira ação Create () é enviado ao servidor.</span><span class="sxs-lookup"><span data-stu-id="edcdc-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="edcdc-140">Observe que a segunda ação Create () contém linhas de código a seguir:</span><span class="sxs-lookup"><span data-stu-id="edcdc-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

<span data-ttu-id="edcdc-141">A propriedade IsValid retorna false quando há um erro de validação.</span><span class="sxs-lookup"><span data-stu-id="edcdc-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="edcdc-142">Nesse caso, o modo de exibição de criação que contém o formulário HTML para a criação de um filme é exibida novamente.</span><span class="sxs-lookup"><span data-stu-id="edcdc-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="edcdc-143">Criar uma classe parcial</span><span class="sxs-lookup"><span data-stu-id="edcdc-143">Creating a Partial Class</span></span>

<span data-ttu-id="edcdc-144">A classe de filme é gerada pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="edcdc-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="edcdc-145">Você pode ver o código para a classe de filme, se você expandir o arquivo MoviesDBModel.edmx na janela do Gerenciador de soluções e abra o arquivo MoviesDBModel.Designer.cs no Editor de código (consulte a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="edcdc-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.cs file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="edcdc-146">[![O código para a entidade de filme](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="edcdc-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)</span></span>

<span data-ttu-id="edcdc-147">**Figura 02**: O código para a entidade de filme ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="edcdc-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))</span></span>


<span data-ttu-id="edcdc-148">A classe de filme é uma classe parcial.</span><span class="sxs-lookup"><span data-stu-id="edcdc-148">The Movie class is a partial class.</span></span> <span data-ttu-id="edcdc-149">Isso significa que podemos adicionar outra classe parcial com o mesmo nome para estender a funcionalidade da classe filme.</span><span class="sxs-lookup"><span data-stu-id="edcdc-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="edcdc-150">Vamos adicionar nossa lógica de validação para a nova classe parcial.</span><span class="sxs-lookup"><span data-stu-id="edcdc-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="edcdc-151">Adicione a classe na lista 2 para a pasta de modelos.</span><span class="sxs-lookup"><span data-stu-id="edcdc-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="edcdc-152">**A listagem 2 - Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="edcdc-152">**Listing 2 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

<span data-ttu-id="edcdc-153">Observe que a classe na lista 2 inclui a *parcial* modificador.</span><span class="sxs-lookup"><span data-stu-id="edcdc-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="edcdc-154">Quaisquer métodos ou propriedades que você adicionar a essa classe se tornam parte da classe de filme gerado pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="edcdc-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="edcdc-155">Adicionando OnChanging e métodos OnChanged Partial</span><span class="sxs-lookup"><span data-stu-id="edcdc-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="edcdc-156">Quando o Entity Framework gera uma classe de entidade, o Entity Framework adiciona métodos parciais para a classe automaticamente.</span><span class="sxs-lookup"><span data-stu-id="edcdc-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="edcdc-157">O Entity Framework gera OnChanging e OnChanged métodos parciais que correspondem a cada propriedade da classe.</span><span class="sxs-lookup"><span data-stu-id="edcdc-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="edcdc-158">No caso da classe de filme, o Entity Framework cria os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="edcdc-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="edcdc-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="edcdc-159">OnIdChanging</span></span>
- <span data-ttu-id="edcdc-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="edcdc-160">OnIdChanged</span></span>
- <span data-ttu-id="edcdc-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="edcdc-161">OnTitleChanging</span></span>
- <span data-ttu-id="edcdc-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="edcdc-162">OnTitleChanged</span></span>
- <span data-ttu-id="edcdc-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="edcdc-163">OnDirectorChanging</span></span>
- <span data-ttu-id="edcdc-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="edcdc-164">OnDirectorChanged</span></span>
- <span data-ttu-id="edcdc-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="edcdc-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="edcdc-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="edcdc-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="edcdc-167">O método OnChanging é chamado correto antes da propriedade correspondente é alterada.</span><span class="sxs-lookup"><span data-stu-id="edcdc-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="edcdc-168">O método OnChanged é chamado direita depois que a propriedade é alterada.</span><span class="sxs-lookup"><span data-stu-id="edcdc-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="edcdc-169">Você pode tirar proveito desses métodos parciais para adicionar lógica de validação para a classe do filme.</span><span class="sxs-lookup"><span data-stu-id="edcdc-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="edcdc-170">A classe de filme na listagem 3 de atualização verifica que as propriedades Title e diretor são atribuídas valores não vazios.</span><span class="sxs-lookup"><span data-stu-id="edcdc-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="edcdc-171">Um método parcial é um método definido em uma classe que não é necessário para implementar.</span><span class="sxs-lookup"><span data-stu-id="edcdc-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="edcdc-172">Se você não implementa um método parcial, em seguida, o compilador remove a assinatura do método e todas as chamadas para o método para que estão sem qualquer custo de tempo de execução associado com o método parcial.</span><span class="sxs-lookup"><span data-stu-id="edcdc-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="edcdc-173">No código de Editor do Visual Studio, você pode adicionar um método parcial, digitando a palavra-chave *parcial* seguido por um espaço para exibir uma lista de existe meio-termo para implementar.</span><span class="sxs-lookup"><span data-stu-id="edcdc-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="edcdc-174">**A listagem 3 - Models\Movie.cs**</span><span class="sxs-lookup"><span data-stu-id="edcdc-174">**Listing 3 - Models\Movie.cs**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

<span data-ttu-id="edcdc-175">Por exemplo, se você tentar atribuir uma cadeia de caracteres vazia para a propriedade Title, em seguida, uma mensagem de erro é atribuída a um dicionário, chamado \_erros.</span><span class="sxs-lookup"><span data-stu-id="edcdc-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="edcdc-176">Neste ponto, nada acontece mesmo quando você atribui uma cadeia de caracteres vazia para a propriedade de título e um erro será adicionado à particular \_campo de erros.</span><span class="sxs-lookup"><span data-stu-id="edcdc-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="edcdc-177">É necessário implementar a interface IDataErrorInfo para expor esses erros de validação para a estrutura ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="edcdc-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="edcdc-178">Implementando a Interface IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="edcdc-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="edcdc-179">A interface IDataErrorInfo foi parte do .NET framework desde a primeira versão.</span><span class="sxs-lookup"><span data-stu-id="edcdc-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="edcdc-180">Esta é uma interface muito simple:</span><span class="sxs-lookup"><span data-stu-id="edcdc-180">This interface is a very simple interface:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

<span data-ttu-id="edcdc-181">Se uma classe implementa a interface IDataErrorInfo, a estrutura ASP.NET MVC usará essa interface ao criar uma instância da classe.</span><span class="sxs-lookup"><span data-stu-id="edcdc-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="edcdc-182">Por exemplo, o controlador Home ação Create () aceita uma instância da classe filme:</span><span class="sxs-lookup"><span data-stu-id="edcdc-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

<span data-ttu-id="edcdc-183">A estrutura ASP.NET MVC cria a instância do filme passado para a ação Create () usando um associador de modelo (o DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="edcdc-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="edcdc-184">O associador de modelo é responsável pela criação de uma instância do objeto do filme ao associar os campos de formulário HTML para uma instância do objeto de filme.</span><span class="sxs-lookup"><span data-stu-id="edcdc-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="edcdc-185">O DefaultModelBinder detecta se uma classe implementa a interface IDataErrorInfo ou não.</span><span class="sxs-lookup"><span data-stu-id="edcdc-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="edcdc-186">Se uma classe implementa essa interface o associador de modelo invoca o indexador IDataErrorInfo.this para cada propriedade da classe.</span><span class="sxs-lookup"><span data-stu-id="edcdc-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="edcdc-187">Se o indexador retorna uma mensagem de erro o associador de modelo adiciona essa mensagem de erro para modelar o estado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="edcdc-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="edcdc-188">O DefaultModelBinder também verifica a propriedade IDataErrorInfo.Error.</span><span class="sxs-lookup"><span data-stu-id="edcdc-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="edcdc-189">Essa propriedade é serve para representar os erros de validação específicos de propriedade não associados à classe.</span><span class="sxs-lookup"><span data-stu-id="edcdc-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="edcdc-190">Por exemplo, você talvez queira aplicar uma regra de validação que depende dos valores de várias propriedades da classe filme.</span><span class="sxs-lookup"><span data-stu-id="edcdc-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="edcdc-191">Nesse caso, você retornará um erro de validação da propriedade de erro.</span><span class="sxs-lookup"><span data-stu-id="edcdc-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="edcdc-192">A classe de filme atualizada na listagem 4 implementa a interface IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="edcdc-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="edcdc-193">**A listagem 4 - Models\Movie.cs (implementa IDataErrorInfo)**</span><span class="sxs-lookup"><span data-stu-id="edcdc-193">**Listing 4 - Models\Movie.cs (implements IDataErrorInfo)**</span></span>

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

<span data-ttu-id="edcdc-194">Na listagem 4, verifica a propriedade do indexador a \_coleção de erros para ver se ele contém uma chave que corresponde ao nome da propriedade é passado para o indexador.</span><span class="sxs-lookup"><span data-stu-id="edcdc-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="edcdc-195">Se não houver nenhum erro de validação associado com a propriedade é retornada uma cadeia de caracteres vazia.</span><span class="sxs-lookup"><span data-stu-id="edcdc-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="edcdc-196">Você não precisa modificar o controlador inicial de qualquer forma ao usar a classe de filme modificada.</span><span class="sxs-lookup"><span data-stu-id="edcdc-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="edcdc-197">A página exibida na Figura 3 ilustra o que acontece quando nenhum valor for inserido para os campos de formulário título ou Director.</span><span class="sxs-lookup"><span data-stu-id="edcdc-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="edcdc-198">[![Criação automática de métodos de ação](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="edcdc-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)</span></span>

<span data-ttu-id="edcdc-199">**Figura 03**: um formulário com valores ausentes ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="edcdc-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))</span></span>


<span data-ttu-id="edcdc-200">Observe que o valor de DateReleased é validada automaticamente.</span><span class="sxs-lookup"><span data-stu-id="edcdc-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="edcdc-201">Porque a propriedade DateReleased não aceita valores NULL, o DefaultModelBinder gera um erro de validação para esta propriedade automaticamente quando ele não tem um valor.</span><span class="sxs-lookup"><span data-stu-id="edcdc-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="edcdc-202">Se você quiser modificar a mensagem de erro para a propriedade DateReleased, em seguida, você precisa criar um associador de modelo personalizado.</span><span class="sxs-lookup"><span data-stu-id="edcdc-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="edcdc-203">Resumo</span><span class="sxs-lookup"><span data-stu-id="edcdc-203">Summary</span></span>

<span data-ttu-id="edcdc-204">Neste tutorial, você aprendeu a usar a interface IDataErrorInfo para gerar mensagens de erro de validação.</span><span class="sxs-lookup"><span data-stu-id="edcdc-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="edcdc-205">Primeiro, criamos uma classe parcial de filme que estende a funcionalidade da classe filme parcial gerada pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="edcdc-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="edcdc-206">Em seguida, adicionamos a lógica de validação para os filme classe OnTitleChanging() e OnDirectorChanging() métodos parciais.</span><span class="sxs-lookup"><span data-stu-id="edcdc-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="edcdc-207">Por fim, implementamos a interface IDataErrorInfo para expor essas mensagens de validação para a estrutura ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="edcdc-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="edcdc-208">[Anterior](performing-simple-validation-cs.md)
[Próximo](validating-with-a-service-layer-cs.md)</span><span class="sxs-lookup"><span data-stu-id="edcdc-208">[Previous](performing-simple-validation-cs.md)
[Next](validating-with-a-service-layer-cs.md)</span></span>
