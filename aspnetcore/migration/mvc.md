---
title: "Migrando do ASP.NET MVC para o núcleo do ASP.NET MVC"
author: ardalis
description: 
keywords: ASP.NET Core, MVC, migrando
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: 7a4357da4cc97d7c60cc7e309add7583ef096597
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migrando do ASP.NET MVC para o núcleo do ASP.NET MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), e [Scott Addie](https://scottaddie.com)

Este artigo mostra como começar a migração de um projeto ASP.NET MVC para [MVC do ASP.NET Core](../mvc/overview.md). O processo, ele destaca muitas coisas que foram alterados desde o ASP.NET MVC. Migrando do ASP.NET MVC é um processo de várias etapas e este artigo aborda a configuração inicial, controladores básico e modos de exibição, conteúdo estático e dependências do lado do cliente. Artigos adicionais abrangem migrando configuração e código de identidade encontrada em muitos projetos do ASP.NET MVC.

> [!NOTE]
> Os números de versão nos exemplos podem não ser atuais. Talvez seja necessário atualizar seus projetos adequadamente.

## <a name="create-the-starter-aspnet-mvc-project"></a>Criar o projeto ASP.NET MVC starter

Para demonstrar a atualização, vamos começar criando um aplicativo ASP.NET MVC. Criá-lo com o nome *WebApp1* para o namespace corresponderá o projeto do ASP.NET Core que criamos na próxima etapa.

![Caixa de diálogo do Visual Studio novo projeto](mvc/_static/new-project.png)

![Caixa de diálogo nova aplicativo Web: modelo de projeto MVC selecionado no painel de modelos do ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Opcional:* alterar o nome da solução de *WebApp1* para *Mvc5*. O Visual Studio exibirá o nome da nova solução (*Mvc5*), tornando mais fácil informar esse projeto da próximo projeto.

## <a name="create-the-aspnet-core-project"></a>Criar o projeto do ASP.NET Core

Criar um novo *vazio* aplicativo web do ASP.NET Core com o mesmo nome que o projeto anterior (*WebApp1*) para corresponder os namespaces em dois projetos. Ter o mesmo namespace torna mais fácil copiar código entre os dois projetos. Você precisará criar este projeto em um diretório diferente do projeto anterior para usar o mesmo nome.

![Caixa de diálogo Novo Projeto](mvc/_static/new_core.png)

![Caixa de diálogo nova aplicativo Web do ASP.NET: modelo de projeto vazio selecionado no painel de modelos do ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Opcional:* criar um aplicativo ASP.NET Core novo usando o *aplicativo Web* modelo de projeto. Nomeie o projeto *WebApp1*e selecione uma opção de autenticação de **contas de usuário individuais**. Renomear este aplicativo para *FullAspNetCore*. Criar este projeto economizará tempo na conversão. Você pode examinar o código gerado pelo modelo para ver o resultado final ou copiar o código para o projeto de conversão. Também é útil quando você tiver problemas em uma etapa de conversão para comparar com o projeto de modelo gerado.

## <a name="configure-the-site-to-use-mvc"></a>Configurar o site para usar o MVC

* Instalar o `Microsoft.AspNetCore.Mvc` e `Microsoft.AspNetCore.StaticFiles` pacotes do NuGet.

  `Microsoft.AspNetCore.Mvc`é a estrutura MVC do ASP.NET Core. `Microsoft.AspNetCore.StaticFiles`é o manipulador de arquivo estático. O tempo de execução do ASP.NET é modular, e você deve optar explicitamente para servir arquivos estáticos (consulte [trabalhando com arquivos estáticos](../fundamentals/static-files.md)).

* Abra o *. csproj* arquivo (com o botão direito no projeto no **Solution Explorer** e selecione **Editar WebApp1.csproj**) e adicione um `PrepareForPublish` destino:

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  O `PrepareForPublish` destino é necessária para adquirir as bibliotecas de cliente via Bower. Falaremos sobre isso mais tarde.

* Abra o *Startup.cs* de arquivo e altere o código para coincidir com o seguinte:

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  O `UseStaticFiles` método de extensão adiciona o manipulador de arquivo estático. Conforme mencionado anteriormente, o tempo de execução do ASP.NET é modular, e você deve optar explicitamente para servir arquivos estáticos. O `UseMvc` método de extensão adiciona o roteamento. Para obter mais informações, consulte [inicialização do aplicativo](../fundamentals/startup.md) e [roteamento](../fundamentals/routing.md).

## <a name="add-a-controller-and-view"></a>Adicionar um controlador e o modo de exibição

Nesta seção, você adicionará um controlador mínimo e o modo de exibição para servir como espaços reservados para o controlador do ASP.NET MVC e modos de exibição que você vai migrar na próxima seção.

* Adicionar um *controladores* pasta.

* Adicionar uma **classe do controlador MVC** com o nome *HomeController* para o *controladores* pasta.

![Caixa de diálogo Adicionar Novo Item](mvc/_static/add_mvc_ctl.png)

* Adicionar um *exibições* pasta.

* Adicionar um *exibições/inicial* pasta.

* Adicionar uma *cshtml* página de exibição do MVC para o *exibições/inicial* pasta.

![Caixa de diálogo Adicionar Novo Item](mvc/_static/view.png)

A estrutura do projeto é mostrada abaixo:

![Gerenciador de soluções mostrando arquivos e pastas de WebApp1](mvc/_static/project-structure-controller-view.png)

Substitua o conteúdo do *Views/Home/Index.cshtml* arquivo com o seguinte:

```html
<h1>Hello world!</h1>
```

Execute o aplicativo.

![Aplicativo Web aberto no Microsoft Edge](mvc/_static/hello-world.png)

Consulte [controladores](../mvc/controllers/index.md) e [exibições](../mvc/views/index.md) para obter mais informações.

Agora que temos um projeto do ASP.NET Core trabalho mínimo, podemos começar a migrar a funcionalidade do projeto ASP.NET MVC. Será necessário mover o seguinte:

* conteúdo do lado do cliente (CSS, fontes e scripts)

* controladores

* modos de exibição

* modelos

* Agrupamento

* filtros

* Log de entrada/saída, de identidade (Isso será feito o seguinte tutorial.)

## <a name="controllers-and-views"></a>Controladores e exibições

* Copiar cada um dos métodos do ASP.NET MVC `HomeController` para o novo `HomeController`. Observe que, no ASP.NET MVC, tipo de método de ação retorno controlador do modelo interno é [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); no ASP.NET MVC de núcleo, os métodos de ação retorno `IActionResult` em vez disso. `ActionResult`implementa `IActionResult`, portanto, não é necessário alterar o tipo de retorno de métodos de ação.

* Copie o *About.cshtml*, *Contact.cshtml*, e *cshtml* arquivos de exibição Razor do projeto ASP.NET MVC para o projeto do ASP.NET Core.

* Executar o aplicativo do ASP.NET Core e cada método de teste. Ainda não migramos o arquivo de layout ou estilos ainda, para que os modos de exibição renderizados conterá apenas o conteúdo nos arquivos de exibição. Você não terá os links dos arquivos gerados layout para o `About` e `Contact` exibe, para que você precisará invocá-los a partir do navegador (substitua **4492** com o número da porta usado no projeto).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Página de contato](mvc/_static/contact-page.png)

Observe que a falta de estilo e itens de menu. Corrigiremos que na próxima seção.

## <a name="static-content"></a>Conteúdo estático

Em versões anteriores do ASP.NET MVC, conteúdo estático hospedado da raiz do projeto da web e foi misturado com os arquivos do servidor. No núcleo do ASP.NET, conteúdo estático é hospedado no *wwwroot* pasta. Você desejará copiar o conteúdo estático de seu aplicativo ASP.NET MVC antigo para o *wwwroot* pasta em seu projeto do ASP.NET Core. Nessa conversão de exemplo:

* Copiar o *favicon.ico* arquivo de projeto MVC antigo para o *wwwroot* pasta do projeto do ASP.NET Core.

O ASP.NET MVC antigo projeto usa [Bootstrap](http://getbootstrap.com/) para seu estilo e repositórios de arquivos a inicialização a *conteúdo* e *Scripts* pastas. O modelo, que gerou o antigo projeto ASP.NET MVC, referencia o Bootstrap no arquivo de layout (*Views/Shared/_Layout.cshtml*). Você poderá copiar o *bootstrap.js* e *bootstrap.css* projeto de arquivos do ASP.NET MVC para o *wwwroot* pasta no novo projeto, mas essa abordagem não usa o melhor mecanismo para gerenciar o cliente dependências no núcleo do ASP.NET.

No novo projeto, vamos adicionar suporte para inicialização (e outras bibliotecas de cliente) usando [Bower](https://bower.io/):

* Adicionar um [Bower](https://bower.io/) arquivo de configuração chamado *bower. JSON* para a raiz do projeto (com o botão direito no projeto e, em seguida, **Adicionar > Novo Item > arquivo de configuração Bower**). Adicionar [Bootstrap](http://getbootstrap.com/) e [jQuery](https://jquery.com/) para o arquivo (consulte as linhas destacadas abaixo).

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

Após salvar o arquivo, o Bower baixará automaticamente as dependências para o *wwwroot/lib* pasta. Você pode usar o **pesquisar Gerenciador de soluções** para localizar o caminho dos ativos:

![jQuery ativos mostrados nos resultados da pesquisa do Gerenciador de soluções](mvc/_static/search.png)

Consulte [gerenciar pacotes do lado do cliente com Bower](../client-side/bower.md) para obter mais informações.

<a name="migrate-layout-file"></a>

## <a name="migrate-the-layout-file"></a>Migrar o arquivo de layout

* Copiar o *viewstart* arquivo a partir do projeto ASP.NET MVC antigo *exibições* pasta para do projeto ASP.NET Core *exibições* pasta. O *viewstart* arquivo não foi alterado no MVC do ASP.NET Core.

* Criar um *exibições/compartilhadas* pasta.

* *Opcional:* cópia *viewimports. cshtml* do *FullAspNetCore* do projeto MVC *exibições* pasta para o projeto de ASP.NET Core *Exibições* pasta. Remover qualquer declaração de namespace no *viewimports. cshtml* arquivo. O *viewimports. cshtml* arquivo fornece namespaces para todos os arquivos de exibição e coloca [auxiliares de marcação](../mvc/views/tag-helpers/index.md). Os auxiliares de marca são usados no novo arquivo de layout. O *viewimports. cshtml* arquivo é novo para o ASP.NET Core.

* Copie o *cshtml* arquivo a partir do projeto ASP.NET MVC antigo *exibições/compartilhadas* pasta para do projeto ASP.NET Core *exibições/compartilhadas* pasta.

Abra *cshtml* de arquivo e faça as alterações a seguir (o código completo é mostrado abaixo):

   * Substituir `@Styles.Render("~/Content/css")` com um `<link>` elemento carregar *bootstrap.css* (veja abaixo).

   * Remova `@Scripts.Render("~/bundles/modernizr")`.

   * Comente o `@Html.Partial("_LoginPartial")` linha (envolvem a linha com `@*...*@`). Retornaremos a ele um tutorial futuras.

   * Substituir `@Scripts.Render("~/bundles/jquery")` com um `<script>` elemento (veja abaixo).

   * Substituir `@Scripts.Render("~/bundles/bootstrap")` com um `<script>` elemento (veja abaixo).

O link CSS de substituição:

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

As marcas de script de substituição:

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

A atualização *cshtml* arquivo é mostrado abaixo:

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

Exiba o site no navegador. Ele agora deve carregar corretamente, com os estilos esperados em vigor.

* *Opcional:* você talvez queira usar o novo arquivo de layout. Para este projeto, você pode copiar o arquivo de layout do *FullAspNetCore* projeto. O novo arquivo de layout usa [auxiliares de marcação](../mvc/views/tag-helpers/index.md) e tiver outros aprimoramentos.

## <a name="configure-bundling--minification"></a>Configurar o agrupamento de & minimização

Para obter informações sobre como configurar o empacotamento e minimização, consulte [empacotamento e minimização](../client-side/bundling-and-minification.md).

## <a name="solving-http-500-errors"></a>Resolver erros HTTP 500

Há muitos problemas que podem causar uma mensagem de erro HTTP 500 que não contêm informações sobre a origem do problema. Por exemplo, se o *Views/_ViewImports.cshtml* arquivo contém um namespace que não existe em seu projeto, você obterá um erro HTTP 500. Para obter uma mensagem de erro detalhada, adicione o seguinte código:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

Consulte **usando a página de exceção de desenvolvedor** na [tratamento de erros](../fundamentals/error-handling.md) para obter mais informações.

## <a name="additional-resources"></a>Recursos adicionais

* [Desenvolvimento no Lado do Cliente](../client-side/index.md)

* [Auxiliares de marcação](../mvc/views/tag-helpers/index.md)
