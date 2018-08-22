---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Noções básicas sobre os recursos de depuração do ASP.NET AJAX | Microsoft Docs
author: scottcate
description: A capacidade de depurar o código é uma habilidade que todo desenvolvedor deve ter em seu arsenal independentemente da tecnologia que está sendo usada. Embora muitos desenvolvedores estão...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 13fd8f05cd994ff1c902bd067fb4ed425010d64e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824388"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Noções básicas sobre depuração de recursos do AJAX ASP.NET
====================
por [Scott Cate](https://github.com/scottcate)

[Baixar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> A capacidade de depurar o código é uma habilidade que todo desenvolvedor deve ter em seu arsenal independentemente da tecnologia que está sendo usada. Embora muitos desenvolvedores estão acostumados a usar o Visual Studio .NET ou o Web Developer Express para depurar aplicativos ASP.NET que usam código VB.NET ou c#, alguns não estão cientes de que também é extremamente útil para depurar o código do lado do cliente, como JavaScript. Também pode ser aplicado o mesmo tipo de técnicas usadas para depurar aplicativos .NET para aplicativos habilitados para AJAX e mais especificamente os aplicativos do ASP.NET AJAX.


## <a name="debugging-aspnet-ajax-applications"></a>Depuração de aplicativos do ASP.NET AJAX

Dan Wahlin

A capacidade de depurar o código é uma habilidade que todo desenvolvedor deve ter em seu arsenal independentemente da tecnologia que está sendo usada. É preciso dizer que Noções básicas sobre as diferentes opções de depuração que estão disponíveis pode salvar uma enorme quantidade de tempo em um projeto e talvez até mesmo algumas dores de cabeça. Embora muitos desenvolvedores estão acostumados a usar o Visual Studio .NET ou o Web Developer Express para depurar aplicativos ASP.NET que usam código VB.NET ou c#, alguns não estão cientes de que também é extremamente útil para depurar o código do lado do cliente, como JavaScript. Também pode ser aplicado o mesmo tipo de técnicas usadas para depurar aplicativos .NET para aplicativos habilitados para AJAX e mais especificamente os aplicativos do ASP.NET AJAX.

Neste artigo, você verá como o Visual Studio 2008 e várias outras ferramentas podem ser usadas para depurar aplicativos ASP.NET AJAX para localizar rapidamente os bugs e outros problemas. Esta discussão inclui informações sobre como habilitar o Internet Explorer 6 ou superior para depuração, usando o Visual Studio 2008 e o Gerenciador de Script para percorrer o código, bem como usar outras ferramentas gratuitas como o Web Development Helper. Você também aprenderá como depurar aplicativos ASP.NET AJAX no Firefox usando que uma extensão chamada Firebug que permite que você percorre o código de JavaScript diretamente no navegador sem qualquer outra ferramenta. Por fim, você será apresentado a classes na biblioteca do AJAX ASP.NET que podem ajudar com várias tarefas de depuração, como o rastreamento e as instruções de declaração de código.

Antes de tentar depurar páginas exibidas no Internet Explorer, há algumas etapas básicas que você precisará executar para habilitá-lo para depuração. Vamos dar uma olhada em alguns requisitos de configuração básica que precisam ser executadas para começar a usar.

## <a name="configuring-internet-explorer-for-debugging"></a>Configurando o Internet Explorer para depuração

A maioria das pessoas não esteja interessado em ver problemas de JavaScript encontrados em um site exibido com o Internet Explorer. Na verdade, o usuário médio ainda não saberia o que fazer se eles viam uma mensagem de erro. Como resultado, as opções de depuração estão desativadas por padrão no navegador. No entanto, é muito simples ativar a depuração e colocá-lo para usar ao desenvolver novos aplicativos AJAX.

Para habilitar a funcionalidade de depuração, vá para Ferramentas Opções da Internet no menu do Internet Explorer e selecione a guia Avançado. Dentro da seção de navegação garantir que os itens a seguir unchecked:

- Desabilitar depuração de scripts (Internet Explorer)
- Desabilitar depuração de scripts (outros)

Embora não obrigatório, se você está tentando depurar um aplicativo que você provavelmente vai querer quaisquer erros de JavaScript na página seja imediatamente visível e óbvia. Você pode forçar todos os erros a serem mostradas com uma caixa de mensagem, marcando a caixa de seleção "Exibir notificação sobre cada erro de script". Embora essa seja uma ótima opção para ativar enquanto você estiver desenvolvendo um aplicativo, ele pode se tornar rapidamente irritante, se você estiver apenas folheia outros sites, pois suas chances de encontrar erros de JavaScript são muito boas.

Figura 1 mostra que o Internet Explorer avançada da caixa de diálogo deve parecer depois que ele foi configurado corretamente para depuração.


[![Configurando o Internet Explorer para depuração.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Figura 1**: Configurando o Internet Explorer para depuração.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Depois que a depuração foi ativada, você verá um novo item de menu aparecem no menu de exibição denominado depurador de Script. Ele tem duas opções disponíveis, incluindo abrir e quebra na próxima instrução. Quando abertos for marcado você será solicitado a depurar a página no Visual Studio 2008 (Observe que o Visual Web Developer Express também pode ser usado para depuração). Se estiver executando o Visual Studio .NET, você pode escolher para usar essa instância ou para criar uma nova instância. Quando a interrupção na próxima instrução é selecionada você será solicitado a depurar a página quando o código JavaScript é executado. Se o código JavaScript é executado no evento onLoad da página, você pode atualizar a página para disparar uma sessão de depuração. Se o código JavaScript é executado depois que um botão é clicado, em seguida, o depurador será executado imediatamente depois que o botão é clicado.

> [!NOTE]
> Se você estiver executando no Windows Vista com o usuário acesso UAC (controle) habilitado e tiver o Visual Studio 2008 definido para ser executado como um administrador, o Visual Studio falhará anexar ao processo quando você for solicitado para anexar. Para contornar esse problema, inicie o Visual Studio pela primeira vez e usar essa instância para depurar.

Embora a próxima seção demonstrará como depurar uma página ASP.NET AJAX diretamente de dentro do Visual Studio 2008, usando a opção de depurador de Script do Internet Explorer é útil quando uma página já está aberta e você gostaria de mais completamente inspecioná-la.

## <a name="debugging-with-visual-studio-2008"></a>Depuração com o Visual Studio 2008

O Visual Studio 2008 fornece a funcionalidade de depuração que os desenvolvedores em todo o mundo dependem de todos os dias para depurar aplicativos do .NET. O depurador interno permite que você percorrer o código, a exibição de dados de objeto, inspeção de variáveis específicas, monitoram a pilha de chamadas e muito mais. Além da depuração de código VB.NET ou c#, o depurador também é útil para depuração de aplicativos do ASP.NET AJAX e permitirá que você percorra o código JavaScript linha por linha. Os detalhes que seguem o foco em técnicas que podem ser usadas para depurar arquivos de script do lado do cliente em vez de fornecer um discourse sobre o processo geral de depuração de aplicativos usando o Visual Studio 2008.

O processo de depuração de uma página no Visual Studio 2008 pode ser iniciado de diversas maneiras. Primeiro, você pode usar a opção de depurador de Script do Internet Explorer mencionada na seção anterior. Isso funciona bem quando uma página já está carregada no navegador e você quiser começar a depurá-lo. Como alternativa, você pode clique duas vezes em uma página. aspx no Gerenciador de soluções e selecione Definir como página inicial, no menu. Se você estiver acostumado a depuração de páginas ASP.NET, em seguida, você provavelmente fizemos isso antes. Quando F5 é pressionado a página pode ser depurada. No entanto, embora geralmente você pode definir um ponto de interrupção em qualquer lugar você gostaria no código VB.NET ou c#, que nem sempre é o caso com o JavaScript como você verá em seguida.

*Embedded Versus Scripts externos*

O depurador do Visual Studio 2008 trata JavaScript inserido em uma página diferente de arquivos JavaScript externos. Com os arquivos de script externo, você pode abrir o arquivo e definir um ponto de interrupção em qualquer linha que você escolher. Pontos de interrupção podem ser definidos clicando na área de bandeja cinza à esquerda da janela do editor de código. Quando o JavaScript é incorporado diretamente em uma página usando o `<script>` marca, definindo um ponto de interrupção clicando na área cinza bandeja não é uma opção. Tenta definir um ponto de interrupção em uma linha de script incorporado resultará em um aviso informando que "Não é um local válido para um ponto de interrupção".

Você pode contornar esse problema, movendo o código em um arquivo. js externo e fazer referência a ele usando o atributo src do &lt;script&gt; marca:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

E se movendo o código em um arquivo externo não é uma opção ou requer mais trabalho do que vale a pena? Embora você não pode definir um ponto de interrupção usando o editor, você pode adicionar a instrução de depurador diretamente no código onde você deseja iniciar a depuração. Você também pode usar a classe Sys. Debug disponível na biblioteca do AJAX ASP.NET para forçar a depuração para iniciar. Você aprenderá mais sobre a classe Sys. Debug neste artigo.

Um exemplo de como usar o `debugger` palavra-chave é mostrada na listagem 1. Este exemplo força o depurador para interromper correto antes de uma chamada para uma função de atualização é feita.

**Listagem 1. Usando a palavra-chave do depurador para forçar o depurador do Visual Studio .NET para interromper.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Depois que a instrução de depurador é atingida, você será solicitado a depurar a página usando o Visual Studio .NET e pode começar a percorrer o código. Ao fazer isso, você pode encontrar um problema com o acesso a arquivos de script do ASP.NET AJAX library usados na página de então, vamos dar uma olhada usando o Visual Studio. Gerenciador de Script da rede.

## <a name="using-visual-studio-net-windows-to-debug"></a>Usando o Visual Studio .NET Windows para depurar

Depois de uma sessão de depuração é iniciada e começar a percorrer o código usando a tecla F11 padrão, você pode encontrar a caixa de diálogo de erro mostrada na veja a Figura 2, a menos que todos os arquivos de script usados na página de estejam abertas e disponíveis para depuração.


[![Caixa de diálogo de erro mostrada quando nenhum código-fonte está disponível para depuração.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Figura 2**: caixa de diálogo de erro mostrada quando nenhum código-fonte está disponível para depuração.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Essa caixa de diálogo é mostrada porque o Visual Studio .NET não estiver certo sobre como obter o código-fonte de alguns dos scripts referenciados pela página. Embora isso possa ser muito frustrante primeiro, há uma correção simple. Depois de iniciada uma sessão de depuração e um ponto de interrupção, vá para a janela do Gerenciador de Script do Windows de depuração no menu do Visual Studio 2008 ou use a tecla de atalho Ctrl + Alt + N.

> [!NOTE]
> Se você não puder ver o menu do Gerenciador de Script listado, vá para **ferramentas** > **personalizar** > **comandos** no menu do Visual Studio .NET. Localize o **depurar** entrada nas categorias de seção e clique nele para mostrar todas as entradas de menu disponíveis. Na lista de comandos, role para baixo até o Gerenciador de Script e, em seguida, arraste-o para o Windows depurar menu no mencionado anteriormente. Isso disponibilizará a entrada de menu do Gerenciador de Script sempre que você executar o Visual Studio .NET.

O Gerenciador de Script pode ser usado para exibir todos os scripts usados em uma página e abri-los no editor de códigos. Quando o Gerenciador de Script é aberto, clique duas vezes na página. aspx que estão sendo depurada no momento para abri-lo na janela do editor de código. Execute a mesma ação para todos os outros scripts mostrados no Gerenciador de Script. Depois que todos os scripts estão abertos na janela de código, você pode pressione F11 (e use as outras teclas de atalho de depuração) para percorrer seu código. Figura 3 mostra um exemplo de como o Gerenciador de Script. Ela lista o arquivo atual que está sendo depurado (Demo.aspx), bem como dois scripts personalizados e dois scripts dinamicamente injetados na página pelo ScriptManager ASP.NET AJAX.


[![O Gerenciador de Script fornece acesso fácil a scripts usados em uma página.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Figura 3**. O Gerenciador de Script fornece acesso fácil a scripts usados em uma página.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Várias outras windows também podem ser usados para fornecer informações úteis conforme você percorre o código em uma página. Por exemplo, você pode usar a janela locais para ver os valores de variáveis diferentes usados na página, a janela imediata para avaliar variáveis específicas ou condições e exibir a saída. Você também pode usar a janela de saída para exibir instruções de rastreamento gravadas usando a função Sys.Debug.trace (que será abordada neste artigo) ou uma função de Debug. writeln do Internet Explorer.

Conforme você percorre o código usando o depurador pode passar o mouse sobre as variáveis no código para exibir o valor que são atribuídas. No entanto, o depurador de scripts, ocasionalmente, não mostre nada conforme você passa o mouse sobre uma determinada variável de JavaScript. Para ver o valor, realce a instrução ou uma variável que você está tentando ver na janela do editor de código e, em seguida, passe o mouse sobre ele. Embora essa técnica não funciona em todas as situações, muitas vezes você poderá ver o valor sem ter que procurar em uma janela de depuração diferentes, como a janela locais.

Um tutorial em vídeo que demonstra alguns dos recursos discutidos aqui pode ser exibido no [ http://www.xmlforasp.net ](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Depuração com o Web Development Helper

Embora o Visual Studio 2008 (e o Visual Web Developer Express 2008) são muito capazes de ferramentas de depuração, há opções adicionais que podem ser usadas também que são mais leves. Uma das ferramentas mais recentes sejam liberados é o Web Development Helper. Nikhil Kothari da Microsoft (um dos principais arquitetos do ASP.NET AJAX na Microsoft) escreveu essa ferramenta excelente que pode executar muitas tarefas diferentes da depuração simples para exibir mensagens de solicitação e resposta HTTP. Web Development Helper pode ser baixado em [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx ](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Auxiliar de desenvolvimento da Web pode ser usado diretamente dentro do Internet Explorer, que é conveniente usar. Ele é iniciado, selecionando Ferramentas Web Development Helper de menu do Internet Explorer. Isso abrirá a ferramenta na parte inferior do navegador que é bom, pois você não precisa deixar o navegador para executar várias tarefas, como o log de mensagem de solicitação e resposta HTTP. Figura 4 mostra a aparência de Web Development Helper em ação.


[![Web Development Helper](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Figura 4**: Web Development Helper ([clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Auxiliar de desenvolvimento da Web não é uma ferramenta que você usará para percorrer o código linha por linha enquanto o Visual Studio 2008. No entanto, ele pode ser usado para exibir a saída de rastreamento, facilmente avaliar as variáveis em um script ou explorar os dados são dentro de um objeto JSON. Também é muito útil para exibir os dados que são passados de e para uma página ASP.NET AJAX e um servidor.

Depois que o Web Development Helper é aberto no Internet Explorer, a depuração de script deve ser habilitada pelo selecionando a depuração de Script habilitar o Script a partir do menu de auxiliar de desenvolvimento da Web, conforme mostrado anteriormente na Figura 4. Isso permite que a ferramenta para interceptar erros que ocorrem quando uma página é executada. Ele também permite o fácil acesso às mensagens de rastreamento que são produzidas na página. Para exibir informações de rastreamento ou executar comandos de script para testar as diferentes funções dentro de uma página, selecione o Script Mostrar Script de Console, no menu Web Development Helper. Isso fornece acesso a uma janela de comando e uma janela imediata simples.

*Exibindo mensagens de rastreamento e dados de objeto JSON*

A janela imediata pode ser usada para executar comandos de script ou até mesmo carregar ou salvar os scripts que são usados para testar funções diferentes em uma página. A janela de comando exibe as mensagens de rastreamento ou depuração gravadas pela página que está sendo visualizada. Listagem 2 mostra como gravar uma mensagem de rastreamento usando a função de Debug. writeln do Internet Explorer.

**Listagem 2. Gravar uma mensagem de rastreamento do lado do cliente usando a classe de depuração.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Se a propriedade LastName contiver um valor de Doe, o Web Development Helper exibirá a mensagem "nome de pessoa: Doe" na janela de comando do console de script (supondo que a depuração foi habilitada). Web Development Helper também adiciona um objeto de nível superior debugService em páginas que podem ser usadas para gravar informações de rastreamento ou exibir o conteúdo dos objetos JSON. Listagem 3 mostra um exemplo de como usar a função de rastreamento da classe debugService.

**Listagem 3. Usando a classe de debugService da Web Development Helper para gravar uma mensagem de rastreamento.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Um recurso interessante da classe debugService é que ele funcionará mesmo se a depuração não está habilitada no Internet Explorer, tornando mais fácil para sempre acessar dados de rastreamento quando Web Development Helper está em execução. Quando a ferramenta não está sendo usada para depurar uma página, instruções de rastreamento serão ignoradas, desde que a chamada para window.debugService retornará false.

A classe debugService também permite que os dados de objeto JSON ser exibido usando a janela do Inspetor da Web Development Helper. Listagem 4 cria um objeto JSON simples que contém dados de pessoa. Depois que o objeto é criado, é feita uma chamada para o debugService classe inspecionar a função para permitir que o objeto JSON ser inspecionados visualmente.

**Listagem 4. Usando a função debugService.inspect para exibir dados de objeto JSON.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Chamar a função de GetPerson() na página ou por meio da janela imediata resultará na janela de diálogo do Inspetor de objetos que aparecem conforme mostrado na Figura 5. Propriedades em um objeto podem ser alteradas dinamicamente, realçando-os, alterando o valor exibido na caixa de texto valor e, em seguida, clicando no link de atualização. Usando o Inspetor de objeto simplifica exibir dados de objeto JSON e fazer experiências com aplicando diferentes valores a propriedades.

*Erros de depuração*

Além de permitir que os dados de rastreamento e objetos do JSON a ser exibido, o Web Development helper também pode ajudar na depuração de erros em uma página. Se um erro for encontrado, você será solicitado a continuar para a próxima linha de código ou depurar o script (consulte a Figura 6). A janela de caixa de diálogo de erro de Script mostra a chamada completa de pilha, bem como os números de linha para que você pode facilmente identificar onde os problemas estão dentro de um script.


[![Usando a janela Inspetor de objeto para exibir um objeto JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Figura 5**: usando a janela Inspetor de objeto para exibir um objeto JSON.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Selecionando a opção de depuração permite que você execute instruções de script diretamente na janela de imediato do Web Development Helper para exibir o valor de variáveis, gravar objetos JSON, e muito mais. Se a mesma ação que disparou o erro é executada novamente e o Visual Studio 2008 está disponível no computador, você será solicitado para iniciar uma sessão de depuração para que você pode percorrer o código linha por linha conforme discutido na seção anterior.


[![Caixa de diálogo de erro de Script do auxiliar de desenvolvimento de Web](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Figura 6**: caixa de diálogo de erro de Script da Web Development Helper ([clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Inspecionar mensagens de resposta e solicitação*

Durante a depuração de páginas ASP.NET AJAX geralmente é útil ver mensagens de solicitação e resposta enviadas entre um servidor e de página. Exibir o conteúdo dentro de mensagens permite que você veja se os dados apropriados está sendo passados, bem como o tamanho das mensagens. Web Development Helper fornece um excelente recurso de agente de mensagem HTTP que torna mais fácil de exibir dados como texto não processado ou em um formato mais legível.

Para exibir mensagens de solicitação e resposta do ASP.NET AJAX, o agente de log HTTP deve ser habilitado selecionando o log do HTTP habilitar HTTP no menu Web Development Helper. Uma vez habilitada, todas as mensagens enviadas da página atual podem ser exibidas no Visualizador de log HTTP, que pode ser acessado selecionando HTTP Mostrar Logs de HTTP.

Embora a exibir o texto não processado enviado em cada mensagem de solicitação/resposta é certamente útil (e uma opção no Web Development Helper), geralmente é mais fácil de exibir dados de mensagem em um formato mais gráfico. Depois que o log HTTP foi habilitado e foram registradas mensagens, os dados de mensagem podem ser exibidos clicando duas vezes na mensagem no Visualizador de log HTTP. Isso permite que você exiba todos os cabeçalhos associados com uma mensagem, bem como a mensagem real conteúdo. Figura 7 mostra um exemplo de uma mensagem de solicitação e a mensagem de resposta exibidos na janela do Visualizador de Log de HTTP.


[![Usando o Visualizador de Log de HTTP para exibir dados de mensagem de solicitação e resposta.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Figura 7**: usando o Visualizador de Log de HTTP para exibir dados de mensagem de solicitação e resposta.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


O Visualizador de Log HTTP automaticamente analisa os objetos JSON e exibe-os usando uma exibição de árvore, tornando rápido e fácil de exibir dados de propriedade do objeto. Quando um UpdatePanel é usado em uma página ASP.NET AJAX, o Visualizador de quebra em cada parte da mensagem em partes individuais, conforme mostrado na Figura 8. Isso é um ótimo recurso que torna muito mais fácil de ver e entender o que está na mensagem em comparação com a exibição dos dados brutos de mensagem.


[![Uma mensagem de resposta de UpdatePanel exibida usando o Visualizador de Log de HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Figura 8**: mensagem de resposta de um UpdatePanel exibidos com o Visualizador de Log de HTTP.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Há várias outras ferramentas que podem ser usadas para exibir mensagens de solicitação e resposta, além de Web Development Helper. Outra boa opção é o Fiddler, que está disponível gratuitamente no [ http://www.fiddlertool.com ](http://www.fiddlertool.com). Embora o Fiddler não será discutida aqui, também é uma boa opção quando você precisar inspecione completamente os dados e os cabeçalhos da mensagem.

## <a name="debugging-with-firefox-and-firebug"></a>Depuração com o Firefox e o Firebug

Enquanto ainda, o Internet Explorer é o navegador mais amplamente usado, outros navegadores como Firefox têm se tornado muito populares e estão sendo mais usados. Como resultado, você desejará exibir e depurar suas páginas ASP.NET AJAX no Firefox, bem como Internet Explorer para garantir que seus aplicativos funcionem corretamente. Embora o Firefox não é possível vincular diretamente no Visual Studio 2008 para depuração, ele tem uma extensão chamada Firebug que pode ser usado para depurar páginas. O Firebug pode ser baixado gratuitamente, acessando [ http://www.getfirebug.com ](http://www.getfirebug.com).

O Firebug fornece um ambiente de depuração completa que pode ser usado para percorrer o código linha por linha, acessar todos os scripts usados dentro de uma página, exibir estruturas de DOM, exibir os estilos CSS e até mesmo rastreia eventos que ocorrem em uma página. Uma vez instalado, o Firebug pode ser acessado selecionando Ferramentas Firebug aberto Firebug no menu do Firefox. Como o Web Development Helper, Firebug é usado diretamente no navegador, embora também possa ser usado como um aplicativo autônomo.

Depois que estiver executando o Firebug, pontos de interrupção podem ser definidos em qualquer linha de um arquivo JavaScript, se o script é inserido em uma página ou não. Para definir um ponto de interrupção, primeiro carregue a página apropriada, que você deseja depurar no Firefox. Depois que a página for carregada, selecione o script para depurar a partir Scripts na lista suspensa do Firebug. Todos os scripts usados pela página serão mostrados. Um ponto de interrupção é definido clicando na área da bandeja cinza do Firebug na linha onde o ponto de interrupção deve ficar deve como você faria no Visual Studio 2008.

Depois que um ponto de interrupção foi definido no Firebug, você pode executar a ação necessária para executar o script que precisa ser depurado, como clicar em um botão ou atualizar o navegador para disparar o evento onLoad. Automaticamente, a execução será interrompida na linha que contém o ponto de interrupção. Figura 9 mostra um exemplo de um ponto de interrupção foi acionado no Firebug.


[![Tratamento de pontos de interrupção no Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Figura 9**: tratamento de pontos de interrupção no Firebug.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Depois que um ponto de interrupção é atingido pode intervir, depuração parcial ou sair do código usando os botões de seta. Conforme você percorre o código, as variáveis de script são exibidas na parte direita do depurador, permitindo que você veja valores e uma busca detalhada em objetos. O Firebug também inclui uma lista suspensa de pilha de chamadas para exibir as etapas de execução do script que levam à linha atual que está sendo depurada.

O Firebug também inclui uma janela de console que pode ser usada para testar as instruções de script diferente, avaliar as variáveis e exibir saída de rastreamento. Ela é acessada clicando na guia na parte superior da janela Firebug Console. A página que está sendo depurada pode também ser "inspecionada" para ver seu conteúdo e a estrutura de DOM clicando na guia inspecionar. Como você a coloque o mouse sobre os diferentes elementos de DOM, mostrado na janela Inspetor a parte apropriada da página será realçado tornando mais fácil ver onde o elemento é usado na página. Os valores de atributo associados com um determinado elemento podem ser alterados "live" para fazer experiências com a aplicação de larguras diferentes, estilos, etc. para um elemento. Isso é um recurso interessante que poupa de precisar mudar constantemente entre o editor de código fonte e o navegador Firefox para exibir o efeito de alterações simples como uma página.

Figura 10 mostra um exemplo de como usar o Inspetor de DOM para localizar uma caixa de texto chamada txtCountry na página. O Inspetor de Firebug também pode ser usado para exibir os estilos CSS usados em uma página, bem como os eventos que ocorrem como acompanhar os movimentos do mouse, cliques de botão e muito mais.


[![Usando o Inspetor de DOM do Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Figura 10**: Inspetor do usando o Firebug DOM.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


O Firebug fornece uma maneira leve rapidamente depurar uma página diretamente no Firefox, bem como uma ferramenta excelente para inspecionar os diferentes elementos dentro da página.

## <a name="debugging-support-in-aspnet-ajax"></a>Suporte à depuração no ASP.NET AJAX

A biblioteca do ASP.NET AJAX inclui muitas classes diferentes que podem ser usadas para simplificar o processo de adição de recursos do AJAX em uma página da Web. Você pode usar essas classes para localizar elementos em uma página e manipulá-los, adicione novos controles, chamar serviços da Web e até mesmo manipular eventos. A biblioteca ASP.NET AJAX também contém classes que podem ser usadas para aprimorar o processo de depuração de páginas. Nesta seção, você será apresentado à classe Debug e ver como ele pode ser usado em aplicativos.

*Usando a classe Sys. Debug*

A classe Debug (uma classe JavaScript localizada no namespace Sys) pode ser usada para executar várias funções diferentes, incluindo a gravação da saída de rastreamento, a execução de declarações de código e forçando o código falhar para que ele possa ser depurado. Ele é usado extensivamente em arquivos de depuração da biblioteca AJAX do ASP.NET (instalados por padrão em C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0) para executar testes condicionais ( chamado asserções) que garantir que parâmetros são passados corretamente para funções, que objetos contêm os dados esperados e escrever instruções de rastreamento.

A classe Sys. Debug expõe várias funções diferentes que podem ser usadas para lidar com o rastreamento, declarações de código ou falhas, conforme mostrado na tabela 1.

**Tabela 1. Funções de classe Sys. Debug.**

| **Nome da Função** | **Descrição** |
| --- | --- |
| Assert (condição, mensagem, displayCaller) | Declara que o parâmetro de condição é verdadeiro. Se a condição testada for false, uma caixa de mensagem será usada para exibir o valor do parâmetro de mensagem. Se o parâmetro displayCaller for true, o método também exibe informações sobre o chamador. |
| clearTrace() | Apaga a saída de instruções de operações de rastreamento. |
| Fail(Message) | Faz com que o programa parar a execução e invadir o depurador. O parâmetro da mensagem pode ser usado para fornecer um motivo da falha. |
| trace(Message) | Grava o parâmetro da mensagem para a saída de rastreamento. |
| traceDump (object, nome) | Gera dados de um objeto em um formato legível. O parâmetro de nome pode ser usado para fornecer um rótulo para o despejo de rastreamento. Quaisquer subobjetos dentro do objeto sendo despejada serão gravados por padrão. |

Rastreamento do lado do cliente pode ser usado da mesma forma como a funcionalidade de rastreamento disponível no ASP.NET. Ele permite que mensagens diferentes ser facilmente vistos sem interromper o fluxo do aplicativo. Listagem 5 mostra um exemplo de como usar a função Sys.Debug.trace para gravar no log de rastreamento. Essa função simplesmente pega a mensagem que deve ser escrita como um parâmetro.

**Listagem 5. Usando a função Sys.Debug.trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Se você executar o código mostrado na listagem 5, você não verá nenhuma saída de rastreamento na página. A única maneira de vê-lo é usar uma janela do console disponível no Visual Studio .NET, Web Development Helper ou Firebug. Se você quiser ver a saída de rastreamento na página, em seguida, você precisará adicionar uma marca de área de texto e dê a ele uma identificação de TraceConsole conforme mostrado a seguir:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

As instruções de Sys.Debug.trace na página serão gravadas TraceConsole TextArea.

Em casos onde você deseja ver os dados contidos em um objeto JSON você pode usar a função de traceDump da classe Sys. Debug. Essa função usa dois parâmetros, incluindo o objeto que deve ser despejado para o console de rastreamento e um nome que pode ser usado para identificar o objeto na saída do rastreamento. Listagem 6 mostra um exemplo de como usar a função traceDump.

**Listagem 6. Usando a função Sys.Debug.traceDump.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

Figura 11 mostra a saída da chamada da função Sys.Debug.traceDump. Observe que, além de gravar dados do objeto de pessoa, ele também grava os dados de endereço subdo objeto.

Além de rastreamento, a classe Sys. Debug também pode ser usada para executar declarações de código. Asserções são usadas para testar que condições específicas forem atendidas, enquanto um aplicativo está em execução. A versão de depuração dos scripts de biblioteca do ASP.NET AJAX contêm várias instruções para testar uma variedade de condições de assert.

Listagem 7 mostra um exemplo de como usar a função Sys.Debug.assert para testar uma condição. O código testa se o objeto de endereço é nulo antes de atualizar um objeto Person.


[![Saída da função Sys.Debug.traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Figura 11**: saída da função Sys.Debug.traceDump.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Listagem 7. Usando a função Debug. Assert.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Três parâmetros são passados, incluindo a condição a ser avaliada, a mensagem a ser exibida se a asserção retornar false e ou não as informações sobre o chamador devem ser exibidas. Em casos onde uma asserção falha, a mensagem será exibida, bem como informações do chamador se o terceiro parâmetro for verdadeiro. Figura 12 mostra um exemplo da caixa de diálogo falha será exibida se a asserção mostrada na listagem 7 falhará.

A função final para cobrir é Sys.Debug.fail. Quando você deseja forçar o código falhar em uma linha específica em um script, você pode adicionar uma chamada Sys.Debug.fail em vez da instrução do depurador geralmente usadas em aplicativos JavaScript. A função Sys.Debug.fail aceita um parâmetro único de cadeia de caracteres que representa o motivo da falha, conforme mostrado a seguir:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Uma mensagem de falha de Sys.Debug.assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Figura 12**: Sys.Debug.assert uma mensagem de falha.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Quando uma instrução Sys.Debug.fail é encontrada durante a execução de um script, o valor do parâmetro de mensagem será exibido no console de um aplicativo de depuração, como o Visual Studio 2008 e você será solicitado para depurar o aplicativo. Um caso em que isso pode ser muito útil é quando você não pode definir um ponto de interrupção com o Visual Studio 2008 em um script embutido, mas gostaria de ter o código para parar na linha específica para que você possa inspecionar o valor de variáveis.

*Noções básicas sobre ScriptMode propriedade do controle ScriptManager*

A biblioteca ASP.NET AJAX inclui depuração e versão de versões de script que são instaladas em C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 por padrão. Os scripts de depuração são bem formatada, fácil de ler e tem várias chamadas de Sys.Debug.assert espalhados-los enquanto os scripts de versão têm espaço em branco eliminado e usam a classe Sys. Debug com moderação para minimizar o tamanho geral.

O controle do ScriptManager adicionado às páginas ASP.NET AJAX lê atributo de depuração o elemento de compilação na Web. config para determinar quais versões de scripts de biblioteca para carregar. No entanto, você pode controlar se debug ou release scripts são carregados (scripts de biblioteca ou seus próprios scripts personalizados) alterando a propriedade ScriptMode. ScriptMode aceita uma enumeração ScriptMode cujos membros incluem automática, depuração, versão e herdar.

ScriptMode assume como padrão um valor de Auto, que significa que o ScriptManager verificará o atributo de depuração na Web. config. Quando a depuração é false ScriptManager carregará a versão de lançamento de scripts de biblioteca do AJAX ASP.NET. Quando a depuração é verdadeira a versão de depuração dos scripts será carregada. Alterar a propriedade ScriptMode para liberação ou depuração forçará o ScriptManager para carregar os scripts apropriados, independentemente de qual valor o atributo de depuração tem no Web. config. Listagem 8 mostra um exemplo de como usar o controle ScriptManager para carregar scripts de depuração da biblioteca do AJAX ASP.NET.

**Listagem 8. Carregamento de scripts de depuração usando o ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Você também pode carregar versões diferentes (debug ou release) de seus próprios scripts personalizados usando a propriedade de Scripts do ScriptManager, juntamente com o componente ScriptReference conforme mostrado na listagem 9.

**Listagem 9. Carregamento de scripts personalizados que usam o ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Se você está carregando scripts personalizados que usam o componente ScriptReference deve notificar o ScriptManager, quando o script tiver concluído o carregamento, adicionando o seguinte código na parte inferior do script:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

O código mostrado na listagem 9 informa o ScriptManager para procurar por uma versão de depuração do script pessoa para que ele irá procurar automaticamente Person.debug.js em vez de Person. Se o arquivo Person.debug.js não for encontrado que um erro será gerado.

Em casos em que você deseja uma depuração ou versão de lançamento de um script personalizado a ser carregado com base no valor da propriedade ScriptMode definido no controle do ScriptManager, você pode definir a ScriptReference propriedade do controle ScriptMode para herdar. Isso fará com que a versão apropriada do script personalizado para ser carregado com base na propriedade de ScriptMode do ScriptManager como mostrado na listagem 10. Como a propriedade ScriptMode do controle ScriptManager está definida para depuração, o script Person.debug.js será carregado e usado na página.

**Listagem 10. Herdando o ScriptMode do ScriptManager para scripts personalizados.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Usando a propriedade ScriptMode adequadamente com mais facilidade você pode depurar aplicativos e simplificar o processo geral. Scripts de versão da biblioteca do AJAX ASP.NET são em vez disso, difíceis de percorrer e lidos desde que a formatação de código foi removida enquanto os scripts de depuração são formatados especificamente para fins de depuração.

## <a name="conclusion"></a>Conclusão

Uma tecnologia da Microsoft ASP.NET AJAX fornece uma base sólida para a criação de aplicativos habilitados para AJAX que podem melhorar a experiência geral do usuário final. No entanto, como com qualquer tecnologia de programação, bugs e outros problemas de aplicativo certamente surgirão. Sabendo sobre as diferentes opções de depuração disponíveis pode economizar muito tempo e o resultado em um produto mais estável.

Neste artigo você foi apresentado para várias técnicas diferentes para depuração de páginas ASP.NET AJAX incluindo o Internet Explorer com o Visual Studio 2008, o Web Development Helper e o Firebug. Essas ferramentas podem simplificar o processo geral de depuração, pois você pode acessar dados da variável, percorrer o código linha por linha e exibir as instruções de rastreamento. Além das ferramentas de depuração diferentes discutidas, você também viu como a classe de sys. Debug da biblioteca AJAX do ASP.NET pode ser usado em um aplicativo e como a classe ScriptManager pode ser usada para carregar depurar ou liberação de versões de scripts.

## <a name="bio"></a>Biografia

Dan Wahlin (Microsoft Most Valuable Professional do ASP.NET e XML Web Services) é desenvolvimento instrutor e arquitetura consultor .NET no treinamento técnico de Interface ([www.interfacett.com)](http://www.interfacett.com). Dan fundou o XML para o site da Web de desenvolvedores do ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), está na agência do palestrante da INETA e dá palestras em várias conferências. Dan é co-autor Professional Windows DNA (Wrox), ASP.NET: dicas, tutoriais e código (Sams), soluções do ASP.NET 1.1 Insider, Professional ASP.NET 2.0 AJAX (Wrox), Hacks do ASP.NET 2.0 MVP e XML criado para desenvolvedores do ASP.NET (Sams). Quando ele não está escrevendo código, artigos ou livros, Dan gosta de escrever e gravando música e reprodução de Golfe e o basquete com sua esposa e filhos.

Scott Cate tem trabalhado com tecnologias Web Microsoft desde 1997 e é o presidente da myKB.com ([www.myKB.com](http://www.myKB.com)) onde ele é especialista na escrita de ASP.NET com base em aplicativos com foco em soluções de Software da Base de dados de Conhecimento. Scott pode ser contatado através do email [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou em seu blog em [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-web-services.md)
