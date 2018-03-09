---
uid: mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
title: Como atualizar um ASP.NET MVC 4 e Web API projeto ASP.NET MVC 5 e Web API 2 | Microsoft Docs
author: Rick-Anderson
description: "O ASP.NET MVC 5 e Web API 2 colocar um host de novos recursos, incluindo roteamento de atributo, filtros de autenticação e muito mais."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: db0d02d9-58e8-4a0b-8d7d-b8df8ea97b88
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 05a3189cf105d1230b96e90b46ea5ab60fef1bf1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2"></a>Como atualizar um ASP.NET MVC 4 e o projeto de API da Web para ASP.NET MVC 5 e Web API 2
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> O ASP.NET MVC 5 e Web API 2 colocar um host de novos recursos, incluindo roteamento de atributo, filtros de autenticação e muito mais. Consulte [https://www.asp.net/vnext](https://www.asp.net/core) para obter mais detalhes.
> 
> Este passo a passo irá guiá-lo com as etapas necessárias para atualizar o aplicativo para a versão mais recente.  
> 
> > [!NOTE]
> > Consulte [ASP.NET e Web Tools para notas de versão do Visual Studio 2013](../../../visual-studio/overview/2013/release-notes.md) para obter informações sobre as alterações significativas de MVC 4 e a API da Web para a próxima versão.
> 
>   
> 
> Este artigo foi escrito, Youngjune Hong e Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) )


## <a name="upgrade-steps"></a>Etapas de atualização

1. Fazer backup de seu projeto. Este passo a passo exigirá que você faça alterações em seu arquivo de projeto, a configuração do pacote e a arquivos Web. config.
2. Para atualizar da API da Web para API Web 2 em global. asax, altere:

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample1.cs)]

 para

    [!code-csharp[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample2.cs)]
3. Verifique se todos os pacotes que usam seus projetos são compatíveis com MVC 5 e a API Web 2. A seguinte tabela mostra o MVC 4 e API da Web relacionadas a pacotes que precisam ser alteradas. Se você tiver um pacote que depende de um dos pacotes listados abaixo, entre em contato com os editores para obter as versões mais recentes são compatíveis com MVC 5 e a API Web 2. Se você tiver o código-fonte para esses pacotes, você deve recompilá-los com os novos assemblies de MVC 5 e a API Web 2.   

    | **Id do pacote** | **Versão antiga** | **Nova versão** |
    | --- | --- | --- |
    | Microsoft.AspNet.Razor | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.WebData | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.WebPages.OAuth | 2.0.x.x | 3.0.0 |
    | Microsoft.AspNet.Mvc | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.Mvc.Facebook | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Core | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.SelfHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Client | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.OData | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.WebHost | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.Tracing | 4.0.x.x | 5.0.0 |
    | Microsoft.AspNet.WebApi.HelpPage | 4.0.x.x | 5.0.0 |
    | Microsoft.Net.Http | 2.0. x. | 2.2. |
    | Exibidos | 5.2 | 5.6.x |
    | System. Spatial | 5.2 | 5.6.x |
    | Microsoft.Data.Edm | 5.2 | 5.6.x |
    | Microsoft.AspNet.Mvc.FixedDisplayModes | < o: p >< / o: p > | Removido |
    | Microsoft.AspNet.WebPages.Administration | < o: p >< / o: p > | Removido |
    | Auxiliares da Web Microsoft | < o: p >< / o: p > | Microsoft.AspNet.WebHelpers |

    > [!NOTE]
    > Auxiliares da Web Microsoft foi substituído pelo Microsoft.AspNet.WebHelpers. Você deve remover o pacote antigo primeiro e, em seguida, instale o pacote mais recente.   
    >   
    > Não há nenhuma compatibilidade de versão cruzada entre pacotes principais do ASP.NET. Por exemplo, o MVC 5 é compatível com apenas 3 Razor e não Razor 2.
4. Abra seu projeto no Visual Studio 2013.
5. Remova qualquer um dos seguintes pacotes do NuGet do ASP.NET que estão instalados. Você removerá essas usando o Console de Gerenciador de pacote (PMC). Para abrir o PMC, selecione o **ferramentas** menu e selecione **Gerenciador de biblioteca de pacote,** , em seguida, selecione **Package Manager Console**. Seu projeto pode não incluir todas elas.

    1. `Microsoft.AspNet.WebPages.Administration`  
 Este pacote normalmente é adicionado ao atualizar do MVC 3 para MVC 4. Para removê-lo, execute o seguinte comando no PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.WebPages.Administration`
    2. `Microsoft-Web-Helpers`   
 Este pacote foi rebatizado como `Microsoft.AspNet.WebHelpers`. Para removê-lo, execute o seguinte comando no PMC:  
        `Uninstall-Package -Id Microsoft-Web-Helpers`
    3. `Microsoft.AspNet.Mvc.FixedDisplayMode`  
 Este pacote contém uma solução alternativa para um bug no MVC 4 que foi corrigido no MVC 5. Para removê-lo, execute o seguinte comando no PMC:  
        `Uninstall-Package -Id Microsoft.AspNet.Mvc.FixedDisplayModes`
6. Atualize todos os pacotes do NuGet do ASP.NET usando o PMC. Em PMC, execute o seguinte comando:  
    `Update-Package`  
 O `Update-Package` sem qualquer parâmetro atualizará todos os pacotes de comando. Você pode atualizar pacotes individualmente usando o argumento de ID. Para obter mais informações sobre o comando de atualização, execute `get-help update-package` .

## <a name="update-the-application-webconfig-file"></a>Atualizar o aplicativo *Web. config* arquivo

Certifique-se de fazer essas alterações no aplicativo *Web. config* arquivo, não o *Web. config* arquivo o *exibições* pasta.

Localize o `<runtime>/<assemblyBinding>` seção e faça as seguintes alterações:

1. Os elementos com o atributo de nome "System.Web.Mvc", altere o número de versão de "4.0.0.0" para "5.0.0.0". (Duas alterações no elemento.)
2. Em elementos com o nome do atributo &quot;Helpers "e &quot;System.Web.WebPages&quot; alterar o número de versão de"2.0.0.0"a"3.0.0.0". Quatro alterações ocorrerá, dois em cada um dos elementos.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample3.xml?highlight=6,10,14)]
3. Localize o `<appSettings>` seção e atualize webpages:version de 2.0.0.0.0 para 3.0.0.0, conforme mostrado abaixo:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample4.xml?highlight=2)]
4. Remova quaisquer níveis de confiança que não seja completo. Por exemplo:

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample5.xml?highlight=2)]

## <a name="update-the-webconfig-files-under-the-views-folder"></a>Atualização de *Web. config* arquivos na pasta modos de exibição

Se seu aplicativo estiver usando áreas, você também precisará atualizar cada *Web. config* arquivo o *exibições* subpasta da pasta de cada área.

1. Atualize todos os elementos que contêm "System.Web.Mvc" da versão "4.0.0.0" versão "5.0.0.0".  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample6.xml?highlight=2)]

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample7.xml?highlight=4-6,8)]
2. Atualize todos os elementos que contêm "Webpages" da versão "2.0.0.0" versão "3.0.0.0". Se esta seção contém "System.Web.WebPages", atualize esses elementos de versão "2.0.0.0" versão "3.0.0.0"  

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample8.xml?highlight=3-5)]
3. Se você tiver removido o `Microsoft-Web-Helpers` instalar o pacote do NuGet em uma etapa anterior, `Microsoft.AspNet.WebHelpers` com o seguinte comando no PMC:  
    `Install-Package -Id Microsoft.AspNet.WebHelpers`
4. Se seu aplicativo usa o [User.IsInRole()](https://msdn.microsoft.com/en-us/library/system.web.security.roleprincipal.isinrole(v=vs.110).aspx) método, adicione o seguinte para o *Web. config* arquivo.

    [!code-xml[Main](how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2/samples/sample9.xml)]

## <a name="final-steps"></a>Etapas finais

Compilar e testar o aplicativo.

Remova o tipo de projeto MVC 4 GUID dos arquivos de projeto.

1. No Gerenciador de soluções, clique no nome do projeto e, em seguida, selecione **descarregar projeto**.
2. Clique com o botão direito e selecione Editar ProjectName.csproj.
3. Localize o `ProjectTypeGuids` elemento e, em seguida, remova o MVC 4 GUID do projeto, `{E3E379DF-F4C6-4180-9B81-6769533ABE47}`.
4. Salve e feche o arquivo de projeto aberto.
5. Clique com o botão direito e selecione **recarregar projeto**.
