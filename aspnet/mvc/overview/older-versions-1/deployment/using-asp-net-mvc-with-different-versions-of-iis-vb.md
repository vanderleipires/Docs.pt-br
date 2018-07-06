---
uid: mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
title: Usando o ASP.NET MVC com diferentes versões do IIS (VB) | Microsoft Docs
author: microsoft
description: Neste tutorial, você aprenderá como usar o ASP.NET MVC e o roteamento de URL, com diferentes versões dos serviços de informações da Internet. Você aprenderá as diferentes estratégias de...
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 1c1283b2-6956-4937-b568-d30de432ce23
msc.legacyurl: /mvc/overview/older-versions-1/deployment/using-asp-net-mvc-with-different-versions-of-iis-vb
msc.type: authoredcontent
ms.openlocfilehash: 4c0dc1ac08cfe06ad7ea35a6e6552ab1174ff989
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818198"
---
<a name="using-aspnet-mvc-with-different-versions-of-iis-vb"></a>Usando o ASP.NET MVC com diferentes versões do IIS (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Neste tutorial, você aprenderá como usar o ASP.NET MVC e o roteamento de URL, com diferentes versões dos serviços de informações da Internet. Você aprenderá as diferentes estratégias para usar o ASP.NET MVC com o IIS 7.0 (modo clássico), o IIS 6.0 e versões anteriores do IIS.


A estrutura ASP.NET MVC depende do roteamento do ASP.NET para rotear solicitações de navegador às ações do controlador. Para tirar proveito do roteamento do ASP.NET, você talvez precise executar etapas de configuração adicionais em seu servidor web. Tudo depende da versão dos serviços de informações da Internet (IIS) e o modo para seu aplicativo de processamento de solicitação.

Aqui está um resumo das diferentes versões do IIS:

- O IIS 7.0 (modo integrado) - nenhuma configuração especial é necessária para usar o roteamento do ASP.NET.
- O IIS 7.0 (modo clássico) - você precisa executar configuração especial para usar o roteamento do ASP.NET.
- IIS 6.0 ou abaixo - você precisa executar configuração especial para usar o roteamento do ASP.NET.

A versão mais recente do IIS é a versão 7.5 (no Win7). IIS 7 do IIS está incluído com o Windows Server 2008 e VISTA SP1 e superior. Você também pode instalar o IIS 7.0 em qualquer versão do sistema operacional Vista, exceto Home Basic (veja [ https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx ](https://technet.microsoft.com/library/cc731179%28WS.10%29.aspx)).

O IIS 7.0 oferece suporte a dois modos para processar solicitações. Você pode usar o modo integrado ou clássico. Você não precisa executar nenhuma etapa de configuração especial ao usar o IIS 7.0 no modo integrado. No entanto, você precisa executar uma configuração adicional ao usar o IIS 7.0 no modo clássico.

Microsoft Windows Server 2003 inclui o IIS 6.0. Você não pode atualizar o IIS 6.0 para o IIS 7.0 ao usar o sistema operacional Windows Server 2003. Ao usar o IIS 6.0, você deve executar etapas adicionais de configuração.

Microsoft Windows XP Professional inclui o IIS 5.1. Ao usar o IIS 5.1, você deve executar etapas adicionais de configuração.

Por fim, o Microsoft Windows 2000 e o Microsoft Windows 2000 Professional inclui o IIS 5.0. Ao usar o IIS 5.0, você deve executar etapas adicionais de configuração.

## <a name="integrated-versus-classic-mode"></a>Integrado versus modo clássico

O IIS 7.0 pode processar solicitações que usam os dois modos de processamento de solicitação diferentes: integrado e clássico. Modo integrado do fornece um desempenho melhor e mais recursos. Modo clássico está incluído com versões anteriores para compatibilidade com versões anteriores do IIS.

O modo de processamento de solicitação é determinado pelo pool de aplicativos. Você pode determinar qual modo de processamento está sendo usado por um aplicativo web específico, determinando o pool de aplicativos associado ao aplicativo. Siga estas etapas:

1. Inicie o Gerenciador de serviços de informações da Internet
2. Na janela conexões, selecione um aplicativo
3. Na janela de ações, clique o **configurações básicas** link para abrir a caixa de diálogo Editar aplicativo caixa (veja a Figura 1)
4. Anote o pool de aplicativos selecionado.

Por padrão, o IIS é configurado para dar suporte a dois pools de aplicativos: **DefaultAppPool** e **AppPool clássico do .NET**. Se DefaultAppPool for selecionada, seu aplicativo está em execução no modo de processamento de solicitação integrado. Se o .NET AppPool clássico está selecionada, seu aplicativo está em execução no modo de processamento de solicitação clássica.


[![A caixa de diálogo Novo projeto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image1.png)

**Figura 1**: detectando o modo de processamento de solicitação ([clique para exibir a imagem em tamanho normal](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.png))


Observe que você pode modificar o modo de processamento de solicitação dentro da caixa de diálogo Editar aplicativo. Clique no botão Selecionar e alterar o pool de aplicativos associado ao aplicativo. Perceba que há problemas de compatibilidade ao alterar um aplicativo ASP.NET do clássico para o modo integrado. Para obter mais informações, consulte os seguintes artigos:

- Atualizando o ASP.NET 1.1 para o IIS 7.0 no Windows Vista e Windows Server 2008 – [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/upgrading-aspnet-11-to-iis-on-windows-vista-and-windows-server-2008)

- Integração do ASP.NET com o IIS 7.0 – [https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)


Se um aplicativo ASP.NET estiver usando o DefaultAppPool, você não precisa executar nenhuma etapa adicional para obter o roteamento do ASP.NET (e, portanto, ASP.NET MVC) para trabalhar. No entanto, se o aplicativo ASP.NET estiver configurado para usar o .NET AppPool clássico, em seguida, continue lendo, você terá mais trabalho a fazer.

## <a name="using-aspnet-mvc-with-older-versions-of-iis"></a>Usando o ASP.NET MVC com versões anteriores do IIS

Se você precisar usar o ASP.NET MVC com uma versão anterior do IIS ao IIS 7.0, ou você precisa usar o IIS 7.0 no modo clássico, você tem duas opções. Primeiro, você pode modificar a tabela de rotas para usar extensões de arquivo. Por exemplo, em vez de solicitar uma URL como /Store/Details, você pode solicitar uma URL como /Store.aspx/Details.

A segunda opção é criar algo chamado de um *mapa de script curinga*. Um mapa de script curinga permite que você mapear todas as solicitações para a estrutura do ASP.NET.

Se você não tiver acesso ao seu servidor web (por exemplo, o ASP.NET MVC aplicativo está sendo hospedado por um provedor de serviços de Internet), em seguida, você precisará usar a primeira opção. Se você não quiser modificar a aparência de suas URLs, e você tem acesso ao seu servidor web, você pode usar a segunda opção.

Vamos explorar cada opção em detalhes nas seções a seguir.

## <a name="adding-extensions-to-the-route-table"></a>Adicionar extensões à tabela de rotas

É a maneira mais fácil de obter o roteamento do ASP.NET para trabalhar com versões anteriores do IIS modificar sua tabela de rotas no arquivo global. asax. O padrão e o arquivo global asax sem modificações na listagem 1 configura uma rota nomeada a rota padrão.

**Listagem 1 - global. asax (não modificado)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample1.vb)]

A rota padrão configurada na listagem 1 permite que você as URLs de rota que ter esta aparência:

/ Home/Index

Produto/3/detalhes

/ Produto

Infelizmente, as versões mais antigas do IIS não passam essas solicitações para a estrutura do ASP.NET. Portanto, essas solicitações não serão roteadas para um controlador. Por exemplo, se você fizer uma solicitação do navegador para o URL /Home/índice, em seguida, você obterá a página de erro na Figura 2.


[![A caixa de diálogo Novo projeto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image2.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.png)

**Figura 2**: recebendo um erro 404 não encontrado ([clique para exibir a imagem em tamanho normal](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.png))


Versões anteriores do IIS ser mapeado apenas determinadas solicitações para a estrutura do ASP.NET. A solicitação deve ser para uma URL com a extensão de arquivo correto. Por exemplo, uma solicitação para /SomePage.aspx é mapeada para a estrutura do ASP.NET. No entanto, uma solicitação para /SomePage.htm não permite.

Portanto, para obter o roteamento do ASP.NET para trabalhar, podemos deve modificar a rota padrão para que ele inclui uma extensão de arquivo que é mapeada para a estrutura do ASP.NET.

Isso é feito usando um script chamado `registermvc.wsf`. Ele foi incluído com o lançamento do ASP.NET MVC 1 em `C:\Program Files\Microsoft ASP.NET\ASP.NET MVC\Scripts`, mas a partir do ASP.NET 2 Este script foi movido para o ASP.NET Futures, disponível em [ http://aspnet.codeplex.com/releases/view/39978 ](http://aspnet.codeplex.com/releases/view/39978).

Executando esse script registra uma nova extensão. MVC com o IIS. Depois de registrar a extensão. MVC, você pode modificar suas rotas no arquivo global. asax para que as rotas de usam a extensão. MVC.

O arquivo global. asax modificado na listagem 2 funciona com versões anteriores do IIS.

**Listagem 2 - global. asax (modificado com extensões)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample2.vb)]


Importante: Lembre-se de criar seu aplicativo ASP.NET MVC novamente depois de alterar o arquivo global asax.


Há duas alterações importantes para o arquivo global asax na listagem 2. Agora há duas rotas definidas em global. asax. O padrão de URL para a rota padrão, a primeira rota, agora se parece com:

{controller}.mvc/{action}/{id}

A adição da extensão. MVC altera o tipo de arquivos que intercepta o módulo de roteamento do ASP.NET. Com essa alteração, o aplicativo ASP.NET MVC agora roteia solicitações semelhante ao seguinte:

/Home.MVC/index/

/Product.MVC/Details/3

/Product.mvc/

A segunda rota, a rota de raiz, há de nova. Esse padrão de URL para a rota de raiz é uma cadeia de caracteres vazia. Essa rota é necessária para correspondência de solicitações feitas na raiz do seu aplicativo. Por exemplo, a rota raiz corresponderá a uma solicitação que tem esta aparência:

[http://www.YourApplication.com/](http://www.YourApplication.com/)

Depois de fazer essas modificações à sua tabela de rotas, você precisará certificar-se de que todos os links em seu aplicativo são compatíveis com esses novos padrões de URL. Em outras palavras, certifique-se de que todos os seus links incluem a extensão. MVC. Se você usar o método auxiliar Html.ActionLink() para gerar seus links, não deve precisar fazer alterações.


Em vez de usar o script registermvc.wcf, você pode adicionar uma nova extensão para IIS que é mapeado para a estrutura do ASP.NET manualmente. Ao adicionar uma nova extensão, certifique-se de que a caixa de seleção rotulada **Verifique se esse arquivo existe** não está marcada.


## <a name="hosted-server"></a>Servidor hospedado

Você sempre não tem acesso ao seu servidor web. Por exemplo, se você estiver hospedando seu aplicativo ASP.NET MVC usando um provedor de hospedagem de Internet, em seguida, você não necessariamente terá acesso ao IIS.

Nesse caso, você deve usar uma das extensões de arquivo existentes que são mapeadas para a estrutura do ASP.NET. A. aspx,. axd e. ashx extensões são exemplos de extensões de arquivo mapeadas para o ASP.NET.

Por exemplo, o arquivo global. asax modificado na listagem 3 usa a extensão. aspx em vez da extensão. MVC.

**Listagem 3 - global. asax (modificado com extensões. aspx)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample3.vb)]

O arquivo global asax na listagem 3 é exatamente o mesmo que o arquivo global. asax anterior, exceto o fato de que ele usa a extensão. aspx, em vez da extensão. MVC. Você não precisa executar qualquer instalação em seu servidor web remoto para usar a extensão. aspx.

## <a name="creating-a-wildcard-script-map"></a>Criar um mapa de Script curinga

Se você não deseja modificar as URLs para seu aplicativo ASP.NET MVC, e você tem acesso ao seu servidor web, você tem uma opção adicional. Você pode criar um mapa de script curinga que mapeia todas as solicitações para o servidor web para a estrutura do ASP.NET. Dessa forma, você pode usar o padrão de tabela de rotas do ASP.NET MVC com o IIS 7.0 (no modo clássico) ou IIS 6.0.

Lembre-se de que essa opção faz com que o IIS interceptar todas as solicitações feitas em relação ao servidor web. Isso inclui solicitações de imagens, páginas clássicas ASP e páginas HTML. Portanto, permitindo que um caractere curinga mapa de script para o ASP.NET tem implicações de desempenho.

Aqui está como habilitar um mapa de script curinga para o IIS 7.0:

1. Selecione seu aplicativo na janela Conexões
2. Certifique-se de que o **recursos** é selecionado
3. Clique duas vezes o **mapeamentos de manipulador** botão
4. Clique o **Adicionar mapa de Script curinga** vincular (veja a Figura 3)
5. Insira o caminho para o aspnet\_arquivo isapi.dll (você pode copiar esse caminho de mapa de script PageHandlerFactory)
6. Insira o nome do MVC
7. Clique o **Okey** botão


[![A caixa de diálogo Novo projeto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image3.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.png)

**Figura 3**: criar um mapa de script curinga com o IIS 7.0 ([clique para exibir a imagem em tamanho normal](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image6.png))


Siga estas etapas para criar um mapa de script curinga com o IIS 6.0:

1. Um site com o botão direito e selecione Propriedades
2. Selecione o **diretório base** guia
3. Clique o **configuração** botão
4. Selecione o **mapeamentos** guia
5. Clique o **inserir** botão (consulte a Figura 4)
6. Cole o caminho para o aspnet\_isapi.dll no campo de executável (você pode copiar esse caminho do mapa de script para arquivos. aspx)
7. Desmarque a caixa de seleção **Verifique se o arquivo já existe**
8. Clique o **Okey** botão


[![A caixa de diálogo Novo projeto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image4.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image7.png)

**Figura 4**: criar um mapa de script curinga com o IIS 6.0 ([clique para exibir a imagem em tamanho normal](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image8.png))


Depois de habilitar os mapas de script curinga, você precisará modificar a tabela de rotas no arquivo global. asax para que ele inclua uma rota de raiz. Caso contrário, você obterá a página de erro na Figura 5 quando você faz uma solicitação para a página de raiz do seu aplicativo. Você pode usar o arquivo global. asax modificado na listagem 4.


[![A caixa de diálogo Novo projeto](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image5.jpg)](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image9.png)

**Figura 5**: erro de rota de raiz ausente ([clique para exibir a imagem em tamanho normal](using-asp-net-mvc-with-different-versions-of-iis-vb/_static/image10.png))


**Listagem 4 - global. asax (modificado com a rota de raiz)**

[!code-vb[Main](using-asp-net-mvc-with-different-versions-of-iis-vb/samples/sample4.vb)]

Depois de habilitar um mapa de script curinga para o IIS 7.0 ou IIS 6.0, você pode fazer solicitações que funcionam com a tabela de rotas padrão que ter esta aparência:

/

/ Home/Index

Produto/3/detalhes

/ Produto

## <a name="summary"></a>Resumo

O objetivo deste tutorial era explicar como você pode usar o ASP.NET MVC ao usar uma versão anterior do IIS (ou o IIS 7.0 no modo clássico). Discutimos dois métodos de obtenção de roteamento do ASP.NET para trabalhar com versões anteriores do IIS: modificar a tabela de rota padrão ou criar um mapa de script curinga.

A primeira opção exige que você modifique as URLs usadas em seu aplicativo ASP.NET MVC. Uma vantagem muito importante dessa primeira opção é que você não precisa acesso a um servidor web para modificar a tabela de rotas. Isso significa que você pode usar essa opção primeiro, mesmo ao hospedar seu aplicativo ASP.NET MVC com um Internet da empresa de hospedagem.

A segunda opção é criar um mapa de script curinga. A vantagem dessa opção é que você não precisa modificar suas URLs. A desvantagem dessa opção é que ele pode afetar o desempenho do seu aplicativo ASP.NET MVC.

> [!div class="step-by-step"]
> [Anterior](using-asp-net-mvc-with-different-versions-of-iis-cs.md)
