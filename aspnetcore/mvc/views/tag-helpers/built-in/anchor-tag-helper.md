---
title: Auxiliar de marca de ancoragem | Microsoft Docs
author: pkellner
description: "Mostra como trabalhar com o auxiliar de marca de âncora"
keywords: "ASP.NET Core, auxiliar de marcação"
ms.author: riande
manager: wpickett
ms.date: 12/20/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a011
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/anchor-tag-helper
ms.openlocfilehash: 86756a1d09e6e55ca79aed6e5b718088b82b782c
ms.sourcegitcommit: 2b263e87217658caa42eedc4f9d2d21ef0ab5d59
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/12/2018
---
# <a name="anchor-tag-helper"></a>Auxiliar de marca de âncora

Por [Peter Kellner](http://peterkellner.net) 

O auxiliar de marca de âncora aprimora a âncora HTML (`<a ... ></a>`) marca adicionando novos atributos. O link gerado (no `href` marca) é criado usando os novos atributos. Essa URL pode incluir um protocolo opcional, como HTTP.

O controlador do apresentador abaixo é usado nos exemplos neste documento.

**SpeakerController.cs** 

[!code-csharp[SpeakerController](sample/TagHelpersBuiltInAspNetCore/src/TagHelpersBuiltInAspNetCore/Controllers/SpeakerController.cs)]


## <a name="anchor-tag-helper-attributes"></a>Atributos de auxiliar de marca de âncora

### <a name="asp-controller"></a>controlador de ASP

`asp-controller`é usado para associar o controlador será usado para gerar a URL. Os controladores especificados devem existir no projeto atual. O código a seguir lista todos os alto-falantes: 

```cshtml
<a asp-controller="Speaker" asp-action="Index">All Speakers</a>
```

A marcação gerada será:

```html
<a href="/Speaker">All Speakers</a>
```

Se o `asp-controller` for especificado e `asp-action` não, é o padrão `asp-action` será o método do controlador padrão do modo de exibição atualmente em execução. Que é, no exemplo acima, se `asp-action` é à esquerda, e este auxiliar de marca de âncora é gerado a partir *HomeController*do `Index` exibição (**/home**), a marcação gerada será:

```html
<a href="/Home">All Speakers</a>
```

### <a name="asp-action"></a>ação de ASP

`asp-action`é o nome do método de ação no controlador que serão incluído no gerado `href`. Por exemplo, o código a seguir defina gerado `href` para apontar para a página de detalhes do apresentador:

```html
<a asp-controller="Speaker" asp-action="Detail">Speaker Detail</a>
```

A marcação gerada será:

```html
<a href="/Speaker/Detail">Speaker Detail</a>
```

Se nenhum `asp-controller` atributo for especificado, o controlador padrão chamando o modo de exibição que executar a exibição atual será usado.  
 
Se o atributo `asp-action` é `Index`, em seguida, nenhuma ação é anexada à URL, à esquerda para o padrão `Index` método ser chamado. A ação especificada (ou padrão), deve existir no controlador referenciado em `asp-controller`.

### <a name="asp-page"></a>página ASP

Use o `asp-page` atributo em uma marca de âncora para definir a URL para apontar para uma página específica. Prefixando o nome de página com uma barra "/" cria a URL. A URL de exemplo abaixo aponta para a página "Locutor" no diretório atual.

```cshtml
<a asp-page="/Speakers">All Speakers</a>
```

O `asp-page` atributo no exemplo de código anterior renderiza a saída HTML no modo de exibição semelhante para o trecho a seguir:

```html
<a href="/items?page=%2FSpeakers">Speakers</a>
```

O `asp-page` atributo é mutuamente exclusivo com o `asp-route`, `asp-controller`, e `asp-action` atributos. No entanto, `asp-page` pode ser usado com `asp-route-id` para controlar o roteamento, como mostra o exemplo de código a seguir:

```cshtml
<a asp-page="/Speaker" asp-route-id="@speaker.Id">View Speaker</a>
```

O `asp-route-id` produz a seguinte saída:

```html
https://localhost:44399/Speakers/Index/2?page=%2FSpeaker
```

> [!NOTE]
> Para usar o `asp-page` atributo nas páginas Razor, as URLs deve ser um caminho relativo, por exemplo `"./Speaker"`. Caminhos relativos no `asp-page` atributo não estão disponíveis nos modos de exibição do MVC. Use a sintaxe "/" para exibições do MVC.

### <a name="asp-route-value"></a>ASP - rota-{value}

`asp-route-`é um prefixo de rota de curinga. Qualquer valor colocado após o traço à direita será interpretado como um parâmetro de rota potencial. Se uma rota padrão não for encontrada, esse prefixo de rota será anexado para o href gerado como um parâmetro de solicitação e um valor. Caso contrário, ele será substituído no modelo de rota.

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

E que o modelo de rota padrão definido no seu *Startup.cs* da seguinte maneira:

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{id?}");
});

```

O **cshtml** arquivo que contém o auxiliar de marca de âncora necessário usar o **locutor** parâmetro de modelo passado do controlador para o modo de exibição é o seguinte:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-id=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

O código HTML gerado será da seguinte maneira porque **id** foi encontrado na rota padrão.

```html
<a href='/Speaker/Detail/12'>SpeakerId: 12</a>
```

Se o prefixo da rota não é parte do modelo de roteamento encontrado, que é o caso com o seguinte **cshtml** arquivo:

```cshtml
@model SpeakerData
<!DOCTYPE html>
<html><body>
<a asp-controller='Speaker' asp-action='Detail' asp-route-speakerid=@Model.SpeakerId>SpeakerId: @Model.SpeakerId</a>
<body></html>
```

O código HTML gerado será da seguinte maneira porque **speakerid** não foi encontrado na rota correspondida:

```html
<a href='/Speaker/Detail?speakerid=12'>SpeakerId: 12</a>
```

Se qualquer um dos `asp-controller` ou `asp-action` não forem especificados, e o processamento padrão mesmo é seguido porque está sendo o `asp-route` atributo.

### <a name="asp-route"></a>rota de ASP

`asp-route`Fornece uma maneira de criar uma URL que vincula-se diretamente a uma rota nomeada. Usando atributos de roteamento, uma rota pode ser nomeada como mostra o `SpeakerController` e usado em seu `Evaluations` método.

`Name = "speakerevals"`informa o auxiliar de marca de âncora para gerar uma rota diretamente para esse método de controlador usando a URL `/Speaker/Evaluations`. Se `asp-controller` ou `asp-action` é especificado além `asp-route`, a rota gerada não pode ser o esperado. `asp-route`não deve ser usada com qualquer um dos atributos `asp-controller` ou `asp-action` para evitar um conflito de rota.

### <a name="asp-all-route-data"></a>ASP-all-dados de rota

`asp-all-route-data`permite a criação de um dicionário de pares chave-valor em que a chave é o nome do parâmetro e o valor é o valor associado a essa chave.

Como o exemplo a seguir mostra, um dicionário embutido é criado e os dados são passados para o modo de exibição do razor. Como alternativa, os dados podem ser passados com seu modelo.

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

Quando o link é clicado, o método do controlador `EvaluationsCurrent` é chamado. Ele é chamado porque esse controlador tem dois parâmetros de cadeia de caracteres que correspondem o que foi criado a partir de `asp-all-route-data` dicionário.

Se todas as chaves na correspondência de dicionário de parâmetros de rota, esses valores são substituídos na rota conforme apropriado e os outros valores não correspondentes serão gerados como parâmetros de solicitação.

### <a name="asp-fragment"></a>fragmento de ASP

`asp-fragment`define um fragmento de URL para acrescentar à URL. O auxiliar de marca de âncora adicionará o caractere de hash (#). Se você criar uma marca:

```cshtml
<a asp-action="Evaluations" asp-controller="Speaker"  
   asp-fragment="SpeakerEvaluations">About Speaker Evals</a>
```

A URL gerada será: http://localhost/Speaker/Evaluations#SpeakerEvaluations

Marcas de hash são úteis ao criar aplicativos do lado do cliente. Eles podem ser usados para marcar fácil e pesquisa em JavaScript, por exemplo.

### <a name="asp-area"></a>área de ASP

`asp-area`Define o nome da área que usa o ASP.NET Core para definir a rota apropriada. A seguir estão exemplos de como o atributo área faz com que um remapeamento de rotas. Configuração `asp-area` blogs prefixos de diretório `Areas/Blogs` para as rotas do associado controladores e exibições para a marca de âncora.

* Nome do projeto
  * wwwroot
  * Áreas
    * Blogs
      * Controladores
        * HomeController
      * Exibições
        * Home
          * Cshtml
          * AboutBlog.cshtml
  * Controladores

Especificando uma marca de área é válida, como ```area="Blogs"``` ao referenciar o ```AboutBlog.cshtml``` arquivo será semelhante a seguir usando o auxiliar de marca de âncora.

```cshtml
<a asp-action="AboutBlog" asp-controller="Home" asp-area="Blogs">Blogs About</a>
```

O código HTML gerado incluirá o segmento de áreas e será da seguinte maneira:

```html
<a href="/Blogs/Home/AboutBlog">Blogs About</a>
```

> [!TIP]
> Para as áreas do MVC trabalhar em um aplicativo web, o modelo de rota deve incluir uma referência para a área se ele existir. Esse modelo, que é o segundo parâmetro do `routes.MapRoute` chamada de método, será exibida como:`template: '"{area:exists}/{controller=Home}/{action=Index}"'`

### <a name="asp-protocol"></a>protocolo de ASP

O `asp-protocol` é para especificar um protocolo (como `https`) em sua URL. Um auxiliar de marca de âncora que contenha o protocolo de exemplo será semelhante ao seguinte:

```<a asp-protocol="https" asp-action="About" asp-controller="Home">About</a>```

e irá gerar o HTML da seguinte maneira:

```<a href="https://localhost/Home/About">About</a>```

O domínio no exemplo é localhost, mas o auxiliar de marca de âncora usará o domínio do site público ao gerar a URL.

## <a name="additional-resources"></a>Recursos adicionais

* [Áreas](xref:mvc/controllers/areas)
