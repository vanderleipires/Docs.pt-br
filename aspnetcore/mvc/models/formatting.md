---
title: "Formato de dados de resposta no ASP.NET MVC de núcleo"
author: ardalis
description: Saiba como formatar os dados de resposta MVC do ASP.NET Core.
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ddda494e0db22031af9d20325e14e8458756cbfd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a>Introdução à formatação de dados de resposta no ASP.NET MVC de núcleo

Por [Steve Smith](https://ardalis.com/)

Núcleo do ASP.NET MVC tem suporte interno para formatação de dados de resposta, usando formatos fixos ou em resposta às especificações do cliente.

[Exibir ou baixar o exemplo do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample).

## <a name="format-specific-action-results"></a>Resultados de ação específica de formato

Alguns tipos de resultado de ação são específicos para um determinado formato, como `JsonResult` e `ContentResult`. Ações podem retornar resultados específicos que são sempre formatados de maneira específica. Por exemplo, retornando um `JsonResult` retornará dados formatados em JSON, independentemente de preferências do cliente. Da mesma forma, retornando um `ContentResult` retornará dados de cadeia de caracteres formatada para texto sem formatação (assim como simplesmente retornar uma cadeia de caracteres).

> [!NOTE]
> Uma ação não é necessário para retornar qualquer tipo determinado; MVC oferece suporte a qualquer valor de retorno do objeto. Se uma ação retorna um `IActionResult` herda de implementação e o controlador de `Controller`, os desenvolvedores têm muitos métodos auxiliares correspondente a muitas das opções. Resultados de ações que retornam objetos que não são `IActionResult` tipos serão serializados usando apropriada `IOutputFormatter` implementação.

Para retornar dados em um formato específico de um controlador que herda o `Controller` classe base, use o método auxiliar interno `Json` para retornar JSON e `Content` para texto sem formatação. O método de ação deve retornar o tipo de resultado específico (por exemplo, `JsonResult`) ou `IActionResult`.

Retornando dados formatados em JSON:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

Exemplo de resposta desta ação:

![Guia de rede das ferramentas de desenvolvedor no Microsoft Edge mostrando o tipo de conteúdo da resposta é application/json](formatting/_static/json-response.png)

Observe que o tipo de conteúdo da resposta é `application/json`, conforme mostrado na lista de solicitações de rede e na seção de cabeçalhos de resposta. Além disso, observe a lista de opções apresentadas pelo navegador (nesse caso, Microsoft Edge) no cabeçalho Accept na seção de cabeçalhos de solicitação. A técnica atual está ignorando esse cabeçalho. obedecer a ele é discutido abaixo.

Para retornar dados de texto simples formatado, use `ContentResult` e `Content` auxiliar:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

Uma resposta desta ação:

![Guia de rede das ferramentas de desenvolvedor no Microsoft Edge mostrando o tipo de conteúdo da resposta é texto/sem formatação](formatting/_static/text-response.png)

Observe neste caso o `Content-Type` retornado é `text/plain`. Você também pode obter esse mesmo comportamento usando apenas um tipo de resposta de cadeia de caracteres:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> Para ações não trivial com várias opções (por exemplo, códigos de status HTTP diferentes com base no resultado de operações executadas) ou tipos de retorno, prefira `IActionResult` como o tipo de retorno.

## <a name="content-negotiation"></a>Negociação de conteúdo

Negociação de conteúdo (*conneg* abreviada) ocorre quando o cliente especifica uma [cabeçalho Accept](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html). O formato padrão usado pelo ASP.NET MVC de núcleo é JSON. Negociação de conteúdo é implementada pelo `ObjectResult`. Ele também é compilado em código de status retornados de resultados de ação específica dos métodos auxiliares (que se baseiam em `ObjectResult`). Você também pode retornar um tipo de modelo (uma classe que você definiu como seu tipo de transferência de dados) e o framework quebrará automaticamente em um `ObjectResult` para você.

O método de ação a seguir usa o `Ok` e `NotFound` métodos auxiliares:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

Retornará uma resposta formatada em JSON, a menos que outro formato foi solicitado e o servidor pode retornar o formato solicitado. Você pode usar uma ferramenta como [Fiddler](http://www.telerik.com/fiddler) para criar uma solicitação que inclui um cabeçalho Accept e especifique outro formato. Nesse caso, se o servidor tiver um *formatador* que pode produzir uma resposta no formato solicitado, o resultado será retornado no formato preferencial de cliente.

![Console do Fiddler mostrando um criados manualmente obter solicitação com um valor de cabeçalho Accept de application/xml](formatting/_static/fiddler-composer.png)

A captura de tela acima, o criador do Fiddler foi usado para gerar uma solicitação, especificando `Accept: application/xml`. Por padrão, MVC do ASP.NET Core só dá suporte a JSON, portanto, mesmo quando outro formato é especificado, o resultado retornado é ainda formatada em JSON. Você verá como adicionar formatadores adicionais na próxima seção.

Ações do controlador podem retornar POCOs (Plain Old CLR Objects), caso em que o ASP.NET MVC criará automaticamente um `ObjectResult` para você que encapsula o objeto. O cliente receberá o objeto serializado formatado (formato JSON é o padrão; você pode configurar outros formatos ou XML). Se o objeto que está sendo retornado é `null`, a estrutura será retornado um `204 No Content` resposta.

Retornando um tipo de objeto:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

No exemplo, uma solicitação para um alias válido autor receberá uma resposta 200 Okey com dados do autor. Uma solicitação para um alias inválido receberá uma resposta de 204 sem conteúdo. Capturas de tela mostrando a resposta nos formatos XML e JSON são mostradas abaixo.

### <a name="content-negotiation-process"></a>Processo de negociação de conteúdo

Conteúdo *negociação* só ocorre se um `Accept` cabeçalho aparece na solicitação. Quando uma solicitação contém um cabeçalho accept, o framework vai enumerar os tipos de mídia no cabeçalho accept na ordem de preferência e tentará encontrar um formatador que pode produzir uma resposta em um dos formatos especificados pelo cabeçalho accept. No caso de nenhum formatador for encontrado que pode satisfazer a solicitação do cliente, a estrutura tentará localizar o primeiro formatador que pode produzir uma resposta (a menos que o desenvolvedor tenha configurado a opção em `MvcOptions` para retornar 406 não aceitável em vez disso). Se a solicitação especifica XML, mas o formatador XML não tiver sido configurado, será usado o formatador JSON. Geralmente, se nenhum formatador for configurado que pode fornecer o formato solicitado e o primeiro formatador que pode formatar o objeto é usado. Se nenhum cabeçalho for especificado, o primeiro formatador que pode lidar com o objeto a ser retornado será usado para serializar a resposta. Nesse caso, não há nenhuma negociação ocorra - o servidor é determinar em qual formato será usado.

> [!NOTE]
> Se o cabeçalho Accept contém `*/*`, o cabeçalho será ignorado, a menos que `RespectBrowserAcceptHeader` é definido como true no `MvcOptions`.

### <a name="browsers-and-content-negotiation"></a>Navegadores e negociação de conteúdo

Diferentemente dos clientes de API típica, os navegadores da web tendem a fornecer `Accept` cabeçalhos que incluem uma ampla variedade de formatos, incluindo caracteres curinga. Por padrão, quando o framework detecta que a solicitação é proveniente de um navegador, ele ignorará o `Accept` cabeçalho e retorno em vez disso, o conteúdo do aplicativo configurada do formato padrão (JSON, a menos que o contrário configurada). Isso fornece uma experiência mais consistente ao usar navegadores diferentes para consumir APIs.

Se você preferir cabeçalhos de aceitação do seu navegador de honrar do aplicativo, você pode configurar isso como parte da configuração do MVC definindo `RespectBrowserAcceptHeader` para `true` no `ConfigureServices` método *Startup.cs*.

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>Configurando formatadores

Se seu aplicativo precisa para dar suporte a formatos adicionais além do padrão de JSON, você pode adicionar pacotes do NuGet e configurar MVC para dar suporte a elas. Há formatadores separados para entrada e saída. Formatadores de entrada são usados por [modelo associação](model-binding.md); formatadores de saída são usados para formatar as respostas. Você também pode configurar [personalizado formatadores](../advanced/custom-formatters.md).

### <a name="adding-xml-format-support"></a>Adicionando suporte para o formato XML

Para adicionar suporte para formatação XML, instale o `Microsoft.AspNetCore.Mvc.Formatters.Xml` pacote NuGet.

Adicionar o XmlSerializerFormatters a configuração do MVC no *Startup.cs*:

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

Como alternativa, você pode adicionar apenas o formatador de saída:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

Essas duas abordagens serializa os resultados usando `System.Xml.Serialization.XmlSerializer`. Se preferir, você pode usar o `System.Runtime.Serialization.DataContractSerializer` adicionando o formatador associado:

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

Depois de adicionar suporte para formatação XML, seus métodos de controlador devem retornar o formato apropriado com base na solicitação de `Accept` cabeçalho, como este exemplo demonstra o Fiddler:

![Console do Fiddler: guia o bruto para a solicitação mostra o valor do cabeçalho Accept é aplicativo/xml. A guia bruta para a resposta mostra o valor do cabeçalho Content-Type de application/xml.](formatting/_static/xml-response.png)

Você pode ver na guia inspetores que a solicitação GET bruto foi feita com um `Accept: application/xml` conjunto de cabeçalhos. O painel de resposta mostra o `Content-Type: application/xml` cabeçalho e o `Author` objeto tem foi serializado como XML.

Use a guia criador para modificar a solicitação para especificar `application/json` no `Accept` cabeçalho. Executar a solicitação e a resposta será formatada como JSON:

![Console do Fiddler: guia o bruto para a solicitação mostra o valor do cabeçalho Accept é application/json. A guia bruta para a resposta mostra o valor do cabeçalho Content-Type do aplicativo/json.](formatting/_static/json-response-fiddler.png)

Nessa tela, você pode ver a solicitação define um cabeçalho de `Accept: application/json` e a resposta especifica o mesmo que seu `Content-Type`. O `Author` objeto é mostrado no corpo da resposta no formato JSON.

### <a name="forcing-a-particular-format"></a>Forçar um formato específico

Se você quiser restringir os formatos de resposta para uma ação específica é possível, você pode aplicar o `[Produces]` filtro. O `[Produces]` filtro especifica os formatos de resposta para uma ação específica (ou controlador). Como a maioria [filtros](../controllers/filters.md), isso pode ser aplicado no escopo global, controlador ou ação.

```csharp
[Produces("application/json")]
public class AuthorsController
```

O `[Produces]` filtro força todas as ações dentro a `AuthorsController` para retornar respostas formatadas em JSON, mesmo se outros formatadores foram configurados para o aplicativo e o cliente fornecida um `Accept` cabeçalho de solicitação diferente, disponível formato. Consulte [filtros](../controllers/filters.md) para saber mais, incluindo a aplicação de filtros globais.

### <a name="special-case-formatters"></a>Formatadores casos especiais

Alguns casos especiais são implementados usando formatadores internos. Por padrão, `string` tipos de retorno serão formatados como *texto/simples* (*texto/html* se solicitado `Accept` cabeçalho). Esse comportamento pode ser removido, removendo o `TextOutputFormatter`. Remover formatadores no `Configure` método *Startup.cs* (mostrada abaixo). Ações com um objeto de modelo retornam tipo retornará um 204 sem conteúdo resposta ao retornar `null`. Esse comportamento pode ser removido, removendo o `HttpNoContentOutputFormatter`. O código a seguir remove o `TextOutputFormatter` e `HttpNoContentOutputFormatter`.

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

Sem o `TextOutputFormatter`, `string` retornar tipos de retorno 406 não aceitável, por exemplo. Observe que se um formatador XML existir, ele será formato `string` tipos de retorno se a `TextOutputFormatter` é removido.

Sem o `HttpNoContentOutputFormatter`, objetos nulos são formatados usando o formatador configurado. Por exemplo, o formatador JSON simplesmente retornará uma resposta com um corpo de `null`, enquanto o formatador XML retorna um elemento XML vazio com o atributo `xsi:nil="true"` definido.

## <a name="response-format-url-mappings"></a>Mapeamentos de URL do formato de resposta

Os clientes podem solicitar um formato específico como parte da URL, como na cadeia de caracteres de consulta ou parte do caminho, ou usando uma extensão de arquivo de formato específico como. XML ou. JSON. O mapeamento de caminho da solicitação deve ser especificado na rota que está usando a API. Por exemplo:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

Essa rota permitiria que o formato solicitado ser especificado como uma extensão de arquivo opcional. O `[FormatFilter]` atributo verifica a existência do valor no formato de `RouteData` e mapeará o formato da resposta para o formatador adequado quando a resposta é criada.

| Rota                      | Formatador                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | O formatador de saída padrão       |
| `/products/GetById/5.json` | O formatador JSON (se configurado) |
| `/products/GetById/5.xml`  | O formatador XML (se configurado)  |
