---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: Diferenças de configuração comuns entre o desenvolvimento e produção (c#) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores implantamos nosso site copiando todos os arquivos relevantes do ambiente de desenvolvimento para o ambiente de produção. No entanto, eu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 2694e0dba774a5bca13b9acc6b14c3e47226a064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="common-configuration-differences-between-development-and-production-c"></a>Diferenças de configuração comuns entre o desenvolvimento e produção (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> Nos tutoriais anteriores implantamos nosso site copiando todos os arquivos relevantes do ambiente de desenvolvimento para o ambiente de produção. No entanto, não é incomum para haver diferenças de configuração entre os ambientes, que exige que cada ambiente tem um arquivo Web. config exclusivo. Este tutorial examina as diferenças de configuração típica e examina estratégias para manter informações de configuração separado.


## <a name="introduction"></a>Introdução


Os dois últimos tutoriais movimentada por meio de implantar um aplicativo web simples. O [ *implantar o Site usando um cliente FTP* ](deploying-your-site-using-an-ftp-client-cs.md) tutorial mostrou como usar um cliente FTP autônomo para copiar os arquivos necessários do ambiente de desenvolvimento até a produção. O tutorial anterior, [ *Implantando seu Site usando o Visual Studio*](deploying-your-site-using-visual-studio-cs.md), pesquisada em implantação usando a ferramenta Copy Web Site e a opção de publicação do Visual Studio. Em ambos os tutoriais de todos os arquivos no ambiente de produção foram uma cópia de um arquivo no ambiente de desenvolvimento. No entanto, não é incomum para arquivos de configuração no ambiente de produção para diferem no ambiente de desenvolvimento. Configuração de um aplicativo web é armazenada no `Web.config` de arquivo e normalmente incluem informações sobre os recursos externos, como banco de dados, web e servidores de email. Ele também indica o comportamento do aplicativo em determinadas situações, como o curso de ação a ser executada quando ocorre uma exceção sem tratamento.

Ao implantar um aplicativo web é importante que as informações de configuração correto acabar no ambiente de produção. Na maioria dos casos o `Web.config` não é possível copiar o arquivo no ambiente de desenvolvimento para o ambiente de produção como-é. Em vez disso, uma versão personalizada do `Web.config` deve ser carregado para a produção. Este tutorial brevemente examina algumas das diferenças mais comuns de configuração; Ele também resume algumas técnicas para manter informações de configuração diferentes entre os ambientes.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Diferenças de configuração comum entre os ambientes de desenvolvimento e produção

O `Web.config` arquivo inclui uma variedade de informações de configuração para um aplicativo ASP.NET. Algumas dessas informações de configuração é o mesmo, independentemente do ambiente. Por exemplo, as configurações de autenticação e regras de autorização de URL descrito de forma a `Web.config` do arquivo `<authentication>` e `<authorization>` elementos geralmente são as mesmas, independentemente do ambiente. Mas outras informações de configuração - como obter informações sobre os recursos externos - normalmente difere dependendo do ambiente.

Cadeias de conexão de banco de dados são um excelente exemplo das informações de configuração diferente com base no ambiente. Quando um aplicativo web se comunica com um servidor de banco de dados pela primeira vez, ele deve estabelecer uma conexão, e isso é conseguido por meio de um [cadeia de caracteres de conexão](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Embora seja possível codificar a cadeia de caracteres de conexão de banco de dados diretamente em páginas da web ou o código que se conecta ao banco de dados, é melhor para colocá-lo `Web.config`do [ `<connectionStrings>` elemento](https://msdn.microsoft.com/library/bf7sd233.aspx) para que a cadeia de caracteres de conexão informações estão em um único local centralizado. Muitas vezes, outro banco de dados é usado durante o desenvolvimento, que é usado na produção. Consequentemente, as informações de cadeia de caracteres de conexão devem ser exclusivas para cada ambiente.

> [!NOTE]
> Tutoriais futuros exploram a implantação de aplicativos orientados a dados, no ponto em que vamos mergulhar em detalhes de como cadeias de conexão de banco de dados são armazenadas no arquivo de configuração.


O comportamento desejado dos ambientes de desenvolvimento e produção difere substancialmente. Um aplicativo web no ambiente de desenvolvimento está sendo criado, testado e depurado por um pequeno grupo de desenvolvedores. No ambiente de produção que mesmo aplicativo que está sendo acessado por vários usuários simultâneos diferentes. O ASP.NET inclui uma série de recursos que ajudam os desenvolvedores em testes e depuração de um aplicativo, mas esses recursos devem ser desabilitados por motivos de segurança no ambiente de produção e de desempenho. Vamos examinar alguns essas definições de configuração.

### <a name="configuration-settings-that-impact-performance"></a>Definições de configuração que afetam o desempenho

Quando uma página for visitada na primeira vez (ou na primeira vez após ele ter sido alterado), sua marcação declarativa deve ser convertida em uma classe e essa classe deve ser compilada. Se o aplicativo web usa compilação automática classe de code-behind da página precisa ser compilado, muito. Você pode configurar uma variedade de opções de compilação por meio de `Web.config` do arquivo [ `<compilation>` elemento](https://msdn.microsoft.com/library/s10awwz0.aspx).

O atributo de depuração é um dos atributos mais importantes no `<compilation>` elemento. Se o `debug` atributo é definido como "true", em seguida, os assemblies compilados incluem símbolos de depuração, que são necessários ao depurar um aplicativo no Visual Studio. Mas símbolos de depuração, aumentam o tamanho do assembly e impõem requisitos de memória adicional quando a execução do código. Além disso, quando o `debug` atributo é definido como "true" qualquer conteúdo retornado por `WebResource.axd` não estão em cache, que significa que cada vez que um usuário visita uma página, eles precisarão baixar novamente o conteúdo estático retornado por `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` é um manipulador de HTTP interna introduzido no ASP.NET 2.0 que controles de servidor usam para recuperar recursos internos, como arquivos de script, imagens, arquivos CSS e outros tipos de conteúdo. Para obter mais informações sobre como `WebResource.axd` funciona e como você pode usá-lo para acessar os recursos inseridos em seus controles de servidor personalizado, consulte [acessando incorporado recursos por meio de uma URL usando `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


O `<compilation>` do elemento `debug` atributo geralmente é definido como "true" no ambiente de desenvolvimento. Na verdade, esse atributo deve ser definido como "true" para depurar um aplicativo da web. Se você tentar depurar um aplicativo ASP.NET no Visual Studio e o `debug` atributo é definido como "false", o Visual Studio exibirá uma mensagem informando que o aplicativo não pode ser depurado até que o `debug` atributo é definido como "true" e irá oferta para fazer essa alteração para você.

Você deve **nunca** tem o `debug` atributo definido como "true" em um ambiente de produção devido ao seu impacto no desempenho. Para obter uma discussão mais completa sobre esse tópico, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da postagem de blog, [não executar produção ASP.NET aplicativos com `debug="true"` habilitado](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Rastreamento e erros personalizados

Quando ocorre uma exceção sem tratamento em um aplicativo ASP.NET-bolhas até o tempo de execução no ponto em que um dos três coisas acontece:

- Será exibida uma mensagem de erro de tempo de execução genérico. Esta página informa ao usuário que há um erro de tempo de execução, mas não fornece detalhes sobre o erro.
- É exibida uma mensagem de detalhes da exceção, que inclui informações sobre a exceção que acabou de ser gerada.
- É exibida uma página de erro personalizada, que é uma página ASP.NET que você criar que exibe uma mensagem que você deseja.

O que acontece no caso de uma exceção sem tratamento depende de `Web.config` do arquivo [ `<customErrors>` seção](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Ao desenvolver e testar um aplicativo ajuda a ver os detalhes de qualquer exceção no navegador. No entanto, mostrar os detalhes da exceção em um aplicativo em produção é um risco à segurança. Além disso, ele é unflattering e faz com que o seu site pareça não profissional. Idealmente, no caso de uma exceção sem tratamento um aplicativo web no ambiente de desenvolvimento mostrará os detalhes da exceção enquanto o mesmo aplicativo em produção mostrará uma página de erro personalizada.

> [!NOTE]
> O padrão `<customErrors>` configuração da seção mostra os detalhes da exceção de mensagem somente quando a página que está sendo acessada por meio de localhost e mostra a página de erro de tempo de execução genérico caso contrário. Isso não é ideal, mas ele é para garantir que para saber que o comportamento padrão não revela detalhes da exceção para os visitantes do local não. Um tutorial futuras examina o `<customErrors>` seção mais detalhadamente e mostra como fazer com que uma página de erro personalizada mostrada quando ocorre um erro em produção.


Rastreamento de outro recurso do ASP.NET que é útil durante o desenvolvimento. O rastreamento, se habilitado, registra as informações sobre cada solicitação de entrada e fornece uma página da web especial, `Trace.axd`, para exibir os detalhes da solicitação recentes. Você pode ativar e configurar o rastreamento por meio de [ `<trace>` elemento](https://msdn.microsoft.com/library/6915t83k.aspx) em `Web.config`.

Se você habilitar o rastreamento Certifique-se que ele está desabilitado em produção. Como as informações de rastreamento incluem cookies, dados de sessão e outras informações potencialmente confidenciais, é importante desabilitar o rastreamento em produção. A boa notícia é que, por padrão, o rastreamento está desabilitado e o `Trace.axd` arquivo só está acessível por meio de localhost. Se você alterar essas configurações padrão em desenvolvimento, certifique-se de que eles estão desativados novamente em produção.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Técnicas para manter informações de configuração separado

Ter diferentes definições de configuração em ambientes de desenvolvimento e produção complica o processo de implantação. Nos dois tutoriais anteriores o processo de implantação envolvidas copiar todos os arquivos necessários de desenvolvimento para produção, mas essa abordagem funciona somente se as informações de configuração serão o mesmo em ambos os ambientes. Há uma variedade de técnicas para implantar um aplicativo com informações de configuração diferentes. Vamos catálogo algumas dessas opções para aplicativos web hospedados.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Implantar manualmente o arquivo de configuração do ambiente de produção

A abordagem mais simples é manter duas versões do `Web.config` arquivos: um para o ambiente de desenvolvimento e um ambiente de produção. Implantar um site em produção envolve copiar todos os arquivos para o servidor de produção no ambiente de desenvolvimento *exceto* para o `Web.config` arquivo. Em vez disso, o ambiente de produção específico `Web.config` arquivo será copiado para a produção.

Essa abordagem não é bastante sofisticada, mas é fácil de implementar, pois as informações de configuração são alterados com frequência. Ele funciona melhor para aplicativos com uma pequena equipe de desenvolvimento que são hospedados em um único servidor web e cujas informações de configuração raramente são alteradas. É mais tenable ao implantar manualmente os arquivos de aplicativo usando um cliente FTP autônomo. Ao usar a ferramenta Copiar Web Site ou a opção de publicação do Visual Studio você precisará primeiro alternar de implantação específicos `Web.config` arquivo pela produção específicos antes de implantar e, em seguida, trocá-los novamente após a conclusão da implantação.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Alterar a configuração durante a compilação ou o processo de implantação

Discussões até o momento tem considerado um processo de compilação e implantação do ad-hoc. Muitos projetos de software maiores têm mais formalizado processos que fazem uso do código-fonte aberto, personalizados, ou ferramentas de terceiros. Para esses projetos provavelmente você pode personalizar o processo de compilação ou implantação para modificar corretamente as informações de configuração antes que ela é enviada para a produção. Se você criar seu aplicativo da web usando [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), ou alguma outra ferramenta de compilação, provavelmente você pode adicionar uma etapa de compilação para modificar o `Web.config` arquivo para incluir as configurações específicas de produção. O fluxo de trabalho de implantação programaticamente possível se conectar ao servidor de controle de origem e recuperar apropriada `Web.config` arquivo.

A abordagem real para obter informações de configuração apropriadas para a produção varia amplamente com base em suas ferramentas e o fluxo de trabalho. Como tal, nós não será aprofundar ainda mais nesse tópico. Se você estiver usando uma ferramenta de compilação populares como MSBuild ou NAnt você encontrará tutoriais e artigos sobre implantação específicas para essas ferramentas por meio de uma pesquisa na web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Diferenças de configuração de gerenciamento por meio de implantação da Web do projeto suplementar

Em 2006, a Microsoft lançou o Add-In de projeto de desenvolvimento de Web do Visual Studio 2005. Um suplemento para Visual Studio 2008 foi lançado em 2008. Este suplemento permite que os desenvolvedores do ASP.NET criar um projeto de implantação da Web separado junto com seu projeto de aplicativo web que, quando criado, explicitamente compila o aplicativo web e copia os arquivos para implantar em um diretório local de saída. O projeto de aplicativo Web usa o MSBuild em segundo plano.

Por padrão, o ambiente de desenvolvimento `Web.config` arquivo é copiado para o diretório de saída, mas você pode configurar o projeto de implantação da Web para personalizar o

informações de configuração que são copiadas para esse diretório das seguintes maneiras:

- Por meio de `Web.config` substituição de seção, em que você especificar para substituir a seção e um arquivo XML que contém o texto de substituição de arquivo.
- Fornecendo um caminho para um arquivo de origem de configuração externo. Com essa opção selecionada, o projeto de implantação da Web copia um determinado `Web.config` arquivo para o diretório de saída (em vez de `Web.config` arquivo usado no ambiente de desenvolvimento).
- Adicionando regras personalizadas para o arquivo de MSBuild usado pelo projeto de implantação da Web.

Para implantar a compilação do aplicativo web do projeto de implantação da Web e, em seguida, copie os arquivos da pasta de saída do projeto para o ambiente de produção.

Para saber mais sobre como usar o projeto de implantação da Web check-out [neste artigo projetos de implantação da Web](https://msdn.microsoft.com/magazine/cc163448.aspx) da edição de abril de 2007 da [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), ou consulte os links na seção de leitura adicional no fim deste tutorial.

> [!NOTE]
> Você não pode usar o projeto de implantação da Web com o Visual Web Developer porque o projeto de implantação da Web é implementado como um Visual Studio Add-In e o Visual Studio Express Editions (incluindo o Visual Web Developer) não dão suporte a suplementos.


## <a name="summary"></a>Resumo

O comportamento de um aplicativo web no desenvolvimento e recursos externos são normalmente diferentes quando o mesmo aplicativo estiver em produção. Por exemplo, cadeias de conexão de banco de dados, opções de compilação e o comportamento quando uma exceção sem tratamento ocorre normalmente diferem entre ambientes. O processo de implantação deve acomodar essas diferenças. Conforme abordado neste tutorial, a abordagem mais simples é copiar manualmente um arquivo de configuração alternativa para o ambiente de produção. As soluções mais elegantes são possíveis ao usar o Add-In de projeto de implantação de Web ou com um processo de compilação ou implantação mais formal que pode acomodar essas personalizações.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Cadeias de caracteres de Conexão explicadas](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [@ ConnectionStrings.com de cadeias de caracteres de Conexão de banco de dados](http://www.connectionstrings.com/)
- [Não execute aplicativos ASP.NET de produção com `debug="true"` habilitado](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Respondendo normalmente para exceções não tratadas - exibindo páginas de erro amigável](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Como fazer: usar um projeto de implantação de Web do Visual Studio 2008?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Configurações de chave de configuração ao implantar um banco de dados](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Download de projetos do Visual Studio 2008 Web implantação](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Download de projetos do Visual Studio 2005 Web Deployment](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Projetos de implantação da Web do VS 2008](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [suporte de projeto de implantação do VS 2008 Web lançado](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Projetos de Implantação da Web](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-your-site-using-visual-studio-cs.md)
> [Próximo](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
