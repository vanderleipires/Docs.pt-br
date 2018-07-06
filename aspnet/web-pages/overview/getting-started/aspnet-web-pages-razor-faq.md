---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: Perguntas frequentes sobre o (Razor) de páginas da Web ASP.NET | Microsoft Docs
author: tfitzmac
description: Este artigo lista algumas perguntas frequentes sobre o ASP.NET Web Pages (Razor) e o WebMatrix. Versões de software usadas no tutorial páginas da Web ASP.NET (R...
ms.author: aspnetcontent
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 1f97056c952757ea2d009eaca57d904791e80ca3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814230"
---
<a name="aspnet-web-pages-razor-faq"></a>Perguntas frequentes sobre o (Razor) de páginas da Web ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > O WebMatrix não é mais recomendado como um ambiente de desenvolvimento integrado para páginas da Web do ASP.NET. Use [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) ou [Visual Studio Code](https://code.visualstudio.com/).
>
> Este artigo lista algumas perguntas frequentes sobre o ASP.NET Web Pages (Razor) e o WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 3
> - Visual Studio 2013
> - O WebMatrix 3
>   
> 
> Este tutorial também funciona com o Visual Studio 2012, o WebMatrix 2 e ASP.NET Web Pages 2.


- [Qual é a diferença entre páginas da Web ASP.NET, Web Forms do ASP.NET e ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [O WebMatrix é necessário para trabalhar com páginas da Web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [Posso usar controles de Web Forms do ASP.NET em uma página de páginas da Web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [Pode implantar um site de páginas da Web ASP.NET sem usar o WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [É necessário usar o auxiliar WebSecurity para dar suporte a logons?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [Páginas da Web ASP.NET oferece suporte a HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [Pode usar JavaScript e jQuery com páginas da Web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Recursos adicionais](#AdditionalResources)

Para perguntas sobre erros e outros problemas, consulte o [guia de solução de problemas de páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>Qual é a diferença entre páginas da Web ASP.NET, Web Forms do ASP.NET e ASP.NET MVC?

Todos os três são tecnologias ASP.NET para a criação de aplicativos web dinâmicos:

- Páginas da Web ASP.NET concentra-se na adição de código dinâmico de (servidor) e acesso de banco de dados para páginas HTML e a sintaxe simples e leve de recursos.
- ASP.NET Web Forms se baseia em um modelo de objeto de página e controles tradicionais de tipo de janela (botões, listas, etc.). Formulários da Web usa um modelo baseado em evento que é familiar àqueles que já trabalhou com o desenvolvimento baseado no cliente de (formulários do Windows).
- O ASP.NET MVC implementa o padrão model-view-controller para o ASP.NET. A ênfase está em "separação de preocupações" (processamento de dados e camadas de interface do usuário).

Todas as três estruturas são totalmente suportadas e continuam sendo desenvolvidos pela equipe do ASP.NET. Em geral, a escolha de qual estrutura a ser usada depende do seu plano de fundo e experiência com o ASP.NET.

Em particular, as páginas da Web do ASP.NET foi projetado para torna mais fácil para as pessoas que já conhecem o HTML para adicionar processamento do servidor para suas páginas. É uma boa opção para estudantes, diletantes e, em geral de pessoas que são novos para a programação. Ele também pode ser uma boa escolha para desenvolvedores que têm experiência com tecnologias da web não sejam do ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>O WebMatrix é necessário para trabalhar com páginas da Web?

Nº O WebMatrix não é mais recomendado como um ambiente de desenvolvimento integrado para páginas da Web do ASP.NET. Use [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) ou [Visual Studio Code](https://code.visualstudio.com/).

Se você não quiser usar o Visual Studio ou Visual Studio Code, você pode instalar os produtos de componente individualmente usando [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Você precisará os seguintes produtos:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (que é instalado também a estrutura de páginas da Web ASP.NET)
- O IIS Express (servidor web)
- Microsoft SQL Server Compact 4.0 (banco de dados)

Você pode usar um editor de texto para editar *. cshtml* (ou *. vbhtml*) páginas.

Gerenciando bancos de dados do SQL Server Compact (*sdf* arquivos) sem uma ferramenta é um pouco mais difícil. Ferramentas do Visual Studio containds para gerenciar *sdf* bancos de dados. Você também pode executar comandos SQL no código para executar muitas tarefas de gerenciamento do SQL Server.

Para testar *. cshtml* páginas sem usar um ambiente de desenvolvimento integrado (IDE), você pode implantá-los a um servidor ativo. (Consulte [posso implantar um site de páginas da Web ASP.NET sem usar o WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Executando o IIS Express sem usar um IDE

Se você instalar o IIS Express no computador como um servidor web, você pode usar que para testar as páginas. Você pode executar o IIS Express na linha de comando e associá-lo a um número de porta específico. Especifique essa porta, em seguida, quando você solicita *. cshtml* arquivos em seu navegador.

No Windows, abra um prompt de comando com privilégios de administrador e altere para *Files\IIS de C:\Program Express.* (Para sistemas de 64 bits, use a pasta *C:\Program Files (x86) \IIS Express.)* Em seguida, insira o seguinte comando, usando o caminho real para seu site:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Você pode usar qualquer número de porta que já não está reservado por outro processo. (Números de porta acima de 1024 são normalmente livres). Para o `path` o valor, use o caminho da pasta do site em que o *. cshtml* são arquivos.

Depois de executar este comando para configurar o IIS Express para atender a suas páginas, você pode abrir um navegador e navegue até um *. cshtml* arquivo. Use uma URL semelhante à seguinte:

`http://localhost:35896/default.cshtml`

Para obter ajuda com o IIS Express opções de linha de comando, digite `iisexpress.exe /?` na linha de comando.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>Posso usar controles de Web Forms do ASP.NET em uma página de páginas da Web?

Nº Controles do Web Forms, como o [caixa de seleção](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) controle, o [controles de validação](https://msdn.microsoft.com/library/bwd43d0x)e o [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) controlar funciona somente nas páginas de Web Forms (*. aspx* arquivos). Esses controles exigem a estrutura da página de Web Forms.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>Pode implantar um site de páginas da Web ASP.NET sem usar o WebMatrix?

Sim. Manualmente, você pode copiar arquivos do site para um servidor (normalmente por meio de FTP). Se você executar uma cópia manual, você precisa copiar os arquivos que dão suporte ao SQL Server Compact (banco de dados). Para obter detalhes, consulte a entrada de blog [Implantando páginas da Web de aplicativos sem uma ferramenta](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>É necessário usar o auxiliar WebSecurity para dar suporte a logons?

Nº O `SimpleMembership` provedor que faz parte das páginas da Web do ASP.NET é uma opção. Os provedores de segurança que fazem parte do ASP.NET (que você pode ser usado para trabalhar com formulários da Web) também estão disponíveis. Por exemplo, você pode usar autenticação de formulários em páginas da Web do ASP.NET como faria em formulários da Web. Para um exemplo de como usar a autenticação de formulários, consulte o artigo do Microsoft Support [How To Implement Forms-Based autenticação em seu aplicativo ASP.NET usando C# .NET](https://support.microsoft.com/kb/301240). Para baixar um exemplo simples, consulte [versão do ASP.NET de "logon &amp; senha](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Para obter informações sobre como usar a autenticação do Windows, consulte o postagem no blog [autenticação do Windows usando in ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>Páginas da Web ASP.NET oferece suporte a HTML5?

Sim. As páginas criadas com o ASP.NET Web Pages (*. cshtml* ou *. vbhtml* páginas) são, essencialmente, páginas HTML que também contêm código que é executado no servidor, antes da página é renderizada. Desde que o navegador do usuário oferece suporte a HTML5, você pode usar elementos HTML5 em uma *. cshtml* ou *. vbhtml* página.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>Pode usar JavaScript e jQuery com páginas da Web?

Com certeza. As páginas criadas com o ASP.NET Web Pages (*. cshtml* ou *. vbhtml* páginas) são apenas as páginas HTML com código de servidor contidos neles. Portanto, qualquer coisa que você pode fazer em uma página HTML normal por usando JavaScript ou jQuery você também pode fazer em um *. cshtml* ou *. vbhtml* página.

O **Starter Site** modelo no WebMatrix contém um número de bibliotecas do jQuery. Se você criar um site usando esse modelo, o *Scripts* pasta contém uma biblioteca principal do jQuery (*1.6.2.js jquery)* e bibliotecas para validação do jQuery (*jquery.validate.js*, etc.).

Aqui estão algumas postagens em blog que ilustram as maneiras de usar jQuery com páginas da Web do ASP.NET:

- [Adicionar jQuery adequação para páginas da Web ASP.NET usando o WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) de Rachel Appel
- [5 minutos: WebMatrix + jQuery UI + json + jQuery modelos](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) por Jonas Eriksson
- [O WebMatrix e formulários de jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) por Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionais


[Guia de solução de problemas de Páginas da Web do ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[Fórum do WebMatrix e páginas da Web ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix) no site do ASP.NET
