---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: Determinando quais arquivos precisam ser implantados (c#) | Microsoft Docs
author: rick-anderson
description: Quais arquivos precisam ser implantados do ambiente de desenvolvimento para o ambiente de produção depende em parte se o aplicativo ASP.NET tiver sido criado nos...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: 3fb54feb32c3c4a4903c65751bf1a4ae4f016a22
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831029"
---
<a name="determining-what-files-need-to-be-deployed-c"></a>Determinando quais arquivos precisam ser implantados (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Quais arquivos precisam ser implantados do ambiente de desenvolvimento para o ambiente de produção depende em parte se o aplicativo ASP.NET tiver sido criado usando o modelo de Site da Web ou o modelo de aplicativo Web. Saiba mais sobre esses dois modelos de projeto e como o modelo de projeto afeta a implantação.


## <a name="introduction"></a>Introdução

Implantar um aplicativo web ASP.NET envolve copiar os arquivos relacionados ao ASP.NET do ambiente de desenvolvimento para o ambiente de produção. Os arquivos relacionados ao ASP.NET incluem a marcação de página da web do ASP.NET e código e o cliente e servidor de arquivos de suporte. Arquivos de suporte do lado do cliente são os arquivos referenciados por suas páginas da web e enviadas diretamente para o navegador – imagens, arquivos CSS e arquivos JavaScript, por exemplo. Arquivos de suporte do lado do servidor incluem aqueles que são usados para processar uma solicitação no lado do servidor. Isso inclui arquivos de configuração, serviços da web, arquivos de classe, DataSets tipados e LINQ para arquivos do SQL, entre outros.

Em geral, todos os arquivos de suporte do lado do cliente devem ser copiados do ambiente de desenvolvimento para o ambiente de produção, mas os arquivos de suporte do lado do servidor sejam copiados depende se você estiver compilando explicitamente o código do lado do servidor em um assembly (um `.dll` arquivo) ou se você estiver tendo esses assemblies gerados automaticamente. Este tutorial destaca quais arquivos precisam ser implantados quando explicitamente compilar o código em um assembly versus a ausência dessa etapa de compilação ocorrem automaticamente.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Compilação explícita Versus compilação automática

Páginas da web ASP.NET são divididas em código-fonte e de marcação declarativo. A parte de marcação declarativa inclui HTML, controles da Web e sintaxe de associação de dados; a parte de código contém manipuladores de eventos escritos em código Visual Basic ou c#. As partes de código e marcação normalmente são separadas em arquivos diferentes: `WebPage.aspx` contém a marcação declarativa enquanto `WebPage.aspx.cs` abriga o código.

Considere uma página ASP.NET chamada Clock.aspx que contém um controle de rótulo cuja propriedade de texto é definida como a data e hora atuais no carregamento da página. A parte de marcação declarativa (no `Clock.aspx`) conteria a marcação para um controle de rótulo Web -`<asp:Label runat="server" id="TimeLabel" />` – durante a parte de código (no `Clock.aspx.cs`) teria um `Page_Load` manipulador de eventos com o código a seguir:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

Para que o mecanismo do ASP.NET atender uma solicitação para essa página, parte do código da página (o `WebPage.aspx.cs` arquivo) devem ser compilados primeiro. Esta compilação pode acontecer automaticamente ou explicitamente.

Se a compilação acontece explicitamente, o código-fonte do aplicativo inteiro é compilado em um ou mais assemblies (`.dll` arquivos) localizados na caixa de diálogo `Bin` directory. Se a compilação ocorre automaticamente e em seguida, resultante gerada automaticamente é de assembly, por padrão, colocado na `Temporary ASP.NET` pasta de arquivos, que pode ser encontrada em `%WINDOWS%\Microsoft.NET\Framework\`  *&lt;versão&gt;*, Embora esse local é configurável por meio de [ `<compilation>` elemento](https://msdn.microsoft.com/library/s10awwz0.aspx) em `Web.config`. Com a compilação explícita, você deve executar alguma ação para compilar o código do aplicativo ASP.NET em um assembly, e esta etapa ocorre antes da implantação. Com a compilação automática o processo de compilação ocorre no servidor web quando o recurso for acessado pela primeira vez.

Independentemente de qual modelo de compilação que você usar, a parte de marcação de todas as páginas ASP.NET (o `WebPage.aspx` arquivos) precisam ser copiados para o ambiente de produção. Com a compilação explícita que você precisa copiar os assemblies na `Bin` pasta, mas você não precisará copiar partes do código as páginas do ASP.NET (o `WebPage.aspx.cs` arquivos). Com a compilação automática, você precisa copiar os arquivos de parte do código para que o código está presente e pode ser compilado automaticamente quando a página for visitada. A parte de marcação de cada página da web ASP.NET inclui um `@Page` diretiva com atributos que indicam se o código da página associado explicitamente já foi compilado ou se ele precisa ser compilado automaticamente. Como resultado, o ambiente de produção pode funcionar perfeitamente com qualquer um dos modelos de compilação e não é necessário aplicar as configurações de qualquer configuração especial para indicar que a compilação explícita ou automática é usada.

A tabela 1 resume os diferentes arquivos implantar ao usar a compilação explícita versus compilação automática. Observe que, independentemente da compilação modelo usado você sempre deve implantar os assemblies no `Bin` pasta, se essa pasta existe. O `Bin` pasta contém os assemblies específicos para o aplicativo web, que incluem o código-fonte compilado ao usar o modelo de compilação explícita. O `Bin` diretório também contém assemblies de outros projetos e qualquer assembly de terceiros ou de código-fonte aberto, talvez você esteja usando, e eles precisam estar no servidor de produção. Portanto, como um regra geral, copie o `Bin` pasta até a produção durante a implantação. (Se você estiver usando o modelo de compilação automática e não estiver usando todos os assemblies externos, você não terá um `Bin` diretório - Okey!)

| **Modelo de compilação** | **Implantar o arquivo de marcação de parte?** | **Implantar o arquivo de código-fonte?** | **Implantar Assemblies no `Bin` diretório?** |
| --- | --- | --- | --- |
| Compilação explícita | Sim | Não | Sim |
| Compilação automática | Sim | Sim | Sim (se houver) |

**Tabela 1:** depende de quais arquivos você implanta o modelo de compilação usado.

## <a name="taking-a-trip-down-memory-lane"></a>Uma viagem de pista de memória

O método de compilação é usado depende, em parte, como o aplicativo ASP.NET é gerenciado no Visual Studio. Desde o. Concepção da rede do ano 2000, houve quatro versões diferentes do Visual Studio – Visual Studio .NET 2002, o Visual Studio .NET 2003, o Visual Studio 2005 e o Visual Studio 2008. Visual Studio .NET 2002 e 2003 gerenciados aplicativos ASP.NET usando o *modelo de projeto de aplicativo Web*. Os principais recursos do modelo de projeto de aplicativo Web são:

- Os arquivos que o projeto de composição são definidos em um único arquivo de projeto. Todos os arquivos não está definidos no arquivo de projeto não são considerados parte do aplicativo web pelo Visual Studio.
- Usa a compilação explícita. Compilar o projeto compila os arquivos de código dentro do projeto em um único assembly que é colocado no `Bin` pasta.

Quando a Microsoft lançou o Visual Studio 2005 elas removido o suporte para o modelo de projeto de aplicativo Web e ele substituirá o modelo de projeto de Site. O modelo de projeto de Site em si diferenciados do modelo de projeto de aplicativo Web das seguintes maneiras:

- Em vez de ter um único arquivo de projeto que explica os arquivos do projeto, o sistema de arquivos é usado em vez disso. Em resumo, todos os arquivos dentro da pasta do aplicativo web (ou subpastas) são considerados parte do projeto.
- Criando um projeto no Visual Studio não cria um assembly no `Bin` directory. Em vez disso, a criação de um projeto de Site da Web relata os erros de tempo de compilação.
- Suporte para compilação automática. Projetos de Site são normalmente implantados copiando-se a marcação e código-fonte para o ambiente de produção, embora o código pode ser pré-compilados (compilação explícita).

Microsoft reativada o modelo de projeto de aplicativo Web quando ele lançado o Visual Studio 2005 Service Pack 1. No entanto, o Visual Web Developer continua a só há suporte para o modelo de projeto de Site. A boa notícia é que essa limitação foi descartada com o Visual Web Developer 2008 Service Pack 1. Hoje, você pode criar aplicativos ASP.NET no Visual Studio (e o Visual Web Developer) usando o modelo de projeto de aplicativo Web ou o modelo de projeto de Site. Os dois modelos têm seus prós e contras. Consulte a [Introduction to Web Application Projects: comparando projetos de Site da Web e projetos de aplicativos Web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) para obter uma comparação dos dois modelos e para ajudar a decidir qual modelo de projeto funciona melhor para sua situação.

## <a name="exploring-the-sample-web-application"></a>Explorando o aplicativo Web de exemplo

O download para este tutorial inclui um aplicativo ASP.NET chamado resenhas de livros. O site imita um site hobby alguém pode criar compartilhar seu livro examina com a comunidade online. Neste aplicativo web do ASP.NET é muito simple e consiste nos seguintes recursos:

- `Web.config`, o arquivo de configuração do aplicativo.
- Uma página mestra (`Site.master`).
- Sete páginas ASP.NET diferentes: 

    - ~`/Default.aspx`-homepage do site.
    - ~`/About.aspx` -uma página "Sobre o Site".
    - ~`/Fiction/Default.aspx` -uma página listando os livros de ficção foram revisados. 

        - ~`/Fiction/Blaze.aspx` -uma revisão do livro Richard Bachman *Blaze*.
    - ~/`Tech/Default.aspx` -uma página listando os livros de tecnologia que foram revisados. 

        - ~/`Tech/CYOW.aspx`-uma análise dos *criar seu próprio site*.
        - ~/`Tech/TYASP35.aspx` -uma análise dos *ensinar por conta própria ASP.NET 3.5 in 24 horas*.
- Três arquivos CSS diferentes na pasta estilos.
- Quatro arquivos de imagem - funciona com o logotipo do ASP.NET e as imagens de bastidores dos três revisados livros - todos os localizados no `Images` pasta.
- Um `Web.sitemap` arquivo, que define o mapa de site e é usado para exibir menus na `Default.aspx` páginas no diretório raiz e `Fiction` e `Tech` pastas.
- Um arquivo de classe chamado `BasePage.cs` que define uma base de `Page` classe. Essa classe estende a funcionalidade dos `Page` classe automaticamente definindo a `Title` propriedade com base na posição da página no mapa do site. Em poucas palavras, qualquer classe de code-behind do ASP.NET que se estende `BasePage` (em vez de `System.Web.UI.Page`) terá seu título definido como um valor, dependendo de sua posição no mapa do site. Por exemplo, ao exibir a ~ /`Tech/CYOW.aspx` página, o título é definido como "Home: tecnologia: criar seu próprio site".

Figura 1 mostra uma captura de tela do site resenhas de livros quando visualizado por meio de um navegador. Aqui você verá a página ~ /`Tech/TYASP35.aspx`, que analisa o livro *ensinar por conta própria ASP.NET 3.5 in 24 horas*. A trilha de navegação que abrange a parte superior da página e no menu na coluna à esquerda são baseados na estrutura de mapa de site definida em `Web.sitemap`. A imagem no canto superior direito é uma da tampa de livro imagens localizadas no `Images` pasta. Aparência do site da Web são definidas por meio de regras de folha de estilo escritas pelos arquivos CSS na pasta estilos, enquanto o layout da página abrangente é definido na página mestra, em cascata `Site.master`.


[![O site do livro analisa oferece revisões em uma variedade de títulos](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Figura 1:** site do livro analisa oferece revisões em uma variedade de títulos ([clique para exibir a imagem em tamanho normal](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


Este aplicativo não usa um banco de dados; cada revisão é implementado como uma página da web separada no aplicativo. Neste tutorial (e os próximo vários tutoriais) orientam durante a implantação de um aplicativo web que não tem um banco de dados. No entanto, em um tutorial futuro podemos melhorar este aplicativo para armazenar as revisões, comentários dos leitores e outras informações dentro de um banco de dados e irá explorar quais etapas precisam ser executadas para implantar corretamente um aplicativo web controlado por dados.

> [!NOTE]
> Esses tutoriais se concentrar em hospedar aplicativos ASP.NET com um provedor de host da web e não explorar tópicos complementares, como o ASP. Sistema de mapa de site da rede ou usando uma base de `Page` classe. Para obter mais informações sobre essas tecnologias e para obter mais informações sobre outros tópicos abordados em todo o tutorial, consulte a seção leitura adicional no final da cada tutorial.


Download deste tutorial tem duas cópias do aplicativo web, cada um implementado como um tipo diferente de projeto do Visual Studio: BookReviewsWAP, um projeto de aplicativo Web e BookReviewsWSP, um projeto de Site da Web. Ambos os projetos criados com o Visual Web Developer 2008 SP1 e usam o ASP.NET 3.5 SP1. Para trabalhar com esses projetos são iniciados ao descompactar o conteúdo em sua área de trabalho. Abra o projeto de aplicativo Web (BookReviewsWAP), navegue até a pasta BookReviewsWAP e clique duas vezes no arquivo de solução, `BookReviewsWAP.sln`. Para abrir o projeto de Site da Web (BookReviewsWSP), inicie o Visual Studio e em seguida, no menu Arquivo, escolha a opção de abrir o Site da Web, navegue até o `BookReviewsWSP` pasta na área de trabalho e clique em Okey.


As duas seções restantes neste tutorial examinar quais arquivos você precisará copiar para o ambiente de produção ao implantar o aplicativo. Os próximos dois tutoriais - *[Implantando seu Site usando FTP](deploying-your-site-using-an-ftp-client-cs.md)* e *[implantando seu Site usando o Visual Studio](deploying-your-site-using-visual-studio-cs.md)* -mostram diferentes maneiras Copie esses arquivos em um provedor de host da web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Determinando os arquivos para implantar para o projeto de aplicativo Web

O modelo de projeto de aplicativo Web usa a compilação explícita - código-fonte do projeto é compilado em um único assembly sempre que você compilar o aplicativo. Esta compilação inclui arquivos code-behind das páginas do ASP.NET (~ /`Default.aspx.cs`, ~ /`About.aspx.cs`e assim por diante), bem como o `BasePage.cs` classe. O assembly resultante é chamado BookReviewsWAP.dll e está localizado na caixa de diálogo `Bin` directory.

Figura 2 mostra os arquivos que compõem o projeto de aplicativo Web do livro revisões.


[![O Gerenciador de soluções lista os arquivos que compõem o projeto de aplicativo Web](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Figura 2**: O Gerenciador de soluções lista os arquivos que compõem o projeto de aplicativo Web


Para implantar um aplicativo ASP.NET desenvolvido usando o início do modelo de projeto de aplicativo Web, criando o aplicativo para compilar o código-fonte mais recente de explicitamente em um assembly. Em seguida, copie os seguintes arquivos para o ambiente de produção:

- Os arquivos que contêm a marcação declarativa para cada ASP.NET de página, como ~ /`Default.aspx`, ~ /`About.aspx`e assim por diante. Além disso, cópia de backup a marcação declarativa para quaisquer páginas mestras e os controles de usuário.
- Os assemblies (`.dll` arquivos) na `Bin` pasta. Você não precisa copiar os arquivos de banco de dados do programa (`.pdb`) ou quaisquer arquivos XML que talvez você encontre o `Bin` directory.

Você não precisa copiar os arquivos de código-fonte as páginas do ASP.NET para o ambiente de produção, nem é preciso copiar o `BasePage.cs` arquivo de classe.

> [!NOTE]
> Como mostra a Figura 2, o `BasePage` classe é implementada como um arquivo de classe no projeto, colocado na pasta denominada `HelperClasses`. Quando o projeto for compilado o código a `BasePage.cs` arquivo é compilado, juntamente com classes code-behind das páginas do ASP.NET no único assembly, `BookReviewsWAP.dll.` ASP.NET tem uma pasta especial denominada `App_Code` que foi projetado para armazenar arquivos de classe para a Web Projetos de site. O código a `App_Code` pasta será compilada automaticamente e, portanto, não deve ser usada com projetos de aplicativos Web. Em vez disso, você deve colocar os arquivos de classe do seu aplicativo em uma pasta normal chamada `HelperClasses`, ou `Classes`, ou algo parecido. Como alternativa, você pode colocar arquivos de classe em um projeto de biblioteca de classes separado.


Além de copiar os arquivos de marcação relacionados ao ASP.NET e o assembly na `Bin` pasta, você também precisará copiar os arquivos de suporte do lado do cliente – imagens e arquivos CSS –, bem como os outros arquivos de suporte do lado do servidor, `Web.config` e `Web.sitemap`. Esse e servidor-cliente dão suporte a necessidade de arquivos a serem copiados para o ambiente de produção, independentemente de você usar compilação explícita ou automática.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Determinando os arquivos para implantar para os arquivos de projeto de Site da Web

O modelo de projeto de Site dá suporte à compilação automática, um recurso não está disponível ao usar o modelo de projeto de aplicativo Web. Com a compilação explícita, você deve compilar o código-fonte do seu projeto em um assembly e copiar esse assembly para o ambiente de produção. Por outro lado, com a compilação automática você simplesmente copiar o código-fonte para o ambiente de produção e é compilado pelo tempo de execução sob demanda conforme necessário.

A opção de menu de compilação no Visual Studio está presente em projetos de aplicativos Web e projetos de Site. Criar um Web Application Projects compila o código-fonte do projeto em um único assembly localizado no `Bin` diretório; criando um projeto de Site da Web verifica os erros de tempo de compilação, mas não cria todos os assemblies. Para implantar um aplicativo ASP.NET desenvolvido usando o modelo de projeto de Site todos os que você precisa fazer é copiar os arquivos apropriados para o ambiente de produção, mas recomendo que você primeiro compilar o projeto para garantir que não há nenhum erro de tempo de compilação.

Figura 3 mostra os arquivos que compõem o projeto de Site da Web do catálogo de revisões.


 [![O Gerenciador de soluções lista os arquivos que compõem o projeto de Site da Web](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Figura 3**: O Gerenciador de soluções lista os arquivos que compõem o projeto de Site da Web


Implantando um projeto de Site da Web envolve a cópia de todos os arquivos relacionados ao ASP.NET para o ambiente de produção – que inclui as páginas de marcação para páginas ASP.NET, páginas mestras e controles de usuário, juntamente com seus arquivos de código. Você também precisará copiar backup de quaisquer arquivos de classe, como BasePage.cs. Observe que o `BasePage.cs` arquivo está localizado no `App_Code` pasta, que é uma pasta especial do ASP.NET usada em projetos de Site da Web para arquivos de classe. A pasta especial precisa ser criado em produção, além disso, como os arquivos de classe na `App_Code` pasta no ambiente de desenvolvimento deve ser copiada para o `App_Code` pasta em produção.

Além de copiar os arquivos de código de marcação e código-fonte do ASP.NET, você também precisará copiar os arquivos de suporte do lado do cliente – imagens e arquivos CSS –, bem como os outros arquivos de suporte do lado do servidor, `Web.config` e `Web.sitemap`.

> [!NOTE]
> Projetos de Site também pode usar a compilação explícita. Um tutorial futuro examinará como explicitamente compilar um projeto de Site da Web.


## <a name="summary"></a>Resumo

Implantando um aplicativo ASP.NET envolve copiar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. O conjunto preciso de arquivos que precisam ser sincronizados depende se código do aplicativo ASP.NET é explicitamente ou automaticamente compilado. A estratégia de compilação empregada é influenciada pelo se o Visual Studio está configurado para gerenciar o aplicativo ASP.NET usando o modelo de projeto de aplicativo Web ou o modelo de projeto de Site.

O modelo de projeto de aplicativo Web usa a compilação explícita e compila o código do projeto em um único assembly no `Bin` pasta. Ao implantar o aplicativo, a parte de marcação de páginas ASP.NET e o conteúdo do `Bin` pasta deve ser passada para o ambiente de produção; o código-fonte do aplicativo - os arquivos de código e classes code-behind, por exemplo - não é necessário a ser copiado para o ambiente de produção.

O modelo de projeto de Site usa compilação automática por padrão, embora seja possível compilar explicitamente um projeto de Site da Web, como veremos no futuro tutoriais. Implantar um aplicativo ASP.NET que usa a compilação automática requer que a parte de marcação *e* código-fonte deve ser copiado para o ambiente de produção. O código é compilado automaticamente no ambiente de produção quando solicitado pela primeira vez.

Agora que examinamos quais arquivos precisam ser sincronizados entre os ambientes de desenvolvimento e produção estamos prontos para implantar o aplicativo de resenhas de livros em um provedor de host da web.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Visão geral de compilação do ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Controles de usuário ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Examinando o ASP. Navegação no Site da rede](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Introdução aos projetos de aplicativos Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [Tutoriais de página mestra](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Compartilhando código entre as páginas](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Usando uma classe Base personalizada para as Classes Code-Behind de suas páginas ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Sistema de projeto de Site da Web do Visual Studio 2005: o que é e por que o fizemos?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Passo a passo: Convertendo um projeto de Site da Web em um projeto de aplicativo Web no Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Anterior](asp-net-hosting-options-cs.md)
> [Próximo](deploying-your-site-using-an-ftp-client-cs.md)
