---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: Os principais recursos do ASP.NET Web Pages 2 | Microsoft Docs
author: microsoft
description: Este tópico fornece uma visão geral dos principais novos recursos no ASP.NET Web Pages 2, uma estrutura de programação na web leve que é incluída com o WebMatr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: 3cdb9d83e0f612ad7404bfaa9721b580916e112d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385260"
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Os principais recursos do ASP.NET Web Pages 2
====================
por [Microsoft](https://github.com/microsoft)

> Este artigo fornece uma visão geral dos principais recursos novos no ASP.NET páginas da Web 2 RC, uma estrutura de programação na web leve que acompanha [o Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **O que está incluído:** 
> 
> - [Instalar o WebMatrix](#install)
> - [Recursos novos e aprimorados](#New_and_Enhanced_Features)
> 
>     - [Alterações para a versão RC](#Changes_for_the_RC_Version)
>     - [Alterações da versão Beta](#Changes_for_the_Beta_Version)
>     - [Usando os modelos de Site novos e atualizados](#templates)
>     - [Validação de entrada do usuário](#validation)
>     - [Habilitar os logons do Facebook e outros sites usando OAuth e OpenID](#oauthsetup)
>     - [Adicionar mapas usando o auxiliar de mapas](#maphelper)
>     - [Executando aplicativos de páginas da Web lado a lado](#sidebyside)
>     - [Renderização de páginas para dispositivos móveis](#mobile)
> - [Recursos adicionais](#resources)
> 
> > [!NOTE]
> > Este tópico pressupõe que você está usando o WebMatrix para trabalhar com seu código ASP.NET Web Pages 2. No entanto, como com páginas da Web 1, você também pode criar sites da Web Pages 2 usando o Visual Studio, que lhe aprimorado de recursos do IntelliSense e depuração. Para trabalhar com páginas da Web no Visual Studio, você deve instalar primeiro o Visual Studio 2010 SP1, o Visual Web Developer Express 2010 SP1 ou o Visual Studio 11 Beta. Em seguida, instale o ASP.NET MVC 4 Beta, que inclui modelos e ferramentas para a criação de aplicativos ASP.NET MVC 4 e Web Pages 2 no Visual Studio.
> 
> 
> *Última atualização: 18 de junho de 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Instalar o WebMatrix

Para instalar as páginas da Web, você pode usar o Microsoft Web Platform Installer, que é um aplicativo gratuito que torna mais fácil instalar e configurar tecnologias relacionadas à web. Você instalará a versão Beta 2 do WebMatrix, que inclui a versão Beta 2 de páginas da Web.

1. Navegue até a página de instalação para a versão mais recente do Web Platform Installer:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Se você já tiver o WebMatrix 1 instalado, a instalação atualiza-lo para o WebMatrix 2 Beta. Você pode executar sites que foram criados usando a versão 1 ou 2 no mesmo computador. Para obter mais informações, consulte a seção sobre [aplicativos de páginas da Web em execução lado a lado](#sidebyside).
2. Escolher **instalar agora**. 

    Se você usar o Internet Explorer, vá para a próxima etapa. Se você usar um navegador diferente, como o Mozilla Firefox ou o Google Chrome, você precisará salvar o *Webmatrix.exe* arquivo em seu computador. Salve o arquivo e, em seguida, clique nele para iniciar o instalador.
3. Execute o instalador e escolha o **instalar** botão. Isso instala o WebMatrix e páginas da Web.

## <a id="New_and_Enhanced_Features"></a>  Recursos novos e aprimorados

### <a id="Changes_for_the_RC_Version"></a>  Alterações para a versão RC (junho de 2012)

O lançamento da versão RC em junho de 2012 tem algumas alterações da atualização da versão Beta que foi lançado em março de 2012. Essas alterações são:

- Um `Validation.AddFormError` método foi adicionado para o `Validation` auxiliar. Isso será útil se você executar a validação manualmente (por exemplo, você valida um valor que é passado na cadeia de caracteres de consulta) e você deseja adicionar uma mensagem de erro que pode ser exibida pelo `Html.ValidationSummary` método. Para obter mais informações, consulte a seção [Validando dados que não vêm diretamente de usuários](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) na [Validando entrada do usuário em páginas da Web do ASP.NET (Razor) Sites](https://go.microsoft.com/fwlink/?LinkId=253002).
- A funcionalidade de agrupamento e minificação foi removida dos assemblies de núcleo ASP.NET Web Pages 2. Como consequência, o `Assets` auxiliar listado neste documento não está disponível. Em vez disso, você deve instalar o [otimização ASP.NET](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) pacote do NuGet. Para obter mais informações, consulte [agrupamento e Minificação de ativos em um Site de páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=255373).
- Assemblies adicionais para dar suporte a ASP.NET Web Pages 2 foram adicionados. O efeito visível somente dessa alteração é que você pode ver mais assemblies em um site *bin* pasta depois de criar um site ou implantar um site em um provedor de hospedagem.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Alterações para a versão Beta (fevereiro de 2012)

A versão Beta lançada em fevereiro de 2012 tem apenas algumas alterações da versão Beta foi lançado em dezembro de 2011. Essas alterações são:

- Agora, o Razor dá suporte a atributos condicionais. Em um HTML elemento, se você definir um atributo como um valor que é resolvido no código do servidor para `false` ou `null`, o ASP.NET não processa o atributo em todos os. Por exemplo, imagine que você tem a seguinte marcação para uma caixa de seleção:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Se o valor de `checked1` resolve `false` ou a `null`, o `checked` atributo não é renderizado. Isso é uma alteração significativa.
- O `Validation.GetHtml` método foi renomeado para `Validation.For`. Essa é uma alteração significativa; `Validation.GetHtml` não funcionarão na versão Beta.
- Agora você pode incluir a `~` operador na marcação para referenciar a raiz do site sem usar o `Href` função. (Ou seja, o analisador Razor agora pode encontrar e resolver o `~` operador sem a necessidade de uma chamada de método explícito para `Href`.) O `Href` método ainda funciona, portanto, isso não é uma alteração significativa.

    Por exemplo, se você já tinha marcação como essa:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Agora você pode usar a marcação como essa:

    `<a href="~/Default.cshtml">Home</a>`
- O `Scripts` auxiliar para o gerenciamento de ativos (recurso) foi substituído com o `Assets` auxiliar, que tem métodos ligeiramente diferentes, como o seguinte:

  - Para `Scripts.Add`, use `Assets.AddScript`
  - Para `Scripts.GetScriptTags`, use `Assets.GetScripts`

    Essa é uma alteração significativa; o `Scripts` classe não está disponível na versão Beta. Os exemplos de código neste documento que usam o gerenciamento de ativos foram atualizados com essa alteração.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Usando os modelos de Site novos e atualizados

O **Starter Site** modelo tiver sido atualizado para que ele seja executado no Web Pages 2 por padrão. Ele também inclui os seguintes novos recursos:

- Renderização de página de dispositivos móveis. Com o uso de estilos CSS e o `@media` seletor, o **Starter Site** fornece melhor renderização de páginas em telas menores, inclusive telas de dispositivos móveis.
- Opções de autenticação e associação aprimoradas. Você pode permitir que os usuários façam logon em seu site usando suas contas de outros sites de rede social, como Windows Live, Facebook e Twitter. Para obter mais informações, consulte o [permitindo logons do Facebook e outros Sites usando OAuth e OpenID](#oauthsetup) seção.
- Elementos do HTML5.

O novo **Site Pessoal** modelo permite que você crie um site que contém um blog pessoal, uma página de fotos e uma página do Twitter. Você pode personalizar um site com base nas **Site Pessoal** modelo fazendo o seguinte:

- Alterar a aparência do site, editando o arquivo de layout (*\_SiteLayout.cshtml*) e o arquivo de estilos (*CSS*).
- Instale pacotes do NuGet que adicionam funcionalidade ao seu site. Para obter informações sobre como instalar pacotes, incluindo o ASP.NET Web Helpers Library, consulte o tutorial [instalando auxiliares](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Para acessar o **Site Pessoal** modelo, escolha **modelos** sobre o WebMatrix **início rápido** tela.

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

No **modelos** diálogo caixa, escolha o **Site Pessoal** modelo.

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

Página de aterrissagem do **Site Pessoal** modelo permite que você siga os links para configurar seu blog, Twitter, página e página de fotos.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Validação de entrada do usuário

1 de páginas da Web, para validar a entrada do usuário em formulários enviados, você usa o `System.Web.WebPages.Html.ModelState` classe. (Isso é ilustrado em vários dos exemplos de código do tutorial do Web Pages 1 intitulada [trabalhando com dados](../data/5-working-with-data.md).) Você ainda pode usar essa abordagem no Web Pages 2. No entanto, o Web Pages 2 também oferece ferramentas aprimoradas para validar a entrada do usuário:

- Novas classes de validação, incluindo `System.Web.WebPages.ValidationHelper` e `System.Web.WebPages.Validator`, que permitem que você realizar tarefas de validação avançados com algumas linhas de código.
- Opcionalmente, validação do lado do cliente, que fornece comentários imediatos ao usuário em vez de exigir uma ida e volta ao servidor para verificar erros de validação. (Por motivos de segurança, validação é executada no servidor, mesmo se as verificações foram executadas no cliente com antecedência.)

Para usar os novos recursos de validação, faça o seguinte:

No código da página, registre um elemento a ser validada usando métodos das `Validation` auxiliar: `Validation.RequireField`, `Validation.RequireFields` (para registrar vários elementos como obrigatória), ou `Validation.Add`. O `Add` método permite que você especifique outros tipos de verificações de validação, como verificação, comparando as entradas nos campos diferentes, verificações de comprimento de cadeia de caracteres de tipo de dados e padrões (usando expressões regulares). Estes são alguns exemplos:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Para exibir um erro específico do campo, chame `Html.ValidationMessage` na marcação para cada elemento que está sendo validada:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Para exibir um resumo (`<ul>` lista) de todos os erros na página, `Html.ValidationSummary` na marcação:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Essas etapas são suficientes para implementar a validação do lado do servidor. Se você quiser adicionar a validação do lado do cliente, além de fazer o seguinte.

Adicione as seguintes referências de arquivo de script dentro de `<head>` seção de uma página da web. As primeiras duas referências de script apontam para os arquivos remotos em um servidor de distribuição de conteúdo (CDN) da rede. A terceira referência aponta para um arquivo de script de local. Aplicativos de produção devem implementar um fallback quando o CDN não estiver disponível. Teste o fallback.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

A maneira mais fácil de obter uma cópia local do *jquery.validate.unobtrusive.min.js* biblioteca é criar um novo site de páginas da Web com base em um dos modelos de site (como o Site inicial). O site criado pelo modelo inclui *jquery.validate.unobtrusive.js* arquivo em sua pasta de Scripts, da qual você pode copiá-lo ao seu site.

Se seu site usa um<em>\_SiteLayout</em> página para controlar o layout da página, você poderá incluir essas referências de script na página para que a validação está disponível para todas as páginas de conteúdo. Se você quiser executar a validação apenas em páginas específicas, você pode usar o Gerenciador de ativos para registrar os scripts apenas nessas páginas. Para fazer isso, chame `Assets.AddScript(path)` na página que você deseja validar e fazer referência a cada um dos arquivos de script. Em seguida, adicione uma chamada para `Assets.GetScripts` no  <em>\_SiteLayout</em> página para renderizar o registrado `<script>` marcas. Para obter mais informações, consulte a seção [registrando Scripts com o Gerenciador de ativos](#resmanagement).

Na marcação para um elemento individual, chame o `Validation.For` método. Esse método emite atributos que o jQuery pode capturar a fim de fornecer a validação do lado do cliente. Por exemplo:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

O exemplo a seguir mostra uma página que valida a entrada do usuário em um formulário. Para executar e testar esse código de validação, faça o seguinte:

1. Criar um novo site usando um dos modelos de site do WebMatrix 2 que inclui um *Scripts* pasta, como o **Starter Site** modelo.
2. No novo site, crie um novo *. cshtml* página e substitua o conteúdo da página com o código a seguir.
3. Execute a página em um navegador. Insira os valores válidos e inválidos para ver os efeitos sobre a validação. Por exemplo, deixe em branco um campo obrigatório ou digite uma letra na **créditos** campo.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Aqui está a página quando um usuário envia uma entrada válida:

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Aqui está a página quando um usuário a envia com um campo obrigatório deixado em branco:

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Aqui está a página quando um usuário a envia com algo diferente de um número inteiro na **créditos** campo:

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Para obter mais informações, consulte as postagens de blog a seguir:

- [Atualizada a validação no Web Pages v2](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) Noções básicas de adição de validação usando o `Validation` auxiliar (lado do servidor somente)
- [Atualizada a validação no Web Pages v2, parte 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) adicionando uma validação do lado do cliente.
- [Atualizada a validação no Web Pages v2, parte 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) erros de validação de formatação.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>Registrar Scripts usando o Gerenciador de ativos

O Gerenciador de ativos é um novo recurso que você pode usar no código do servidor para registrar e renderizar os scripts do cliente. Esse recurso é útil quando você estiver trabalhando com código de vários arquivos (como páginas de layout, páginas de conteúdo, os auxiliares, etc.) que são combinadas em uma única página em tempo de execução. O Gerenciador de ativos coordena os arquivos de origem para certificar-se de que os arquivos de script são referenciados corretamente e com eficiência na página renderizada, independentemente de quais arquivos de código, eles são chamados de ou quantas vezes eles são chamados. O Gerenciador de ativos também renderiza `<script>` marcas no lugar certo para que a página pode carregar rapidamente (sem fazer o download de scripts durante o processamento) e evitar erros que podem ocorrer se os scripts são chamados antes da renderização for concluída.

Por exemplo, suponha que você crie um auxiliar personalizado que chama um arquivo JavaScript e chamar esse auxiliar em três locais diferentes em seu código de página de conteúdo. Se você não usar o Gerenciador de ativos para registrar o script chama na auxiliares, três diferentes `<script>` marcas que apontam para o mesmo arquivo de script aparecerão na sua página renderizada. Além disso, dependendo de onde o `<script>` marcas são inseridas na página renderizada, poderão ocorrer erros se o script tenta acessar determinados elementos de página antes da página é totalmente carregada. Se você usar o Gerenciador de ativos para registrar o script, você pode evitar esses problemas.

Você pode registrar um script com o Gerenciador de ativos ao fazer isso:

- O código que precisa para referenciar o script, chame o `Assets.AddScript` método.
- Em um  *\_SiteLayout* página, chame o `Assets.GetScripts` método para renderizar o `<script>` marcas. 

    > [!NOTE]
    > Coloque chamadas para `Assets.GetScripts` como o último item na `<body>` elemento da  *\_SiteLayout* página. Isso ajuda a página carregada mais rapidamente e pode ajudar a evitar erros de script.

O exemplo a seguir mostra como funciona o Gerenciador de ativos. O código contém os seguintes itens:

- Um auxiliar personalizado chamado `MakeNote`. Esse auxiliar renderiza uma cadeia de caracteres dentro de uma caixa, encapsulando uma `div` elemento em torno dele que tem o estilo com uma borda e adicionando &quot;Observação:&quot; a ele. O auxiliar também chama um arquivo JavaScript que adiciona o comportamento de tempo de execução para a anotação. Em vez de referenciar o script com um `<script>` marca, o auxiliar registra o script chamando `Assets.AddScript` .
- Um arquivo JavaScript. Esse é o arquivo que é chamado pelo auxiliar e temporariamente aumenta o tamanho da fonte de itens de anotação durante um `mouseover` eventos.
- Uma página de conteúdo, que faz referência a<em>\_SiteLayout</em> renderiza algum conteúdo no corpo da página e, em seguida, chama o `MakeNote` auxiliar.
- Um  *\_SiteLayout* página. Esta página fornece um cabeçalho comum e uma estrutura de layout de página. Ele também inclui uma chamada para `Assets.GetScripts`, que é como o Gerenciador de ativos renderiza o script chama em uma página.

Para executar a amostra:

1. Crie um site Web Pages 2 vazio. Você pode usar o WebMatrix **Site vazio** modelo para isso.
2. Crie uma pasta chamada *Scripts* no site.
3. No *Scripts* pasta, crie um arquivo chamado *Test. js*, copie o *Test. js* do exemplo de conteúdo nele e salve o arquivo...
4. Crie uma pasta chamada *App\_código* no site.
5. No *App\_código* pasta, crie um arquivo chamado *Helpers.cshtml*, copie o código de exemplo nele e salve-o em uma pasta chamada *aplicativo\_código*na pasta raiz.
6. Na pasta da raiz do site, crie um arquivo chamado  *\_SiteLayout.cshtml,* copie o exemplo nele e salve o arquivo.
7. Na raiz do site, crie um arquivo chamado *ContentPage.cshtml*, adicione o código de exemplo e salvá-lo.
8. Execute *ContentPage* em um navegador. A cadeia de caracteres passada para o `MakeNote` auxiliar é renderizado como uma observação demarcada.
9. Passe o ponteiro do mouse sobre a anotação. O script temporariamente aumenta o tamanho da fonte da anotação.
10. Exiba a origem da página renderizada. Por causa de onde você colocou a chamada para `Assets.GetScripts`, o renderizado `<script>` marca chama *Test. js* é o último item no corpo da página.

*Test.js*

[!code-javascript[Main](top-features-in-web-pages-2/samples/sample8.js)]

*Helpers.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample9.cshtml)]

*\_SiteLayout.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample10.html)]

*ContentPage.cshtml*

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample11.cshtml)]

A captura de tela a seguir mostra *ContentPage.cshtml* em um navegador quando você mantém o ponteiro do mouse sobre a Observação:

[![topSeven-resmgr-1](top-features-in-web-pages-2/_static/image14.png)](top-features-in-web-pages-2/_static/image13.png)

<a id="oauthsetup"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Habilitar os logons do Facebook e outros Sites usando OAuth e OpenID

2 páginas da Web oferecem opções aprimoradas de associação e autenticação. O principal aprimoramento é que há novos [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) provedores. Usando esses provedores, você pode permitir que os usuários façam logon em seu site usando as credenciais existentes do Facebook, Twitter, Windows Live, Google e Yahoo. Por exemplo, para fazer logon usando uma conta do Facebook, os usuários podem escolher apenas um ícone de Facebook, que redireciona para a página de logon do Facebook em que eles inserirem suas informações de usuário. Em seguida, podem associar o logon do Facebook com sua conta em seu site. Um aprimoramento relacionado aos recursos de associação de páginas da Web é que os usuários podem associar vários logons (incluindo logons de sites de rede social) com uma única conta em seu site.

Esta imagem mostra a página de logon a partir de **Starter Site** modelo, em que um usuário pode escolher um ícone do Facebook, Twitter e Live do Windows para habilitar o registro em log com uma conta externa:

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

Você pode habilitar a associação de OAuth e OpenID usando apenas algumas linhas de código. Os métodos e propriedades que você usa para trabalhar com o OAuth e OpenID provedores estão no `WebMatrix.Security.OAuthWebSecurity` classe.

No entanto, em vez de usar código para habilitar logons de outros sites, uma maneira recomendada para começar com os novos provedores é usar a nova **Starter Site** modelo que está incluído com o WebMatrix 2 Beta. O **Starter Site** modelo inclui uma infraestrutura completa de associação, completo com uma página de logon, um banco de dados de associação e todo o código necessário permitir que os usuários façam logon em seu site usando credenciais locais ou aqueles de outro site .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Como habilitar os logons usando o OAuth e provedores do OpenID

Esta seção fornece um exemplo de como permitir que os usuários a fazer logon de sites externos (Facebook, Twitter, Windows Live, Google ou Yahoo) a um site que se baseia o **Starter Site** modelo. Depois de criar um site inicial, você tanto (detalhes):

- Para os sites que usam um provedor de OAuth (Facebook, Twitter e Live do Windows), crie um aplicativo no site externo. Isso lhe dá as chaves de aplicativo que são necessários para invocar o recurso de logon para esses sites. Para sites que usam um provedor do OpenID (Google, Yahoo), você não precisa criar um aplicativo. Todos esses sites, você deve ter uma conta para fazer logon e para criar aplicativos de desenvolvedor. 

    > [!NOTE]
    > Aplicativos do Windows Live só aceitam uma URL ao vivo para um site de trabalho, portanto você não pode usar uma URL do site local para logons de teste.
- Edite alguns arquivos no seu site para especificar o provedor de autenticação apropriado e enviar um logon para o site que você deseja usar.

**Para habilitar o Google e Yahoo logons**:

1. No seu site, edite o  *\_AppStart.cshtml* página e adicione as duas linhas de código a seguir no bloco de código Razor após a chamada para o `WebSecurity.InitializeDatabaseConnection` método. Esse código permite que provedores de OpenID do Yahoo e Google. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. No *~/Account/Login.cshtml* página, remova os comentários das seguintes `<fieldset>` bloco de marcação perto do fim da página. Para remover os comentários de código, remova os `@*` caracteres que precedem e seguem o `<fieldset>` bloco. O bloco de código resultante tem esta aparência:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Adicionar um `<input>` elemento para o provedor Google ou do Yahoo a `<fieldset>` grupo de *~/Account/Login.cshtml* página. Atualizada `<fieldset>` grupo com `<input>` elementos para o Google e Yahoo parece como o exemplo a seguir: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. No *~/Account/AssociateServiceAccount.cshtml* página, adicione `<input>` elementos para o Google ou do Yahoo para o `<fieldset>` grupo perto do fim do arquivo. Você pode copiar o mesmo `<input>` elementos que você acabou de adicionar para o `<fieldset>` seção o *~/Account/Login.cshtml* página. 

    O *~/Account/AssociateServiceAccount.cshtml* página no modelo de Site inicial pode ser usada se você quiser criar uma página em que os usuários podem associar vários logons de outros sites com uma única conta em seu site.

Agora você pode testar os logons do Google e Yahoo.

1. Execute o *default. cshtml* página do seu site e escolha o **faça logon no** botão.
2. No *logon* página, o **Use outro serviço para fazer logon** , escolha qualquer um o **Google** ou **Yahoo** botão Enviar. Este exemplo usa o logon do Google. 

    A página da web redireciona a solicitação para a página de logon do Google.

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Insira as credenciais para uma conta existente do Google.
4. Se o Google pede que você queira permitir que o host local para usar as informações da conta, clique em **permitir**.

    O código usa o token do Google para autenticar o usuário e, em seguida, retorna a esta página em seu site. Esta página permite que os usuários a associar seu logon do Google com uma conta existente no seu site, ou eles podem registrar uma nova conta no seu site para associar o logon externo com.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Escolha o **associar** botão. O navegador retorna à página inicial do seu aplicativo.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Para habilitar logons Facebook**:

1. Vá para o [site de desenvolvedores do Facebook](https://developers.facebook.com/apps) (faça logon se você ainda não esteja conectado).
2. Escolha o **criar novo aplicativo** botão e, em seguida, siga os prompts para nomear e criar o novo aplicativo.
3. Na seção **selecione como o seu aplicativo se integrará ao Facebook**, escolha o **site** seção.
4. Preencha a **URL do Site** campo com a URL do seu site (por exemplo, [ `http://www.example.com` ](http://www.example.com)). O **domínio** campo é opcional; você pode usar isso para fornecer autenticação para um domínio inteiro (como *exemplo.com*). 

    > [!NOTE]
    > Se você estiver executando um site no computador local com uma URL como `http://localhost:12345` (em que o número é um número de porta local), você pode adicionar esse valor para o **URL do Site** field para testar seu site. No entanto, o número da porta de suas alterações de site local a qualquer momento, você precisará atualizar o **URL do Site** campo do seu aplicativo.
5. Escolha o **salvar alterações** botão.
6. Escolha o **aplicativos** guia novamente e, em seguida, exibir a página inicial do seu aplicativo.
7. Cópia de **ID do aplicativo** e **segredo do aplicativo** valores para seu aplicativo e colá-los em um arquivo de texto temporário. Você irá passar esses valores para o provedor do Facebook no código do site.
8. Saia do site do desenvolvedor do Facebook.

Agora você fazer alterações para duas páginas em seu site para que os usuários serão capazes de fazer logon no site usando suas contas do Facebook.

1. No seu site, edite o  *\_AppStart.cshtml* página e remova o código para o provedor de OAuth do Facebook. O bloco de código sem marca de comentário é semelhante ao seguinte: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Cópia de **ID do aplicativo** o valor do aplicativo Facebook como o valor do `consumerKey` parâmetro (entre aspas).
3. Cópia **segredo do aplicativo** o valor do aplicativo Facebook como o `consumerSecret` valor do parâmetro.
4. Salve e feche o arquivo.
5. Editar o *~/Account/Login.cshtml* da página e remover os comentários do `<fieldset>` bloco perto do fim da página. Para remover os comentários de código, remova os `@*` caracteres que precedem e seguem o `<fieldset>` bloco. O bloco de código com comentários removidos é semelhante ao seguinte: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Salve e feche o arquivo.

Agora você pode testar o logon do Facebook.

1. Executar o site *default. cshtml* da página e escolher o **Login** botão.
2. No *logon* página, o **Use outro serviço para fazer logon** , escolha o **Facebook** ícone. 

    A página da web redireciona a solicitação para a página de logon do Facebook.

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Faça logon em uma conta do Facebook. 

    O código usa o token do Facebook para autenticá-lo e, em seguida, retorna a uma página onde você pode associar seu logon do Facebook com o logon do seu site. Seu endereço de email ou nome de usuário é preenchido para o **Email** campo no formulário.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Escolha o **associar** botão. 

    O navegador retorna à home page e você está conectado.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Para habilitar logons do Twitter:** 

1. Navegue até a [site de desenvolvedores do Twitter](https://dev.twitter.com/).
2. Escolha o **criar um aplicativo** vincular e, em seguida, faça logon no site.
3. Sobre o **criar um aplicativo** formar, preencha o **nome** e **descrição** campos.
4. No **site** , insira a URL do seu site (por exemplo, [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Se você estiver testando o seu site localmente (usando uma URL como `http://localhost:12345`), Twitter pode não aceitar a URL. No entanto, você poderá usar o endereço IP de loopback local (por exemplo `http://127.0.0.1:12345`). Isso simplifica o processo de testar seu aplicativo localmente. No entanto, sempre que o número da porta do seu site local for alterada, você precisará atualizar o **site** campo do seu aplicativo.
5. No **URL de retorno de chamada** , insira uma URL para a página em seu site que você deseja que os usuários retornem ao depois de fazer logon no Twitter. Por exemplo, para enviar aos usuários para a home page do Site inicial (que reconheça seu status conectado), insira a mesma URL que você inseriu na **site** campo.
6. Aceite os termos e escolha o **criar seu aplicativo Twitter** botão.
7. Sobre o **meus aplicativos** inicial da página, escolha o aplicativo que você criou.
8. Sobre o **detalhes** guia, role para baixo e escolha o **criar meu Token de acesso** botão.
9. Sobre o **detalhes** guia, copie o **chave do consumidor** e **segredo do consumidor** valores para seu aplicativo e colá-los em um arquivo de texto temporário. Você irá passar esses valores para o provedor do Twitter no código do site.
10. Saia do site do Twitter.

Agora você alterar duas páginas em seu site para que os usuários poderão fazer logon no site usando suas contas do Twitter.

1. No seu site, edite o  *\_AppStart.cshtml* página e remova o código para o provedor de OAuth do Twitter. O bloco de código sem marca de comentário tem esta aparência: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Cópia de **chave do consumidor** o valor do aplicativo Twitter como o valor do `consumerKey` parâmetro (entre aspas).
3. Cópia de **segredo do consumidor** o valor do aplicativo Twitter como o valor do `consumerSecret` parâmetro.
4. Salve e feche o arquivo.
5. Editar o *~/Account/Login.cshtml* da página e remover os comentários do `<fieldset>` bloco perto do fim da página. Para remover os comentários de código, remova os `@*` caracteres que precedem e seguem o `<fieldset>` bloco. O bloco de código com comentários removidos é semelhante ao seguinte: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Salve e feche o arquivo.

Agora você pode testar o logon do Twitter.

1. Execute o *default. cshtml* página do seu site e escolha o **Login** botão.
2. No *logon* página, o **Use outro serviço para fazer logon** , escolha o **Twitter** ícone. 

    A página da web redireciona a solicitação para uma página de logon do Twitter para o aplicativo que você criou.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Faça logon em uma conta do Twitter.
4. O código usa o token do Twitter para autenticar o usuário e, em seguida, retorna a uma página onde você pode associar seu logon com sua conta do site. Seu nome ou endereço de email está preenchido para o **Email** campo no formulário.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Escolha o **associar** botão. 

    O navegador retorna à home page e você está conectado.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Adicionar mapas usando o auxiliar de mapas

Web Pages 2 inclui adições para o ASP.NET Web Helpers Library, que é um pacote de suplementos para um site de páginas da Web. Um deles é um componente de mapeamento fornecido pelo `Microsoft.Web.Helpers.Maps` classe. Você pode usar o `Maps` classe para gerar mapas com base em um endereço ou em um conjunto de coordenadas de longitude e latitude. O `Maps` classe permite que você chame diretamente em mecanismos de mapa populares, incluindo Bing, Google, MapQuest e Yahoo.

Para usar o novo `Maps` classe no seu site, você deve primeiro instalar a versão 2 da biblioteca auxiliares Web. Para fazer isso, vá para as instruções para instalar a versão lançada atualmente do [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) e instale a versão 2.

As etapas para adicionar o mapeamento para uma página são os mesmos, independentemente de qual dos mecanismos de mapa que você chamar. Basta adicionar uma referência de arquivo do JavaScript para sua página de mapeamento e, em seguida, adicione uma chamada que renderiza o `<script>` marcas na sua página. Em seguida, em sua página de mapeamento, chame o mecanismo de mapa que deseja usar.

O exemplo a seguir mostra como criar uma página que renderiza um mapa com base em um endereço e outra página que renderiza um mapa com base nas coordenadas de longitude e latitude. O exemplo de mapeamento de endereço usa o Google Maps e o exemplo de mapeamento de coordenadas usa o Bing Maps. Observe os seguintes elementos no código:

- A chamada para `Assets.AddScript` na parte superior das duas páginas de mapeamento. Esse método adiciona uma referência à *jquery 1.6.2.min.js* arquivo que está incluído com o **Starter Site** modelo e que não é necessário o `Maps` classe.
- A chamada para o `Assets.GetScripts` método no arquivo de layout. Este método renderiza a `<script>` de marca em duas páginas de mapeamento.
- A chamada para o `@Maps.GetGoogleHtml` e o `@Maps.GetBingHtml` métodos nas páginas de mapeamento. Para mapear um endereço, você deve passar uma cadeia de caracteres de endereço. Para coordenadas do mapa, você deve passar a longitude e latitude coordenadas. Para o mecanismo do Bing Maps, você também deve passar uma chave (que você recebe gratuitamente inscrevendo-se na [site de desenvolvedores do Bing Maps](https://www.microsoft.com/maps/developers/web.aspx)). Os métodos para os outros mecanismos de mapa funcionam de maneira semelhante (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Para criar páginas de mapeamento:

1. Criar um site com base nas **Starter Site** modelo.
2. Crie um arquivo chamado *MapAddress.cshtml* na raiz do site. Nesta página irá gerar um mapa com base em um endereço que você passa para ele.
3. Copie o código a seguir para o arquivo, substituindo o conteúdo existente. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Crie um arquivo chamado  *\_MapLayout.cshtml* na raiz do site. Esta página será a página de layout para as duas páginas de mapeamento.
5. Copie o código a seguir para o arquivo, substituindo o conteúdo existente. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Crie um arquivo chamado *MapCoordinates.cshtml* na raiz do site. Nesta página irá gerar um mapa com base em um conjunto de coordenadas que você passa para ele.
7. Copie o código a seguir para o arquivo, substituindo o conteúdo existente. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Para testar suas páginas de mapeamento:

1. Execute a página *MapAddress.cshtml* arquivo.
2. Insira uma cadeia de caracteres de endereço completo incluindo um endereço de rua, estado ou província e código postal e, em seguida, escolha o **mapa ele** botão. A página renderiza um mapa do Google Maps: 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Localize um conjunto de coordenadas de latitude e longitude para um local específico.
4. Execute a página *MapCoordinates.cshtml*. Insira as coordenadas e, em seguida, escolha o **mapa ele** botão. A página renderiza um mapa do Bing Maps: 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Executando aplicativos de páginas da Web lado a lado

2 páginas da Web adiciona a capacidade de executar aplicativos lado a lado. Isso permite que você continue a executar seus aplicativos Web Pages 1, criar novos aplicativos Web Pages 2 e executa todas elas no mesmo computador.

Aqui estão algumas coisas para se lembrar ao instalar a versão Beta 2 de páginas da Web com o WebMatrix:

- Por padrão, os aplicativos existentes de páginas da Web serão executado como aplicativos da versão 2 em seu computador. (Os assemblies para a versão 2 são instalados no GAC e serão usados automaticamente.)
- Se você quiser executar um site usando a versão de páginas da Web 1 (em vez do padrão, como o ponto anterior), você pode configurar o site para fazer isso. Se seu site ainda não tiver um *Web. config* de arquivo na raiz do site, crie um novo e copie o XML a seguir para ele, substituindo o conteúdo existente. Se o site já contém um *Web. config* do arquivo, adicione uma `<appSettings>` elemento como a seguir para o `<configuration>` seção.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  '-Se você não especificar uma versão na *Web. config* arquivo, um site é implantado como um site da versão 2. (Os assemblies da versão 2 são copiados para o *bin* pasta no site implantado.)
- Novos aplicativos que você cria usando os modelos de site na versão Web Matrix Beta 2 incluem os assemblies da versão 2 páginas da Web do site *bin* pasta.

Em geral, você sempre pode controlar qual versão das páginas da Web para usar com seu site usando o NuGet para instalar os assemblies apropriados para o site *bin* pasta. Para encontrar pacotes, visite [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Renderização de páginas para dispositivos móveis

2 páginas da Web permite criar exibições personalizadas para processar o conteúdo em celular ou outros dispositivos.

O `System.Web.WebPages` namespace contém as seguintes classes que permitem que você trabalhe com modos de exibição: `DefaultDisplayMode`, `DisplayInfo`, e `DisplayModes`. Você pode usar essas classes diretamente e escrever um código que renderiza a saída à direita para dispositivos específicos.

Como alternativa, você pode criar páginas específicas de dispositivo usando um padrão de nomenclatura de arquivo como este: <em>FileName.</em> <em>Mobile</em><em>. cshtml</em>. Por exemplo, você pode criar duas versões de uma página, um denominado <em>MyFile.cshtml</em> e outro chamado <em>MyFile.Mobile.cshtml</em>. No tempo de execução, quando um dispositivo móvel solicita <em>MyFile.cshtml</em>, as páginas da Web renderiza o conteúdo do <em>MyFile.Mobile.cshtml</em>. Caso contrário, <em>MyFile.cshtml</em> é renderizado.

O exemplo a seguir mostra como habilitar a renderização móvel com a adição de uma página de conteúdo para dispositivos móveis. *Page1.cshtml* contém conteúdo além de uma barra lateral de navegação. *Page1.Mobile.cshtml* contém o mesmo conteúdo, mas omite a barra lateral.

Para compilar e executar o exemplo de código:

1. Em um site de páginas da Web, crie um arquivo chamado *Page1.cshtml* e copie o *Page1.cshtml* conteúdo do exemplo.
2. Crie um arquivo chamado *Page1.Mobile.cshtml* e copie o *Page1.Mobile.cshtml* conteúdo do exemplo. Observe que a versão móvel da página omite a seção de navegação para a renderização de melhor em uma tela menor.
3. Execute um navegador de desktop e navegue até *Page1.cshtml*.
4. Execute um navegador móvel (ou um emulador de dispositivo móvel) e navegue até *Page1.cshtml*. Observe que esse tempo, as páginas da Web processa a versão móvel da página. 

    > [!NOTE]
    > Para testar páginas para dispositivos móveis, você pode usar um simulador de dispositivo móvel que é executado em um computador desktop. Essa ferramenta lhe permite testar páginas da web conforme eles ficaria em dispositivos móveis (isto é, normalmente com muito menor área de exibição). Um exemplo de um simulador de é o [complemento de alternador de agente do usuário](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) para o Mozilla Firefox, que lhe permite emular vários navegadores para dispositivos móveis de uma versão da área de trabalho do Firefox.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* renderizado em um navegador da área de trabalho:

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* exibido em um modo de exibição do simulador de iPhone da Apple no navegador Firefox. Mesmo que a solicitação é para *Page1.cshtml*, a aplicativo processa *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

### <a name="aspnet-web-pages-1-resources"></a>Recursos do ASP.NET Web Pages 1

> [!NOTE]
> A maioria dos Web Pages 1 programação e recursos da API ainda se aplicam a páginas da Web 2.

- [Introdução à programação do ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>Recursos do WebMatrix

- [O WebMatrix 2 o que há de novo](http://webmatrix.com/next)
- [Site do Microsoft WebMatrix](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Iniciando o desenvolvimento da Web com o Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(inclui um aplicativo de páginas da Web de exemplo completos)
