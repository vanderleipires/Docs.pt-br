---
title: "Trabalhar com arquivos estáticos no núcleo do ASP.NET"
author: rick-anderson
description: "Aprenda a atender e proteger arquivos estáticos e configurar arquivos estáticos hospedagem comportamentos de middleware em um aplicativo web do ASP.NET Core."
keywords: "ASP.NET Core, arquivos estáticos, ativos estáticos, HTML, CSS, JavaScript"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 912923860939a1d1dd91ccc79862e23f9095d161
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/18/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a>Trabalhar com arquivos estáticos no núcleo do ASP.NET

Por [Rick Anderson](https://twitter.com/RickAndMSFT) e [Scott Addie](https://twitter.com/Scott_Addie)

Arquivos estáticos, como HTML, CSS, imagens e JavaScript, estão ativos que atua como um aplicativo do ASP.NET Core diretamente para clientes. Algumas etapas de configuração é necessária para habilitar o fornecimento desses arquivos.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="serve-static-files"></a>Servir arquivos estáticos

Arquivos estáticos são armazenados no diretório de raiz do projeto da web. O diretório padrão é  *\<content_root > / wwwroot*, mas pode ser alterada por meio de [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) método. Consulte [conteúdo raiz](xref:fundamentals/index#content-root) e [raiz Web](xref:fundamentals/index#web-root) para obter mais informações.

O host do aplicativo da web deve ser informado do diretório raiz de conteúdo.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

O `WebHost.CreateDefaultBuilder` método define a raiz de conteúdo para o diretório atual:

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Definir a raiz de conteúdo para o diretório atual invocando [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) dentro de `Program.Main`:

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

Arquivos estáticos são acessíveis por meio de um caminho relativo à raiz da web. Por exemplo, o **aplicativo Web** várias pastas dentro do modelo de projeto contém o *wwwroot* pasta:

* **wwwroot**
  * **css**
  * **images**
  * **js**

O formato URI para acessar um arquivo no *imagens* subpasta é *http://\<server_address > /images/\<image_file_name >*. Por exemplo, *http://localhost:9189/images/banner3.svg*.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

Se o destino do .NET Framework, adicione o [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pacote ao seu projeto. Se o objetivo principal do .NET, o [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) inclui esse pacote.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

Adicionar o [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) pacote ao seu projeto.

---

Configurar o [middleware](xref:fundamentals/middleware) que permite o fornecimento de arquivos estáticos.

### <a name="serve-files-inside-of-web-root"></a>Serve arquivos dentro da raiz da web

Invocar o [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) método dentro de `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

Sem o parâmetro `UseStaticFiles` sobrecarga do método marca os arquivos na raiz da web como servable. As seguintes referências de marcação *wwwroot/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a>Serve arquivos fora da raiz da web

Considere uma hierarquia de diretórios na qual os arquivos estáticos sejam atendidos residam fora da raiz da web:

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*

Uma solicitação pode acessar o *banner1.svg* arquivo Configurando o middleware de arquivo estático da seguinte maneira:

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

No código anterior, o *MyStaticFiles* hierarquia de diretórios é exposta publicamente por meio de *StaticFiles* segmento do URI. Uma solicitação para *http://\<server_address > /StaticFiles/images/banner1.svg* serve a *banner1.svg* arquivo.

As seguintes referências de marcação *MyStaticFiles/images/banner1.svg*:

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a>Definir cabeçalhos de resposta HTTP

Um [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) objeto pode ser usado para definir cabeçalhos de resposta HTTP. Além de configurar os serviços de arquivo estático da raiz da web, o código a seguir define o `Cache-Control` cabeçalho:

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

O [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) método existe o [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) pacote.

Os arquivos foram feitos publicamente armazenável em cache por 10 minutos (600 segundos):

![Mostra o cabeçalho Cache-Control de cabeçalhos de resposta foi adicionado](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Autorização de arquivo estático

O middleware de arquivo estático não fornece verificações de autorização. Todos os arquivos servidas por ele, incluindo aqueles em *wwwroot*, podem ser acessados publicamente. Para servir arquivos com base na autorização:

* Armazená-los fora do *wwwroot* e qualquer diretório acessível para o middleware de arquivo estático **e**
* Servi-los por meio de um método de ação para o qual a autorização é aplicada. Retornar um [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) objeto:

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a>Habilitar a pesquisa no diretório

Pesquisa no diretório permite que os usuários de seu aplicativo web ver uma lista de pastas e arquivos em um diretório especificado. Pesquisa no diretório é desabilitada por padrão por motivos de segurança (consulte [considerações](#considerations)). Habilitar pesquisa invocando no diretório de [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) método `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

Adicionar serviços necessários invocando o [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) método de `Startup.ConfigureServices`:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

O código anterior permite que a pesquisa no diretório do *wwwroot/imagens* pasta usando a URL *http://\<server_address > / MyImages*, com links para cada arquivo e pasta:

![pesquisa no diretório](static-files/_static/dir-browse.png)

Consulte [considerações](#considerations) sobre os riscos de segurança ao habilitar a pesquisa.

Observe os dois `UseStaticFiles` chama no exemplo a seguir. A primeira chamada permite o fornecimento de arquivos estáticos no *wwwroot* pasta. A segunda chamada permite que a pesquisa no diretório do *wwwroot/imagens* pasta usando a URL *http://\<server_address > / MyImages*:

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a>Fornecer um documento padrão

Definir uma home page padrão fornece ao visitantes um ponto de partida lógico de ao visitar o site. Para atender a uma página padrão sem qualificar totalmente o URI do usuário, chame o [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) método de `Startup.Configure`:

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> `UseDefaultFiles`deve ser chamado antes de `UseStaticFiles` para servir o arquivo padrão. `UseDefaultFiles`é um regravador de URL que, na verdade, não servem para o arquivo. Habilitar o middleware de arquivo estático via `UseStaticFiles` para servir o arquivo.

Com `UseDefaultFiles`, solicitações para pesquisar uma pasta:

* *default.htm*
* *default.html*
* *index.htm*
* *index.html*

O primeiro arquivo encontrado na lista é tratado como se a solicitação fosse o URI totalmente qualificado. Continua a URL do navegador refletir o URI solicitado.

O código a seguir altera o nome de arquivo padrão para *mydefault.html*:

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a>UseFileServer

[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combina a funcionalidade de `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`.

O código a seguir permite que o servidor de arquivos estáticos e o arquivo padrão. Pesquisa no diretório não está habilitada.

```csharp
app.UseFileServer();
```

O código a seguir baseia-se a sobrecarga sem parâmetros, permitindo a pesquisa no diretório:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

Considere a seguinte hierarquia de diretório:

* **wwwroot**
  * **css**
  * **images**
  * **js**
* **MyStaticFiles**
  * **images**
      * *banner1.svg*
  * *default.html*

O código a seguir permite que arquivos estáticos, arquivos padrão e a pesquisa no diretório de `MyStaticFiles`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

`AddDirectoryBrowser`deve ser chamado quando o `EnableDirectoryBrowsing` é o valor da propriedade `true`:

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

URLs de código usando a hierarquia de arquivos e o anterior, resolver da seguinte maneira:

| URI            |                             Resposta  |
| ------- | ------|
| *http://\<server_address>/StaticFiles/images/banner1.svg*    |      MyStaticFiles/images/banner1.svg |
| *http://\<server_address>/StaticFiles*             |     MyStaticFiles/default.html |

Se nenhum arquivo nomeados padrão existe o *MyStaticFiles* diretório, *http://\<server_address > / StaticFiles* retorna o diretório listando com links clicáveis:

![Lista de arquivos estáticos](static-files/_static/db2.png)

> [!NOTE]
> `UseDefaultFiles`e `UseDirectoryBrowser` usar a URL *http://\<server_address > / StaticFiles* sem a barra à direita para disparar um cliente redirecionar *http://\<server_address > / StaticFiles /*. Observe a adição da barra à direita. URLs relativas dentro dos documentos são considerados inválidos sem uma barra à direita.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

O [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) classe contém um `Mappings` propriedade servindo como um mapeamento de extensões de arquivo para tipos de conteúdo MIME. No exemplo a seguir, são registradas várias extensões de arquivo para os tipos MIME conhecidos. O *. rtf* extensão é substituída, e *. mp4* é removido.

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

Consulte [tipos de conteúdo MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Tipos de conteúdo não padrão

O middleware de arquivo estático compreende quase 400 tipos de conteúdo de arquivo conhecidos. Se o usuário solicita um arquivo de um tipo de arquivo desconhecido, o middleware de arquivo estático retorna uma resposta HTTP 404 (não encontrado). Se a pesquisa no diretório estiver habilitada, será exibido um link para o arquivo. O URI retorna um erro HTTP 404.

O código a seguir habilita atendendo tipos desconhecidos e renderiza o arquivo desconhecido como uma imagem:

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

Com o código anterior, uma solicitação para um arquivo com um tipo de conteúdo desconhecido é retornada como uma imagem.

> [!WARNING]
> Habilitando [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) é um risco de segurança. Ele é desabilitado por padrão, e seu uso não é recomendado. [FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) fornece uma alternativa mais segura para servir arquivos com extensões não padrão.

### <a name="considerations"></a>Considerações

> [!WARNING]
> `UseDirectoryBrowser`e `UseStaticFiles` podem vazar segredos. Desabilitar pesquisa na produção no diretório é altamente recomendável. Leia com atenção os diretórios que são habilitados por meio de `UseStaticFiles` ou `UseDirectoryBrowser`. Todo o diretório e seus subdiretórios se tornar acessíveis publicamente. Armazenamento de arquivos adequado para serviços ao público em um diretório dedicado, como  *\<content_root > / wwwroot*. Esses arquivos separados de exibições do MVC, páginas Razor (2. x somente), arquivos de configuração, etc.

* As URLs para conteúdo exposto com `UseDirectoryBrowser` e `UseStaticFiles` estão sujeitos a diferenciação de maiusculas e minúsculas e restrições de caracteres do sistema de arquivos subjacente. Por exemplo, Windows diferencia maiusculas de minúsculas&mdash;Mac e Linux não são.

* Aplicativos do ASP.NET Core hospedados no IIS use o [ASP.NET Core módulo (ANCM)](xref:fundamentals/servers/aspnet-core-module) para encaminhar todas as solicitações para o aplicativo, inclusive solicitações de arquivo estático. O manipulador de arquivo estático do IIS não é usado. Ele tem nenhuma possibilidade de manipular solicitações antes que eles são manipulados pelo ANCM.

* Conclua as etapas a seguir no Gerenciador do IIS para remover o manipulador de arquivo estático no IIS no nível do servidor ou site:
    1. Navegue até o **módulos** recurso.
    1. Selecione **StaticFileModule** na lista.
    1. Clique em **remover** no **ações** barra lateral.

> [!WARNING]
> Se o manipulador de arquivo estático no IIS é habilitado **e** o ANCM está configurado incorretamente, arquivos estáticos são atendidos. Isso acontece, por exemplo, se o *Web. config* arquivo não está implantado.

* Colocar arquivos de código (incluindo *. CS* e *. cshtml*) fora da aplicativo raiz do projeto da web. Uma separação lógica, portanto, é criada entre o aplicativo cliente conteúdo e código de servidor. Isso impede que o código do lado do servidor vazem.

## <a name="additional-resources"></a>Recursos adicionais

* [Middleware](xref:fundamentals/middleware)

* [Introdução ao ASP.NET Core](xref:index)