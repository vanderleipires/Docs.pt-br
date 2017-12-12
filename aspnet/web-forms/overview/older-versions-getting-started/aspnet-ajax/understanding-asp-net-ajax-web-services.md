---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: "Noções básicas sobre os serviços Web do ASP.NET AJAX | Microsoft Docs"
author: scottcate
description: "Serviços Web são uma parte integrante do .NET framework que fornecem uma solução de plataforma cruzada para trocar dados entre sistemas distribuídos. Embora o Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 8eb3486c9b3f4ddb6a8bc2c1cdcac774a6852574
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="understanding-aspnet-ajax-web-services"></a>Noções básicas sobre os serviços Web do ASP.NET AJAX
====================
por [Scott Ndicar](https://github.com/scottcate)

[Baixar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Serviços Web são uma parte integrante do .NET framework que fornecem uma solução de plataforma cruzada para trocar dados entre sistemas distribuídos. Embora os serviços da Web normalmente são usados para permitir que diferentes sistemas operacionais, modelos de objeto e linguagens de programação para enviar e receber dados, eles também podem ser usados para inserir dados em uma página ASP.NET AJAX ou enviar dados de uma página para um sistema back-end dinamicamente. Tudo isso pode ser feito sem recorrer para operações de postback.


## <a name="calling-web-services-with-aspnet-ajax"></a>Chamando serviços Web com o ASP.NET AJAX

Dan Wahlin

Serviços Web são uma parte integrante do .NET framework que fornecem uma solução de plataforma cruzada para trocar dados entre sistemas distribuídos. Embora os serviços da Web normalmente são usados para permitir que diferentes sistemas operacionais, modelos de objeto e linguagens de programação para enviar e receber dados, eles também podem ser usados para inserir dados em uma página ASP.NET AJAX ou enviar dados de uma página para um sistema back-end dinamicamente. Tudo isso pode ser feito sem recorrer para operações de postback.

Enquanto o controle UpdatePanel do ASP.NET AJAX fornece uma maneira simples de AJAX habilitar qualquer página do ASP.NET, pode haver ocasiões em que você precisa para acessar dados no servidor dinamicamente sem usar um UpdatePanel. Neste artigo, você verá como fazer isso criando e consumir serviços da Web em páginas ASP.NET AJAX.

Este artigo concentra-se na funcionalidade disponível em extensões de AJAX do ASP.NET core, bem como um controle de serviço da Web habilitado no Kit de ferramentas do AJAX do ASP.NET chamado o AutoCompleteExtender. Tópicos abordados incluem definir os serviços da Web habilitado para AJAX, criando proxies de cliente e chamar os serviços Web com JavaScript. Você também verá como é possível fazer chamadas de serviço da Web diretamente para os métodos de página ASP.NET.

## <a name="web-services-configuration"></a>Configuração de serviços Web

Quando um novo projeto de Site da Web é criado com o Visual Studio 2008, o arquivo Web. config tem um número de novas adições que podem não ser familiares para usuários de versões anteriores do Visual Studio. Algumas dessas modificações mapeiam o prefixo "asp" para controles do ASP.NET AJAX para que possam ser usados nas páginas enquanto outros definem HttpHandlers e HttpModules necessários. Listagem 1 mostra as modificações feitas para o `<httpHandlers>` elemento no Web. config que afeta as chamadas de serviço Web. O padrão que HttpHandler usado para processar chamadas. asmx é removido e substituído por uma classe ScriptHandlerFactory localizada no assembly System.Web.Extensions.dll. System.Web.Extensions.dll contém toda a funcionalidade de núcleos usada pelo ASP.NET AJAX.

**Listando 1. Configuração de manipulador de serviços Web do ASP.NET AJAX**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Essa substituição HttpHandler é feita para permitir chamadas de notação JSON (JavaScript Object) a serem feitas nas páginas do ASP.NET AJAX nos serviços Web .NET usando um proxy do serviço Web de JavaScript. ASP.NET AJAX envia mensagens JSON para serviços da Web em vez de chamadas de protocolo de acesso a objeto simples (SOAP) padrão normalmente associadas aos serviços da Web. Isso resulta em menor solicitação e mensagens de resposta gerais. Ele também permite processamento do lado do cliente mais eficiente de dados desde que a biblioteca JavaScript do ASP.NET AJAX é otimizada para trabalhar com objetos JSON. Listando 2 e 3 listando mostram exemplos de mensagens de solicitação e resposta do serviço Web serializadas em formato JSON. A mensagem de solicitação mostrada na listagem 2 passa um parâmetro de país com um valor de "Bélgica", enquanto a mensagem de resposta na listagem 3 passa uma matriz de objetos de cliente e suas propriedades associadas.

**A listagem 2. Mensagem de solicitação de serviço Web serializada para JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *>[!NOTE] o nome da operação é definido como parte da URL para o serviço web; Além disso, as mensagens de solicitação não são sempre enviadas por meio de JSON. Serviços Web podem utilizar o atributo ScriptMethod com o parâmetro UseHttpGet definido como true, o que faz com que os parâmetros a serem passados por meio de um os parâmetros de cadeia de caracteres de consulta.*


**A listagem 3. Mensagem de resposta do serviço Web serializada para JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

Na próxima seção, você verá como criar serviços Web capaz de lidar com mensagens de solicitação JSON e responder com tipos simples e complexos.

## <a name="creating-ajax-enabled-web-services"></a>Criação de serviços da Web habilitado para AJAX

A estrutura do ASP.NET AJAX fornece várias maneiras de chamar serviços da Web. Você pode usar o controle AutoCompleteExtender (disponível no Kit de ferramentas do ASP.NET AJAX) ou JavaScript. No entanto, antes de chamar um serviço você precisa habilitar AJAX-lo para que ele pode ser chamado pelo código de script de cliente.

Se você estiver familiarizado com serviços Web ASP.NET, ou não, você encontrará ele simples de criar e habilitar AJAX serviços. O .NET framework tem suporte para a criação de serviços Web ASP.NET desde seu lançamento inicial em 2002 e o ASP.NET AJAX Extensions oferecem funcionalidade AJAX adicional que se baseia a conjunto de recursos padrão do .NET framework. 2008 Beta 2 do Visual Studio .NET tem suporte interno para a criação de arquivos de serviço Web. asmx e deriva automaticamente o código associado ao lado de classes da classe WebService. Como adicionar métodos na classe, você deve aplicar o atributo WebMethod para poderem ser chamado pelos consumidores de serviço da Web.

A listagem 4 mostra um exemplo de como aplicar o atributo WebMethod a um método chamado GetCustomersByCountry().

**A listagem 4. Usando o atributo WebMethod em um serviço Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

O método GetCustomersByCountry() aceita um parâmetro de país e retorna um cliente a matriz de objetos. O valor do país passado para o método é encaminhado a uma classe de camada de negócios que por sua vez chama uma classe de camada de dados para recuperar os dados do banco de dados, preencha as propriedades do objeto cliente com dados e retornar a matriz.

## <a name="using-the-scriptservice-attribute"></a>Usando o atributo ScriptService

Ao adicionar o WebMethod atributo permite que o método GetCustomersByCountry() deve ser chamado por clientes que enviam mensagens SOAP padrão para o serviço da Web, ele não permite chamadas JSON sejam feitas de aplicativos do ASP.NET AJAX sem a necessidade de. Para permitir chamadas JSON a ser feita a aplicar a extensão ASP.NET AJAX `ScriptService` atributo para a classe de serviço da Web. Isso permite que um serviço Web enviar mensagens de resposta formatadas usando JSON e permite que o script do lado do cliente chamar um serviço, enviando mensagens JSON.

Listagem 5 mostra um exemplo de como aplicar o atributo ScriptService a uma classe de serviço da Web denominada CustomersService.

**Listagem 5. Usando o atributo ScriptService AJAX-habilitar um serviço Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

O atributo ScriptService atua como um marcador que indica que ele pode ser chamado de código de script AJAX. Na verdade, não trata qualquer as tarefas de serialização ou desserialização de JSON que ocorrem em segundo plano. O ScriptHandlerFactory (configurado no Web. config) e outras classes relacionadas para fazer a maior parte do processamento de JSON.

## <a name="using-the-scriptmethod-attribute"></a>Usando o atributo ScriptMethod

O atributo ScriptService é o único atributo ASP.NET AJAX que deve ser definido em um serviço da Web .NET para que a ser usado por páginas ASP.NET AJAX. No entanto, outro atributo denominado ScriptMethod também pode ser aplicado diretamente para os métodos da Web em um serviço. ScriptMethod define três propriedades incluindo `UseHttpGet`, `ResponseFormat` e `XmlSerializeString`. Alterar os valores dessas propriedades pode ser útil em casos em que o tipo de solicitação aceita por um método Web precisa ser alterado para GET, quando um método Web precisa retornar dados brutos de XML na forma de um `XmlDocument` ou `XmlElement` objeto ou quando os dados retornados de um  serviço sempre deve ser serializado como XML, em vez de JSON.

O UseHttpGet propriedade pode ser usada quando um método Web deve aceitar obter solicitações em vez de solicitações POST. Solicitações são enviadas usando uma URL com parâmetros de entrada do método da Web convertido em Parâmetros QueryString. UseHttpGet propriedade assume false como padrão e só deverá ser definido como `true` quando as operações são conhecidas ser seguras e quando os dados confidenciais não são passados para um serviço Web. Listar 6 mostra um exemplo de como usar o atributo ScriptMethod com a propriedade UseHttpGet.

**Listagem 6. Usando o atributo ScriptMethod com a propriedade UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Um exemplo de como os cabeçalhos enviados quando é chamado de método da Web HttpGetEcho mostrado na listagem 6 são mostrado a seguir:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Além de permitir que os métodos da Web aceitar solicitações HTTP GET, o atributo ScriptMethod também pode ser usado quando respostas XML precisam ser retornado de um serviço em vez de JSON. Por exemplo, um serviço Web pode recuperar um RSS feed de um site remoto e retorná-lo como um objeto XmlDocument ou XmlElement. Processamento do XML dados, em seguida, pode ocorrer no cliente.

Listagem 7 mostra um exemplo de como usar a propriedade ResponseFormat para especificar que os dados XML devem ser retornados de um método Web.

**Listagem 7. Usando o atributo ScriptMethod com a propriedade ResponseFormat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

A propriedade ResponseFormat também pode ser usada junto com a propriedade XmlSerializeString. A propriedade XmlSerializeString tem um valor padrão de false, que significa que todos retornam tipos exceto cadeias de caracteres retornadas de um método Web são serializados como XML quando o `ResponseFormat` está definida como `ResponseFormat.Xml`. Quando `XmlSerializeString` é definido como `true`, todos os tipos retornados de um método Web são serializados como XML, incluindo tipos de cadeia de caracteres. Se a propriedade ResponseFormat tem um valor de `ResponseFormat.Json` a propriedade XmlSerializeString é ignorada.

Listagem 8 mostra um exemplo de como usar a propriedade XmlSerializeString para forçar a cadeias de caracteres a ser serializado como XML.

**Listando 8. Usando o atributo ScriptMethod com a propriedade XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

O valor retornado da chamada do método de Web GetXmlString mostrado na listagem 8 é mostrado a seguir:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Embora o formato JSON padrão minimiza o tamanho total de mensagens de solicitação e resposta e mais prontamente é consumido pelos clientes de ASP.NET AJAX, de maneira entre navegadores, as propriedades ResponseFormat e XmlSerializeString podem ser utilizada ao cliente aplicativos como o Internet Explorer 5 ou superior esperam que os dados XML a ser retornado de um método Web.

## <a name="working-with-complex-types"></a>Trabalhando com tipos complexos

Listagem 5 mostramos um exemplo de retorno de um tipo complexo chamado cliente de um serviço Web. A classe de cliente define vários tipos diferentes de simples internamente como propriedades, como nome e sobrenome. Tipos complexos usado como um parâmetro de entrada ou tipo de retorno em um método Web habilitado para AJAX automaticamente são serializados em JSON antes de serem enviados ao lado do cliente. No entanto, tipos complexos aninhados (aquelas definido internamente dentro de outro tipo) não ficam disponíveis para o cliente como objetos autônomos por padrão.

Em casos onde um tipo complexo aninhado usado por um serviço Web também deverá ser usado em uma página de cliente, o atributo de ASP.NET AJAX GenerateScriptType pode ser adicionado ao serviço Web. Por exemplo, a classe CustomerDetails mostrada na listagem 9 contém propriedades de endereço e sexo que ***representar aninhados tipos complexos.***

**Listagem 9. A classe CustomerDetails mostrada aqui contém dois tipos complexos aninhados.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Os objetos de endereço e sexo definidos dentro da classe CustomerDetails mostrada na listagem 9 não automaticamente ser disponibilizados para uso no lado do cliente por meio de JavaScript como eles são tipos aninhados (endereço é uma classe e sexo é uma enumeração). Em situações em que um tipo aninhado usado dentro de um serviço Web deve estar disponível no lado do cliente, o atributo GenerateScriptType mencionado anteriormente pode ser usado (consulte a listagem 10). Esse atributo pode ser adicionado várias vezes em casos em que diferentes tipos complexos aninhados são retornados de um serviço. Ele pode ser aplicado diretamente à classe de serviço da Web ou acima os métodos de Web específicos.

**Listagem 10. Usando o atributo GenerateScriptService para definir tipos aninhados que devem estar disponíveis para o cliente.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Aplicando o `GenerateScriptType` atributo para os tipos de serviço da Web, o endereço e sexo será automaticamente disponibilizado para uso pelo código do ASP.NET AJAX JavaScript do lado do cliente. Um exemplo do JavaScript que é automaticamente gerado e enviado ao cliente, adicionando o atributo GenerateScriptType em um serviço Web é mostrado na listagem 11. Você verá como usar tipos complexos aninhados posteriormente neste artigo.

**Listagem 11. Tipos complexos aninhados disponibilizados para uma página ASP.NET AJAX.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Agora que você viu como criar serviços Web e torná-los acessíveis para páginas ASP.NET AJAX, vamos dar uma olhada em como criar e usar proxies JavaScript para que os dados podem ser recuperados ou enviados para os serviços Web.

## <a name="creating-javascript-proxies"></a>Criando Proxies JavaScript

Chamar um serviço da Web padrão (.NET ou outra plataforma) normalmente envolve a criação de um objeto proxy que protege contra as complexidades de envio de mensagens de solicitação e resposta SOAP. Com chamadas de serviço Web do ASP.NET AJAX, proxies JavaScript podem ser criados e usados para chamar serviços facilmente sem se preocupar com a serialização e desserialização JSON mensagens. Os proxies JavaScript podem ser gerados automaticamente usando o ASP.NET AJAX ScriptManager.

Criando um proxy JavaScript que pode chamar serviços da Web é realizada usando a propriedade de serviços do ScriptManager. Essa propriedade permite que você defina um ou mais serviços que uma página ASP.NET AJAX pode chamar de forma assíncrona para enviar ou receber dados sem a necessidade de operações de postback. Definir um serviço usando o ASP.NET AJAX `ServiceReference` controle e atribuindo a URL do serviço da Web para o controle `Path` propriedade. Listar 12 mostra um exemplo de referência a um serviço chamado CustomersService.asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Listando 12. Definição de um serviço da Web usado em uma página ASP.NET AJAX.**

Adicionar uma referência a CustomersService.asmx por meio do controle ScriptManager faz com que um proxy JavaScript a ser gerado dinamicamente e referenciado pela página. O proxy é inserido usando o &lt;script&gt; marca e carregados dinamicamente chamando o arquivo CustomersService.asmx e acrescentando /js ao final dele. O exemplo a seguir mostra como o proxy JavaScript é inserido na página quando a depuração está desabilitada no Web. config:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *>[!NOTE] Se você gostaria de ver o código de proxy JavaScript real que é gerado pode digitar a URL para o serviço da Web .NET desejado na caixa de endereços do Internet Explorer e acrescentar /js ao final dele.*


Se a depuração é ativada em Web. config, que uma versão de depuração de proxy JavaScript será inserida na página, como mostrado a seguir:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

O proxy JavaScript criado pelo ScriptManager também podem ser inserido diretamente na página, em vez de referenciados usando a &lt;script&gt; atributo src da marca. Isso pode ser feito definindo a ServiceReference propriedade do controle InlineScript como true (o padrão é falso). Isso pode ser útil quando um proxy não é compartilhado por várias páginas e você deseja reduzir o número de chamadas de rede com o servidor. Quando InlineScript é definida como true, o script de proxy não ser armazenadas em cache pelo navegador para que o valor padrão de false é recomendado em casos em que o proxy é usado por várias páginas em um aplicativo ASP.NET AJAX. Um exemplo de como usar a propriedade InlineScript é mostrado a seguir:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Uso de Proxies de JavaScript

Depois que um serviço Web é referenciado por uma página ASP.NET AJAX usando o controle ScriptManager, pode ser feita uma chamada para o serviço Web e os dados retornados podem ser tratados usando funções de retorno de chamada. Um serviço Web é chamado pelo referenciando seu namespace (se houver), nome da classe e o nome do método da Web. Todos os parâmetros passados para o serviço Web podem ser definidos com uma função de retorno de chamada que manipula os dados retornados.

Um exemplo do uso de um proxy JavaScript para chamar um método Web chamado GetCustomersByCountry() é mostrado na listagem 13. A função GetCustomersByCountry() é chamada quando um usuário final clica em um botão na página.

**A listagem 13. Chamando um serviço da Web com um proxy JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Essa chamada faz referência ao namespace de InterfaceTraining, CustomersService classe e método de Web GetCustomersByCountry definido no serviço. Ele passa um valor de país obtido de uma caixa de texto, bem como uma função de retorno de chamada denominado OnWSRequestComplete que deve ser invocada quando a chamada assíncrona do serviço da Web retorna. OnWSRequestComplete trata a matriz de objetos de cliente retornados do serviço e converte-os em uma tabela que é exibida na página. A saída gerada da chamada é mostrada na Figura 1.


[![Associação de dados obtidos fazendo uma chamada AJAX assíncrona para um serviço Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Figura 1**: associação de dados obtido fazendo uma chamada AJAX assíncrona para um serviço Web.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-web-services/_static/image3.png))


Proxies JavaScript também podem fazer chamadas unidirecionais para serviços da Web em casos onde um método Web deve ser chamado, mas o proxy não deve aguardar uma resposta. Por exemplo, você talvez queira chamar um serviço Web para iniciar um processo, como um fluxo de trabalho, mas não espera um valor de retorno do serviço. Em casos em que uma chamada unidirecional precisa ser feita para um serviço, a função de retorno de chamada mostrada na listagem 13 simplesmente pode ser omitida. Uma vez que nenhuma função de retorno de chamada é definida o objeto de proxy não aguardará para o serviço Web retornar dados.

## <a name="handling-errors"></a>Manipulando erros

Retornos de chamada para os serviços Web podem encontrar diferentes tipos de erros, como de rede, o serviço Web está indisponível ou uma exceção que está sendo retornado. Felizmente, os objetos de proxy JavaScript gerados pelo ScriptManager permitem que vários retornos de chamada a ser definido para tratar erros e falhas além do retorno de chamada de êxito mostrado anteriormente. Uma função de retorno de chamada do erro pode ser definida imediatamente após a função de retorno de chamada padrão na chamada ao método da Web, conforme mostrado na 14 de listagem.

**A listagem 14. Definindo uma função de retorno de chamada do erro e exibir erros.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Quaisquer erros que ocorram quando o serviço Web é chamado disparará a função de retorno de chamada de OnWSRequestFailed() para ser chamado que aceita um objeto que representa o erro como um parâmetro. O objeto de erro expõe várias funções diferentes para determinar a causa do erro, bem como se a chamada atingiu o tempo limite. A listagem 14 mostra um exemplo de como usar as funções de erro diferentes e a Figura 2 mostra um exemplo da saída gerada pelas funções.


[![Saída gerada ao chamar funções de erro do ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Figura 2**: saída gerada ao chamar funções de erro do ASP.NET AJAX.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Manipulação de dados XML retornados de um serviço Web

Anteriormente, você viu como um método Web poderia retornar dados XML não processados usando o atributo ScriptMethod junto com sua propriedade ResponseFormat. Quando ResponseFormat é definido como ResponseFormat.Xml, os dados retornados do serviço da Web são serializados como XML, em vez de JSON. Isso pode ser útil quando os dados XML precisam ser passado diretamente para o cliente para processamento usando JavaScript ou XSLT. No momento, o Internet Explorer 5 ou superior fornece o melhor modelo de objeto do lado do cliente para análise e filtragem de dados XML devido a seu suporte interno para MSXML.

Recuperando dados XML de um serviço Web não é diferente de recuperar outros tipos de dados. Iniciar invocando o proxy JavaScript para chamar a função apropriada e definir uma função de retorno de chamada. Depois que a chamada retorna, em seguida, você pode processar os dados na função de retorno de chamada.

Listagem 15 mostra um exemplo de como chamar um método Web chamado GetRssFeed() que retorna um objeto XmlElement. GetRssFeed() aceita um único parâmetro que representa a URL para o RSS feed para recuperar.

**Listando 15. Trabalhando com dados XML retornados de um serviço Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

Este exemplo passa uma URL para um RSS feed e processa os dados XML retornados na função OnWSRequestComplete(). OnWSRequestComplete() primeiro verifica para ver se o navegador Internet Explorer para saber se o analisador MSXML está disponível. Se for, uma instrução XPath é usada para localizar todos os &lt;item&gt; marcas dentro do RSS feed. Iteração, em seguida, cada item e os respectivos &lt;título&gt; e &lt;link&gt; marcas estão localizadas e processadas para exibir dados de cada item. A Figura 3 mostra um exemplo da saída gerada a partir de fazer com que um ASP.NET AJAX chamada através de um proxy JavaScript para o método da Web GetRssFeed().

## <a name="handling-complex-types"></a>Tratamento de tipos complexos

Tipos complexos aceitas ou retornado por um serviço Web automaticamente são expostos por meio de um proxy JavaScript. No entanto, tipos complexos aninhados não são diretamente acessíveis no lado do cliente, a menos que o atributo GenerateScriptType é aplicado ao serviço, conforme discutido anteriormente. Por que você deseja usar um tipo complexo aninhado no lado do cliente?

Para responder essa pergunta, suponha que uma página ASP.NET AJAX exibe os dados do cliente e permite que os usuários finais atualizar o endereço do cliente. Se o serviço da Web Especifica que o tipo de endereço (um tipo complexo definido dentro de uma classe CustomerDetails) pode ser enviado ao cliente, em seguida, o processo de atualização pode ser dividido em funções separadas para melhor código reutilização.


[![Criando a partir de chamar um serviço Web que retorna dados RSS a saída.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Figura 3**: criação de chamar um serviço Web que retorna dados RSS de saída.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-web-services/_static/image9.png))


Listar 16 mostra um exemplo de código do lado do cliente que invoca um objeto de endereço definido em um namespace de modelo, ele é preenchido com dados atualizados e o atribui ao CustomerDetails endereço propriedade de um objeto. O objeto CustomerDetails é então passado para o serviço Web para processamento.

**Listando 16. Usando tipos complexos aninhados**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Criando e usando os métodos de página

Serviços Web fornecem uma maneira excelente para expor serviços utilizáveis novamente a uma variedade de clientes, incluindo páginas ASP.NET AJAX. No entanto, pode haver casos em que uma página precisa recuperar dados que já não usados ou compartilhados por outras páginas. Nesse caso, tornar um arquivo. asmx para permitir que a página acessar os dados pode parecer um exagero desde que o serviço é usado somente por uma única página.

ASP.NET AJAX fornece outro mecanismo para fazer chamadas de tipo de serviço da Web sem criar arquivos. asmx de autônomo. Isso é feito usando uma técnica conhecida como "métodos de página". Métodos de página são métodos estáticos (compartilhado no VB.NET) inseridos diretamente em um arquivo de página ou ao lado do código que têm o atributo WebMethod aplicado a eles. Aplicando o atributo WebMethod eles podem ser chamados usando um objeto JavaScript especial chamado PageMethods que é criada dinamicamente em tempo de execução. O objeto PageMethods atua como um proxy que protege contra o processo de serialização/desserialização JSON. Observe que, para usar o objeto PageMethods você deve definir propriedade de EnablePageMethods do ScriptManager como true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Listar 17 mostra um exemplo de definição de dois métodos de página em uma classe ao lado do código do ASP.NET. Esses métodos de recuperam dados de uma classe de camada de negócios localizada no aplicativo do\_pasta de código do site.

**Listando 17. Definir métodos de página.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Quando o ScriptManager detecta a presença de métodos Web na página, ele gera uma referência dinâmica para o objeto PageMethods mencionado anteriormente. Chamando um método Web é feito pela referência à classe PageMethods seguida do nome do método e quaisquer dados de parâmetro necessário que devem ser fornecidos. Listar 18 mostra exemplos de como chamar os dois métodos de página mostrados anteriormente.

**Listando 18. Chamando métodos de página com o objeto PageMethods JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Usando o objeto PageMethods é muito semelhante ao uso de um objeto de proxy JavaScript. Você primeiro especificar todos os dados de parâmetro que devem ser passados para o método de página e, em seguida, definem a função de retorno de chamada que deve ser chamada quando a chamada assíncrona retorna. Também pode ser especificado um retorno de chamada de falha (consulte a listagem 14 para obter um exemplo de tratamento de falhas).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>O AutoCompleteExtender e o Kit de ferramentas do ASP.NET AJAX

O Kit de ferramentas do ASP.NET AJAX (disponível em [http://ajax.asp.net](http://ajax.asp.net)) oferece vários controles que podem ser usados para acessar os serviços Web. Especificamente, o Kit de ferramentas contém um controle útil chamado `AutoCompleteExtender` que pode ser usado para chamar serviços da Web e exibir dados em páginas sem gravar qualquer código JavaScript em todos os.

O controle AutoCompleteExtender pode ser usado para estender a funcionalidade existente de uma caixa de texto e ajudar os usuários mais facilmente localizar dados que estão procurando. Como eles digitam em uma caixa de texto o controle pode ser usado para consultar um serviço Web e mostra os resultados abaixo da caixa de texto dinamicamente. A Figura 4 mostra um exemplo de como usar o controle AutoCompleteExtender para exibir as ids de cliente para um aplicativo de suporte. Como o usuário digita caracteres diferentes na caixa de texto, itens diferentes serão mostradas abaixo com base na sua entrada. Os usuários podem selecionar a id do cliente desejado.

Usar o AutoCompleteExtender dentro de uma página ASP.NET AJAX requer que o assembly de AjaxControlToolkit ser adicionado à pasta da Lixeira do site. Após a adição de assembly do Kit de ferramentas, você desejará fazer referência a ele no Web. config para que os controles que ele contém estão disponíveis para todas as páginas em um aplicativo. Isso pode ser feito adicionando a seguinte marca dentro do Web. config &lt;controles&gt; marca:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

Em casos em que você só precisa usar o controle em uma página específica, você poderá referenciá-lo adicionando a diretiva de referência para a parte superior de uma página, como mostrado a seguir, em vez de atualizar Web. config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Usando o controle AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Figura 4**: usando o controle AutoCompleteExtender.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-web-services/_static/image12.png))


Depois que o site tiver sido configurado para usar o Kit de ferramentas do ASP.NET AJAX, um controle AutoCompleteExtender pode ser adicionado à página muito que você adicionaria um controle de servidor ASP.NET regular. Listar 19 mostra um exemplo de como usar o controle para chamar um serviço Web.

**Listando 19. Usando o controle AutoCompleteExtender de kit de ferramentas do ASP.NET AJAX.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

O AutoCompleteExtender tem várias propriedades diferentes, incluindo as propriedades padrão de ID e runat encontradas em controles de servidor. Além dessas, ele permite que você defina o número de caracteres um tipos de usuário final antes do serviço Web é consultado para dados. A propriedade MinimumPrefixLength mostrada na listagem 19 faz com que o serviço seja chamado a cada vez que um caractere é digitado na caixa de texto. Você desejará cuidado definir esse valor, pois cada vez que o usuário digita um caractere, o serviço da Web será chamado para pesquisar valores que correspondem aos caracteres na caixa de texto. O serviço Web chamar, bem como o método da Web de destino é definido usando as propriedades ServicePath e ServiceMethod respectivamente. Por fim, a propriedade TargetControlID identifica qual caixa de texto para conectar-se com o controle AutoCompleteExtender.

O serviço Web que está sendo chamada deve ter o atributo ScriptService aplicado conforme discutido anteriormente, e o método da Web de destino deve aceitar dois parâmetros nomeados prefixText e contagem. O parâmetro prefixText representa os caracteres digitados pelo usuário final e o parâmetro de contagem representa quantos itens para retornar (o padrão é 10). Listar 20 mostra um exemplo do método da Web GetCustomerIDs chamado pelo controle AutoCompleteExtender mostrado anteriormente listando 19. O método da Web chama um método de camada de negócios que por sua vez chama um camada de dados método que manipula a filtragem de dados e retornar os resultados de correspondência. O código para o método de camada de dados é mostrado na listagem 21.

**Listando 20. Filtrando dados enviados do controle AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Listando 21. Filtrando resultados com base na entrada do usuário final.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Conclusão

ASP.NET AJAX fornece excelente suporte para chamar serviços Web sem escrever tanto código JavaScript personalizado para lidar com as mensagens de solicitação e resposta. Neste artigo, você viu como habilitar AJAX .NET Web Services para habilitá-los para processar mensagens JSON e como definir os proxies JavaScript usando o controle ScriptManager. Também vimos como JavaScript proxies podem ser usados para chamar serviços da Web, lidar com tipos simples e complexos e lidar com falhas. Por fim, você viu como métodos de página podem ser usados para simplificar o processo de criação e fazer chamadas de serviço Web e como o controle AutoCompleteExtender pode fornecer ajuda para os usuários finais conforme eles digitar. Embora o UpdatePanel disponível no ASP.NET AJAX será o controle ideal para muitos programadores AJAX devido à sua simplicidade, saber como chamar serviços da Web por meio de proxies JavaScript pode ser útil em muitos aplicativos.

## <a name="bio"></a>Biografia do

Dan Wahlin (Microsoft Most Valuable Professional para ASP.NET e XML Web Services) é um consultor .NET de instrutor e arquitetura de desenvolvimento no treinamento de Interface ([http://www.interfacett.com](http://www.interfacett.com)). Dan fundada o XML para o site da Web de desenvolvedores do ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), está em agência no apresentador INETA e participa de várias conferências. Dan autoria conjunta Professional Windows DNA (Wrox), ASP.NET: dicas, tutoriais e código (Sams), ASP.NET 1.1 Insider soluções, Professional ASP.NET 2.0 AJAX (Wrox), ASP.NET 2.0 MVP experimenta e XML criado para desenvolvedores do ASP.NET (Sams). Quando ele não está escrevendo código, artigos ou manuais, Dan gosta de escrever e música de gravação e execução Golfe e basquete com sua mulher e filhos.

Scott Cate trabalha com tecnologias Microsoft Web desde 1997 e é presidente da myKB.com ([www.myKB.com](http://www.myKB.com)) onde ele é especializada em escrever ASP.NET com base em aplicativos voltados para soluções de Software da Base de dados de Conhecimento. Scott pode ser contatado via email em [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou em seu blog [ScottCate.com](http://ScottCate.com)

>[!div class="step-by-step"]
[Anterior](understanding-asp-net-ajax-localization.md)
[Próximo](understanding-asp-net-ajax-debugging-capabilities.md)
