---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Determinar quais arquivos precisam ser implantados (c#) | Microsoft Docs
author: rick-anderson
description: Quais arquivos precisam ser implantados no ambiente de desenvolvimento para o ambiente de produção depende em parte se o aplicativo ASP.NET foi criado nos...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: ff5f1d7d156efa12d97382db56211a07c43178fd
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="determining-what-files-need-to-be-deployed-c"></a>Determinar quais arquivos precisam ser implantados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Quais arquivos precisam ser implantados no ambiente de desenvolvimento para o ambiente de produção depende em parte se o aplicativo ASP.NET foi criado usando o modelo de Site da Web ou o modelo de aplicativo Web. Saiba mais sobre esses dois modelos de projeto e como o modelo de projeto afeta a implantação.


## <a name="introduction"></a>Introdução

Implantar um aplicativo web ASP.NET envolve copiar os arquivos relacionados ao ASP.NET do ambiente de desenvolvimento para o ambiente de produção. Os arquivos relacionados ao ASP.NET incluem marcação de página da web do ASP.NET e arquivos de suporte de código e o cliente e servidor. Arquivos de suporte do lado do cliente são os arquivos referenciados por suas páginas da web e enviadas diretamente para o navegador - imagens, CSS e JavaScript, por exemplo. Arquivos de suporte do servidor incluem aqueles que são usados para processar uma solicitação no lado do servidor. Isso inclui arquivos de configuração, serviços web, arquivos de classe, DataSets tipados e LINQ para arquivos do SQL, entre outros.

Em geral, todos os arquivos de suporte do lado do cliente devem ser copiados no ambiente de desenvolvimento para o ambiente de produção, mas os arquivos de suporte do servidor sejam copiados depende se você estiver compilando explicitamente o código do lado do servidor em um assembly (um `.dll` arquivo) ou se você estiver tendo esses assemblies gerados automaticamente. Este tutorial realça quais arquivos precisam ser implantados quando explicitamente Compilando o código em um assembly em vez de ter que essa etapa de compilação ocorrem automaticamente.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Compilação explícita Versus compilação automática

Páginas da web ASP.NET são divididas em declarativo marcação e código-fonte. A parte de marcação declarativa inclui HTML, controles da Web e sintaxe de associação de dados. a parte de código contém manipuladores de eventos escritos em código do Visual Basic ou c#. As partes de marcação e código normalmente são separadas em arquivos diferentes: `WebPage.aspx` contém a marcação declarativa ao `WebPage.aspx.cs` hospeda o código.

Considere uma página ASP.NET chamada Clock.aspx que contém um controle de rótulo cuja propriedade de texto é definida como a data e hora atual quando a página for carregada. A parte de marcação declarativa (em `Clock.aspx`) conteria a marcação para um controle de rótulo Web -`<asp:Label runat="server" id="TimeLabel" />` - durante a parte de código (em `Clock.aspx.cs`) teria um `Page_Load` manipulador de eventos com o código a seguir:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

Para que o mecanismo do ASP.NET atender uma solicitação para essa página, parte do código da página (o `WebPage.aspx.cs` arquivo) devem ser compilados primeiro. Esta compilação pode acontecer automaticamente ou explicitamente.

Se a compilação acontece explicitamente, o código-fonte do aplicativo inteiro é compilado em um ou mais assemblies (`.dll` arquivos) localizado no aplicativo de `Bin` directory. Se a compilação ocorre automaticamente e resultante gerada automaticamente é assembly, por padrão, colocado no `Temporary ASP.NET` pasta de arquivos, que pode ser encontrada em `%WINDOWS%\Microsoft.NET\Framework\`  *&lt;versão&gt;*, Embora esse local é configurável por meio de [ `<compilation>` elemento](https://msdn.microsoft.com/library/s10awwz0.aspx) em `Web.config`. Com a compilação explícita você deve executar alguma ação para compilar o código do aplicativo ASP.NET em um assembly, e esta etapa ocorre antes da implantação. Com a compilação automática o processo de compilação ocorre no servidor web quando o recurso é acessado pela primeira vez.

Independentemente de qual modelo de compilação que você usar, a parte de marcação de todas as páginas ASP.NET (o `WebPage.aspx` arquivos) precisam ser copiados para o ambiente de produção. Com a compilação explícita, você precisa copiar os assemblies no `Bin` pasta, mas você não precisa copiar a partes do código das páginas do ASP.NET (o `WebPage.aspx.cs` arquivos). Com a compilação automática, você precisa copiar os arquivos de parte do código para que o código está presente e pode ser compilado automaticamente quando a página for visitada. A parte de marcação de cada página da web inclui um `@Page` diretiva com atributos que indicam se o código da página associado explicitamente já foi compilado, ou se ele precisa ser automaticamente compilado. Como resultado, o ambiente de produção pode funcionar perfeitamente com o modelo de compilação e você não precisa aplicar as configurações de qualquer configuração especial para indicar que a compilação automática ou explícita é usada.

A tabela 1 resume os diferentes arquivos implantar ao usar a compilação explícita versus compilação automática. Observe que, independentemente da compilação de modelo usada você sempre deve implantar assemblies no `Bin` pasta, se essa pasta existe. O `Bin` pasta contém os assemblies específicos ao aplicativo web, que incluem o código-fonte compilado usando o modelo de compilação explícita. O `Bin` diretório também contém assemblies de outros projetos e qualquer assemblies de código-fonte aberto ou de terceiros, talvez você esteja usando, e ele deve estar no servidor de produção. Portanto, como um regra geral, copie o `Bin` pasta até durante a implantação de produção. (Se você estiver usando o modelo de compilação automática e não estiver usando qualquer assembly externo, você não terá um `Bin` diretório - Okey!)

| **Modelo de compilação** | **Implantar o arquivo de parte de marcação?** | **Implantar o arquivo de código fonte?** | **Implantar Assemblies em `Bin` diretório?** |
| --- | --- | --- | --- |
| Compilação explícita | Sim | Não | Sim |
| Compilação automática | Sim | Sim | Sim (se houver) |

**Tabela 1:** arquivos que você implantar depende do modelo de compilação usado.

## <a name="taking-a-trip-down-memory-lane"></a>Fazer uma viagem pista de memória

O método de compilação é usado depende, em parte, como o aplicativo ASP.NET é gerenciado no Visual Studio. Desde. Início do NET no ano 2000 foram quatro versões diferentes do Visual Studio - Visual Studio .NET 2002, o Visual Studio .NET 2003, o Visual Studio 2005 e o Visual Studio 2008. O Visual Studio .NET 2002 e 2003 gerenciados aplicativos ASP.NET usando o *modelo de projeto de aplicativo Web*. São os principais recursos do modelo de projeto de aplicativo Web:

- Os arquivos que a criação do projeto são definidos em um único arquivo de projeto. Todos os arquivos não está definidos no arquivo de projeto não são considerados parte do aplicativo web pelo Visual Studio.
- Usa a compilação explícita. Compilar o projeto compila os arquivos de código dentro do projeto em um único assembly que é colocado no `Bin` pasta.

Quando a Microsoft lançou o Visual Studio 2005, eles removido o suporte para o modelo de projeto de aplicativo Web e substituído com o modelo de projeto de Site. O modelo de projeto de Site em si diferenciados do modelo de projeto de aplicativo Web das seguintes maneiras:

- Em vez de um arquivo de projeto único que explica os arquivos do projeto, o sistema de arquivos é usado em vez disso. Em resumo, todos os arquivos dentro da pasta de aplicativo da web (ou subpastas) são considerados parte do projeto.
- Compilando um projeto no Visual Studio não cria um assembly no `Bin` directory. Em vez disso, a Compilando um projeto de Site da Web relata os erros de tempo de compilação.
- Suporte para compilação automática. Projetos de Site normalmente são implantados por meio de cópia o marcação e código-fonte para o ambiente de produção, embora o código pode ser pré-compilados (compilação explícita).

Microsoft reativada o modelo de projeto de aplicativo Web quando lançado Visual Studio 2005 Service Pack 1. No entanto, o Visual Web Developer continua a suportam apenas o modelo de projeto de Site. A boa notícia é que essa limitação foi removida com o Visual Web Developer 2008 Service Pack 1. Atualmente, você pode criar aplicativos ASP.NET no Visual Studio (e o Visual Web Developer) usando o modelo de projeto de aplicativo Web ou o modelo de projeto de Site. Os dois modelos têm seus prós e contras. Consulte [Introdução aos projetos de aplicativo Web: comparando projetos de Site da Web e projetos de aplicativo Web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) para obter uma comparação dos dois modelos e para ajudar a decidir qual modelo de projeto funciona melhor para sua situação.

## <a name="exploring-the-sample-web-application"></a>Explorando o aplicativo Web de exemplo

O download para este tutorial inclui um aplicativo ASP.NET chamado revisões de livros. O site imita a um site hobbies alguém pode criar compartilhar seu livro analisa com a comunidade online. Este aplicativo web do ASP.NET é muito simple e consiste dos seguintes recursos:

- `Web.config`, o arquivo de configuração do aplicativo.
- Uma página mestra (`Site.master`).
- Sete páginas ASP.NET diferentes: 

    - ~`/Default.aspx`-homepage do site.
    - ~`/About.aspx` -uma página de "Sobre o Site".
    - ~`/Fiction/Default.aspx` -uma página listando os livros de ficção foram revisados. 

        - ~`/Fiction/Blaze.aspx` -uma revisão do livro Richard Bachman *Blaze*.
    - ~/`Tech/Default.aspx` -uma página listando os livros de tecnologia que foram examinados. 

        - ~/`Tech/CYOW.aspx`-uma revisão do *criar seu próprio site*.
        - ~/`Tech/TYASP35.aspx` -uma revisão do *ensinar por conta própria ASP.NET 3.5 nas 24 horas*.
- Três arquivos CSS diferentes na pasta estilos.
- Quatro arquivos de imagem - funciona com o logotipo do ASP.NET e as imagens das tampas dos livros revisadas três - todos localizados no `Images` pasta.
- Um `Web.sitemap` arquivo, que define o mapa de site e é usado para exibir menus de `Default.aspx` páginas no diretório raiz e `Fiction` e `Tech` pastas.
- Um arquivo de classe chamado `BasePage.cs` que define uma base de `Page` classe. Essa classe estende a funcionalidade do `Page` classe definindo automaticamente o `Title` propriedade com base na posição da página no mapa do site. Resumindo, qualquer classe de code-behind do ASP.NET que estende `BasePage` (em vez de `System.Web.UI.Page`) terá seu título definido como um valor de acordo com sua posição no mapa do site. Por exemplo, ao exibir o ~ /`Tech/CYOW.aspx` página, o título é definido como "Início: tecnologia: criar seu próprio site".

A Figura 1 mostra uma captura de tela do site revisões de livros quando visualizada através de um navegador. Veja a página ~ /`Tech/TYASP35.aspx`, que analisa o catálogo *ensinar por conta própria ASP.NET 3.5 nas 24 horas*. A trilha de navegação que abrange a parte superior da página e no menu na coluna esquerda são baseados na estrutura de mapa de site definida no `Web.sitemap`. A imagem no canto direito superior é uma da capa do livro imagens localizadas no `Images` pasta. Aparência do site são definidos por meio de regras de folha de estilo escritas pelos arquivos CSS na pasta estilos, enquanto o layout da página geral é definido na página mestra, em cascata `Site.master`.


[![O site do catálogo revisa oferece análises em uma variedade de títulos](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Figura 1:** o site de catálogo revisa oferece análises em uma variedade de títulos ([clique para exibir a imagem em tamanho normal](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


Este aplicativo não usar um banco de dados; cada revisão é implementado como uma página da web separada no aplicativo. Este tutorial (e os próximos vários tutoriais) percorrer Implantando um aplicativo web que não tem um banco de dados. No entanto, em um tutorial futuras podemos melhorar este aplicativo para armazenar revisões, comentários dos leitores e outras informações no banco de dados e explorar quais etapas precisam ser executadas para implantar corretamente um aplicativo web controladas por dados.

> [!NOTE]
> Esses tutoriais se concentrar em aplicativos ASP.NET com um provedor de host da web de hospedagem e não explorar auxiliares tópicos como ASP. Sistema de mapa de site da rede ou usando uma base de `Page` classe. Para obter mais informações sobre essas tecnologias e para obter mais informações sobre outros tópicos abordados em todo o tutorial, consulte a seção de leitura adicional no final de cada tutorial.


Download deste tutorial tem duas cópias do aplicativo web, cada um implementado como um tipo diferente de projeto do Visual Studio: BookReviewsWAP, um projeto de aplicativo Web e BookReviewsWSP, um projeto de Site da Web. Ambos os projetos foram criados com o Visual Web Developer 2008 SP1 e usam o ASP.NET 3.5 SP1. Para trabalhar com esses projetos iniciar ao descompactar o conteúdo em sua área de trabalho. Para abrir o projeto de aplicativo da Web (BookReviewsWAP), navegue até a pasta BookReviewsWAP e clique duas vezes no arquivo de solução, `BookReviewsWAP.sln`. Para abrir o projeto de Site da Web (BookReviewsWSP), inicie o Visual Studio e, em seguida, no menu Arquivo, escolha a opção de abrir o Site da Web, navegue até o `BookReviewsWSP` pasta na área de trabalho e clique em Okey.


As duas seções restantes neste tutorial examinar quais arquivos você precisará copiar para o ambiente de produção ao implantar o aplicativo. Os próximos dois tutoriais - *[Implantando seu Site usando FTP](deploying-your-site-using-an-ftp-client-cs.md)* e *[implantando seu Site usando o Visual Studio](deploying-your-site-using-visual-studio-cs.md)* -mostram maneiras diferentes de Copie esses arquivos para um provedor de host da web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Determinando os arquivos para implantar o projeto de aplicativo Web

O modelo de projeto de aplicativo Web usa a compilação explícita - código-fonte do projeto é compilado em um único assembly sempre que você compila o aplicativo. Esta compilação inclui arquivos de code-behind as páginas do ASP.NET (~ /`Default.aspx.cs`, ~ /`About.aspx.cs`, e assim por diante), bem como a `BasePage.cs` classe. O assembly resultante é chamado BookReviewsWAP.dll e está localizado no aplicativo de `Bin` directory.

A Figura 2 mostra os arquivos que compõem o projeto de aplicativo Web do catálogo revisões.


[![O Gerenciador de soluções lista os arquivos que compõem o projeto de aplicativo Web](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Figura 2**: O Gerenciador de soluções lista os arquivos que compõem o projeto de aplicativo Web


Para implantar um aplicativo ASP.NET desenvolvido usando o início do modelo de projeto de aplicativo Web, criando o aplicativo para compilar o código-fonte mais recente explicitamente em um assembly. Em seguida, copie os seguintes arquivos para o ambiente de produção:

- Página, os arquivos que contêm a marcação declarativa para cada ASP.NET, como ~ /`Default.aspx`, ~ /`About.aspx`, e assim por diante. Além disso, cópia de backup a marcação declarativa para quaisquer páginas mestras e controles de usuário.
- Os assemblies (`.dll` arquivos) no `Bin` pasta. Você não precisa copiar os arquivos de banco de dados do programa (`.pdb`) ou arquivos XML, talvez você encontre o `Bin` directory.

Você não precisa copiar os arquivos de código fonte das páginas do ASP.NET para o ambiente de produção, nem você precisa copiar o `BasePage.cs` arquivo de classe.

> [!NOTE]
> Como mostra a Figura 2, o `BasePage` classe é implementada como um arquivo de classe no projeto, colocado na pasta chamada `HelperClasses`. Quando o projeto é compilado o código de `BasePage.cs` arquivo é compilado com classes de code-behind as páginas do ASP.NET em um único assembly, `BookReviewsWAP.dll.` ASP.NET tem uma pasta especial denominada `App_Code` que foi projetado para armazenar arquivos de classe para Web Projetos de site. O código de `App_Code` pasta será compilada automaticamente e, portanto, não deve ser usada com projetos de aplicativo Web. Em vez disso, você deve colocar os arquivos de classe do aplicativo em uma pasta normal denominada `HelperClasses`, ou `Classes`, ou algo semelhante. Como alternativa, você pode colocar arquivos de classe em um projeto de biblioteca de classes separado.


Além de copiar os arquivos relacionados ao ASP.NET marcação e o assembly no `Bin` pasta, você também precisará copiar os arquivos de suporte do lado do cliente (imagens e arquivos CSS), bem como os outros arquivos de suporte do servidor, `Web.config` e `Web.sitemap`. Esse lado de cliente e de servidor oferecer suporte a necessidade de arquivos a serem copiados para o ambiente de produção, independentemente de você usar compilação automática ou explícita.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Determinando os arquivos para implantar os arquivos de projeto de Site da Web

O modelo de projeto de Site oferece suporte à compilação automática, um recurso não está disponível ao usar o modelo de projeto de aplicativo Web. Com a compilação explícita, você deve compilar o código-fonte do seu projeto em um assembly e copiar o assembly para o ambiente de produção. Por outro lado, com a compilação automática você simplesmente copiar o código-fonte para o ambiente de produção e ele é compilado em tempo de execução sob demanda, conforme necessário.

A opção de menu Build no Visual Studio está presente em projetos de aplicativos Web e projetos de Site. Criar um projeto de aplicativo Web compila o código-fonte do projeto em um único assembly localizado no `Bin` diretório; criando um projeto Web verifica os erros de tempo de compilação, mas não cria todos os assemblies. Para implantar um aplicativo ASP.NET desenvolvido usando o modelo de projeto de Site todos que você precisa fazer é copiar os arquivos apropriados para o ambiente de produção, mas eu recomendo que a primeira compilação do projeto para garantir que não há nenhum erro de tempo de compilação.

A Figura 3 mostra os arquivos que compõem o projeto de Site da Web do catálogo de revisões.


 [![O Gerenciador de soluções lista os arquivos que compõem o projeto de Site da Web](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Figura 3**: O Gerenciador de soluções lista os arquivos que compõem o projeto de Site da Web


Implantando um projeto de Site da Web envolve a cópia de todos os arquivos relacionados ao ASP.NET para o ambiente de produção - que inclui as páginas de marcação para páginas ASP.NET, páginas mestras e controles de usuário, juntamente com seus arquivos de código. Você também precisa copiar quaisquer arquivos de classe, como BasePage.cs. Observe que o `BasePage.cs` arquivo está localizado no `App_Code` pasta, que é uma pasta ASP.NET especial usada em projetos de Site da Web para arquivos de classe. A pasta especial precisa ser criado em produção, também, como os arquivos de classe no `App_Code` pasta no ambiente de desenvolvimento deve ser copiada para o `App_Code` pasta em produção.

Além de copiar os arquivos de código de marcação e código-fonte do ASP.NET, você também precisará copiar os arquivos de suporte do lado do cliente (imagens e arquivos CSS), bem como os outros arquivos de suporte do servidor, `Web.config` e `Web.sitemap`.

> [!NOTE]
> Projetos de Site também pode usar a compilação explícita. Um tutorial futuras examinará como explicitamente compilar um projeto de Site da Web.


## <a name="summary"></a>Resumo

Implantar um aplicativo ASP.NET envolve copiar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. O conjunto exato de arquivos que precisam ser sincronizados depende se código do aplicativo ASP.NET é automaticamente ou explicitamente compilado. A estratégia de compilação empregada é influenciada por se o Visual Studio está configurado para gerenciar o aplicativo ASP.NET usando o modelo de projeto de aplicativo Web ou o modelo de projeto de Site.

O modelo de projeto de aplicativo Web usa a compilação explícita e compila o código do projeto em um único assembly no `Bin` pasta. Ao implantar o aplicativo, a parte de marcação de páginas ASP.NET e o conteúdo do `Bin` pasta deve ser passada para o ambiente de produção; o código-fonte do aplicativo - os arquivos de código e classes de code-behind, por exemplo - não é necessário a ser copiado para o ambiente de produção.

O modelo de projeto de Site usa compilação automática por padrão, embora seja possível compilar explicitamente um projeto de Site da Web, conforme veremos no futuro tutoriais. Implantar um aplicativo ASP.NET que usa compilação automática exige que a parte de marcação *e* código-fonte deve ser copiado para o ambiente de produção. O código é compilado automaticamente no ambiente de produção quando for solicitado pela primeira vez.

Agora que examinamos quais arquivos precisam ser sincronizados entre os ambientes de desenvolvimento e produção, você está pronto para implantar o aplicativo de catálogo revisões para um provedor de host da web.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Visão geral de compilação do ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Controles de usuário do ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Examinando ASP. Navegação do Site da rede](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Introdução aos projetos de aplicativo Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [Tutoriais de página mestra](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Compartilhando código entre as páginas](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Usando uma classe Base personalizada para Code-Behind Classes suas páginas ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Sistema de projeto de Site da Web do Visual Studio 2005: o que é e por que o fizemos?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Passo a passo: Convertendo um projeto de Site da Web para um projeto de aplicativo Web no Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Anterior](asp-net-hosting-options-cs.md)
> [Próximo](deploying-your-site-using-an-ftp-client-cs.md)
