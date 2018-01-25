---
title: "Áreas"
author: rick-anderson
description: "Mostra como trabalhar com áreas."
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/areas
ms.openlocfilehash: 87bf2eaad1c13d21412051be769992411f685e2e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="areas"></a>Áreas

Por [Dhananjay Kumar](https://twitter.com/debug_mode) e [Rick Anderson](https://twitter.com/RickAndMSFT)

Áreas são um recurso do ASP.NET MVC usado para organizar as funcionalidades relacionadas em um grupo como um namespace separado (para o roteamento) e a estrutura de pasta (para modos de exibição). Usando áreas cria uma hierarquia com a finalidade de roteamento, adicionando outro parâmetro de rota, `area`, `controller` e `action`.

Áreas fornecem uma forma de particionar um aplicativo da Web MVC do ASP.NET Core grande em menores agrupamentos funcionais. Uma área é efetivamente uma estrutura MVC dentro de um aplicativo. Em um projeto MVC, componentes lógicos como modelo, o controlador e o modo de exibição são mantidos em pastas diferentes e MVC usa convenções de nomenclatura para criar a relação entre esses componentes. Para um aplicativo grande, pode ser vantajoso para dividir o aplicativo em áreas separadas de nível alto de funcionalidade. Por exemplo, um aplicativo de comércio eletrônico com várias unidades de negócios, como check-out, cobrança e pesquisa etc. Cada uma dessas unidades têm seus próprios modos de exibição do componente lógico, controladores e modelos. Nesse cenário, você pode usar áreas para particionar fisicamente os componentes comerciais no mesmo projeto.

Uma área pode ser definida como menores unidades funcionais em um projeto MVC do ASP.NET Core com seu próprio conjunto de controladores, modos de exibição e modelos.

Considere o uso de áreas em um MVC projeto quando:

* Seu aplicativo é composto de vários componentes funcionais alto nível que devem ser separados logicamente

* Você deseja particionar seu projeto MVC para que cada área funcional pode ser trabalhada independentemente

Recursos de área:

* Um aplicativo MVC do ASP.NET Core pode ter qualquer número de áreas

* Cada área tem seus próprios controladores, modelos e modos de exibição

* Permite organizar projetos MVC grandes em vários componentes de alto nível que podem ser executados independentemente

* Oferece suporte a vários controladores com o mesmo nome - desde que eles têm diferentes *áreas*

Vamos dar uma olhada em um exemplo para ilustrar como áreas são criadas e usadas. Digamos que você tenha um aplicativo de armazenamento que tem dois grupos distintos de controladores e exibições: produtos e serviços. Uma pasta comum estrutura que usando áreas do MVC aparência abaixo:

* Nome do projeto

  * Áreas

    * Produtos

      * Controladores

        * HomeController.cs

        * ManageController.cs

      * Exibições

        * Home

          * Index.cshtml

        * Gerenciar

          * Index.cshtml

    * Serviços

      * Controladores

        * HomeController.cs

      * Exibições

        * Home

          * Index.cshtml

Quando tenta MVC renderizar uma exibição em uma área, por padrão, ele tenta pesquisar nos seguintes locais:

```text
/Areas/<Area-Name>/Views/<Controller-Name>/<Action-Name>.cshtml
   /Areas/<Area-Name>/Views/Shared/<Action-Name>.cshtml
   /Views/Shared/<Action-Name>.cshtml
   ```

Estes são os locais padrão que podem ser alterados por meio de `AreaViewLocationFormats` no `Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions`.

Por exemplo, o código em vez de ter o nome da pasta como 'Áreas', abaixo foi alterado para 'Categorias'.

```csharp
services.Configure<RazorViewEngineOptions>(options =>
   {
       options.AreaViewLocationFormats.Clear();
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/{1}/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Categories/{2}/Views/Shared/{0}.cshtml");
       options.AreaViewLocationFormats.Add("/Views/Shared/{0}.cshtml");
   });
   ```

Observe que é a estrutura do *modos de exibição* pasta é a única que é considerada importante aqui e, como o conteúdo do restante das pastas *controladores* e *modelos* does **não** importa. Por exemplo, você não precisa ter um *controladores* e *modelos* pasta todos. Isso funciona porque o conteúdo de *controladores* e *modelos* é apenas o código que é compilado em um. dll, enquanto que o conteúdo do *exibições* não até que uma solicitação para que modo de exibição foi feito.

Depois que você definiu a hierarquia de pastas, você precisa informar ao MVC que cada controlador está associado uma área. Você pode fazer isso, decorando o nome do controlador com o `[Area]` atributo.

```csharp
...
   namespace MyStore.Areas.Products.Controllers
   {
       [Area("Products")]
       public class HomeController : Controller
       {
           // GET: /Products/Home/Index
           public IActionResult Index()
           {
               return View();
           }

           // GET: /Products/Home/Create
           public IActionResult Create()
           {
               return View();
           }
       }
   }
   ```

Configure uma definição de rota que funciona com as áreas recém-criado. O [roteamento para ações do controlador](routing.md) artigo apresenta detalhes sobre como criar definições de rota, incluindo o uso convencionais rotas versus rotas de atributo. Neste exemplo, vamos usar uma rota convencional. Para fazer isso, abra o *Startup.cs* de arquivo e modificá-lo adicionando o `areaRoute` chamado de definição da rota abaixo.

```csharp
...
   app.UseMvc(routes =>
   {
     routes.MapRoute(
         name: "areaRoute",
         template: "{area:exists}/{controller=Home}/{action=Index}/{id?}");

     routes.MapRoute(
         name: "default",
         template: "{controller=Home}/{action=Index}/{id?}");
   });
   ```

Navegando para `http://<yourApp>/products`, o `Index` método de ação a `HomeController` no `Products` área será invocada.

## <a name="link-generation"></a>Geração de link

* Geração de links de uma ação dentro de uma área com base em controlador de outra ação dentro do mesmo controlador.

  Digamos que o caminho da solicitação atual é como`/Products/Home/Create`

  Sintaxe de HtmlHelper:`@Html.ActionLink("Go to Product's Home Page", "Index")`

  Sintaxe de TagHelper:`<a asp-action="Index">Go to Product's Home Page</a>`

  Observe que não é necessário fornecer os valores de 'área' e 'controller' aqui elas já estão disponíveis no contexto da solicitação atual. Esses tipos de valores são chamados `ambient` valores.

* Geração de links de uma ação dentro de uma área com base no controlador de outra ação em um controlador diferente

  Digamos que o caminho da solicitação atual é como`/Products/Home/Create`

  Sintaxe de HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Manage")`

  Sintaxe de TagHelper:`<a asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  Observe que o valor de ambiente de uma área é usado aqui, mas o valor de 'controller' é especificado explicitamente acima.

* Geração de links de uma ação dentro de uma área, controlador para outra ação com base em um controlador diferente e uma área diferente.

  Digamos que o caminho da solicitação atual é como`/Products/Home/Create`

  Sintaxe de HtmlHelper:`@Html.ActionLink("Go to Services’ Home Page", "Index", "Home", new { area = "Services" })`

  Sintaxe de TagHelper:`<a asp-area="Services" asp-controller="Home" asp-action="Index">Go to Services’ Home Page</a>`

  Observe que aqui sem valores de ambiente são usados.

* Geração de links de uma ação dentro de um controlador de área com base em outra ação em um controlador diferente e **não** em uma área.

  Sintaxe de HtmlHelper:`@Html.ActionLink("Go to Manage Products’  Home Page", "Index", "Home", new { area = "" })`

  Sintaxe de TagHelper:`<a asp-area="" asp-controller="Manage" asp-action="Index">Go to Manage Products’  Home Page</a>`

  Desde que você deseja gerar links para uma área não-com base em ação do controlador, vazio é o valor de ambiente para 'área' aqui.

## <a name="publishing-areas"></a>Áreas de publicação

Todos os `*.cshtml` e `wwwroot/**` para dar saída quando os arquivos são publicados `<Project Sdk="Microsoft.NET.Sdk.Web">` está incluído no *. csproj* arquivo.
