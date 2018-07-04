---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: Principais diferenças entre o IIS e o ASP.NET Development Server (VB) | Microsoft Docs
author: rick-anderson
description: Ao testar um aplicativo ASP.NET localmente, provavelmente que você está usando o Web do ASP.NET Development Server. No entanto, o site de produção é mais provável pow...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: d16e4d20f2e1fe85b6844ea714546fbae8c95b4e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377026"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>Principais diferenças entre o IIS e o ASP.NET Development Server (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip) ou [baixar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> Ao testar um aplicativo ASP.NET localmente, provavelmente que você está usando o Web do ASP.NET Development Server. No entanto, o site de produção é provavelmente IIS alimentado. Há algumas diferenças entre esses servidores web lidarem com solicitações, e essas diferenças podem ter consequências importantes. Este tutorial explora algumas das diferenças mais similar.


## <a name="introduction"></a>Introdução

Sempre que um usuário acessa um aplicativo ASP.NET seu navegador envia uma solicitação para o site. Essa solicitação é captada pelo software do servidor web, que coordena com o tempo de execução do ASP.NET para gerar e retornar o conteúdo para o recurso solicitado. O[**eu** nto **eu** nformation **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) são um conjunto de serviços que fornecem funcionalidades comuns baseados na Internet para Servidores do Windows. O IIS é o servidor web mais comumente usado para aplicativos ASP.NET em ambientes de produção; ele é provavelmente o software do servidor web que está sendo usado por seu provedor de host da web para servir ao aplicativo ASP.NET. O IIS também pode ser usado como o software do servidor web no ambiente de desenvolvimento, embora isso envolve a instalação do IIS e configurar adequadamente a ele.


O ASP.NET Development Server é uma opção de servidor web alternativos para o ambiente de desenvolvimento. Ele acompanha e é integrado ao Visual Studio. A menos que o aplicativo web tiver sido configurado para usar o IIS, o ASP.NET Development Server é iniciado automaticamente e usado como o servidor web na primeira vez em que você visitar uma página da web dentro do Visual Studio. Os aplicativos da web de demonstração criado na [ *determinando quais arquivos precisam ser implantados* ](determining-what-files-need-to-be-deployed-vb.md) tutorial foram ambos os aplicativos da web com base no sistema de arquivos que não foram configurados para usar o IIS. Portanto, ao visitar qualquer um desses sites de dentro do Visual Studio o ASP.NET Development Server é usado.

Em um mundo perfeito, ambientes de desenvolvimento e produção seriam idênticos. No entanto, como discutimos no tutorial anterior não é incomum para os ambientes de ter diferentes definições de configuração. Usar o software do servidor web diferente nos ambientes adiciona outra variável que deve ser levada em consideração ao implantar um aplicativo. Este tutorial aborda as principais diferenças entre o IIS e o ASP.NET Development Server. Por causa dessas diferenças, há cenários em que o código que é executado perfeitamente no ambiente de desenvolvimento gera uma exceção ou tem um comportamento diferente quando executado em produção.

## <a name="security-context-differences"></a>Diferenças de contexto de segurança

Sempre que o software do servidor web manipula uma solicitação de entrada associa essa solicitação com um contexto de segurança específico. Essas informações de contexto de segurança são usadas pelo sistema operacional para determinar quais ações são permitidas pela solicitação. Por exemplo, uma página ASP.NET pode incluir código que registra alguma mensagem em um arquivo no disco. Em ordem para essa página ASP.NET seja executado sem erros, o contexto de segurança deve ter o devido as permissões de nível de sistema de arquivos, ou seja, permissões no arquivo de gravação.

O ASP.NET Development Server associa as solicitações de entrada com o contexto de segurança do usuário conectado no momento. Se você estiver conectado à sua área de trabalho como um administrador, as páginas ASP.NET atendidas pelo ASP.NET Development Server terá os mesmos direitos de acesso como administrador. No entanto, solicitações ASP.NET manipuladas pelo IIS são associadas uma conta de computador específico. Por padrão, a conta de serviço de rede do computador é usada pelo IIS versões 6 e 7, embora seu provedor de host da web pode ter configurado uma conta exclusiva para cada cliente. Além disso, seu provedor de host da web provavelmente concedeu permissões limitadas a essa conta de computador. O resultado líquido é que você pode ter páginas da web que executam sem erro no ambiente de desenvolvimento, mas gera exceções relacionadas a autorização quando hospedado no ambiente de produção.

Para mostrar esse tipo de erro em ação, eu criei uma página no site resenhas de livros que cria um arquivo no disco que armazena a data e a hora mais recente alguém exibidos os *ensinar por conta própria ASP.NET 3.5 in 24 horas* examinar. Para acompanhar, abra o `~/Tech/TYASP35.aspx` da página e adicione o seguinte código para o `Page_Load` manipulador de eventos:

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> O [ `File.WriteAllText` método](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) cria um novo arquivo se ele não existe e, em seguida, grava o conteúdo especificado. Se o arquivo já existir, seu conteúdo existente será substituído.


Em seguida, visite o *ensinar por conta própria ASP.NET 3.5 in 24 horas* página de revisão de livro no ambiente de desenvolvimento usando o ASP.NET Development Server. Supondo que você efetuou logon em seu computador com uma conta que tenha as permissões adequadas para criar e modificar um arquivo de texto da web diretório de raiz do aplicativo resenha de livro é exibida os mesmos de antes, mas cada vez que a página é visitada a data e hora e do usuário  Endereço IP é armazenado no `LastTYASP35Access.txt` arquivo. Aponte seu navegador para esse arquivo; Você deve ver uma mensagem semelhante à mostrada na Figura 1.


[![O arquivo de texto contém a última data e hora que a revisão de livro foi visitada&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**Figura 1**: O arquivo de texto contém a última data e hora que a revisão de livro foi visitada ([clique para exibir a imagem em tamanho normal](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))


Implantar o aplicativo web para produção e, em seguida, visite hospedado *ensinar por conta própria ASP.NET 3.5 in 24 horas* página de revisão de livro. Neste ponto, você deve ver a página de revisão do livro de tanto como normal ou a mensagem de erro mostrada na Figura 2. Alguns provedores de host web conceder permissões de gravação para a conta do computador ASP.NET anônima, nesse caso a página funcionará sem erro. Se, no entanto, seu provedor de host da web proíbe o acesso de gravação para a conta anônima, uma [ `UnauthorizedAccessException` exceção](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) é gerado quando o `TYASP35.aspx` página tenta gravar a data e hora atuais para o `LastTYASP35Access.txt` arquivo.


[![A conta do computador padrão usada pelo IIS não tem permissões para gravar no sistema de arquivos](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**Figura 2**: O padrão máquina conta usada pelo IIS faz não tem permissões para gravação para o sistema de arquivos ([clique para exibir a imagem em tamanho normal](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))


A boa notícia é que a maioria dos provedores de host da web tenham algum tipo de ferramenta de permissões que permite que você especifique permissões de sistema de arquivos em seu site. Conceda o acesso de gravação conta anônimo ASP.NET para o diretório raiz e, em seguida, examine a página de revisão de livro. (Se necessário, entre em contato com seu provedor de host da web para obter assistência sobre como conceder permissões de gravação para a conta padrão do ASP.NET.) Neste momento, a página deve ser carregada sem erros e o `LastTYASP35Access.txt` arquivo deve ser criado com êxito.

O take distância aqui é que, como o ASP.NET Development Server opera em um contexto de segurança diferentes que o IIS, é possível que as páginas ASP.NET que ler ou gravar em um sistema de arquivos, ler ou gravar para o Log de eventos do Windows, ou ler ou gravar para o Windows  Registro irá funcionar conforme esperado no desenvolvimento, mas geram exceções quando em produção. Ao criar um aplicativo web que será implantado em um ambiente de hospedagem de web compartilhado, não ler ou gravar no Log de eventos ou o registro do Windows. Além disso, tome nota das páginas ASP.NET que ler ou gravar ao sistema de arquivos, talvez você precise conceder a leitura e gravação privilégios nas pastas apropriadas no ambiente de produção.

## <a name="differences-on-serving-static-content"></a>Diferenças em servir conteúdo estático

Outra diferença principal entre o IIS e o ASP.NET Development Server é como eles lidam com solicitações de conteúdo estático. Cada solicitação que entra no ASP.NET Development Server, para uma página ASP.NET, uma imagem ou um arquivo JavaScript, é processada pelo tempo de execução do ASP.NET. Por padrão, o IIS invoca somente o tempo de execução do ASP.NET quando chega uma solicitação para um recurso do ASP.NET, como uma página da web, um serviço Web e assim por diante. Solicitações de conteúdo estático – imagens, arquivos CSS, arquivos JavaScript, arquivos PDF, arquivos ZIP e assim por diante - são recuperadas pelo IIS sem o envolvimento do tempo de execução do ASP.NET. (É possível instruir o IIS para trabalhar com o tempo de execução do ASP.NET ao servir conteúdo estático; consulte a seção "Realizando a autenticação baseada em formulários e autenticação de URL em arquivos estáticos com o IIS 7" neste tutorial para obter mais informações.)

O tempo de execução do ASP.NET realiza uma série de etapas para gerar o conteúdo solicitado, incluindo (identificando o solicitante) de autenticação e autorização (determinar se o solicitante tem permissão para exibir o conteúdo solicitado). É uma forma popular de autenticação *autenticação baseada em formulários*, no qual um usuário é identificado, inserindo suas credenciais - normalmente, um nome de usuário e senha - nas caixas de texto em uma página da web. Após validar suas credenciais, o site armazena uma *tíquete de autenticação* cookie no navegador do usuário, que é enviado com cada solicitação subsequente para o site e é usado para autenticar o usuário. Além disso, é possível especificar *autorização de URL* regras que determinam o que os usuários podem ou não é possível acessar uma pasta específica. Muitos sites ASP.NET usam autenticação baseada em formulários e a autorização de URL para dar suporte a contas de usuário e para definir as partes do site que só são acessíveis a usuários autenticados ou usuários que pertencem a uma determinada função.

> [!NOTE]
> Para obter uma análise minuciosa do ASP. Autenticação baseada em formulários da rede, autorização de URL e outros recursos relacionados à conta de usuário, certifique-se de fazer check-out minha [tutoriais de segurança de site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Considere um site que oferece suporte a contas de usuário usando a autorização baseada em formulários e tem uma pasta que, usando a autorização de URL está configurada para permitir que somente usuários autenticados. Imagine que essa pasta contém páginas ASP.NET e arquivos PDF e que a intenção é que somente usuários autenticados podem exibir esses arquivos PDF.

O que acontece se um visitante tenta exibir um desses arquivos PDF inserindo a URL diretamente na barra de endereço do seu navegador? Para descobrir, vamos criar uma nova pasta no site resenhas de livros, adicionar alguns arquivos PDF e configurar o site para usar a autorização de URL para impedir que usuários anônimos de visitar essa pasta. Se você baixar o aplicativo de demonstração você verá que eu criei uma pasta chamada `PrivateDocs` e adicionado a um PDF do meu [tutoriais de segurança de site](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (como ajuste!). O `PrivateDocs` pasta também contém um `Web.config` arquivo que especifica as regras de autorização de URL para negar a usuários anônimos:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

Por fim, eu configurei o aplicativo web para usar a autenticação baseada em formulários, atualizando o arquivo Web. config no diretório raiz, substituindo:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

Por:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

Usando o ASP.NET Development Server, visite o site e insira a URL direta para um dos arquivos PDF na barra de endereços do navegador. Se você baixou o site associado a este tutorial, que a URL deve ser semelhante: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Inserir esta URL na barra de endereços faz com que o navegador para enviar uma solicitação para o ASP.NET Development Server para o arquivo. A ASP.NET Development Server entrega a solicitação para o tempo de execução do ASP.NET para processamento. Como estamos ainda não tiver fez logon e como o `Web.config` no `PrivateDocs` pasta está configurada para negar o acesso anônimo, o tempo de execução do ASP.NET redireciona nos automaticamente para a página de logon, `Login.aspx` (veja a Figura 3). Ao redirecionar o usuário para a página de logon, o ASP.NET inclui um `ReturnUrl` parâmetro querystring que indica a página do usuário estava tentando exibir. Depois de fazer logon com êxito o usuário pode ser retornado para esta página.


[![Usuários não autorizados são automaticamente redirecionados para a página de logon](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**Figura 3**: os usuários não autorizados são automaticamente redirecionados para a página de logon ([clique para exibir a imagem em tamanho normal](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))


Agora vamos ver como isso se comporta em produção. Implantar seu aplicativo e insira a URL direta dentre os PDFs o `PrivateDocs` pasta em produção. Isso solicita que seu navegador para enviar uma solicitação IIS para o arquivo. Como um arquivo estático é solicitado, o IIS recupera e retorna o arquivo sem invocar o tempo de execução do ASP.NET. Como resultado, não havia nenhuma verificação de autorização de URL executada; o conteúdo do PDF supostamente particular é acessível a qualquer pessoa que souber a URL direta para o arquivo.


[![Os usuários anônimos poderão baixar os arquivos de PDF de privada inserindo a URL direta para o arquivo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**Figura 4**: os usuários anônimos podem baixar a privada PDF arquivos por inserir a URL direta para o arquivo ([clique para exibir a imagem em tamanho normal](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Executar a autenticação baseada em formulários e autenticação de URL em arquivos estáticos com o IIS 7

Existem algumas técnicas que você pode usar para proteger o conteúdo estático de usuários não autorizados. IIS 7 introduziu o *pipeline integrado*, que combina o fluxo de trabalho do IIS com o fluxo de trabalho do tempo de execução ASP.NET. Em resumo, você pode instruir o IIS para invocar módulos de autorização e autenticação do tempo de execução ASP.NET todas as solicitações de entrada (incluindo conteúdo estático, como arquivos PDF). Entre em contato com seu provedor de host da web para saber como configurar seu site para usar o pipeline integrado.

Depois que o IIS foi configurado para usar o pipeline integrado adicione a seguinte marcação para o `Web.config` arquivo no diretório raiz:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

Essa marcação instrui o IIS 7 para usar os módulos de autenticação e autorização baseada em ASP.NET. Reimplante seu aplicativo e, em seguida, visite novamente o arquivo PDF. Desta vez, quando o IIS lida com a solicitação-lógica de autenticação e autorização do tempo de execução ASP.NET uma oportunidade para inspecionar a solicitação. Porque somente usuários autenticados tenham autorização para exibir o conteúdo no `PrivateDocs` pasta, o visitante anônimo é automaticamente redirecionada para a página de logon (consulte novamente a Figura 3).

> [!NOTE]
> Se seu provedor de host da web ainda estiver usando o IIS 6, em seguida, você não pode usar o recurso de pipeline integrado. Uma solução alternativa é colocar seus documentos particulares em uma pasta que proíbe o acesso HTTP (tal como `App_Data`) e, em seguida, crie uma página para atender a esses documentos. Esta página pode ser chamada `GetPDF.aspx`e é passado o nome do PDF por meio de um parâmetro querystring. O `GetPDF.aspx` página pela primeira vez, verificaria a que o usuário tem permissão para exibir o arquivo e, nesse caso, use o [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) método para enviar o conteúdo do arquivo PDF solicitado para o cliente solicitante. Essa técnica também funciona para o IIS 7 se você habilitar o pipeline integrado.


## <a name="summary"></a>Resumo

Aplicativos da Web em um ambiente de produção são hospedados usando software de servidor de web IIS da Microsoft. No ambiente de desenvolvimento, no entanto, o aplicativo pode ser hospedado usando o IIS ou o ASP.NET Development Server. O ideal é que o mesmo software de servidor web deve ser usado em ambos os ambientes porque usando softwares diferentes adiciona outra variável na combinação. No entanto, a facilidade de uso do ASP.NET Development Server torna uma opção atraente no ambiente de desenvolvimento. A boa notícia é que há apenas algumas diferenças fundamentais entre o IIS e o ASP.NET Development Server, e se você estiver ciente dessas diferenças você pode tomar medidas para ajudar a garantir que o aplicativo funciona e funciona da mesma forma, independentemente do ambiente.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [Integração do ASP.NET com o IIS 7.0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Usando a autenticação de fóruns do ASP.NET com todos os tipos de conteúdo no IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (vídeo)
- [Servidores Web no Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Anterior](common-configuration-differences-between-development-and-production-vb.md)
> [Próximo](deploying-a-database-vb.md)
