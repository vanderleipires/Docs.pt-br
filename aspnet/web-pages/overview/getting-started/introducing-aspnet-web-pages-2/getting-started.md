---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: "Introdução a páginas da Web do ASP.NET - Introdução | Microsoft Docs"
author: tfitzmac
description: "O WebMatrix não é recomendado como um ambiente de desenvolvimento integrado para páginas da Web do ASP.NET. Use o Visual Studio ou o código do Visual Studio. Este guia um..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: a6789ee75b4ca6e9443681cc7ec0bd3ab94cedcd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="introducing-aspnet-web-pages---getting-started"></a>Introdução a páginas da Web ASP.NET - Introdução
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > O WebMatrix não é recomendado como um ambiente de desenvolvimento integrado para páginas da Web do ASP.NET. Use [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) ou [código do Visual Studio](https://code.visualstudio.com/).
> 
> 
> Este guia e o aplicativo fornece uma visão geral das páginas da Web do ASP.NET (versão 2 ou posterior) e a sintaxe do Razor, uma estrutura leve para a criação de sites dinâmicos. Ele também apresenta o WebMatrix, uma ferramenta para criar páginas e sites.
> 
> **Nível de**: novo para páginas da Web ASP.NET.  
> **Presume-se que as habilidades**: HTML, folhas de estilo em cascata (CSS) básica.
> 
> O que você aprenderá no primeiro tutorial do conjunto:
> 
> - Qual tecnologia de páginas da Web ASP.NET e o que ele é.
> - O que é o WebMatrix.
> - Como instalar tudo.
> - Como criar um site usando o WebMatrix.
>   
> 
> Recursos/tecnologias abordadas:
> 
> - Microsoft Web Platform Installer.
> - WebMatrix.
> - *. cshtml* páginas
>   
> 
> Mike Pope originalmente gravou neste tutorial. Tom FitzMacken atualizá-lo para o Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - WebMatrix 3


## <a name="what-should-you-know"></a>O que você deve saber?

Estamos supondo que você está familiarizado com:

- **HTML**. Nenhum conhecimento profundo é necessário. Não explicaremos HTML, mas também não usamos nada complexas. Forneceremos links para tutoriais do HTML onde achamos que eles são úteis.
- **Folhas de estilo em cascata (CSS)**. Mesmo que com HTML.
- **Ideias de banco de dados básicos**. Se você tiver usado uma planilha de dados e classificados e filtrados de dados, que é o nível de conhecimento geralmente pressupomos para este conjunto de tutorial.

Também pressupomos que você está interessado em aprender a programação básica. Páginas da Web ASP.NET usam uma linguagem de programação c#. Você não precisa ter qualquer plano de fundo em programação, apenas um interesse nele. Se você já tenha escrito qualquer JavaScript em uma página da web antes, você tem muita em segundo plano.

Observe que se você estiver familiarizado com programação, talvez descubra que este tutorial é definida inicialmente move lentamente enquanto trouxemos para programadores de novo rapidamente. Como podemos passar por alguns tutoriais primeiro, no entanto, haverá menos básicas de programação para explicar e coisas moverá ao longo em um clipe mais rápido.

## <a name="what-do-you-need"></a>O que você precisa?

A seguir, o que será necessário:

- Um computador que está executando o Windows 8, Windows 7, Windows Server 2008 ou Windows Server 2012.
- Uma conexão de internet ao vivo.
- Privilégios de administrador (necessários para o processo de instalação).

## <a name="what-is-aspnet-web-pages"></a>O que é a páginas da Web ASP.NET?

Páginas da Web ASP.NET é uma estrutura que você pode usar para criar páginas da web dinâmicas. Uma página de web HTML simples é estático; seu conteúdo é determinado pelo fixa marcação HTML que está na página. Páginas dinâmicas, como aqueles criados com páginas da Web do ASP.NET permitem que você crie o conteúdo da página em tempo real, por meio de código.

Páginas dinâmicas permitem que você faça todos os tipos de itens. Você pode solicitar um usuário usando um formulário de entrada e, em seguida, altere a página exibe ou a aparência. Pode levar informações de um usuário, salvá-la em um banco de dados e, em seguida, listá-lo mais tarde. Você pode enviar email de seu site. Você pode interagir com outros serviços na web (por exemplo, um serviço de mapeamento) e produzir páginas que se integram informações dessas fontes.

## <a name="what-is-webmatrix"></a>O que é o WebMatrix?

O WebMatrix é uma ferramenta que integra um editor de página da web, um utilitário de banco de dados, um servidor web para testar a páginas e recursos para publicar seu site na Internet. O WebMatrix é gratuito e é fácil de instalar e fácil de usar. (Também funciona apenas páginas em HTML, bem como a outras tecnologias, como PHP.)

Você não fizer isso, na verdade, *ter* usar WebMatrix para trabalhar com páginas da Web do ASP.NET. Você pode criar páginas usando um texto de editor, por exemplo e testar páginas usando um servidor web que você tem acesso ao. No entanto, o WebMatrix facilita todos os, portanto, esses tutoriais usará o WebMatrix em todo.

## <a name="about-these-tutorials"></a>Sobre esses tutoriais

Este tutorial conjunto é uma introdução para como usar páginas da Web ASP.NET. Há 9 tutoriais total neste conjunto de tutorial de Introdução. Ele 's parte de uma série de conjuntos de tutorial que acompanha você da ASP.NET Web Pages iniciante à criação de sites da Web real, profissionais.

Este tutorial primeiro definir concentra-se em mostrando os fundamentos de como trabalhar com páginas da Web do ASP.NET. Quando estiver pronto, você pode trabalhar com conjuntos de tutorial adicionais que pegar em que este termina e que explorar páginas da Web mais detalhadamente.

Podemos deliberadamente vá fácil as explicações detalhadas. E sempre que mostramos algo, para este tutorial conjunto é sempre escolha da maneira que acreditamos que é mais fácil de entender. Conjuntos de tutorial posteriores em mais detalhes e mostram abordagens mais eficientes ou mais flexíveis (também divertidas). Mas esses tutoriais exigem que você entender os princípios básicos primeiro.

O conjunto de tutorial que apenas iniciou aborda esses recursos de páginas da Web do ASP.NET:

- Introdução e obter tudo instalado. (Que é o tutorial, você está lendo.)
- Noções básicas de programação de páginas da Web ASP.NET.
- Criando um banco de dados.
- Criar e processar um formulário de entrada do usuário.
- Adicionar, atualizar e excluir dados no banco de dados.

## <a name="what-will-you-create"></a>O que você criará?

Este tutorial definido e os demais giram em torno de um site onde você pode listar filmes que você deseja. Você poderá inserir filmes, editá-los e listá-los. Aqui estão algumas das páginas que você criará neste tutorial conjunto. O primeiro deles mostra o filme listando página que você criará:

![Aplicativo de filme terminou mostrando uma lista de filmes](getting-started/_static/image1.png)

E aqui está a página que permite adicionar novas informações do filme ao seu site:

![Aplicativo de filme concluído mostrando a página Adicionar filme](getting-started/_static/image2.png)

Compilação de conjuntos de tutorial subsequentes neste defina e adicione mais funcionalidades, como carregar imagens, permitindo que as pessoas entrar, enviar email e integrando mídia social.

## <a name="see-this-app-running-on-azure"></a>Consulte este aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo web em tempo real? Você pode implantar uma versão completa do aplicativo para sua conta do Azure, clicando no botão a seguir.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você não tiver uma conta, você tem as seguintes opções:

- [Abra uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -você obtém créditos você pode usar para experimentar os serviços do Azure pagos e mesmo depois que eles são usados até que você pode manter a conta e livre de usar os serviços do Azure.
- [Ativar os benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -assinatura do MSDN fornece créditos de cada mês em que você pode usar para os serviços do Azure pagos.

## <a name="installing-everything"></a>Instalar tudo

Você pode instalar tudo usando o Web Platform Installer da Microsoft. Na verdade, instale o instalador e, em seguida, usá-lo para instalar todo o resto.

Para usar as páginas da Web, você precisa ter pelo menos Windows XP com SP3 instalado, ou Windows Server 2008 ou posterior.

Sobre o [página da Web](../../../index.md) do site do ASP.NET, clique em **instalar**.

![Exibição de site da Web do ASP.NET &quot;instalar o WebMatrix&quot; botão](getting-started/_static/image3.png)

Você será solicitado a aceitar os termos de licença e a declaração de privacidade antes de instalar o WebMatrix.

![aceitar o termo para iniciar a instalação](getting-started/_static/image4.png)

Clique em **executar** para iniciar a instalação. (Se você deseja salvar o instalador, clique em **salvar** e, em seguida, execute o instalador da pasta onde você salvou.)

![](getting-started/_static/image5.png)

O Web Platform Installer é exibido, pronto para instalar o WebMatrix. Clique em **Instalar**.

![](getting-started/_static/image6.png)

O processo de instalação detecta que ele tem a instalar no computador e inicia o processo. Dependendo exatamente o que deve ser instalado, o processo pode levar de alguns instantes vários minutos. Selecione **aceito** para aceitar os termos de licença.

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Quando estiver pronto, o processo de instalação pode iniciar automaticamente o WebMatrix. Caso contrário, no Windows, do **iniciar** menu, inicie **Microsoft WebMatrix**.

Quando você inicia o WebMatrix pela primeira vez, você terá a oportunidade de entrar no Microsoft Azure com sua conta da Microsoft. Ao entrar, você receberá 10 aplicativos web gratuitos por meio do Azure. Esses aplicativos web gratuitos fornecem uma maneira conveniente para testar seus aplicativos. Se você ainda não tiver uma conta do Azure, mas você tem uma assinatura do MSDN, você pode [ativar os benefícios de assinatura do MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). Caso contrário, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Você não precisa entrar no momento para continuar este tutorial. Se você não entrar agora, você ainda terá a opção para entrar mais tarde. A última [tópico](publishing.md) neste tutorial série aborda como implantar o site para o Azure; portanto, você precisa entrar para concluir esse tópico.

Neste ponto, a entrar com sua conta da Microsoft ou selecione **agora não** no canto inferior direito.

![Entrar](getting-started/_static/image7.png)

Para começar, você criará um site da Web em branco e adicionar uma página. Um tutorial posterior neste conjunto, você experimentar um dos modelos internos do site.

Na janela de início, clique em **novo**.

![Tela de inicialização do WebMatrix](getting-started/_static/image8.png)

Os modelos são arquivos pré-criados e páginas para diferentes tipos de sites. Para ver todos os modelos que estão disponíveis por padrão, selecione a opção da Galeria de modelos.

![Selecione Galeria de modelos](getting-started/_static/image9.png)

No **início rápido** janela, selecione **Site vazio** do **ASP.NET** grupo e nomeie o novo site "WebPagesMovies".

![Janela de início rápido do WebMatrix com o modelo de Site vazio selecionado](getting-started/_static/image10.png)

Clique em **Avançar**.

Se você entrou sua conta da Microsoft, você terá a oportunidade de criar o site no Azure. Com base no nome do seu site, o nome padrão do **WebPagesMovies.azurewebsites.net** é sugerida; no entanto, o ponto de exclamação indica que esse nome não está disponível no Windows Azure. Para simplificar, selecione **ignorar** para ignorar a criação do site no Azure no momento. Posteriormente nesta série, publicaremos o site para o Azure.

![Criar site do azure](getting-started/_static/image11.png)

O WebMatrix cria e abre o site:

![Novo site WebPagesMovies abertos no WebMatrix](getting-started/_static/image12.png)

Na parte superior, há uma barra de ferramentas de acesso rápido e uma faixa de opções. Na parte inferior esquerda, você vir o seletor de espaço de trabalho onde você alternar entre as tarefas (**Site**, **arquivos**, **bancos de dados**, **relatórios**). À direita é o painel de conteúdo para o editor e relatórios. E, na parte inferior, ocasionalmente, verá uma barra de notificação para mensagens.

Você aprenderá mais sobre o WebMatrix e seus recursos, como percorrer esses tutoriais.

## <a name="creating-a-web-page"></a>Criando uma página da Web

Para se familiarizar com o WebMatrix e páginas da Web ASP.NET, você criará uma página simples.

No seletor de espaço de trabalho, selecione o **arquivos** espaço de trabalho. Este espaço de trabalho permite que você trabalhe com arquivos e pastas. O painel esquerdo mostra a estrutura de arquivos do seu site. As alterações de faixa de opções para mostrar as tarefas relacionadas ao arquivo.

![Espaço de trabalho do arquivo no WebMatrix](getting-started/_static/image13.png)

Na faixa de opções, clique na seta sob **novo** e, em seguida, clique em **novo arquivo**.

![Usando o &quot;novo&quot; comando na faixa de opções para criar um novo arquivo](getting-started/_static/image14.png)

O WebMatrix exibe uma lista de tipos de arquivo. Selecione **CSHTML**e no **nome** , digite "Olámundo". Uma página CSHTML é uma página da Web do ASP.NET.

![Criando uma nova página CSHTML nomeados HelloWorld.cshtml](getting-started/_static/image15.png)

Clique em **OK**.

O WebMatrix cria a página e abre-o no editor.

![A nova página HelloWorld no editor do WebMatrix](getting-started/_static/image16.png)

Como você pode ver, a página contém principalmente marcação HTML comum, com exceção de um bloco na parte superior que tem esta aparência:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Isso para adicionar o código, como você verá em breve.

Observe que as diferentes partes da página &mdash; os nomes dos elementos, atributos e texto, além de bloco na parte superior, estão todos em cores diferentes. Isso é chamado de *realce de sintaxe*, e ela torna mais fácil manter Limpar tudo. É um dos recursos que torna mais fácil trabalhar com páginas da web no WebMatrix.

Adicionar conteúdo para o `<head>` e `<body>` elementos como no exemplo a seguir. (Se desejar, você pode apenas copiar o seguinte bloco e substitua toda a página existente por este código.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Na barra de ferramentas de acesso rápido ou no **arquivo** menu, clique em **salvar**.

![Botão Salvar no WebMatrix ferramentas de acesso rápido](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>A página de teste

No **arquivos** espaço de trabalho, clique o *HelloWorld.cshtml* página e, em seguida, clique em **iniciar no navegador**.

![Executando uma página com o botão Executar na faixa de opções do WebMatrix](getting-started/_static/image18.png)

O WebMatrix inicia um servidor da web interno (IIS Express) que você pode usar para testar páginas em seu computador. (Sem o IIS Express no WebMatrix, você precisa publicar a página em um servidor web em algum lugar antes de você pode testá-lo.) A página é exibida no navegador padrão.

![&quot;Olá, mundo&quot; página em execução no navegador](getting-started/_static/image19.png)

Observe que, quando você testar uma página no WebMatrix, a URL no navegador é algo como `http://localhost:33651/HelloWorld.cshtml.` o nome *localhost* refere-se a um servidor local, o que significa que a página é atendida por um servidor web que está em seu próprio computador. Conforme observado, o WebMatrix inclui um programa de servidor da web denominado IIS Express que é executado quando você iniciar uma página.

O número após *localhost* (por exemplo, *localhost:33651*) refere-se a uma *número da porta* em seu computador. Este é o número do "canal" que usa o IIS Express para este site específico. O número da porta é selecionado aleatoriamente no intervalo 1024 a 65536 quando você criar seu site, e é diferente para cada site que você criar. (Quando você testar seu próprio site, o número da porta certamente serão um número diferente de 33561.) Usando uma porta diferente para cada site, o IIS Express podem manter retas que ele se comunica com sites.

Posteriormente, quando você publica seu site em um servidor web públicos, você não verá mais *localhost* na URL. Nesse ponto, você verá uma URL mais comum como `http://myhostingsite/mywebsite/HelloWorld.cshtml` ou qualquer página. Você aprenderá mais sobre como publicar um site mais tarde nesta série de tutoriais.

## <a name="adding-some-server-side-code"></a>Adicionar um código do lado do servidor

Feche o navegador e volte para a página no WebMatrix.

Adicione uma linha para o bloco de código para que ele se parece com o código a seguir:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Este é um pouco de código Razor. Está provavelmente claro que ele obtém a data e hora atuais e coloca esse valor em uma *variável* chamado `currentDateTime`. Você poderá ler mais sobre a sintaxe do Razor no tutorial Avançar.

No corpo da página, após o `<p>Hello World!</p>` elemento, adicione o seguinte:

[!code-html[Main](getting-started/samples/sample4.html)]

Esse código obtém o valor que você coloque o `currentDateTime` variável na parte superior e o insere na marcação da página. O `@` caractere marca o código de páginas da Web ASP.NET na página.

Execute novamente a página (o WebMatrix salva as alterações para você antes de executar a página). Neste momento você ver a data e hora em que a página.

![&quot;Olá, mundo&quot; página em execução no navegador com uma exibição de tempo geradas dinamicamente](getting-started/_static/image20.png)

Aguarde alguns minutos e, em seguida, atualize a página no navegador. A exibição de data e hora é atualizada.

No navegador, observe a origem da página. Ele se parece com a seguinte marcação:

[!code-html[Main](getting-started/samples/sample5.html)]

Observe que o `@{ }` bloco na parte superior não estiver lá. Observe também que a exibição de data e hora mostra uma cadeia de caracteres real (`1/18/2012 2:49:50 PM` ou qualquer), não `@currentDateTime` que você tinha no *. cshtml* página. O que aconteceu aqui é que, quando você executou a página, o ASP.NET processadas todo o código (muito pouco neste caso) que foi marcado com `@`. O código produz saída, e essa saída foi inserida na página.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Isso ocorre com as páginas da Web ASP.NET sobre

Ao ler páginas da Web ASP.NET produz conteúdo dinâmico da web, o que é visto aqui é a ideia. A página que você acabou de criar contém a mesma marcação HTML que você viu antes. Ele também pode conter código que pode executar todos os tipos de tarefas. Neste exemplo, que a tarefa simples de obter a data e hora atuais. Como você viu, você pode intercalar código com o HTML para produzir saída de página. Quando alguém solicita um *. cshtml* página no navegador, o ASP.NET processa a página enquanto ele ainda está em mãos o servidor web. ASP.NET insere a saída do código (se houver) na página como HTML. Quando o processamento de código é feito, o ASP.NET envia a página resultante para o navegador. Tudo o que o navegador obtém nunca é HTML. Aqui está um diagrama:

![Fluxo conceitual de como o ASP.NET gera HTML dinamicamente](getting-started/_static/image21.png)

A ideia é simple, mas há muitas tarefas interessantes que pode executar o código e há muitas maneiras interessantes dinamicamente no qual você pode adicionar conteúdo HTML para a página. E o ASP.NET *. cshtml* páginas, como uma página HTML, também podem incluir o código que é executado no navegador em si (código de JavaScript e jQuery). Você irá explorar todos esses itens neste conjunto de tutorial em subsequentes.

## <a name="coming-up-next"></a>Próximo

O seguinte tutorial nesta série, você explorar um pouco mais de programação páginas da Web ASP.NET.

## <a name="additional-resources"></a>Recursos adicionais

[Criar um site ASP.NET do zero](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Este é um tutorial que seja especificamente sobre usando o WebMatrix (não o ASP.NET Web Pages). Entra em um pouco mais detalhes sobre alguns dos recursos adicionais do WebMatrix não serão abordadas neste tutorial conjunto.

>[!div class="step-by-step"]
[Avançar](intro-to-web-pages-programming.md)
