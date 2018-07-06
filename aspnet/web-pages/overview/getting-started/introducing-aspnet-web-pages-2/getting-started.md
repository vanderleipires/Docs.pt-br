---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
title: Introdução ao ASP.NET Web Pages - Introdução | Microsoft Docs
author: tfitzmac
description: O WebMatrix não é mais recomendado como um ambiente de desenvolvimento integrado para páginas da Web do ASP.NET. Use o Visual Studio ou Visual Studio Code. Neste guia um...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: a36d3bdf-ef1b-47a4-b932-3a0cf4cad716
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/getting-started
msc.type: authoredcontent
ms.openlocfilehash: b4f554d2bf8bf564fd69239fcc7cc605158c83c3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825031"
---
<a name="introducing-aspnet-web-pages---getting-started"></a>Introdução a páginas da Web ASP.NET - Introdução
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > 
> > O WebMatrix não é mais recomendado como um ambiente de desenvolvimento integrado para páginas da Web do ASP.NET. Use [Visual Studio](../program-asp-net-web-pages-in-visual-studio.md) ou [Visual Studio Code](https://code.visualstudio.com/).
> 
> 
> Este guia e o aplicativo fornece uma visão geral das páginas da Web do ASP.NET (versão 2 ou posterior) e a sintaxe do Razor, uma estrutura leve para a criação de sites dinâmicos. Ele também apresenta o WebMatrix, uma ferramenta para a criação de páginas e sites.
> 
> **Nível**: nova para páginas da Web ASP.NET.  
> **Supõe-se de habilidades**: HTML, folhas de estilos em cascata (CSS) básica.
> 
> O que você vai aprender no primeiro tutorial do conjunto:
> 
> - É o que a tecnologia ASP.NET Web Pages e o que ele é.
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
> Mike Pope originalmente escreveram este tutorial. Tom FitzMacken atualizou para o Microsoft WebMatrix 3.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - Páginas da Web do ASP.NET (Razor) 2
> - O WebMatrix 3


## <a name="what-should-you-know"></a>O que você deveria saber?

Vamos supor que você esteja familiarizado com:

- **HTML**. Nenhum conhecimento aprofundado é necessário. Não explicaremos HTML, mas também não usamos algo complexo. Forneceremos links para tutoriais HTML em que acreditamos que eles são úteis.
- **Folhas de estilos em cascata (CSS)**. Mesmo que com HTML.
- **Ideias de banco de dados básico**. Se você tiver usado uma planilha de dados e classificados e filtrados de dados, que é o nível de conhecimento geralmente supondo para este conjunto de tutoriais.

Vamos supor também que você está interessado em aprender programação básica. Páginas da Web ASP.NET usam uma linguagem de programação c#. Você não precisa ter qualquer experiência em programação, apenas um interesse nele. Se você já escreveu nenhum JavaScript em uma página da web antes, você tem muita em segundo plano.

Observe que, se você estiver familiarizado com programação, você pode achar que este tutorial é definida inicialmente se move lentamente enquanto oferecemos os novos programadores a agilizar. Como podemos superar alguns tutoriais primeiro, no entanto, haverá menos básicas de programação para explicar e as coisas se moverá ao longo em um clipe mais rápido.

## <a name="what-do-you-need"></a>O que você precisa?

A seguir, o que será necessário:

- Um computador que está executando o Windows 8, Windows 7, Windows Server 2008 ou Windows Server 2012.
- Uma conexão de internet em tempo real.
- Privilégios de administrador (necessários para o processo de instalação).

## <a name="what-is-aspnet-web-pages"></a>O que é ASP.NET Web Pages?

Páginas da Web ASP.NET é uma estrutura que você pode usar para criar páginas da web dinâmicas. Uma página de web HTML simples é estático; seu conteúdo é determinado pela marcação HTML fixa que está na página. Páginas dinâmicas, como aqueles criados com páginas da Web do ASP.NET permitem que você crie o conteúdo da página em tempo real, usando código.

Páginas dinâmicas permitem fazer todos os tipos de coisas. Você pode solicitar um usuário usando um formulário de entrada e, em seguida, altere a página exibe ou a aparência dele. Pode levar informações de um usuário, salvá-la em um banco de dados e, em seguida, listá-lo mais tarde. Você pode enviar o email do seu site. Você pode interagir com outros serviços na web (por exemplo, um serviço de mapeamento) e produzir páginas que se integram informações dessas fontes.

## <a name="what-is-webmatrix"></a>O que é o WebMatrix?

O WebMatrix é uma ferramenta que integra um editor de página da web, um utilitário de banco de dados, um servidor web para testar páginas e recursos para a publicação de seu site com a Internet. O WebMatrix é gratuito e é fácil de instalar e fácil de usar. (Isso também funciona para simplesmente páginas HTML, bem como para outras tecnologias como PHP.)

Você não realmente *ter* usar WebMatrix para trabalhar com páginas da Web do ASP.NET. Você pode criar páginas usando um texto de editor, por exemplo e testar páginas usando um servidor web que você tem acesso ao. No entanto, o WebMatrix facilita tudo muito, para que esses tutoriais usará o WebMatrix em todo.

## <a name="about-these-tutorials"></a>Sobre esses tutoriais

Este conjunto de tutoriais é uma introdução a como usar páginas da Web ASP.NET. Há 9 tutoriais total neste conjunto de tutoriais introdutórios. É um parte de uma série de tutoriais conjuntos que leva você de iniciante de páginas da Web ASP.NET para criar sites reais, com aparência profissional.

Este primeiro tutorial definir concentra-se no que mostra as Noções básicas de como trabalhar com páginas da Web do ASP.NET. Quando você terminar, você pode trabalhar com conjuntos de tutoriais adicionais que selecionar em que este termina e que explore as páginas da Web mais detalhadamente.

Deliberadamente prosseguimos fácil as explicações detalhadas. E sempre que vamos mostrar algo, para este conjunto de tutoriais sempre escolhemos a maneira que achamos que é mais fácil de entender. Conjuntos de tutorial posteriores com mais detalhes e mostram a você as abordagens mais eficientes ou mais flexíveis (também mais divertido). Mas esses tutoriais exigem que você conhece os fundamentos básicos pela primeira vez.

O conjunto de tutoriais recém-iniciada aborda esses recursos de páginas da Web do ASP.NET:

- Introdução e obter tudo instalado. (Que é o tutorial, que você está lendo).
- Os conceitos básicos da programação de páginas da Web ASP.NET.
- Criando um banco de dados.
- Criando e processando um formulário de entrada do usuário.
- Adicionar, atualizar e excluir dados no banco de dados.

## <a name="what-will-you-create"></a>O que você irá criar?

Definir este tutorial e os demais giram em torno de um site onde você pode listar os filmes que você deseja. Você poderá inserir filmes, editá-los e listá-los. Aqui estão algumas das páginas que você criará neste tutorial conjunto. A primeira mostra o filme a listagem de página que você criará:

![Aplicativo de filme termina mostrando uma listagem de filmes](getting-started/_static/image1.png)

E aqui está a página que permite que você adicione novas informações do filme ao seu site:

![Aplicativo de filme concluído mostrando a página Adicionar filme](getting-started/_static/image2.png)

Compilação subsequente conjuntos de tutorial sobre isso definir e adicionar mais funcionalidade, como carregamento de imagens, permitindo que as pessoas a fazer logon no, envio de email e a integração com a mídia social.

## <a name="see-this-app-running-on-azure"></a>Consulte esse aplicativo em execução no Azure

Você gostaria de ver o site concluído em execução como um aplicativo web ao vivo? Você pode implantar uma versão completa do aplicativo para sua conta do Azure, simplesmente clicando no botão a seguir.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/WebPagesMovies)

Você precisa de uma conta do Azure para implantar essa solução no Azure. Se você ainda não tiver uma conta, você tem as seguintes opções:

- [Abrir uma conta do Azure gratuitamente](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) -obtenha créditos você pode usar para experimentar serviços pagos do Azure e até mesmo após eles serem utilizados, você pode manter a conta e utilizar os serviços do Azure gratuitos.
- [Ativar benefícios de assinante do MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -sua assinatura do MSDN concede créditos todos os meses que você pode usar para serviços pagos do Azure.

## <a name="installing-everything"></a>Instalação de tudo

Você pode instalar tudo usando o Web Platform Installer da Microsoft. Na verdade, instalar o instalador e, em seguida, usá-lo para instalar todo o resto.

Para usar as páginas da Web, você precisa ter pelo menos Windows XP com SP3 instalado, ou o Windows Server 2008 ou posterior.

Sobre o [página de páginas da Web](../../../index.md) do site do ASP.NET, clique em **instalar**.

![Mostrando do site da Web do ASP.NET &quot;instalar o WebMatrix&quot; botão](getting-started/_static/image3.png)

Você será solicitado a aceitar os termos de licença e declaração de privacidade antes de instalar o WebMatrix.

![aceitar o termo para iniciar a instalação](getting-started/_static/image4.png)

Clique em **executar** para iniciar a instalação. (Se você deseja salvar o instalador, clique em **salvar** e, em seguida, execute o instalador da pasta onde ele foi salvo.)

![](getting-started/_static/image5.png)

O Web Platform Installer é exibido, pronto para instalar o WebMatrix. Clique em **Instalar**.

![](getting-started/_static/image6.png)

O processo de instalação descobre o que ele deve instalar no seu computador e inicia o processo. Dependendo exatamente o que deve ser instalado, o processo pode levar de alguns instantes para vários minutos. Selecione **aceito** para aceitar os termos de licença.

## <a name="hello-webmatrix"></a>Hello, WebMatrix

Quando estiver pronto, o processo de instalação pode iniciar automaticamente o WebMatrix. Caso contrário, no Windows, do **inicie** menu, inicie **o Microsoft WebMatrix**.

Quando você inicia o WebMatrix pela primeira vez, você terá a oportunidade de entrar no Microsoft Azure com sua conta da Microsoft. Ao entrar, você receberá 10 aplicativos web gratuitos por meio do Azure. Esses aplicativos web gratuitos fornecem uma maneira conveniente para testar seus aplicativos. Se você ainda não tiver uma conta do Azure, mas você tem uma assinatura do MSDN, você pode [ativar os benefícios de assinatura do MSDN](https://www.windowsazure.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604). Caso contrário, você pode criar uma conta de avaliação gratuita em apenas alguns minutos. Para obter detalhes, consulte [avaliação gratuita do Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

Você não precisa entrar no momento para continuar com este tutorial. Se você não fizer login no agora, você ainda terá a opção entrar mais tarde. A última [tópico](publishing.md) neste tutorial série aborda como implantar seu site para o Azure; portanto, você precisará entrar para concluir esse tópico.

Neste ponto, qualquer um entre com sua conta da Microsoft ou o select **agora não** no canto inferior direito.

![Entrar](getting-started/_static/image7.png)

Para começar, você vai criar um site em branco e adicione uma página. Um tutorial posterior neste conjunto, você experimentar um dos modelos de site interna.

Na janela de início, clique em **New**.

![Tela de inicialização do WebMatrix](getting-started/_static/image8.png)

Modelos são arquivos pré-criados e páginas para diferentes tipos de sites. Para ver todos os modelos que estão disponíveis por padrão, selecione a opção da Galeria de modelos.

![Galeria de modelos de Select](getting-started/_static/image9.png)

No **início rápido** janela, selecione **Site vazio** do **ASP.NET** de grupo e nomeie o novo site de "WebPagesMovies".

![Janela de início rápido do WebMatrix com o modelo Site vazio selecionado](getting-started/_static/image10.png)

Clique em **Avançar**.

Se você entrar sua conta da Microsoft, você terá a oportunidade de criar o site no Azure. Com base no nome do seu site, o nome padrão do **WebPagesMovies.azurewebsites.net** é sugerida; no entanto, o ponto de exclamação indica que esse nome não está disponível no Windows Azure. Para manter a simplicidade, selecione **Skip** para ignorar a criação de site da web no Azure no momento. Mais tarde nessa série, publicaremos o site para o Azure.

![Criar site do azure](getting-started/_static/image11.png)

O WebMatrix cria e abre o site:

![Abra o novo site WebPagesMovies no WebMatrix](getting-started/_static/image12.png)

Na parte superior, há uma barra de ferramentas de acesso rápido e uma faixa de opções. Na parte inferior esquerda, você ver o seletor de espaço de trabalho em que você alterne entre as tarefas (**Site**, **arquivos**, **bancos de dados**, **relatórios**). À direita está o painel de conteúdo para o editor e relatórios. E, na parte inferior, ocasionalmente, verá uma barra de notificação para mensagens.

Você aprenderá mais sobre o WebMatrix e seus recursos ao percorrer esses tutoriais.

## <a name="creating-a-web-page"></a>Criando uma página da Web

Para se familiarizar com o WebMatrix e páginas da Web ASP.NET, você criará uma página simples.

No seletor de espaço de trabalho, selecione o **arquivos** espaço de trabalho. Este espaço de trabalho permite que você trabalhe com arquivos e pastas. O painel esquerdo mostra a estrutura de arquivos do seu site. As alterações de faixa de opções para mostrar as tarefas relacionadas ao arquivo.

![Espaço de trabalho de arquivo no WebMatrix](getting-started/_static/image13.png)

Na faixa de opções, clique na seta sob **New** e, em seguida, clique em **novo arquivo**.

![Usando o &quot;New&quot; comando na faixa de opções para criar um novo arquivo](getting-started/_static/image14.png)

O WebMatrix exibe uma lista dos tipos de arquivo. Selecione **CSHTML**e, nas **nome** , digite "HelloWorld". Uma página CSHTML é uma página da Web do ASP.NET.

![Criando uma nova página CSHTML chamado HelloWorld.cshtml](getting-started/_static/image15.png)

Clique em **OK**.

O WebMatrix cria a página e o abre no editor.

![A nova página HelloWorld no editor do WebMatrix](getting-started/_static/image16.png)

Como você pode ver, a página contém principalmente marcação HTML comum, exceto para um bloco na parte superior que tem esta aparência:

[!code-cshtml[Main](getting-started/samples/sample1.cshtml)]

Isso para adicionar o código, como você verá em breve.

Observe que as diferentes partes da página &mdash; os nomes dos elementos, atributos e texto, além do bloco na parte superior — estão todos em cores diferentes. Isso é chamado *realce de sintaxe*, e ela torna mais fácil manter tudo clara. É um dos recursos que torna mais fácil trabalhar com páginas da web no WebMatrix.

Adicionar conteúdo para o `<head>` e `<body>` elementos como no exemplo a seguir. (Se você quiser, você pode simplesmente copie o seguinte bloco e substitua toda a página existente por este código.)

[!code-cshtml[Main](getting-started/samples/sample2.cshtml)]

Na barra de ferramentas de acesso rápido ou nos **arquivo** menu, clique em **salvar**.

![Botão Salvar no WebMatrix ferramentas de acesso rápido](getting-started/_static/image17.png)

## <a name="testing-the-page"></a>Testando a página

No **arquivos** espaço de trabalho, com o botão direito do *HelloWorld.cshtml* da página e, em seguida, clique em **iniciar no navegador**.

![Executando uma página usando o botão Executar na faixa de opções do WebMatrix](getting-started/_static/image18.png)

O WebMatrix inicia um servidor web interno (IIS Express) que você pode usar para testar páginas em seu computador. (Sem o IIS Express no WebMatrix, você precisa publicar sua página em um servidor web em algum lugar antes de você pode testá-lo.) A página é exibida no navegador padrão.

![&quot;Olá, mundo&quot; página em execução no navegador](getting-started/_static/image19.png)

Observe que quando você testa uma página no WebMatrix, a URL no navegador é algo como `http://localhost:33651/HelloWorld.cshtml.` o nome *localhost* refere-se a um servidor local, o que significa que a página é atendida por um servidor web que está em seu próprio computador. Conforme observado, o WebMatrix inclui um programa de servidor da web chamado IIS Express que é executado quando você iniciar uma página.

O número após *localhost* (por exemplo, *localhost:33651*) se refere a um *número da porta* em seu computador. Este é o número de "canal" que usa o IIS Express para esse site específico. O número da porta é selecionado aleatoriamente do intervalo de 1024 a 65536 quando você criar seu site, e é diferente para cada site que você cria. (Quando você testar seu próprio site, o número da porta quase que certamente será um número diferente de 33561.) Ao usar uma porta diferente para cada site, o IIS Express pode manter retas quais dos seus sites que ele está se comunicando com.

Posteriormente, quando você publica seu site para um servidor web pública, você não verá mais *localhost* na URL. Nesse ponto, você verá uma URL mais típica como `http://myhostingsite/mywebsite/HelloWorld.cshtml` ou qualquer que seja a página está. Você aprenderá mais sobre como publicar um site mais tarde nessa série de tutoriais.

## <a name="adding-some-server-side-code"></a>Adicionar algum código do lado do servidor

Feche o navegador e vá até a página no WebMatrix.

Adicione uma linha para o bloco de código para que ele se parece com o código a seguir:

[!code-cshtml[Main](getting-started/samples/sample3.cshtml)]

Isso é um pouco de código do Razor. É provavelmente claro que ele obtém a data e hora atuais e coloca esse valor em uma *variável* denominado `currentDateTime`. Você vai ler mais sobre a sintaxe do Razor no próximo tutorial.

No corpo da página, após o `<p>Hello World!</p>` elemento, adicione o seguinte:

[!code-html[Main](getting-started/samples/sample4.html)]

Esse código obtém o valor que você colocar nas `currentDateTime` variáveis na parte superior e o insere na marcação da página. O `@` caractere marca o código de páginas da Web ASP.NET na página.

Execute novamente a página (o WebMatrix salva as alterações para você antes de executar a página). Neste momento você ver a data e hora em que a página.

![&quot;Olá, mundo&quot; página em execução no navegador com uma exibição de tempo gerado dinamicamente](getting-started/_static/image20.png)

Aguarde alguns minutos e, em seguida, atualize a página no navegador. A exibição de data e hora é atualizada.

No navegador, examine a origem da página. Ele se parece com a seguinte marcação:

[!code-html[Main](getting-started/samples/sample5.html)]

Observe que o `@{ }` bloco na parte superior não estiver lá. Observe também que a exibição de data e hora mostra uma cadeia de caracteres real de caracteres (`1/18/2012 2:49:50 PM` ou por qualquer), e não `@currentDateTime` que você tinha na *. cshtml* página. O que aconteceu aqui é que, quando você executou a página, o ASP.NET processadas todo o código (muito pouco neste caso) que foi marcado com `@`. O código produz a saída, e essa saída foi inserida na página.

## <a name="this-is-what-aspnet-web-pages-are-about"></a>Isso ocorre com as páginas da Web do ASP.NET sobre

Quando você lê que páginas da Web ASP.NET produz o conteúdo dinâmico da web, o que você viu aqui é a ideia. A página que você acabou de criar contém a mesma marcação HTML que você viu antes. Ele também pode conter código que pode executar todos os tipos de tarefas. Neste exemplo, ele fez a tarefa trivial de obter a data e hora atuais. Como você viu, você pode intecalar código com o HTML para produzir a saída na página. Quando alguém solicita um *. cshtml* página no navegador, o ASP.NET processa a página enquanto ele ainda está em mãos do servidor web. ASP.NET insere a saída do código (se houver) na página como HTML. Quando o processamento de código é concluído, o ASP.NET envia a página resultante para o navegador. Tudo o que o navegador obtém nunca é HTML. Aqui está um diagrama:

![Fluxo conceitual de como o ASP.NET gera HTML dinamicamente](getting-started/_static/image21.png)

A ideia é simples, mas há muitas tarefas interessantes que o código pode executar, e há muitas maneiras interessantes nos quais você pode adicionar dinamicamente conteúdo HTML para a página. E o ASP.NET *. cshtml* páginas, como em qualquer página HTML, também podem incluir código que é executado no navegador em si (código de JavaScript e jQuery). Você explorará todas essas coisas neste conjunto de tutoriais em subsequentes.

## <a name="coming-up-next"></a>Próximo

No próximo tutorial desta série, você explorar um pouco mais de programação páginas da Web ASP.NET.

## <a name="additional-resources"></a>Recursos adicionais

[Criar um site ASP.NET do zero](https://www.microsoft.com/web/post/create-an-aspnet-website-from-scratch). Este é um tutorial que é especificamente sobre como usar o WebMatrix (não o ASP.NET Web Pages). Ele entra em um pouco mais detalhes sobre alguns dos recursos adicionais do WebMatrix que não abordaremos neste conjunto de tutoriais.

> [!div class="step-by-step"]
> [Avançar](intro-to-web-pages-programming.md)
