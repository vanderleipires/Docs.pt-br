---
title: "Visão geral do núcleo do ASP.NET MVC"
author: ardalis
description: "Saiba como o ASP.NET Core MVC é uma estrutura avançada para a criação de aplicativos web e APIs usando Model-View-Controller design padrão."
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 89af38d1-52e0-4db7-b791-dbce909b0714
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/overview
ms.openlocfilehash: 2492b6aa4602dbbf3b9cd3dca00d40690c640cab
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/28/2017
---
# <a name="overview-of-aspnet-core-mvc"></a>Visão geral do núcleo do ASP.NET MVC

Por [Steve Smith](https://ardalis.com/)

Núcleo do ASP.NET MVC é uma estrutura avançada para a criação de aplicativos web e APIs usando Model-View-Controller design padrão.

## <a name="what-is-the-mvc-pattern"></a>O que é o padrão MVC?

O padrão de arquitetura Model-View-Controller (MVC) separa um aplicativo em três grupos principais de componentes: modelos, exibições e controladores. Esse padrão ajuda a alcançar [separação de preocupações](http://deviq.com/separation-of-concerns/). Solicitações do usuário usando esse padrão, são roteadas para um controlador que é responsável para trabalhar com o modelo para executar ações de usuário e/ou recuperar os resultados de consultas. O controlador escolhe a exibição a ser exibida para o usuário e fornece-o com os dados de modelo requer.

O diagrama a seguir mostra os três componentes principais e quais fazem referência a outras pessoas:

![Padrão MVC](overview/_static/mvc.png)

Essa descrição das responsabilidades ajuda você a dimensionar o aplicativo em termos de complexidade porque é mais fácil de código, depurar e testar algo (modelo, exibição ou controlador) que tem um único trabalho (e segue o [princípio da responsabilidade única ](http://deviq.com/single-responsibility-principle/)). É mais difícil de atualização, testar e depurar código que tem dependências que abrange dois ou mais dessas três áreas. Por exemplo, a lógica da interface de usuário tende a ser alterado com mais frequência do que a lógica de negócios. Se a lógica de negócios e o código de apresentação é combinada em um único objeto, você precisa modificar um objeto que contém a lógica de negócios sempre que alterar a interface do usuário. Isso é adequado para introduzir erros e exigem o novo teste de toda a lógica de negócios depois de alterar cada interface mínima do usuário.

> [!NOTE]
> O modo de exibição e o controlador dependem do modelo. No entanto, o modelo depende o modo de exibição, nem o controlador. Esse é um dos principais benefícios da separação. Essa separação permite que o modelo a ser criado e testado independentemente da apresentação visual.

### <a name="model-responsibilities"></a>Responsabilidades do modelo

O modelo em um aplicativo MVC representa o estado do aplicativo e qualquer lógica de negócios ou operações que devem ser executadas por ele. Lógica de negócios deve ser encapsulada no modelo, juntamente com qualquer lógica de implementação para persistir o estado do aplicativo. Exibições fortemente tipadas normalmente usará tipos ViewModel especificamente criados para conter os dados para exibir no modo de exibição; o controlador será criar e preencher essas instâncias ViewModel do modelo.

> [!NOTE]
> Há várias maneiras de organizar o modelo em um aplicativo que usa o padrão arquitetônico MVC. Saiba mais sobre alguns [tipos diferentes de tipos de modelo](http://deviq.com/kinds-of-models/).

### <a name="view-responsibilities"></a>Responsabilidades do modo de exibição

Modos de exibição são responsáveis por apresentar conteúdo por meio da interface do usuário. Eles usam o [mecanismo de exibição Razor](#razor-view-engine) para inserir o código .NET em uma marcação HTML. Deve haver mínimo lógica em modos de exibição e qualquer lógica neles deve relacionar a apresentação do conteúdo. Se você encontrar arquivos a necessidade de executar uma grande quantidade de lógica no modo de exibição para exibir os dados de um modelo complexo, considere usar um [componentes da exibição](views/view-components.md), ViewModel, ou um modelo de exibição para simplificar a exibição.

### <a name="controller-responsibilities"></a>Responsabilidades do controlador

Os controladores são os componentes que lidar com a interação do usuário, trabalhar com o modelo e, por fim, selecionam um modo de exibição para renderizar. Em um aplicativo MVC, a exibição só mostra informações; o controlador manipula e responde à entrada do usuário e interação. O padrão MVC, o controlador é o ponto de entrada inicial e é responsável por selecionar qual modelo de tipos para trabalhar com e modo de exibição para renderizar (portanto, seu nome - controla como o aplicativo responde a uma determinada solicitação).

> [!NOTE]
> Controladores não devem ser excessivamente complicados por muitas responsabilidades. Para manter a lógica do controlador de se tornar excessivamente complexas, use o [princípio da responsabilidade única](http://deviq.com/single-responsibility-principle/) a lógica de negócios por push fora o controlador e o modelo de domínio.

>[!TIP]
> Se você achar que as ações do controlador com frequência executam os mesmos tipos de ações, você pode seguir o [não repetitivo princípio](http://deviq.com/don-t-repeat-yourself/) movendo essas ações comuns em [filtros](#filters).

## <a name="what-is-aspnet-core-mvc"></a>O que é o núcleo de ASP.NET MVC

A estrutura MVC do ASP.NET Core é uma fonte leve, abra, estrutura de apresentação testável altamente otimizada para uso com o ASP.NET Core.

Núcleo do ASP.NET MVC fornece uma maneira com base em padrões para criar sites dinâmicos que habilitam uma separação limpa de preocupações. Ele lhe dá controle total sobre a marcação, dá suporte ao desenvolvimento amigável a TDD e usa os padrões da web mais recentes.

## <a name="features"></a>Recursos

Núcleo do ASP.NET MVC inclui o seguinte:

* [Roteamento](#routing)
* [Associação de modelo](#model-binding)
* [Validação de modelo](#model-validation)
* [Injeção de dependência](../fundamentals/dependency-injection.md)
* [Filtros](#filters)
* [Áreas](#areas)
* [APIs da Web](#web-apis)
* [Capacidade de teste](#testability)
* [Mecanismo de exibição Razor](#razor-view-engine)
* [Modos de exibição fortemente tipados](#strongly-typed-views)
* [Auxiliares de marcação](#tag-helpers)
* [Componentes do modo de exibição](#view-components)

### <a name="routing"></a>Roteamento

ASP.NET MVC de núcleo é criado na parte superior do [roteamento ASP.NET Core](../fundamentals/routing.md), um componente de mapeamento de URL poderoso que permite que você crie aplicativos que têm URLs abrangentes e pesquisáveis. Isso permite que você defina URL padrões de nomenclatura do seu aplicativo que funcionam bem para (SEO) de otimização do mecanismo de pesquisa e para a geração de link, sem levar em consideração como os arquivos no servidor web são organizados. Você pode definir suas rotas usando uma sintaxe de modelo de rota conveniente que dá suporte a restrições de valor de rota, padrões e valores opcionais.

*Roteamento baseado em convenção* permite definir globalmente a URL de formatos que seu aplicativo aceita e como cada um dos formatos é mapeado para um método de ação específica em determinado controlador. Quando uma solicitação de entrada é recebida, o mecanismo de roteamento analisa a URL corresponde a um dos formatos de URL definidos e, em seguida, chama o método de ação do controlador associado.

```csharp
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

*Roteamento de atributo* permite que você especifique as informações de roteamento de decoração de seus controladores e ações com atributos que definem rotas do seu aplicativo. Isso significa que suas definições de rota são colocadas ao lado do controlador e ação com a qual está associados.

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
  [HttpGet("{id}")]
  public IActionResult GetProduct(int id)
  {
    ...
  }
}
```

### <a name="model-binding"></a>Associação de modelo

Núcleo do ASP.NET MVC [associação de modelo](models/model-binding.md) converte dados de solicitação de cliente (valores de formulário, os dados de rota, parâmetros de cadeia de caracteres de consulta, os cabeçalhos HTTP) em objetos que o controlador pode manipular. Como resultado, a lógica de controlador não precisa fazer o trabalho de descobrir os dados de solicitação de entrada; ele simplesmente tem os dados como parâmetros para os métodos de ação.

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
   ```

### <a name="model-validation"></a>validação de modelo

Dá suporte ao MVC do ASP.NET Core [validação](models/validation.md) decorando seu objeto de modelo com atributos de validação de anotação de dados. Os atributos de validação são verificadas no lado do cliente antes que os valores são postados no servidor, bem como no servidor antes da ação de controlador é chamado.

```csharp
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

Uma ação do controlador:

```csharp
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // If we got this far, something failed, redisplay form
    return View(model);
}
```

A estrutura tratará a validação de solicitação de dados no cliente e no servidor. Lógica de validação especificada em tipos de modelo é adicionada aos modos de exibição renderizados como anotações discretas e é imposta no navegador com [jQuery validação](https://jqueryvalidation.org/).

### <a name="dependency-injection"></a>Injeção de dependência

ASP.NET Core tem suporte interno para [injeção de dependência (DI)](../fundamentals/dependency-injection.md). No ASP.NET MVC de núcleo, [controladores](controllers/dependency-injection.md) pode solicitação necessários serviços por meio de seus construtores, possibilitando que siga a [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/).

O aplicativo também pode usar [arquivos no modo de exibição de injeção de dependência](views/dependency-injection.md), usando o `@inject` diretiva:

```cshtml
@inject SomeService ServiceName
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

### <a name="filters"></a>Filtros

[Filtros](controllers/filters.md) ajudar os desenvolvedores a encapsular resolvem preocupações, como o tratamento de exceção ou autorização. Filtros de habilitar a lógica de pré e pós-processamento personalizada em execução para os métodos de ação e podem ser configurados para execução em determinados pontos no pipeline de execução para uma determinada solicitação. Filtros podem ser aplicados a controladores ou ações como atributos (ou podem ser executados global). Vários filtros (como `Authorize`) são incluídas na estrutura.


```csharp
[Authorize]
   public class AccountController : Controller
   {
```

### <a name="areas"></a>Áreas

[Áreas](controllers/areas.md) fornecem uma maneira de particionar um aplicativo da Web MVC do ASP.NET Core grande em menores agrupamentos funcionais. Uma área é efetivamente uma estrutura MVC dentro de um aplicativo. Em um projeto MVC, componentes lógicos como modelo, o controlador e o modo de exibição são mantidos em pastas diferentes e MVC usa convenções de nomenclatura para criar a relação entre esses componentes. Para um aplicativo grande, pode ser vantajoso para dividir o aplicativo em áreas separadas de nível alto de funcionalidade. Por exemplo, um aplicativo de comércio eletrônico com várias unidades de negócios, como check-out, cobrança e pesquisa etc. Cada uma dessas unidades têm seus próprios modos de exibição do componente lógico, controladores e modelos.

### <a name="web-apis"></a>APIs da Web

Além de ser uma excelente plataforma para a criação de sites da web, MVC do ASP.NET Core tem excelente suporte para a criação de APIs da Web. Você pode criar serviços que podem alcançar uma ampla gama de clientes, incluindo navegadores e dispositivos móveis.

A estrutura inclui suporte para a negociação de conteúdo HTTP com suporte interno para [dados de formatação](models/formatting.md) como JSON ou XML. Gravar [formatadores personalizados](advanced/custom-formatters.md) para adicionar suporte para seus próprios formatos.

Use geração de link para habilitar o suporte para hipermídia. Habilitar o suporte para [recursos entre origens (CORS) de compartilhamento](http://www.w3.org/TR/cors/) para que suas APIs da Web podem ser compartilhados entre vários aplicativos Web.

### <a name="testability"></a>Capacidade de teste

Uso da estrutura de injeção de dependência e interfaces, é adequado para testes de unidade e a estrutura inclui recursos (como um provedor TestHost e InMemory para Entity Framework) que tornam [testes de integração](../testing/integration-testing.md) rápido e fácil também. Saiba mais sobre [lógica do controlador de teste](controllers/testing.md).

### <a name="razor-view-engine"></a>Mecanismo de exibição Razor

[Modos de exibição do ASP.NET MVC Core](views/overview.md) usar o [mecanismo de exibição Razor](views/razor.md) renderizar exibições. Razor é uma linguagem de marcação de modelo compact, expressivas e fluidos para definir modos de exibição usando o código c# inserido. Razor é usado para gerar dinamicamente o conteúdo da web no servidor. Corretamente, você pode combinar código de servidor com o código e conteúdo do lado cliente.

```text
<ul>
  @for (int i = 0; i < 5; i++) {
    <li>List item @i</li>
  }
</ul>
```

Usando o mecanismo de exibição Razor você pode definir [layouts](views/layout.md), [exibições parciais](views/partial.md) e seções substituíveis.

### <a name="strongly-typed-views"></a>Modos de exibição fortemente tipados

Modos de exibição Razor do MVC podem ser fortemente tipados com base no seu modelo. Controladores podem passar um modelo fortemente tipado para exibições habilitando seus modos de exibição para que a verificação de tipo e suporte do IntelliSense.

Por exemplo, a exibição a seguir define um modelo do tipo `IEnumerable<Product>`:

```cshtml
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

### <a name="tag-helpers"></a>Auxiliares de marcação

[Auxiliares da marcação](views/tag-helpers/intro.md) habilitar código do lado do servidor participar de criação e renderização de elementos HTML em arquivos do Razor. Você pode usar os auxiliares de marcação para definir marcas personalizadas (por exemplo, `<environment>`) ou para modificar o comportamento de marcas existentes (por exemplo, `<label>`). Auxiliares de marcação vincular a elementos específicos com base no nome do elemento e seus atributos. Eles fornecem os benefícios de renderização do lado do servidor e ainda preservar um experiência de edição de HTML.

Há muitos auxiliares de marcação interna para tarefas comuns - como a criação de formulários, links, ativos de carregamento e pacotes mais - e ainda mais disponíveis em repositórios GitHub públicos e como NuGet. Auxiliares de marca são criados no c#, e eles se destinam a elementos HTML com base no nome do elemento, o nome do atributo ou marca pai. Por exemplo, o interno LinkTagHelper pode ser usado para criar um link para o `Login` ação o `AccountsController`:

```cshtml
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

O `EnvironmentTagHelper` pode ser usado para incluir scripts diferentes em exibições (por exemplo, minimizada ou raw) com base no ambiente de tempo de execução, como desenvolvimento, teste ou produção:

```cshtml
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

Auxiliares de marcação fornecem uma experiência de desenvolvimento compatível com HTML e um ambiente rico de IntelliSense para criar marcação HTML e Razor. A maioria os auxiliares de marca internos elementos HTML existentes de destino e fornece os atributos do lado do servidor para o elemento.

### <a name="view-components"></a>Componentes do modo de exibição

[Exibir componentes](views/view-components.md) permitem a lógica de processamento do pacote e reutilizá-lo em todo o aplicativo. Elas são semelhantes às [exibições parciais](views/partial.md), mas com a lógica associada.
