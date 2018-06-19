---
title: Migrar do ASP.NET MVC para o núcleo do ASP.NET MVC
author: ardalis
description: Saiba como começar a migração de um projeto ASP.NET MVC ao MVC do ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851021"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migrar do ASP.NET MVC para o núcleo do ASP.NET MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), e [Scott Addie](https://scottaddie.com)

Este artigo mostra como começar a migração de um projeto ASP.NET MVC para [MVC do ASP.NET Core](../mvc/overview.md). O processo, ele destaca muitas coisas que foram alterados desde o ASP.NET MVC. Migrando do ASP.NET MVC é um processo de várias etapas e este artigo aborda a configuração inicial, controladores básico e modos de exibição, conteúdo estático e dependências do lado do cliente. Artigos adicionais abrangem migrando configuração e código de identidade encontrada em muitos projetos do ASP.NET MVC.

> [!NOTE]
> Os números de versão nos exemplos podem não ser atuais. Talvez seja necessário atualizar seus projetos adequadamente.

## <a name="create-the-starter-aspnet-mvc-project"></a>Criar o projeto ASP.NET MVC starter

Para demonstrar a atualização, vamos começar criando um aplicativo ASP.NET MVC. Criá-lo com o nome *WebApp1* para o espaço para nome coincida com o projeto do ASP.NET Core que criamos na próxima etapa.

![Caixa de diálogo do Visual Studio novo projeto](mvc/_static/new-project.png)

![Caixa de diálogo nova aplicativo Web: modelo de projeto MVC selecionado no painel de modelos do ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Opcional:* alterar o nome da solução de *WebApp1* para *Mvc5*. O Visual Studio exibe o novo nome de solução (*Mvc5*), que torna mais fácil informar esse projeto da próximo projeto.

## <a name="create-the-aspnet-core-project"></a>Criar o projeto do ASP.NET Core

Criar um novo *vazio* aplicativo web do ASP.NET Core com o mesmo nome que o projeto anterior (*WebApp1*) para corresponder os namespaces em dois projetos. Ter o mesmo namespace torna mais fácil copiar código entre os dois projetos. Você precisará criar este projeto em um diretório diferente do projeto anterior para usar o mesmo nome.

![Caixa de diálogo Novo Projeto](mvc/_static/new_core.png)

![Caixa de diálogo nova aplicativo Web do ASP.NET: modelo de projeto vazio selecionado no painel de modelos do ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Opcional:* criar um aplicativo ASP.NET Core novo usando o *aplicativo Web* modelo de projeto. Nomeie o projeto *WebApp1*e selecione uma opção de autenticação de **contas de usuário individuais**. Renomear este aplicativo para *FullAspNetCore*. Criando isso economiza projeto tempo na conversão. Você pode examinar o código gerado pelo modelo para ver o resultado final ou copiar o código para o projeto de conversão. Também é útil quando você tiver problemas em uma etapa de conversão para comparar com o projeto de modelo gerado.

## <a name="configure-the-site-to-use-mvc"></a>Configurar o site para usar o MVC

* Durante o direcionamento o núcleo do .NET, o ASP.NET Core metapackage é adicionado ao projeto, chamado `Microsoft.AspNetCore.All` por padrão. Este pacote contém os pacotes como `Microsoft.AspNetCore.Mvc` e `Microsoft.AspNetCore.StaticFiles`. Se o destino do .NET Framework, referências de pacote precisam listada individualmente no arquivo csproj.

`Microsoft.AspNetCore.Mvc` é a estrutura MVC do ASP.NET Core. `Microsoft.AspNetCore.StaticFiles` é o manipulador de arquivo estático. O tempo de execução do ASP.NET Core é modular, e você deve optar explicitamente para servir arquivos estáticos (consulte [arquivos estáticos](xref:fundamentals/static-files)).

* Abra o *Startup.cs* de arquivo e altere o código para coincidir com o seguinte:

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

O `UseStaticFiles` método de extensão adiciona o manipulador de arquivo estático. Conforme mencionado anteriormente, o tempo de execução do ASP.NET é modular, e você deve optar explicitamente para servir arquivos estáticos. O `UseMvc` método de extensão adiciona o roteamento. Para obter mais informações, consulte [inicialização do aplicativo](xref:fundamentals/startup) e [roteamento](xref:fundamentals/routing).

## <a name="add-a-controller-and-view"></a>Adicionar um controlador e o modo de exibição

Nesta seção, você adicionará um controlador mínimo e o modo de exibição para servir como espaços reservados para o controlador do ASP.NET MVC e modos de exibição que você vai migrar na próxima seção.

* Adicionar um *controladores* pasta.

* Adicionar um **classe Controller** chamado *HomeController* para o *controladores* pasta.

![Caixa de diálogo Adicionar Novo Item](mvc/_static/add_mvc_ctl.png)

* Adicionar um *exibições* pasta.

* Adicionar um *exibições/inicial* pasta.

* Adicionar um **exibição Razor** chamado *cshtml* para o *exibições/inicial* pasta.

![Caixa de diálogo Adicionar Novo Item](mvc/_static/view.png)

A estrutura do projeto é mostrada abaixo:

![Gerenciador de soluções mostrando arquivos e pastas de WebApp1](mvc/_static/project-structure-controller-view.png)

Substitua o conteúdo do *Views/Home/Index.cshtml* arquivo com o seguinte:

```html
<h1>Hello world!</h1>
```

Execute o aplicativo.

![Aplicativo Web aberto no Microsoft Edge](mvc/_static/hello-world.png)

Consulte [controladores](xref:mvc/controllers/actions) e [exibições](xref:mvc/views/overview) para obter mais informações.

Agora que temos um projeto do ASP.NET Core trabalho mínimo, podemos começar a migrar a funcionalidade do projeto ASP.NET MVC. É necessário mover o seguinte:

* conteúdo do lado do cliente (CSS, fontes e scripts)

* controladores

* modos de exibição

* modelos

* Agrupamento

* filtros

* Log de entrada/saída, de identidade (Isso é feito no tutorial próximo).

## <a name="controllers-and-views"></a>Controladores e exibições

* Copiar cada um dos métodos do ASP.NET MVC `HomeController` para o novo `HomeController`. Observe que, no ASP.NET MVC, tipo de método de ação retorno controlador do modelo interno é [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); no ASP.NET MVC de núcleo, os métodos de ação retorno `IActionResult` em vez disso. `ActionResult` implementa `IActionResult`, portanto, não é necessário alterar o tipo de retorno de métodos de ação.

* Copie o *About.cshtml*, *Contact.cshtml*, e *cshtml* arquivos de exibição Razor do projeto ASP.NET MVC para o projeto do ASP.NET Core.

* Executar o aplicativo do ASP.NET Core e cada método de teste. Ainda não migramos o arquivo de layout ou estilos ainda, para que os modos de exibição renderizados só contêm o conteúdo de arquivos de exibição. Você não terá os links dos arquivos gerados layout para o `About` e `Contact` exibe, para que você precisará invocá-los a partir do navegador (substitua **4492** com o número da porta usado no projeto).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Página de contato](mvc/_static/contact-page.png)

Observe que a falta de estilo e itens de menu. Corrigiremos isso na próxima seção.

## <a name="static-content"></a>Conteúdo estático

Em versões anteriores do ASP.NET MVC, conteúdo estático hospedado da raiz do projeto da web e foi misturado com os arquivos do servidor. No núcleo do ASP.NET, conteúdo estático é hospedado no *wwwroot* pasta. Você desejará copiar o conteúdo estático de seu aplicativo ASP.NET MVC antigo para o *wwwroot* pasta em seu projeto do ASP.NET Core. Nessa conversão de exemplo:

* Copiar o *favicon.ico* arquivo de projeto MVC antigo para o *wwwroot* pasta do projeto do ASP.NET Core.

O ASP.NET MVC antigo projeto usa [Bootstrap](https://getbootstrap.com/) para seu estilo e repositórios de arquivos a inicialização a *conteúdo* e *Scripts* pastas. O modelo, que gerou o antigo projeto ASP.NET MVC, referencia o Bootstrap no arquivo de layout (*Views/Shared/_Layout.cshtml*). Você poderá copiar o *bootstrap.js* e *bootstrap.css* projeto de arquivos do ASP.NET MVC para o *wwwroot* pasta no novo projeto. Em vez disso, vamos adicionar suporte para inicialização (e outras bibliotecas de cliente) usando CDNs na próxima seção.

## <a name="migrate-the-layout-file"></a>Migrar o arquivo de layout

* Copiar o *viewstart* arquivo a partir do projeto ASP.NET MVC antigo *exibições* pasta para do projeto ASP.NET Core *exibições* pasta. O *viewstart* arquivo não foi alterado no MVC do ASP.NET Core.

* Criar um *exibições/compartilhadas* pasta.

* *Opcional:* cópia *viewimports. cshtml* do *FullAspNetCore* do projeto MVC *exibições* pasta para do projeto ASP.NET Core  *Modos de exibição* pasta. Remover qualquer declaração de namespace no *viewimports. cshtml* arquivo. O *viewimports. cshtml* arquivo fornece namespaces para todos os arquivos de exibição e coloca [auxiliares de marcação](xref:mvc/views/tag-helpers/intro). Os auxiliares de marca são usados no novo arquivo de layout. O *viewimports. cshtml* arquivo é novo para o ASP.NET Core.

* Copie o *cshtml* arquivo a partir do projeto ASP.NET MVC antigo *exibições/compartilhadas* pasta para do projeto ASP.NET Core *exibições/compartilhadas* pasta.

Abra *cshtml* de arquivo e faça as alterações a seguir (o código completo é mostrado abaixo):

* Substituir `@Styles.Render("~/Content/css")` com um `<link>` elemento carregar *bootstrap.css* (veja abaixo).

* Remova `@Scripts.Render("~/bundles/modernizr")`.

* Comente o `@Html.Partial("_LoginPartial")` linha (envolvem a linha com `@*...*@`). Retornaremos a ele um tutorial futuras.

* Substituir `@Scripts.Render("~/bundles/jquery")` com um `<script>` elemento (veja abaixo).

* Substituir `@Scripts.Render("~/bundles/bootstrap")` com um `<script>` elemento (veja abaixo).

A marcação de substituição para inclusão de inicialização CSS:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

A marcação de substituição para jQuery e inclusão de inicialização JavaScript:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

A atualização *cshtml* arquivo é mostrado abaixo:

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

Exiba o site no navegador. Ele agora deve carregar corretamente, com os estilos esperados em vigor.

* *Opcional:* você talvez queira usar o novo arquivo de layout. Para este projeto, você pode copiar o arquivo de layout do *FullAspNetCore* projeto. O novo arquivo de layout usa [auxiliares de marcação](xref:mvc/views/tag-helpers/intro) e tiver outros aprimoramentos.

## <a name="configure-bundling-and-minification"></a>Configurar o empacotamento e minimização

Para obter informações sobre como configurar o empacotamento e minimização, consulte [empacotamento e minimização](../client-side/bundling-and-minification.md).

## <a name="solve-http-500-errors"></a>Resolver erros HTTP 500

Há muitos problemas que podem causar uma mensagem de erro HTTP 500 que não contêm informações sobre a origem do problema. Por exemplo, se o *Views/_ViewImports.cshtml* arquivo contém um namespace que não existe em seu projeto, você obterá um erro HTTP 500. Por padrão em aplicativos do ASP.NET Core, o `UseDeveloperExceptionPage` extensão é adicionada para o `IApplicationBuilder` e executado quando a configuração é *desenvolvimento*. Isso é detalhado no código a seguir:

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core converte as exceções sem tratamento em um aplicativo web em respostas de erro HTTP 500. Normalmente, os detalhes do erro não estão incluídos nessas respostas para evitar a divulgação de informações potencialmente confidenciais sobre o servidor. Consulte **usando a página de exceção de desenvolvedor** na [tratar erros](../fundamentals/error-handling.md) para obter mais informações.

## <a name="additional-resources"></a>Recursos adicionais

* [Desenvolvimento do lado do cliente](xref:client-side/index)
* [Auxiliares de marcação](xref:mvc/views/tag-helpers/intro)
