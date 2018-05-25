---
uid: web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
title: Solucionando problemas de HTTP 405 erros após a publicação da Web API 2 aplicativos | Microsoft Docs
author: rmcmurray
description: Este tutorial descreve como solucionar problemas de erros HTTP 405 depois de publicar um aplicativo de API da Web em um servidor web de produção.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/01/2014
ms.topic: article
ms.assetid: 07ec7d37-023f-43ea-b471-60b08ce338f7
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/troubleshooting-http-405-errors-after-publishing-web-api-applications
msc.type: authoredcontent
ms.openlocfilehash: b87ae7420e1295030e90c30e97b1e331413ce263
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/15/2017
---
<a name="troubleshooting-http-405-errors-after-publishing-web-api-2-applications"></a>Solucionando problemas de HTTP 405 erros após a publicação da Web API 2 aplicativos
====================
por [Robert McMurray](https://github.com/rmcmurray)

> Este tutorial descreve como solucionar problemas de erros HTTP 405 depois de publicar um aplicativo de API da Web em um servidor web de produção.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versões de software usadas no tutorial
> 
> 
> - [Serviços de informações da Internet (IIS)](https://www.iis.net/) (versão 7 ou posterior)
> - [Web API](../../index.md) (versão 1 ou 2)


Aplicativos de API da Web geralmente usam vários verbos HTTP comuns: GET, POST, PUT, DELETE e às vezes PATCH. Dito isso, os desenvolvedores podem executar em situações em que os verbos são implementados por outro módulo do IIS em seu servidor de produção, o que leva a uma situação em que um controlador de API da Web que funciona corretamente no Visual Studio ou em um servidor de desenvolvimento retornará um HTTP 405 erro quando ele é implantado em um servidor de produção. Felizmente, esse problema é resolvido facilmente, mas a resolução garante uma explicação de por que o problema está ocorrendo.

## <a name="what-causes-http-405-errors"></a>O que provoca o HTTP 405 erros

A primeira etapa para aprender a problemas de erros HTTP 405 é entender o que um erro de HTTP 405 realmente significa. O principal controle de documento para HTTP é [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), que define o código de status HTTP 405 como ***método não permitido***e descreve ainda mais esse código de status como uma situação onde &quot;o método especificado na linha de solicitação não é permitida para o recurso identificado pelo URI da solicitação.&quot; Em outras palavras, o verbo HTTP não é permitido para a URL específica que um cliente HTTP solicitado.

Como uma breve revisão, aqui estão vários métodos HTTP mais usadas conforme definido na RFC 2616, RFC 4918 e RFC 5789:

| Método HTTP | Descrição |
| --- | --- |
| **OBTER** | Esse método é usado para recuperar dados de um URI e provavelmente o método HTTP mais usadas. |
| **CABEÇALHO** | Esse método é muito parecido com o método GET, exceto que, na verdade, ele não recupera os dados do URI de solicitação - ele simplesmente recupera o status HTTP. |
| **POSTAR** | Esse método é normalmente usado para enviar dados novos para o URI. POST geralmente é usado para enviar dados de formulário. |
| **PUT** | Esse método é normalmente usado para enviar dados brutos para o URI. PUT geralmente é usado para enviar dados JSON ou XML para aplicativos de API da Web. |
| **EXCLUIR** | Esse método é usado para remover dados de um URI. |
| **OPÇÕES** | Normalmente, esse método é usado para recuperar a lista de métodos HTTP com suporte para um URI. |
| **COPIAR MOVER** | Esses dois métodos são usados com o WebDAV e sua finalidade é auto-explicativo. |
| **MKCOL** | Esse método é usado com WebDAV e ele é usado para criar uma coleção (por exemplo, um diretório) no URI especificado. |
| **PROPPATCH PROPFIND** | Esses dois métodos são usados com WebDAV, e eles são usados para consultar ou definir propriedades para um URI. |
| **DESBLOQUEAR BLOQUEIO** | Esses dois métodos são usados com WebDAV, e elas são usadas para bloquear/desbloquear o recurso identificado pelo URI de solicitação ao criar. |
| **PATCH** | Esse método é usado para modificar um recurso existente de HTTP. |

Quando um dos seguintes métodos HTTP está configurado para uso no servidor, o servidor responderá com o status HTTP e outros dados que é apropriados para a solicitação. (Por exemplo, um método GET pode receber um HTTP 200 ***Okey*** resposta e um método PUT podem receber um HTTP 201 ***criado*** resposta.)

Se o método HTTP não está configurado para uso no servidor, o servidor responderá com um HTTP 501 ***não implementado*** erro.

No entanto, quando um método HTTP está configurado para uso no servidor, mas foi desabilitada para um determinado URI, o servidor responderá com um HTTP 405 ***método não permitido*** erro.

## <a name="example-http-405-error"></a>Exemplo HTTP 405 erro

O seguinte exemplo de solicitação HTTP e resposta ilustram uma situação em que um cliente HTTP está tentando inserir um valor para um aplicativo de API da Web em um servidor web e o servidor retornará um erro HTTP que indica que o método PUT não é permitida:


Solicitação de HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample1.cmd)]


Resposta de HTTP:


[!code-console[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample2.cmd)]


Neste exemplo, o cliente HTTP enviou uma solicitação JSON válida para a URL para um aplicativo de API da Web em um servidor web, mas o servidor retornou uma mensagem de erro HTTP 405 que indica que o método PUT não foi permitido para a URL. Por outro lado, se o URI de solicitação não correspondeu a uma rota para o aplicativo de API da Web, o servidor retornará um HTTP 404 ***não encontrado*** erro.

## <a name="resolving-http-405-errors"></a>Resolvendo HTTP 405 erros

Há várias razões por um verbo HTTP específico pode não estar disponíveis, mas não há um cenário principal que é a principal causa desse erro no IIS: vários manipuladores são definidos para o mesmo verbo/método, e um dos manipuladores está bloqueando o manipulador esperado do processar a solicitação. Por meio de explicação, o IIS processa manipuladores do primeiro ao último baseados nas entradas de manipulador de ordem nos arquivos de applicationHost. config e Web. config, onde a primeira combinação de correspondência de caminho, verbo, recursos, etc., será usada para manipular a solicitação.

O exemplo a seguir é um trecho de um arquivo applicationHost. config para um servidor IIS que estava retornando um erro HTTP 405 ao usar o método PUT para enviar dados para um aplicativo de API da Web. Esse trecho, vários manipuladores HTTP são definidos e cada manipulador tem um conjunto diferente de métodos HTTP para o qual ele está configurado - a última entrada na lista é o manipulador de conteúdo estático, o que é o manipulador padrão que é usado depois que os outros manipuladores tiveram um chanc e para examinar a solicitação:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample3.xml)]

No exemplo acima, o manipulador de WebDAV e o manipulador de URL sem extensão para o ASP.NET (que é usado para a API da Web) claramente são definidos para listas separadas dos métodos HTTP. Observe que o manipulador de DLL ISAPI está configurado para todos os métodos HTTP, embora essa configuração não cause necessariamente um erro. No entanto, as definições de configuração como essa precisam ser considerados ao solucionar problemas de erros de HTTP 405.

No exemplo acima, o manipulador DLL ISAPI não era o problema; Na verdade, o problema não foi definido no arquivo applicationHost. config para o servidor IIS - o problema foi causado por uma entrada que foi feita no arquivo Web. config quando o aplicativo de API da Web foi criado no Visual Studio. O seguinte trecho do arquivo de Web. config do aplicativo mostra o local do problema:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample4.xml)]

Esse trecho, o manipulador de URL sem extensão para o ASP.NET é redefinido para incluir os métodos HTTP adicionais que serão usados com o aplicativo de API da Web. No entanto, uma vez que um conjunto similar de métodos HTTP é definido para o manipulador de WebDAV, ocorrerá um conflito. Nesse caso específico, o manipulador de WebDAV é definido e carregado pelo IIS, mesmo que o WebDAV está desabilitado para o site que inclui o aplicativo de API da Web. Durante o processamento de uma solicitação HTTP PUT, o IIS chama o módulo de WebDAV, pois ele está definido para o verbo PUT. Quando o módulo de WebDAV é chamado, ele verifica sua configuração e vê que ele está desabilitado, portanto, ele retornará um HTTP 405 ***método não permitido*** erro para qualquer solicitação que é semelhante a uma solicitação de WebDAV. Para resolver esse problema, você deve remover o WebDAV na lista de módulos HTTP para o site onde o aplicativo de API da Web está definido. O exemplo a seguir mostra o que a que talvez o seguinte:

[!code-xml[Main](troubleshooting-http-405-errors-after-publishing-web-api-applications/samples/sample5.xml)]

Neste cenário geralmente é encontrado depois que um aplicativo é publicado em um ambiente de desenvolvimento para um ambiente de produção, e isso ocorre porque a lista de manipuladores/módulos é diferente entre seus ambientes de desenvolvimento e produção. Por exemplo, se você estiver usando o Visual Studio 2012 ou 2013 para desenvolver um aplicativo de API da Web, o IIS Express 8 é o servidor de web padrão para teste. Este servidor da web de desenvolvimento é uma versão reduzida a funcionalidade completa do IIS que é fornecido em um produto de servidor, e esse servidor web de desenvolvimento contém algumas alterações que foram adicionadas para cenários de desenvolvimento. Por exemplo, o módulo de WebDAV geralmente é instalado em um servidor web de produção que está executando a versão completa do IIS, embora ele não pode estar em uso real. A versão de desenvolvimento do IIS (IIS Express), instala o módulo de WebDAV, mas as entradas para o módulo de WebDAV são intencionalmente comentadas, para que o módulo de WebDAV nunca é carregado no IIS Express, a menos que você altere a configuração do IIS Express especificamente configurações para adicionar funcionalidade WebDAV à instalação do IIS Express. Como resultado, o aplicativo da web pode funcionar corretamente em seu computador de desenvolvimento, mas você pode encontrar erros de HTTP 405 quando você publica seu aplicativo de API da Web no seu servidor web de produção.

## <a name="summary"></a>Resumo

HTTP 405 erros são causados quando um método HTTP não é permitido por um servidor web para uma URL solicitada. Essa condição geralmente é vista quando um determinado manipulador foi definido para um verbo específico e esse manipulador está substituindo o manipulador para processar a solicitação.

Se você encontrar uma situação onde você recebe uma mensagem de erro HTTP 501, o que significa que a funcionalidade específica não foi implementada no servidor, isso geralmente significa que não há nenhum manipulador definido nas configurações do IIS que corresponde à solicitação de HTTP, que provavelmente indica que algo não foi instalado corretamente no sistema ou algo tiver modificado as configurações do IIS para que existem que sem manipuladores definidos para esse método de suporte o HTTP específico. Para resolver esse problema, você precisa reinstalar qualquer aplicativo que está tentando usar um método HTTP para o qual ele não tem o módulo correspondente ou definições de manipulador.
