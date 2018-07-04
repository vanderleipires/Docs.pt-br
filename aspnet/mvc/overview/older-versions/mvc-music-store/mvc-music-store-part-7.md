---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: Associação e a autorização | Microsoft Docs'
author: jongalloway
description: Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC. Parte 7 aborda a associação e a autorização.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 41b17315cbe1f6d93001a736bc24bf003df24061
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393164"
---
<a name="part-7-membership-and-authorization"></a><span data-ttu-id="88a91-104">Parte 7: Associação e a autorização</span><span class="sxs-lookup"><span data-stu-id="88a91-104">Part 7: Membership and Authorization</span></span>
====================
<span data-ttu-id="88a91-105">por [Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="88a91-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="88a91-106">A Store de música do MVC é um aplicativo tutorial que apresenta e explica passo a passo de como usar o ASP.NET MVC e o Visual Studio para desenvolvimento da web.</span><span class="sxs-lookup"><span data-stu-id="88a91-106">The MVC Music Store is a tutorial application that introduces and explains step-by-step how to use ASP.NET MVC and Visual Studio for web development.</span></span>  
>   
> <span data-ttu-id="88a91-107">A Store de música do MVC é uma implementação de repositório de exemplo leve que vende álbuns de música online e implementa a administração de site básico, entrada do usuário e a funcionalidade de carrinho de compras.</span><span class="sxs-lookup"><span data-stu-id="88a91-107">The MVC Music Store is a lightweight sample store implementation which sells music albums online, and implements basic site administration, user sign-in, and shopping cart functionality.</span></span>  
>   
> <span data-ttu-id="88a91-108">Esta série de tutoriais fornece detalhes sobre todas as etapas realizadas para compilar o aplicativo de exemplo de Store de música do ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="88a91-108">This tutorial series details all of the steps taken to build the ASP.NET MVC Music Store sample application.</span></span> <span data-ttu-id="88a91-109">Parte 7 aborda a associação e a autorização.</span><span class="sxs-lookup"><span data-stu-id="88a91-109">Part 7 covers Membership and Authorization.</span></span>


<span data-ttu-id="88a91-110">Nosso controlador Store Manager é acessível no momento para qualquer pessoa que visitar nosso site.</span><span class="sxs-lookup"><span data-stu-id="88a91-110">Our Store Manager controller is currently accessible to anyone visiting our site.</span></span> <span data-ttu-id="88a91-111">Vamos alterar isso para restringir a permissão para administradores de site.</span><span class="sxs-lookup"><span data-stu-id="88a91-111">Let's change this to restrict permission to site administrators.</span></span>

## <a name="adding-the-accountcontroller-and-views"></a><span data-ttu-id="88a91-112">Adicionando o AccountController e modos de exibição</span><span class="sxs-lookup"><span data-stu-id="88a91-112">Adding the AccountController and Views</span></span>

<span data-ttu-id="88a91-113">Uma diferença entre o modelo completo do aplicativo Web do ASP.NET MVC 3 e o modelo de aplicativo de Web vazio do ASP.NET MVC 3 é que o modelo vazio não inclui um controlador de conta.</span><span class="sxs-lookup"><span data-stu-id="88a91-113">One difference between the full ASP.NET MVC 3 Web Application template and the ASP.NET MVC 3 Empty Web Application template is that the empty template doesn't include an Account Controller.</span></span> <span data-ttu-id="88a91-114">Vamos adicionar um controlador de conta, copiando alguns arquivos de um novo aplicativo ASP.NET MVC criado usando o modelo completo do aplicativo Web do ASP.NET MVC 3.</span><span class="sxs-lookup"><span data-stu-id="88a91-114">We'll add an Account Controller by copying a few files from a new ASP.NET MVC application created from the full ASP.NET MVC 3 Web Application template.</span></span>

<span data-ttu-id="88a91-115">Criar um novo aplicativo ASP.NET MVC usando o modelo completo do aplicativo Web do ASP.NET MVC 3 e copie os seguintes arquivos para os mesmos diretórios em nosso projeto:</span><span class="sxs-lookup"><span data-stu-id="88a91-115">Create a new ASP.NET MVC application using the full ASP.NET MVC 3 Web Application template and copy the following files into the same directories in our project:</span></span>

1. <span data-ttu-id="88a91-116">AccountController.cs de cópia no diretório controladores</span><span class="sxs-lookup"><span data-stu-id="88a91-116">Copy AccountController.cs in the Controllers directory</span></span>
2. <span data-ttu-id="88a91-117">AccountModels de cópia no diretório de modelos</span><span class="sxs-lookup"><span data-stu-id="88a91-117">Copy AccountModels in the Models directory</span></span>
3. <span data-ttu-id="88a91-118">Crie um diretório de conta dentro do diretório de modos de exibição e copie todos os quatro modos de exibição no</span><span class="sxs-lookup"><span data-stu-id="88a91-118">Create an Account directory inside the Views directory and copy all four views in</span></span>

<span data-ttu-id="88a91-119">Altere o namespace para as classes de controlador e o modelo para que eles começam com MvcMusicStore.</span><span class="sxs-lookup"><span data-stu-id="88a91-119">Change the namespace for the Controller and Model classes so they begin with MvcMusicStore.</span></span> <span data-ttu-id="88a91-120">A classe AccountController deve usar o namespace de MvcMusicStore.Controllers e a classe AccountModels deve usar o namespace de MvcMusicStore.Models.</span><span class="sxs-lookup"><span data-stu-id="88a91-120">The AccountController class should use the MvcMusicStore.Controllers namespace, and the AccountModels class should use the MvcMusicStore.Models namespace.</span></span>

<span data-ttu-id="88a91-121">*Observação: Esses arquivos também estão disponíveis no download MvcMusicStore Assets.zip do qual copiamos nossos arquivos de projeto de site no início do tutorial. Os arquivos de associação estão localizados no diretório do código.*</span><span class="sxs-lookup"><span data-stu-id="88a91-121">*Note: These files are also available in the MvcMusicStore-Assets.zip download from which we copied our site design files at the beginning of the tutorial. The Membership files are located in the Code directory.*</span></span>

<span data-ttu-id="88a91-122">A solução atualizada deve ser semelhante ao seguinte:</span><span class="sxs-lookup"><span data-stu-id="88a91-122">The updated solution should look like the following:</span></span>

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a><span data-ttu-id="88a91-123">Adicionando um usuário administrativo com o site de configuração do ASP.NET</span><span class="sxs-lookup"><span data-stu-id="88a91-123">Adding an Administrative User with the ASP.NET Configuration site</span></span>

<span data-ttu-id="88a91-124">Antes de exigimos autorização em nosso site, precisaremos criar um usuário com acesso.</span><span class="sxs-lookup"><span data-stu-id="88a91-124">Before we require Authorization in our website, we'll need to create a user with access.</span></span> <span data-ttu-id="88a91-125">A maneira mais fácil para criar um usuário é usar o site de configuração do ASP.NET interno.</span><span class="sxs-lookup"><span data-stu-id="88a91-125">The easiest way to create a user is to use the built-in ASP.NET Configuration website.</span></span>

<span data-ttu-id="88a91-126">Inicie o site de configuração do ASP.NET, clicando no ícone no Gerenciador de soluções a seguir.</span><span class="sxs-lookup"><span data-stu-id="88a91-126">Launch the ASP.NET Configuration website by clicking following the icon in the Solution Explorer.</span></span>

![](mvc-music-store-part-7/_static/image2.png)

<span data-ttu-id="88a91-127">Isso inicia um site de configuração.</span><span class="sxs-lookup"><span data-stu-id="88a91-127">This launches a configuration website.</span></span> <span data-ttu-id="88a91-128">Clique na guia Segurança na tela inicial e, em seguida, clique no link "Ativar funções" no centro da tela.</span><span class="sxs-lookup"><span data-stu-id="88a91-128">Click on the Security tab on the home screen, then click the "Enable roles" link in the center of the screen.</span></span>

![](mvc-music-store-part-7/_static/image3.png)

<span data-ttu-id="88a91-129">Clique no link "criar ou gerenciar funções".</span><span class="sxs-lookup"><span data-stu-id="88a91-129">Click the "Create or Manage roles" link.</span></span>

![](mvc-music-store-part-7/_static/image4.png)

<span data-ttu-id="88a91-130">Insira "Administrador" como o nome da função e pressione o botão Adicionar função.</span><span class="sxs-lookup"><span data-stu-id="88a91-130">Enter "Administrator" as the role name and press the Add Role button.</span></span>

![](mvc-music-store-part-7/_static/image5.png)

<span data-ttu-id="88a91-131">Clique no botão Voltar e, em seguida, clique no link Criar usuário no lado esquerdo.</span><span class="sxs-lookup"><span data-stu-id="88a91-131">Click the Back button, then click on the Create user link on the left side.</span></span>

![](mvc-music-store-part-7/_static/image6.png)

<span data-ttu-id="88a91-132">Preencha os campos de informações do usuário à esquerda usando as seguintes informações:</span><span class="sxs-lookup"><span data-stu-id="88a91-132">Fill in the user information fields on the left using the following information:</span></span>

| <span data-ttu-id="88a91-133">**Campo**</span><span class="sxs-lookup"><span data-stu-id="88a91-133">**Field**</span></span> | <span data-ttu-id="88a91-134">**Value**</span><span class="sxs-lookup"><span data-stu-id="88a91-134">**Value**</span></span> |
| --- | --- |
| <span data-ttu-id="88a91-135">**Nome de usuário**</span><span class="sxs-lookup"><span data-stu-id="88a91-135">**User Name**</span></span> | <span data-ttu-id="88a91-136">Administrador</span><span class="sxs-lookup"><span data-stu-id="88a91-136">Administrator</span></span> |
| <span data-ttu-id="88a91-137">**Senha**</span><span class="sxs-lookup"><span data-stu-id="88a91-137">**Password**</span></span> | <span data-ttu-id="88a91-138">password123!</span><span class="sxs-lookup"><span data-stu-id="88a91-138">password123!</span></span> |
| <span data-ttu-id="88a91-139">**Confirmar senha**</span><span class="sxs-lookup"><span data-stu-id="88a91-139">**Confirm Password**</span></span> | <span data-ttu-id="88a91-140">password123!</span><span class="sxs-lookup"><span data-stu-id="88a91-140">password123!</span></span> |
| <span data-ttu-id="88a91-141">**Email**</span><span class="sxs-lookup"><span data-stu-id="88a91-141">**E-mail**</span></span> | <span data-ttu-id="88a91-142">(qualquer endereço de email funcionarão)</span><span class="sxs-lookup"><span data-stu-id="88a91-142">(any email address will work)</span></span> |
| <span data-ttu-id="88a91-143">**Pergunta de Segurança**</span><span class="sxs-lookup"><span data-stu-id="88a91-143">**Security Question**</span></span> | <span data-ttu-id="88a91-144">(o que quiser)</span><span class="sxs-lookup"><span data-stu-id="88a91-144">(whatever you like)</span></span> |
| <span data-ttu-id="88a91-145">**Resposta de Segurança**</span><span class="sxs-lookup"><span data-stu-id="88a91-145">**Security Answer**</span></span> | <span data-ttu-id="88a91-146">(o que quiser)</span><span class="sxs-lookup"><span data-stu-id="88a91-146">(whatever you like)</span></span> |

<span data-ttu-id="88a91-147">*Observação: Você certamente pode usar qualquer senha que você gostaria. As configurações de segurança de senha padrão exigem uma senha que é de 7 caracteres e contém um caractere não alfanumérico.*</span><span class="sxs-lookup"><span data-stu-id="88a91-147">*Note: You can of course use any password you'd like. The default password security settings require a password that is 7 characters long and contains one non-alphanumeric character.*</span></span>

<span data-ttu-id="88a91-148">Selecione a função de administrador para esse usuário e clique no botão Create User.</span><span class="sxs-lookup"><span data-stu-id="88a91-148">Select the Administrator role for this user, and click the Create User button.</span></span>

![](mvc-music-store-part-7/_static/image7.png)

<span data-ttu-id="88a91-149">Neste ponto, você verá uma mensagem indicando que o usuário foi criado com êxito.</span><span class="sxs-lookup"><span data-stu-id="88a91-149">At this point, you should see a message indicating that the user was created successfully.</span></span>

![](mvc-music-store-part-7/_static/image8.png)

<span data-ttu-id="88a91-150">Agora você pode fechar a janela do navegador.</span><span class="sxs-lookup"><span data-stu-id="88a91-150">You can now close the browser window.</span></span>

## <a name="role-based-authorization"></a><span data-ttu-id="88a91-151">Autorização baseada em função</span><span class="sxs-lookup"><span data-stu-id="88a91-151">Role-based Authorization</span></span>

<span data-ttu-id="88a91-152">Agora podemos pode restringir o acesso para o StoreManagerController usando o atributo [autorizar], especificando que o usuário deve ser na função de administrador para acessar qualquer ação do controlador na classe.</span><span class="sxs-lookup"><span data-stu-id="88a91-152">Now we can restrict access to the StoreManagerController using the [Authorize] attribute, specifying that the user must be in the Administrator role to access any controller action in the class.</span></span>

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

<span data-ttu-id="88a91-153">*Observação: O atributo [autorizar] pode ser colocado nos métodos de ação específica, bem como no nível de classe do controlador.*</span><span class="sxs-lookup"><span data-stu-id="88a91-153">*Note: The [Authorize] attribute can be placed on specific action methods as well as at the Controller class level.*</span></span>

<span data-ttu-id="88a91-154">Agora navegando até /StoreManager abre uma caixa de diálogo de logon:</span><span class="sxs-lookup"><span data-stu-id="88a91-154">Now browsing to /StoreManager brings up a Log On dialog:</span></span>

![](mvc-music-store-part-7/_static/image9.png)

<span data-ttu-id="88a91-155">Depois de fazer logon com a nova conta de administrador, podemos ir para a tela Editar álbum como antes.</span><span class="sxs-lookup"><span data-stu-id="88a91-155">After logging on with our new Administrator account, we're able to go to the Album Edit screen as before.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="88a91-156">[Anterior](mvc-music-store-part-6.md)
> [Próximo](mvc-music-store-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="88a91-156">[Previous](mvc-music-store-part-6.md)
[Next](mvc-music-store-part-8.md)</span></span>
