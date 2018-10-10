---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Criar o capítulo Downloads para o EF 5 MVC 4 tutoriais | Microsoft Docs
author: Rick-Anderson
description: Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 6b5d10ba9e878908953e999bd1fd44970acf4ca5
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911949"
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="2e0ec-103">Criar o capítulo Downloads para o EF 5 MVC 4 tutoriais</span><span class="sxs-lookup"><span data-stu-id="2e0ec-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="2e0ec-104">por [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="2e0ec-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[<span data-ttu-id="2e0ec-105">Baixe o projeto concluído</span><span class="sxs-lookup"><span data-stu-id="2e0ec-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="2e0ec-106">Aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 4 usando o Code First do Entity Framework 5 e o Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="2e0ec-107">Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="2e0ec-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="2e0ec-108">Criando os Downloads do capítulo</span><span class="sxs-lookup"><span data-stu-id="2e0ec-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="2e0ec-109">Baixe e descompacte o arquivo zip de exemplo de projeto.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="2e0ec-110">O pacote de download descompactado, você encontrará os arquivos zip adicionais, um para a conclusão de cada capítulo.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="2e0ec-111">Clique com botão direito no arquivo zip desejado, clique em **propriedades**e clique no **Unblock** botão.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="2e0ec-112">Descompacte o arquivo.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-112">Unzip the file.</span></span>
4. <span data-ttu-id="2e0ec-113">Clique duas vezes o *CUx.sln* arquivo para iniciar o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="2e0ec-114">Do **ferramentas** menu, clique em **Gerenciador de pacotes NuGet**, em seguida, **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="2e0ec-115">No pacote Manager Console (PMC), clique em **restaurar**.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="2e0ec-116">Saia do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="2e0ec-117">Reinicie o Visual Studio, abrir o arquivo de solução que você fechou na etapa anterior.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="2e0ec-118">No pacote Manager Console (PMC), insira o `Update-Database` comando:</span><span class="sxs-lookup"><span data-stu-id="2e0ec-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="2e0ec-119">Se você receber o erro a seguir:</span><span class="sxs-lookup"><span data-stu-id="2e0ec-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="2e0ec-120">*O termo 'Update-Database' não é reconhecido como o nome de um cmdlet, função, arquivo de script ou programa operável. Verifique a ortografia do nome, ou se um caminho foi incluído, verifique se o caminho está correto e tente novamente.*</span><span class="sxs-lookup"><span data-stu-id="2e0ec-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="2e0ec-121">Saia e reinicie o Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="2e0ec-122">Cada migração será executado e, em seguida, o método de propagação será executado.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="2e0ec-123">Agora você pode executar o aplicativo.</span><span class="sxs-lookup"><span data-stu-id="2e0ec-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="2e0ec-124">Anterior</span><span class="sxs-lookup"><span data-stu-id="2e0ec-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
