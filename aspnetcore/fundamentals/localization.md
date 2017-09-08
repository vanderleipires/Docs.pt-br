---
title: "Globalização e localização em ASP.NET Core"
author: rick-anderson
description: "Saiba como o ASP.NET Core fornece serviços e middleware para localizar conteúdo em diferentes idiomas e culturas."
keywords: "ASP.NET Core, localização, cultura, idioma, arquivo de recurso, globalização, internacionalização, localidade"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 7f275a09-f118-41c9-88d1-8de52d6a5aa1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/localization
ms.openlocfilehash: c6c9db21a95131a3d7920054e32004791b499c11
ms.sourcegitcommit: fb518f856f31fe53c09196a13309eacb85b37a22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2017
---
# <a name="globalization-and-localization-in-aspnet-core"></a>Globalização e localização em ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), e [Ateya Hisham Bin](https://twitter.com/hishambinateya)

Criar um site multilíngue com ASP.NET Core permitirá que seu site alcançar um público maior. ASP.NET Core fornece serviços e middleware para localizar em diferentes idiomas e culturas.

Internacionalização envolve [globalização](https://msdn.microsoft.com/library/aa292081(v=vs.71).aspx) e [localização](https://msdn.microsoft.com/library/aa292137(v=vs.71).aspx). Globalização é o processo de criação de aplicativos que dão suporte a diferentes culturas. Globalização adiciona suporte para entrada, exibição e saída de um conjunto definido de scripts de idiomas relacionados a áreas geográficas específicas.

A localização é o processo de adaptar um aplicativo globalizado, que você já tiver processado para localização, para uma determinada cultura/localidade.  Para obter mais informações, consulte **termos de globalização e localização** perto do final deste documento.

Localização do aplicativo envolve o seguinte:

1. Verifique o conteúdo do aplicativo localizável

2. Fornecer recursos localizados para os idiomas e culturas que você oferece suporte

3. Implementar uma estratégia para selecionar a idioma/cultura para cada solicitação

## <a name="make-the-apps-content-localizable"></a>Verifique o conteúdo do aplicativo localizável

Introduzido no ASP.NET Core `IStringLocalizer` e `IStringLocalizer<T>` foram projetados para melhorar a produtividade ao desenvolver aplicativos de localizado. `IStringLocalizer`usa o [ResourceManager](https://msdn.microsoft.com/library/system.resources.resourcemanager(v=vs.110).aspx) e [ResourceReader](https://msdn.microsoft.com/library/system.resources.resourcereader(v=vs.110).aspx) para fornecer recursos específicos de cultura em tempo de execução. A interface simple tem um indexador e um `IEnumerable` para retornar cadeias de caracteres localizadas. `IStringLocalizer`não requer que você armazene as cadeias de caracteres de idioma padrão em um arquivo de recurso. Você pode desenvolver um aplicativo direcionado para a localização e não precisa criar arquivos de recurso no início do desenvolvimento. O código a seguir mostra como encapsular a cadeia de caracteres "Sobre o título" para localização.

[!code-csharp[Main](localization/sample/Controllers/AboutController.cs)]

No código acima, o `IStringLocalizer<T>` implementação vêm [injeção de dependência](dependency-injection.md). Se o valor localizado do "Sobre o título" não foi encontrado, a chave do indexador for retornada, ou seja, a cadeia de caracteres "Sobre o título". Você pode deixar o padrão de literais de cadeias de caracteres no aplicativo e encapsulá-los no localizador, para que você possa se concentrar no desenvolvimento do aplicativo. Você pode desenvolve seu aplicativo com o idioma padrão e prepará-la para a etapa de localização sem primeiro criar um arquivo de recurso padrão. Como alternativa, você pode usar a abordagem tradicional e fornecer uma chave para recuperar a cadeia de caracteres de idioma padrão. Para muitos desenvolvedores novo fluxo de trabalho de não ter um idioma padrão *. resx* de arquivo e simplesmente encapsulamento os literais de cadeia de caracteres podem reduzir a sobrecarga de localização de um aplicativo. Outros desenvolvedores preferirão o fluxo de trabalho tradicional, como ele pode tornar mais fácil trabalhar com literais de cadeia de caracteres mais longas e torná-lo mais fácil atualizar cadeias de caracteres localizadas.

Use o `IHtmlLocalizer<T>` implementação para recursos que contêm o HTML. `IHtmlLocalizer`HTML codifica os argumentos que são formatados na cadeia de caracteres de recurso, mas não a cadeia de caracteres de recurso. No exemplo realçado abaixo, apenas o valor de `name` parâmetro é codificada em HTML.

[!code-csharp[Main](../fundamentals/localization/sample/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

Observação: Você geralmente deseja localizar somente texto e não em HTML.

O nível mais baixo, você pode obter `IStringLocalizerFactory` de [injeção de dependência](dependency-injection.md):

[!code-csharp[Main](localization/sample/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

O código anterior demonstra cada fábrica de dois métodos de criar.

Você pode particionar suas cadeias de caracteres localizadas por controlador, área, ou ter apenas um contêiner. No aplicativo de exemplo, uma classe fictícia chamada `SharedResource` é usado para recursos compartilhados.

[!code-csharp[Main](localization/sample/Resources/SharedResource.cs)]

Alguns desenvolvedores usam o `Startup` classe para conter cadeias de caracteres globais ou compartilhadas.  No exemplo abaixo, o `InfoController` e `SharedResource` os localizadores são usados:

[!code-csharp[Main](localization/sample/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>Localização de exibição

O `IViewLocalizer` serviço fornece cadeias de caracteres localizadas para uma [exibição](http://docs.asp.net/projects/mvc/en/latest/views/index.html). O `ViewLocalizer` classe implementa essa interface e encontra o local do recurso do caminho de arquivo do modo de exibição. O código a seguir mostra como usar a implementação padrão de `IViewLocalizer`:

[!code-HTML[Main](localization/sample/Views/Home/About.cshtml)]

A implementação padrão de `IViewLocalizer` localiza o arquivo de recurso com base no nome do arquivo do modo de exibição. Não há nenhuma opção para usar um arquivo de recurso compartilhado global. `ViewLocalizer`implementa o localizador usando `IHtmlLocalizer`, portanto, Razor não HTML codificar a cadeia de caracteres localizada. Você pode parametrizar cadeias de caracteres de recurso e `IViewLocalizer` HTML codifica os parâmetros, mas não a cadeia de caracteres de recurso. Considere a seguinte marcação Razor:

```HTML
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
   ```

Um arquivo de recurso francês pode conter o seguinte:

| Chave | Valor |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

Modo de exibição renderizado conteria a marcação HTML do arquivo de recurso.

Notas:
- Exibição de localização requer o pacote NuGet "Localization.AspNetCore.TagHelpers".
- Você geralmente deseja localizar somente o texto e não em HTML.

Para usar um arquivo de recurso compartilhado em uma exibição, inserir `IHtmlLocalizer<T>`:

[!code-HTML[Main](../fundamentals/localization/sample/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>Localização de DataAnnotations

Mensagens de erro de DataAnnotations são localizadas com `IStringLocalizer<T>`. Usando a opção `ResourcesPath = "Resources"`, mensagens de erro no `RegisterViewModel` podem ser armazenados em qualquer um dos seguintes caminhos:

* Resources/ViewModels.Account.RegisterViewModel.fr.resx
* Resources/ViewModels/Account/RegisterViewModel.fr.resx

[!code-csharp[Main](localization/sample/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

No núcleo do ASP.NET MVC 1.1.0 e atributos mais, a validação não são localizadas. Núcleo do ASP.NET MVC 1.0 **não** pesquisar cadeias de caracteres localizadas para atributos de não validação.

<a name=one-resource-string-multiple-classes></a>
### <a name="using-one-resource-string-for-multiple-classes"></a>Usando uma cadeia de caracteres de recurso para várias classes

O código a seguir mostra como usar uma cadeia de caracteres de recurso para atributos de validação com várias classes:

```
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

No código anterior, `SharedResource` é a classe correspondente a resx onde as mensagens de validação são armazenadas. Com essa abordagem, DataAnnotations usará apenas `SharedResource`, em vez de para cada classe de recurso. 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>Fornecer recursos localizados para os idiomas e culturas que você oferece suporte  

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures e SupportedUICultures

ASP.NET Core permite que você especifique dois valores de cultura, `SupportedCultures` e `SupportedUICultures`. O [CultureInfo](https://msdn.microsoft.com/library/system.globalization.cultureinfo(v=vs.110).aspx) de objeto para `SupportedCultures` determina os resultados das funções dependentes de cultura, como data, hora, número e formatação de moeda. `SupportedCultures`também determina a ordem de classificação de comparações de cadeia de caracteres de texto e convenções de maiusculas e minúsculas. Consulte [CultureInfo](https://msdn.microsoft.com/library/system.globalization.cultureinfo.currentculture%28v=vs.110%29.aspx) para obter mais informações sobre como o servidor obtém a cultura. O `SupportedUICultures` determina qual converte cadeias de caracteres (de *. resx* arquivos) são pesquisados pelo [ResourceManager](https://msdn.microsoft.com/library/system.resources.resourcemanager(v=vs.110).aspx). O `ResourceManager` simplesmente procura cadeias de caracteres específicas da cultura que é determinado pelo `CurrentUICulture`. Cada thread no .NET tem `CurrentCulture` e `CurrentUICulture` objetos. ASP.NET Core inspeciona esses valores de durante a renderização funções dependentes de cultura. Por exemplo, se a cultura do thread atual está definida como "en-US" (em inglês, Estados Unidos), `DateTime.Now.ToLongDateString()` exibe "Quinta-feira, 18 de fevereiro de 2016", mas se `CurrentCulture` for definido como "es-ES" (espanhol, Espanha) a saída será "jueves, 18 de febrero de 2016".

## <a name="working-with-resource-files"></a>Trabalhando com arquivos de recurso

Um arquivo de recurso é um mecanismo útil para separar cadeias de caracteres localizáveis de código. Cadeias de caracteres traduzidas para o idioma padrão não são isoladas *. resx* arquivos de recurso. Por exemplo, você talvez queira criar o arquivo de recurso espanhol denominado *Welcome.es.resx* contendo convertidas de cadeias de caracteres. "es" são o código de idioma para espanhol. Para criar esse arquivo de recurso no Visual Studio:

1. Em **Solution Explorer**, clique com o botão direito na pasta que conterá o arquivo de recurso > **adicionar** > **Novo Item**.

    ![Menu de contexto aninhado: no Gerenciador de soluções, um menu contextual é aberto para recursos. Um segundo menu contextual é aberto para adicionar mostrando o comando de novo Item realçado.](localization/_static/newi.png)

2. No **pesquisar modelos instalados** caixa, digite "recursos" e nomeie o arquivo.

    ![Adicionar Novo Item de caixa de diálogo](localization/_static/res.png)

3. Insira o valor da chave (cadeia de caracteres nativo) no **nome** coluna e a cadeia de caracteres traduzida no **valor** coluna.

    ![Arquivo Welcome.es.resx (o arquivo de recurso de boas-vindas para espanhol) com a palavra Hello na coluna Nome e a palavra Hola (Olá em espanhol) na coluna valor](localization/_static/hola.png)

    O Visual Studio mostra a *Welcome.es.resx* arquivo.

    ![Gerenciador de soluções mostrando o arquivo de recurso de boas-vindas Espanhol (es)](localization/_static/se.png)

<a name="error"></a>

Se você estiver usando a versão do Visual Studio 2017 Preview 15,3, você obterá um indicador de erro no editor de recursos. Remover o *ResXFileCodeGenerator* valor o *ferramenta personalizada* grade de propriedades para evitar essa mensagem de erro:

![Editor de resx](localization/_static/err.png)

Como alternativa, você pode ignorar esse erro. Esperamos que para corrigir isso na próxima versão.

## <a name="resource-file-naming"></a>Nomes de arquivos de recurso

Recursos são nomeados para o nome completo do tipo de sua classe menos o nome do assembly. Por exemplo, um recurso francês em um projeto cuja assembly principal é `LocalizationWebsite.Web.dll` para a classe `LocalizationWebsite.Web.Startup` seria nomeado *Startup.fr.resx*. Um recurso para a classe `LocalizationWebsite.Web.Controllers.HomeController` seria nomeado *Controllers.HomeController.fr.resx*. Se o namespace da classe de destino não é o mesmo que o nome do assembly, você precisará do nome completo do tipo. Por exemplo, no exemplo de projeto um recurso para o tipo de `ExtraNamespace.Tools` seria nomeado *ExtraNamespace.Tools.fr.resx*.

No projeto de exemplo, o `ConfigureServices` método define o `ResourcesPath` "Recursos", então o caminho relativo do projeto para arquivo de recurso do controlador principal francês é *Resources/Controllers.HomeController.fr.resx*. Como alternativa, você pode usar pastas para organizar arquivos de recurso. Para o controlador principal, o caminho deverá ser *Resources/Controllers/HomeController.fr.resx*. Se você não usar o `ResourcesPath` opção, o *. resx* arquivo entrarão no diretório base do projeto. O arquivo de recurso `HomeController` seria nomeado *Controllers.HomeController.fr.resx*. A opção de usar a convenção de nomenclatura de ponto ou caminho depende de como você deseja organizar seus arquivos de recurso.

| Nome do recurso | Ponto ou caminho de nomenclatura |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | Ponto  |
| Resources/Controllers/HomeController.fr.resx  | Caminho |
|    |     |

Arquivos de recursos usando `@inject IViewLocalizer` em modos de exibição Razor seguem um padrão semelhante. O arquivo de recurso para um modo de exibição pode ser nomeado usando nomes de ponto ou nomes de caminho. Arquivos de recurso de exibição Razor imitam o caminho do seu arquivo de exibição associada. Assumindo que definimos o `ResourcesPath` "Recursos", o arquivo de recurso francês associada com o *Views/Home/About.cshtml* exibição pode ser um dos seguintes:

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

Se você não usar o `ResourcesPath` opção, o *. resx* arquivo para um modo de exibição deve estar localizado na mesma pasta que o modo de exibição.

Se você remover o designador de cultura. "fr" e você tem a cultura definida como francês (via cookie ou outro mecanismo), o arquivo de recurso padrão é lido e cadeias de caracteres são localizadas. O Gerenciador de recursos designa um recurso padrão ou de retorno, quando nada atende sua cultura solicitada está servido arquivo resx sem um designador de cultura. Se você quiser retornar apenas a chave quando um recurso ausente para a solicitação de cultura não deve ter um arquivo de recurso padrão.

### <a name="generating-resource-files-with-visual-studio"></a>Gerar arquivos de recurso com o Visual Studio

Se você criar um arquivo de recurso no Visual Studio sem uma cultura no nome do arquivo (por exemplo, *Welcome.resx*), o Visual Studio criará uma classe c# com uma propriedade para cada cadeia de caracteres. É geralmente não é o desejado com o ASP.NET Core; Você normalmente não tem um padrão *. resx* arquivo de recurso (um *. resx* arquivo sem o nome de cultura). Sugerimos que você criar o *. resx* arquivo com um nome de cultura (por exemplo *Welcome.fr.resx*). Quando você cria um *. resx* arquivo com um nome de cultura, o Visual Studio não irá gerar o arquivo de classe. Estimamos que muitos desenvolvedores serão **não** criar um arquivo de recurso de idioma padrão.

### <a name="adding-other-cultures"></a>Adicionando outras culturas

Cada combinação de idioma e cultura (que não seja o idioma padrão) requer um arquivo de recurso exclusivo. Criar arquivos de recursos para diferentes culturas e localidades criando novos arquivos de recurso nos quais os códigos de idioma ISO fazem parte do nome do arquivo (por exemplo, **en-us**, **fr-ca**, e  **en-gb**). Esses códigos ISO são colocados entre o nome do arquivo e o *. resx* arquivo de extensão de nome, como em *Welcome.es-MX. resx* (espanhol/México). Para especificar uma linguagem culturalmente neutra, remova o código do país (`MX` no exemplo anterior). É o nome do arquivo de recurso espanhol culturalmente neutro *Welcome.es.resx*.

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>Implementar uma estratégia para selecionar a idioma/cultura para cada solicitação  

### <a name="configuring-localization"></a>Configuração de localização

Localização é configurada no `ConfigureServices` método:

[!code-csharp[Main](localization/sample/Program.cs?name=snippet1)]

* `AddLocalization`Adiciona os serviços de localização para o contêiner de serviços. O código acima também define o caminho de recursos em "Recursos".

* `AddViewLocalization`Adiciona suporte para os arquivos de exibição localizado. Nesse modo de exibição de exemplo localização com base no sufixo do arquivo do modo de exibição. Por exemplo "fr" no *Index.fr.cshtml* arquivo.

* `AddDataAnnotationsLocalization`Adiciona suporte para as versões localizadas `DataAnnotations` validação mensagens pelo `IStringLocalizer` abstrações.

### <a name="localization-middleware"></a>Middleware de localização

A cultura atual em uma solicitação é definida na localização [Middleware](middleware.md). O middleware de localização está habilitado no `Configure` método *Program.cs* arquivo. Observe que o middleware de localização deve ser configurado antes que qualquer middleware que pode verificar a cultura de solicitação (por exemplo, `app.UseMvcWithDefaultRoute()`).

[!code-csharp[Main](localization/sample/Program.cs?name=snippet2)]

`UseRequestLocalization`Inicializa uma `RequestLocalizationOptions` objeto. Em cada solicitação, a lista de `RequestCultureProvider` no `RequestLocalizationOptions` é enumerado e o primeiro provedor com êxito pode determinar a cultura de solicitação é usado. Os provedores padrão são provenientes de `RequestLocalizationOptions` classe:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

A lista padrão varia de mais específico ao menos específico. Posteriormente neste artigo veremos como você pode alterar a ordem e até mesmo adicionar um provedor de cultura personalizada. Se nenhum dos provedores pode determinar a cultura de solicitação, o `DefaultRequestCulture` é usado.

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

Alguns aplicativos usarão uma cadeia de caracteres de consulta para definir o [culture e UI culture](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx#Current). Para aplicativos que usam a abordagem de cabeçalho Accept-Language ou de cookie, adicionar uma cadeia de caracteres de consulta para a URL é útil para depurar e testar o código. Por padrão, o `QueryStringRequestCultureProvider` é registrado como o primeiro provedor de localização no `RequestCultureProvider` lista. Você passa a consulta, parâmetros de cadeia de caracteres `culture` e `ui-culture`. O exemplo a seguir define a cultura específica (idioma e região) para espanhol, México:

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

Se você passar somente em um dos dois (`culture` ou `ui-culture`), o provedor de cadeia de caracteres de consulta definirá os dois valores usando o passado no. Por exemplo, definir a cultura do apenas definirá ambos o `Culture` e `UICulture`:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

Aplicativos de produção geralmente fornecem um mecanismo para definir a cultura com o cookie de cultura do ASP.NET Core. Use o `MakeCookieValue` método para criar um cookie.

O `CookieRequestCultureProvider` `DefaultCookieName` retorna o nome do cookie padrão usado para acompanhar o usuário preferencial de informações de cultura. O nome do cookie padrão é ". AspNetCore.Culture".

O formato de cookie é `c=%LANGCODE%|uic=%LANGCODE%`, onde `c` é `Culture` e `uic` é `UICulture`, por exemplo:

    c=en-UK|uic=en-US

Se você especificar apenas uma das informações de cultura e cultura da interface do usuário, a cultura especificada será usada para informações de cultura e cultura da interface do usuário.

### <a name="the-accept-language-http-header"></a>O cabeçalho HTTP Accept-Language

O [cabeçalho Accept-Language](https://www.w3.org/International/questions/qa-accept-lang-locales) originalmente deve especificar o idioma do usuário e é configurável na maioria dos navegadores. Essa configuração indica que o navegador foi definido para enviar ou foi herdado do sistema operacional subjacente. O cabeçalho Accept-Language HTTP de uma solicitação do navegador não é uma maneira infalível para detectar o idioma preferencial do usuário (consulte [definir preferências de idioma em um navegador](https://www.w3.org/International/questions/qa-lang-priorities.en.php)). Um aplicativo de produção deve incluir uma maneira para que um usuário personalize sua opção de cultura.

### <a name="setting-the-accept-language-http-header-in-ie"></a>Definindo o cabeçalho Accept-Language HTTP no IE

1. Do ícone de engrenagem, toque em **opções da Internet**.

2. Toque em **idiomas**.

    ![Opções da Internet](localization/_static/lang.png)

3. Toque em **definir preferências de idioma**.

4. Toque em **adicionar um idioma**.

5. Adicione o idioma.

6. O idioma de toque e em **mover para cima**.

### <a name="using-a-custom-provider"></a>Usando um provedor personalizado

Suponha que você deseja permitir que os clientes armazenam seu idioma e cultura em seus bancos de dados. Você pode escrever um provedor para pesquisar esses valores para o usuário. O código a seguir mostra como adicionar um provedor personalizado:

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

Use `RequestLocalizationOptions` para adicionar ou remover provedores de localização.

### <a name="setting-the-culture-programmatically"></a>Definir a cultura programaticamente

Este exemplo **Localization.StarterWeb** projeto [GitHub](https://github.com/aspnet/entropy) contém a interface do usuário para definir o `Culture`. O *Views/Shared/_SelectLanguagePartial.cshtml* arquivo permite que você selecione a cultura da lista de culturas com suporte:

[!code-HTML[Main](localization/sample/Views/Shared/_SelectLanguagePartial.cshtml)]

O *Views/Shared/_SelectLanguagePartial.cshtml* arquivo é adicionado para o `footer` seção do arquivo de layout para que fique disponível para todos os modos de exibição:

[!code-HTML[Main](localization/sample/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

O `SetLanguage` método define o cookie de cultura.

[!code-csharp[Main](localization/sample/Controllers/HomeController.cs?range=57-67)]

Não é possível conectar o *_SelectLanguagePartial.cshtml* para código de exemplo para este projeto. O **Localization.StarterWeb** projeto [GitHub](https://github.com/aspnet/entropy) tem o código para o fluxo do `RequestLocalizationOptions` para um Razor parcial por meio de [injeção de dependência](dependency-injection.md) contêiner.

## <a name="globalization-and-localization-terms"></a>Termos de globalização e localização

O processo de localização de seu aplicativo também requer uma compreensão básica relevantes de conjuntos de caracteres comumente usados em desenvolvimento de software moderno e entender os problemas associados a eles. Embora todos os computadores armazenam texto como números (códigos de), diferentes sistemas armazenam o mesmo texto usando números diferentes. O processo de localização se refere ao transferir a interface do usuário de aplicativo (IU) para uma cultura/localidade específica.

[Localização](https://msdn.microsoft.com/library/aa292135(v=vs.71).aspx) é um processo intermediário para verificar se um aplicativo globalizado está pronto para localização.

O [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) de formato para o nome de cultura é "<languagecode2>-< país/regioncode2 >", onde <languagecode2> é o código de idioma e < país/regioncode2 > é o código de subcultura. Por exemplo, `es-CL` para Espanhol (Chile) `en-US` para inglês (Estados Unidos), e `en-AU` para inglês (Austrália). [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) é uma combinação de um código de cultura de duas letras minúsculas associado a um idioma do ISO 639 e um ISO 3166 código subcultura de duas letras maiusculas associado em um país ou região.  Consulte [nome de cultura de idioma](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).

Internacionalização é muitas vezes abreviada como "I18N". A abreviação leva as primeiras e últimas letras e o número de letras entre elas, portanto 18 significa o número de letras entre o primeiro "I" e o último "N". O mesmo se aplica à globalização (G11N) e a localização (L10N).

Termos de:

* Globalização (G11N): O processo de criação de um aplicativo dão suporte a regiões e idiomas diferentes.
* Localização (L10N): O processo de personalização de um aplicativo para um determinado idioma e região.
* Internacionalização (I18N): Descreve globalização e localização.
* Cultura: É uma linguagem e, opcionalmente, uma região.
* Cultura neutra: uma cultura que tem um idioma especificado, mas não uma região. (por exemplo "en", "es")
* Cultura específica: uma cultura que tenha um idioma especificado e uma região. (por exemplo "en-US", "en-GB", "es-CL")
* Localidade: Uma localidade é o mesmo que uma cultura.

## <a name="additional-resources"></a>Recursos adicionais

* [Projeto Localization.StarterWeb](https://github.com/aspnet/entropy) usado no artigo.
* [Arquivos de recurso no Visual Studio](https://msdn.microsoft.com/library/xbx3z216(v=vs.110).aspx#VSResFiles)
* [Recursos em arquivos. resx](https://msdn.microsoft.com/library/xbx3z216(v=vs.110).aspx#ResourcesFiles)
