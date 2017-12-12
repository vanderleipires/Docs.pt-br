---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: "O que há de novo no ASP.NET 4.5 e o Visual Studio 2012 | Microsoft Docs"
author: rick-anderson
description: "Este documento descreve novos recursos e melhorias que estão sendo introduzidas no ASP.NET 4.5. Ele também descreve as melhorias feitas para desenvolvimento na web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 93fdc7ca241198dc1d7c4c1f6be0a61b15790039
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a>O que há de novo no ASP.NET 4.5 e o Visual Studio 2012
====================
> Este documento descreve novos recursos e melhorias que estão sendo introduzidas no ASP.NET 4.5. Ele também descreve as melhorias feitas para o desenvolvimento da web no Visual Studio 2012. Este documento foi publicado originalmente em 29 de fevereiro de 2012.


- [Estrutura e o tempo de execução do ASP.NET Core](#_Toc318097372)

    - [Forma assíncrona lendo e gravando solicitações e respostas HTTP](#_Toc318097373)
    - [Melhorias para tratamento de HttpRequest](#_Toc318097374)
    - [Liberação de maneira assíncrona uma resposta](#_Toc318097375)
    - [Suporte para *await* e *tarefa*-manipuladores e módulos assíncrona com base em](#_Toc318097376)
    - [Módulos HTTP assíncronos](#_Toc318097377)
    - [Manipuladores HTTP assíncronos](#_Toc318097378)
    - [Novos recursos de validação de solicitação do ASP.NET](#_Toc318097379)
    - [Adiado validação de solicitação ("lenta")](#_Toc318097380)
    - [Suporte para solicitações invalidadas](#_Toc318097381)
    - [Biblioteca AntiXSS](#_Toc318097382)
    - [Suporte para protocolo WebSockets](#_Toc318097383)
    - [Empacotamento e minimização](#_Toc318097384)
    - [Melhorias de desempenho para hospedagem na Web](#_Toc_perf)

        - [Fatores de desempenho chave](#_Toc_perf_1)
        - [Requisitos para os novos recursos de desempenho](#_Toc_perf_2)
        - [Compartilhando Assemblies comuns](#_Toc_perf_3)
        - [Usando compilação JIT de vários núcleo para agilizar a inicialização](#_Toc_perf_4)
        - [Coleta de lixo para otimizar a memória de ajuste](#_Toc_perf_5)
        - [A pré-busca para aplicativos web](#_Toc_perf_6)
- [Web Forms do ASP.NET](#_Toc318097385)

    - [Controles de dados fortemente tipados](#_Toc318097386)
    - [Associação de modelos](#_Toc318097387)

        - [Selecionando dados](#_Toc318097388)
        - [Provedores de valor](#_Toc318097389)
        - [Filtragem por valores de um controle](#_Toc318097390)
    - [Expressões de associação de dados codificado em HTML](#_Toc318097391)
    - [Validação discreta](#_Toc318097392)
    - [Atualizações do HTML5](#_Toc318097393)
- [O ASP.NET MVC 4](#_Toc318097394)
- [Páginas da Web do ASP.NET 2](#_Toc318097395)
- [Visual Studio 2012 Release Candidate](#_Toc318097396)

    - [Projeto de compartilhamento entre o Visual Studio 2010 e o Visual Studio 2012 Release Candidate (compatibilidade de projeto)](#project-compatibility)
    - [Alterações de configuração no ASP.NET 4.5 site Modelos](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [Suporte nativo no IIS 7 para o roteamento do ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [Editor de HTML](#_Toc318097397)

        - [Tarefas inteligentes](#_Toc318097398)
        - [Suporte de WAI-ARIA](#_Toc318097399)
        - [Novos trechos do HTML5](#_Toc318097400)
        - [Extrair para controle de usuário](#_Toc318097401)
        - [IntelliSense para as partes de código em atributos](#_Toc318097402)
        - [A renomeação automática de marca correspondente ao renomear uma abertura ou marca de fechamento](#_Toc318097403)
        - [Geração de manipulador de eventos](#_Toc318097404)
        - [Recuo inteligente](#_Toc318097405)
        - [Conclusão de instrução de redução automática](#_Toc318097406)
    - [Editor do JavaScript](#_Toc318097407)

        - [Estruturação do código](#_Toc318097408)
        - [Correspondência de chaves](#_Toc318097409)
        - [Ir para Definição](#_Toc318097410)
        - [Suporte da ECMAScript5](#_Toc318097411)
        - [IntelliSense DOM](#_Toc318097412)
        - [Sobrecargas de assinatura VSDOC](#_Toc318097413)
        - [Referências implícita](#_Toc318097414)
    - [Editor de CSS](#_Toc318097415)

        - [Conclusão de instrução de redução automática](#_Toc318097416)
        - [Recuo hierárquico.](#_Toc318097417)
        - [CSS experimenta suporte](#_Toc318097418)
        - [Esquemas específicos do fornecedor (- moz-, - webkit)](#_Toc318097419)
        - [Suporte de comentário e removendo marca de comentário](#_Toc318097420)
        - [Seletor de cores](#_Toc318097421)
        - [Trechos](#_Toc318097422)
        - [Regiões personalizadas](#_Toc318097423)
    - [Inspetor de Página](#_Toc318097424)
    - [Publicação](#_Toc318097425)

        - [Perfis de publicação](#_Toc318097426)
        - [Mesclar e pré-compilação do ASP.NET](#_Toc318097427)
- [IIS Express](#_Toc318097428)
- [Isenção de responsabilidade](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a>Estrutura e o tempo de execução do ASP.NET Core

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a>Forma assíncrona lendo e gravando solicitações e respostas HTTP

O ASP.NET 4 introduziu a capacidade de ler uma entidade de solicitação HTTP como um fluxo usando o *Getbufferlessinputstream* método. Este método fornecido acesso de streaming para a entidade de solicitação. No entanto, ele executadas de forma síncrona, que ocupada um thread para a duração de uma solicitação.

ASP.NET 4.5 suporta a capacidade de ler os fluxos de forma assíncrona em uma entidade de solicitação HTTP e a capacidade de liberação de forma assíncrona. ASP.NET 4.5 também oferece a capacidade de uma entidade de solicitação HTTP, que fornece integração fácil com downstream manipuladores HTTP, como manipuladores de página. aspx e ASP.NET MVC controladores de buffer duplo.

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a>Melhorias para tratamento de HttpRequest

A referência de fluxo retornada pelo ASP.NET 4.5 do *Getbufferlessinputstream* oferece suporte a métodos de leitura síncronos e assíncronos. O *fluxo* objeto retornado de *GetBufferlessInputStream* agora implementa os métodos de BeginRead e EndRead. O assíncrona *fluxo* métodos permitem que você leia assincronamente a entidade de solicitação em partes, ao ASP.NET libera o thread atual entre cada iteração de um loop de leitura assíncrona.

ASP.NET 4.5 também adicionou um método complementar para a entidade de solicitação de leitura em um modo de buffer: *Getbufferedinputstream*. Essa sobrecarga novo funciona como *GetBufferlessInputStream*, dando suporte a Leituras síncronas e assíncronas. No entanto, como ele lê, *GetBufferedInputStream* também copia os bytes de entidade buffers internos do ASP.NET para que manipuladores e módulos downstream ainda podem acessar a entidade de solicitação. Por exemplo, se alguns upstream código no pipeline já leu a entidade de solicitação usando *GetBufferedInputStream*, você ainda pode usar *HttpRequest* ou *HttpRequest.Files*. Isso permite executar processamento assíncrono em uma solicitação (por exemplo, um carregamento de arquivo grande para um banco de dados de streaming), mas as páginas. aspx ainda execução e os controladores do ASP.NET MVC posteriormente.

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a>Liberação de maneira assíncrona uma resposta

Enviar respostas a um cliente HTTP pode demorar algum tempo quando o cliente está longe ou tem uma conexão de baixa largura de banda. Normalmente ASP.NET armazena em buffer os bytes de resposta conforme eles são criados por um aplicativo. ASP.NET, em seguida, executa uma única operação de envio dos buffers acumulados no final do processamento da solicitação.

Se a resposta em buffer for grande (por exemplo, um arquivo grande para um cliente de fluxo contínuo), você deve chamar periodicamente *HttpResponse.Flush* para enviar a saída em buffer para o cliente e controlar o uso de memória sob controle. No entanto, como *liberar* é uma chamada síncrona, iterativamente chamando *liberar* ainda consome um thread para a duração das solicitações potencialmente de longa execução.

ASP.NET 4.5 adiciona suporte para executar liberações de forma assíncrona usando o *BeginFlush* e *EndFlush* métodos do *HttpResponse* classe. Usando esses métodos, você pode criar manipuladores assíncronos que incrementalmente enviar dados para um cliente sem prender os threads do sistema operacional e módulos assíncronos. Entre *BeginFlush* e *EndFlush* chamadas, ASP.NET libera o thread atual. Isso reduz significativamente o número total de threads ativos que são necessários para oferecer suporte a downloads HTTP de longa execução.

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a>Suporte para *await* e *tarefa* -manipuladores e módulos assíncrona com base em

O .NET Framework 4 introduziu um conceito de programação assíncrono conhecido como um *tarefa*. Tarefas são representadas pelo *tarefa* tipo e tipos relacionados no *Tasks* namespace. O .NET Framework 4.5 baseia com aperfeiçoamentos de compilador que tornam o trabalho com *tarefa* simples de objetos. Suportam a duas novas palavras-chave no .NET Framework 4.5, os compiladores: *await* e *async*. O *await* palavra-chave é a abreviação sintática para indicar que um trecho de código deve aguardar assincronamente em alguma outra parte do código. O *async* palavra-chave representa uma dica que você pode usar para marcar métodos como métodos assíncronos baseado em tarefa.

A combinação de *await*, *async*e o *tarefa* objeto torna muito mais fácil para escrever código assíncrono no .NET 4.5. ASP.NET 4.5 oferece suporte a esses simplificações com novas APIs que permitem a você escrever assíncronos módulos HTTP e manipuladores HTTP assíncronos usando os novos aperfeiçoamentos de compilador.

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a>Módulos HTTP assíncronos

Suponha que você deseja executar o trabalho assíncrono dentro de um método que retorna um *tarefa* objeto. O exemplo de código a seguir define um método assíncrono que faz uma chamada assíncrona para baixar a home page da Microsoft. Observe o uso do *async* palavra-chave na assinatura do método e o *await* chamada para *DownloadStringTaskAsync*.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

Isso é tudo o que você precisa escrever — o .NET Framework manipula automaticamente desenrolamento da pilha de chamadas ao aguardar concluir o download, bem como a pilha de chamadas a restauração automaticamente quando o download for concluído.

Agora suponha que você deseja usar este método assíncrono em um módulo HTTP do ASP.NET assíncrono. O ASP.NET 4.5 inclui um método auxiliar (*EventHandlerTaskAsyncHelper*) e um novo tipo de delegado (*TaskEventHandler*) que você pode usar para integrar os métodos assíncronos baseado em tarefa com o antigo modelo de programação assíncrona exposto pelo pipeline HTTP do ASP.NET. Este exemplo mostra como:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a>Manipuladores HTTP assíncronos

A abordagem tradicional para escrever manipuladores assíncronos no ASP.NET é implementar o *IHttpAsyncHandler* interface. ASP.NET 4.5 apresenta o *HttpTaskAsyncHandler* assíncrono tipo base que você pode derivar de, que torna muito mais fácil escrever manipuladores assíncronos.

O *HttpTaskAsyncHandler* tipo é abstrato e exige que você substituir o *ProcessRequestAsync* método. Internamente ASP.NET cuida de integrar-se a assinatura de retorno (um *tarefa* objeto) do *ProcessRequestAsync* com o modelo de programação assíncrona mais antigo usado pelo pipeline do ASP.NET.

O exemplo a seguir mostra como você pode usar *tarefa* e *await* como parte da implementação de um manipulador HTTP assíncrono:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a>Novos recursos de validação de solicitação do ASP.NET

Por padrão, o ASP.NET executa validação de solicitação, ele examina solicitações para procurar por marcação ou script em campos, cabeçalhos, cookies e assim por diante. Se nenhum for detectado, o ASP.NET gera uma exceção. Isso funciona como uma primeira linha de defesa contra ataques de script entre sites.

ASP.NET 4.5 torna fácil de ler seletivamente os dados de solicitação inválida. ASP.NET 4.5 também se integra a Biblioteca AntiXSS popular, que anteriormente era uma biblioteca externa.

Os desenvolvedores têm frequentes para a capacidade de desativar seletivamente a validação de solicitação para seus aplicativos. Por exemplo, se seu aplicativo é um software de fórum, convém permitir que os usuários enviar comentários e postagens em fóruns formatado em HTML, mas ainda, certifique-se de que a validação de solicitação está verificando tudo.

ASP.NET 4.5 apresenta dois recursos que tornam mais fácil para você trabalhar seletivamente com entrada invalidada: adiada validação de solicitação ("lenta") e o acesso aos dados de solicitação inválida.

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a>Adiado validação de solicitação ("lenta")

No ASP.NET 4.5, por padrão todos os dados de solicitação é ser usada sujeito a validação de solicitação. No entanto, você pode configurar o aplicativo para adiar a validação de solicitação até que você realmente acessar dados de solicitação. (Isto é, às vezes, conhecido como validação de solicitação lenta, com base em termos como carregamento lento para determinados cenários de dados.) Você pode configurar o aplicativo para usar a validação adiada no arquivo Web. config, definindo o *requestValidationMode* 4.5 no atributo de *httpRUntime* elemento, como no exemplo a seguir:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

Quando o modo de validação de solicitação é definido como 4.5, validação de solicitação é disparada somente para um valor de solicitação específica e somente quando seu código acessa esse valor. Por exemplo, se seu código obtém o valor de Request.Form["forum\_postagem"], validação de solicitação é invocada apenas para aquele elemento na coleção de formulário. Nenhum dos elementos no *formulário* coleção são validados. Nas versões anteriores do ASP.NET, a validação de solicitação foi disparada para a coleção inteira de solicitação quando qualquer elemento da coleção foi acessado. O novo comportamento torna mais fácil para os componentes de aplicativo diferente examinar os diferentes tipos de dados de solicitação sem disparar a validação de solicitação em outras partes.

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a>Suporte para solicitações invalidadas

Validação de solicitação adiada sozinho não resolve o problema de seletivamente ignorar a validação de solicitação. A chamada para Request.Form["forum\_postagem"] ainda gatilhos de solicitação de validação para esse valor de solicitação específica. No entanto, convém acessar este campo sem disparar validação porque você deseja permitir marcação nesse campo.

Para permitir isso, o ASP.NET 4.5 agora dá suporte ao acesso invalidado para solicitar dados. ASP.NET 4.5 inclui um novo *Unvalidated* na propriedade da coleção de *HttpRequest* classe. Esta coleção fornece acesso a todos os valores comuns de dados de solicitação, como *formulário*, *QueryString*, *Cookies*, e *Url*.

Usando o exemplo de fórum, para ser capaz de ler dados de solicitação inválida, primeiro você precisa configurar o aplicativo para usar o novo modo de validação de solicitação:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

Você pode usar o *HttpRequest.Unvalidated* propriedade ler o valor de formulário invalidados:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> Segurança - *usar dados de solicitação não validados com cuidado!* ASP.NET 4.5 adicionado as propriedades de solicitação invalidadas e coleções para tornar mais fácil para acessar dados de solicitação inválida muito específicas. No entanto, você ainda deve executar a validação personalizada nos dados brutos de solicitação para garantir que o texto perigoso não seja processado para os usuários.


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a>Biblioteca AntiXSS

Devido a popularidade da Microsoft AntiXSS Library, ASP.NET 4.5 agora incorpora as rotinas de codificação de núcleo da versão 4.0 dessa biblioteca.

As rotinas de codificação são implementadas pelo *AntiXssEncoder* tipo no novo *System.Web.Security.AntiXss* namespace. Você pode usar o *AntiXssEncoder* tipo diretamente ao chamar qualquer um dos métodos de codificação estáticos que são implementados no tipo. No entanto, a abordagem mais simples para usar as novo rotinas de anti-XSS é configurar um aplicativo ASP.NET para usar o *AntiXssEncoder* classe por padrão. Para fazer isso, adicione o seguinte atributo para o arquivo Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

Quando o *encoderType* atributo está definido para usar o *AntiXssEncoder* tipo de saída automaticamente a codificação no ASP.NET usa as rotinas de codificação de novo.

Estas são as partes da Biblioteca AntiXSS externa que foram incorporadas ao ASP.NET 4.5:

- *HtmlEncode*, *HtmlFormUrlEncode*, e *HtmlAttributeEncode*
- *XmlAttributeEncode* e *XmlEncode*
- *UrlEncode* e *UrlPathEncode* (novo)
- *CssEncode*

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a>Suporte para protocolo WebSockets

Protocolo WebSocket é um protocolo de rede baseado em padrões que define como estabelecer uma comunicação bidirecional segura e em tempo real entre um cliente e um servidor via HTTP. Microsoft trabalhou com corpos de padrões a IETF e do W3C para ajudar a definir o protocolo. O protocolo de WebSockets é suportado por qualquer cliente (não apenas navegadores), com a Microsoft investir recursos substanciais que suporte o protocolo WebSocket no cliente e sistemas operacionais móveis.

Protocolo WebSockets torna muito mais fácil criar transferências de dados de longa duração entre um cliente e um servidor. Por exemplo, escrever um aplicativo de chat é muito mais fácil porque você pode estabelecer uma conexão de longa execução true entre um cliente e um servidor. Você não precisa recorrer a soluções alternativas, como a sondagem periódica ou sondagem longa HTTP para simular o comportamento de um soquete.

ASP.NET 4.5 e IIS 8 incluem suporte a WebSockets baixo nível, permitindo que os desenvolvedores do ASP.NET usar APIs gerenciadas de forma assíncrona lendo e gravando dados binários e cadeia de caracteres em um objeto WebSocket. Para o ASP.NET 4.5, há uma nova *System.Web.WebSockets* namespace que contém tipos para trabalhar com o protocolo WebSocket.

Um cliente navegador estabelece uma conexão WebSocket criando um DOM *WebSocket* objeto que aponta para uma URL em um aplicativo ASP.NET, como no exemplo a seguir:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

Você pode criar pontos de extremidade do WebSocket ASP.NET usando qualquer tipo de módulo ou manipulador. No exemplo anterior, um arquivo. ashx foi usado, como arquivos. ashx são uma maneira rápida de criar um manipulador.

Acordo com o protocolo WebSocket, um aplicativo ASP.NET aceita a solicitação de WebSocket do cliente, indicando que a solicitação deve ser atualizada de uma solicitação HTTP GET para uma solicitação WebSocket. Veja um exemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

O *AcceptWebSocketRequest* método aceita um delegado de função porque ASP.NET esvazia a solicitação HTTP atual e, em seguida, transfere o controle para o delegado de função. Conceitualmente, essa abordagem é semelhante à maneira como você usa *System.Threading.Thread*, em que você define um delegado de início de thread em segundo plano o trabalho é executado.

Depois que o ASP.NET e o cliente concluiu com êxito um handshake WebSocket, ASP.NET chama o representante e o aplicativo de WebSockets começa a ser executado. O exemplo de código a seguir mostra um aplicativo de eco simples que usa o suporte interno de WebSockets no ASP.NET:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

O suporte do .NET 4.5 para o *await* palavra-chave e operações assíncronas baseado em tarefa é uma opção natural para escrever aplicativos de WebSocket. O exemplo de código mostra que uma solicitação WebSocket seja executado completamente assíncrona dentro de ASP.NET. O aplicativo aguarda assincronamente uma mensagem a ser enviada de um cliente chamando *await soquete. ReceiveAsync*. Da mesma forma, você pode enviar uma mensagem assíncrona para um cliente chamando *await soquete. SendAsync*.

No navegador, um aplicativo recebe mensagens de WebSockets por meio de um *onmessage* função. Para enviar uma mensagem em um navegador, você deve chamar o *enviar* método o *WebSocket* tipo, DOM, como mostrado neste exemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

No futuro, podemos pode lançar atualizações para esta funcionalidade que abstrata imediatamente algumas a codificação baixo nível que é necessária nesta versão WebSocket aplicativos.

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a>Empacotamento e minimização

Agrupando permite combinar arquivos JavaScript e CSS individuais em um pacote que pode ser tratado como um único arquivo. Minimização Condensa os arquivos JavaScript e CSS, removendo o espaço em branco e outros caracteres que não são necessários. Esses recursos trabalhar com formulários da Web, ASP.NET MVC e páginas da Web.

Pacotes são criados usando a classe de pacote ou uma de suas classes filho, ScriptBundle e StyleBundle. Depois de configurar uma instância de um pacote, o pacote será disponibilizado para solicitações de entrada, simplesmente adicionando-a uma instância no BundleCollection global. Nos modelos padrão, a configuração de pacote é executada em um arquivo BundleConfig. Essa configuração padrão cria pacotes para todos os principais scripts e arquivos de css usados pelos modelos.

Pacotes são mencionados dentro de modos de exibição usando um dos dois métodos de auxiliares possíveis. Para oferecer suporte ao processamento de marcação diferente para um pacote na depuração vs. modo de liberação, as classes ScriptBundle e StyleBundle têm o método auxiliar, a processar. Quando estiver no modo de depuração, renderização irá gerar marcação de cada recurso no pacote. Quando no modo de versão, a renderização gerará um elemento de marcação única para o pacote inteiro. Alternando entre debug e release modo pode ser feito modificando o atributo debug do elemento compilation no Web. config, conforme mostrado abaixo:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

Além disso, habilitar ou desabilitar a otimização pode ser definido diretamente por meio da propriedade de BundleTable.EnableOptimizations.

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

Quando arquivos são incluídos, eles primeiro são classificados em ordem alfabética (o modo de exibição no **Solution Explorer**). Que estão organizadas, em seguida, para que o conhecido bibliotecas e suas extensões personalizadas (como jQuery, MooTools e Dojo) são carregados pela primeira vez. Por exemplo, a ordem final para o agrupamento da pasta de Scripts, como mostrado acima será:

1. 1.6.2.js jQuery
2. ui.js jQuery
3. jQuery.Tools.js
4. a.js

Arquivos CSS são também são classificados em ordem alfabética e, em seguida, reorganizados para que Reset e normalize.css vir antes de qualquer outro arquivo. A classificação final de empacotamento da pasta estilos mostrada acima será:

1. Reset
2. Content.CSS
3. Forms.CSS
4. Globals.CSS
5. menu.CSS
6. Styles

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a>Melhorias de desempenho para hospedagem na Web

O .NET Framework 4.5 e Windows 8 apresentam recursos que podem ajudá-lo a atingir um aumento significativo no desempenho para cargas de trabalho do servidor web. Isso inclui uma redução (até 35%) em ambos os tempo de inicialização e no espaço de memória de sites que usam o ASP.NET de hospedagem na web.

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a>Fatores de desempenho chave

Idealmente, todos os sites devem estar ativos e na memória para garantir uma resposta rápida para a próxima solicitação, sempre que se trata. Fatores que podem afetar a capacidade de resposta do site incluem:

- O tempo necessário para um site para reiniciar depois de um pool de aplicativos é reciclado. Este é o tempo necessário para iniciar um processo do servidor web para o site quando os assemblies do site não estão mais na memória. (Os assemblies de plataforma ainda estão na memória, desde que eles são usados por outros sites.) Essa situação é conhecida como "site frio, inicialização do framework passiva" ou simplesmente "frio site inicialização".
- A quantidade de memória em que o site ocupa. Termos para isso são "consumo de memória por site" ou "compartilhado conjunto de trabalho".

As novas melhorias de desempenho se concentrar em ambos esses fatores.

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a>Requisitos para os novos recursos de desempenho

Os requisitos para os novos recursos podem ser divididos nestas categorias:

- Aprimoramentos que são executados no .NET Framework 4.
- Melhorias que exigem o .NET Framework 4.5, mas podem ser executado em qualquer versão do Windows.
- Melhorias que estão disponíveis apenas com o .NET Framework 4.5 em execução no Windows 8.

O desempenho aumenta com cada nível de melhoria que você possa habilitar.

Algumas das melhorias do .NET Framework 4.5 tirar proveito dos recursos de desempenho mais amplos que se aplicam a outros cenários.

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a>Compartilhando Assemblies comuns

**Requisito**: .NET Framework 4 e Visual Studio 11 Developer Preview SDK

Diferentes sites em um servidor geralmente usam os mesmo assemblies auxiliar (por exemplo, os assemblies de um aplicativo de starter kit ou exemplo). Cada site tem sua própria cópia desses assemblies no diretório Bin. Mesmo que o código de objeto para os assemblies é idêntico, eles são assemblies fisicamente separados, para que cada assembly deve ser lido separadamente durante a inicialização do site frios e mantidas separadamente na memória.

A nova funcionalidade definido como interno resolve esse ineficiência e reduz os requisitos de RAM e carga de tempo. Como internos permite Windows manter uma cópia única de cada assembly no sistema de arquivos e assemblies individuais nas pastas de compartimento do site são substituídos com links simbólicos à cópia única. Se um site individual precisa de uma versão distinta do assembly, o link simbólico é substituído pela nova versão do assembly, e somente esse site é afetado.

Compartilhando assemblies usando links simbólicos requer uma nova ferramenta chamada aspnet\_intern.exe, que lhe permite criar e gerenciar o armazenamento de assemblies internos. Ele é fornecido como parte do Visual Studio 11 Developer Preview SDK. (No entanto, ela funcionará em um sistema que tem apenas o .NET Framework 4 instalado, supondo que você instalou a versão mais recente [atualizar](https://support.microsoft.com/kb/2468871).)

Para certificar-se de que todos os assemblies qualificados tem sido definidos como internos, você executa aspnet\_intern.exe periodicamente (por exemplo, uma vez por semana como uma tarefa agendada). Um uso típico é da seguinte maneira:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

Para ver todas as opções, execute a ferramenta sem argumentos.

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a>Usando compilação JIT de vários núcleo para agilizar a inicialização

**Requisito**: .NET Framework 4.5

Para um novo site estático, não apenas assemblies precisa ser lidas do disco, mas o site deve ser compilado por JIT. Para um site complexo, isso pode adicionar atrasos significativos. Uma técnica de finalidade geral nova no .NET Framework 4.5 reduz esses atrasos distribuindo compilação JIT entre núcleos de processador disponíveis. Ele faz isso tanta e mais cedo possível usando as informações coletadas durante anterior inicia do site. Essa funcionalidade implementada pelo [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/en-us/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) método.

Usando vários núcleos com compilação JIT é habilitado por padrão no ASP.NET, portanto você não precisa fazer nada para tirar proveito desse recurso. Se você quiser desabilitar esse recurso, verifique a configuração a seguir no arquivo Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a>Coleta de lixo para otimizar a memória de ajuste

**Requisito**: .NET Framework 4.5

Quando um site estiver em execução, o uso da pilha de coletor de lixo (GC) pode ser um fator significativo de seu consumo de memória. Como qualquer coletor de lixo, o .NET Framework GC torna compensações entre o tempo de CPU (frequência e o significado de coleções) e o consumo de memória (espaço extra que é usado para objetos novos, livre ou capaz de livre). Para versões anteriores, fornecemos orientação sobre como configurar o GC para alcançar o equilíbrio correto (por exemplo, consulte [ASP.NET 2.0/3.5 configuração compartilhada de hospedagem](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).

Para o .NET Framework 4.5, em vez de várias configurações de autônomo, uma definição de configuração definido por carga de trabalho está disponível que permite que todos a configurações de GC anteriormente recomendadas, bem como nova otimização que proporciona desempenho adicional para o por site conjunto de trabalho.

Para habilitar o ajuste de memória de GC, adicione a seguinte configuração para o arquivo Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

(Se você estiver familiarizado com as orientações anteriores para que as alterações para ASPNET, observe que essa configuração substitui as configurações antigas — por exemplo, não é necessário definir gcServer, gcConcurrent, etc. Não é necessário remover as configurações antigas.)

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a>A pré-busca para aplicativos web

**Requisito**: .NET Framework 4.5 em execução no Windows 8

Para lançamentos, o Windows inclui uma tecnologia conhecida como o [pré-busca](http://en.wikipedia.org/wiki/Prefetcher) que reduz o custo de leitura de disco de inicialização do aplicativo. Como a inicialização a frio é um problema predominantemente para aplicativos cliente, esta tecnologia não foi incluída no Windows Server, que inclui somente os componentes que são essenciais para um servidor. A pré-busca agora está disponível na versão mais recente do Windows Server, onde ele pode otimizar a inicialização de sites individuais.

Para o Windows Server, a pré-busca não está habilitada por padrão. Para habilitar e configurar a pré-busca para hospedagem na web de alta densidade, execute o seguinte conjunto de comandos na linha de comando:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

Em seguida, para integrar a pré-busca com aplicativos ASP.NET, adicione o seguinte ao arquivo Web. config:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a>Web Forms do ASP.NET

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a>Controles de dados fortemente tipados

No ASP.NET 4.5, formulários da Web inclui algumas melhorias para trabalhar com dados. A primeira melhoria é controles de dados fortemente tipados. Para controles de formulários da Web em versões anteriores do ASP.NET, você exibir um valor de associação de dados usando *Eval* e uma expressão de associação de dados:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

Para associação de dados bidirecional, você deve usar *associar*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

Em tempo de execução, essas chamadas usar reflexão para ler o valor do membro especificado e, em seguida, exibir o resultado na marcação. Essa abordagem facilita a associação de dados em relação aos dados arbitrários, unshaped.

No entanto, as expressões de associação de dados assim não oferecem suporte a recursos como IntelliSense para nomes de membros, navegação (como Go To Definition) ou a verificação para esses nomes de tempo de compilação.

Para resolver esse problema, o ASP.NET 4.5 adiciona a capacidade de declarar o tipo de dados de um controle associado a dados. Fazer isso usando o novo *ItemType* propriedade. Quando você define essa propriedade, duas novas variáveis de tipo estão disponíveis no escopo das expressões de associação de dados: *Item* e *BindItem*. Como as variáveis são fortemente tipadas, você obtém todos os benefícios da experiência de desenvolvimento do Visual Studio.


Para expressões de associação de dados bidirecionais, use o *BindItem* variável:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


A maioria dos controles do Framework do Web Forms do ASP.NET que oferecem suporte à associação de dados foram atualizados para dar suporte a *ItemType* propriedade.

<a id="_Toc318097387"></a>
### <a name="model-binding"></a>Associação de modelo

Associação de modelo estende a associação de dados em controles de formulários da Web do ASP.NET para trabalhar com acesso a dados focados no código. Ele inclui conceitos do *ObjectDataSource* controle e de associação de modelo no ASP.NET MVC.

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a>Selecionando dados

Para configurar um controle de dados para usar a associação de modelo para selecionar os dados, você define o controle *SelectMethod* propriedade para o nome de um método no código da página. O controle de dados chama o método no momento apropriado no ciclo de vida da página e vincula automaticamente os dados retornados. Não é necessário chamar explicitamente o *DataBind* método.

No exemplo a seguir, o *GridView* controle está configurado para usar um método chamado *GetCategories*:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

Você cria o *GetCategories* método no código da página. Para uma operação de seleção simple, o método não precisa de parâmetros e deve retornar um *IEnumerable* ou *IQueryable* objeto. Se o novo *ItemType* está definida (que permite fortemente tipado expressões de associação de dados, conforme explicado em [fortemente tipado dados controles](#_Toc318097386) anteriormente), as versões genéricas dessas interfaces deve ser retornado — *IEnumerable&lt;T&gt;*  ou *IQueryable&lt;T&gt;*, com o *T* parâmetro correspondente ao tipo do *ItemType* propriedade (por exemplo, *IQueryable&lt;categoria&gt;*).

O exemplo a seguir mostra o código para um *GetCategories* método. Este exemplo usa o modelo do Entity Framework Code First com o banco de dados de exemplo Northwind. O código torna-se de que a consulta retorna os detalhes de produtos relacionados para cada categoria por meio do *incluir* método. (Isso assegura que o *TemplateField* elemento na marcação exibe a contagem de produtos em cada categoria sem a necessidade de um [n + 1 Selecione](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

Quando a página é executada, o *GridView* controlar chamadas de *GetCategories* método automaticamente e processa os dados retornados usando os campos configurados:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

Como o método select retorna um *IQueryable* objeto, o *GridView* controle pode manipular mais a consulta antes da execução. Por exemplo, o *GridView* controle pode adicionar expressões de consulta para classificação e paginação retornado *IQueryable* objeto antes de ser executado, para que essas operações são executadas por subjacente Provedor LINQ. Nesse caso, Entity Framework irá garantir que essas operações são executadas no banco de dados.

A exemplo a seguir mostra o *GridView* controle modificado para permitir a classificação e paginação:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

Agora quando a página é executada, o controle pode garantir que apenas a página atual de dados é exibida e que ele é ordenado pela coluna selecionada:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

Para filtrar os dados retornados, os parâmetros devem ser adicionados ao método select. Esses parâmetros serão preenchidos pela associação de modelo em tempo de execução, e você pode usá-los para alterar a consulta antes de retornar os dados.

Por exemplo, suponha que você deseja permitir que usuários filtro produtos inserindo uma palavra-chave na cadeia de caracteres de consulta. Você pode adicionar um parâmetro para o método e atualizar o código para usar o valor do parâmetro:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

Esse código inclui um *onde* expressão se for fornecido um valor para *palavra-chave* e, em seguida, retorna os resultados da consulta.

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a>Provedores de valor

O exemplo anterior não era específico sobre onde o valor para o *palavra-chave* parâmetro foi provenientes. Para indicar essas informações, você pode usar um atributo de parâmetro. Neste exemplo, você pode usar o *QueryStringAttribute* classe que está no *System.Web.ModelBinding* namespace:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

Isso instrui a associação de modelo para tentar associar um valor de cadeia de caracteres de consulta para o *palavra-chave* parâmetro em tempo de execução. (Isso pode envolver executar conversão de tipo, embora ele não nesse caso.) Se um valor não pode ser fornecido e o tipo é não anulável, uma exceção será lançada.

As fontes de valores para esses métodos são chamadas de provedores de valor e os atributos de parâmetro que indicam qual provedor de valor para usar são referidos como atributos de provedor de valor. Formulários da Web incluem provedores de valor e atributos correspondentes para todas as fontes típicas de entrada do usuário em um aplicativo de Web Forms, como a cadeia de caracteres de consulta, cookies, valores de formulário, controles, estado de exibição, estado de sessão e propriedades de perfil. Você também pode escrever provedores de valor personalizado.

Por padrão, o nome do parâmetro é usado como a chave para localizar um valor na coleção de provedor de valor. No exemplo, o código irá procurar um valor de cadeia de caracteres de consulta chamado palavra-chave (por exemplo, ~ / default.aspx?keyword=chef). Você pode especificar uma chave personalizada, passando-o como um argumento para o atributo de parâmetro. Por exemplo, para usar o valor da variável de cadeia de caracteres de consulta chamado p, você pode fazer isso:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

Se esse método é no código da página, os usuários podem filtrar os resultados, passando uma palavra-chave usando a cadeia de caracteres de consulta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

Associação de modelo realiza várias tarefas que você teria de código à mão: ler o valor, procurando um valor nulo, tentar convertê-lo para o tipo apropriado, verificando se a conversão foi bem-sucedida e por fim, usando o valor de consulta. Modelo de associação de resultados em muito menos código e a capacidade de reutilizar a funcionalidade em seu aplicativo.

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a>Filtragem por valores de um controle

Suponha que você deseja estender o exemplo para permitir que o usuário escolha um valor de filtro de uma lista suspensa. Adicione a seguinte lista suspensa para a marcação e configurá-lo para obter os dados de outro método usando o *SelectMethod* propriedade:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

Normalmente você também adicionará uma *EmptyDataTemplate* elemento para o *GridView* controlar de forma que o controle exibirá uma mensagem se nenhum produto correspondente forem encontrado:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

Na página de código, adicione que o novo método para obter a lista suspensa de seleção:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

Por fim, atualize o *GetProducts* selecione o método para receber um novo parâmetro que contém a ID da categoria selecionada na lista suspensa:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

Agora, quando a página é executada, os usuários podem selecionar uma categoria na lista suspensa e o *GridView* controle é novamente automaticamente acoplado para mostrar os dados filtrados. Isso é possível porque a associação de modelo controla os valores de parâmetros para os métodos de seleção e detecta se o valor do parâmetro foi alterado após um postback. Nesse caso, a associação de modelo força o controle de dados associados para associar novamente os dados.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a>Expressões de associação de dados codificado em HTML

Você pode agora codificação HTML o resultado das expressões de associação de dados. Adicionar um dois-pontos (:) ao final do &lt;% # prefixo de marca a expressão de associação de dados:

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a>Validação discreta

Agora você pode configurar os controles de validação interna para usar JavaScript discreto para lógica de validação do lado do cliente. Significativamente, isso reduz a quantidade de JavaScript processado embutido na marcação da página e reduz o tamanho da página geral. Você pode configurar o JavaScript discreto para controles de validação em qualquer uma das seguintes maneiras:

- Globalmente, adicionando a seguinte configuração para o  *&lt;appSettings&gt;*  elemento no arquivo Web. config: 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- Definindo globalmente estático *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* propriedade *UnobtrusiveValidationMode.WebForms* (normalmente no *aplicativo \_Iniciar* método no arquivo global. asax).
- Individualmente para uma página definindo a nova *UnobtrusiveValidationMode* propriedade o *página* de classe para *UnobtrusiveValidationMode.WebForms*.

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a>Atualizações do HTML5

Algumas melhorias foram feitas para Web Forms controles de servidor para tirar proveito dos novos recursos do HTML5:

- O *TextMode* propriedade o *TextBox* controle foi atualizado para dar suporte os novos tipos de entrada do HTML5 como *email*, *datetime*, e assim por diante.
- O *FileUpload* controlar agora dá suporte a vários arquivos de carregamentos de navegadores que oferecem suporte a esse recurso do HTML5.
- Controles de validação agora suporte validação HTML5 elementos de entrada.
- Novos elementos de HTML5 que têm atributos que representam uma URL agora oferecem suporte a runat = "server". Como resultado, você pode usar as convenções do ASP.NET em caminhos de URL, como o ~ operador para representar a raiz do aplicativo (por exemplo, &lt;vídeo runat = "server" src="~/myVideo.wmv" /&gt;).
- O *UpdatePanel* controle foi corrigido para dar suporte a campos de entrada de lançamento HTML5.

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a>ASP.NET MVC 4

ASP.NET MVC 4 Beta agora está incluído no Visual Studio 11 Beta. ASP.NET MVC é uma estrutura para o desenvolvimento de aplicativos Web altamente testáveis e sustentáveis aproveitando o padrão Model-View-Controller (MVC). ASP.NET MVC 4 facilita a criação de aplicativos para a Web móvel e inclui a API da Web ASP.NET, o que ajuda a criar serviços HTTP que podem acessar qualquer dispositivo. Para obter mais informações, consulte o [notas de versão do ASP.NET MVC 4](mvc4-release-notes.md).

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a>Páginas da Web do ASP.NET 2

Novos recursos incluem o seguinte:

- Modelos de sites novos e atualizados.
- Inclusão do lado do servidor e a validação do lado do cliente usando o *validação* auxiliar.
- A capacidade de registrar scripts usando um Gerenciador de ativos.
- Habilitar os logons do Facebook e outros sites usando OpenID e OAuth.
- Adicionar mapas usando o *mapeia* auxiliar.
- Executando aplicativos de páginas da Web lado a lado.
- Páginas de renderização para dispositivos móveis.

Para obter mais informações sobre esses recursos e exemplos de código de página inteira, consulte [a recursos da parte superior na versão Beta 2 de páginas da Web](https://go.microsoft.com/fwlink/?LinkID=227824).

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a>Visual Web Developer 11 Beta

Esta seção fornece informações sobre aprimoramentos de desenvolvimento da web no Visual Web Developer 11 Beta e o Visual Studio 2012 Release Candidate.

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a>Projeto de compartilhamento entre o Visual Studio 2010 e o Visual Studio 2012 Release Candidate (compatibilidade de projeto)

Até que o Visual Studio 2012 Release Candidate, abrir um projeto existente em uma versão mais recente do Visual Studio iniciado o Assistente de conversão. Isso forçado uma atualização do conteúdo (ativos) de uma solução e projeto para novos formatos que não eram compatíveis com versões anteriores. Portanto, após a conversão é não foi possível abrir o projeto na versão mais antiga do Visual Studio.

Muitos clientes nos disseram que não era a abordagem certa. No Visual Studio 11 Beta, vamos agora oferecem suporte compartilhamento projetos e soluções com o Visual Studio 2010 SP1. Isso significa que se você abrir um projeto de 2010 no Visual Studio 2012 Release Candidate, você ainda poderá abrir o projeto no Visual Studio 2010 SP1.

> [!NOTE]
> Alguns tipos de projetos não podem ser compartilhados entre o Visual Studio 2010 SP1 e o Visual Studio 2012 Release Candidate. Isso inclui alguns antigos projetos (como projetos do ASP.NET MVC 2) ou para fins especiais (como projetos de instalação).

Quando você abre um projeto do Visual Studio 2010 SP1 Web pela primeira vez no Visual Studio 11 Beta, as propriedades a seguir são adicionadas ao arquivo de projeto:

- FileUpgradeFlags
- UpgradeBackupLocation
- OldToolsVersion
- VisualStudioVersion
- VSToolsPath

FileUpgradeFlags, UpgradeBackupLocation e OldToolsVersion são usados pelo processo que atualiza o arquivo de projeto. Eles não têm impacto sobre como trabalhar com o projeto no Visual Studio 2010.

VisualStudioVersion é uma nova propriedade usada pelo MSBuild 4.5 que indica a versão do Visual Studio para o projeto atual. Como essa propriedade não existe no MSBuild 4.0 (a versão do MSBuild usa o Visual Studio 2010 SP1), inserimos um valor padrão para o arquivo de projeto.

A propriedade VSToolsPath é usada para determinar o arquivo. targets correto para importar do caminho representado pela configuração MSBuildExtensionsPath32.

Também há algumas alterações relacionadas a elementos de importação. Essas alterações são necessárias para oferecer suporte a compatibilidade entre as duas versões do Visual Studio.

> [!NOTE]
> Se um projeto é compartilhado entre o Visual Studio 2010 SP1 e o Visual Studio 11 Beta em dois computadores diferentes, e se o projeto inclui um banco de dados local no aplicativo\_pasta de dados, você deve garantir que a versão do SQL Server usado pelo banco de dados é instalada em ambos os computadores.

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a>Alterações de configuração no ASP.NET 4.5 site Modelos

As seguintes alterações foram feitas para o padrão *Web. config* arquivo para o site que são criadas usando modelos de sites no Visual Studio 2012 Release Candidate:

- No `<httpRuntime>` elemento, o `encoderType` atributo agora está definido por padrão para usar os tipos de AntiXSS que foram adicionados ao ASP.NET. Para obter detalhes, consulte [Biblioteca AntiXSS](#_Toc318097382).
- Também no `<httpRuntime>` elemento, o `requestValidationMode` atributo é definido como "4.5". Isso significa que, por padrão, a validação de solicitação está configurada para usar adiada validação ("lenta"). Para obter detalhes, consulte [recursos de validação de solicitação de ASP.NET novo](#_Toc318097379).
- O `<modules>` elemento o `<system.webServer>` seção não contém um `runAllManagedModulesForAllRequests` atributo. (O valor padrão é falso). Isso significa que, se você estiver usando uma versão do IIS 7 não foi atualizado para SP1, você pode ter problemas com o roteamento de um novo site. Para obter mais informações, consulte [suporte nativo no IIS 7 para roteamento ASP.NET](#Native_Support_In_IIS7_For_ASPNET_Routine).

Essas alterações não afetam os aplicativos existentes. No entanto, eles podem representar uma diferença de comportamento entre sites da Web existentes e novos que você criar para ASP.NET 4.5 usando os novos modelos.

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a>Suporte nativo no IIS 7 para o roteamento do ASP.NET

Não é uma alteração para o ASP.NET como tal, mas uma alteração nos modelos para novos projetos de site que podem afetar a se você estiver trabalhando uma versão do IIS 7 que não tenha a atualização do SP1 aplicada.

No ASP.NET, você pode adicionar a seguinte configuração de aplicativos para oferecer suporte ao roteamento:

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

Quando **runAllManagedModulesForAllRequests** for true, uma URL como `http://mysite/myapp/home` vai para o ASP.NET, mesmo que haja nenhum *. aspx*, *. MVC*, ou extensão semelhante do URL.

Faz com que uma atualização que foi feita para o IIS 7 a **runAllManagedModulesForAllRequests** configuração desnecessários e dá suporte a roteamento de modo nativo do ASP.NET. (Para obter informações sobre a atualização, consulte o artigo Microsoft Support [uma atualização está disponível que permite que determinados manipuladores do IIS 7.0 ou IIS 7.5 para tratar as solicitações cujas URLs não terminam com um período](https://support.microsoft.com/kb/980368).)

Se seu site está em execução no IIS 7 e se o IIS tiver sido atualizado, você não precisará definir **runAllManagedModulesForAllRequests** como true. Na verdade, não é recomendável o configurá-lo como true, porque ela adiciona sobrecarga de processamento desnecessária a solicitação. Quando essa configuração for true, todas as solicitações, incluindo aqueles para *. htm*, *. jpg*, e outros arquivos estáticos, também passar pelo pipeline de solicitação do ASP.NET.

Se você criar um novo site ASP.NET 4.5 usando os modelos que são fornecidos no Visual Studio 2012 RC, a configuração do site não inclui o **runAllManagedModulesForAllRequests** configuração. Isso significa que, por padrão a configuração é false.

Se você executar o site no Windows 7 sem o SP1 instalado, o IIS 7 não incluirá a atualização necessária. Como consequência, o roteamento não funcionará e você verá erros. Se você tiver um problema em que o roteamento não funcionar, você pode fazer o seguinte:

- Atualize o Windows 7 para o SP1, o que adicionará a atualização para o IIS 7.
- Instale a atualização descrita no artigo do Microsoft Support listado anteriormente.
- Definir **runAllManagedModulesForAllRequests** como true no arquivo de Web. config do site. Observe que isso adicionará alguma sobrecarga de solicitações.

<a id="_Toc318097397"></a>
### <a name="html-editor"></a>Editor de HTML

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a>Tarefas inteligentes

No modo Design, propriedades complexas de controles de servidor geralmente associou caixas de diálogo e assistentes para tornar mais fácil para defini-las. Por exemplo, você pode usar uma caixa de diálogo especial para adicionar uma fonte de dados para um *repetidor* controlar ou adicionar colunas a uma *GridView* controle.

No entanto, esse tipo de ajuda de interface do usuário para propriedades complexas não está disponível na exibição da fonte. Portanto, o Visual Studio 11 apresenta tarefas inteligentes para exibição da fonte. Tarefas inteligentes são atalhos com reconhecimento de contexto para recursos normalmente usados nos editores de c# e Visual Basic.

Para controles de formulários da Web ASP.NET, tarefas inteligentes aparecem nas marcas de servidor como um glifo de pequeno quando o ponto de inserção está dentro do elemento:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

A tarefa inteligente se expande quando você clica no glifo ou pressione CTRL +. (ponto), assim como os editores de códigos. Ele exibe, em seguida, os atalhos que são semelhantes às tarefas inteligentes na exibição Design.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

Por exemplo, a tarefa inteligente na ilustração anterior mostra as opções de tarefas GridView. Se você escolher Editar colunas, a caixa de diálogo a seguir será exibida:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

Preencher a caixa de diálogo define as mesmas propriedades podem ser definidas na exibição Design. Quando você clica em Okey, a marcação para o controle é atualizada com as novas configurações:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a>Suporte de WAI-ARIA

Gravar sites acessível está se tornando cada vez mais importante. O [padrão de acessibilidade WAI-ARIA](http://www.w3.org/WAI/intro/aria) define como os desenvolvedores devem escrever sites acessível. Esse padrão agora tem suporte total no Visual Studio.

Por exemplo, o *função* atributo agora tem IntelliSense completo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

O padrão de WAI-ARIA também apresenta atributos que são prefixados com *aria -* que permitem que você adicione semântica para um documento do HTML5. Visual Studio oferece suporte totalmente essas *aria -* atributos:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a>Novos trechos do HTML5

Para torná-lo mais rápido e mais fácil de escrever a marcação de HTML5 usada com frequência, o Visual Studio inclui um número de trechos de código. Um exemplo é o trecho de vídeo:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

Para invocar o trecho de código, pressione Tab duas vezes quando o elemento é selecionado no IntelliSense:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

Isso produz um trecho de código que você pode personalizar.

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a>Extrair para controle de usuário

Em páginas da web grande, ele pode ser uma boa ideia para mover as partes individuais em controles de usuário. Essa forma de refatoração pode ajudar a aumentar a legibilidade da página e pode simplificar a estrutura da página.

Para facilitar isso, quando você editar páginas Web Forms em modo de exibição de fonte, você pode agora selecionar texto em uma página, clique duas vezes e clique em Extrair para controle de usuário:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a>IntelliSense para as partes de código em atributos

O Visual Studio sempre forneceu IntelliSense para partes do código do lado do servidor em qualquer página ou controle. Visual Studio agora inclui o IntelliSense para partes de código em atributos HTML também.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

Isso torna mais fácil criar expressões de associação de dados:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a>A renomeação automática de marca correspondente ao renomear uma abertura ou marca de fechamento

Se você renomear um elemento HTML (por exemplo, você alterar uma *div* marca para ser um *cabeçalho* marca), a abertura ou marca de fechamento correspondente também será alterado em tempo real.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

Isso ajuda a evitar o erro em que você se esquecer de alterar uma marca de fechamento ou alterar a errado.

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a>Geração de manipulador de eventos

O Visual Studio agora inclui recursos no modo de exibição de fonte para ajudá-lo a escrever manipuladores de eventos e associá-las manualmente. Se você estiver editando um nome de evento no modo de exibição de fonte, o IntelliSense exibe &lt;criar um novo evento&gt;, que criará um manipulador de eventos no código da página que tem a assinatura correta:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

Por padrão, o manipulador de eventos usará a identificação do controle para o nome do método de manipulação de eventos:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

O manipulador de eventos resultante será assim (nesse caso, em c#):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a>Recuo inteligente

Quando você pressiona Enter enquanto dentro de um elemento HTML vazio, o editor colocará o ponto de inserção no local correto:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

Se você pressionar Enter nesse local, a marca de fechamento é movida para baixo e recuada para coincidir com a marca de abertura. Também é recuada com o ponto de inserção:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a>Conclusão de instrução de redução automática

A lista do IntelliSense no Visual Studio agora filtros com base no que você digitar para que ela exibe opções somente relevantes:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

IntelliSense também filtros com base no caso de título das palavras individuais na lista do IntelliSense. Por exemplo, se você digitar "dl", dl e asp: DataList são exibidas:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

Este recurso torna mais rápido para obter a conclusão de instrução para elementos conhecidos.

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a>Editor JavaScript

O editor do JavaScript no Visual Studio 2012 Release Candidate é completamente novo e ele melhora significativamente a experiência de trabalhar com JavaScript no Visual Studio.

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a>Estrutura de tópicos de código

Regiões de estrutura de tópicos agora são criados automaticamente para todas as funções, permitindo que você recolher partes que não são relevantes para o foco atual do arquivo.

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a>Correspondência de chaves

Quando você coloca o ponto de inserção em uma abertura ou fechamento, o editor realça uma correspondência.

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a>Ir para Definição

Ir para o comando de definição permite pular para a fonte para uma função ou variável.

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a>Suporte da ECMAScript5

O editor oferece suporte a nova sintaxe e APIs no ECMAScript5, a versão mais recente do padrão que descreve a linguagem JavaScript.

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a>IntelliSense DOM

IntelliSense para APIs de DOM foi aprimorado, com suporte para várias APIs de HTML5, novo incluindo *querySelector*, armazenamento de DOM, mensagens entre documentos e *tela*. DOM IntelliSense agora é orientado por um único arquivo simple de JavaScript, em vez de uma definição de biblioteca de tipo nativo. Isso facilita estender ou substituir.

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a>Sobrecargas de assinatura VSDOC

Comentários detalhados do IntelliSense agora podem ser declarados para separado sobrecargas de funções JavaScript usando o novo  *&lt;assinatura&gt;*  elemento, conforme mostrado neste exemplo:

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a>Referências implícita

Agora você pode adicionar arquivos JavaScript a uma lista central que será implicitamente incluída na lista de arquivos que qualquer determinado arquivo ou bloco Referências JavaScript, que significa que você obterá o IntelliSense para seu conteúdo. Por exemplo, você pode adicionar arquivos jQuery à lista de arquivos central, e você obterá o IntelliSense para funções jQuery em qualquer bloco de JavaScript do arquivo, se você tiver mencionado explicitamente (usando / / / &lt;referência /&gt;) ou não.

<a id="_Toc318097415"></a>
### <a name="css-editor"></a>Editor de CSS

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a>Conclusão de instrução de redução automática

A lista de IntelliSense para filtros do CSS agora com base nas propriedades de CSS e valores com suporte o esquema selecionado.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

Também, o IntelliSense dá suporte a pesquisas de título caso:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a>Recuo hierárquico

O editor de CSS usa o recuo para exibir regras hierárquicas, o que lhe dá uma visão geral de como as regras em cascata são logicamente organizadas. No exemplo a seguir, o #list um seletor é um filho em cascata da lista e, portanto, é recuado.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

O exemplo a seguir mostra a herança mais complexa:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

O recuo de uma regra é determinado por suas regras do pai. Recuo hierárquico é habilitado por padrão, mas você pode desabilitar a caixa de diálogo Opções (ferramentas, opções da barra de menus):

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a>CSS experimenta suporte

Análise de centenas de arquivos CSS do mundo real mostra que hackers CSS são muito comuns, e agora o Visual Studio oferece suporte às mais amplamente utilizadas. Esse suporte inclui o IntelliSense e a validação da estrela (\*) e sublinhado (\_) invasões de propriedade:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

Também há suporte para o seletor típico hackers para que o recuo hierárquico é mantido até mesmo quando elas são aplicadas. Um meio de seletor típica usado para o destino do Internet Explorer 7 é colocar um seletor com  *\*: primeiro filho + html*. Usar essa regra manterá o recuo hierárquico:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a>Esquemas específicos do fornecedor (- moz-, - webkit)

CSS3 apresenta muitas propriedades que foram implementadas por navegadores diferentes em momentos diferentes. Isso forçado anteriormente os desenvolvedores de código para navegadores específicos usando a sintaxe específica do fornecedor. Essas propriedades específicas do navegador agora estão incluídas no IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a>Suporte de comentário e removendo marca de comentário

Agora você pode comentar e remova as regras de CSS usando os mesmos atalhos que você usa no editor de código (Ctrl + K, Ctrl + K, você remova os comentários e C para o comentário).

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a>Seletor de cor

Nas versões anteriores do Visual Studio, o IntelliSense para os atributos relacionados a cor consistia em uma lista suspensa de valores de cor nomeada. Essa lista foi substituída por um seletor de cores completo.

Quando você inserir um valor de cor, o seletor de cores é exibido automaticamente e apresenta uma lista de cores usadas anteriormente, seguido de uma paleta de cores padrão. Você pode selecionar uma cor usando o mouse ou o teclado.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

A lista pode ser expandida em um seletor de cores concluída. O seletor de permite que você controle o canal alfa convertendo automaticamente todas as cores em RGBA quando você move o controle deslizante de opacidade:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a>Trechos de código

Trechos de código no editor de CSS tornam mais fácil e rápido para criar estilos entre navegadores. Muitas propriedades CSS3 que exigem configurações específicas do navegador agora foram revertidas em fragmentos.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

Trechos de código CSS oferecem suporte a cenários avançados (como as consultas de mídia CSS3) digitando no símbolo (@), que mostra a lista do IntelliSense.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

Quando você seleciona @media valor e pressione a tecla Tab, o editor de CSS insere o trecho a seguir:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

Assim como acontece com trechos de código, você pode criar seus próprios trechos de código CSS.

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a>Regiões personalizadas

Chamado regiões de código, que já estão disponíveis no editor de código, agora estão disponíveis para edição de CSS. Isso lhe permite agrupar facilmente blocos de estilo relacionadas.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

Quando uma região é recolhida, ele exibe o nome da região:

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a>Inspetor de Página

O Page Inspector é uma ferramenta que renderiza uma página da web (HTML, formulários da Web, ASP.NET MVC ou páginas da Web) no Visual Studio IDE e permite que você examine o código-fonte e a saída resultante. Para páginas ASP.NET, Page Inspector permite determinar qual código do lado do servidor produziram a marcação HTML que é renderizada no navegador.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

Para obter mais informações sobre o Page Inspector, consulte os seguintes tutoriais:

- Com o Page Inspector em [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)
- Com o Page Inspector em [Web Forms do ASP.NET](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)

<a id="_Toc318097425"></a>
### <a name="publishing"></a>Publicando

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a>Perfis de publicação

No Visual Studio 2010, as informações de publicação para projetos de aplicativo Web não são armazenadas no controle de versão em não foi projetadas para compartilhar com outras pessoas. No Visual Studio 2012 Release Candidate, o formato do perfil de publicação foi alterado. Ele se tornou um artefato de equipe e agora é fácil utilizar de compilações com base em MSBuild. Informações de configuração de compilação é na caixa de diálogo Publicar, para que você pode alternar facilmente as configurações de compilação antes da publicação.

Publicar perfis são armazenados na pasta PublishProfiles. Depende do local da pasta de linguagem de programação que você está usando:

- C#: Properties\PublishProfiles
- Visual Basic: Meu Project\PublishProfiles

Cada perfil é um arquivo MSBuild. Durante a publicação, esse arquivo é importado para o arquivo do projeto MSBuild. No Visual Studio 2010, se você quiser fazer alterações para o processo de publicação ou pacote, você precisa colocar as personalizações em um arquivo chamado **ProjectName**. wpp.targets. Isso ainda é suportado, mas agora você pode colocar suas personalizações no perfil de publicação em si. Dessa forma, as personalizações serão usadas somente para o perfil.

Você pode agora também aproveitar publicar perfis do MSBuild. Para fazer isso, use o comando a seguir quando você compilar o projeto:

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

O valor de project.csproj é o caminho do projeto e ProfileName é o nome do perfil para publicar. Como alternativa, em vez de passar o nome do perfil para o *PublishProfile* propriedade, você pode passar o caminho completo para o perfil de publicação.

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a>Mesclar e pré-compilação do ASP.NET

Para projetos de aplicativo Web, o Visual Studio 2012 Release Candidate adiciona uma opção na página de propriedades de pacote/publicar na Web que permite que você pré-compilar e mesclar o conteúdo do site quando você publica ou pacote do projeto. Para ver essas opções, clique com botão direito no projeto no Gerenciador de soluções, escolha Propriedades e, em seguida, escolha a página de propriedades de pacote/publicar na Web. A ilustração a seguir mostra o Precompile este aplicativo antes de publicar a opção.

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

Quando essa opção é selecionada, o Visual Studio pré-compila o aplicativo sempre que você publicar ou o pacote de aplicativo da web. Se você quiser controlar como o site é pré-compilado ou como os assemblies são mesclados, clique no botão Avançado para configurar essas opções.

<a id="_Toc318097428"></a>
### <a name="iis-express"></a>IIS Express

O servidor de web padrão para projetos de teste da web no Visual Studio é agora o IIS Express. O servidor de desenvolvimento do Visual Studio ainda é uma opção para o servidor web local durante o desenvolvimento, mas o IIS Express é agora o servidor recomendado. A experiência de usar o IIS Express no Visual Studio 11 Beta é muito semelhante ao usá-lo no Visual Studio 2010 SP1.

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a>Aviso de isenção de responsabilidade

Este é um documento preliminar e pode ser alterado substancialmente antes da versão comercial final do software descrito aqui.

As informações contidas neste documento representam a visão atual da Microsoft Corporation sobre os problemas discutidos a partir da data da publicação. Como a Microsoft deve responder às condições do mercado em constante mudança, este documento não deve ser interpretado como um compromisso por parte da Microsoft, e a Microsoft não pode garantir a exatidão de nenhuma informação apresentada após a data da publicação.

Este whitepaper é apenas para fins informativos. A MICROSOFT NÃO OFERECE GARANTIA, EXPRESSA, IMPLÍCITA OU ESTATUTÁRIA, DAS INFORMAÇÕES CONTIDAS NESTE DOCUMENTO.

Estar em conformidade com todas as leis de direitos autorais aplicáveis é responsabilidade do usuário. Sem limitar os direitos autorais, nenhuma parte deste documento pode ser reproduzida, armazenada ou inserida em um sistema de recuperação, ou transmitida de qualquer forma, por qualquer meio (eletrônico, mecânico, fotocópia, gravação ou outro) ou para qualquer fim, sem a permissão expressa, por escrito, da Microsoft Corporation.

A Microsoft pode ter patentes, solicitações de patente, marcas comerciais, direitos autorais ou outros direitos de propriedade intelectual que abrangem o conteúdo deste documento. A não ser que especificado expressamente em um contrato de licença da Microsoft, o fornecimento deste documento não confere a você nenhum direito sobre as supracitadas patentes, marcas comerciais, direitos autorais ou outra propriedade intelectual.

Salvo indicação em contrário, os exemplos de empresas, organizações, produtos, nomes de domínio, endereços de email, logotipos, pessoas, locais e eventos descritos no presente documento são fictícios, e qualquer ligação com qualquer empresa, organização, produto, nome de domínio, endereço de email, logotipo, pessoa, local ou eventos reais é intencional ou deve ser inferida.

© 2012 Microsoft Corporation. Todos os direitos reservados.

Microsoft e Windows são marcas registradas ou comerciais da Microsoft Corporation nos Estados Unidos e/ou em outros países.

Os nomes de empresas reais e produtos mencionados aqui podem ser marcas comerciais de seus respectivos proprietários.
