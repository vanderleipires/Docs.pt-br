---
title: "Injeção de dependência no núcleo do ASP.NET"
author: ardalis
description: "Saiba como o ASP.NET Core implementa injeção de dependência e como usá-lo."
keywords: "ASP.NET Core, injeção de dependência, di"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: fccd69be-7ad1-47fb-b203-b3633b6b9a9b
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/dependency-injection
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8d12960708f9d9bf2bc7c5997f82096d93087d13
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2017
---
# <a name="introduction-to-dependency-injection-in-aspnet-core"></a>Introdução a injeção de dependência no núcleo do ASP.NET

<a name="fundamentals-dependency-injection"></a>

Por [Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)

ASP.NET Core foi projetado desde o início para dar suporte e aproveitar a injeção de dependência. Aplicativos do ASP.NET Core podem aproveitar os serviços de estrutura interna, mantendo-as injetada métodos na classe de inicialização e serviços de aplicativo podem ser configurados para a injeção também. O contêiner de serviços padrão fornecido pelo ASP.NET Core fornece um recurso mínimo definido e não se destina a substituir outros contêineres.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>O que é a injeção de dependência?

Injeção de dependência (DI) é uma técnica para a obtenção de um acoplamento fraco entre objetos e seus colaboradores ou dependências. Em vez de diretamente a instanciação de parceiros, ou usando referências estáticas, os objetos que precisa de uma classe para realizar suas ações são fornecidos para a classe de alguma forma. Geralmente, classes irá declarar suas dependências por meio de seu construtor, possibilitando que siga a [princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/). Essa abordagem é conhecida como "injeção de construtor".

Quando classes são criadas com DI em mente, eles são mais flexíveis porque eles não têm dependências diretas, codificada em seus colaboradores. Isso segue o [princípio de inversão de dependência](http://deviq.com/dependency-inversion-principle/), indicando que *"módulos de nível alto não devem depender módulos de Nível baixos; ambos devem depender das abstrações."* Em vez de fazer referência a implementações específicas, classes de solicitação abstrações (geralmente `interfaces`) que é fornecido a eles quando a classe é construída. Extração de dependências em interfaces e fornecer implementações dessas interfaces como parâmetros também é um exemplo de como o [padrão de design de estratégia](http://deviq.com/strategy-design-pattern/).

Quando um sistema é projetado para usar a DI, com muitas classes solicitando suas dependências por meio de seu construtor (ou propriedades), é útil ter uma classe dedicada à criação dessas classes com suas dependências associadas. Essas classes são chamadas de *contêineres*, ou, mais especificamente, [inversão de controle (IoC)](http://deviq.com/inversion-of-control/) contêineres ou contêineres de injeção de dependência (DI). Um contêiner é essencialmente uma fábrica que é responsável por fornecer instâncias dos tipos que são solicitados a partir dele. Se um determinado tipo declarou que ele tem dependências, e o contêiner tiver sido configurado para fornecer os tipos de dependência, ele criará as dependências como parte da criação da instância solicitada. Dessa forma, os gráficos de dependência complexos podem ser fornecidos para classes sem a necessidade de qualquer construção de objeto inserido no código. Além de criar objetos com suas dependências, contêineres normalmente gerenciar tempos de vida de objeto dentro do aplicativo.

ASP.NET Core inclui um simple contêiner interno (representado pelo `IServiceProvider` interface) que dá suporte a injeção de construtor por padrão, e ASP.NET disponibiliza por meio de DI determinados serviços. AS PÁGINAS ASP. Contêiner do NET refere-se aos tipos que ele gerencia como *serviços*. No restante deste artigo, *serviços* fará referência a tipos que são gerenciados pelo contêiner de IoC do ASP.NET Core. Configurar serviços do contêiner interno no `ConfigureServices` método em seu aplicativo `Startup` classe.

> [!NOTE]
> Martin Fowler escreveu um artigo abrangente sobre [inversão de contêineres de controle e o padrão de injeção de dependência](https://www.martinfowler.com/articles/injection.html). Microsoft Patterns e práticas recomendadas também tem uma descrição excelente de [injeção de dependência](https://msdn.microsoft.com/library/hh323705.aspx).

> [!NOTE]
> Este artigo aborda a injeção de dependência conforme ela se aplica a todos os aplicativos ASP.NET. Injeção de dependência em controladores MVC é abordada em [injeção de dependência e controladores](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Comportamento de injeção de construtor

Injeção de construtor requer que o construtor em questão esteja *público*. Caso contrário, seu aplicativo lançará um `InvalidOperationException`:

> Um construtor adequado para o tipo 'YourType' não pôde ser localizado. Verifique se o tipo é concreto e serviços são registrados para todos os parâmetros de um construtor público.


Injeção de construtor exige que apenas um construtor aplicável existe. Sobrecargas de construtor têm suporte, mas somente uma sobrecarga pode existir cujos argumentos podem ser executados por injeção de dependência. Se houver mais de um, seu aplicativo lançará um `InvalidOperationException`:

> Foram encontrados vários construtores aceitar todos os tipos de argumento fornecido no tipo 'YourType'. Deve haver apenas um construtor aplicável.

Construtores podem aceitar argumentos que não são fornecidos pela injeção de dependência, mas eles devem oferecer suporte a valores padrão. Por exemplo:

```csharp
// throws InvalidOperationException: Unable to resolve service for type 'System.String'...
public CharactersController(ICharacterRepository characterRepository, string title)
{
    _characterRepository = characterRepository;
    _title = title;
}

// runs without error
public CharactersController(ICharacterRepository characterRepository, string title = "Characters")
{
    _characterRepository = characterRepository;
    _title = title;
}
```

## <a name="using-framework-provided-services"></a>Usando os serviços fornecidos pelo Framework

O `ConfigureServices` método o `Startup` classe é responsável por definir os serviços que o aplicativo irá usar, incluindo recursos de plataforma como o Entity Framework Core e ASP.NET MVC de núcleo. Inicialmente, o `IServiceCollection` fornecido para `ConfigureServices` tem os seguintes serviços definidos (dependendo da [como o host foi configurado](xref:fundamentals/hosting)):

| Tipo de serviço | Tempo de vida |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Transitório |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Transitório |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartupfilter) | Transitório |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.iconfigureoptions-1) | Transitório |
| [Microsoft.AspNetCore.Hosting.Server.IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |

Abaixo está um exemplo de como adicionar serviços adicionais ao contêiner usando um número de métodos de extensão como `AddDbContext`, `AddIdentity`, e `AddMvc`.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Os recursos e o middleware fornecido pelo ASP.NET, como MVC, siga uma convenção de usar um único adicionar*ServiceName* método de extensão para registrar todos os serviços exigidos pelo recurso.

>[!TIP]
> Você pode solicitar determinados serviços fornecidos pelo framework em `Startup` métodos por meio de suas listas de parâmetro - consulte [inicialização do aplicativo](startup.md) para obter mais detalhes.

## <a name="registering-your-own-services"></a>Registrar seus próprios serviços

Você pode registrar seus próprios serviços de aplicativo da seguinte maneira. O primeiro tipo genérico representa o tipo (normalmente uma interface) que será solicitado do contêiner. O segundo tipo genérico representa o tipo concreto ser instanciado pelo contêiner e usado para atender a essas solicitações.

[!code-csharp[Main](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Cada `services.Add<ServiceName>` método de extensão adiciona (e possivelmente configura) serviços. Por exemplo, `services.AddMvc()` adiciona os MVC requer os serviços. É recomendável que você siga essa convenção, colocando os métodos de extensão no `Microsoft.Extensions.DependencyInjection` namespace para encapsular a grupos de registros de serviço.

O `AddTransient` método é usado para mapear tipos abstratos concreta serviços que são instanciados separadamente para cada objeto que exija isso. Isso é conhecido como o serviço *tempo de vida*, e opções de tempo de vida adicionais são descritas abaixo. É importante escolher um tempo de vida apropriado para cada um dos serviços que você registrar. Uma nova instância do serviço deve ser fornecida para cada classe de solicitá-lo? Uma instância deve ser usada durante uma solicitação da web especificado? Ou uma única instância deve ser usada para o tempo de vida do aplicativo?

O exemplo deste artigo, há um controlador simple que exibe os nomes de caractere, chamados `CharactersController`. Seu `Index` método exibe a lista atual de caracteres que foram armazenados no aplicativo e inicializa a coleção com uma série de caracteres, se não houver nenhum. Observe que, embora esse aplicativo usa o Entity Framework Core e o `ApplicationDbContext` classe para sua persistência, nada disso é aparente no controlador. Em vez disso, o mecanismo de acesso a dados específicos foram abstraído atrás de uma interface `ICharacterRepository`, que segue o [padrão repositório](http://deviq.com/repository-pattern/). Uma instância de `ICharacterRepository` é solicitado por meio do construtor e atribuído a um campo particular, que é usado para acessar caracteres conforme necessário.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

O `ICharacterRepository` define dois métodos que o controlador precisa trabalhar com `Character` instâncias.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Por sua vez, essa interface é implementada por um tipo concreto, `CharacterRepository`, que é usada em tempo de execução.

> [!NOTE]
> A maneira DI é usada com o `CharacterRepository` classe é um modelo geral que você pode seguir para todos os serviços do aplicativo, não apenas em "repositórios" ou classes de acesso de dados.

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Observe que `CharacterRepository` solicitações um `ApplicationDbContext` em seu construtor. Não é incomum para a injeção de dependência a ser usado de maneira encadeada como este, com cada vez solicitando suas próprias dependências de dependências solicitados. O contêiner é responsável por resolver todas as dependências no gráfico e retornando o serviço totalmente resolvido.

> [!NOTE]
> Criando o objeto solicitado e todos os objetos que ele requer e todos os objetos que os exigem, é, às vezes, conhecido como um *gráfico do objeto*. Da mesma forma, o conjunto de dependências que devem ser resolvidos é normalmente referido como um *árvore de dependência* ou *gráfico de dependência*.

Nesse caso, ambos `ICharacterRepository` e, em seguida `ApplicationDbContext` deve ser registrado com o contêiner de serviços em `ConfigureServices` em `Startup`. `ApplicationDbContext`é configurado com a chamada para o método de extensão `AddDbContext<T>`. O código a seguir mostra o registro da `CharacterRepository` tipo.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Contextos do Entity Framework devem ser adicionados para o contêiner de serviços usando o `Scoped` tempo de vida. Isso é resolvido automaticamente se você usar os métodos auxiliares, como mostrado acima. Os repositórios que fará o uso do Entity Framework devem usar a mesma duração.

>[!WARNING]
> O perigo principal para cuidado está resolvendo um `Scoped` serviço um singleton. É provável, nesse caso o serviço terá estado incorreto durante o processamento de solicitações subsequentes.

Serviços que têm dependências devem registrá-los no contêiner. Se o construtor do serviço requer um primitivo, como um `string`, isso pode ser inserido usando [configuração](xref:fundamentals/configuration/index) e [padrão de opções](xref:fundamentals/configuration/options).

## <a name="service-lifetimes-and-registration-options"></a>Tempos de vida do serviço e opções de registro

Serviços ASP.NET podem ser configurados com os tempos de vida do seguintes:

**Transitório**

Serviços de tempo de vida transitórias são criados cada vez que forem solicitados. Esse tempo de vida funciona melhor para serviços leves e sem monitoração de estado.

**No escopo**

Serviços de tempo de vida no escopo são criados uma vez por solicitação.

**Singleton**

Serviços de tempo de vida de singleton são criados na primeira vez que forem solicitados (ou quando `ConfigureServices` é executado se você especificar uma instância) e, em seguida, todas as solicitações subsequentes usarão a mesma instância. Se seu aplicativo exigir comportamento singleton, permitindo que o contêiner de serviços gerenciar o tempo de vida do serviço é recomendado em vez de implementar o padrão de design de singleton e gerenciar o tempo de vida do objeto na classe por conta própria.

Os serviços podem ser registrados com o contêiner de várias maneiras. Já vimos como registrar uma implementação de serviço com um determinado tipo, especificando o tipo concreto para usar. Além disso, uma fábrica pode ser especificada, que será usado para criar a instância sob demanda. A terceira abordagem é especificar diretamente a instância do tipo a ser usado, nesse caso o contêiner não tentará criar uma instância (nem será dispose da instância).

Para demonstrar a diferença entre essas opções de tempo de vida e o registro, considere a possibilidade de uma interface simples que representa uma ou mais tarefas como um *operação* com um identificador exclusivo, `OperationId`. Dependendo de como é configurar o tempo de vida para este serviço, o contêiner fornecem as instâncias igual ou diferentes do serviço para a classe solicitante. Para tornar claro qual tempo de vida está sendo solicitado, vamos criar um tipo por opção de tempo de vida:

[!code-csharp[Main](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

É implementam essas interfaces usando uma única classe, `Operation`, que aceita um `Guid` em seu construtor, ou usa um novo `Guid` se nenhum for fornecido.

Em seguida, na `ConfigureServices`, cada tipo é adicionado ao contêiner de acordo com seu tempo de vida nomeado:

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Observe que o `IOperationSingletonInstance` serviço está usando uma instância específica com um ID conhecida de `Guid.Empty` para que seja clara quando esse tipo está em uso (seu Guid será todos os zeros). Também registramos um `OperationService` que depende de cada uma das outras `Operation` tipos, para que ela será desmarque dentro de uma solicitação se esse serviço está obtendo a mesma instância que o controlador ou um novo, para cada tipo de operação. Tudo o que esse serviço é expor suas dependências como propriedades, para que possam ser exibidos no modo de exibição.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Para demonstrar os tempos de vida de objeto dentro e entre as solicitações individuais separadas para o aplicativo, o exemplo inclui um `OperationsController` que solicitações de cada tipo de `IOperation` tipo, bem como um `OperationService`. O `Index` ação, em seguida, exibe todos do controlador e do serviço `OperationId` valores.

[!code-csharp[Main](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Agora, duas solicitações separadas são feitas para esta ação de controlador:

![A exibição de operações do aplicativo da web de exemplo de injeção de dependência em execução no Microsoft Edge mostrando valores de ID da operação (GUID) para transitório, escopo, Singleton e instância de controlador e operações de serviço de operação na primeira solicitação.](dependency-injection/_static/lifetimes_request1.png)

![As operações de exibição mostrando os valores de ID de operação para uma segunda solicitação.](dependency-injection/_static/lifetimes_request2.png)

Observe qual o `OperationId` variam de valores dentro de uma solicitação e entre as solicitações.

* *Transitório* objetos sempre são diferentes; uma nova instância é fornecida para cada serviço e cada controlador.

* *Escopo* objetos forem os mesmos em uma solicitação, mas diferente em diferentes solicitações

* *Singleton* objetos são os mesmos para todos os objetos e todas as solicitações (independentemente de se uma instância é fornecida em `ConfigureServices`)

## <a name="request-services"></a>Serviços de solicitação

Os serviços disponíveis dentro de uma ASP.NET de solicitação de `HttpContext` são expostas por meio de `RequestServices` coleção.

![HttpContext solicitação serviços Intellisense contextual caixa de diálogo informando que os serviços de solicitação obtém ou define o IServiceProvider que fornece acesso ao contêiner de serviço da solicitação.](dependency-injection/_static/request-services.png)

Serviços de solicitação representam os serviços que você configure e solicitação como parte do seu aplicativo. Quando os objetos especificam dependências, eles são certificados pelos tipos encontrados em `RequestServices`, não `ApplicationServices`.

Em geral, você não deve usar essas propriedades diretamente, em vez disso, preferindo solicitar os tipos de suas classes que você precisa por meio do construtor da classe e permitindo que a estrutura de injetar essas dependências. Isso resulta em classes que são mais fáceis de teste (consulte [teste](../testing/index.md)) e são mais flexíveis e.

> [!NOTE]
> Preferir solicitando dependências como parâmetros de construtor para acessar o `RequestServices` coleção.

## <a name="designing-your-services-for-dependency-injection"></a>Criar serviços para injeção de dependência

Você deve projetar seus serviços para usar injeção de dependência para seus colaboradores. Isso significa evitar o uso de chamadas de método estático com monitoração de estado (que pode resultar em um cheiro código conhecido como [adesivos](http://deviq.com/static-cling/)) e a instanciação direta classes dependentes em seus serviços. Ele pode ajudar a lembrar a frase [novo é liga](https://ardalis.com/new-is-glue), ao escolher se deseja criar uma instância de um tipo ou solicitá-la por meio de injeção de dependência. Seguindo o [sólido princípios de Design orientado a objeto](http://deviq.com/solid/), suas classes naturalmente tendem a ser pequenos, bem fatorada e facilmente testados.

Se você acha que suas classes tendem a ter muitas dependências que está sendo inseridas de maneira? Isso geralmente é um sinal de que sua classe está tentando fazer muito e provavelmente está violando SRP - o [princípio da responsabilidade única](http://deviq.com/single-responsibility-principle/). Consulte se refatorar a classe ao mover algumas de suas responsabilidades em uma nova classe. Lembre-se de que seu `Controller` classes devem se concentrar no preocupações de interface do usuário para os detalhes de implementação de acesso de dados e regras de negócios devem ser mantidos em classes apropriadas para esses [separar questões](http://deviq.com/separation-of-concerns/).

Com relação a acesso a dados especificamente, você pode injetar o `DbContext` em seus controladores (supondo que você adicionou o EF no contêiner de serviços no `ConfigureServices`). Alguns desenvolvedores preferem usar uma interface de repositório para o banco de dados em vez de injetar o `DbContext` diretamente. Usando uma interface para encapsular os dados de lógica de acesso em um único local pode minimizar quantas casas você terá que alterar quando seu banco de dados é alterado.

### <a name="disposing-of-services"></a>Descarte de serviços

O contêiner chamará `Dispose` para `IDisposable` tipos que ele cria. No entanto, se você adicionar uma instância para o contêiner, ele não será descartado.

Exemplo:

```csharp
// Services implement IDisposable:
public class Service1 : IDisposable {}
public class Service2 : IDisposable {}
public class Service3 : IDisposable {}

public interface ISomeService {}
public class SomeServiceImplementation : ISomeService, IDisposable {}


public void ConfigureServices(IServiceCollection services)
{
    // container will create the instance(s) of these types and will dispose them
    services.AddScoped<Service1>();
    services.AddSingleton<Service2>();
    services.AddSingleton<ISomeService>(sp => new SomeServiceImplementation());

    // container did not create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> Na versão 1.0, o contêiner chamado dispose em *todos os* `IDisposable` objetos, incluindo aqueles não criou.

## <a name="replacing-the-default-services-container"></a>Substituindo o contêiner de serviços padrão

O contêiner de serviços internos destina-se para atender às necessidades básicas do framework e a maioria dos aplicativos de consumidor criados nele. No entanto, os desenvolvedores podem substituir o contêiner interno por seu contêiner preferencial. O `ConfigureServices` método normalmente retorna `void`, mas se sua assinatura for alterada para retornar `IServiceProvider`, um contêiner diferente pode ser configurado e retornado. Há vários contêineres IOC disponíveis para .NET. Neste exemplo, o [Autofac](https://autofac.org/) pacote é usado.

Primeiro, instale o pacote de contêiner apropriado (s):

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

Em seguida, configure o contêiner no `ConfigureServices` e retornar um `IServiceProvider`:

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    // Add other framework services

    // Add Autofac
    var containerBuilder = new ContainerBuilder();
    containerBuilder.RegisterModule<DefaultModule>();
    containerBuilder.Populate(services);
    var container = containerBuilder.Build();
    return new AutofacServiceProvider(container);
}
```

> [!NOTE]
> Ao usar um contêiner de injeção de dependência de terceiros, você deve alterar `ConfigureServices` para que ele retorna `IServiceProvider` em vez de `void`.

Finalmente, configure Autofac normalmente em `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

Em tempo de execução, Autofac será usado para resolver tipos e injetar dependências. [Saiba mais sobre como usar Autofac e ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Acesso thread-safe

Serviços de singleton precisam ser thread-safe. Se um serviço singleton tem uma dependência em um serviço transitória, o serviço transitória também precisará ser thread-safe, dependendo de como ele é usado por um singleton.

## <a name="recommendations"></a>Recomendações

Ao trabalhar com a injeção de dependência, lembre-se as seguintes recomendações:

* DI está para objetos que têm dependências complexas. Controladores, serviços, adaptadores e repositórios são exemplos de objetos que podem ser adicionados à DI.

* Evite armazenar dados e configuração diretamente na injeção de dependência. Por exemplo, o carrinho de compras do usuário normalmente não deve ser adicionado para o contêiner de serviços. Configuração deve usar o [padrão de opções](xref:fundamentals/configuration/options). Da mesma forma, evite objetos de "proprietário de dados" que existem somente para permitir o acesso a algum outro objeto. É melhor solicitar o item real necessário por meio de injeção de dependência, se possível.

* Evite estático acesso aos serviços.

* Evite o local do serviço no código do aplicativo.

* Evitar acessem estático `HttpContext`.

> [!NOTE]
> Como todos os conjuntos de recomendações, você poderá encontrar situações em que é necessário ignorar um. Encontramos exceções ser raros – casos principalmente muito especiais dentro a própria estrutura.

Lembre-se de que a injeção de dependência é um *alternativo* para padrões de acesso a objeto static/globais. Você não poderá obter os benefícios de DI se você a combinar com acesso ao objeto estático.

## <a name="additional-resources"></a>Recursos adicionais

* [Inicialização de aplicativos](startup.md)

* [Teste](../testing/index.md)

* [Escrevendo código limpo no núcleo do ASP.NET com a injeção de dependência (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)

* [Design de aplicativo gerenciado por contêiner, Prelude: Onde é o contêiner pertence?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)

* [Princípio de dependências explícitas](http://deviq.com/explicit-dependencies-principle/)

* [Inversão de contêineres de controle e o padrão de injeção de dependência](https://www.martinfowler.com/articles/injection.html) (Fowler)
