---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
title: Validação com a Interface IDataErrorInfo (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther mostra como exibir mensagens de erro de validação personalizada, Implementando a interface IDataErrorInfo em uma classe de modelo.
ms.author: riande
ms.date: 03/02/2009
ms.assetid: 3a8a9d9f-82dd-4959-b7c6-960e9ce95df1
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: e9b989d0110c3947583fd70bd38b29dcb2bb5c31
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832445"
---
<a name="validating-with-the-idataerrorinfo-interface-vb"></a><span data-ttu-id="b1324-103">Validação com a Interface IDataErrorInfo (VB)</span><span class="sxs-lookup"><span data-stu-id="b1324-103">Validating with the IDataErrorInfo Interface (VB)</span></span>
====================
<span data-ttu-id="b1324-104">por [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="b1324-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="b1324-105">Stephen Walther mostra como exibir mensagens de erro de validação personalizada, Implementando a interface IDataErrorInfo em uma classe de modelo.</span><span class="sxs-lookup"><span data-stu-id="b1324-105">Stephen Walther shows you how to display custom validation error messages by implementing the IDataErrorInfo interface in a model class.</span></span>


<span data-ttu-id="b1324-106">O objetivo deste tutorial é explicar uma abordagem para executar a validação em um aplicativo ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b1324-106">The goal of this tutorial is to explain one approach to performing validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="b1324-107">Você aprenderá a evitar que alguém envie um formulário HTML sem fornecer valores para campos de formulário necessária.</span><span class="sxs-lookup"><span data-stu-id="b1324-107">You learn how to prevent someone from submitting an HTML form without providing values for required form fields.</span></span> <span data-ttu-id="b1324-108">Neste tutorial, você aprenderá a executar a validação usando a interface IErrorDataInfo.</span><span class="sxs-lookup"><span data-stu-id="b1324-108">In this tutorial, you learn how to perform validation by using the IErrorDataInfo interface.</span></span>

## <a name="assumptions"></a><span data-ttu-id="b1324-109">Suposições</span><span class="sxs-lookup"><span data-stu-id="b1324-109">Assumptions</span></span>

<span data-ttu-id="b1324-110">Neste tutorial, usarei o banco de dados MoviesDB e a tabela de banco de dados de filmes.</span><span class="sxs-lookup"><span data-stu-id="b1324-110">In this tutorial, I'll use the MoviesDB database and the Movies database table.</span></span> <span data-ttu-id="b1324-111">Esta tabela tem as seguintes colunas:</span><span class="sxs-lookup"><span data-stu-id="b1324-111">This table has the following columns:</span></span>

<a id="0.6_table01"></a>


| <span data-ttu-id="b1324-112">**Nome da coluna**</span><span class="sxs-lookup"><span data-stu-id="b1324-112">**Column Name**</span></span> | <span data-ttu-id="b1324-113">**Tipo de dados**</span><span class="sxs-lookup"><span data-stu-id="b1324-113">**Data Type**</span></span> | <span data-ttu-id="b1324-114">**Permitir nulos**</span><span class="sxs-lookup"><span data-stu-id="b1324-114">**Allow Nulls**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="b1324-115">Id</span><span class="sxs-lookup"><span data-stu-id="b1324-115">Id</span></span> | <span data-ttu-id="b1324-116">int</span><span class="sxs-lookup"><span data-stu-id="b1324-116">Int</span></span> | <span data-ttu-id="b1324-117">False</span><span class="sxs-lookup"><span data-stu-id="b1324-117">False</span></span> |
| <span data-ttu-id="b1324-118">Título</span><span class="sxs-lookup"><span data-stu-id="b1324-118">Title</span></span> | <span data-ttu-id="b1324-119">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="b1324-119">Nvarchar(100)</span></span> | <span data-ttu-id="b1324-120">False</span><span class="sxs-lookup"><span data-stu-id="b1324-120">False</span></span> |
| <span data-ttu-id="b1324-121">Diretor</span><span class="sxs-lookup"><span data-stu-id="b1324-121">Director</span></span> | <span data-ttu-id="b1324-122">Nvarchar(100)</span><span class="sxs-lookup"><span data-stu-id="b1324-122">Nvarchar(100)</span></span> | <span data-ttu-id="b1324-123">False</span><span class="sxs-lookup"><span data-stu-id="b1324-123">False</span></span> |
| <span data-ttu-id="b1324-124">DateReleased</span><span class="sxs-lookup"><span data-stu-id="b1324-124">DateReleased</span></span> | <span data-ttu-id="b1324-125">DateTime</span><span class="sxs-lookup"><span data-stu-id="b1324-125">DateTime</span></span> | <span data-ttu-id="b1324-126">False</span><span class="sxs-lookup"><span data-stu-id="b1324-126">False</span></span> |


<span data-ttu-id="b1324-127">Neste tutorial, posso usar o Microsoft Entity Framework para gerar minhas classes de modelo de banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b1324-127">In this tutorial, I use the Microsoft Entity Framework to generate my database model classes.</span></span> <span data-ttu-id="b1324-128">A classe Movie gerada pelo Entity Framework é exibida na Figura 1.</span><span class="sxs-lookup"><span data-stu-id="b1324-128">The Movie class generated by the Entity Framework is displayed in Figure 1.</span></span>


<span data-ttu-id="b1324-129">[![A entidade de filme](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b1324-129">[![The Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image1.png)</span></span>

<span data-ttu-id="b1324-130">**Figura 01**: entidade o filme ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b1324-130">**Figure 01**: The Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image2.png))</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="b1324-131">Para saber mais sobre como usar o Entity Framework para gerar classes de modelo de banco de dados, consulte a que minha tutorial o direito de criar Classes de modelo com o Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b1324-131">To learn more about using the Entity Framework to generate your database model classes, see my tutorial entitled Creating Model Classes with the Entity Framework.</span></span>


## <a name="the-controller-class"></a><span data-ttu-id="b1324-132">A classe do controlador</span><span class="sxs-lookup"><span data-stu-id="b1324-132">The Controller Class</span></span>

<span data-ttu-id="b1324-133">Usamos o controlador Home filmes de lista e criar novos filmes.</span><span class="sxs-lookup"><span data-stu-id="b1324-133">We use the Home controller to list movies and create new movies.</span></span> <span data-ttu-id="b1324-134">O código para essa classe está contido na listagem 1.</span><span class="sxs-lookup"><span data-stu-id="b1324-134">The code for this class is contained in Listing 1.</span></span>

<span data-ttu-id="b1324-135">**Listagem 1 - Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="b1324-135">**Listing 1 - Controllers\HomeController.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample1.vb)]

<span data-ttu-id="b1324-136">A classe de controlador Home na listagem 1 contém duas ações Create ().</span><span class="sxs-lookup"><span data-stu-id="b1324-136">The Home controller class in Listing 1 contains two Create() actions.</span></span> <span data-ttu-id="b1324-137">A primeira ação exibe o formulário HTML para a criação de um novo filme.</span><span class="sxs-lookup"><span data-stu-id="b1324-137">The first action displays the HTML form for creating a new movie.</span></span> <span data-ttu-id="b1324-138">A segunda ação Create () executa a inserção real do novo filme no banco de dados.</span><span class="sxs-lookup"><span data-stu-id="b1324-138">The second Create() action performs the actual insert of the new movie into the database.</span></span> <span data-ttu-id="b1324-139">A segunda ação Create () é invocada quando o formulário exibido pela primeira ação Create () é enviado ao servidor.</span><span class="sxs-lookup"><span data-stu-id="b1324-139">The second Create() action is invoked when the form displayed by the first Create() action is submitted to the server.</span></span>

<span data-ttu-id="b1324-140">Observe que a segunda ação Create () contém as seguintes linhas de código:</span><span class="sxs-lookup"><span data-stu-id="b1324-140">Notice that the second Create() action contains the following lines of code:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample2.vb)]

<span data-ttu-id="b1324-141">A propriedade IsValid retorna false quando há um erro de validação.</span><span class="sxs-lookup"><span data-stu-id="b1324-141">The IsValid property returns false when there is a validation error.</span></span> <span data-ttu-id="b1324-142">Nesse caso, o modo de exibição de criação que contém o formulário HTML para a criação de um filme é exibida novamente.</span><span class="sxs-lookup"><span data-stu-id="b1324-142">In that case, the Create view that contains the HTML form for creating a movie is redisplayed.</span></span>

## <a name="creating-a-partial-class"></a><span data-ttu-id="b1324-143">Criar uma classe parcial</span><span class="sxs-lookup"><span data-stu-id="b1324-143">Creating a Partial Class</span></span>

<span data-ttu-id="b1324-144">A classe de filme é gerada pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b1324-144">The Movie class is generated by the Entity Framework.</span></span> <span data-ttu-id="b1324-145">Você pode ver o código para a classe de filme, se você expandir o arquivo MoviesDBModel.edmx na janela do Gerenciador de soluções e abra o arquivo de MoviesDBModel.Designer.vb no Editor de códigos (veja a Figura 2).</span><span class="sxs-lookup"><span data-stu-id="b1324-145">You can see the code for the Movie class if you expand the MoviesDBModel.edmx file in the Solution Explorer window and open the MoviesDBModel.Designer.vb file in the Code Editor (see Figure 2).</span></span>


<span data-ttu-id="b1324-146">[![O código para a entidade de filme](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b1324-146">[![The code for the Movie entity](validating-with-the-idataerrorinfo-interface-vb/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image3.png)</span></span>

<span data-ttu-id="b1324-147">**Figura 02**: O código para a entidade de filme ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="b1324-147">**Figure 02**: The code for the Movie entity([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image4.png))</span></span>


<span data-ttu-id="b1324-148">A classe de filme é uma classe parcial.</span><span class="sxs-lookup"><span data-stu-id="b1324-148">The Movie class is a partial class.</span></span> <span data-ttu-id="b1324-149">Isso significa que podemos adicionar outra classe parcial com o mesmo nome para estender a funcionalidade da classe filme.</span><span class="sxs-lookup"><span data-stu-id="b1324-149">That means that we can add another partial class with the same name to extend the functionality of the Movie class.</span></span> <span data-ttu-id="b1324-150">Vamos adicionar nossa lógica de validação para a nova classe parcial.</span><span class="sxs-lookup"><span data-stu-id="b1324-150">We'll add our validation logic to the new partial class.</span></span>

<span data-ttu-id="b1324-151">Adicione a classe na listagem 2 para a pasta de modelos.</span><span class="sxs-lookup"><span data-stu-id="b1324-151">Add the class in Listing 2 to the Models folder.</span></span>

<span data-ttu-id="b1324-152">**Listagem 2 - Models\Movie.vb**</span><span class="sxs-lookup"><span data-stu-id="b1324-152">**Listing 2 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample3.vb)]

<span data-ttu-id="b1324-153">Observe que a classe na listagem 2 inclui o *parcial* modificador.</span><span class="sxs-lookup"><span data-stu-id="b1324-153">Notice that the class in Listing 2 includes the *partial* modifier.</span></span> <span data-ttu-id="b1324-154">Todos os métodos ou propriedades que você adiciona a essa classe se tornam parte da classe Movie gerada pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b1324-154">Any methods or properties that you add to this class become part of the Movie class generated by the Entity Framework.</span></span>

## <a name="adding-onchanging-and-onchanged-partial-methods"></a><span data-ttu-id="b1324-155">Adicionando OnChanging e métodos de alguns OnChanged parcial</span><span class="sxs-lookup"><span data-stu-id="b1324-155">Adding OnChanging and OnChanged Partial Methods</span></span>

<span data-ttu-id="b1324-156">Quando o Entity Framework gera uma classe de entidade, o Entity Framework adiciona métodos parciais para a classe automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b1324-156">When the Entity Framework generates an entity class, the Entity Framework adds partial methods to the class automatically.</span></span> <span data-ttu-id="b1324-157">O Entity Framework gera OnChanging e alguns OnChanged métodos parciais que correspondem a cada propriedade da classe.</span><span class="sxs-lookup"><span data-stu-id="b1324-157">The Entity Framework generates OnChanging and OnChanged partial methods that correspond to each property of the class.</span></span>

<span data-ttu-id="b1324-158">No caso da classe de filme, o Entity Framework cria os seguintes métodos:</span><span class="sxs-lookup"><span data-stu-id="b1324-158">In the case of the Movie class, the Entity Framework creates the following methods:</span></span>

- <span data-ttu-id="b1324-159">OnIdChanging</span><span class="sxs-lookup"><span data-stu-id="b1324-159">OnIdChanging</span></span>
- <span data-ttu-id="b1324-160">OnIdChanged</span><span class="sxs-lookup"><span data-stu-id="b1324-160">OnIdChanged</span></span>
- <span data-ttu-id="b1324-161">OnTitleChanging</span><span class="sxs-lookup"><span data-stu-id="b1324-161">OnTitleChanging</span></span>
- <span data-ttu-id="b1324-162">OnTitleChanged</span><span class="sxs-lookup"><span data-stu-id="b1324-162">OnTitleChanged</span></span>
- <span data-ttu-id="b1324-163">OnDirectorChanging</span><span class="sxs-lookup"><span data-stu-id="b1324-163">OnDirectorChanging</span></span>
- <span data-ttu-id="b1324-164">OnDirectorChanged</span><span class="sxs-lookup"><span data-stu-id="b1324-164">OnDirectorChanged</span></span>
- <span data-ttu-id="b1324-165">OnDateReleasedChanging</span><span class="sxs-lookup"><span data-stu-id="b1324-165">OnDateReleasedChanging</span></span>
- <span data-ttu-id="b1324-166">OnDateReleasedChanged</span><span class="sxs-lookup"><span data-stu-id="b1324-166">OnDateReleasedChanged</span></span>

<span data-ttu-id="b1324-167">O método OnChanging é chamado correto antes da propriedade correspondente é alterada.</span><span class="sxs-lookup"><span data-stu-id="b1324-167">The OnChanging method is called right before the corresponding property is changed.</span></span> <span data-ttu-id="b1324-168">O método de alguns OnChanged é chamado direita depois que a propriedade é alterada.</span><span class="sxs-lookup"><span data-stu-id="b1324-168">The OnChanged method is called right after the property is changed.</span></span>

<span data-ttu-id="b1324-169">Você pode tirar proveito desses métodos parciais para adicionar lógica de validação para a classe de filme.</span><span class="sxs-lookup"><span data-stu-id="b1324-169">You can take advantage of these partial methods to add validation logic to the Movie class.</span></span> <span data-ttu-id="b1324-170">A classe de filme na listagem 3 de atualização verifica que as propriedades Title e diretor recebem valores não vazios.</span><span class="sxs-lookup"><span data-stu-id="b1324-170">The update Movie class in Listing 3 verifies that the Title and Director properties are assigned nonempty values.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b1324-171">Um método parcial é um método definido em uma classe que não são necessários para implementar.</span><span class="sxs-lookup"><span data-stu-id="b1324-171">A partial method is a method defined in a class that you are not required to implement.</span></span> <span data-ttu-id="b1324-172">Se você não implementar um método parcial, em seguida, o compilador removerá a assinatura do método e todas as chamadas para o método, então, aqui estão sem custos de tempo de execução associados ao método parcial.</span><span class="sxs-lookup"><span data-stu-id="b1324-172">If you don't implement a partial method then the compiler removes the method signature and all calls to the method so there are no run-time costs associated with the partial method.</span></span> <span data-ttu-id="b1324-173">No código de Editor do Visual Studio, você pode adicionar um método parcial, digitando a palavra-chave *parcial* seguido por um espaço para exibir uma lista de parciais para implementar.</span><span class="sxs-lookup"><span data-stu-id="b1324-173">In the Visual Studio Code Editor, you can add a partial method by typing the keyword *partial* followed by a space to view a list of partials to implement.</span></span>


<span data-ttu-id="b1324-174">**Listagem 3 - Models\Movie.vb**</span><span class="sxs-lookup"><span data-stu-id="b1324-174">**Listing 3 - Models\Movie.vb**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample4.vb)]

<span data-ttu-id="b1324-175">Por exemplo, se você tentar atribuir uma cadeia de caracteres vazia para a propriedade de título, em seguida, uma mensagem de erro é atribuída a um dicionário chamado \_erros.</span><span class="sxs-lookup"><span data-stu-id="b1324-175">For example, if you attempt to assign an empty string to the Title property, then an error message is assigned to a Dictionary named \_errors.</span></span>

<span data-ttu-id="b1324-176">Neste ponto, nada realmente acontece quando você atribuir uma cadeia de caracteres vazia para a propriedade de título e um erro é adicionado ao particular \_campo de erros.</span><span class="sxs-lookup"><span data-stu-id="b1324-176">At this point, nothing actually happens when you assign an empty string to the Title property and an error is added to the private \_errors field.</span></span> <span data-ttu-id="b1324-177">É necessário implementar a interface IDataErrorInfo para expor esses erros de validação para a estrutura ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="b1324-177">We need to implement the IDataErrorInfo interface to expose these validation errors to the ASP.NET MVC framework.</span></span>

## <a name="implementing-the-idataerrorinfo-interface"></a><span data-ttu-id="b1324-178">Implementando a Interface IDataErrorInfo</span><span class="sxs-lookup"><span data-stu-id="b1324-178">Implementing the IDataErrorInfo Interface</span></span>

<span data-ttu-id="b1324-179">A interface IDataErrorInfo tem sido parte do .NET framework desde a primeira versão.</span><span class="sxs-lookup"><span data-stu-id="b1324-179">The IDataErrorInfo interface has been part of the .NET framework since the first version.</span></span> <span data-ttu-id="b1324-180">Esta é uma interface muito simple:</span><span class="sxs-lookup"><span data-stu-id="b1324-180">This interface is a very simple interface:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample5.vb)]

<span data-ttu-id="b1324-181">Se uma classe implementa a interface IDataErrorInfo, o ASP.NET MVC framework usará essa interface ao criar uma instância da classe.</span><span class="sxs-lookup"><span data-stu-id="b1324-181">If a class implements the IDataErrorInfo interface, the ASP.NET MVC framework will use this interface when creating an instance of the class.</span></span> <span data-ttu-id="b1324-182">Por exemplo, o controlador Home ação Create () aceita uma instância da classe filme:</span><span class="sxs-lookup"><span data-stu-id="b1324-182">For example, the Home controller Create() action accepts an instance of the Movie class:</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample6.vb)]

<span data-ttu-id="b1324-183">O ASP.NET MVC framework cria a instância do filme passado para a ação Create () usando um associador de modelo (o DefaultModelBinder).</span><span class="sxs-lookup"><span data-stu-id="b1324-183">The ASP.NET MVC framework creates the instance of the Movie passed to the Create() action by using a model binder (the DefaultModelBinder).</span></span> <span data-ttu-id="b1324-184">O associador de modelo é responsável por criar uma instância do objeto de filme, associando os campos de formulário HTML a uma instância do objeto de filme.</span><span class="sxs-lookup"><span data-stu-id="b1324-184">The model binder is responsible for creating an instance of the Movie object by binding the HTML form fields to an instance of the Movie object.</span></span>

<span data-ttu-id="b1324-185">O DefaultModelBinder detecta se uma classe implementa a interface IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="b1324-185">The DefaultModelBinder detects whether or not a class implements the IDataErrorInfo interface.</span></span> <span data-ttu-id="b1324-186">Se uma classe implementa essa interface, em seguida, o associador de modelo invoca o indexador IDataErrorInfo.this para cada propriedade da classe.</span><span class="sxs-lookup"><span data-stu-id="b1324-186">If a class implements this interface then the model binder invokes the IDataErrorInfo.this indexer for each property of the class.</span></span> <span data-ttu-id="b1324-187">Se o indexador retorna que uma mensagem de erro, em seguida, o associador de modelos adiciona essa mensagem de erro para modelar o estado automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b1324-187">If the indexer returns an error message then the model binder adds this error message to model state automatically.</span></span>

<span data-ttu-id="b1324-188">O DefaultModelBinder também verifica a propriedade IDataErrorInfo.Error.</span><span class="sxs-lookup"><span data-stu-id="b1324-188">The DefaultModelBinder also checks the IDataErrorInfo.Error property.</span></span> <span data-ttu-id="b1324-189">Esta propriedade destina-se para representar erros de validação específica da propriedade não associados à classe.</span><span class="sxs-lookup"><span data-stu-id="b1324-189">This property is intended to represent non-property specific validation errors associated with the class.</span></span> <span data-ttu-id="b1324-190">Por exemplo, você talvez queira aplicar uma regra de validação que depende dos valores de várias propriedades da classe filme.</span><span class="sxs-lookup"><span data-stu-id="b1324-190">For example, you might want to enforce a validation rule that depends on the values of multiple properties of the Movie class.</span></span> <span data-ttu-id="b1324-191">Nesse caso, retornaria um erro de validação da propriedade de erro.</span><span class="sxs-lookup"><span data-stu-id="b1324-191">In that case, you would return a validation error from the Error property.</span></span>

<span data-ttu-id="b1324-192">A classe Movie atualizada na listagem 4 implementa a interface IDataErrorInfo.</span><span class="sxs-lookup"><span data-stu-id="b1324-192">The updated Movie class in Listing 4 implements the IDataErrorInfo interface.</span></span>

<span data-ttu-id="b1324-193">**Listagem 4 - Models\Movie.vb (implementa IDataErrorInfo)**</span><span class="sxs-lookup"><span data-stu-id="b1324-193">**Listing 4 - Models\Movie.vb (implements IDataErrorInfo)**</span></span>

[!code-vb[Main](validating-with-the-idataerrorinfo-interface-vb/samples/sample7.vb)]

<span data-ttu-id="b1324-194">Na listagem 4, verifica a propriedade do indexador a \_coleção de erros para ver se ele contém uma chave que corresponde ao nome da propriedade é passado para o indexador.</span><span class="sxs-lookup"><span data-stu-id="b1324-194">In Listing 4, the indexer property checks the \_errors collection to see if it contains a key that corresponds to the property name passed to the indexer.</span></span> <span data-ttu-id="b1324-195">Se não houver nenhum erro de validação associado à propriedade, uma cadeia de caracteres vazia será retornada.</span><span class="sxs-lookup"><span data-stu-id="b1324-195">If there is no validation error associated with the property then an empty string is returned.</span></span>

<span data-ttu-id="b1324-196">Você não precisa modificar o controlador inicial de qualquer forma ao usar a classe Movie modificada.</span><span class="sxs-lookup"><span data-stu-id="b1324-196">You don't need to modify the Home controller in any way to use the modified Movie class.</span></span> <span data-ttu-id="b1324-197">A página exibida na Figura 3 ilustra o que acontece quando nenhum valor for inserido para os campos de título ou diretor do formulário.</span><span class="sxs-lookup"><span data-stu-id="b1324-197">The page displayed in Figure 3 illustrates what happens when no value is entered for the Title or Director form fields.</span></span>


<span data-ttu-id="b1324-198">[![Criação automática de métodos de ação](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="b1324-198">[![Creating action methods automatically](validating-with-the-idataerrorinfo-interface-vb/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-vb/_static/image5.png)</span></span>

<span data-ttu-id="b1324-199">**Figura 03**: um formulário com valores ausentes ([clique para exibir a imagem em tamanho normal](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b1324-199">**Figure 03**: A form with missing values ([Click to view full-size image](validating-with-the-idataerrorinfo-interface-vb/_static/image6.png))</span></span>


<span data-ttu-id="b1324-200">Observe que o valor de DateReleased é validada automaticamente.</span><span class="sxs-lookup"><span data-stu-id="b1324-200">Notice that the DateReleased value is validated automatically.</span></span> <span data-ttu-id="b1324-201">Porque a propriedade DateReleased não aceita valores NULL, o DefaultModelBinder gera um erro de validação para essa propriedade automaticamente quando ele não tem um valor.</span><span class="sxs-lookup"><span data-stu-id="b1324-201">Because the DateReleased property does not accept NULL values, the DefaultModelBinder generates a validation error for this property automatically when it does not have a value.</span></span> <span data-ttu-id="b1324-202">Se você quiser modificar a mensagem de erro para a propriedade DateReleased, em seguida, você precisa criar um associador de modelo personalizado.</span><span class="sxs-lookup"><span data-stu-id="b1324-202">If you want to modify the error message for the DateReleased property then you need to create a custom model binder.</span></span>

## <a name="summary"></a><span data-ttu-id="b1324-203">Resumo</span><span class="sxs-lookup"><span data-stu-id="b1324-203">Summary</span></span>

<span data-ttu-id="b1324-204">Neste tutorial, você aprendeu como usar a interface IDataErrorInfo para gerar mensagens de erro de validação.</span><span class="sxs-lookup"><span data-stu-id="b1324-204">In this tutorial, you learned how to use the IDataErrorInfo interface to generate validation error messages.</span></span> <span data-ttu-id="b1324-205">Primeiro, criamos uma classe parcial do filme que estende a funcionalidade da classe parcial do filme gerada pelo Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b1324-205">First, we created a partial Movie class that extends the functionality of the partial Movie class generated by the Entity Framework.</span></span> <span data-ttu-id="b1324-206">Em seguida, adicionamos lógica de validação ao filme métodos da classe OnTitleChanging() e OnDirectorChanging() parciais.</span><span class="sxs-lookup"><span data-stu-id="b1324-206">Next, we added validation logic to the Movie class OnTitleChanging() and OnDirectorChanging() partial methods.</span></span> <span data-ttu-id="b1324-207">Por fim, implementamos a interface IDataErrorInfo para expor essas mensagens de validação para a estrutura MVC do ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="b1324-207">Finally, we implemented the IDataErrorInfo interface in order to expose these validation messages to the ASP.NET MVC framework.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b1324-208">[Anterior](performing-simple-validation-vb.md)
> [Próximo](validating-with-a-service-layer-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b1324-208">[Previous](performing-simple-validation-vb.md)
[Next](validating-with-a-service-layer-vb.md)</span></span>
