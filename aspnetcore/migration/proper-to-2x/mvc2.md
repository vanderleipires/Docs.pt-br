---
title: "Migrando do ASP.NET para ASP.NET 2.0 de núcleo"
author: isaac2004
description: "Este documento de referência fornece diretrizes para migrar aplicativos de API Web ou ASP.NET MVC existentes para o ASP.NET Core 2.0."
keywords: ASP.NET Core, MVC, migrando
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc2
ms.openlocfilehash: 8005d23ad00774e488eecc9771f36a244a051126
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2017
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a>Migrando do ASP.NET para ASP.NET 2.0 de núcleo

Por [Isaac Levin](https://isaaclevin.com)

Este artigo serve como um guia de referência para migração de aplicativos ASP.NET para o ASP.NET Core 2.0.

## <a name="prerequisites"></a>Pré-requisitos

* [SDK do .NET Core 2.0.0](https://www.microsoft.com/net/core) ou posterior.

## <a name="target-frameworks"></a>Frameworks de destino
Projetos do ASP.NET Core 2.0 oferecem aos desenvolvedores a flexibilidade de direcionamento para o .NET Core, .NET Framework ou ambos. Consulte [Escolhendo entre o .NET Core e .NET Framework para aplicativos de servidor](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) para determinar qual estrutura de destino é mais apropriada.

Ao usar o .NET Framework como destino, projetos precisam fazer referência a pacotes NuGet individuais.

Usar o .NET Core como destino permite que você elimine várias referências de pacote explícitas, graças ao [metapacote](xref:fundamentals/metapackage) do ASP.NET Core 2.0. Instale o metapacote `Microsoft.AspNetCore.All` em seu projeto:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

Quando o metapacote é usado, nenhum pacote referenciado no metapacote é implantado com o aplicativo. O repositório de tempo de execução do .NET Core inclui esses ativos e eles são pré-compilados para melhorar o desempenho. Consulte [Metapacote do Microsoft.AspNetCore.All para ASP.NET Core 2.x](xref:fundamentals/metapackage) para obter mais detalhes.

## <a name="project-structure-differences"></a>Diferenças de estrutura do projeto
O formato de arquivo *.csproj* foi simplificado no ASP.NET Core. Algumas alterações importantes incluem:
- A inclusão explícita de arquivos não é necessária para que eles possam ser considerados como parte do projeto. Isso reduz o risco de conflitos de mesclagem XML ao trabalhar em grandes equipes.
- Não há referências baseadas em GUID a outros projetos, o que melhora a legibilidade do arquivo.
- O arquivo pode ser editado sem descarregá-lo no Visual Studio:

    ![Opção de menu de contexto Editar CSPROJ no Visual Studio 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Substituição do arquivo Global.asax
O ASP.NET Core introduziu um novo mecanismo para inicializar um aplicativo. O ponto de entrada para aplicativos ASP.NET é o arquivo *Global.asax*. Tarefas como configuração de roteamento e registros de filtro e de área são tratadas no arquivo *Global.asax*.

[!code-csharp[Main](samples/globalasax-sample.cs)]

Essa abordagem associa o aplicativo e o servidor no qual ele é implantado de uma forma que interfere na implementação. Em um esforço para desassociar, a [OWIN](http://owin.org/) foi introduzida para fornecer uma maneira mais limpa de usar várias estruturas juntas. A OWIN fornece um pipeline para adicionar somente os módulos necessários. O ambiente de hospedagem leva uma função [Startup](xref:fundamentals/startup) para configurar serviços e pipeline de solicitação do aplicativo. `Startup` registra um conjunto de middleware com o aplicativo. Para cada solicitação, o aplicativo chama cada um dos componentes de middleware com o ponteiro de cabeçalho de uma lista vinculada para um conjunto existente de manipuladores. Cada componente de middleware pode adicionar um ou mais manipuladores para a pipeline de tratamento de solicitação. Isso é feito retornando uma referência para o manipulador que é o novo cabeçalho da lista. Cada manipulador é responsável por se lembrar do próximo manipulador na lista e por invocá-lo. Com o ASP.NET Core, o ponto de entrada para um aplicativo é `Startup` e você não tem mais uma dependência de *Global.asax*. Ao usar a OWIN com o .NET Framework, use algo parecido com o seguinte como um pipeline:

[!code-csharp[Main](samples/webapi-owin.cs)]

Isso configura as rotas padrão e usa XmlSerialization em Json por padrão. Adicione outro Middleware para este pipeline conforme necessário (carregamento de serviços, definições de configuração, arquivos estáticos, etc.).

O ASP.NET Core usa uma abordagem semelhante, mas não depende de OWIN para manipular a entrada. Em vez disso, isso é feito por meio do método `Main` de *Program.cs* (semelhante a aplicativos de console) e `Startup` é carregado por lá.

[!code-csharp[Main](samples/program.cs)]

`Startup` deve incluir um método `Configure`. Em `Configure`, adicione o middleware necessário ao pipeline. No exemplo a seguir (com base no modelo de site da Web padrão), vários métodos de extensão são usados para configurar o pipeline com suporte para:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Páginas de erro
* Arquivos estáticos
* ASP.NET Core MVC
* Identidade

[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

O host e o aplicativo foram separados, o que fornece a flexibilidade de mover para uma plataforma diferente no futuro.

**Observação:** para obter uma referência mais aprofundada sobre o Startup do ASP.NET Core e middleware, consulte [Startup no ASP.NET Core](xref:fundamentals/startup)

## <a name="storing-configurations"></a>Armazenando configurações
O ASP.NET dá suporte ao armazenamento de configurações. Essas configurações são usadas, por exemplo, para dar suporte ao ambiente no qual os aplicativos foram implantados. Uma prática comum era armazenar todos os pares chave-valor personalizados na seção `<appSettings>` do arquivo *Web.config*:

[!code-xml[Main](samples/webconfig-sample.xml)]

Aplicativos leem essas configurações usando a coleção `ConfigurationManager.AppSettings` no namespace `System.Configuration`:

[!code-csharp[Main](samples/read-webconfig.cs)]

O ASP.NET Core pode armazenar dados de configuração para o aplicativo em qualquer arquivo e carregá-los como parte da inicialização de middleware. O arquivo padrão usado em modelos de projeto é *appsettings.json*:

[!code-json[Main](samples/appsettings-sample.json)]

Carregar esse arquivo em uma instância de `IConfiguration` dentro de seu aplicativo é feito em *Startup.cs*:

[!code-csharp[Main](samples/startup-builder.cs)]

O aplicativo lê de `Configuration` para obter as configurações:

[!code-csharp[Main](samples/read-appsettings.cs)]

Existem extensões para essa abordagem para tornar o processo mais robusto, tais como o uso de DI ([injeção de dependência](xref:fundamentals/dependency-injection)) para carregar um serviço com esses valores. A abordagem de DI fornece um conjunto fortemente tipado de objetos de configuração.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**Observação:** para uma referência mais detalhada sobre configuração do ASP.NET Core, consulte [Configuração no ASP.NET Core](xref:fundamentals/configuration/index).

## <a name="native-dependency-injection"></a>Injeção de dependência nativa
Uma meta importante ao criar aplicativos escalonáveis e grandes é o acoplamento flexível de componentes e serviços. A [injeção de dependência](xref:fundamentals/dependency-injection) é uma técnica popular para conseguir isso e é também um componente nativo do ASP.NET Core.

Em aplicativos ASP.NET, os desenvolvedores contam com uma biblioteca de terceiros para implementar injeção de dependência. Um biblioteca desse tipo é a [Unity](https://github.com/unitycontainer/unity), fornecida pelas Diretrizes da Microsoft. 

Um exemplo de configuração da injeção de dependência com Unity é a implementação de `IDependencyResolver`, que encapsula uma `UnityContainer`:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Crie uma instância de sua `UnityContainer`, registre seu serviço e defina o resolvedor de dependência de `HttpConfiguration` para a nova instância de `UnityResolver` para o contêiner:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Injete `IProductRepository` quando necessário:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Já que a injeção de dependência é parte do ASP.NET Core, você pode adicionar o serviço no método `ConfigureServices` de *Startup.cs*:

[!code-csharp[Main](samples/configure-services.cs)]

O repositório pode ser injetado em qualquer lugar, como ocorria com a Unity.

**Observação:** para uma referência detalhada sobre a injeção de dependência no ASP.NET Core, consulte [Injeção de dependência no ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)

## <a name="serving-static-files"></a>Servir arquivos estáticos
Uma parte importante do desenvolvimento da Web é a capacidade de servir ativos estáticos, do lado do cliente. Os exemplos mais comuns de arquivos estáticos são HTML, CSS, Javascript e imagens. Esses arquivos precisam ser salvos no local de publicação do aplicativo (ou CDN) e referenciados para que eles possam ser carregados por uma solicitação. Esse processo foi alterado no ASP.NET Core.

No ASP.NET, arquivos estáticos são armazenados em vários diretórios e referenciados nas exibições.

No ASP.NET Core, arquivos estáticos são armazenados na "raiz da Web" (*&lt;raiz do conteúdo&gt;/wwwroot*), a menos que configurado de outra forma. Os arquivos são carregados no pipeline de solicitação invocando o método de extensão `UseStaticFiles` de `Startup.Configure`:

[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

**Observação**: se você usar o .NET Framework como destino, instale o pacote NuGet `Microsoft.AspNetCore.StaticFiles`.

Por exemplo, um ativo de imagem na pasta *wwwroot/imagens* está acessível para o navegador em um local como `http://<app>/images/<imageFileName>`.

**Observação:** para obter uma referência mais aprofundada sobre como servir arquivos estáticos no ASP.NET Core, consulte [Introdução ao trabalho com arquivos estáticos no ASP.NET Core](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Recursos adicionais
* [Fazendo a portabilidade de Bibliotecas para o .NET Core](https://docs.microsoft.com/dotnet/core/porting/libraries)
