---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web Pages FAQ (Razor) | Microsoft Docs
author: tfitzmac
description: Este artigo lista algumas perguntas frequentes sobre páginas da Web do ASP.NET (Razor) e o WebMatrix. Versões de software usadas no tutorial páginas da Web ASP.NET (R....
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 60cc4ca364923cb131d5e91cd7b6307b1e68644b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
ms.locfileid: "28042453"
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages FAQ (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > O WebMatrix não é recomendado como um ambiente de desenvolvimento integrado para páginas da Web do ASP.NET. Use [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) ou [código do Visual Studio](https://code.visualstudio.com/).
>
> Este artigo lista algumas perguntas frequentes sobre páginas da Web do ASP.NET (Razor) e o WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Este tutorial também funciona com o WebMatrix 2, 2 de páginas da Web do ASP.NET e Visual Studio 2012.


- [Qual é a diferença entre páginas da Web ASP.NET Web Forms do ASP.NET e ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [O WebMatrix é necessário para trabalhar com páginas da Web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [É possível usar os controles de formulários da Web ASP.NET em uma página de páginas da Web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Pode implantar um site de páginas da Web ASP.NET sem usar o WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [É necessário usar o auxiliar WebSecurity para oferecer suporte a logons?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Páginas da Web ASP.NET oferece suporte a HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Pode usar JavaScript e jQuery com páginas da Web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Recursos adicionais](#AdditionalResources)

Para perguntas sobre erros e outros problemas, consulte o [guia de solução de problemas de páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Qual é a diferença entre páginas da Web ASP.NET Web Forms do ASP.NET e ASP.NET MVC?

Todos os três são tecnologias ASP.NET para a criação de aplicativos web dinâmicos:

- Páginas da Web ASP.NET se concentra na adição de código (lado do servidor) dinâmico e acesso de banco de dados para páginas HTML e a sintaxe simples e leve de recursos.
- ASP.NET Web Forms baseia-se em um modelo de objeto de página e controles do tipo de janela tradicional (botões, listas, etc.). Formulários da Web usa um modelo baseado em evento que é familiar para aqueles que já trabalhou com o desenvolvimento de (Windows forms) com base no cliente.
- ASP.NET MVC implementa o padrão model-view-controller para ASP.NET. O foco é a "separação de preocupações" (processamento de dados e camadas de interface do usuário).

Todas as estruturas de três são totalmente suportadas e continuam a ser desenvolvido pela equipe do ASP.NET. Em geral, a opção de estrutura a ser usada depende de seu plano de fundo e experiência com o ASP.NET.

Em particular, páginas da Web do ASP.NET foi projetado para tornar mais fácil para as pessoas que já conhecem o HTML para adicionar processamento do servidor para suas páginas. É uma boa escolha para estudantes, entusiastas, pessoas em geral que são iniciantes em programação. Ele também pode ser uma boa escolha para desenvolvedores que têm experiência com tecnologias da web do ASP.NET não.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>O WebMatrix é necessário para trabalhar com páginas da Web?

Nº O WebMatrix não é recomendado como um ambiente de desenvolvimento integrado para páginas da Web do ASP.NET. Use [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) ou [código do Visual Studio](https://code.visualstudio.com/).

Se você não quiser usar o Visual Studio ou o código do Visual Studio, você pode instalar os produtos de componente individualmente usando [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). É necessário que os seguintes produtos:

- Microsoft .NET Framework 4.5
- O ASP.NET MVC 5 (que instala o framework de páginas da Web ASP.NET também)
- O IIS Express (servidor web)
- Microsoft SQL Server Compact 4.0 (o banco de dados)

Você pode usar um editor de texto para editar *. cshtml* (ou *. vbhtml*) páginas.

Gerenciando bancos de dados do SQL Server Compact (*. sdf* arquivos) sem uma ferramenta é um pouco mais difícil. Ferramentas do Visual Studio containds para gerenciar *. sdf* bancos de dados. Você também pode executar comandos SQL no código para executar muitas tarefas de gerenciamento do SQL Server.

Para testar *. cshtml* páginas sem usar um ambiente de desenvolvimento integrado (IDE), você pode implantá-los em um servidor ativo. (Consulte [posso implantar um site de páginas da Web ASP.NET sem usar o WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Executando o IIS Express sem usar um IDE

Se você instalar o IIS Express no computador como um servidor web, você pode usar isso para testar as páginas. Você pode executar o IIS Express da linha de comando e associá-lo com um número de porta específico. Especifique essa porta, em seguida, quando você solicitar *. cshtml* arquivos em seu navegador.

No Windows, abra um prompt de comando com privilégios de administrador e altere para *C:\Program Files\IIS Express.* (Para sistemas de 64 bits, use a pasta *C:\Program Files (x86) \IIS Express.)* Em seguida, digite o seguinte comando, usando o caminho real para o site:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Você pode usar qualquer número de porta que já não está reservado por outro processo. (Números de porta acima de 1024 são normalmente livres). Para o `path` valor, use o caminho de pasta do site onde o *. cshtml* são arquivos.

Depois de executar esse comando para configurar o IIS Express para atender a suas páginas, você pode abrir um navegador e navegue até um *. cshtml* arquivo. Use uma URL semelhante ao seguinte:

`http://localhost:35896/default.cshtml`

Para obter ajuda com o IIS Express opções de linha de comando, digite `iisexpress.exe /?` na linha de comando.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>É possível usar os controles de formulários da Web ASP.NET em uma página de páginas da Web?

Nº Controles do Web Forms como o [caixa de seleção](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) controle, o [controles de validação](https://msdn.microsoft.com/library/bwd43d0x)e o [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) controlar somente o trabalho em páginas Web Forms (*. aspx* arquivos). Esses controles requerem a estrutura da página de Web Forms.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Pode implantar um site de páginas da Web ASP.NET sem usar o WebMatrix?

Sim. Você pode copiar manualmente os arquivos de site para um servidor (normalmente, usando o FTP). Se você executar uma cópia manual, você também precisa copiar os arquivos que dão suporte ao SQL Server Compact (banco de dados). Para obter detalhes, consulte a postagem do blog [aplicativos de implantação de páginas da Web sem uma ferramenta](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>É necessário usar o auxiliar WebSecurity para oferecer suporte a logons?

Nº O `SimpleMembership` provedor que faz parte de páginas da Web do ASP.NET é uma opção. Os provedores de segurança que fazem parte do ASP.NET (que você pode ser usado para trabalhar com formulários da Web) também estão disponíveis. Por exemplo, você pode usar autenticação de formulários em páginas da Web do ASP.NET exatamente como você faria em Web Forms. Para um exemplo de como usar a autenticação de formulários, consulte o artigo Microsoft Support [How To Implement Forms-Based autenticação em seu aplicativo ASP.NET usando C# .NET](https://support.microsoft.com/kb/301240). Para baixar um exemplo simples, consulte [versão do ASP.NET de "logon &amp; senha](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Para obter informações sobre como usar a autenticação do Windows, consulte o postagem de blog [autenticação usando o Windows em páginas da Web do ASP.NET](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Páginas da Web ASP.NET oferece suporte a HTML5?

Sim. As páginas criadas com páginas da Web do ASP.NET (*. cshtml* ou *. vbhtml* páginas) são essencialmente páginas HTML que também contêm código que é executado no servidor, antes da página é renderizada. Como o navegador do usuário oferece suporte a HTML5, você pode usar elementos de HTML5 em um *. cshtml* ou *. vbhtml* página.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Pode usar JavaScript e jQuery com páginas da Web?

Com certeza. As páginas criadas com páginas da Web do ASP.NET (*. cshtml* ou *. vbhtml* páginas) são apenas as páginas HTML com o código de servidor neles. Portanto, qualquer coisa que você pode fazer em uma página HTML normal usando JavaScript ou jQuery você também pode fazer um *. cshtml* ou *. vbhtml* página.

O **Site inicial** modelo no WebMatrix contém um número de bibliotecas jQuery. Se você criar um site usando esse modelo, o *Scripts* pasta contém uma biblioteca principal do jQuery (*1.6.2.js jquery)* e bibliotecas para validação jQuery (*jquery.validate.js*, etc.).

Aqui estão algumas postagens de blog que ilustram como usar jQuery com páginas da Web do ASP.NET:

- [Adicionando jQuery adequação para páginas da Web ASP.NET usando o WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) por Rachel Appel
- [5 minutos: WebMatrix jQuery UI + json + jQuery modelos](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) por Jonas Eriksson
- [O WebMatrix e formulários de jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) por Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Guia de solução de problemas de Páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[O WebMatrix e páginas da Web ASP.NET de fórum](https://forums.asp.net/1224.aspx/1?WebMatrix) no site do ASP.NET
