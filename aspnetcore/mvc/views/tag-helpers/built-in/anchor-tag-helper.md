---
title: "Auxiliar de Marca de Âncora no ASP.NET Core"
author: pkellner
description: "Mostra como trabalhar com o Auxiliar de marca de âncora"
manager: wpickett
ms.author: riande
ms.date: 12/20/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 404fc7bc3b35114066f035e1ac28d10a8279ccbc
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/30/2018
---
# <a name="anchor-tag-helper"></a>Auxiliar de marca de âncora

Por [Peter Kellner](http://peterkellner.net) 

O Auxiliar de marca de âncora aprimora a marca de âncora HTML (`<a ... ></a>`) adicionando novos atributos. O link gerado (na marca `href`) é criado usando os novos atributos. Essa URL pode incluir um protocolo opcional, como HTTPS.

O controlador de alto-falante abaixo é usado nos exemplos deste documento.

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Atributos do Auxiliar de marca de âncora

### <a name="asp-controller"></a>asp-controller

`asp-controller` é usado para associar qual controlador será usado para gerar a URL. Os controladores especificados devem existir no projeto atual. O código a seguir lista todos os alto-falantes: 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

A marcação gerada será:

```html
<a href="/Speaker">All Speakers</a>
```

Se o `asp-controller` for especificado e `asp-action` não for, o `asp-action` padrão será o método do controlador padrão da exibição em execução. Ou seja, no exemplo acima, se `asp-action` for deixado de fora e este Auxiliar de Marca de Âncora for gerado do (**/Home**) da exibição `Index` de *HomeController*, a marcação gerada será:

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>asp-action

`asp-action` é o nome do método de ação no controlador que será incluído no `href` gerado. Por exemplo, o código a seguir define o `href` gerado para apontar para a página de detalhes do alto-falante:

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

A marcação gerada será:

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Se nenhum atributo `asp-controller` for especificado, o controlador padrão que está chamando a exibição que executa a exibição atual será usado.  
 
Se o atributo `asp-action` for `Index`, nenhuma ação será anexada à URL, fazendo com que o método `Index` padrão seja chamado. A ação especificada (ou padrão), deve existir no controlador referenciado em `asp-controller`.

### <a name="asp-page"></a>asp-page

Use o atributo `asp-page` em uma marca de âncora para definir sua URL para apontar para uma página específica. Prefixar o nome de página com uma barra "/" cria a URL. A URL no exemplo abaixo aponta para a página "Speaker" no diretório atual.

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

O atributo `asp-page` no exemplo de código anterior renderiza uma saída HTML na exibição semelhante ao trecho a seguir:

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

O atributo `asp-page` é mutuamente exclusivo com os atributos `asp-route`, `asp-controller` e `asp-action`. No entanto, `asp-page` pode ser usado com `asp-route-id` para controlar o roteamento, como mostra o exemplo de código a seguir:

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

O `asp-route-id` produz a saída a seguir:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> Para usar o atributo `asp-page` em páginas Razor, as URLs devem ser um caminho relativo, por exemplo, `"./Speaker"`. Caminhos relativos no atributo `asp-page` não estão disponíveis em exibições MVC. Use a sintaxe "/" para exibições MVC.

### <a name="asp-route-value"></a>asp-route-{value}

`asp-route-` é um prefixo de rota curinga. Qualquer valor colocado após o traço à direita será interpretado como um potencial parâmetro de rota. Se uma rota padrão não for encontrada, esse prefixo de rota será anexado ao href gerado como um valor e parâmetro de solicitação. Caso contrário, ele será substituído no modelo de rota.

Supondo que você tenha um método de controlador definido da seguinte maneira:

```csharp
public IActionResult AnchorTagHelper(string id)
{
    var speaker = new SpeakerData()
    {
        SpeakerId = 12
    };
    return View(viewName, speaker);
}
```

E que tenha o modelo de rota padrão definido em seu *Startup.cs* da seguinte maneira:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

O arquivo **cshtml** que contém o Auxiliar de marca de âncora necessário para usar o parâmetro de modelo **speaker** passado do controlador para a exibição será o seguinte:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

O código HTML gerado, então, será o seguinte, porque a **id** foi encontrada na rota padrão.

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Se o prefixo da rota não fizer parte do modelo de roteamento encontrado, que é o caso com o seguinte arquivo **cshtml**:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

O código HTML gerado, então, será o seguinte, porque **speakerid** não foi encontrado na rota correspondente:

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Se `asp-controller` ou `asp-action` não for especificado, o mesmo processamento padrão será seguido, como no atributo `asp-route`.

### <a name="asp-route"></a>asp-route

`asp-route` possibilita criar uma URL que leva diretamente a uma rota nomeada. Usando atributos de roteamento, uma rota pode ser nomeada como mostrado em `SpeakerController` e usada em seu método `Evaluations`.

`Name = "speakerevals"` instrui o Auxiliar de Marca de Âncora a gerar uma rota diretamente para esse método de controlador usando a URL `/Speaker/Evaluations`. Se `asp-controller` ou `asp-action` for especificado além de `asp-route`, a rota gerada poderá não ser a esperada. `asp-route` não deve ser usado com os atributos `asp-controller` ou `asp-action` para evitar um conflito de rota.

### <a name="asp-all-route-data"></a>asp-all-route-data

`asp-all-route-data` permite criar um dicionário de pares chave-valor, em que a chave é o nome do parâmetro e o valor é o valor associado a essa chave.

Como o exemplo a seguir mostra, um dicionário embutido é criado e os dados são passados para a exibição do Razor. Como alternativa, os dados tambám poderiam ser passados com seu modelo.

```cshtml
@{
    var dict =
        new Dictionary<string, string>
        {
            {"speakerId", "11"},
            {"currentYear", "true"}
        };
}
<a asp-route="speakerevalscurrent"
asp-all-route-data="dict">SpeakerEvals</a>
```

O código anterior gera a seguinte URL: http://localhost/Speaker/EvaluationsCurrent?speakerId=11&currentYear=true

Quando o link é acionado, o método do controlador `EvaluationsCurrent` é chamado. Ele é chamado porque esse controlador tem dois parâmetros de cadeia de caracteres que correspondem ao que foi criado do dicionário `asp-all-route-data`.

Se alguma chave no dicionário for correspondente aos parâmetros da rota, esses valores serão substituídos na rota conforme apropriado e os outros valores não correspondentes serão gerados como parâmetros de solicitação.

### <a name="asp-fragment"></a>asp-fragment

`asp-fragment` define um fragmento de URL para ser acrescentado à URL. O Auxiliar de marca de âncora adicionará o caractere de hash (#). Se você criar uma marca:

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

A URL gerada será: http://localhost/Speaker/Evaluations#SpeakerEvaluations

Marcas de hash são úteis ao criar aplicativos do lado do cliente. Elas podem ser usadas para marcar e pesquisar com facilidade em JavaScript, por exemplo.

### <a name="asp-area"></a>asp-area

`asp-area` define o nome da área que o ASP.NET Core usa para definir a rota apropriada. A seguir, há exemplos de como o atributo de área causa um remapeamento de rotas. Definir `asp-area` como "Blogs" faz com que o diretório `Areas/Blogs` seja prefixado nas rotas dos controladores e exibições associados a essa marca de âncora.

* Nome do projeto
  * wwwroot
  * Áreas
    * Blogs
      * Controladores
        * HomeController.cs
      * Exibições
        * Home
          * Index.cshtml
          * AboutBlog.cshtml
  * Controladores

Especificar uma marca de área válida, como ```area="Blogs"```, ao referenciar o arquivo ```AboutBlog.cshtml``` será semelhante ao seguinte quando o Auxiliar de marca de âncora estiver sendo usado.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

O código HTML gerado incluirá o segmento de áreas e será o seguinte:

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Para que áreas do MVC funcionem em um aplicativo Web, o modelo de rota deve incluir uma referência à área, se ela existir. Esse modelo, que é o segundo parâmetro da chamada de método `routes.MapRoute`, será exibido como: `template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>asp-protocol

O `asp-protocol` é destinado a especificar um protocolo (como `https`) em sua URL. Um exemplo de Auxiliar de marca de âncora que incluir o protocolo será semelhante ao seguinte:

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

e gerará o seguinte HTML:

```<a href="https://localhost/Home/About">About</a>```

O domínio no exemplo é localhost, mas o Auxiliar de marca de âncora usará o domínio público do site ao gerar a URL.

## <a name="additional-resources"></a>Recursos adicionais

* [Áreas](xref:mvc/controllers/areas)
