---
title: "Trabalhando com arquivos estáticos no núcleo do ASP.NET"
author: rick-anderson
description: "Trabalhando com arquivos estáticos"
keywords: "ASP.NET Core, arquivos estáticos, ativos estáticos, HTML, CSS, JavaScript"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ea6c180332dd5ab3a7238dcd73a4a1c8534c6243
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-working-with-static-files-in-aspnet-core"></a>Introdução ao trabalho com arquivos estáticos no núcleo do ASP.NET

<a name=fundamentals-static-files></a>

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Arquivos estáticos, como HTML, CSS, imagem e JavaScript, estão ativos que pode ser usado por um aplicativo do ASP.NET Core diretamente aos clientes.

[Exibir ou baixar o código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample)

## <a name="serving-static-files"></a>Servir arquivos estáticos

Arquivos estáticos geralmente estão localizados no `web root` (*\<conteúdo raiz > / wwwroot*) pasta. Consulte [conteúdo raiz](xref:fundamentals/index#content-root) e [raiz Web](xref:fundamentals/index#web-root) para obter mais informações. Você geralmente define a raiz de conteúdo para ser o diretório atual para que seu projeto `web root` estão em desenvolvimento.

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

Arquivos estáticos podem ser armazenados em qualquer pasta sob o `web root` e acessado com um caminho relativo para essa raiz. Por exemplo, quando você cria um projeto de aplicativo Web padrão usando o Visual Studio, há várias pastas criadas dentro de *wwwroot* pasta - *css*, *imagens*, e *js*. O URI para acessar uma imagem no *imagens* subpasta:

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

Para arquivos estáticos sejam atendidos, você deve configurar o [Middleware](middleware.md) para adicionar arquivos estáticos para o pipeline. O middleware de arquivo estático pode ser configurado com a adição de uma dependência no *Microsoft.AspNetCore.StaticFiles* pacote no seu projeto e, em seguida, chamar o `UseStaticFiles` método de extensão de `Startup.Configure`:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

`app.UseStaticFiles();`permite que os arquivos em `web root` (*wwwroot* por padrão) servable. Posteriormente, mostrarei como fazer outro conteúdo do diretório servable com `UseStaticFiles`.

Você deve incluir o pacote NuGet "Microsoft.AspNetCore.StaticFiles".

> [!NOTE]
> `web root`assume como padrão o *wwwroot* diretório, mas você pode definir o `web root` diretório com `UseWebRoot`.

Suponha que você tem uma hierarquia de projeto em que os arquivos estáticos que você deseja atender estão fora de `web root`. Por exemplo:

* wwwroot
  * CSS
  * imagens
  * ...
* MyStaticFiles
  * Test.PNG

Para uma solicitação de acesso *test.png*, configure o middleware de arquivos estáticos da seguinte maneira:

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

Uma solicitação para `http://<app>/StaticFiles/test.png` servirá o *test.png* arquivo.

`StaticFileOptions()`pode definir cabeçalhos de resposta. Por exemplo, o código a seguir define a servir de arquivos estáticos a *wwwroot* pasta e define o `Cache-Control` cabeçalho para torná-los publicamente armazenável em cache por 10 minutos (600 segundos):

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

![Mostra o cabeçalho Cache-Control de cabeçalhos de resposta foi adicionado](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a>Autorização de arquivo estático

O módulo de arquivo estático fornece **sem** verificações de autorização. Todos os arquivos servidas por ele, incluindo aqueles em *wwwroot* publicamente disponíveis. Para servir arquivos com base na autorização:

* Armazená-los fora do *wwwroot* e qualquer diretório acessível para o middleware de arquivo estático **e**

* Servi-los por meio de uma ação do controlador, retornando um `FileResult` onde a autorização é aplicada

## <a name="enabling-directory-browsing"></a>Habilitando a pesquisa no diretório

Pesquisa no diretório permite que o usuário de seu aplicativo web ver uma lista de diretórios e arquivos em um diretório especificado. Pesquisa no diretório é desabilitada por padrão por motivos de segurança (consulte [considerações](#considerations)). Para habilitar a pesquisa no diretório, chamar o `UseDirectoryBrowser` método de extensão de `Startup.Configure`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

E adicionar os serviços necessários chamando `AddDirectoryBrowser` método de extensão de `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

O código acima permite que a pesquisa no diretório do *wwwroot/imagens* pasta usando a URL http://\<aplicativo > / MyImages, com links para cada arquivo e pasta:

![pesquisa no diretório](static-files/_static/dir-browse.png)

Consulte [considerações](#considerations) sobre os riscos de segurança ao habilitar a pesquisa.

Observe os dois `app.UseStaticFiles` chamadas. O primeiro deles é necessário para atender a CSS, imagens e JavaScript no *wwwroot* pasta e a segunda chamada para a pesquisa no diretório do *wwwroot/imagens* pasta usando a URL http://\<aplicativo > / MyImages:

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a>Atendendo a um documento padrão

Definir uma home page padrão fornece os visitantes do site um ponto de partida ao visitar o site. Para seu aplicativo Web atender a uma página padrão sem ter que qualificar totalmente o URI do usuário, chamar o `UseDefaultFiles` método de extensão de `Startup.Configure` da seguinte maneira.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> `UseDefaultFiles`deve ser chamado antes de `UseStaticFiles` para servir o arquivo padrão. `UseDefaultFiles`é um gravador de nova URL que, na verdade, não servem para o arquivo. Você deve habilitar o middleware de arquivo estático (`UseStaticFiles`) para servir o arquivo.

Com `UseDefaultFiles`, solicitações em uma pasta pesquisará:

* default.htm
* Default
* index.htm
* index

O primeiro arquivo encontrado na lista será disponibilizado como se a solicitação foi o URI totalmente qualificado (embora a URL do navegador continuarão mostrando o URI solicitado).

O código a seguir mostra como alterar o nome de arquivo padrão para *mydefault.html*.

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a>UseFileServer

`UseFileServer`combina a funcionalidade de `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`.

O código a seguir permite que arquivos estáticos e o arquivo padrão a ser servido, mas não permite a pesquisa no diretório:

```csharp
app.UseFileServer();
   ```

O código a seguir permite que arquivos estáticos, arquivos padrão e a pesquisa no diretório:

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

Consulte [considerações](#considerations) sobre os riscos de segurança ao habilitar a pesquisa. Assim como acontece com `UseStaticFiles`, `UseDefaultFiles`, e `UseDirectoryBrowser`, se você deseja servir arquivos que existem fora do `web root`, instanciar e configurar um `FileServerOptions` objeto que você passa como um parâmetro para `UseFileServer`. Por exemplo, dada a seguinte hierarquia de diretório em seu aplicativo Web:

* wwwroot

  * CSS

  * imagens

  * ...

* MyStaticFiles

  * Test.PNG

  * Default

Usando o exemplo de hierarquia acima, talvez você queira habilitar arquivos estáticos, arquivos padrão e procurando o `MyStaticFiles` directory. No trecho de código a seguir, que pode ser feito com uma única chamada para `FileServerOptions`.

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

Se `enableDirectoryBrowsing` é definido como `true` é necessário chamar `AddDirectoryBrowser` método de extensão de `Startup.ConfigureServices`:

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

Usando a hierarquia de arquivos e o código acima:

| URI            |                             Resposta  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      MyStaticFiles/test.png |
| `http://<app>/StaticFiles`              |     MyStaticFiles/default.html |

Se nenhum padrão chamado arquivos estiverem no *MyStaticFiles* diretório, http://\<aplicativo > / StaticFiles retorna o diretório listando com links clicáveis:

![Lista de arquivos estáticos](static-files/_static/db2.PNG)

> [!NOTE]
> `UseDefaultFiles`e `UseDirectoryBrowser` levará a url http://\<aplicativo > / StaticFiles sem a barra à direita e a causa de um lado do cliente redirecionamento para http://\<aplicativo > /StaticFiles/ (Adicionar a barra à direita). Sem as URLs relativas de barra à direita dentro dos documentos seria incorretas.

## <a name="fileextensioncontenttypeprovider"></a>FileExtensionContentTypeProvider

O `FileExtensionContentTypeProvider` classe contém uma coleção que mapeia as extensões de arquivo para tipos de conteúdo MIME. No exemplo a seguir, várias extensões de arquivo são registradas em tipos MIME conhecidos, "RTF" é substituído e ". mp4" é removido.

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

Consulte [tipos de conteúdo MIME](http://www.iana.org/assignments/media-types/media-types.xhtml).

## <a name="non-standard-content-types"></a>Tipos de conteúdo não padrão

O middleware de arquivo estático ASP.NET compreende quase 400 tipos de conteúdo de arquivo conhecidos. Se o usuário solicita um arquivo de um tipo de arquivo desconhecido, o middleware de arquivo estático retorna uma resposta HTTP 404 (não encontrado). Se a pesquisa no diretório estiver habilitada, será exibido um link para o arquivo, mas o URI retornará um erro HTTP 404.

O código a seguir habilita atendendo tipos desconhecidos e processará o arquivo desconhecido como uma imagem.

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

Com o código acima, uma solicitação para um arquivo com um tipo de conteúdo desconhecido será retornada como uma imagem.

>[!WARNING]
> Habilitando `ServeUnknownFileTypes` é um risco de segurança e usá-lo não é recomendado.  `FileExtensionContentTypeProvider`(explicado acima) fornece uma alternativa mais segura para servir arquivos com extensões não padrão.

### <a name="considerations"></a>Considerações

>[!WARNING]
> `UseDirectoryBrowser`e `UseStaticFiles` podem vazar segredos. É recomendável que você **não** directory Habilitar navegação em produção. Tenha cuidado sobre os diretórios que você habilite com `UseStaticFiles` ou `UseDirectoryBrowser` como o diretório inteiro e todos os subdiretórios estarão acessíveis. É recomendável manter o conteúdo público em seu próprio diretório, como  *\<conteúdo raiz > / wwwroot*, longe de modos de exibição do aplicativo, arquivos de configuração, etc.

* As URLs para conteúdo exposto com `UseDirectoryBrowser` e `UseStaticFiles` estão sujeitos a diferenciação de maiusculas e minúsculas e restrições de caracteres de seu sistema de arquivos subjacente. Por exemplo, o Windows diferencia maiusculas de minúsculas, mas o Mac e Linux não são.

* Aplicativos do ASP.NET Core hospedados no IIS usam o módulo do ASP.NET Core para encaminhar todas as solicitações para o aplicativo, incluindo solicitações de arquivos estáticos. O manipulador de arquivo estático do IIS não é usado porque ele não obtém a oportunidade de lidar com solicitações antes que eles são tratados pelo módulo ASP.NET Core.

* Para remover o manipulador de arquivo estático do IIS (no nível do servidor ou site):

     * Navegue até o **módulos** recurso

     * Selecione **StaticFileModule** na lista

     * Toque em **remover** no **ações** barra lateral

>[!WARNING]
> Se o manipulador de arquivo estático no IIS é habilitado **e** o ASP.NET Core módulo (ANCM) não está configurado corretamente (por exemplo se *Web. config* não foi implantado), arquivos estáticos serão servidos.

* Arquivos de código (incluindo c# e Razor) devem ser colocados fora do projeto de aplicativo `web root` (*wwwroot* por padrão). Isso cria uma separação clara entre o conteúdo do lado cliente do aplicativo e código fonte no lado servidor, que impede que o código no lado servidor vazem.

## <a name="additional-resources"></a>Recursos adicionais

* [Middleware](middleware.md)

* [Introdução ao ASP.NET Core](../index.md)
