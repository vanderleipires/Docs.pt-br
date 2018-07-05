---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
title: Implantando o Site usando um cliente FTP (c#) | Microsoft Docs
author: rick-anderson
description: A maneira mais simples de implantar um aplicativo ASP.NET é copiar manualmente os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. EST...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: a3599cf7-8474-4006-954a-3bc693736b66
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-cs
msc.type: authoredcontent
ms.openlocfilehash: 50e1522a39cb7b8ea2ee4c664f166b45f22ff070
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842277"
---
<a name="deploying-your-site-using-an-ftp-client-c"></a>Implantando o Site usando um cliente FTP (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_cs.pdf)

> A maneira mais simples de implantar um aplicativo ASP.NET é copiar manualmente os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. Este tutorial mostra como usar um cliente de FTP para obter os arquivos da área de trabalho para o provedor de host da web.


## <a name="introduction"></a>Introdução

O tutorial anterior introduziu um aplicativo de web simples do ASP.NET de revisão do livro, que é composto de um punhado de páginas ASP.NET, uma página mestra, uma base personalizada `Page` de classe, um número de imagens, e folhas de estilos três CSS. Agora estamos prontos para implantar esse aplicativo em um provedor de host web, no ponto em que o aplicativo estará acessível para qualquer pessoa com uma conexão com a Internet!


Nossas discussões na [ *determinando quais arquivos precisam ser implantados* ](determining-what-files-need-to-be-deployed-cs.md) tutorial, nós sabemos quais arquivos precisam ser copiados para o provedor de host da web. (Lembre-se de que os arquivos que são copiados depende se seu aplicativo é explicitamente ou automaticamente compilado.) Mas como fazer para que os arquivos do ambiente de desenvolvimento (nossa área de trabalho) até o ambiente de produção (o servidor de web gerenciado pelo provedor de host da web)? O [ **F** ile **T** transferir **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) é um protocolo comumente usado para copiar arquivos de um computador para outro em uma rede. Outra opção é extensões de servidor do FrontPage (FPSE). Este tutorial se concentra no uso de software de cliente FTP autônoma para implantar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção.

> [!NOTE]
> Visual Studio inclui ferramentas para publicação de sites por meio de FTP; Essas ferramentas, bem como ferramentas que usam FPSE, são abordadas no próximo tutorial.


Para copiar os arquivos usando FTP, precisamos de um *cliente FTP* no ambiente de desenvolvimento. Um cliente de FTP é um aplicativo que foi projetado para copiar arquivos do computador em que ele é instalado em um computador que está executando uma *servidor FTP*. (Se seu provedor de host da web dá suporte a transferências de arquivos via FTP, assim como a maioria, em seguida, há um servidor FTP em execução em seus servidores web.) Há um número de aplicativos cliente FTP disponíveis. Seu navegador da web pode até mesmo double um cliente FTP. Meu cliente FTP favorito, o que usarei para este tutorial, é [FileZilla](http://filezilla-project.org/), um cliente FTP gratuito, código-fonte aberto que está disponível para Windows, Linux e Mac. Qualquer cliente FTP funcionará, no entanto, então sinta-se livre para usar qualquer cliente que você está mais familiarizado.

Se você estiver acompanhando você será necessário criar uma conta com um provedor de host da web antes de você pode concluir este tutorial ou subsequentes. Conforme observado no tutorial anterior, há alguns empresas de provedor de host da web com um amplo espectro de preços, recursos e qualidade de serviço. Para esta série de tutoriais que esteja usando o [desconto ASP.NET](http://discountasp.net) como meu host web provedor, mas você pode prosseguir com qualquer provedor de host da web desde que eles oferecem suporte a versão do ASP.NET desenvolvido em seu site. (Esses tutoriais foram criados usando ASP.NET 3.5). Além disso, porque podemos copiará os arquivos para o provedor de host da web usando FTP neste tutorial e no futuro aqueles é imperativo que seu provedor de host da web dá suporte ao acesso FTP em seus servidores web. Praticamente todos os provedores de host da web oferecem esse recurso, mas você deve verificar novamente antes de se inscrever.

## <a name="deploying-the-book-review-web-application-project"></a>Implantando o projeto de aplicativo Web do catálogo revisão

Lembre-se de que há duas versões do aplicativo web resenha de livro: um implementado usando o modelo de projeto de aplicativo Web (BookReviewsWAP) e outra usando o modelo de projeto de Site (BookReviewsWSP). O tipo de projeto influencia se o site seja compilado automaticamente ou explicitamente, e esse modelo de compilação determina quais arquivos precisam ser implantados. Consequentemente, vamos examinar implantar os projetos BookReviewsWAP e BookReviewsWSP separadamente, começando com o BookReviewsWAP. Reserve um tempo para baixar esses dois aplicativos do ASP.NET, se você ainda não fez isso.

Inicie o projeto BookReviewsWAP, navegando até a `BookReviewsWAP` pasta e clicando duas vezes o `BookReviewsWAP.sln` arquivo. Antes de implantar o projeto é importante para criá-la para garantir que todas as alterações no código-fonte são incluídas no assembly compilado. Para compilar o projeto vá para o menu de compilação e escolha a opção de menu compilar BookReviewsWAP. Isso compila o código-fonte no projeto em um único assembly, `BookReviewsWAP.dll`, que é colocado no `Bin` pasta.

Agora estamos prontos para implantar os arquivos necessários! Inicie seu cliente FTP e conecte-se ao servidor web no seu provedor de host da web. (Quando você se inscreve em uma empresa de hospedagem na web enviará um email para você obter informações sobre como se conectar ao servidor FTP; isso inclui o endereço para o servidor FTP, bem como um nome de usuário e senha).

Copie os seguintes arquivos da área de trabalho para a pasta raiz do site no seu provedor de host da web. Quando você FTP para o servidor web na web hospeda o provedor provavelmente no diretório raiz de site. No entanto, alguns provedores de host da web tem uma subpasta nomeada `www` ou `wwwroot` que serve como a pasta raiz de arquivos de seu site. Por fim, quando FTPing os arquivos talvez seja necessário criar a estrutura de pasta correspondente no ambiente de produção - a `Bin` pasta, o `Fiction` pasta, o `Images` pasta e assim por diante.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Todo o conteúdo do `Styles` pasta
- Todo o conteúdo da `Images` pasta (e em sua subpasta `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Figura 1 mostra o FileZilla depois que os arquivos necessários foram copiados. O FileZilla exibe os arquivos no computador local à esquerda e os arquivos no computador remoto no lado direito. Como a Figura 1 mostra, os arquivos de código do código-fonte do ASP.NET, tais como `About.aspx.cs`, estão no computador local (o ambiente de desenvolvimento), mas não foram copiados para o provedor de host da web (o ambiente de produção) como arquivos de código não precisa ser implantado ao usar compilação explícita.

> [!NOTE]
> Não há nenhum problema em que os arquivos de código fonte no servidor de produção, como eles são ignorados. ASP.NET proíbe solicitações HTTP para arquivos de código-fonte por padrão, para que, mesmo se os arquivos de código-fonte estão presentes no servidor de produção sejam inacessíveis para os visitantes do site. (Ou seja, se um usuário tentar visitar `http://www.yoursite.com/Default.aspx.cs` obterão uma página de erro que explica que esses tipos de arquivos - `.cs` arquivos – são proibidos.)


[![Usar um cliente de FTP para copiar os arquivos necessários da área de trabalho para o servidor Web no provedor de Host da Web](deploying-your-site-using-an-ftp-client-cs/_static/image2.png)](deploying-your-site-using-an-ftp-client-cs/_static/image1.png)

**Figura 1**: Use um cliente de FTP para copiar os arquivos necessários de sua área de trabalho para o servidor Web no provedor de Host da Web ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-an-ftp-client-cs/_static/image3.png))


Depois de implantar seu site, reserve um tempo para testar o site. Se você tiver adquirido um nome de domínio e defina as configurações de DNS corretamente, você pode visitar seu site, inserindo seu nome de domínio. Como alternativa, seu provedor de host da web deve ter lhe forneceu uma URL para seu site, que será algo parecido com *accountname*. *webhostprovider*.com ou *webhostprovider*.com /*accountname*. Por exemplo, a URL da minha conta no ASP.NET de desconto é: `http://httpruntime.web703.discountasp.net`.

Figura 2 mostra um site implantado resenhas de livros. Observe que estou exibindo-la em ASP com desconto. Servidores da rede, em `http://httpruntime.web703.discountasp.net`. Neste momento, qualquer pessoa com uma conexão à Internet pode exibir meu site! Como podemos poderia esperar, o site se parece e se comporta exatamente como ele fez ao testá-lo no ambiente de desenvolvimento.

> [!NOTE]
> Se você receber um erro ao seu aplicativo de exibição levar alguns instantes para garantir que você implantou o conjunto correto de arquivos. Em seguida, verifique a mensagem de erro para ver se ele revela qualquer pistas sobre o problema. Depois disso, pode ser a assistência técnica de web host da sua empresa ou poste sua pergunta para o fórum apropriado na [fóruns do ASP.NET](https://forums.asp.net/).


[![O Site de revisões do livro é agora acessível para qualquer pessoa com uma Conexão de Internet](deploying-your-site-using-an-ftp-client-cs/_static/image5.png)](deploying-your-site-using-an-ftp-client-cs/_static/image4.png)

**Figura 2**: O Site de revisões de livro é agora acessível para qualquer pessoa com uma Conexão de Internet ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-an-ftp-client-cs/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Implantar o projeto de Site da Web do catálogo revisão

Ao implantar um aplicativo ASP.NET que usa a compilação automática, como o projeto de Site da Web BookReviewsWSP, não há nenhum assembly compilado no `Bin` pasta. Como resultado, arquivos de código fonte do aplicativo web devem ser implantados no ambiente de produção. Vamos examinar esse processo.

Como com o projeto de aplicativo Web, é aconselhável primeiro compilar o aplicativo antes de implantá-lo. Enquanto a criação de um projeto de Site da Web não cria um assembly, ele verifica os erros de tempo de compilação na página. Melhor para encontrar esses erros agora em vez de ter um visitante do site descobri-los para você!

Depois que você criou com êxito o projeto, use seu cliente FTP para copiar os arquivos a seguir para a pasta raiz do site no seu provedor de host da web. Talvez você precise criar a estrutura de pasta correspondente no ambiente de produção.

> [!NOTE]
> Se você já implantou o projeto, mas ainda quiser tentar implantar o projeto BookReviewsWSP de BookReviewsWAP, primeiro exclua todos os arquivos no servidor web que foram carregados durante a implantação de BookReviewsWAP e, em seguida, implante os arquivos para BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.cs`
- `~/About.aspx`
- `~/About.aspx.cs`
- `~/Site.master`
- `~/Site.master.cs`
- `~/Web.config`
- `~/Web.sitemap`
- Todo o conteúdo do `Styles` pasta
- Todo o conteúdo da `Images` pasta (e em sua subpasta `BookCovers`)
- `~/App_Code/BasePage.cs`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.cs`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.cs`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.cs`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.cs`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.cs`

Figura 3 mostra o FileZilla depois de copiar os arquivos necessários. Como você pode ver, o ASP.NET fonte arquivos de código, como `About.aspx.cs`, estão presentes no computador local (o ambiente de desenvolvimento) e o provedor de host da web (o ambiente de produção) como arquivos de código precisam ser implantados ao usar automática compilação.


[![Usar um cliente de FTP para copiar os arquivos necessários da área de trabalho para o servidor Web no provedor de Host da Web](deploying-your-site-using-an-ftp-client-cs/_static/image8.png)](deploying-your-site-using-an-ftp-client-cs/_static/image7.png)

**Figura 3**: Use um cliente de FTP para copiar os arquivos necessários de sua área de trabalho para o servidor Web no provedor de Host da Web ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-an-ftp-client-cs/_static/image9.png))


A experiência do usuário não é afetada pelo modelo de compilação do aplicativo. As mesmas páginas ASP.NET são acessíveis e sua aparência e se comportam da mesma se o site foi criado usando o modelo de projeto de aplicativo Web ou o modelo de projeto de Site.

### <a name="updating-a-web-application-on-production"></a>Atualizando um aplicativo Web em produção

Implantação e desenvolvimento de aplicativos web não são um processo único. Por exemplo, ao criar o site resenha de livro eu criado várias páginas e escreveu o código que acompanha este artigo no meu computador pessoal (o ambiente de desenvolvimento). Depois de atingir um determinado estado estável, eu Implantei meu aplicativo para que outras pessoas pudessem visite o site e leia minhas avaliações. Mas a implantação não marca o final do meu desenvolvimento neste site. Eu pode adicionar mais crítica literária ou implementa novos recursos, como permitir que meu os visitantes manuais taxa ou deixar seus próprios comentários. Essas melhorias poderiam ser desenvolvidas no ambiente de desenvolvimento e, ao concluir, seriam necessário ser implantado. Desenvolvimento e implantação, portanto, são cíclicos. Você pode desenvolve um aplicativo e, em seguida, implantá-lo. Embora o site está funcionando e em produção, novos recursos são adicionados e bugs corrigidos ao longo do tempo, o que exige a reimplantação do aplicativo. E assim por diante e assim por diante.

Como você pode esperar, ao reimplantar em um aplicativo web que você só precisará copiar os arquivos novos e alterados. Não é necessário reimplantar páginas inalteradas ou arquivos de suporte do servidor ou cliente (embora não haja nenhum problema em fazer isso).

> [!NOTE]
> Uma coisa a ter em mente ao usar a compilação explícita é que sempre que você adicionar uma nova página ASP.NET para o projeto ou fazer alterações relacionadas ao código, você precisará recompilar o projeto, que atualiza o assembly no `Bin` pasta. Consequentemente, você precisará copiar esse assembly atualizado para a produção, ao atualizar um aplicativo web em produção (juntamente com o outro conteúdo novo e atualizado).


Também entende que todas as alterações para o `Web.config` ou os arquivos no `Bin` diretório para e reinicia o Pool de aplicativos do site. Se o estado da sessão é armazenado usando o `InProc` modo (o padrão), em seguida, os visitantes do site perderá seu estado de sessão sempre que esses arquivos de chave são modificados. Para evitar esta armadilha, considere armazenar a sessão usando o `StateServer` ou `SQLServer` modos. Para obter mais informações sobre esse tópico, leia [modos de estado de sessão](https://msdn.microsoft.com/library/ms178586.aspx).

Por fim, tenha em mente que implantá-los novamente um aplicativo pode levar de alguns segundos-vários minutos, dependendo do número e tamanho dos arquivos que precisam ser copiados para o ambiente de produção. Durante esse tempo, os usuários que visitam seu site pode enfrentar erros ou comportamento estranho. Você pode "desligar" todo o seu aplicativo com a adição de uma página chamada `App_Offline.htm` para o diretório raiz do seu aplicativo que explica aos usuários que o site está inativo para manutenção (ou qualquer outro) e será ser fazer backup em breve. Quando o `App_Offline.htm` arquivo estiver presente, o tempo de execução do ASP.NET redireciona todas as solicitações recebidas para a página.

## <a name="summary"></a>Resumo

Implantar um aplicativo web envolve copiar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. O meio mais comum pelo qual os arquivos são transferidos pela rede é o protocolo FTP (File Transfer) e a maioria dos provedores de host da web dão suporte a acesso ao FTP para seus servidores web. Neste tutorial vimos como usar um cliente de FTP para implantar os arquivos necessários para o servidor web. Uma vez implantado, o site pode ser visitado por qualquer pessoa com uma conexão com a Internet!

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Aplicativo\_Offline.htm e trabalhar para resolver o recurso de "Erros de amigável do IE"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Modos de estado de sessão](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Anterior](determining-what-files-need-to-be-deployed-cs.md)
> [Próximo](deploying-your-site-using-visual-studio-cs.md)
