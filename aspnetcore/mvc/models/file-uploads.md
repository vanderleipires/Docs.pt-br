---
title: "Carregamentos de arquivos no núcleo do ASP.NET"
author: ardalis
description: "Como usar o modelo de associação e streaming para carregar arquivos no ASP.NET MVC de núcleo."
ms.author: riande
manager: wpickett
ms.date: 07/05/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/file-uploads
ms.openlocfilehash: 3c5abe84a5c7cc399e0586e680a414fab7a26c1d
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/19/2018
---
# <a name="file-uploads-in-aspnet-core"></a>Carregamentos de arquivos no núcleo do ASP.NET

Por [Steve Smith](https://ardalis.com/)

Ações do ASP.NET MVC suportam a carregamento de um ou mais arquivos usando o modelo simples de associação para os arquivos menores ou streaming para arquivos maiores.

[Exibir ou baixar o exemplo do GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/file-uploads/sample/FileUploadSample)

## <a name="uploading-small-files-with-model-binding"></a>Carregando arquivos pequenos com associação de modelo

Para carregar arquivos pequenos, você pode usar um formulário HTML várias parte ou construir uma solicitação POST usando JavaScript. Um formulário de exemplo usando o Razor, que dá suporte a vários arquivos carregados, é mostrado abaixo:

```html
<form method="post" enctype="multipart/form-data" asp-controller="UploadFiles" asp-action="Index">
    <div class="form-group">
        <div class="col-md-10">
            <p>Upload one or more files using this form:</p>
            <input type="file" name="files" multiple />
        </div>
    </div>
    <div class="form-group">
        <div class="col-md-10">
            <input type="submit" value="Upload" />
        </div>
    </div>
</form>
```

Para dar suporte a carregamentos de arquivos, formulários HTML devem especificar um `enctype` de `multipart/form-data`. O `files` mostrado acima do elemento de entrada dá suporte ao carregamento de vários arquivos. Omitir a `multiple` atributo neste elemento de entrada para permitir que apenas um arquivo a ser carregado. Renderiza a marcação acima em um navegador, como:

![Formulário de carregamento de arquivo](file-uploads/_static/upload-form.png)

O carregados no servidor de arquivos individuais podem ser acessados por meio de [associação de modelo](xref:mvc/models/model-binding) usando o [IFormFile](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.iformfile) interface. `IFormFile`tem a seguinte estrutura:

```csharp
public interface IFormFile
{
    string ContentType { get; }
    string ContentDisposition { get; }
    IHeaderDictionary Headers { get; }
    long Length { get; }
    string Name { get; }
    string FileName { get; }
    Stream OpenReadStream();
    void CopyTo(Stream target);
    Task CopyToAsync(Stream target, CancellationToken cancellationToken = null);
}
```

> [!WARNING]
> Não dependa ou relação de confiança de `FileName` propriedade sem validação. O `FileName` propriedade deve ser usada somente para fins de exibição.

Ao carregar arquivos usando a associação de modelo e o `IFormFile` interface, o método de ação pode aceitar um único `IFormFile` ou um `IEnumerable<IFormFile>` (ou `List<IFormFile>`) que representa vários arquivos. O exemplo a seguir executa um loop em um ou mais arquivos carregados, salva-os para o sistema de arquivos local e retorna o número total e o tamanho dos arquivos carregados.

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/UploadFilesController.cs?name=snippet1)]

Os arquivos carregados usando o `IFormFile` técnica são armazenados em buffer na memória ou no disco no servidor web antes de ser processada. Dentro do método de ação, o `IFormFile` conteúdo podem ser acessado como um fluxo. O sistema de arquivos local, além de arquivos podem ser transmitidos para [armazenamento de BLOBs do Azure](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/) ou [do Entity Framework](https://docs.microsoft.com/ef/core/index).

Para armazenar dados de arquivo binário em um banco de dados usando o Entity Framework, definir uma propriedade do tipo `byte[]` na entidade:

```csharp
public class ApplicationUser : IdentityUser
{
    public byte[] AvatarImage { get; set; }
}
```

Especifique uma propriedade de viewmodel do tipo `IFormFile`:

```csharp
public class RegisterViewModel
{
    // other properties omitted

    public IFormFile AvatarImage { get; set; }
}
```

> [!NOTE]
> `IFormFile`pode ser usado diretamente como um parâmetro de método de ação ou como uma propriedade de viewmodel, como mostrado acima.

Copie o `IFormFile` em um fluxo e salvá-lo para a matriz de bytes:

```csharp
// POST: /Account/Register
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Register(RegisterViewModel model)
{
    ViewData["ReturnUrl"] = returnUrl;
    if  (ModelState.IsValid)
    {
        var user = new ApplicationUser {
          UserName = model.Email,
          Email = model.Email
        };
        using (var memoryStream = new MemoryStream())
        {
            await model.AvatarImage.CopyToAsync(memoryStream);
            user.AvatarImage = memoryStream.ToArray();
        }
    // additional logic omitted
    
    // Don't rely on or trust the model.AvatarImage.FileName property 
    // without validation.
}
```

> [!NOTE]
> Tenha cuidado ao armazenar dados binários em bancos de dados relacionais, como ele pode afetar negativamente o desempenho.

## <a name="uploading-large-files-with-streaming"></a>Carregamento de arquivos grandes de streaming

Se o tamanho ou a frequência de carregamentos de arquivos está causando problemas de recurso para o aplicativo, considere o carregamento do arquivo de streaming em vez de armazenamento em buffer em sua totalidade, assim como a abordagem de associação de modelo mostrada acima. Ao usar `IFormFile` e associação de modelo é uma quantidade solução mais simples, transmissão exige um número de etapas para implementar corretamente.

> [!NOTE]
> Qualquer arquivo armazenado em buffer único exceder 64KB será movido de RAM para um arquivo temporário em disco no servidor. Os recursos (disco RAM) usados pelo carregamentos de arquivos dependem do número e tamanho dos carregamentos de arquivo simultâneas. Fluxo não é muito sobre o desempenho, trata-se de escala. Se você tentar carregamentos de excesso de buffer, seu site falhará quando ele ficar sem memória ou espaço em disco.

O exemplo a seguir demonstra como usar JavaScript/Angular para transmitir para uma ação do controlador. Token antiforgery do arquivo é gerado com um atributo de filtro personalizado e passada nos cabeçalhos HTTP em vez de no corpo da solicitação. Como o método de ação processa os dados carregados diretamente, a associação de modelo é desabilitada por outro filtro. Em ação, o conteúdo do formulário é lidos usando um `MultipartReader`, que lê cada indivíduo `MultipartSection`, processar o arquivo ou armazenar o conteúdo conforme apropriado. Depois que todas as seções foram lidos, a ação executa sua própria associação de modelo.

A ação inicial carrega o formulário e salva um token antiforgery em um cookie (por meio de `GenerateAntiforgeryTokenCookieForAjax` atributo):

```csharp
[HttpGet]
[GenerateAntiforgeryTokenCookieForAjax]
public IActionResult Index()
{
    return View();
}
```

O atributo usa internas do ASP.NET Core [Antiforgery](xref:security/anti-request-forgery) suporte para definir um cookie com um token de solicitação:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/GenerateAntiforgeryTokenCookieForAjaxAttribute.cs?name=snippet1)]

Angular automaticamente passa um token antiforgery em um cabeçalho de solicitação chamado `X-XSRF-TOKEN`. O aplicativo ASP.NET MVC de núcleo é configurado para se referir a esse cabeçalho em sua configuração no *Startup.cs*:

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Startup.cs?name=snippet1)]

O `DisableFormValueModelBinding` atributo, mostrado abaixo, é usado para desabilitar a associação de modelo para o `Upload` método de ação.

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Filters/DisableFormValueModelBindingAttribute.cs?name=snippet1)]

Desde que a associação de modelo é desabilitada, o `Upload` método de ação não aceita parâmetros. Ele trabalha diretamente com o `Request` propriedade `ControllerBase`. Um `MultipartReader` é usado para ler cada seção. O arquivo é salvo com um nome de arquivo GUID e os dados de chave/valor são armazenados em um `KeyValueAccumulator`. Depois que todas as seções tenham sido lidos, o conteúdo do `KeyValueAccumulator` são usados para associar os dados de formulário para um tipo de modelo.

Completo `Upload` método é mostrado abaixo:

[!INCLUDE [GetTempFileName](../../includes/GetTempFileName.md)]

[!code-csharp[Main](file-uploads/sample/FileUploadSample/Controllers/StreamingController.cs?name=snippet1)]

## <a name="troubleshooting"></a>Solução de problemas

Abaixo estão alguns problemas comuns encontrados ao trabalhar com o carregamento de arquivos e suas soluções possíveis.

### <a name="unexpected-not-found-error-with-iis"></a>Erro inesperado não encontrado com o IIS

O erro a seguir indica que o carregamento do arquivo excede o servidor configurado `maxAllowedContentLength`:

```
HTTP 404.13 - Not Found
The request filtering module is configured to deny a request that exceeds the request content length.
```

A configuração padrão é `30000000`, que é aproximadamente 28.6 MB. O valor pode ser personalizado por meio da edição *Web. config*:

```xml
<system.webServer>
  <security>
    <requestFiltering>
      <!-- This will handle requests up to 50MB -->
      <requestLimits maxAllowedContentLength="52428800" />
    </requestFiltering>
  </security>
</system.webServer>
```

Essa configuração só se aplica ao IIS. O comportamento não ocorre por padrão quando Kestrel de hospedagem. Para obter mais informações, consulte [limites de solicitações \<requestLimits\>](https://docs.microsoft.com/iis/configuration/system.webServer/security/requestFiltering/requestLimits/).

### <a name="null-reference-exception-with-iformfile"></a>Exceção de referência nula com IFormFile

Se o controlador está aceitando carregar arquivos usando `IFormFile` , mas você achar que o valor é sempre nulo, confirme que o formulário HTML é especificando um `enctype` valor `multipart/form-data`. Se esse atributo não for definido no `<form>` elemento, o carregamento de arquivo não ocorrerá e nenhum limite `IFormFile` argumentos será nulos.
