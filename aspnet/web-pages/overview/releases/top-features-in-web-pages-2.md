---
uid: web-pages/overview/releases/top-features-in-web-pages-2
title: Os principais recursos em páginas da Web do ASP.NET 2 | Microsoft Docs
author: microsoft
description: Este tópico fornece uma visão geral dos principais novos recursos no 2 páginas da Web ASP.NET, uma estrutura de programação da web que está incluída com o WebMatr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: cc712e72-c3d0-4e43-bc2d-28cc09cd8f71
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/top-features-in-web-pages-2
msc.type: authoredcontent
ms.openlocfilehash: f0d32edd3ab54c55aa06c803cd91e01cbbb8f08a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="the-top-features-in-aspnet-web-pages-2"></a>Os principais recursos em páginas da Web do ASP.NET 2
====================
por [Microsoft](https://github.com/microsoft)

> Este artigo fornece uma visão geral dos principais novos recursos no ASP.NET páginas da Web 2 RC, uma estrutura de programação da web que está incluída no [Microsoft WebMatrix 2 RC](https://www.microsoft.com/web/).
> 
> **O que está incluído:** 
> 
> - [Instalar o WebMatrix](#install)
> - [Recursos novos e aprimorados](#New_and_Enhanced_Features)
> 
>     - [Alterações para a versão RC](#Changes_for_the_RC_Version)
>     - [Alterações da versão Beta](#Changes_for_the_Beta_Version)
>     - [Usando os modelos de Site novo e atualizado](#templates)
>     - [Validando a entrada do usuário](#validation)
>     - [Habilitar os logons do Facebook e outros sites usando OpenID e OAuth](#oauthsetup)
>     - [Adicionar mapas usando o auxiliar de mapas](#maphelper)
>     - [Executando aplicativos de páginas da Web lado a lado](#sidebyside)
>     - [Páginas de renderização para dispositivos móveis](#mobile)
> - [Recursos adicionais](#resources)
> 
> > [!NOTE]
> > Este tópico pressupõe que você estiver usando o WebMatrix para trabalhar com o código de 2 de páginas da Web do ASP.NET. No entanto, como com páginas da Web 1, você também pode criar sites 2 páginas da Web usando o Visual Studio, que lhe aprimorado de recursos do IntelliSense e depuração. Para trabalhar com páginas da Web no Visual Studio, você deve primeiro instalar o Visual Studio 2010 SP1, o Visual Web Developer Express 2010 SP1 ou o Visual Studio 11 Beta. Em seguida, instale o ASP.NET MVC 4 Beta, que inclui modelos e ferramentas para criar aplicativos ASP.NET MVC 4 e 2 páginas da Web no Visual Studio.
> 
> 
> *Última atualização: 18 de junho de 2012*


<a id="install"></a>
## <a name="installing-webmatrix"></a>Instalar o WebMatrix

Para instalar as páginas da Web, você pode usar o Microsoft Web Platform Installer, que é um aplicativo gratuito que torna mais fácil de instalar e configurar as tecnologias relacionadas à web. Instale a versão Beta 2 do WebMatrix, que inclui a versão Beta 2 de páginas da Web.

1. Navegue até a página de instalação para a versão mais recente do Web Platform Installer:

    [https://go.microsoft.com/fwlink/?LinkId=226883](https://go.microsoft.com/fwlink/?LinkId=226883)

    > [!NOTE]
    > Se você já tiver o WebMatrix 1 instalado, a instalação atualiza para o WebMatrix 2 Beta. Você pode executar sites que foram criados usando a versão 1 ou 2 no mesmo computador. Para obter mais informações, consulte a seção sobre [aplicativos de páginas da Web em execução lado a lado](#sidebyside).
2. Escolha **instalar agora**. 

    Se você usar o Internet Explorer, vá para a próxima etapa. Se você usar um navegador diferente como o Google Chrome ou o Mozilla Firefox, você será solicitado para salvar o *Webmatrix.exe* para o computador. Salve o arquivo e, em seguida, clique em para iniciar o instalador.
3. Execute o instalador e escolha o **instalar** botão. Isso instala o WebMatrix e páginas da Web.

## <a id="New_and_Enhanced_Features"></a>  Recursos novos e aprimorados

### <a id="Changes_for_the_RC_Version"></a>  Alterações para a versão RC (junho de 2012)

A versão RC em junho de 2012 tem algumas alterações da atualização de versão Beta que foi lançado em março de 2012. Essas alterações são:

- Um `Validation.AddFormError` método foi adicionado para o `Validation` auxiliar. Isso é útil se você executar a validação manualmente (por exemplo, você validar um valor que é passado na cadeia de caracteres de consulta) e você deseja adicionar uma mensagem de erro pode ser exibida, o `Html.ValidationSummary` método. Para obter mais informações, consulte a seção [validação de dados que não vêm diretamente de usuários](https://go.microsoft.com/fwlink/?LinkId=253002#Validating_Data_That_Doesnt_Come_Directly_from_Users) na [Validando a entrada do usuário em páginas da Web do ASP.NET (Razor) Sites](https://go.microsoft.com/fwlink/?LinkId=253002).
- A funcionalidade de empacotamento e minimização foi removida dos assemblies de núcleo 2 de páginas da Web do ASP.NET. Como consequência, o `Assets` auxiliar listado neste documento não está disponível. Em vez disso, você deve instalar o [otimização ASP.NET](http://nuget.org/packages/Microsoft.Web.Optimization/0.1) pacote NuGet. Para obter mais informações, consulte [reagrupamento e minimizando os ativos em um Site de páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=255373).
- Assemblies adicionais para dar suporte a páginas da Web de ASP.NET 2 foram adicionados. O efeito notável somente dessa alteração é que você pode ver mais assemblies em um site *bin* pasta depois de criar um site ou implantar um site em um provedor de hospedagem.

<a id="Changes_for_the_Beta_Version"></a>
### <a name="changes-for-the-beta-version-february-2012"></a>Alterações para a versão Beta (fevereiro de 2012)

A versão Beta lançada em fevereiro de 2012 tem apenas algumas alterações da versão Beta que foi lançado em dezembro de 2011. Essas alterações são:

- Razor agora dá suporte a atributos condicionais. Em um HTML elemento, se você definir um atributo para um valor que é resolvido no código do servidor para `false` ou `null`, o ASP.NET não processa o atributo em todos os. Por exemplo, imagine que você tem a seguinte marcação para uma caixa de seleção:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample1.html)]

    Se o valor de `checked1` resolve `false` ou `null`, o `checked` atributo não será renderizado. Isso é uma alteração significativa.
- O `Validation.GetHtml` método foi renomeado para `Validation.For`. Isso é uma alteração significativa; `Validation.GetHtml` não funcionará na versão Beta.
- Você pode incluir o `~` operador na marcação para referenciar a raiz do site sem usar o `Href` função. (Ou seja, o analisador do Razor agora pode localizar e resolver o `~` operador sem a necessidade de uma chamada de método explícito para `Href`.) O `Href` método ainda funciona, portanto, esta não é uma alteração significativa.

    Por exemplo, se você já tinha marcação da seguinte forma:

    `<a href="@Href("~/Default.cshtml")">Home</a>`

    Agora você pode usar a marcação da seguinte forma:

    `<a href="~/Default.cshtml">Home</a>`
- O `Scripts` auxiliar para gerenciamento de ativos (recurso) foi substituído com o `Assets` auxiliar, que tem métodos ligeiramente diferentes, como o seguinte:

  - Para `Scripts.Add`, use `Assets.AddScript`
  - Para `Scripts.GetScriptTags`, use `Assets.GetScripts`

    Isso é uma alteração significativa; o `Scripts` classe não está disponível na versão Beta. Os exemplos de código neste documento que usam o gerenciamento de ativos foram atualizados com essa alteração.

<a id="templates"></a>
### <a name="using-the-new-and-updated-site-templates"></a>Usando os modelos de Site novo e atualizado

O **Site inicial** modelo tiver sido atualizado para que ele seja executado em 2 de páginas da Web por padrão. Ela também inclui os seguintes novos recursos:

- Renderização de página de dispositivos móveis. Com o uso de estilos CSS e o `@media` seletor, o **Site inicial** fornece renderização aprimorada de páginas em telas menores, incluindo telas de dispositivo móvel.
- Opções de autenticação e associação aprimoradas. Você pode permitir que os usuários façam logon no seu site usando suas contas de outros sites de rede social, como Windows Live, Facebook e Twitter. Para obter mais informações, consulte o [habilitar logons do Facebook e outros Sites usando OpenID e OAuth](#oauthsetup) seção.
- Elementos do HTML5.

O novo **Site Pessoal** modelo permite que você crie um site que contém um blog pessoal, uma página de fotos e uma página do Twitter. Você pode personalizar um site com base no **Site Pessoal** modelo fazendo o seguinte:

- Alterar a aparência do site, editando o arquivo de layout (*\_SiteLayout.cshtml*) e o arquivo de estilos (*Site.css*).
- Instale os pacotes do NuGet que adicionam funcionalidade ao seu site. Para obter informações sobre como instalar pacotes, incluindo o ASP.NET Web Helpers Library, consulte o tutorial [instalando auxiliares](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers).

Para acessar o **Site Pessoal** modelo, escolha **modelos** sobre o WebMatrix **início rápido** tela.

[![topseven-personalsite-1](top-features-in-web-pages-2/_static/image2.png)](top-features-in-web-pages-2/_static/image1.png)

No **modelos** caixa de diálogo caixa, escolha o **Site Pessoal** modelo.

[![topseven-personalsite-2](top-features-in-web-pages-2/_static/image4.png)](top-features-in-web-pages-2/_static/image3.png)

A página de aterrissagem do **Site Pessoal** modelo permite que você siga os links para configurar seu blog Twitter página e página de fotos.

[![topseven-personalsite-3](top-features-in-web-pages-2/_static/image6.png)](top-features-in-web-pages-2/_static/image5.png)

<a id="validation"></a>
### <a name="validating-user-input"></a>Validando a entrada do usuário

Em páginas da Web 1, para validar a entrada do usuário em formulários enviados, use o `System.Web.WebPages.Html.ModelState` classe. (Isso é ilustrado em vários exemplos de código no tutorial 1 de páginas da Web denominado [trabalhando com dados](../data/5-working-with-data.md).) Você ainda pode usar essa abordagem 2 páginas da Web. No entanto, 2 páginas da Web também oferece ferramentas aprimoradas para validar a entrada do usuário:

- Novas classes de validação, incluindo `System.Web.WebPages.ValidationHelper` e `System.Web.WebPages.Validator`, que permitem realizar tarefas de validação avançados com algumas linhas de código.
- Opcionalmente, validação do lado do cliente, que fornece comentários imediatos para o usuário em vez de exigir uma viagem para o servidor para verificar erros de validação. (Por motivos de segurança, a validação é executada no servidor mesmo se as verificações tem sido executadas no cliente com antecedência.)

Para usar os novos recursos de validação, faça o seguinte:

No código da página, registrar um elemento a ser validado usando métodos do `Validation` auxiliar: `Validation.RequireField`, `Validation.RequireFields` (para registrar vários elementos obrigatória), ou `Validation.Add`. O `Add` método permite que você especifique outros tipos de verificações de validação, como verificação, comparando as entradas nos campos diferentes, verificações de comprimento de cadeia de caracteres de tipo de dados e padrões (usando expressões regulares). Estes são alguns exemplos:

[!code-html[Main](top-features-in-web-pages-2/samples/sample2.html)]

Para exibir um erro específico do campo, chame `Html.ValidationMessage` na marcação para cada elemento que está sendo validada:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample3.cshtml)]

Para exibir um resumo (`<ul>` lista) de todos os erros na página, `Html.ValidationSummary` na marcação:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample4.cshtml)]

Essas etapas são suficientes para implementar a validação do lado do servidor. Se você quiser adicionar validação do lado do cliente, além disso, faça o seguinte.

Adicione as seguintes referências de arquivo de script dentro do `<head>` seção de uma página da web. As primeiras duas referências de script apontam para arquivos remotos em um servidor de entrega de conteúdo (CDN). A terceira referência aponta para um arquivo de script de local. Aplicativos de produção devem implementar um fallback quando o CDN não estiver disponível. Teste o fallback.

[!code-html[Main](top-features-in-web-pages-2/samples/sample5.html)]

A maneira mais fácil de obter uma cópia local do *jquery.validate.unobtrusive.min.js* biblioteca é criar um novo site de páginas da Web com base em um dos modelos de site (por exemplo, o Site inicial). O site criado pelo modelo inclui *jquery.validate.unobtrusive.js* arquivo em sua pasta de Scripts, do qual você pode copiá-lo para seu site.

Se seu site usa um<em>\_SiteLayout</em> página para controlar o layout de página, você pode incluir essas referências de script na página para que a validação está disponível para todas as páginas de conteúdo. Se você deseja executar a validação apenas em páginas em particular, você pode usar o Gerenciador de ativos para registrar os scripts apenas nessas páginas. Para fazer isso, chame `Assets.AddScript(path)` na página que você deseja validar e fazer referência a cada um dos arquivos de script. Em seguida, adicione uma chamada para `Assets.GetScripts` no  <em>\_SiteLayout</em> página para renderizar registrado `<script>` marcas. Para obter mais informações, consulte a seção [registrar Scripts com o Gerenciador de ativos](#resmanagement).

A marcação para um elemento individual, chamar o `Validation.For` método. Esse método emite atributos que jQuery pode conectar-se para fornecer a validação do lado do cliente. Por exemplo:

[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample6.cshtml)]

O exemplo a seguir mostra uma página que valida a entrada do usuário em um formulário. Para executar e testar esse código de validação, faça o seguinte:

1. Criar um novo site usando um dos modelos de site do WebMatrix 2 inclui um *Scripts* pasta, como o **Site inicial** modelo.
2. No novo site, crie um novo *. cshtml* página e substitua o conteúdo da página com o código a seguir.
3. Execute a página em um navegador. Insira valores válidos e inválidos para ver os efeitos sobre a validação. Por exemplo, deixar um campo obrigatório em branco ou digitar uma letra no **créditos** campo.


[!code-cshtml[Main](top-features-in-web-pages-2/samples/sample7.cshtml)]

Aqui está a página quando um usuário envia uma entrada válida:

[![topSeven-valid-1](top-features-in-web-pages-2/_static/image8.png)](top-features-in-web-pages-2/_static/image7.png)

Aqui está a página quando um usuário envia-lo com um campo obrigatório deixado em branco:

[![topSeven-valid-2](top-features-in-web-pages-2/_static/image10.png)](top-features-in-web-pages-2/_static/image9.png)

Aqui está a página quando um usuário envia-la com algo diferente de um número inteiro no **créditos** campo:

[![topSeven-valid-3](top-features-in-web-pages-2/_static/image12.png)](top-features-in-web-pages-2/_static/image11.png)

Para obter mais informações, consulte as postagens de blog a seguir:

- [Atualizada a validação em páginas da Web v2](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2344) Noções básicas de adição de validação usando o `Validation` auxiliar (somente do servidor)
- [Atualizada a validação em páginas da Web v2, parte 2](http://www.mikepope.com/blog/DisplayBlog.aspx?permalink=2347) adicionando validação do lado do cliente.
- [Atualizada a validação em páginas da Web v2, parte 3](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2351) erros de validação de formatação.

<a id="resmanagement"></a>
### <a name="registering-scripts-using-the-assets-manager"></a>Registrar Scripts usando o Gerenciador de ativos

O Gerenciador de ativos é um novo recurso que você pode usar no código do servidor para registrar e processar os scripts de cliente. Este recurso é útil quando você estiver trabalhando com código de vários arquivos (como páginas de layout, páginas de conteúdo, auxiliares, etc.) que são combinadas em uma única página em tempo de execução. O Gerenciador de ativos coordena os arquivos de origem para certificar-se de que os arquivos de script são referenciados corretamente e com eficiência na página renderizada, independentemente de quais arquivos de código são chamados de ou quantas vezes são chamadas. O Gerenciador de ativos também processa `<script>` marcas no lugar certo para que a página pode carregar rapidamente (sem fazer o download de scripts durante o processamento) e evitar erros que podem ocorrer se os scripts são chamados antes do processamento é concluído.

Por exemplo, suponha que você crie um auxiliar personalizado que chama um arquivo JavaScript e chamar este auxiliar em três locais diferentes no seu código de página de conteúdo. Se você não usar o Gerenciador de ativos para registrar o script chama no auxiliar, três diferentes `<script>` marcas que todos os pontos no mesmo arquivo de script serão exibido na página renderizada. Além disso, dependendo de onde o `<script>` marcas são inseridas na página renderizada, poderão ocorrer erros se o script tenta acessar determinados elementos da página antes da página totalmente carregado. Se você usar o Gerenciador de ativos para registrar o script, você pode evitar esses problemas.

Você pode registrar um script com o Gerenciador de ativos com isso:

- O código que precisa referenciar o script, chame o `Assets.AddScript` método.
- Em um  *\_SiteLayout* página, chame o `Assets.GetScripts` método para renderizar o `<script>` marcas. 

    > [!NOTE]
    > Coloque chamadas para `Assets.GetScripts` como o último item no `<body>` elemento o  *\_SiteLayout* página. Isso ajuda a página carregar mais rápido e pode ajudar a evitar erros de script.

O exemplo a seguir mostra como funciona o Gerenciador de ativos. O código contém os seguintes itens:

- Um auxiliar personalizado chamado `MakeNote`. Este auxiliar renderiza uma cadeia de caracteres dentro de uma caixa encapsulando uma `div` elemento que tem o estilo com uma borda e adicionando &quot;Observação:&quot; a ele. O auxiliar também chama um arquivo JavaScript que adiciona o comportamento de tempo de execução para a anotação. Em vez de fazer referência o script com um `<script>` marca, o auxiliar registra o script chamando `Assets.AddScript` .
- Um arquivo JavaScript. Esse é o arquivo que é chamado pelo auxiliar e aumentar temporariamente o tamanho da fonte de itens de anotação durante um `mouseover` eventos.
- Uma página de conteúdo, que faz referência a<em>\_SiteLayout</em> processa algum conteúdo no corpo da página e, em seguida, chama o `MakeNote` auxiliar.
- Um  *\_SiteLayout* página. Esta página fornece um cabeçalho comuns e uma estrutura de layout de página. Ele também inclui uma chamada para `Assets.GetScripts`, que é como o Gerenciador de ativos processa o script chama em uma página.

Para executar o exemplo:

1. Crie um site vazio de 2 páginas da Web. Você pode usar o WebMatrix **Site vazio** modelo para isso.
2. Crie uma pasta chamada *Scripts* no site.
3. No *Scripts* pasta, crie um arquivo chamado *Test.js*, copiar o *Test.js* no conteúdo de exemplo e salve o arquivo.
4. Crie uma pasta chamada *aplicativo\_código* no site.
5. No *aplicativo\_código* pasta, crie um arquivo chamado *Helpers.cshtml*, copie o código de exemplo para ela e salvá-lo em uma pasta chamada *aplicativo\_código*na pasta raiz.
6. Na pasta da raiz do site, crie um arquivo chamado  *\_SiteLayout.cshtml,* copie o exemplo nele e salve o arquivo.
7. Na raiz do site, crie um arquivo chamado *ContentPage.cshtml*, adicione o código de exemplo e salvá-lo.
8. Executar *ContentPage* em um navegador. A cadeia de caracteres passada para o `MakeNote` auxiliar é renderizado como uma nota demarcada.
9. Passe o ponteiro do mouse sobre a observação. O script temporariamente aumenta o tamanho da fonte da anotação.
10. Exiba a origem da página renderizada. Por causa de onde você colocou a chamada para `Assets.GetScripts`, renderizado `<script>` marca que chama *Test.js* é o último item no corpo da página.

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
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Habilitar os logons do Facebook e outros Sites usando OpenID e OAuth

2 de páginas da Web oferece opções aprimoradas para autenticação e associação. O principal aprimoramento é que há novos [OAuth](http://oauth.net/) e [OpenID](http://openid.net/) provedores. Usando esses provedores, você pode permitir que os usuários façam logon em seu site usando suas credenciais existentes do Facebook, Twitter, Windows Live, Google e Yahoo. Por exemplo, para efetuar login usando uma conta do Facebook, os usuários podem escolher apenas um ícone do Facebook, que redireciona para a página de logon do Facebook onde digite suas informações de usuário. Em seguida, podem associar o logon do Facebook com suas contas no seu site. Um aprimoramento relacionado aos recursos de associação de páginas da Web é que os usuários podem associar vários logons (incluindo logons de sites de rede social) com uma única conta no seu site.

Esta imagem mostra a página de logon a partir de **Site inicial** modelo, onde um usuário pode escolher um ícone do Facebook, Twitter ou do Windows Live para habilitar o registro em log com uma conta externa:

[![topSeven-oauth-1](top-features-in-web-pages-2/_static/image16.png)](top-features-in-web-pages-2/_static/image15.png)

Você pode habilitar associação OpenID e OAuth usando apenas algumas linhas de código. Os métodos e propriedades que você usar para trabalhar com o OAuth e provedores de OpenID estão no `WebMatrix.Security.OAuthWebSecurity` classe.

No entanto, uma maneira recomendada para começar com os provedores de novo em vez de usar código para habilitar logons de outros sites, é usar a nova **Site inicial** modelo incluído com o WebMatrix 2 Beta. O **Site inicial** modelo inclui uma infraestrutura completa de associação, completo com uma página de logon, um banco de dados de associação e todo o código necessário permitir que os usuários façam logon em seu site usando credenciais locais ou outro site .

#### <a name="how-to-enable-logins-using-the-oauth-and-openid-providers"></a>Como habilitar logons usando o OAuth e provedores de OpenID

Esta seção fornece um exemplo de como permitir aos usuários fazer logon em sites externos (Facebook, Twitter, Windows Live, Google e Yahoo) a um site que se baseia o **Site inicial** modelo. Depois de criar um site inicial, você tanto (detalhes):

- Para os sites que usam um provedor OAuth (Facebook, Twitter e do Windows Live), crie um aplicativo no site externo. Isso fornece chaves do aplicativo que você vai precisar para invocar o recurso de logon para esses sites. Para sites que usam um provedor OpenID (Google, Yahoo), você não precisa criar um aplicativo. Para todos os sites, você deve ter uma conta para efetuar login e criar aplicativos de desenvolvedor. 

    > [!NOTE]
    > Aplicativos do Windows Live só aceitam uma URL ao vivo para um site de trabalho, portanto você não pode usar uma URL do site local para logons de teste.
- Edite alguns arquivos em seu site para especificar o provedor de autenticação apropriado e enviar um logon para o site que você deseja usar.

**Para habilitar logons Google e Yahoo**:

1. Em seu site, edite o  *\_AppStart.cshtml* página e adicione as duas linhas de código a seguir no bloco de código Razor após a chamada para o `WebSecurity.InitializeDatabaseConnection` método. Esse código permite que os provedores de OpenID do Yahoo e o Google. 

    [!code-css[Main](top-features-in-web-pages-2/samples/sample12.css)]
2. No *~/Account/Login.cshtml* página, remova os comentários a seguir `<fieldset>` bloco de marcação no final da página. Para remover os comentários de código, remova o `@*` caracteres que precedem e seguem o `<fieldset>` bloco. O bloco de código resultante tem esta aparência:

    [!code-html[Main](top-features-in-web-pages-2/samples/sample13.html)]
3. Adicionar uma `<input>` elemento para o provedor Google ou do Yahoo o `<fieldset>` grupo o *~/Account/Login.cshtml* página. A atualização `<fieldset>` grupo `<input>` elementos para o Google e Yahoo parece como o exemplo a seguir: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample14.html)]
4. No *~/Account/AssociateServiceAccount.cshtml* página, adicione `<input>` elementos para o Google ou do Yahoo para o `<fieldset>` grupo próximo ao final do arquivo. Você pode copiar o mesmo `<input>` elementos que você acabou de adicionar para o `<fieldset>` seção o *~/Account/Login.cshtml* página. 

    O *~/Account/AssociateServiceAccount.cshtml* página no modelo de Site inicial pode ser usada se você deseja criar uma página na qual os usuários podem associar vários logons de outros sites com uma única conta no seu site.

Agora você pode testar logons Google e Yahoo.

1. Execute o *cshtml* página do seu site e escolha o **login** botão.
2. No *logon* página, o **utilize outro serviço para fazer logon no** seção, escolha o o **Google** ou **Yahoo** enviar botão. Este exemplo usa o logon do Google. 

    A página da web redireciona a solicitação para a página de logon do Google.

    [![topSeven-oauth-6](top-features-in-web-pages-2/_static/image18.png)](top-features-in-web-pages-2/_static/image17.png)
3. Insira as credenciais para uma conta existente do Google.
4. Se o Google pergunta se você deseja permitir que o host local para usar as informações da conta, clique em **permitir**.

    O código usa o token do Google para autenticar o usuário e, em seguida, retorna a esta página no seu site. Esta página permite que os usuários associar seu logon do Google com uma conta existente no seu site, ou eles podem registrar uma nova conta no seu site para associar o logon externo com.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image20.png)](top-features-in-web-pages-2/_static/image19.png)
5. Escolha o **associar** botão. O navegador retorna à página inicial do aplicativo.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image22.png)](top-features-in-web-pages-2/_static/image21.png)

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image24.png)](top-features-in-web-pages-2/_static/image23.png)

**Para habilitar logons Facebook**:

1. Vá para o [site de desenvolvedores do Facebook](https://developers.facebook.com/apps) (entrar se você ainda não estiver conectado).
2. Escolha o **criar novo aplicativo** botão e, em seguida, siga os prompts para nomear e criar o novo aplicativo.
3. Na seção **selecione como o aplicativo integra-se ao Facebook**, escolha o **site** seção.
4. Preencha o **URL do Site** campo com a URL do seu site (por exemplo, [ `http://www.example.com` ](http://www.example.com)). O **domínio** campo é opcional; você pode usar isso para fornecer autenticação para um domínio inteiro (como *example.com*). 

    > [!NOTE]
    > Se você estiver executando um site no computador local com uma URL como `http://localhost:12345` (onde o número é um número de porta local), você pode adicionar esse valor para o **URL do Site** campo testando o seu site. No entanto, sempre que o número de porta de suas alterações do site local, você precisará atualizar o **URL do Site** campo do seu aplicativo.
5. Escolha o **salvar alterações** botão.
6. Escolha o **aplicativos** guia novamente e, em seguida, exibir a página inicial do seu aplicativo.
7. Copie o **ID do aplicativo** e **segredo do aplicativo** valores para o seu aplicativo e colá-los em um arquivo de texto temporário. Você passará esses valores para o provedor do Facebook no código do site.
8. Saia do site do desenvolvedor de Facebook.

Agora você fazer alterações para duas páginas em seu site para que os usuários poderão fazer logon no site usando suas contas do Facebook.

1. Em seu site, edite o  *\_AppStart.cshtml* página e descomente o código para o provedor OAuth do Facebook. O bloco de código sem marca de comentário é semelhante ao seguinte: 

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample15.xml)]
2. Copiar o **ID do aplicativo** valor do aplicativo Facebook como o valor da `consumerKey` parâmetro (entre aspas).
3. Cópia **segredo do aplicativo** valor do aplicativo Facebook como o `consumerSecret` o valor do parâmetro.
4. Salve e feche o arquivo.
5. Editar o *~/Account/Login.cshtml* página e remova os comentários do `<fieldset>` bloco próximo ao final da página. Para remover os comentários de código, remova o `@*` caracteres que precedem e seguem o `<fieldset>` bloco. O bloco de código com comentários removido é semelhante ao seguinte: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample16.html)]
6. Salve e feche o arquivo.

Agora você pode testar o logon do Facebook.

1. Executar o site *cshtml* página e selecione o **Login** botão.
2. No *logon* página, o **utilize outro serviço para fazer logon no** , escolha o **Facebook** ícone. 

    A página da web redireciona a solicitação para a página de logon do Facebook.

    [![topSeven-oauth-2](top-features-in-web-pages-2/_static/image26.png)](top-features-in-web-pages-2/_static/image25.png)
3. Faça logon em uma conta do Facebook. 

    O código usa o token do Facebook para autenticação e, em seguida, retorna a uma página onde você pode associar o logon do Facebook com o logon do seu site. Seu endereço de email ou nome de usuário é preenchido para o **Email** campo no formulário.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image28.png)](top-features-in-web-pages-2/_static/image27.png)
4. Escolha o **associar** botão. 

    Retorna o navegador para a home page e você está conectado.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image30.png)](top-features-in-web-pages-2/_static/image29.png)

**Para habilitar logons Twitter:** 

1. Navegue até o [site de desenvolvedores do Twitter](https://dev.twitter.com/).
2. Escolha o **criar um aplicativo** link e, em seguida, faça logon no site.
3. Sobre o **criar um aplicativo** formulário, preencha o **nome** e **descrição** campos.
4. No **site** campo, insira a URL do seu site (por exemplo, [ `http://www.example.com` ](http://www.example.com)). 

    > [!NOTE]
    > Se você estiver testando o seu site localmente (usando uma URL como `http://localhost:12345`), Twitter pode não aceitar a URL. No entanto, você poderá usar o endereço IP de loopback local (por exemplo `http://127.0.0.1:12345`). Isso simplifica o processo de testar seu aplicativo localmente. No entanto, sempre que altera o número da porta do site local, você precisará atualizar o **site** campo do seu aplicativo.
5. No **URL de retorno de chamada** campo, digite uma URL para a página no seu site que você deseja que os usuários para retornar ao depois de fazer logon no Twitter. Por exemplo, para enviar aos usuários para a home page do Site inicial (que reconheça seu status conectado), digite a mesma URL que você inseriu no **site** campo.
6. Aceite os termos e escolha o **criar seu aplicativo Twitter** botão.
7. Sobre o **meus aplicativos** inicial da página, escolha o aplicativo que você criou.
8. Sobre o **detalhes** guia, role até a parte inferior e escolha o **criar meu Token de acesso** botão.
9. No **detalhes** guia, copie o **chave do consumidor** e **segredo do consumidor** valores para o seu aplicativo e colá-los em um arquivo de texto temporário. Você irá passar esses valores para o provedor do Twitter em seu código de site.
10. Saia do site do Twitter.

Agora você fazer alterações para duas páginas em seu site para que os usuários poderão fazer logon no site usando suas contas do Twitter.

1. Em seu site, edite o  *\_AppStart.cshtml* página e descomente o código para o provedor OAuth do Twitter. O bloco de código sem marca de comentário tem esta aparência: 

    [!code-csharp[Main](top-features-in-web-pages-2/samples/sample17.cs)]
2. Copiar o **chave do consumidor** valor do aplicativo como o valor do Twitter o `consumerKey` parâmetro (entre aspas).
3. Copiar o **segredo do consumidor** valor do aplicativo como o valor do Twitter o `consumerSecret` parâmetro.
4. Salve e feche o arquivo.
5. Editar o *~/Account/Login.cshtml* página e remova os comentários do `<fieldset>` bloco próximo ao final da página. Para remover os comentários de código, remova o `@*` caracteres que precedem e seguem o `<fieldset>` bloco. O bloco de código com comentários removido é semelhante ao seguinte: 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample18.html)]
6. Salve e feche o arquivo.

Agora você pode testar o logon do Twitter.

1. Execute o *cshtml* página do seu site e escolha o **Login** botão.
2. No *logon* página, o **utilize outro serviço para fazer logon no** , escolha o **Twitter** ícone. 

    A página da web redireciona a solicitação para uma página de logon do Twitter para o aplicativo que você criou.

    [![topSeven-oauth-4](top-features-in-web-pages-2/_static/image32.png)](top-features-in-web-pages-2/_static/image31.png)
3. Faça logon em uma conta do Twitter.
4. O código usa o token do Twitter para autenticar o usuário e, em seguida, retorna a uma página onde você pode associar seu logon com sua conta do site. O nome ou endereço de email é preenchido para o **Email** campo no formulário.

    [![topSeven-oauth-5](top-features-in-web-pages-2/_static/image34.png)](top-features-in-web-pages-2/_static/image33.png)
5. Escolha o **associar** botão. 

    Retorna o navegador para a home page e você está conectado.

    [![topSeven-oauth-3](top-features-in-web-pages-2/_static/image36.png)](top-features-in-web-pages-2/_static/image35.png)

<a id="maphelper"></a>
### <a name="adding-maps-using-the-maps-helper"></a>Adicionar mapas usando o auxiliar de mapas

2 de páginas da Web inclui adições para o ASP.NET Web Helpers Library, que é um pacote de suplementos para um site de páginas da Web. Um deles é um componente de mapeamento fornecido pelo `Microsoft.Web.Helpers.Maps` classe. Você pode usar o `Maps` classe para gerar mapas com base em um endereço ou em um conjunto de coordenadas de longitude e latitude. O `Maps` classe permite que você chamar diretamente em mecanismos de mapa populares, incluindo Yahoo, Google, MapQuest e Bing.

Para usar o novo `Maps` classe em seu site, você deve primeiro instalar a versão 2 da biblioteca de auxiliares de Web. Para fazer isso, vá para as instruções para instalar a versão lançada do [ASP.NET Web Helpers Library](https://go.microsoft.com/fwlink/?LinkId=202889#webhelpers) e instale a versão 2.

As etapas para adicionar o mapeamento para uma página são os mesmos, independentemente de qual dos mecanismos de mapa que você chamar. Basta adicionar uma referência de arquivo do JavaScript para a página de mapeamento e, em seguida, adicione uma chamada que renderiza o `<script>` marcas na sua página. Na página de mapeamento, chame o mecanismo de mapa que você deseja usar.

O exemplo a seguir mostra como criar uma página que renderiza um mapa com base em um endereço e outra página que renderiza um mapa com base em coordenadas de longitude e latitude. O exemplo de mapeamento de endereço usa o Google Maps, e o exemplo de mapeamento de coordenadas usa o Bing Maps. Observe os seguintes elementos no código:

- A chamada para `Assets.AddScript` na parte superior das duas páginas de mapeamento. Este método adiciona uma referência para o *jquery 1.6.2.min.js* arquivo que é incluído com o **Site inicial** modelo e que é exigido pelo `Maps` classe.
- A chamada para o `Assets.GetScripts` método no arquivo de layout. Este método renderiza a `<script>` marca em duas páginas de mapeamento.
- A chamada para o `@Maps.GetGoogleHtml` e `@Maps.GetBingHtml` métodos nas páginas de mapeamento. Para mapear um endereço, você deve passar uma cadeia de caracteres do endereço. Para coordenadas do mapa, você deve passar longitude e latitude coordenadas. Para o mecanismo do Bing Maps, você também deve transmitir uma chave (que você obtém gratuitamente inscrevendo-se no [site de desenvolvedores do Bing Maps](https://www.microsoft.com/maps/developers/web.aspx)). Os métodos para os outros mecanismos de mapa funcionam de maneira semelhante (`@Maps.GetYahooHtml`, `@Maps.GetMapQuestHtml`).

Para criar páginas de mapeamento:

1. Criar um site baseado no **Site inicial** modelo.
2. Crie um arquivo chamado *MapAddress.cshtml* na raiz do site. Esta página irá gerar um mapa com base em um endereço que você passa para ele.
3. Copie o código a seguir para o arquivo, substituindo o conteúdo existente. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample19.cshtml)]
4. Crie um arquivo chamado  *\_MapLayout.cshtml* na raiz do site. Essa página será a página de layout para as duas páginas de mapeamento.
5. Copie o código a seguir para o arquivo, substituindo o conteúdo existente. 

    [!code-html[Main](top-features-in-web-pages-2/samples/sample20.html)]
6. Crie um arquivo chamado *MapCoordinates.cshtml* na raiz do site. Esta página irá gerar um mapa com base em um conjunto de coordenadas que você passa para ele.
7. Copie o código a seguir para o arquivo, substituindo o conteúdo existente. 

    [!code-cshtml[Main](top-features-in-web-pages-2/samples/sample21.cshtml)]

Para testar suas páginas de mapeamento:

1. Execute a página *MapAddress.cshtml* arquivo.
2. Insira uma cadeia de caracteres de endereço completo incluindo um endereço de rua, estado ou província e CEP e, em seguida, escolha o **mapa-** botão. A página renderiza um mapa do Google Maps: 

    [![topseven-maphelper-1](top-features-in-web-pages-2/_static/image38.png)](top-features-in-web-pages-2/_static/image37.png)
3. Localize um conjunto de coordenadas de latitude e longitude para um local específico.
4. Execute a página *MapCoordinates.cshtml*. Insira as coordenadas e, em seguida, escolha o **mapa-** botão. A página renderiza um mapa do Bing Maps: 

    [![topseven-maphelper-2](top-features-in-web-pages-2/_static/image40.png)](top-features-in-web-pages-2/_static/image39.png)

<a id="sidebyside"></a>
### <a name="running-web-pages-applications-side-by-side"></a>Executando aplicativos de páginas da Web lado a lado

2 de páginas da Web adiciona a capacidade de executar aplicativos lado a lado. Isso lhe permite continuar a executar os aplicativos de páginas da Web 1, criar novos aplicativos de 2 páginas da Web e executar todos os no mesmo computador.

Aqui estão alguns itens a lembrar quando você instala a versão Beta 2 de páginas da Web com o WebMatrix:

- Por padrão, aplicativos de páginas da Web existentes serão executados como aplicativos versão 2 no seu computador. (Os assemblies de versão 2 são instalados no GAC e serão usados automaticamente.)
- Se você desejar executar um site usando a versão de páginas da Web 1 (em vez do padrão, como o ponto anterior), você pode configurar o site para fazer isso. Se seu site ainda não tiver um *Web. config* arquivo na raiz do site, crie um novo e copie o XML a seguir, substituindo o conteúdo existente. Se o site já contém um *Web. config* de arquivo, adicione uma `<appSettings>` elemento como a seguir para o `<configuration>` seção.

    [!code-xml[Main](top-features-in-web-pages-2/samples/sample22.xml)]
  '-Se você não especificar uma versão de *Web. config* arquivo, um site é implantado como um site de versão 2. (Os assemblies da versão 2 são copiados para o *bin* pasta no site implantado.)
- Novos aplicativos que você cria usando os modelos de site na versão Web Matrix Beta 2 incluem os assemblies da versão 2 páginas da Web do site de *bin* pasta.

Em geral, você pode controlar qual versão de páginas da Web para usar com seu site usando o NuGet para instalar os assemblies apropriados para o site sempre *bin* pasta. Para localizar pacotes, visite [NuGet.org](http://NuGet.org).

<a id="mobile"></a>
### <a name="rendering-pages-for-mobile-devices"></a>Páginas de renderização para dispositivos móveis

2 de páginas da Web permite criar exibições personalizadas para processar o conteúdo no celular ou outros dispositivos.

O `System.Web.WebPages` namespace contém as seguintes classes que permitem que você trabalhe com modos de exibição: `DefaultDisplayMode`, `DisplayInfo`, e `DisplayModes`. Você pode usar essas classes diretamente e gravar o código que processa a saída direita para dispositivos específicos.

Como alternativa, você pode criar páginas específicas de dispositivo usando um padrão de nomenclatura de arquivo como este: <em>FileName.</em> <em>Mobile</em><em>. cshtml</em>. Por exemplo, você pode criar duas versões de uma página, um denominado <em>MyFile.cshtml</em> e um chamado <em>MyFile.Mobile.cshtml</em>. No tempo de execução, quando um dispositivo móvel solicita <em>MyFile.cshtml</em>, páginas da Web processa o conteúdo de <em>MyFile.Mobile.cshtml</em>. Caso contrário, <em>MyFile.cshtml</em> é renderizado.

O exemplo a seguir mostra como habilitar renderização móvel com a adição de uma página de conteúdo para dispositivos móveis. *Page1.cshtml* contém conteúdo mais de uma barra lateral de navegação. *Page1.Mobile.cshtml* contém o mesmo conteúdo, mas omite a barra lateral.

Para compilar e executar o exemplo de código:

1. Em um site de páginas da Web, crie um arquivo chamado *Page1.cshtml* e copie o *Page1.cshtml* conteúdo do exemplo.
2. Crie um arquivo chamado *Page1.Mobile.cshtml* e copie o *Page1.Mobile.cshtml* conteúdo do exemplo. Observe que a versão móvel da página omite a seção de navegação para renderização melhor em uma tela menor.
3. Executar um navegador da área de trabalho e navegue até *Page1.cshtml*.
4. Executar um navegador móvel (ou um emulador de dispositivo móvel) e navegue até *Page1.cshtml*. Observe que esse tempo páginas da Web processa a versão móvel da página. 

    > [!NOTE]
    > Para testar as páginas de celular, você pode usar um simulador de dispositivo móvel que é executado em um computador desktop. Essa ferramenta permite que você teste páginas da web, como seria aparecem em dispositivos móveis (isto é, normalmente com muito menor Exibir área). É um exemplo de um simulador de [complemento alternador de agente do usuário](http://addons.mozilla.org/en-us/firefox/addon/user-agent-switcher/) para Mozilla Firefox, que permite que você emular vários navegadores móveis de uma versão de área de trabalho do Firefox.

*Page1.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample23.html)]

*Page1.Mobile.cshtml*

[!code-html[Main](top-features-in-web-pages-2/samples/sample24.html)]

*Page1.cshtml* renderizado em um navegador da área de trabalho:

[![topseven-displaymodes-1](top-features-in-web-pages-2/_static/image42.png)](top-features-in-web-pages-2/_static/image41.png)

*Page1.Mobile.cshtml* exibida em uma exibição de simulador de iPhone da Apple no navegador Firefox. Mesmo que a solicitação é para *Page1.cshtml*, a aplicativo processa *Page1.Mobile.cshtml*.

[![topseven-displaymodes-2](top-features-in-web-pages-2/_static/image44.png)](top-features-in-web-pages-2/_static/image43.png)

<a id="resources"></a>
## <a name="additional-resources"></a>Recursos adicionais

### <a name="aspnet-web-pages-1-resources"></a>1 recursos ASP.NET Web Pages

> [!NOTE]
> A maioria das páginas da Web 1 programação e recursos da API ainda se aplicam a páginas da Web 2.

- [Introdução à programação do ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202890)

### <a name="webmatrix-resources"></a>Recursos do WebMatrix

- [O WebMatrix 2 o que há de novo](http://webmatrix.com/next)
- [Site do Microsoft WebMatrix](https://go.microsoft.com/fwlink/?LinkID=195076)
- [Iniciando o desenvolvimento da Web com o Microsoft WebMatrix](https://msdn.microsoft.com/en-us/library/hh145669(v=VS.99).aspx)(inclui um aplicativo de páginas da Web de exemplo completos)
