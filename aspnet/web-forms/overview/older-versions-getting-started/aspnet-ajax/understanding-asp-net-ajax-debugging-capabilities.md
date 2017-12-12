---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: "Noções básicas sobre os recursos de depuração do ASP.NET AJAX | Microsoft Docs"
author: scottcate
description: "A capacidade de depurar o código é uma habilidade que todos os desenvolvedores devem ter em seu arsenal independentemente da tecnologia que está sendo usada. Enquanto muitos desenvolvedores..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 426d0182978faf7fc7516203fcc84ef0152790ba
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>Noções básicas sobre os recursos de depuração do ASP.NET AJAX
====================
por [Scott Ndicar](https://github.com/scottcate)

[Baixar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> A capacidade de depurar o código é uma habilidade que todos os desenvolvedores devem ter em seu arsenal independentemente da tecnologia que está sendo usada. Muitos desenvolvedores estão acostumados a usar o Visual Studio .NET ou o Web Developer Express para depurar aplicativos ASP.NET que usam código VB.NET ou c#, alguns não cientes de que também são extremamente úteis para depurar o código do lado do cliente, como JavaScript. Também pode ser aplicado o mesmo tipo de técnicas usadas para depurar aplicativos .NET para aplicativos habilitados para AJAX e mais especificamente a aplicativos ASP.NET AJAX.


## <a name="debugging-aspnet-ajax-applications"></a>Depurando aplicativos do ASP.NET AJAX

Dan Wahlin

A capacidade de depurar o código é uma habilidade que todos os desenvolvedores devem ter em seu arsenal independentemente da tecnologia que está sendo usada. É preciso dizer que Noções básicas sobre as diferentes opções de depuração que estão disponíveis pode salvar uma enorme quantidade de tempo em um projeto e até mesmo alguns problemas. Muitos desenvolvedores estão acostumados a usar o Visual Studio .NET ou o Web Developer Express para depurar aplicativos ASP.NET que usam código VB.NET ou c#, alguns não cientes de que também são extremamente úteis para depurar o código do lado do cliente, como JavaScript. Também pode ser aplicado o mesmo tipo de técnicas usadas para depurar aplicativos .NET para aplicativos habilitados para AJAX e mais especificamente a aplicativos ASP.NET AJAX.

Neste artigo, você verá como o Visual Studio 2008 e várias outras ferramentas podem ser usadas para depurar aplicativos do ASP.NET AJAX para localizar rapidamente bugs e outros problemas. Esta discussão inclui informações sobre como habilitar o Internet Explorer 6 ou posterior para depuração, usando o Visual Studio 2008 e o Gerenciador de Script ao percorrer o código, bem como usar outras ferramentas gratuitas como auxiliar de desenvolvimento da Web. Você também aprenderá como depurar aplicativos do ASP.NET AJAX no Firefox usando que uma extensão denominada Firebug permite que você percorrer o código JavaScript diretamente no navegador sem qualquer outra ferramenta. Por fim, você será apresentado para classes da biblioteca do AJAX do ASP.NET que pode ajudar com várias tarefas de depuração, como o rastreamento e instruções de declaração de código.

Antes de você tentar depurar páginas exibidas no Internet Explorer, há algumas etapas básicas, que você precisará executar para habilitá-la para depuração. Vamos dar uma olhada em alguns requisitos de configuração básica que precisam ser executadas para começar.

## <a name="configuring-internet-explorer-for-debugging"></a>Configurando o Internet Explorer para depuração

A maioria das pessoas não estiver interessado em ver JavaScript problemas encontrados em um site exibido com o Internet Explorer. Na verdade, o usuário médio ainda não saberia o que fazer se eles viam uma mensagem de erro. Como resultado, opções de depuração estão desativadas por padrão no navegador. No entanto, é muito simples ativar a depuração e colocá-lo para usar como desenvolver novos aplicativos AJAX.

Para habilitar a funcionalidade de depuração, vá para Ferramentas Opções da Internet no menu do Internet Explorer e selecione a guia Avançado. Na seção navegação Certifique-se de que os seguintes itens estão desmarcados:

- Desabilitar depuração de scripts (Internet Explorer)
- Desabilitar depuração de scripts (outros)

Embora não obrigatório, se você está tentando depurar um aplicativo que você provavelmente vai querer quaisquer erros de JavaScript na página para ser imediatamente óbvia e visível. Você pode forçar todos os erros sejam exibidos com uma caixa de mensagem, marcando a caixa de seleção "Exibir notificação sobre cada erro de script". Enquanto isso é uma ótima opção para ativar enquanto você estiver desenvolvendo um aplicativo, ele pode se tornar rapidamente irritante, se você estiver apenas usando outros sites porque as chances de encontrar erros de JavaScript são muito boas.

Figura 1 mostra que o Internet Explorer Avançado da caixa de diálogo deve aparecer depois que ele foi configurado corretamente para depuração.


[![Configurar o Internet Explorer para depuração.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Figura 1**: configurar o Internet Explorer para depuração.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


Depois que a depuração foi ativada, você verá um novo item de menu aparecer no menu de exibição denominado depurador de Script. Ele tem duas opções disponíveis, incluindo aberto e quebra na próxima instrução. Quando abrir é selecionado você será solicitado para a página depuração no Visual Studio 2008 (Observe que o Visual Web Developer Express também pode ser usado para depuração). Se estiver executando o Visual Studio .NET, você pode escolher para usar essa instância ou para criar uma nova instância. Quando a interrupção na próxima instrução é selecionada você será solicitado a depurar a página quando o código JavaScript é executado. Se o código JavaScript é executado no evento onLoad da página, você pode atualizar a página para disparar uma sessão de depuração. Se o código JavaScript é executado depois que um botão é clicado, em seguida, o depurador será executado imediatamente depois que o botão é clicado.

> *>[!NOTE] se você estiver executando no Windows Vista com usuário acesso UAC (controle) habilitado e tiver o Visual Studio 2008 definido para ser executado como um administrador, o Visual Studio falhará ao anexar ao processo quando você for solicitado a anexar. Para contornar esse problema, inicie o Visual Studio e usar essa instância para depurar.*


Embora a próxima seção demonstrará como depurar uma página ASP.NET AJAX diretamente de dentro do Visual Studio 2008, usando a opção de depurador de Script do Internet Explorer é útil quando uma página já está aberta e você gostaria de mais totalmente inspecioná-lo.

## <a name="debugging-with-visual-studio-2008"></a>Depuração no Visual Studio 2008

O Visual Studio 2008 fornece funcionalidades de depuração para desenvolvedores em todo o mundo dependem de todos os dias para aplicativos .NET de depuração. O depurador interno permite que você percorrer o código, a exibição de dados de objeto de inspeção de variáveis específicas, monitoram a pilha de chamadas e muito mais. Além de depuração de código VB.NET ou c#, o depurador também é útil para depuração de aplicativos do ASP.NET AJAX e permitirá que você percorrer o código JavaScript, linha por linha. Os detalhes que seguem o foco em técnicas que podem ser usadas para depurar arquivos de script do lado do cliente em vez de fornecer um discourse sobre o processo geral de depuração de aplicativos usando o Visual Studio 2008.

O processo de depuração de uma página no Visual Studio 2008 pode ser iniciado de várias maneiras diferentes. Primeiro, você pode usar a opção de depurador de Script do Internet Explorer mencionada na seção anterior. Isso funciona bem quando uma página já está carregada no navegador e você gostaria de iniciar a depuração-lo. Como alternativa, você pode com o botão direito em uma página. aspx no Gerenciador de soluções e selecione Definir como página inicial no menu. Se você estiver acostumado a depuração de páginas ASP.NET, em seguida, você provavelmente fez isso antes. Se for pressionado F5 a página pode ser depurada. No entanto, embora geralmente você pode definir um ponto de interrupção em qualquer lugar você gostaria que no código VB.NET ou c#, que não é sempre o caso com JavaScript, você verá em seguida.

*Incorporado em vez de Scripts externos*

O depurador do Visual Studio 2008 trata JavaScript inserida em uma página diferente de arquivos JavaScript externos. Com arquivos de script externo, você pode abrir o arquivo e definir um ponto de interrupção em qualquer linha que você escolher. Pontos de interrupção podem ser definidos clicando na área de bandeja cinza à esquerda da janela do editor de código. Quando o JavaScript é inserido diretamente em uma página que usa o `<script>` marca, definindo um ponto de interrupção clicando na área de bandeja cinza não é uma opção. Tentativa de definir um ponto de interrupção em uma linha de script incorporado resulta em um aviso dizendo "Não é um local válido para um ponto de interrupção".

Você pode contornar esse problema movendo o código em um arquivo. js externo e fazem referência a ele usando o atributo src do &lt;script&gt; marca:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

E se movendo o código em um arquivo externo não é uma opção ou exige mais trabalho do que vale a pena? Enquanto você não pode definir um ponto de interrupção usando o editor, você pode adicionar a instrução de depurador diretamente no código onde você deseja iniciar a depuração. Você também pode usar a classe sys disponível na biblioteca do ASP.NET AJAX para forçar a depuração para iniciar. Você aprenderá mais sobre a classe sys neste artigo.

Um exemplo de como usar o `debugger` palavra-chave é mostrado na listagem 1. Este exemplo força o depurador para interromper direita antes de que é feita uma chamada para uma função de atualização.

**Listando 1. Usando a palavra-chave do depurador para forçar o depurador do Visual Studio .NET para interromper.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Depois que a instrução de depurador é atingida você será solicitado a depurar a página usando o Visual Studio .NET e pode começar percorrendo o código. Ao fazer isso, você pode encontrar um problema com o acesso a arquivos de script do ASP.NET AJAX biblioteca usados na página, portanto, vamos dar uma olhada usando o Visual Studio. Gerenciador de Script do NET.

## <a name="using-visual-studio-net-windows-to-debug"></a>Usando o Visual Studio .NET Windows para depurar

Depois que uma sessão de depuração é iniciada e percorrer o código usando a tecla F11 padrão de começar, você pode encontrar a caixa de diálogo de erro mostrada no consulte a Figura 2, a menos que todos os arquivos de script usados na página são abertos e disponível para depuração.


[![Caixa de diálogo de erro mostrada quando nenhum código-fonte está disponível para depuração.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Figura 2**: caixa de diálogo de erro mostrada quando nenhum código-fonte está disponível para depuração.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Essa caixa de diálogo é mostrada porque o Visual Studio .NET não-se como obter o código-fonte de alguns dos scripts referenciados pela página. Enquanto isso pode ser muito frustrante primeiro, há uma correção simple. Depois de iniciar uma sessão de depuração e um ponto de interrupção, vá para a janela do Gerenciador de Script do Windows de depuração no menu do Visual Studio 2008 ou use a tecla de atalho Ctrl + Alt + N.

> *>[!NOTE] Se você não vir o menu do Gerenciador de Script listado, vá para ferramentas* *personalizar* *comandos no menu do Visual Studio .NET. Localize a entrada de depuração na seção de categorias e clique nele para mostrar todas as entradas de menu disponíveis. Na lista de comandos, role para baixo até o Gerenciador de Script e, em seguida, arraste-o para cima para a depuração* *menu do Windows em mencionado anteriormente. Isso disponibilizará a entrada de menu do Gerenciador de Script toda vez que executar o Visual Studio .NET.*


O Gerenciador de Script pode ser usado para exibir todos os scripts usados em uma página e abri-los no editor de códigos. Quando o Gerenciador de Script é aberto, clique duas vezes na página. aspx sendo depurada no momento para abri-lo na janela do editor de código. Execute a mesma ação para todos os outros scripts mostrados no Gerenciador de Script. Depois que todos os scripts estão abertos na janela de código, você pode pressione F11 (e use as outras teclas de atalho de depuração) para depurar seu código. A Figura 3 mostra um exemplo do Gerenciador de Script. Ele lista o arquivo atual que está sendo depurado (Demo.aspx), bem como dois scripts personalizados e dois scripts injetadas dinamicamente a página, o ASP.NET AJAX ScriptManager.


[![O Gerenciador de Script oferece acesso fácil para scripts usados em uma página.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Figura 3**. O Gerenciador de Script oferece acesso fácil para scripts usados em uma página.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


Várias outras windows também podem ser usados para fornecer informações úteis, como percorrer o código em uma página. Por exemplo, você pode usar a janela locais para ver os valores das variáveis diferentes usadas na página, a janela imediata para avaliar variáveis específicas ou condições e exibir a saída. Você também pode usar a janela de saída para ver instruções de rastreamento é gravadas usando a função Sys.Debug.trace (que será abordada neste artigo) ou função Debug writeln do Internet Explorer.

Conforme você avança através do código usando o depurador pode passar o mouse sobre as variáveis no código para exibir o valor que são atribuídos. No entanto, o depurador de scripts ocasionalmente não mostrará nada que você passe o mouse sobre uma variável JavaScript. Para ver o valor, realce a instrução ou a variável que você está tentando ver na janela do editor de código e, em seguida, passe o mouse sobre ele. Embora essa técnica não funciona em todas as situações, muitas vezes você poderá ver o valor sem precisar consultar em uma janela de depuração diferente como a janela locais.

Um tutorial em vídeo demonstrando alguns dos recursos discutidos aqui pode ser exibido no [http://www.xmlforasp.net](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Depurando com o auxiliar de desenvolvimento da Web

Embora o Visual Studio 2008 (e o Visual Web Developer Express 2008) são muito compatíveis com as ferramentas de depuração, há opções adicionais que podem ser usadas também que são mais leve. Uma das ferramentas mais recentes sejam liberados é o auxiliar de desenvolvimento da Web. Nikhil Kothari da Microsoft (um dos arquitetos de ASP.NET AJAX chave da Microsoft) gravou esta ferramenta excelente que pode executar muitas tarefas diferentes de depuração simples para exibir mensagens de solicitação e resposta HTTP. Auxiliar de desenvolvimento da Web pode ser baixado em [http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

Auxiliar de desenvolvimento da Web pode ser usado diretamente no Internet Explorer, que é conveniente usar. Ele é iniciado, selecionando o auxiliar de desenvolvimento de Web de ferramentas do menu do Internet Explorer. Isso abrirá a ferramenta na parte inferior do navegador que é bom, pois você não precisa sair do navegador para executar várias tarefas, como o log de mensagem de solicitação e resposta HTTP. A Figura 4 mostra a aparência de auxiliar de desenvolvimento da Web em ação.


[![Auxiliar de desenvolvimento da Web](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Figura 4**: auxiliar de desenvolvimento da Web ([clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Auxiliar de desenvolvimento da Web não é uma ferramenta que você usará para percorrer o código linha por linha como o Visual Studio 2008. No entanto, ele pode ser usado para exibir a saída de rastreamento, facilmente avaliar as variáveis em um script ou explorar dados estão dentro de um objeto JSON. Também é muito útil para exibir os dados que são passados de e para uma página ASP.NET AJAX e um servidor.

Quando auxiliar de desenvolvimento da Web é aberto no Internet Explorer, a depuração de script deve ser habilitada pelo selecionando a Script Ativar depuração de Script a partir do menu do auxiliar de desenvolvimento na Web, conforme mostrado anteriormente na Figura 4. Isso permite que a ferramenta de interceptação erros que ocorrem como uma página é executada. Ele também permite o fácil acesso a mensagens de rastreamento que são produzidas na página. Para exibir informações de rastreamento ou executar comandos de script para testar funções diferentes dentro de uma página, selecione o Script Mostrar Script Console no menu auxiliar de desenvolvimento da Web. Fornece acesso a uma janela de comando e uma janela imediata simples.

*Exibindo mensagens de rastreamento e dados de objeto JSON*

A janela imediata pode ser usada para executar comandos de script ou até mesmo carregar ou salvar scripts que são usados para testar funções diferentes em uma página. A janela de comando exibe as mensagens de rastreamento ou de depuração gravadas por página que está sendo exibida. A listagem 2 mostra como escrever uma mensagem de rastreamento usando a função Debug writeln do Internet Explorer.

**A listagem 2. Gravando mensagens de rastreamento do lado do cliente usando a classe de depuração.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Se a propriedade LastName contém um valor de Silva, o auxiliar de desenvolvimento da Web exibirá a mensagem "nome da pessoa: Doe" na janela de comando do console de script (supondo que a depuração foi habilitada). Auxiliar de desenvolvimento da Web também adiciona um objeto de nível superior debugService em páginas que podem ser usadas para gravar informações de rastreamento ou exibir o conteúdo de objetos JSON. A listagem 3 mostra um exemplo de como usar a função de rastreamento da classe debugService.

**A listagem 3. Usando classe de debugService do auxiliar de desenvolvimento da Web para gravar uma mensagem de rastreamento.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Um bom recurso da classe debugService é que ela funcionará mesmo que a depuração não está habilitada no Internet Explorer, tornando mais fácil para sempre acessar dados de rastreamento quando o auxiliar de desenvolvimento da Web está em execução. Quando a ferramenta não está sendo usada para depurar uma página, instruções de rastreamento serão ignoradas, desde que a chamada para window.debugService retornará false.

A classe debugService também permite que os dados de objeto JSON a ser exibido usando a janela do Inspetor do auxiliar de desenvolvimento da Web. A listagem 4 cria um objeto JSON simple que contém dados. Depois que o objeto é criado, é feita uma chamada para o debugService classe inspecionar função para permitir que o objeto JSON a ser inspecionado visualmente.

**A listagem 4. Usando a função debugService.inspect para exibir dados de objeto JSON.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

Na página ou através da janela imediata da chamada da função GetPerson() resultará na janela de diálogo do Inspetor de objetos que aparecem conforme mostrado na Figura 5. Propriedades em um objeto podem ser alteradas dinamicamente, realçando-los, alterando o valor exibido na caixa de texto valor e, em seguida, clique no link de atualização. Usar o Inspetor de objetos torna simples exibir dados de objeto JSON e experimentar aplicar valores diferentes de propriedades.

*Erros de depuração*

Além de permitir que os dados de rastreamento e objetos JSON a ser exibida, o auxiliar de desenvolvimento na Web também pode ajudar na depuração de erros em uma página. Se um erro for encontrado, você será solicitado para continuar para a próxima linha de código ou depurar o script (consulte a Figura 6). A janela de diálogo de erro de Script mostra pilha de chamadas completa, bem como os números de linha, para que você possa identificar facilmente onde os problemas estão dentro de um script.


[![Usando a janela do Inspetor de objetos para exibir um objeto JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Figura 5**: usando a janela do Inspetor de objetos para exibir um objeto JSON.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


Selecionando a opção de depuração permite que você execute instruções de script diretamente na janela de imediato do auxiliar de desenvolvimento da Web para exibir o valor de variáveis, gravar objetos JSON, além de mais. Se a mesma ação que acionou o erro é executada novamente e o Visual Studio 2008 está disponível no computador, você será solicitado a iniciar uma sessão de depuração para que você possa percorrer o código linha por linha conforme discutido na seção anterior.


[![Caixa de diálogo de erro de Script do auxiliar de desenvolvimento de Web](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Figura 6**: caixa de diálogo de erro de Script do auxiliar de desenvolvimento da Web ([clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*Inspecionar mensagens de resposta e solicitação*

Durante a depuração de páginas ASP.NET AJAX geralmente é útil ver mensagens de solicitação e resposta enviadas entre um servidor e de página. Exibir o conteúdo de mensagens permite que você veja se os dados apropriados está sendo passados, bem como o tamanho das mensagens. Auxiliar de desenvolvimento da Web fornece um excelente recurso de agente de log de mensagens HTTP que torna mais fácil exibir os dados como texto não processado ou em um formato mais legível.

Para exibir mensagens de solicitação e resposta do ASP.NET AJAX, o agente de log HTTP deve ser habilitado, selecionando o log HTTP HTTP habilitar no menu auxiliar de desenvolvimento da Web. Uma vez habilitada, todas as mensagens enviadas da página atual podem ser exibidas no Visualizador de log de HTTP que pode ser acessado selecionando HTTP Mostrar Logs de HTTP.

Embora é certamente útil exibir o texto não processado enviado em cada mensagem de solicitação/resposta (e uma opção no auxiliar de desenvolvimento da Web), geralmente é mais fácil de exibir dados de mensagem em um formato gráfico mais. Depois que o log HTTP foi habilitado e foram registradas mensagens, dados de mensagem podem ser exibidos clicando duas vezes na mensagem no Visualizador de log HTTP. Isso permite que você exiba todos os cabeçalhos associados a uma mensagem, bem como a mensagem real conteúdo. Figura 7 mostra um exemplo de uma mensagem de solicitação e a mensagem de resposta exibidos na janela do Visualizador de Log de HTTP.


[![Usando o Visualizador de Log de HTTP para exibir dados de mensagem de solicitação e resposta.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Figura 7**: usando o Visualizador de Log de HTTP para exibir dados de mensagem de solicitação e resposta.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


O Visualizador de Log HTTP automaticamente analisa objetos JSON e exibe-os usando uma exibição de árvore torna rápido e fácil de exibir dados de propriedade do objeto. Quando um UpdatePanel está sendo usado em uma página ASP.NET AJAX, o Visualizador de quebra cada parte da mensagem em partes individuais, conforme mostrado na Figura 8. Este é um ótimo recurso que torna muito mais fácil de ver e entender o que é a mensagem em comparação com a exibição dos dados brutos de mensagem.


[![Uma mensagem de resposta do UpdatePanel exibida com o Visualizador de Log HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Figura 8**: mensagem de resposta de um UpdatePanel exibido usando o Visualizador de Log HTTP.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Há várias outras ferramentas que podem ser usadas para exibir mensagens de solicitação e resposta, além de auxiliar de desenvolvimento da Web. Outra boa opção é o Fiddler, que está disponível gratuitamente no [http://www.fiddlertool.com](http://www.fiddlertool.com). Embora o Fiddler não será discutido aqui, também é uma boa opção quando você precisa Inspecione os cabeçalhos de mensagem e dados.

## <a name="debugging-with-firefox-and-firebug"></a>Depurando com o Firefox e Firebug

Enquanto o Internet Explorer ainda é o navegador mais amplamente usado, outros navegadores como Firefox se tornou bastante populares e estão sendo usados mais. Como resultado, você desejará exibir e depurar suas páginas ASP.NET AJAX no Firefox, bem como Internet Explorer para garantir que seus aplicativos funcionem corretamente. Embora o Firefox não é possível vincular diretamente no Visual Studio 2008 para depuração, ele tem uma extensão chamada Firebug que pode ser usado para depurar páginas. Firebug pode ser baixado gratuitamente acessando [http://www.getfirebug.com](http://www.getfirebug.com).

Firebug fornece um ambiente de depuração completo que pode ser usado para percorrer o código linha por linha, acessar todos os scripts usados em uma página, exibir estruturas de DOM, exibir estilos CSS e até mesmo rastreia eventos que ocorrem em uma página. Uma vez instalado, Firebug pode ser acessado selecionando Ferramentas Firebug abrir Firebug no menu Firefox. Como auxiliar de desenvolvimento da Web, Firebug é usado diretamente no navegador, embora também possa ser usado como um aplicativo autônomo.

Quando estiver em execução Firebug, pontos de interrupção podem ser definidos em qualquer linha de um arquivo JavaScript se o script é inserido em uma página ou não. Para definir um ponto de interrupção, primeiro carregue a página apropriada, que você deseja depurar no Firefox. Depois que a página for carregada, selecione o script para depurar na lista de lista suspensa de Scripts do Firebug. Todos os scripts usados pela página serão exibidos. Um ponto de interrupção é definido clicando na área de bandeja cinza do Firebug na linha em que o ponto de interrupção deve ir deve como você faria no Visual Studio 2008.

Quando um ponto de interrupção foi definido no Firebug, você pode executar a ação necessária para executar o script que precisa ser depurado como clicar em um botão ou atualizar o navegador para acionar o evento onLoad. Execução serão interrompidos automaticamente na linha que contém o ponto de interrupção. Figura 9 mostra um exemplo de um ponto de interrupção foi acionado no Firebug.


[![Manipulação de pontos de interrupção no Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Figura 9**: manipulação de pontos de interrupção no Firebug.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


Depois que um ponto de interrupção é atingido pode entrar, passar por ou sair do código usando os botões de seta. À medida que você percorrer o código, variáveis de script são exibidos na parte direita do depurador, permitindo que você veja valores e fazer drill-down em objetos. O Firebug também inclui uma lista suspensa de pilha de chamadas para exibir as etapas de execução do script que levam à linha atual que está sendo depurada.

O Firebug também inclui uma janela de console que pode ser usada para testar as instruções de script diferente, variáveis e exibir saída de rastreamento. Ele é acessado clicando na guia na parte superior da janela Firebug Console. A página que está sendo depurada pode também ser "inspecionada" para ver sua estrutura de DOM e conteúdo clicando na guia inspecionar. Como você o mouse sobre os diferentes elementos de DOM mostrado na janela do Inspetor a parte apropriada da página será realçado tornando mais fácil ver onde o elemento é usado na página. Valores de atributo associados a um determinado elemento podem ser alterados "live" para fazer experiências com a aplicação de larguras diferentes, estilos, etc. para um elemento. Este é um recurso interessante que evita que você precise constantemente alternar entre o editor de código fonte e o navegador Firefox para exibir o efeito de alterações simples como uma página.

Figura 10 mostra um exemplo de como usar o Inspetor de DOM para localizar uma caixa de texto denominada txtCountry na página. O Inspetor de Firebug também pode ser usado para exibir os estilos CSS usados em uma página, bem como os eventos que ocorrem, como controle de seus movimentos, cliques de botão, além de mais.


[![Usando o Inspetor de DOM do Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Figura 10**: Inspetor de DOM do Firebug usando.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug fornece uma maneira de leve para depurar rapidamente uma página diretamente no Firefox, bem como uma excelente ferramenta para inspecionar diferentes elementos dentro da página.

## <a name="debugging-support-in-aspnet-ajax"></a>Suporte à depuração no AJAX ASP.NET

A biblioteca ASP.NET AJAX inclui muitas classes diferentes que podem ser usados para simplificar o processo de adição de recursos de AJAX em uma página da Web. Você pode usar essas classes para localizar elementos em uma página e manipulá-los, adicione novos controles, chamar serviços da Web e até mesmo tratar eventos. A biblioteca ASP.NET AJAX também contém classes que podem ser usadas para aprimorar o processo de depuração de páginas. Nesta seção, você será apresentado à classe sys e ver como ele pode ser usado em aplicativos.

*Usando a classe Sys*

A classe de sys. Debug (uma classe JavaScript localizada no namespace Sys) pode ser usada para executar várias funções diferentes, incluindo gravar saída de rastreamento, executar declarações de código e forçar o código de falha para que ele possa ser depurado. Ele é usado extensivamente em arquivos de depuração da biblioteca ASP.NET AJAX (instalados por padrão em C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0) para executar testes condicionais ( chamado asserções) que certifique-se de parâmetros são passados corretamente para funções, objetos contêm os dados esperados e para gravar instruções de rastreamento.

A classe sys expõe várias funções diferentes que podem ser usadas para controlar o rastreamento, declarações de código ou falhas, conforme mostrado na tabela 1.

**Tabela 1. Funções de classe Sys.**

| **Nome da Função** | **Descrição** |
| --- | --- |
| Assert (condição, mensagem, displayCaller) | Declara que o parâmetro de condição é verdadeiro. Se a condição que está sendo testada for false, uma caixa de mensagem será usada para exibir o valor do parâmetro de mensagem. Se o parâmetro displayCaller for true, o método também exibe informações sobre o chamador. |
| clearTrace() | Apaga a saída de instruções de operações de rastreamento. |
| Fail(Message) | Faz com que o programa para interromper a execução e invadir o depurador. O parâmetro de mensagem pode ser usado para fornecer um motivo da falha. |
| trace(Message) | Grava o parâmetro de mensagem para a saída de rastreamento. |
| traceDump (objeto, nome) | Gera dados de um objeto em um formato legível. O parâmetro de nome pode ser usado para fornecer um rótulo para o despejo de rastreamento. Todos os objetos dentro do objeto sendo despejada sub serão gravados por padrão. |

Rastreamento do lado do cliente pode ser usado da mesma maneira como a funcionalidade de rastreamento disponível no ASP.NET. Ele permite que mensagens diferentes ser visto facilmente sem interromper o fluxo do aplicativo. Listagem 5 mostra um exemplo de como usar a função Sys.Debug.trace para gravar no log de rastreamento. Essa função usa apenas a mensagem que deve ser escrita como um parâmetro.

**Listagem 5. Usando a função Sys.Debug.trace.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Se você executar o código mostrado na listagem 5, você não verá nenhuma saída de rastreamento na página. É a única maneira de vê-lo usar uma janela de console disponível no Visual Studio .NET, o auxiliar de desenvolvimento da Web ou Firebug. Se você deseja ver a saída de rastreamento na página, em seguida, você precisará adicionar uma marca de área de texto e dê a ele uma identificação de TraceConsole como mostrado a seguir:


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Todas as instruções na página Sys.Debug.trace serão gravadas TraceConsole TextArea.

Em casos em que você deseja ver os dados contidos em um objeto JSON você pode usar a função de traceDump da classe Sys. Essa função usa dois parâmetros, incluindo o objeto que deve ser despejado no console de rastreamento e um nome que pode ser usado para identificar o objeto na saída do rastreamento. Listagem 6 mostra um exemplo de como usar a função traceDump.

**Listagem 6. Usando a função Sys.Debug.traceDump.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

A Figura 11 mostra a saída da chamada da função Sys.Debug.traceDump. Observe que além de escrever os dados do objeto de pessoa, ele também grava os dados de endereço subdo objeto.

Além de rastreamento, a classe sys pode ser usada para executar declarações de código. Declarações são usadas para testar se condições específicas forem atendidas, enquanto um aplicativo está em execução. A versão de depuração de scripts da biblioteca do ASP.NET AJAX contêm várias declarações instruções para uma variedade de condições de teste.

Listagem 7 mostra um exemplo de como usar a função Sys.Debug.assert para testar uma condição. O código testa se o objeto de endereço é nulo antes de atualizar o objeto de uma pessoa ou não.


[![Saída da função Sys.Debug.traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Figura 11**: saída da função Sys.Debug.traceDump.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**Listagem 7. Usando a função Assert.**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Três parâmetros são passados como a condição a ser avaliada, a mensagem a ser exibida se a asserção retornar false e se as informações sobre o chamador devem ser exibidas. Em casos onde uma asserção falha, a mensagem será exibida, bem como informações do chamador se o terceiro parâmetro for true. Figura 12 mostra um exemplo da caixa de diálogo falha que será exibido se a declaração mostrada na listagem 7 falhará.

A função final para cobrir é Sys.Debug.fail. Quando você quiser forçar o código de falha em uma linha específica em um script, você pode adicionar uma chamada Sys.Debug.fail em vez da instrução de depurador geralmente usadas em aplicativos JavaScript. A função Sys.Debug.fail aceita um parâmetro único de cadeia de caracteres que representa o motivo da falha, como mostrado a seguir:


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Uma mensagem de falha de Sys.Debug.assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Figura 12**: Sys.Debug.assert A mensagem de falha.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Quando uma instrução Sys.Debug.fail é encontrada durante a execução de um script, o valor do parâmetro de mensagem será exibido no console de um aplicativo de depuração, como o Visual Studio 2008 e você será solicitado a depurar o aplicativo. Um caso em que isso pode ser muito útil é quando você não pode definir um ponto de interrupção com o Visual Studio 2008 em um script embutido, mas deseja que o código para parar em linha específica para que você possa inspecionar o valor de variáveis.

*Noções básicas sobre ScriptMode propriedade do controle ScriptManager*

A biblioteca ASP.NET AJAX inclui de depuração e lançamento versões de script que são instaladas em C:\Program Files\Microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 por padrão. Os scripts de depuração são bem formatada, fácil de ler e tem várias chamadas de Sys.Debug.assert espalhados-los enquanto os scripts de versão tem espaço em branco eliminado e usam a classe sys moderadamente para minimizar o tamanho geral.

O controle ScriptManager adicionado a páginas ASP.NET AJAX lê atributo de depuração o elemento de compilação no Web. config para determinar quais versões dos scripts da biblioteca para carregar. No entanto, você pode controlar se depurar ou scripts de versão são carregados (scripts de biblioteca ou seus próprios scripts personalizados) alterando a propriedade ScriptMode. ScriptMode aceita uma enumeração ScriptMode cujos membros incluem automática, depuração, versão e herdar.

O padrão é ScriptMode como um valor de Auto, que significa que o ScriptManager verificará se o atributo de depuração no Web. config. Quando a depuração é false ScriptManager carregará a versão de lançamento de scripts da biblioteca ASP.NET AJAX. Quando a depuração é verdadeira a versão de depuração de scripts será carregada. A alteração da propriedade ScriptMode para liberar ou depurar forçará o ScriptManager carregue scripts apropriados, independentemente de qual é o valor que tem o atributo de depuração no Web. config. Listagem 8 mostra um exemplo do uso de controle ScriptManager para carregar scripts de depuração da biblioteca do ASP.NET AJAX.

**Listando 8. Carregar scripts de depuração usando o ScriptManager**.


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

Você também pode carregar versões diferentes (debug ou release) de seus próprios scripts personalizados usando a propriedade de Scripts do ScriptManager junto com o componente ScriptReference conforme mostrado na listagem 9.

**Listagem 9. Carregar scripts personalizados que usam o ScriptManager.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Se você estiver carregando scripts personalizados que usam o componente ScriptReference você deve notificar o ScriptManager quando o script terminou o carregamento, adicionando o seguinte código na parte inferior do script:


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

O código mostrado na listagem 9 informa o ScriptManager para procurar por uma versão de depuração do script pessoa para que ela procurará automaticamente Person.debug.js em vez de Person.js. Se o arquivo Person.debug.js não foi encontrado que um erro será gerado.

Em casos onde você deseja uma depuração ou versão de lançamento de um script personalizado a ser carregado com base no valor da propriedade ScriptMode definido no controle ScriptManager, você pode definir a ScriptReference propriedade do controle ScriptMode para herdar. Isso fará com que a versão apropriada do script personalizado para ser carregado com base na propriedade de ScriptMode do ScriptManager como mostrado na listagem 10. Como a propriedade ScriptMode do controle ScriptManager é definida para depuração, o script Person.debug.js será carregado e usado na página.

**Listagem 10. Herdando a ScriptMode ScriptManager para scripts personalizados.**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Usando a propriedade ScriptMode adequadamente mais facilmente você pode depurar aplicativos e simplificar o processo geral. Os scripts de versão da biblioteca ASP.NET AJAX são difíceis de depurar e ler desde a formatação de código foi removido enquanto os scripts de depuração são formatados especificamente para fins de depuração.

## <a name="conclusion"></a>Conclusão

Tecnologia de ASP.NET AJAX da Microsoft fornece uma base sólida para a criação de aplicativos habilitados para AJAX que podem melhorar a experiência geral do usuário final. No entanto, como com qualquer tecnologia de programação, bugs e outros problemas de aplicativo certamente ocorrerá. Saber sobre as diferentes opções de depuração disponíveis pode economizar muito tempo e resultados em um produto mais estável.

Neste artigo, você já foi apresentado para várias técnicas diferentes para depuração de páginas de ASP.NET AJAX, incluindo o Internet Explorer com o Visual Studio 2008, o auxiliar de desenvolvimento da Web e o Firebug. Essas ferramentas podem simplificar o processo geral de depuração, desde que você pode acessar dados da variável, percorrer o código linha por linha e exibir as instruções de rastreamento. Além das ferramentas de depuração diferentes discutidas, você também viu como classe de sys. Debug da biblioteca ASP.NET AJAX pode ser usado em um aplicativo e como a classe de ScriptManager pode ser usada para carregar depurar ou lançar versões de scripts.

## <a name="bio"></a>Biografia do

Dan Wahlin (Microsoft Most Valuable Professional para ASP.NET e XML Web Services) é um consultor .NET de instrutor e arquitetura de desenvolvimento no treinamento de Interface ([www.interfacett.com)](http://www.interfacett.com). Dan fundada o XML para o site da Web de desenvolvedores do ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), está em agência no apresentador INETA e participa de várias conferências. Dan autoria conjunta Professional Windows DNA (Wrox), ASP.NET: dicas, tutoriais e código (Sams), ASP.NET 1.1 Insider soluções, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0 MVP experimenta e XML criado para desenvolvedores do ASP.NET (Sams). Quando ele não está escrevendo código, artigos ou manuais, Dan gosta de escrever e música de gravação e execução Golfe e basquete com sua mulher e filhos.

Scott Cate trabalha com tecnologias Microsoft Web desde 1997 e é presidente da myKB.com ([www.myKB.com](http://www.myKB.com)) onde ele é especializada em escrever ASP.NET com base em aplicativos voltados para soluções de Software da Base de dados de Conhecimento. Scott pode ser contatado via email em [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou em seu blog [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Anterior](understanding-asp-net-ajax-web-services.md)
