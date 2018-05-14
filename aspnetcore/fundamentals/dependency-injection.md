---
title: Injeção de dependência no ASP.NET Core
author: ardalis
description: Saiba como o ASP.NET Core implementa a injeção de dependência e como usá-la.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/dependency-injection
ms.openlocfilehash: 8a105f835dddfcd0e9f32059e644f60dc1fdbbe1
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/03/2018
---
# <a name="dependency-injection-in-aspnet-core"></a>Injeção de dependência no ASP.NET Core

<a name="fundamentals-dependency-injection"></a>

Por [Steve Smith](https://ardalis.com/) e [Scott Addie](https://scottaddie.com)

O ASP.NET Core foi projetado desde o início para dar suporte e utilizar a injeção de dependência. Os aplicativos ASP.NET Core podem utilizar os serviços de estrutura interna mantendo-os injetados em métodos na classe Startup e os Serviços de Aplicativos também podem ser configurados para injeção. O contêiner de serviços padrão fornecido pelo ASP.NET Core fornece um conjunto de recursos mínimos e não se destina a substituir outros contêineres.

[Exibir ou baixar código de exemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/dependency-injection/sample) ([como baixar](xref:tutorials/index#how-to-download-a-sample))

## <a name="what-is-dependency-injection"></a>O que é injeção de dependência?

A DI (injeção de dependência) é uma técnica para obter um acoplamento flexível entre objetos e seus colaboradores, ou dependências. Em vez de criar uma instância de colaboradores diretamente, ou usar referências estáticas, os objetos de que uma classe precisa para realizar suas ações são fornecidos para a classe de alguma forma. Geralmente, as classes declararão suas dependências por meio de seu construtor, possibilitando que elas sigam o [Princípio de Dependências Explícitas](http://deviq.com/explicit-dependencies-principle/). Essa abordagem é conhecida como "injeção de construtor".

Quando as classes são criadas com a DI em mente, elas têm um acoplamento mais flexível porque não têm dependências diretas e embutidas em código em seus colaboradores. Isso segue o [Princípio de Inversão de Dependência](http://deviq.com/dependency-inversion-principle/), que indica que *"módulos de nível alto não devem depender de módulos de nível baixo; ambos devem depender de abstrações".* Em vez de referenciar implementações específicas, as classes solicitam abstrações (geralmente, `interfaces`) que são fornecidas a elas quando a classe é construída. A extração de dependências em interfaces e fornecimento de implementações dessas interfaces como parâmetros também é um exemplo do [Padrão de design de estratégia](http://deviq.com/strategy-design-pattern/).

Quando um sistema é projetado para usar a DI, com muitas classes solicitando suas dependências por meio de seu construtor (ou propriedades), é útil ter uma classe dedicada à criação dessas classes com suas dependências associadas. Essas classes são chamadas de *contêineres* ou mais especificamente, contêineres [IoC (Inversão de Controle)](http://deviq.com/inversion-of-control/) ou contêineres DI (Injeção de Dependência). Basicamente, um contêiner é um alocador responsável por fornecer instâncias dos tipos solicitados dele. Se determinado tipo declarou que tem dependências e o contêiner foi configurado para fornecer os tipos de dependência, ele criará as dependências como parte da criação da instância solicitada. Dessa forma, grafos de dependência complexos podem ser fornecidos para classes sem a necessidade de uma construção de objeto embutida em código. Além de criar objetos com suas dependências, os contêineres normalmente gerenciam tempos de vida de objeto no aplicativo.

O ASP.NET Core inclui um contêiner interno simples (representado pela interface `IServiceProvider`) que dá suporte à injeção de construtor por padrão e o ASP.NET disponibiliza alguns serviços por meio da DI. O contêiner do ASP.NET refere-se aos tipos que ele gerencia como *serviços*. No restante deste artigo, *serviços* se referirão aos tipos que são gerenciados pelo contêiner IoC do ASP.NET Core. Configure os serviços do contêiner interno no método `ConfigureServices` na classe `Startup` do aplicativo.

> [!NOTE]
> Martin Fowler escreveu um artigo abrangente sobre [Contêineres de Inversão de Controle e o padrão de injeção de dependência](https://www.martinfowler.com/articles/injection.html). O grupo Padrões e Práticas Microsoft também tem uma ótima descrição sobre a [Injeção de Dependência](https://msdn.microsoft.com/library/hh323705.aspx).

> [!NOTE]
> Este artigo aborda a Injeção de Dependência aplicável a todos os aplicativos ASP.NET. A Injeção de Dependência em controladores MVC é abordada em [Injeção de Dependência e controladores](../mvc/controllers/dependency-injection.md).

### <a name="constructor-injection-behavior"></a>Comportamento da injeção de construtor

A injeção de construtor exige que o construtor em questão seja *público*. Caso contrário, o aplicativo gerará uma `InvalidOperationException`:

> Um construtor adequado para o tipo 'YourType' não pôde ser localizado. Verifique se o tipo é concreto e se os serviços estão registrados para todos os parâmetros de um construtor público.

A injeção de construtor exige apenas a existência de um construtor aplicável. Há suporte para sobrecargas de construtor, mas somente uma sobrecarga pode existir, cujos argumentos podem ser todos atendidos pela injeção de dependência. Se houver mais de um, o aplicativo gerará uma `InvalidOperationException`:

> Vários construtores que aceitam todos os tipos de argumento fornecidos foram encontrados no tipo 'YourType'. Deve haver apenas um construtor aplicável.

Os construtores podem aceitar argumentos que não são fornecidos pela injeção de dependência, mas precisam dar suporte a valores padrão. Por exemplo:

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

## <a name="using-framework-provided-services"></a>Usando serviços fornecidos pela estrutura

O método `ConfigureServices` na classe `Startup` é responsável por definir os serviços que serão usados pelo aplicativo, incluindo recursos de plataforma como o Entity Framework Core e ASP.NET Core MVC. Inicialmente, a `IServiceCollection` fornecida ao `ConfigureServices` tem os seguintes serviços definidos (dependendo de [como o host foi configurado](xref:fundamentals/hosting)):

| Tipo de serviço | Tempo de vida |
| ----- | ------- |
| [Microsoft.AspNetCore.Hosting.IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) | Singleton |
| [Microsoft.Extensions.Logging.ILoggerFactory](/dotnet/api/microsoft.extensions.logging.iloggerfactory) | Singleton |
| [Microsoft.Extensions.Logging.ILogger&lt;T&gt;](/dotnet/api/microsoft.extensions.logging.ilogger) | Singleton |
| [Microsoft.AspNetCore.Hosting.Builder.IApplicationBuilderFactory](/dotnet/api/microsoft.aspnetcore.hosting.builder.iapplicationbuilderfactory) | Transitório |
| [Microsoft.AspNetCore.Http.IHttpContextFactory](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextfactory) | Transitório |
| [Microsoft.Extensions.Options.IOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.ioptions-1) | Singleton |
| [System.Diagnostics.DiagnosticSource](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticsource) | Singleton |
| [System.Diagnostics.DiagnosticListener](https://docs.microsoft.com/dotnet/core/api/system.diagnostics.diagnosticlistener) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartupFilter](/dotnet/api/microsoft.aspnetcore.hosting.istartupfilter) | Transitório |
| [Microsoft.Extensions.ObjectPool.ObjectPoolProvider](/dotnet/api/microsoft.extensions.objectpool.objectpoolprovider) | Singleton |
| [Microsoft.Extensions.Options.IConfigureOptions&lt;T&gt;](/dotnet/api/microsoft.extensions.options.iconfigureoptions-1) | Transitório |
| [Microsoft.AspNetCore.Hosting.Server.IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) | Singleton |
| [Microsoft.AspNetCore.Hosting.IStartup](/dotnet/api/microsoft.aspnetcore.hosting.istartup) | Singleton |
| [Microsoft.AspNetCore.Hosting.IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) | Singleton |

Veja abaixo um exemplo de como adicionar outros serviços ao contêiner usando vários métodos de extensão como `AddDbContext`, `AddIdentity` e `AddMvc`.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?highlight=5-6,8-10,12&range=39-56)]

Os recursos e o middleware fornecidos pelo ASP.NET, como o MVC, seguem uma convenção de uso de um único método de extensão Add*ServiceName* para registrar todos os serviços exigidos pelo recurso.

> [!TIP]
> Você pode solicitar alguns serviços fornecidos pela estrutura em métodos `Startup` por meio de suas listas de parâmetros – consulte [Inicialização do aplicativo](startup.md) para obter mais detalhes.

## <a name="registering-services"></a>Registrando serviços

Você pode registrar seus próprios Serviços de Aplicativos conforme mostrado a seguir. O primeiro tipo genérico representa o tipo (normalmente uma interface) que será solicitado do contêiner. O segundo tipo genérico representa o tipo concreto do qual será criada uma instância pelo contêiner e que será usado para atender a essas solicitações.

[!code-csharp[](../common/samples/WebApplication1/Startup.cs?range=53-54)]

> [!NOTE]
> Cada método de extensão `services.Add<ServiceName>` adiciona (e possivelmente configura) serviços. Por exemplo, o `services.AddMvc()` adiciona os serviços exigidos pelo MVC. É recomendável que você siga essa convenção, colocando os métodos de extensão no namespace `Microsoft.Extensions.DependencyInjection`, para encapsular grupos de registros de serviço.

O método `AddTransient` é usado para mapear tipos abstratos para serviços concretos dos quais são criadas instâncias separadamente para cada objeto que exigir isso. Isso é conhecido como o *tempo de vida* do serviço e opções de tempo de vida adicionais são descritas abaixo. É importante escolher um tempo de vida apropriado para cada um dos serviços registrados. Uma nova instância do serviço deve ser fornecida para cada classe que a solicitar? Uma instância deve ser usada durante uma solicitação da Web especificada? Ou uma única instância deve ser usada para o tempo de vida do aplicativo?

Na amostra deste artigo, há um controlador simples que exibe os nomes de caracteres, chamado `CharactersController`. Seu método `Index` exibe a lista atual de caracteres que foram armazenados no aplicativo e inicializa a coleção com uma série de caracteres, se não houver nenhum. Observe que, embora esse aplicativo use o Entity Framework Core e a classe `ApplicationDbContext` para sua persistência, nada disso é aparente no controlador. Em vez disso, o mecanismo de acesso a dados específico foi abstraído por trás de uma interface `ICharacterRepository`, que segue o [padrão do repositório](http://deviq.com/repository-pattern/). Uma instância de `ICharacterRepository` é solicitada por meio do construtor e atribuída a um campo particular, que é então usado para acessar caracteres, conforme necessário.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Controllers/CharactersController.cs?highlight=3,5,6,7,8,14,21-27&range=8-36)]

O `ICharacterRepository` define os dois métodos que o controlador precisa para trabalhar com instâncias `Character`.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/ICharacterRepository.cs?highlight=8,9)]

Por sua vez, essa interface é implementada por um tipo concreto, `CharacterRepository`, que é usado em tempo de execução.

> [!NOTE]
> A maneira como a DI é usada com a classe `CharacterRepository` é um modelo geral que você pode seguir para todos os Serviços de Aplicativos, não apenas em "repositórios" ou classes de acesso a dados.

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Models/CharacterRepository.cs?highlight=9,11,12,13,14)]

Observe que `CharacterRepository` solicita um `ApplicationDbContext` em seu construtor. Não é incomum que a injeção de dependência seja usada de maneira encadeada dessa forma, com cada dependência solicitada solicitando, por sua vez, suas próprias dependências. O contêiner é responsável por resolver todas as dependências no grafo e retornar o serviço totalmente resolvido.

> [!NOTE]
> A criação do objeto solicitado e todos os objetos exigidos por ele, além de todos os objetos que esses outros exigem, é, às vezes, chamado de um *grafo de objeto*. Da mesma forma, o conjunto de dependências que precisa ser resolvido normalmente é chamado de uma *árvore de dependência* ou um *grafo de dependência*.

Nesse caso, `ICharacterRepository` e, por sua vez, `ApplicationDbContext` precisam ser registrados no contêiner de serviços em `ConfigureServices` em `Startup`. `ApplicationDbContext` é configurado com a chamada ao método de extensão `AddDbContext<T>`. O código a seguir mostra o registro do tipo `CharacterRepository`.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?highlight=3-5,11&range=16-32)]

Os contextos do Entity Framework devem ser adicionados ao contêiner de serviços com o tempo de vida `Scoped`. Isso será resolvido automaticamente se você usar os métodos auxiliares, conforme mostrado acima. Os repositórios que farão uso do Entity Framework devem usar o mesmo tempo de vida.

> [!WARNING]
> O principal perigo com o qual é necessário ter cautela é resolver um serviço `Scoped` em um singleton. É provável que, nesse caso, o serviço terá um estado incorreto durante o processamento das solicitações seguintes.

Serviços que têm dependências devem registrá-las no contêiner. Se o construtor de um serviço exigir um primitivo, como uma `string`, isso poderá ser injetado com o uso da [configuração](xref:fundamentals/configuration/index) e do [padrão de opções](xref:fundamentals/configuration/options).

## <a name="service-lifetimes-and-registration-options"></a>Tempos de vida do serviço e opções de registro

Os serviços do ASP.NET podem ser configurados com os seguintes tempos de vida:

**Transitório**

Os serviços com tempo de vida transitório são criados sempre que são solicitados. Esse tempo de vida funciona melhor para serviços leves e sem estado.

**Com escopo**

Os serviços com tempo de vida com escopo são criados uma vez por solicitação.

> [!WARNING]
> Se você estiver usando um serviço com escopo em um middleware, injete o serviço no método `Invoke` ou `InvokeAsync`. Não injete por meio de injeção de construtor porque isso força o serviço a se comportar como um singleton.

**Singleton**

Os serviços com tempo de vida singleton são criados na primeira vez em que são solicitados (ou quando `ConfigureServices` for executado se você especificar uma instância nele) e, em seguida, todas as solicitações seguintes usarão a mesma instância. Se o aplicativo exigir o comportamento singleton, é recomendado permitir que o contêiner de serviços gerencie o tempo de vida do serviço, em vez de implementar o padrão de design singleton e gerenciar o tempo de vida do objeto na classe por conta própria.

Os serviços podem ser registrados no contêiner de várias maneiras. Já vimos como registrar uma implementação de serviço com um tipo fornecido, especificando o tipo concreto a ser usado. Além disso, um alocador pode ser especificado, que, em seguida, será usado para criar a instância sob demanda. A terceira abordagem é especificar diretamente a instância do tipo a ser usada, nesse caso, o contêiner nunca tentará criar uma instância (nem descartará a instância).

Para demonstrar a diferença entre essas opções de tempo de vida e registro, considere uma interface simples que representa uma ou mais tarefas como uma *operação* com um identificador exclusivo, `OperationId`. Dependendo de como configurarmos o tempo de vida para esse serviço, o contêiner fornecerá as mesmas instâncias ou instâncias diferentes do serviço para a classe solicitante. Para tornar claro qual tempo de vida está sendo solicitado, criaremos um tipo por opção de tempo de vida:

[!code-csharp[](../fundamentals/dependency-injection/sample/DependencyInjectionSample/Interfaces/IOperation.cs?highlight=5-8)]

Implementamos essas interfaces usando uma única classe, `Operation`, que aceita um `Guid` em seu construtor, ou usa um novo `Guid`, se nenhum é fornecido.

Em seguida, em `ConfigureServices`, cada tipo é adicionado ao contêiner de acordo com seu tempo de vida nomeado:

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Startup.cs?range=26-32)]

Observe que o serviço `IOperationSingletonInstance` usa uma instância específica com uma ID conhecida `Guid.Empty`, para que fique claro quando esse tipo está em uso (seu Guid serão todos zeros). Também registramos um `OperationService` que depende de cada um dos outros tipos `Operation`, de modo que fique claro em uma solicitação se esse serviço está obtendo a mesma instância do controlador ou uma nova, para cada tipo de operação. Tudo o que esse serviço faz é expor suas dependências como propriedades, para que possam ser mostradas no exibição.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Services/OperationService.cs)]

Para demonstrar os tempos de vida de objeto dentro e entre solicitações individuais separadas para o aplicativo, a amostra inclui um `OperationsController` que solicita cada tipo do tipo `IOperation`, bem como um `OperationService`. A ação `Index` então exibe todos os valores `OperationId` do controlador e do serviço.

[!code-csharp[](dependency-injection/sample/DependencyInjectionSample/Controllers/OperationsController.cs)]

Agora, duas solicitações separadas são feitas para esta ação do controlador:

![A exibição Operações do aplicativo Web da Amostra de Injeção de Dependência em execução no Microsoft Edge mostrando valores de ID da Operação (GUIDs) para Transitório, Com Escopo, Singleton e Controlador da Instância e Operações de Serviço de Operação na primeira solicitação.](dependency-injection/_static/lifetimes_request1.png)

![A exibição de operações mostrando os valores de ID da Operação para uma segunda solicitação.](dependency-injection/_static/lifetimes_request2.png)

Observe qual dos valores `OperationId` varia em uma solicitação e entre as solicitações.

* Objetos *transitórios* são sempre diferentes; uma nova instância é fornecida para cada controlador e serviço.

* Objetos *com escopo* são os mesmos em uma solicitação, mas diferentes em diferentes solicitações

* Objetos *singleton* são os mesmos para cada objeto e solicitação (independentemente de uma instância ser fornecida em `ConfigureServices`)

## <a name="resolve-a-scoped-service-within-the-application-scope"></a>Resolver um serviço com escopo dentro do escopo do aplicativo

Crie um [IServiceScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescope) com [IServiceScopeFactory.CreateScope](/dotnet/api/microsoft.extensions.dependencyinjection.iservicescopefactory.createscope) para resolver um serviço com escopo dentro do escopo do aplicativo. Essa abordagem é útil para acessar um serviço com escopo durante a inicialização para executar tarefas de inicialização. O exemplo a seguir mostra como obter um contexto para o `MyScopedService` em `Program.Main`:

```csharp
public static void Main(string[] args)
{
    var host = BuildWebHost(args);

    using (var serviceScope = host.Services.CreateScope())
    {
        var services = serviceScope.ServiceProvider;

        try
        {
            var serviceContext = services.GetRequiredService<MyScopedService>();
            // Use the context here
        }
        catch (Exception ex)
        {
            var logger = services.GetRequiredService<ILogger<Program>>();
            logger.LogError(ex, "An error occurred.");
        }
    }

    host.Run();
}
```

## <a name="scope-validation"></a>Validação de escopo

Quando o aplicativo é executado no ambiente de desenvolvimento no ASP.NET Core 2.0 ou posterior, o provedor de serviço padrão executa verificações para saber se:

* Os serviços com escopo não são resolvidos direta ou indiretamente pelo provedor de serviço raiz.
* Os serviços com escopo não são injetados direta ou indiretamente em singletons.

O provedor de serviços raiz é criado quando [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) é chamado. O tempo de vida do provedor de serviço raiz corresponde ao tempo de vida do aplicativo/servidor quando o provedor começa com o aplicativo e é descartado quando o aplicativo é desligado.

Os serviços com escopo são descartados pelo contêiner que os criou. Se um serviço com escopo é criado no contêiner raiz, o tempo de vida do serviço é promovido efetivamente para singleton, porque ele só é descartado pelo contêiner raiz quando o aplicativo/servidor é desligado. A validação dos escopos de serviço detecta essas situações quando `BuildServiceProvider` é chamado.

Para obter mais informações, confira [Validação de escopo no tópico Hospedagem](xref:fundamentals/hosting#scope-validation).

## <a name="request-services"></a>Serviços de solicitação

Os serviços disponíveis em uma solicitação do ASP.NET de `HttpContext` são expostos por meio da coleção `RequestServices`.

![Caixa de diálogo contextual Serviços de Solicitação HttpContext do IntelliSense informando que os Serviços de Solicitação obtêm ou definem o IServiceProvider que fornece acesso ao contêiner de serviço da solicitação.](dependency-injection/_static/request-services.png)

Os Serviços de Solicitação representam os serviços configurados e solicitados como parte do aplicativo. Quando os objetos especificam dependências, elas são atendidas pelos tipos encontrados em `RequestServices`, não `ApplicationServices`.

Em geral, você não deve usar essas propriedades diretamente, preferindo solicitar os tipos exigidos pelas classes por meio do construtor da classe e permitindo que a estrutura injete essas dependências. Isso resulta em classes que são mais fáceis de testar (confira [Testar e depurar](../testing/index.md)) e são mais fracamente acopladas.

> [!NOTE]
> Prefira solicitar dependências como parâmetros de construtor para acessar a coleção `RequestServices`.

## <a name="designing-services-for-dependency-injection"></a>Criando serviços para injeção de dependência

Você deve criar seus serviços para que usem a injeção de dependência para obter seus colaboradores. Isso significa evitar o uso de chamadas de método estático com estado (que resultam em um "code smell" conhecido como [static cling](http://deviq.com/static-cling/)) e a criação direta de instâncias de classes dependentes em seus serviços. Isso pode ajudar a lembrar a frase [New is Glue](https://ardalis.com/new-is-glue), ao escolher se deseja criar uma instância de um tipo ou solicitá-lo por meio da injeção de dependência. Seguindo os [Princípios SOLID de Design Orientado a Objeto](http://deviq.com/solid/), as classes naturalmente tendem a ser pequenas, bem fatoradas e facilmente testadas.

E se você descobrir que as classes tendem a ter muitas dependências injetadas? Isso geralmente é um sinal de que a classe está tentando fazer muito e, provavelmente, está violando o SRP – o [Princípio da Responsabilidade Única](http://deviq.com/single-responsibility-principle/). Veja se você pode refatorar a classe movendo algumas de suas responsabilidades para uma nova classe. Lembre-se de que as classes `Controller` devem se concentrar nos interesses da interface do usuário, de modo que os detalhes de implementação de acesso a dados e as regras de negócios devem ser mantidos em classes apropriadas a esses [interesses separados](http://deviq.com/separation-of-concerns/).

Especificamente, com relação ao acesso a dados, você pode injetar o `DbContext` nos controladores (supondo que você adicionou o EF ao contêiner de serviços em `ConfigureServices`). Alguns desenvolvedores preferem usar uma interface de repositório para o banco de dados, em vez de injetar o `DbContext` diretamente. O uso de uma interface para encapsular a lógica de acesso a dados em um único local pode minimizar o número de locais que você precisará alterar quando o banco de dados for alterado.

### <a name="disposing-of-services"></a>Descartando serviços

O contêiner chamará `Dispose` para os tipos `IDisposable` criados por ele. No entanto, se você adicionar uma instância ao contêiner por conta própria, ele não será descartado.

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

    // container didn't create instance so it will NOT dispose it
    services.AddSingleton<Service3>(new Service3());
    services.AddSingleton(new Service3());
}
```

> [!NOTE]
> Na versão 1.0, o contêiner chamado descarta *todos* os objetos `IDisposable`, incluindo aqueles que ele não criou.

## <a name="replacing-the-default-services-container"></a>Substituindo o contêiner de serviços padrão

O contêiner de serviços interno destina-se a atender às necessidades básicas da estrutura e a maioria dos aplicativos de consumidor criada nele. No entanto, os desenvolvedores podem substituir o contêiner interno por seu contêiner preferencial. O método `ConfigureServices` normalmente retorna `void`, mas se sua assinatura for alterada para retornar `IServiceProvider`, um contêiner diferente poderá ser configurado e retornado. Há vários contêineres IOC disponíveis para o .NET. Neste exemplo, o pacote [Autofac](https://autofac.org/) é usado.

Primeiro, instale os pacotes de contêiner apropriados:

* `Autofac`
* `Autofac.Extensions.DependencyInjection`

Em seguida, configure o contêiner no `ConfigureServices` e retorne um `IServiceProvider`:

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
> Ao usar um contêiner de DI de terceiros, é necessário alterar `ConfigureServices` para que ele retorne `IServiceProvider` em vez de `void`.

Finalmente, configure o Autofac normalmente em `DefaultModule`:

```csharp
public class DefaultModule : Module
{
    protected override void Load(ContainerBuilder builder)
    {
        builder.RegisterType<CharacterRepository>().As<ICharacterRepository>();
    }
}
```

Em tempo de execução, o Autofac será usado para resolver tipos e injetar dependências. [Saiba mais sobre como usar o Autofac e o ASP.NET Core](http://docs.autofac.org/en/latest/integration/aspnetcore.html).

### <a name="thread-safety"></a>Acesso thread-safe

Os serviços Singleton precisam ser thread-safe. Se um serviço singleton tiver uma dependência em um serviço transitório, o serviço transitório também precisará ser thread-safe, dependendo de como ele é usado pelo singleton.

## <a name="recommendations"></a>Recomendações

Ao trabalhar com a injeção de dependência, lembre-se das seguintes recomendações:

* A DI destina-se a objetos que têm dependências complexas. Controladores, serviços, adaptadores e repositórios são exemplos de objetos que podem ser adicionados à DI.

* Evite armazenar dados e a configuração diretamente na DI. Por exemplo, o carrinho de compras de um usuário normalmente não deve ser adicionado ao contêiner de serviços. A configuração deve usar o [padrão de opções](xref:fundamentals/configuration/options). Da mesma forma, evite objetos de "suporte de dados" que existem somente para permitir o acesso a outro objeto. É melhor solicitar o item real necessário por meio da DI, se possível.

* Evite o acesso estático aos serviços.

* Evite o local do serviço no código do aplicativo.

* Evite o acesso estático a `HttpContext`.

> [!NOTE]
> Como todos os conjuntos de recomendações, talvez você encontre situações em que é necessário ignorar um. Descobrimos que exceções são raras – a maioria, casos muito especiais dentro da própria estrutura.

Lembre-se de que a injeção de dependência é uma *alternativa* aos padrões de acesso a objeto estático/global. Você não poderá obter os benefícios da DI se combiná-lo com o acesso a objeto estático.

## <a name="additional-resources"></a>Recursos adicionais

* [Inicialização de aplicativos](xref:fundamentals/startup)
* [Testar e depurar](xref:testing/index)
* [Ativação de middleware de fábrica](xref:fundamentals/middleware/extensibility)
* [Como escrever um código limpo no ASP.NET Core com injeção de dependência (MSDN)](https://msdn.microsoft.com/magazine/mt703433.aspx)
* [Design de aplicativo gerenciado por contêiner, prelúdio: a que local o contêiner pertence?](https://blogs.msdn.microsoft.com/nblumhardt/2008/12/26/container-managed-application-design-prelude-where-does-the-container-belong/)
* [Princípio de Dependências Explícitas](http://deviq.com/explicit-dependencies-principle/)
* [Contêineres de Inversão de Controle e o padrão de Injeção de Dependência](https://www.martinfowler.com/articles/injection.html) (Fowler)
