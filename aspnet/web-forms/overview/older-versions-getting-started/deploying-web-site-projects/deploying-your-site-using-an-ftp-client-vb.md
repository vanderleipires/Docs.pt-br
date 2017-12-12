---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: Implantar o Site usando um cliente de FTP (VB) | Microsoft Docs
author: rick-anderson
description: "A maneira mais simples para implantar um aplicativo ASP.NET é copiar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. EST..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: 56b73820f8b770a5332cb39029ae25df4d5753dc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="deploying-your-site-using-an-ftp-client-vb"></a>Implantar o Site usando um cliente de FTP (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> A maneira mais simples para implantar um aplicativo ASP.NET é copiar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. Este tutorial mostra como usar um cliente de FTP para obter os arquivos da área de trabalho para o provedor de host da web.


## <a name="introduction"></a>Introdução

O tutorial anterior introduziu um aplicativo simples de web ASP.NET de revisão do catálogo, que é composto de uma série de páginas ASP.NET, uma página mestra, uma base personalizada `Page` classe, um número de imagens e folhas de estilo três CSS. Agora você está pronto para implantar esse aplicativo para um provedor de host da web, no ponto em que o aplicativo poderão ser acessado por qualquer pessoa com uma conexão à Internet!


De nossas discussões no [ *determinar quais arquivos precisam ser implantados* ](determining-what-files-need-to-be-deployed-vb.md) tutorial, sabemos quais arquivos precisam ser copiados para o provedor de host da web. (Lembre-se de que os arquivos são copiados depende se o aplicativo é explicitamente ou automaticamente compilado.) Mas, como fazemos os arquivos do ambiente de desenvolvimento (nosso desktop) até o ambiente de produção (o servidor da web gerenciado pelo provedor de host da web)? O [ **F** ile **T** transferir **P** rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) é um protocolo usado para copiar arquivos de um computador para outro em uma rede. Outra opção é extensões de servidor do FrontPage (FPSE). Este tutorial concentra-se usando o software de cliente FTP autônoma para implantar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção.

> [!NOTE]
> O Visual Studio inclui ferramentas para publicação de sites por meio de FTP; Essas ferramentas, bem como ferramentas que usam FPSE, são abordadas no próximo tutorial.


Para copiar os arquivos usando o FTP é necessário um *cliente FTP* no ambiente de desenvolvimento. Um cliente de FTP é um aplicativo que foi projetado para copiar arquivos do computador que está instalada em um computador que esteja executando um *servidor FTP*. (Se o seu provedor de host da web dá suporte a transferências de arquivo por meio de FTP, assim como a maioria, em seguida, há um servidor FTP em execução em seus servidores web.) Há um número de aplicativos cliente FTP disponíveis. Seu navegador da web pode até mesmo double um cliente de FTP. O cliente FTP favorito, o que esteja usando para este tutorial, é [FileZilla](http://filezilla-project.org/), um cliente FTP livre, o código-fonte aberto que está disponível para Windows, Linux e Macs. Qualquer cliente FTP funcione, no entanto, caso sinta-se livre para usar qualquer cliente que você está mais familiarizado.

Se você estiver acompanhando você será necessário criar uma conta com um provedor de host da web antes de você pode concluir este tutorial ou subsequentes. Conforme observado no tutorial anterior, há alguns empresas de provedor de host da web com uma ampla variedade de preços, recursos e qualidade de serviço. Para esta série de tutoriais que esteja usando o [ASP.NET desconto](http://discountasp.net) como meu host web provedor, mas você pode acompanhar qualquer provedor de host da web como eles oferecem suporte à versão do ASP.NET seu site é desenvolvido no. (Esses tutoriais foram criados usando o ASP.NET 3.5). Além disso, porque podemos irá copiar os arquivos para o provedor de host da web usando FTP neste tutorial e no futuro aqueles é essencial que o seu provedor de host da web dá suporte ao acesso FTP em seus servidores web. Praticamente todos os provedores de host da web oferecem esse recurso, mas você deve verificar novamente antes de se inscrever.

## <a name="deploying-the-book-review-web-application-project"></a>Implantando o projeto de aplicativo Web do catálogo revisão

Lembre-se de que há duas versões do aplicativo da web de análise de livro: um implementado usando o modelo de projeto de aplicativo Web (BookReviewsWAP) e o outro usando o modelo de projeto de Site da Web (BookReviewsWSP). O tipo de projeto influencia se o site é compilado automaticamente ou explicitamente, e esse modelo de compilação determina quais arquivos precisam ser implantados. Consequentemente, vamos examinar implantar os projetos BookReviewsWAP e BookReviewsWSP separadamente, começando com o BookReviewsWAP. Dedicar um tempo para baixar esses dois aplicativos ASP.NET, se você ainda não fez isso.

Inicie o projeto BookReviewsWAP navegando até o `BookReviewsWAP` pasta e clique duas vezes o `BookReviewsWAP.sln` arquivo. Antes de implantar o projeto é importante para compilá-lo para garantir que as alterações ao código-fonte são incluídas no assembly compilado. Para compilar o projeto, acesse o menu Build e escolha a opção de menu Build BookReviewsWAP. Esse procedimento compila o código-fonte no projeto em um único assembly, `BookReviewsWAP.dll`, que é colocado no `Bin` pasta.

Agora você está pronto para implantar os arquivos necessários! Inicie o cliente FTP e conecte-se ao servidor da web no seu provedor de host da web. (Quando você se inscreve em uma empresa de hospedagem na web enviará um e-mail obter informações sobre como se conectar ao servidor FTP; isso inclui o endereço para o servidor FTP, bem como um nome de usuário e senha).

Copie os seguintes arquivos da área de trabalho para a pasta raiz do site no seu provedor de host da web. Quando o FTP para o servidor web na web provedor são provavelmente o diretório raiz do site. No entanto, alguns provedores de host da web tem uma subpasta chamada `www` ou `wwwroot` que serve como a pasta raiz de arquivos de seu site. Finalmente, quando FTPing os arquivos você precisará criar a estrutura de pasta correspondente no ambiente de produção - o `Bin` pasta, o `Fiction` pasta, o `Images` pasta e assim por diante.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Todo o conteúdo do `Styles` pasta
- Todo o conteúdo do `Images` pasta (e sua subpasta `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

A Figura 1 mostra FileZilla depois que os arquivos necessários foram copiados. FileZilla exibe os arquivos no computador local à esquerda e os arquivos no computador remoto, à direita. Como a Figura 1 mostra, os arquivos de código de origem do ASP.NET, como `About.aspx.vb`, estão no computador local (o ambiente de desenvolvimento), mas não foram copiados para o provedor de host da web (o ambiente de produção), porque os arquivos de código não precisam ser implantados ao usar compilação explícita.

> [!NOTE]
> Não há nenhum problema em que os arquivos de código fonte no servidor de produção, pois elas são ignoradas. ASP.NET não permite solicitações de HTTP para arquivos de código fonte por padrão para que o mesmo que os arquivos de código de origem estejam presentes no servidor de produção não estão acessíveis para os visitantes do site. (Ou seja, se um usuário tenta visite `http://www.yoursite.com/Default.aspx.vb` que eles receberão uma página de erro que explica que esses tipos de arquivos - `.vb` arquivos - são proibidos.)


[![Use um cliente de FTP para copiar os arquivos necessários da área de trabalho para o servidor Web no provedor de Host da Web.](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**Figura 1**: usar um cliente de FTP para copiar os arquivos necessários da sua área de trabalho para o servidor Web no provedor de Host da Web ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))


Depois de implantar seu site dedicar um tempo para testar o site. Se você tiver adquirido um nome de domínio e configurou as configurações de DNS corretamente, você pode visitar o site digitando seu nome de domínio. Como alternativa, seu provedor de host da web deve ter forneceu uma URL do site, que será parecida com *accountname*. *webhostprovider*.com ou *webhostprovider*.com /*accountname*. Por exemplo, a URL da minha conta no ASP.NET desconto é: `http://httpruntime.web703.discountasp.net`.

A Figura 2 mostra o site revisões de livros implantado. Observe que estou vendo-la em ASP desconto. Servidores da rede, em `http://httpruntime.web703.discountasp.net`. Neste momento, qualquer pessoa com uma conexão à Internet pode exibir meu site! Como podemos espera, o site parece e se comporta exatamente como estava quando testá-lo no ambiente de desenvolvimento.

> [!NOTE]
> Se você receber um erro quando o aplicativo de exibição dedicar um tempo para garantir que você implantou o conjunto correto de arquivos. Em seguida, verifique a mensagem de erro para ver se ele revela quaisquer dicas sobre o problema. Depois disso, ative a assistência técnica de web host da sua empresa ou postar a pergunta no fórum apropriado no [ASP.NET fóruns](https://forums.asp.net/).


[![O Site de revisões do catálogo é agora acessível para qualquer pessoa com uma Conexão de Internet.](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**Figura 2**: O Site de revisões do catálogo está agora acessível para qualquer pessoa com uma Conexão de Internet ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Implantar o projeto de Site da Web do catálogo revisão

Ao implantar um aplicativo ASP.NET que usa compilação automática, como o projeto de Site da Web BookReviewsWSP, não há nenhum assembly compilado no `Bin` pasta. Como resultado, os arquivos de código fonte do aplicativo da web devem ser implantados no ambiente de produção. Vamos examinar esse processo.

Como com o projeto de aplicativo Web é aconselhável a primeira compilação o aplicativo antes de implantá-lo. Ao criar um projeto de Site da Web não cria um assembly, ele verifica os erros de tempo de compilação na página. Melhor para localizar esses erros agora, em vez de ter um visitante do site descobri-los para você!

Depois de ter criado com êxito o projeto, use o cliente FTP para copiar os arquivos a seguir para a pasta raiz do site no seu provedor de host da web. Talvez seja necessário criar a estrutura de pasta correspondente no ambiente de produção.

> [!NOTE]
> Se você já implantou o BookReviewsWAP projeto mas ainda deseja tentar implantar o projeto BookReviewsWSP, primeiro exclua todos os arquivos no servidor web que foram carregados durante a implantação de BookReviewsWAP e, em seguida, implantar os arquivos para BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- Todo o conteúdo do `Styles` pasta
- Todo o conteúdo do `Images` pasta (e sua subpasta `BookCovers`)
- `~/App_Code/BasePage.vb`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.vb`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.vb`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.vb`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.vb`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.vb`

A Figura 3 mostra FileZilla depois de copiar os arquivos necessários. Como você pode ver, o ASP.NET fonte arquivos de código, como `About.aspx.vb`, estão presentes no computador local (o ambiente de desenvolvimento) e o provedor de host da web (o ambiente de produção), porque os arquivos de código precisam ser implantadas no uso automático compilação.


[![Use um cliente de FTP para copiar os arquivos necessários da área de trabalho para o servidor Web no provedor de Host da Web](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**Figura 3**: usar um cliente de FTP para copiar os arquivos necessários da sua área de trabalho para o servidor Web no provedor de Host da Web ([clique para exibir a imagem em tamanho normal](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))


A experiência do usuário não é afetada pelo modelo de compilação do aplicativo. As mesmas páginas ASP.NET estão acessíveis e sua aparência e se comportam do mesmo se o site foi criado usando o modelo de projeto de aplicativo Web ou o modelo de projeto de Site.

## <a name="updating-a-web-application-on-production"></a>Atualizando um aplicativo Web na produção

Implantação e desenvolvimento de aplicativo web não são um único processo. Por exemplo, ao criar o site de revisão de catálogo, criado várias páginas e escreveu o código fornecido em Meu computador pessoal (o ambiente de desenvolvimento). Depois de atingir um determinado estado estável, eu Implantei meu aplicativo para que outras pessoas podem visitar o site e Minhas revisões de leitura. Mas a implantação não marcar o final do meu desenvolvimento neste site. Eu pode adicionar mais revisões de livros ou implementa novos recursos, como permitir que meus visitantes manuais taxa ou deixar seus próprios comentários. Essas melhorias seriam desenvolvidas no ambiente de desenvolvimento e, quando concluída, precisa ser implantado. Desenvolvimento e implantação, portanto, são cíclicos. Você pode desenvolve um aplicativo e, em seguida, implantá-lo. Embora o site está ativo e em produção, novos recursos foram adicionados e bugs corrigidos ao longo do tempo, o que exige reimplantar o aplicativo. E assim por diante e assim por diante.

Como esperado, ao reimplantar em um aplicativo web que você só precisará copiar os arquivos novos e alterados. Não é necessário reimplantar páginas inalteradas ou arquivos de suporte do servidor ou cliente (embora não haja nenhum problema em fazer isso).

> [!NOTE]
> Uma coisa para ter em mente ao usar a compilação explícita é a que sempre que você adicionar uma nova página ASP.NET para o projeto ou fazer alterações relacionadas ao código, você precisa recriar seu projeto, o que atualiza o assembly no `Bin` pasta. Consequentemente, você precisará copiar esse assembly atualizado para a produção, ao atualizar um aplicativo web em produção (juntamente com o outros novo e atualizado conteúdo).


Também entender que todas as alterações para o `Web.config` ou os arquivos a `Bin` directory para e reinicia o Pool de aplicativos do site. Se o estado da sessão é armazenado usando o `InProc` modo (o padrão), em seguida, visitantes do site perderá seu estado de sessão sempre que esses arquivos de chave são modificados. Para evitar esse problema, considere o armazenamento de sessão usando o `StateServer` ou `SQLServer` modos. Para obter mais informações sobre este tópico [modos de estado de sessão](https://msdn.microsoft.com/en-us/library/ms178586.aspx).

Finalmente, tenha em mente que reimplantar um aplicativo pode levar de alguns segundos-vários minutos, dependendo do número e tamanho dos arquivos que precisam ser copiados para o ambiente de produção. Durante esse tempo, os usuários que visitam seu site podem enfrentar erros ou comportamento estranho. Você pode "desligar" todo o seu aplicativo com a adição de uma página chamada `App_Offline.htm` para o diretório raiz do aplicativo que explica aos usuários que o site está desativado para manutenção (ou qualquer outro) e será ser fazer backup em breve. Quando o `App_Offline.htm` arquivo estiver presente, o tempo de execução do ASP.NET redireciona todas as solicitações de entrada para a página.

## <a name="summary"></a>Resumo

Implantando um aplicativo web implica a copiar os arquivos necessários do ambiente de desenvolvimento para o ambiente de produção. O meio mais comum pelo qual os arquivos são transferidos pela rede é o protocolo FTP (File Transfer) e a maioria dos provedores de host da web oferecem suporte a acesso FTP em seus servidores web. Neste tutorial, vimos como usar um cliente de FTP para implantar os arquivos necessários para o servidor web. Uma vez implantado, o site pode ser visitado por qualquer pessoa com uma conexão à Internet!

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Aplicativo\_Offline.htm e resolver o recurso de "Erros amigável do IE"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Modos de estado de sessão](https://msdn.microsoft.com/en-us/library/ms178586.aspx)

>[!div class="step-by-step"]
[Anterior](determining-what-files-need-to-be-deployed-vb.md)
[Próximo](deploying-your-site-using-visual-studio-vb.md)
