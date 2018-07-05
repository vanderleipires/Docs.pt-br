---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
title: Diferenças de configuração comuns entre desenvolvimento e produção (VB) | Microsoft Docs
author: rick-anderson
description: Nos tutoriais anteriores, implantamos o nosso site, copiando todos os arquivos pertinentes do ambiente de desenvolvimento para o ambiente de produção. No entanto, eu...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 548e75f6-4d6c-4cb4-8da8-417915eb8393
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
msc.type: authoredcontent
ms.openlocfilehash: 083c07a42fab1f655798f8cfb444ed0e6aa38ff0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842799"
---
<a name="common-configuration-differences-between-development-and-production-vb"></a>Diferenças de configuração comuns entre desenvolvimento e produção (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_vb.pdf)

> Nos tutoriais anteriores, implantamos o nosso site, copiando todos os arquivos pertinentes do ambiente de desenvolvimento para o ambiente de produção. No entanto, não é incomum para haver diferenças de configuração entre ambientes, o que exige que cada ambiente tem um arquivo Web. config exclusivo. Este tutorial examina as diferenças de configuração típico e examina estratégias para manter informações de configuração separado.


## <a name="introduction"></a>Introdução


Os dois últimos tutoriais percorreu Implantando um aplicativo web simples. O [ *Implantando o Site usando um cliente FTP* ](deploying-your-site-using-an-ftp-client-vb.md) tutorial mostrou como usar um cliente autônomo do FTP para copiar os arquivos necessários do ambiente de desenvolvimento até a produção. O tutorial anterior, [ *Implantando seu Site usando o Visual Studio*](deploying-your-site-using-visual-studio-vb.md), analisou a implantação usando a ferramenta Copy Web Site e a opção de publicação do Visual Studio. Em ambos os tutoriais de todos os arquivos no ambiente de produção foram uma cópia de um arquivo no ambiente de desenvolvimento. No entanto, não é incomum para arquivos de configuração no ambiente de produção para ser diferentes no ambiente de desenvolvimento. Configuração de um aplicativo web é armazenada no `Web.config` de arquivo e geralmente inclui informações sobre os recursos externos, como o banco de dados, web e servidores de email. Ele também indica o comportamento do aplicativo em determinadas situações, como o curso de ação a ser executada quando ocorre uma exceção sem tratamento.

Ao implantar um aplicativo web é importante que as informações de configuração correto acabar no ambiente de produção. Na maioria dos casos o `Web.config` arquivo no ambiente de desenvolvimento não pode ser copiado para o ambiente de produção como-está. Em vez disso, uma versão personalizada do `Web.config` precisa ser carregado para a produção. Este tutorial analisa brevemente algumas das diferenças mais comuns de configuração; Ele também resume algumas técnicas para manter informações de configuração diferentes entre os ambientes.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Configuração típica de diferenças entre os ambientes de desenvolvimento e produção

O `Web.config` arquivo contém uma variedade de informações de configuração para um aplicativo ASP.NET. Algumas dessas informações de configuração é o mesmo, independentemente do ambiente. Por exemplo, as configurações de autenticação e as regras de autorização de URL indicados claramente na `Web.config` do arquivo `<authentication>` e `<authorization>` elementos geralmente são os mesmos, independentemente do ambiente. Mas outras informações de configuração - tais como informações sobre os recursos externos - normalmente varia dependendo do ambiente.

Cadeias de conexão de banco de dados são um excelente exemplo de informações de configuração que é diferente com base no ambiente. Quando um aplicativo web se comunica com um servidor de banco de dados pela primeira vez, ele deve estabelecer uma conexão, e isso é conseguido por meio de um [cadeia de caracteres de conexão](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Embora seja possível codificar a cadeia de caracteres de conexão de banco de dados diretamente em páginas da web ou o código que se conecta ao banco de dados, é melhor colocá-lo `Web.config`do [ `<connectionStrings>` elemento](https://msdn.microsoft.com/library/bf7sd233.aspx) , de modo que a cadeia de caracteres de conexão informações estão em um único local centralizado. Muitas vezes, outro banco de dados é usado durante o desenvolvimento, que é usado em produção. Consequentemente, as informações de cadeia de caracteres de conexão devem ser exclusivas para cada ambiente.

> [!NOTE]
> Tutoriais futuros exploram a implantação de aplicativos controlados por dados, no ponto em que vamos nos aprofundar nas especificações de como cadeias de conexão de banco de dados são armazenadas no arquivo de configuração.


O comportamento desejado dos ambientes de desenvolvimento e produção difere substancialmente. Um aplicativo web no ambiente de desenvolvimento está sendo criado, testado e depurado por um pequeno grupo de desenvolvedores. No ambiente de produção que o mesmo aplicativo está sendo visitado por muitos usuários simultâneos diferentes. O ASP.NET inclui uma série de recursos que ajudam os desenvolvedores em testes e depuração de um aplicativo, mas esses recursos devem ser desabilitados por motivos de segurança quando no ambiente de produção e de desempenho. Vamos examinar alguns essas definições de configuração.

### <a name="configuration-settings-that-impact-performance"></a>Definições de configuração que afetam o desempenho

Quando uma página ASP.NET é visitada para na primeira vez (ou na primeira vez após ele ter sido alterado), sua marcação declarativa deve ser convertida em uma classe e essa classe deve ser compilada. Se o aplicativo web usa compilação automática classe de code-behind da página precisa ser compilado, muito. Você pode configurar uma variedade de opções de compilação por meio de `Web.config` do arquivo [ `<compilation>` elemento](https://msdn.microsoft.com/library/s10awwz0.aspx).

O atributo é um dos atributos mais importantes no `<compilation>` elemento. Se o `debug` atributo é definido como "true", em seguida, os assemblies compilados incluem símbolos de depuração, que são necessários ao depurar um aplicativo no Visual Studio. Mas aumentam o tamanho do conjunto de símbolos de depuração e impõem requisitos de memória adicional ao executar o código. Além disso, quando o `debug` atributo é definido como "true" qualquer conteúdo retornado por `WebResource.axd` não está em cache, que significa que cada vez que um usuário visita uma página que eles precisarão baixar novamente o conteúdo estático retornado por `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` é um manipulador HTTP interno introduzido no ASP.NET 2.0 que controles de servidor usam para recuperar recursos inseridos, como arquivos de script, imagens, arquivos CSS e outros tipos de conteúdo. Para obter mais informações sobre como `WebResource.axd` funciona e como você pode usá-lo para acessar os recursos inseridos em seus controles de servidor personalizado, consulte [acessando Embedded recursos por meio de uma URL usando `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


O `<compilation>` do elemento `debug` atributo geralmente é definido como "true" no ambiente de desenvolvimento. Na verdade, esse atributo deve ser definido como "true" para depurar um aplicativo web; Se você tentar depurar um aplicativo ASP.NET do Visual Studio e o `debug` atributo é definido como "false", o Visual Studio exibirá uma mensagem explicando que o aplicativo não pode ser depurado até que o `debug` atributo é definido como "true" e serão a oferta para fazer essa alteração para você.

Você deve **nunca** têm o `debug` atributo definido como "true" em um ambiente de produção devido ao seu impacto no desempenho. Para obter uma discussão mais completa sobre esse tópico, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)da postagem de blog [não executar produção ASP.NET aplicativos com `debug="true"` habilitado](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Rastreamento e erros personalizados

Quando uma exceção não tratada ocorre em um aplicativo ASP.NET são exibidos até o tempo de execução no ponto em que uma das três coisas que acontece:

- Será exibida uma mensagem de erro de tempo de execução genérico. Esta página informa ao usuário que há um erro de tempo de execução, mas não fornece todos os detalhes sobre o erro.
- É exibida uma mensagem de detalhes da exceção, que inclui informações sobre a exceção que foi gerada apenas.
- Uma página de erro personalizada é exibida, que é uma página ASP.NET que você criar que exiba qualquer mensagem que você deseja.

O que acontece em caso de uma exceção sem tratamento depende de `Web.config` do arquivo [ `<customErrors>` seção](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Ao desenvolver e testar um aplicativo que ajuda a ver os detalhes de qualquer exceção no navegador. No entanto, mostrar detalhes da exceção em um aplicativo em produção é um risco de segurança. Além disso, ele é unflattering e faz com que o seu site com aparência profissional. O ideal é que, no caso de uma exceção sem tratamento um aplicativo web no ambiente de desenvolvimento mostrará os detalhes da exceção enquanto o mesmo aplicativo em produção mostrará uma página de erro personalizada.

> [!NOTE]
> O padrão `<customErrors>` configuração da seção mostra os detalhes da exceção de mensagem somente quando a página está sendo visitada por meio de localhost e mostra a página de erro de tempo de execução genérico caso contrário. Isso não é ideal, mas ele está garantindo para saber em que o comportamento padrão não revela detalhes da exceção para visitantes de não-local. Um tutorial futuro examina o `<customErrors>` seção mais detalhadamente e mostra como fazer com que uma página de erro personalizada mostrada quando ocorre um erro em produção.


Rastreamento de outro recurso do ASP.NET que é útil durante o desenvolvimento. Rastreamento, se habilitada, registra as informações sobre cada solicitação de entrada e fornece uma página da web especial, `Trace.axd`, para exibir os detalhes da solicitação recente. Você pode ativar e configurar o rastreamento por meio de [ `<trace>` elemento](https://msdn.microsoft.com/library/6915t83k.aspx) em `Web.config`.

Se você habilitar o rastreamento Certifique-se de que ele está desabilitado na produção. Como as informações de rastreamento incluem cookies, dados de sessão e outras informações potencialmente confidenciais, é importante desabilitar o rastreamento em produção. A boa notícia é que, por padrão, o rastreamento está desabilitado e o `Trace.axd` arquivo só é acessível por meio de localhost. Se você alterar essas configurações padrão em desenvolvimento, certifique-se de que eles estão desativados volta em produção.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Técnicas para manter informações de configuração separado

Ter diferentes definições de configuração em ambientes de desenvolvimento e produção complica o processo de implantação. Nos tutoriais anteriores dois o processo de implantação envolvidos copiando todos os arquivos necessários do desenvolvimento à produção, mas essa abordagem funciona apenas se as informações de configuração são o mesmo em ambos os ambientes. Há uma variedade de técnicas para implantar um aplicativo com informações de configuração diferentes. Vamos catálogo algumas dessas opções para aplicativos web hospedados.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Implantar manualmente o arquivo de configuração do ambiente de produção

A abordagem mais simples é manter duas versões do `Web.config` arquivo: uma para o ambiente de desenvolvimento e outra para o ambiente de produção. Implantar um site para a produção envolve copiar todos os arquivos para o servidor de produção no ambiente de desenvolvimento *exceto* para o `Web.config` arquivo. Em vez disso, o ambiente de produção específico `Web.config` arquivo seria copiado para a produção.

Essa abordagem não é muito sofisticada, mas é fácil de implementar, porque as informações de configuração mudam com pouca frequência. Ele funciona melhor para aplicativos com uma pequena equipe de desenvolvimento que são hospedados em um único servidor web e cujas informações de configuração são alteradas com pouca frequência. É mais tenable ao implantar manualmente os arquivos do aplicativo usando um cliente FTP autônomo. Ao usar a ferramenta Copy Web Site ou a opção de publicação do Visual Studio você precisará primeiro trocar de implantação específicos `Web.config` de arquivos com aquele específicos de produção antes de implantar e, em seguida, trocá-los novamente após a conclusão da implantação.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Alterar a configuração durante a compilação ou o processo de implantação

As discussões até agora tem presume-se um processo de compilação e implantação do ad-hoc. Muitos projetos de software maiores formalizou mais processos que tornam o uso do código-fonte aberto, personalizados, ou ferramentas de terceiros. Para esses projetos, você provavelmente pode personalizar o processo de compilação ou implantação para modificar adequadamente as informações de configuração antes de ser enviada para a produção. Se você compilar seu aplicativo web usando [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), ou alguma outra ferramenta de compilação, provavelmente você pode adicionar uma etapa de compilação para modificar o `Web.config` arquivo para incluir as configurações específicas de produção. Ou seu fluxo de trabalho de implantação por meio de programação pode se conectar ao servidor de controle de origem e recuperar apropriado `Web.config` arquivo.

A abordagem real para obter as informações de configuração apropriado para a produção varia amplamente com base em suas ferramentas e fluxo de trabalho. Dessa forma, podemos não irá aprofundar nesse tópico ainda mais. Se você estiver usando uma ferramenta de compilação populares, como o MSBuild ou NAnt você encontrará tutoriais e artigos sobre implantação específicas para essas ferramentas por meio de uma pesquisa na web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Diferenças de configuração de gerenciamento por meio de implantação da Web de projeto suplementar

Em 2006 Microsoft lançou o Add-In de projeto de desenvolvimento de Web do Visual Studio 2005. Um suplemento do Visual Studio 2008 foi lançado em 2008. Esse suplemento permite que os desenvolvedores do ASP.NET criar um projeto de implantação da Web separada junto com seu projeto de aplicativo web que, quando criado, explicitamente compila o aplicativo web e copia os arquivos para implantar em um diretório de saída local. O projeto de aplicativo Web usa o MSBuild em segundo plano.

Por padrão, o ambiente de desenvolvimento `Web.config` arquivo é copiado para o diretório de saída, mas você pode configurar o projeto de implantação da Web para personalizar o

informações de configuração que são copiadas para esse diretório das seguintes maneiras:

- Por meio de `Web.config` de substituição de seção, em que você especifique a seção para substituir e um arquivo XML que contém o texto de substituição de arquivo.
- Fornecendo um caminho para um arquivo de origem de configuração externo. Com essa opção selecionada, o projeto de implantação da Web copia um determinado `Web.config` arquivo para o diretório de saída (em vez de `Web.config` arquivo usado no ambiente de desenvolvimento).
- Adicionando regras personalizadas para o arquivo do MSBuild usado pelo projeto de implantação da Web.

Para implantar a compilação do aplicativo web do projeto de implantação da Web e, em seguida, copie os arquivos da pasta de saída do projeto para o ambiente de produção.

Para saber mais sobre como usar o projeto de implantação da Web, confira [deste artigo Web Deployment Projects](https://msdn.microsoft.com/magazine/cc163448.aspx) da edição de abril de 2007 da [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), ou consulte os links na seção leitura adicional no fim deste tutorial.

> [!NOTE]
> Você não pode usar o projeto de implantação da Web com o Visual Web Developer, porque o projeto de implantação da Web é implementado como um Visual Studio Add-In e o Visual Studio Express Editions (incluindo o Visual Web Developer) não dão suporte a suplementos.


## <a name="summary"></a>Resumo

O comportamento de um aplicativo web em desenvolvimento e recursos externos são normalmente é diferentes de quando o mesmo aplicativo está em produção. Por exemplo, cadeias de conexão de banco de dados, opções de compilação e o comportamento quando uma exceção sem tratamento ocorre normalmente são diferentes entre ambientes. O processo de implantação deve acomodar essas diferenças. Conforme discutimos neste tutorial, a abordagem mais simples é copiar manualmente um arquivo de configuração alternativa para o ambiente de produção. Soluções mais apuradas são possíveis ao usar o suplemento da Web Deployment Project ou com um processo de compilação ou implantação mais formal que pode acomodar tais personalizações.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Cadeias de caracteres de Conexão explicadas](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [ConnectionStrings.com @ cadeias de caracteres de Conexão de banco de dados](http://www.connectionstrings.com/)
- [Não execute aplicativos do ASP.NET de produção com `debug="true"` habilitado](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Normalmente, responder a exceções sem tratamento - exibir páginas de erro amigável](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Como fazer: usar um projeto de implantação de Web do Visual Studio 2008?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Configurações de chave de configuração ao implantar um banco de dados](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Download de projetos do Visual Studio 2008 Web Deployment](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Download de projetos do Visual Studio 2005 Web Deployment](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [VS 2008 Web Deployment Projects](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [suporte de projeto de implantação do VS 2008 Web lançado](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Projetos de Implantação da Web](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-your-site-using-visual-studio-vb.md)
> [Próximo](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
