---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: Introdução ao ASP.NET Web Pages – publicar um Site usando o WebMatrix | Microsoft Docs
author: tfitzmac
description: Este tutorial é a parte final no conjunto de tutoriais que apresenta a páginas da Web ASP.NET e o Microsoft WebMatrix. Ele discute como publicar seu site t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: c6fa7692527b7aa65e93cd57ed5bd56f42e54bd6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373481"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Introdução ao ASP.NET Web Pages - publicar um Site usando o WebMatrix
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial é a parte final no conjunto de tutoriais que apresenta a páginas da Web ASP.NET e o Microsoft WebMatrix. Ele discute como publicar seu site na Internet para que outras pessoas possam trabalhar com ele. Ele pressupõe que você tenha concluído a série por meio [criar uma aparência consistente para Sites de páginas da Web do ASP.NET](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Você aprenderá como publicar seu site usando:
> 
> - Microsoft Azure
> - Empresa de hospedagem da Web


## <a name="about-publishing-your-site"></a>Sobre como publicar seu Site

Até agora, você fez seu trabalho em um computador local, incluindo testes de suas páginas. Para executar sua<em>. cshtml</em> páginas, você já usou o servidor web que está incorporado ao WebMatrix, ou seja, o IIS Express. Mas é claro, ninguém pode ver o site que você criou, exceto que você. Para permitir que outras pessoas trabalhem com o seu site, você precisa publicá-lo com a Internet.

A menos que você já tiver acesso a um servidor web pública, a publicação significa que você precisa ter uma conta com um *plataforma de nuvem* ou um *provedor de hospedagem*. Uma plataforma de nuvem, como o Microsoft Azure fornece infraestrutura sob demanda para seus aplicativos. Um provedor de hospedagem é uma empresa que possui servidores web acessível publicamente e que será alugar você espaço para o seu site. Planos de execução de alguns dólares por mês (ou até mesmo gratuitos) de hospedagem para sites pequenos para centenas de dólares por mês para sites comerciais de alto volume.

> [!NOTE]
> Você pode ter acesso a um servidor web público por meio do provedor de serviço da internet (ISP) que você usa para obter o serviço de internet em casa. No entanto, seu provedor de hospedagem deve dar suporte a páginas da Web ASP.NET. Muitos provedores de Internet não, mas sempre vale a pena verificar.


Neste tutorial, daremos a você uma visão geral de como publicar. Não é prático fornecer detalhes exatos para tudo, porque o processo um pouco diferente para cada provedor de hospedagem. Mas, você obterá uma boa ideia de como funciona o processo.

Este tutorial contém quatro seções:

1. [Como configurar a página padrão](#defaultpage)
2. Publicação (escolha uma das seguintes opções)  
 a. [Publicar seu Site no Microsoft Azure](#azure)  
 b. [Publicar seu Site em uma empresa de hospedagem na Web](#host)
3. [Atualizar o Site ao vivo: republicação](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Como configurar a página padrão

Quando um usuário navega para o endereço base para seu site da web, a página padrão para o seu site é exibida ao usuário. Por exemplo, quando default. htm é definida como a página padrão para o site em www.contoso.com, navegando até <strong>www.contoso.com</strong> é o mesmo que navegando para <strong>www.contoso.com/Default.htm</strong>.

No momento, seu site utiliza **cshtml** como a página padrão. Esta página é bom para sua página padrão, mas neste tutorial você não adicionou nenhum conteúdo para a página para que ele exibiria uma página em branco. Abra default. cshtml e substitua o conteúdo com o código a seguir.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Agora seu site está pronto para publicação. Primeiro, você verá como implantar o site para o Azure e como implantá-lo em uma empresa de hospedagem na web. Qualquer opção funciona para seu site da web e você só precisa seguir uma das opções de implantação.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Publicar seu Site no Microsoft Azure

Este tutorial primeiro mostrará a você como implantar seu site para o Microsoft Azure. Ao entrar com uma conta da Microsoft, você pode criar até 10 sites gratuitos no Azure. Esses sites gratuitos fornecem uma maneira conveniente para testar seus sites. Você sempre pode excluir este site de exemplo posteriormente para evitar o uso de todos os seus sites gratuitos. Você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Na faixa de opções do WebMatrix, clique o **publicar** botão.

!['Publicar' botão na faixa de opções do WebMatrix](publishing/_static/image1.png)

O **publicar seu Site** caixa de diálogo é exibida. Se você não entrou sua conta da Microsoft, a caixa de diálogo conterá uma **Introdução ao Azure** link. Clique neste link.

![Publique seu site](publishing/_static/image2.png)

Se você não tiver entrado uma conta da Microsoft, novamente você terá a oportunidade de entrar. Você deve entrar uma conta da Microsoft para publicar seu site no Azure.

![Entrar](publishing/_static/image3.png)

Depois de entrar sua conta da Microsoft, a caixa de diálogo contém links para criar um novo site no Azure ou se conectar a um de seus sites existentes no Azure.

![Criar novo Site](publishing/_static/image4.png)

Selecione **criar um novo site**.

Se tiver nomeado seu projeto **WebPagesMovies**, o nome padrão para seu site estará **webpagesmovies.azurewebsites.net**. Esse nome padrão é provavelmente não está disponível, conforme indicado pelo ponto de exclamação vermelho.

![nome do site da Web padrão](publishing/_static/image5.png)

Altere o nome do site para algo que está disponível e selecione um local perto de seu local.

![nome do site alterado](publishing/_static/image6.png)

Clique em **OK**.

O WebMatrix performss um teste para determinar se o servidor é compatível com o seu site.

![teste de compatibilidade](publishing/_static/image7.png)

Selecione **continuar**.

Os resultados do teste de compatibilidade são exibidos.

![resultados de compatibilidade](publishing/_static/image8.png)

Selecione **continuar**.

O WebMatrix exibe os arquivos e bancos de dados que serão publicados no site. Como essa é a primeira vez que você está publicando o site, todos os arquivos são listados. Você pode desmarcar um arquivo que não está pronto para ser publicado. Em publicações subsequentes, somente os arquivos que foram alterados serão exibidos. Ver [atualizar o Site ao vivo: republicação](#update).

![visualização de publicação](publishing/_static/image9.png)

Selecione **continuar**.

Depois que o site tiver sido implantado no Azure, será exibida uma mensagem que indica que a implantação for concluída.

![publicação concluída](publishing/_static/image10.png)

O site e o banco de dados foi publicados no Azure e agora estão disponíveis ao público. Clique no link na mensagem que indica a publicação estiver concluída e agora você verá seu site implantado. Você ou qualquer pessoa com acesso à Internet pode adicionar ou modificar registros no banco de dados.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Publicar seu Site em uma empresa de hospedagem na Web

Se você decidir não publicar no Azure, você pode publicar em vez disso, seu site para uma empresa de hospedagem na web.

Clique o **localizar host da web** link.

!['Find hospedagem na web' botão na caixa de diálogo Configurações de publicação](publishing/_static/image12.png)

Ir para uma página no site da Microsoft que lista os provedores de hospedagem que dão suporte ao ASP.NET.

![Página no site da Microsoft que lista os provedores de hospedagem](publishing/_static/image13.png)

Obviamente, pode ser difícil saber exatamente quais recursos de hospedagem que pode exigir a longo prazo. Aqui estão algumas coisas a considerar:

- Para fins do site WebPagesMovies, você não precisa ter um complemento separado para o SQL Server, que frequentemente custa extra. No seu site, você está usando o SQL Server Compact Edition, que é independente. No entanto, talvez seja necessário acesso ao SQL Server para algum trabalho futuro site que faria. Se você achar que você pode, certifique-se de que você pode adicionar funcionalidade do SQL Server mais tarde.
- Verifique se o provedor de hospedagem oferece suporte ao protocolo de publicação de implantação da Web. Você pode publicar usando o protocolo FTP, mas é mais conveniente usar a implantação da Web.

Alguns sites oferecem um período de avaliação gratuita. Uma avaliação gratuita é uma boa maneira de testar a publicação e de hospedagem enquanto você ainda está experimentando com o WebMatrix e páginas da Web ASP.NET.

Escolha um que você deseja. Para este tutorial, selecionamos DiscountASP.NET, porque enquanto estamos estivesse criando o tutorial, essa empresa tinha uma promoção que permitem que as pessoas hospede um site gratuito por alguns meses.

> [!NOTE]
> Nossa escolha de um provedor de hospedagem para este tutorial não deve ser interpretada como um endosso dessa empresa sobre qualquer outro. Mas tivemos que escolher uma para fins ilustrativos e DiscountASP.NET é uma das muitas empresas que dá suporte a páginas da Web ASP.NET e o protocolo de implantação da Web para publicação.


Normalmente, depois que você se inscrever com o provedor de hospedagem, a empresa envia um email que contém um nome de usuário e senha, a URL do servidor web e assim por diante. Se a empresa de hospedagem oferece suporte ao protocolo de implantação da Web, eles podem enviar você é um arquivo que contém as configurações de publicação ou permitem que você baixe um. Um arquivo de configurações de publicação simplifica o processo para você.

Quando você se inscrever e está pronto para publicar, clique no **publicar** botão na faixa de opções do WebMatrix. O **configurações de publicação** caixa de diálogo é exibida.

Se o provedor de hospedagem enviado a você um arquivo de configurações de publicação, clique no **importar configurações de publicação** vincular e importe o arquivo. Se você não tiver um arquivo de configurações de publicação, preencha os campos usando os valores que a empresa de hospedagem enviados a você por email. Aqui está o que o **configurações de publicação** caixa de diálogo pode ter aparência quando você terminar:

![Configurações de publicação preenchido na caixa de diálogo 'Configurações de publicação'](publishing/_static/image14.png)

Clique em **validar Conexão**. Se tudo estiver okey, a caixa de diálogo informa **conectado com êxito**, que significa que ele pode se comunicar com o servidor do provedor de hospedagem.

![Mensagem de êxito se publicar configurações estão corretas](publishing/_static/image15.png)

Se houver um problema, o WebMatrix faz o melhor para dizer o que é o problema:

![Mensagem de erro se não houver um problema com as configurações de publicação](publishing/_static/image16.png)

Clique em **salvar** para salvar suas configurações. O WebMatrix oferece para executar um teste para certificar-se de que ele possa se comunicar corretamente com o site de hospedagem:

![Mensagem oferecendo para executar um teste do processo de publicação](publishing/_static/image17.png)

Clique em **Sim**. O WebMatrix carrega alguns arquivos de exemplo para o provedor de hospedagem. Quando o teste de compatibilidade é concluído, o WebMatrix relata os resultados:

![Resultados do teste de publicação](publishing/_static/image18.png)

Se você estiver pronto para começar, vá em frente e clique em **continuar** para iniciar o processo de publicação de verdade. O WebMatrix descobre quais arquivos estão em seu site e já estão no servidor de host (no momento, nenhum) e oferece uma visualização do processo de publicação:

![Visualização dos quais arquivos que carregará o processo de publicação](publishing/_static/image19.png)

A lista de arquivos a serem publicados inclui páginas da web que você criou, como *Movies.cshtml*. A lista também inclui arquivos para os auxiliares que você tenha instalado, os arquivos para executar o SQL Server Compact Edition para seu banco de dados e assim por diante. Como resultado, o processo de publicação inicial pode ser significativa.

Clique em **Continue**. O WebMatrix copia os arquivos ao servidor do provedor de hospedagem. Quando estiver pronto, os resultados são relatados na barra de status:

![Mensagem de barra de status quando o processo de publicação foi concluída com êxito](publishing/_static/image20.png)

Para ver seu site ativo, clique no link na barra de status. Adicione *filmes* para a URL, e você verá o *Movies.cshtml* arquivo que você criou:

![O site ao vivo mostrando a página de filmes](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Atualizar o Site ao vivo: republicação

Depois de publicar seu site (para Azure ou em uma empresa de hospedagem na web), há duas cópias dele &mdash; a versão em seu computador e a versão do provedor de serviços. Provavelmente você desejará continuar a desenvolver o site (não bastasse, como parte do próximo conjunto de tutoriais). Quando você fizer isso, você precisará publicar novamente seu site para copiar as alterações do seu computador para o provedor de serviços. O processo de publicação no WebMatrix pode determinar quais arquivos foram alterados em seu site e publicar apenas os arquivos.

Para ver como funciona a republicação, abra o *Movies.cshtml* do site, faça algumas pequenas alterações e, em seguida, salve o arquivo. Por exemplo, altere o título para `Movies - Updated`.

Clique o **publicar** botão na faixa de opções. O WebMatrix determina o que é alterado e exibe uma visualização dos arquivos, que ele publicará.

![A caixa de diálogo 'Publicar' mostrando alterados arquivos prontos para republicação](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> Por padrão, o WebMatrix publica seu banco de dados (*sdf* arquivo) somente na primeira vez que você publicar o site. Depois que o site é publicado e as pessoas estão interagindo com o site, o banco de dados no site ativo normalmente tem dados reais do site. Você precisa ter muito cuidado para não substituir o banco de dados ao vivo com o *sdf* arquivo no seu computador, que geralmente contém somente os dados de teste. É por isso que você veja o aviso **publicação irá substituir quaisquer bancos de dados remotos**, e por que a caixa de seleção *WebPagesMovies.sdf* está desmarcada por padrão.


Clique em **Continue**. O WebMatrix publica os arquivos alterados e mostra uma mensagem de êxito, como ele fez na primeira vez que você publicou.

Vá para o site ativo (você pode clicar no link na mensagem de êxito, se ele ainda está mostrando) e verifique se que a alteração foi publicada.

> [!TIP] 
> 
> **Editar arquivos remotamente**
> 
> Como alternativa à alteração de seu site e, em seguida, republicar, você pode editar os arquivos remotos diretamente no WebMatrix. Nesse cenário, você abre um arquivo que é o provedor de serviço e o WebMatrix baixa uma cópia para edição. Sempre que você salvar o arquivo, o WebMatrix envia as alterações para o site.
> 
> Edição remota é uma maneira fácil de fazer alterações em seu site ativo. No entanto, as alterações feitas dessa maneira não estão sincronizadas com os arquivos em seu site local. Para sincronizar os arquivos locais com o site remoto, você pode baixar os arquivos remotos. Esse processo funciona muito parecida com a publicação, exceto na ordem inversa.
> 
> Não descreveremos mais sobre os recursos de edição remota e remote-download do WebMatrix aqui. Eles são bastante úteis se várias pessoas precisam trabalhar no mesmo site em computadores diferentes. Para obter mais informações, consulte [publicar e editar um local remoto com o WebMatrix 2 Beta](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Recursos adicionais

- [Fórum do WebMatrix ASP.NET Web Pages ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), um ótimo lugar para postar perguntas e obtenha respostas.

> [!div class="step-by-step"]
> [Anterior](layouts.md)
