---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Criar um novo projeto ASP.NET MVC | Microsoft Docs
author: microsoft
description: Etapa 1 mostra como implementar a estrutura básica do aplicativo NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: d15ca67f0ddd8db6842bc5112996ae2dee433536
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869251"
---
<a name="create-a-new-aspnet-mvc-project"></a><span data-ttu-id="36db4-103">Criar um novo projeto ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="36db4-103">Create a New ASP.NET MVC Project</span></span>
====================
<span data-ttu-id="36db4-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="36db4-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="36db4-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="36db4-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="36db4-106">Esta é a etapa 1 de livremente [aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) que aborda-por meio de como criar um pequeno, mas concluir, o aplicativo web usando o ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="36db4-106">This is step 1 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="36db4-107">Etapa 1 mostra como implementar a estrutura básica do aplicativo NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="36db4-107">Step 1 shows you how to put the basic NerdDinner application structure in place.</span></span>
> 
> <span data-ttu-id="36db4-108">Se você estiver usando o ASP.NET MVC 3, recomendamos que você siga o [obtendo iniciado com MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) ou [repositório de música MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriais.</span><span class="sxs-lookup"><span data-stu-id="36db4-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-1-file-gtnew-project"></a><span data-ttu-id="36db4-109">NerdDinner etapa 1: Arquivo -&gt;novo projeto</span><span class="sxs-lookup"><span data-stu-id="36db4-109">NerdDinner Step 1: File-&gt;New Project</span></span>

<span data-ttu-id="36db4-110">Vamos começar nosso aplicativo NerdDinner selecionando o **arquivo -&gt;novo projeto** item de menu no Visual Studio 2008 ou o livre Visual Web Developer 2008 Express.</span><span class="sxs-lookup"><span data-stu-id="36db4-110">We'll begin our NerdDinner application by selecting the **File-&gt;New Project** menu item within either Visual Studio 2008 or the free Visual Web Developer 2008 Express.</span></span>

<span data-ttu-id="36db4-111">Isso abrirá a caixa de diálogo "Novo projeto".</span><span class="sxs-lookup"><span data-stu-id="36db4-111">This will bring up the "New Project" dialog.</span></span> <span data-ttu-id="36db4-112">Para criar um novo aplicativo ASP.NET MVC, vamos selecionar o nó "Web" no lado esquerdo da caixa de diálogo e, em seguida, escolha o modelo de projeto "Aplicativo Web ASP.NET MVC" à direita:</span><span class="sxs-lookup"><span data-stu-id="36db4-112">To create a new ASP.NET MVC application, we'll select the "Web" node on the left-hand side of the dialog and then choose the "ASP.NET MVC Web Application" project template on the right:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image1.png)

<span data-ttu-id="36db4-113">*Importante: Verifique se você baixou e instalou o ASP.NET MVC - caso contrário não aparecerão na caixa de diálogo Novo projeto. Você pode usar V2 do [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) se você ainda não instalou-(ASP.NET MVC está disponível na "Web plataforma -&gt;estruturas e tempos de execução" seção).*</span><span class="sxs-lookup"><span data-stu-id="36db4-113">*Important: Make sure you've downloaded and installed ASP.NET MVC - otherwise it won't show up in the New Project dialog. You can use V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) if you haven't installed it yet (ASP.NET MVC is available within the "Web Platform-&gt;Frameworks and Runtimes" section).*</span></span>

<span data-ttu-id="36db4-114">Chamaremos o novo projeto, vamos criar "NerdDinner" e, em seguida, clique no botão "okey" para criá-lo.</span><span class="sxs-lookup"><span data-stu-id="36db4-114">We'll name the new project we are going to create "NerdDinner" and then click the "ok" button to create it.</span></span>

<span data-ttu-id="36db4-115">Quando clicarmos "okey" Visual Studio abrirá uma caixa de diálogo adicional que solicita a opcionalmente, crie um projeto de teste de unidade para o novo aplicativo.</span><span class="sxs-lookup"><span data-stu-id="36db4-115">When we click "ok" Visual Studio will bring up an additional dialog that prompts us to optionally create a unit test project for the new application as well.</span></span> <span data-ttu-id="36db4-116">Este projeto de teste de unidade nos permite criar testes automatizados que verificam a funcionalidade e o comportamento do nosso aplicativo (algo, abordaremos como tarefas em breve neste tutorial).</span><span class="sxs-lookup"><span data-stu-id="36db4-116">This unit test project enables us to create automated tests that verify the functionality and behavior of our application (something we'll cover how to-do later in this tutorial).</span></span>

![](create-a-new-aspnet-mvc-project/_static/image2.png)

<span data-ttu-id="36db4-117">Menu suspenso "Estrutura de teste" na caixa de diálogo acima é populado com todos os disponível ASP.NET MVC unidade teste modelos de projeto instalados no computador.</span><span class="sxs-lookup"><span data-stu-id="36db4-117">The "Test framework" dropdown in the above dialog is populated with all available ASP.NET MVC unit test project templates installed on the machine.</span></span> <span data-ttu-id="36db4-118">As versões podem ser baixadas para NUnit, MBUnit e XUnit.</span><span class="sxs-lookup"><span data-stu-id="36db4-118">Versions can be downloaded for NUnit, MBUnit, and XUnit.</span></span> <span data-ttu-id="36db4-119">Também há suporte para a estrutura interna do teste de unidade do Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="36db4-119">The built-in Visual Studio Unit Test framework is also supported.</span></span>

<span data-ttu-id="36db4-120">*Observação: O Visual Studio Unit Test Framework só está disponível com o Visual Studio 2008 Professional e versões posteriores. Se você estiver usando o VS 2008 Standard Edition ou do Visual Web Developer 2008 Express, você precisará baixar e instalar as extensões do NUnit, MBUnit ou XUnit para ASP.NET MVC para essa caixa de diálogo a ser mostrado. A caixa de diálogo não será exibida se não há nenhuma estrutura de teste instalada.*</span><span class="sxs-lookup"><span data-stu-id="36db4-120">*Note: The Visual Studio Unit Test Framework is only available with Visual Studio 2008 Professional and higher versions. If you are using VS 2008 Standard Edition or Visual Web Developer 2008 Express you will need to download and install the NUnit, MBUnit or XUnit extensions for ASP.NET MVC in order for this dialog to be shown. The dialog will not display if there aren't any test frameworks installed.*</span></span>

<span data-ttu-id="36db4-121">Vamos usar o nome de "NerdDinner.Tests" padrão para o projeto de teste que criarmos e usar a opção de estrutura "Visual Studio de teste de unidade".</span><span class="sxs-lookup"><span data-stu-id="36db4-121">We'll use the default "NerdDinner.Tests" name for the test project we create, and use the "Visual Studio Unit Test" framework option.</span></span> <span data-ttu-id="36db4-122">Quando clicamos no botão "okey" Visual Studio criará uma solução para nós com dois projetos nele - uma para o nosso aplicativo web e outra para nossos testes de unidade:</span><span class="sxs-lookup"><span data-stu-id="36db4-122">When we click the "ok" button Visual Studio will create a solution for us with two projects in it - one for our web application and one for our unit tests:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a><span data-ttu-id="36db4-123">Examinando a estrutura de diretório NerdDinner</span><span class="sxs-lookup"><span data-stu-id="36db4-123">Examining the NerdDinner directory structure</span></span>

<span data-ttu-id="36db4-124">Quando você cria um novo aplicativo ASP.NET MVC com o Visual Studio, ele adiciona automaticamente um número de arquivos e diretórios para o projeto:</span><span class="sxs-lookup"><span data-stu-id="36db4-124">When you create a new ASP.NET MVC application with Visual Studio, it automatically adds a number of files and directories to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image4.png)

<span data-ttu-id="36db4-125">Projetos do ASP.NET MVC por padrão tem seis diretórios de alto nível:</span><span class="sxs-lookup"><span data-stu-id="36db4-125">ASP.NET MVC projects by default have six top-level directories:</span></span>

| <span data-ttu-id="36db4-126">**Diretório**</span><span class="sxs-lookup"><span data-stu-id="36db4-126">**Directory**</span></span> | <span data-ttu-id="36db4-127">**Finalidade**</span><span class="sxs-lookup"><span data-stu-id="36db4-127">**Purpose**</span></span> |
| --- | --- |
| <span data-ttu-id="36db4-128">**/Controllers**</span><span class="sxs-lookup"><span data-stu-id="36db4-128">**/Controllers**</span></span> | <span data-ttu-id="36db4-129">Onde você colocou classes do controlador que manipulam solicitações de URL</span><span class="sxs-lookup"><span data-stu-id="36db4-129">Where you put Controller classes that handle URL requests</span></span> |
| <span data-ttu-id="36db4-130">**/Models**</span><span class="sxs-lookup"><span data-stu-id="36db4-130">**/Models**</span></span> | <span data-ttu-id="36db4-131">Onde você colocou classes que representam e manipulam dados</span><span class="sxs-lookup"><span data-stu-id="36db4-131">Where you put classes that represent and manipulate data</span></span> |
| <span data-ttu-id="36db4-132">**/Views**</span><span class="sxs-lookup"><span data-stu-id="36db4-132">**/Views**</span></span> | <span data-ttu-id="36db4-133">Em que você pode colocar arquivos de modelo de interface do usuário que são responsáveis pela saída de renderização</span><span class="sxs-lookup"><span data-stu-id="36db4-133">Where you put UI template files that are responsible for rendering output</span></span> |
| <span data-ttu-id="36db4-134">**/Scripts**</span><span class="sxs-lookup"><span data-stu-id="36db4-134">**/Scripts**</span></span> | <span data-ttu-id="36db4-135">Onde colocar os arquivos de biblioteca de JavaScript e scripts (. js)</span><span class="sxs-lookup"><span data-stu-id="36db4-135">Where you put JavaScript library files and scripts (.js)</span></span> |
| <span data-ttu-id="36db4-136">**/Content**</span><span class="sxs-lookup"><span data-stu-id="36db4-136">**/Content**</span></span> | <span data-ttu-id="36db4-137">Onde você colocou o CSS e arquivos de imagem e outros tipos de conteúdo não dinâmico/não JavaScript</span><span class="sxs-lookup"><span data-stu-id="36db4-137">Where you put CSS and image files, and other non-dynamic/non-JavaScript content</span></span> |
| <span data-ttu-id="36db4-138">**/App\_Data**</span><span class="sxs-lookup"><span data-stu-id="36db4-138">**/App\_Data**</span></span> | <span data-ttu-id="36db4-139">Onde armazenar arquivos de dados ser leitura/gravação.</span><span class="sxs-lookup"><span data-stu-id="36db4-139">Where you store data files you want to read/write.</span></span> |

<span data-ttu-id="36db4-140">ASP.NET MVC não requer essa estrutura.</span><span class="sxs-lookup"><span data-stu-id="36db4-140">ASP.NET MVC does not require this structure.</span></span> <span data-ttu-id="36db4-141">Na verdade, desenvolvedores que trabalham em aplicativos grandes normalmente particionará o aplicativo de backup em vários projetos para tornar mais fácil de gerenciar (por exemplo: classes de modelo de dados geralmente vão em um projeto de biblioteca de classe separada do aplicativo web).</span><span class="sxs-lookup"><span data-stu-id="36db4-141">In fact, developers working on large applications will typically partition the application up across multiple projects to make it more manageable (for example: data model classes often go in a separate class library project from the web application).</span></span> <span data-ttu-id="36db4-142">No entanto, a estrutura de projeto padrão, fornecer uma convenção de diretório padrão adequado que podemos usar para manter nossos preocupações de aplicativo normal.</span><span class="sxs-lookup"><span data-stu-id="36db4-142">The default project structure, however, does provide a nice default directory convention that we can use to keep our application concerns clean.</span></span>

<span data-ttu-id="36db4-143">Quando, expanda a pasta /Controllers descobriremos que o Visual Studio adicionou as duas classes de controlador – HomeController e AccountController – por padrão ao projeto:</span><span class="sxs-lookup"><span data-stu-id="36db4-143">When we expand the /Controllers directory we'll find that Visual Studio added two controller classes – HomeController and AccountController – by default to the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image5.png)

<span data-ttu-id="36db4-144">Quando, expanda o diretório /Views, descobriremos três subdiretórios – /Home, conta e /Shared –, bem como modelo de vários arquivos dentro deles também foram adicionados ao projeto por padrão:</span><span class="sxs-lookup"><span data-stu-id="36db4-144">When we expand the /Views directory, we'll find three sub-directories – /Home, /Account and /Shared – as well as several template files within them were also added to the project by default:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image6.png)

<span data-ttu-id="36db4-145">Quando é expande o /Content e diretórios /Scripts, descobriremos que um arquivo de site que é usado para definir o estilo de todo o HTML no site, bem como bibliotecas JavaScript que podem habilitar o ASP.NET AJAX e jQuery suportem no aplicativo:</span><span class="sxs-lookup"><span data-stu-id="36db4-145">When we expand the /Content and /Scripts directories, we'll find a Site.css file that is used to style all HTML on the site, as well as JavaScript libraries that can enable ASP.NET AJAX and jQuery support within the application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image7.png)

<span data-ttu-id="36db4-146">Quando, expanda o projeto NerdDinner.Tests descobriremos duas classes que contêm os testes de unidade para nosso classes do controlador:</span><span class="sxs-lookup"><span data-stu-id="36db4-146">When we expand the NerdDinner.Tests project we'll find two classes that contain unit tests for our controller classes:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image8.png)

<span data-ttu-id="36db4-147">Esses arquivos padrão adicionados pelo Visual Studio nos fornecem uma estrutura básica para um aplicativo de trabalho - completo com a página inicial, sobre, páginas de logon/logoff/registro de conta e uma página de erro não tratado (todos com fio-up e trabalhar fora da caixa).</span><span class="sxs-lookup"><span data-stu-id="36db4-147">These default files added by Visual Studio provide us with a basic structure for a working application - complete with home page, about page, account login/logout/registration pages, and an unhandled error page (all wired-up and working out of the box).</span></span>

### <a name="running-the-nerddinner-application"></a><span data-ttu-id="36db4-148">Executando o aplicativo NerdDinner</span><span class="sxs-lookup"><span data-stu-id="36db4-148">Running the NerdDinner Application</span></span>

<span data-ttu-id="36db4-149">Podemos executar o projeto, escolhendo um o **Debug -&gt;iniciar depuração** ou **Debug -&gt;Start Without Debugging** itens de menu:</span><span class="sxs-lookup"><span data-stu-id="36db4-149">We can run the project by choosing either the **Debug-&gt;Start Debugging** or **Debug-&gt;Start Without Debugging** menu items:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image9.png)

<span data-ttu-id="36db4-150">Isso irá iniciar o servidor Web ASP.NET interno-que vem com o Visual Studio e execute o nosso aplicativo:</span><span class="sxs-lookup"><span data-stu-id="36db4-150">This will launch the built-in ASP.NET Web-server that comes with Visual Studio, and run our application:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image10.png)

<span data-ttu-id="36db4-151">Abaixo está a home page do nosso novo projeto (URL: "/") quando ele é executado:</span><span class="sxs-lookup"><span data-stu-id="36db4-151">Below is the home page for our new project (URL: "/") when it runs:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image11.png)

<span data-ttu-id="36db4-152">Clique na guia "Sobre" exibe um sobre a página (URL: "/ Home e sobre"):</span><span class="sxs-lookup"><span data-stu-id="36db4-152">Clicking the "About" tab displays an about page (URL: "/Home/About"):</span></span>

![](create-a-new-aspnet-mvc-project/_static/image12.png)

<span data-ttu-id="36db4-153">Clicando no link "Logon" na parte superior direita nos leva a uma página de logon (URL: "Conta/LogOn")</span><span class="sxs-lookup"><span data-stu-id="36db4-153">Clicking the "Log On" link on the top-right takes us to a Login page (URL: "/Account/LogOn")</span></span>

![](create-a-new-aspnet-mvc-project/_static/image13.png)

<span data-ttu-id="36db4-154">Se não temos uma conta de logon, clique no link de registro (URL: "Conta/Register") para criar um:</span><span class="sxs-lookup"><span data-stu-id="36db4-154">If we don't have a login account we can click the register link (URL: "/Account/Register") to create one:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image14.png)

<span data-ttu-id="36db4-155">O código para implementar o home acima, e logout /Register funcionalidade foi adicionada por padrão, quando criamos nosso novo projeto.</span><span class="sxs-lookup"><span data-stu-id="36db4-155">The code to implement the above home, about, and logout/ register functionality was added by default when we created our new project.</span></span> <span data-ttu-id="36db4-156">Vamos usá-lo como ponto de partida do nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="36db4-156">We'll use it as the starting point of our application.</span></span>

### <a name="testing-the-nerddinner-application"></a><span data-ttu-id="36db4-157">Testando o aplicativo NerdDinner</span><span class="sxs-lookup"><span data-stu-id="36db4-157">Testing the NerdDinner Application</span></span>

<span data-ttu-id="36db4-158">Se estiver usando o Professional Edition ou a versão mais recente do Visual Studio 2008, podemos usar o suporte do IDE do Visual Studio de teste de unidade interna para o projeto de teste:</span><span class="sxs-lookup"><span data-stu-id="36db4-158">If we are using the Professional Edition or higher version of Visual Studio 2008, we can use the built-in unit testing IDE support within Visual Studio to test the project:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image15.png)

<span data-ttu-id="36db4-159">Escolhendo uma das opções acima, abra o painel de "resultados de teste" dentro do IDE e fornecer com o status de aprovação/reprovação sobre os testes de 27 unidade incluído em nosso novo projeto que abrangem a funcionalidade interna:</span><span class="sxs-lookup"><span data-stu-id="36db4-159">Choosing one of the above options will open the "Test Results" pane within the IDE and provide us with pass/fail status on the 27 unit tests included in our new project that cover the built-in functionality:</span></span>

![](create-a-new-aspnet-mvc-project/_static/image16.png)

<span data-ttu-id="36db4-160">Posteriormente neste tutorial vamos falar mais sobre testes automatizados e adicionar testes de unidade adicionais que abrangem a implementamos a funcionalidade do aplicativo.</span><span class="sxs-lookup"><span data-stu-id="36db4-160">Later in this tutorial we'll talk more about automated testing and add additional unit tests that cover the application functionality we implement.</span></span>

### <a name="next-step"></a><span data-ttu-id="36db4-161">Próxima etapa</span><span class="sxs-lookup"><span data-stu-id="36db4-161">Next Step</span></span>

<span data-ttu-id="36db4-162">Agora temos uma estrutura de aplicativo básico em vigor.</span><span class="sxs-lookup"><span data-stu-id="36db4-162">We've now got a basic application structure in place.</span></span> <span data-ttu-id="36db4-163">Agora vamos [criar um banco de dados para armazenar os dados de aplicativo](create-a-database.md).</span><span class="sxs-lookup"><span data-stu-id="36db4-163">Let's now [create a database to store our application data](create-a-database.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="36db4-164">[Anterior](introducing-the-nerddinner-tutorial.md)
> [Próximo](create-a-database.md)</span><span class="sxs-lookup"><span data-stu-id="36db4-164">[Previous](introducing-the-nerddinner-tutorial.md)
[Next](create-a-database.md)</span></span>
