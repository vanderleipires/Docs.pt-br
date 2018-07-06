---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Noções básicas sobre serviços Web do ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Serviços Web são parte integrante do .NET framework que fornecem uma solução de plataforma cruzada para a troca de dados entre sistemas distribuídos. Embora o Web...
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: b98ef4c27ab7b4b729e9e5b68e7d2642a6418ab6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838595"
---
<a name="understanding-aspnet-ajax-web-services"></a>Noções básicas sobre serviços Web do ASP.NET AJAX
====================
por [Scott Cate](https://github.com/scottcate)

[Baixar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Serviços Web são parte integrante do .NET framework que fornecem uma solução de plataforma cruzada para a troca de dados entre sistemas distribuídos. Embora os serviços da Web normalmente são usados para permitir que diferentes sistemas operacionais, modelos de objeto e linguagens de programação para enviar e receber dados, eles também podem ser usados para injetar dados em uma página ASP.NET AJAX ou enviar dados de uma página para um sistema back-end dinamicamente. Tudo isso pode ser feito sem recorrer para operações de postback.


## <a name="calling-web-services-with-aspnet-ajax"></a>Chamando serviços Web com AJAX ASP.NET

Dan Wahlin

Serviços Web são parte integrante do .NET framework que fornecem uma solução de plataforma cruzada para a troca de dados entre sistemas distribuídos. Embora os serviços da Web normalmente são usados para permitir que diferentes sistemas operacionais, modelos de objeto e linguagens de programação para enviar e receber dados, eles também podem ser usados para injetar dados em uma página ASP.NET AJAX ou enviar dados de uma página para um sistema back-end dinamicamente. Tudo isso pode ser feito sem recorrer para operações de postback.

Enquanto o controle UpdatePanel do AJAX ASP.NET fornece uma maneira simples de AJAX habilitar qualquer página do ASP.NET, pode haver ocasiões em que você precisa acessar dinamicamente os dados no servidor sem usar um UpdatePanel. Neste artigo, você verá como fazer isso criando e consumindo serviços Web em páginas ASP.NET AJAX.

Este artigo se concentra na funcionalidade disponível em extensões do AJAX do ASP.NET core, bem como um controle de serviço da Web habilitado no ASP.NET AJAX Toolkit chamado o AutoCompleteExtender. Os tópicos abordados incluem a definição de serviços de Web habilitados para AJAX, criando proxies de cliente e chamar os serviços Web com JavaScript. Você também verá como as chamadas de serviço da Web podem ser feitas diretamente para os métodos de página ASP.NET.

## <a name="web-services-configuration"></a>Configuração de serviços da Web

Quando um novo projeto de Site da Web é criado com o Visual Studio 2008, o arquivo Web. config tem um número de novas adições que pode não ser familiar aos usuários de versões anteriores do Visual Studio. Algumas dessas modificações mapeiam o prefixo "asp" para controles do ASP.NET AJAX para que possam ser usados nas páginas enquanto outros definem HttpHandlers e HttpModules necessários. A listagem 1 mostra as modificações feitas a `<httpHandlers>` elemento no Web. config que afeta as chamadas de serviço Web. O padrão de que HttpHandler é usado para processar chamadas de. asmx é removido e substituído por uma classe ScriptHandlerFactory localizada no assembly Extensions. Extensions contém toda a funcionalidade de núcleo usada pelo ASP.NET AJAX.

**Listagem 1. Configuração do manipulador do ASP.NET AJAX Web Service**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Essa substituição HttpHandler é feita para permitir que o objeto notação JSON (JavaScript) chamadas sejam feitas de páginas ASP.NET AJAX para serviços Web do .NET usando um proxy de serviço Web de JavaScript. ASP.NET AJAX envia as mensagens JSON para serviços Web em vez das chamadas simples (SOAP Object Access Protocol) padrão normalmente associadas aos serviços da Web. Isso resulta na solicitação menor e mensagens de resposta geral. Ele também permite processamento mais eficiente do lado do cliente de dados, pois a biblioteca JavaScript do AJAX ASP.NET é otimizada para trabalhar com objetos JSON. Listagem 2 e listagem 3 mostra exemplos de mensagens de solicitação e resposta do serviço Web serializados em formato JSON. A mensagem de solicitação mostrada na listagem 2 passa um parâmetro de país com um valor de "Bélgica", enquanto a mensagem de resposta na listagem 3 passa uma matriz de objetos Customer e suas propriedades associadas.

**Listagem 2. Mensagem de solicitação de serviço Web serializada para JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] o nome da operação é definido como parte da URL para o serviço web. Além disso, mensagens de solicitação não são sempre enviadas via JSON. Serviços Web podem utilizar o atributo ScriptMethod com o parâmetro UseHttpGet definido como true, o que faz com que os parâmetros a serem passados por meio de um os parâmetros de cadeia de caracteres de consulta.*


**Listagem 3. Mensagem de resposta do serviço Web serializada para JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

Na próxima seção, você verá como criar serviços da Web capaz de lidar com mensagens de solicitação JSON e responder com tipos complexos e simples.

## <a name="creating-ajax-enabled-web-services"></a>Criar serviços Web habilitados para AJAX

A estrutura ASP.NET AJAX fornece várias maneiras diferentes para chamar serviços Web. Você pode usar o controle AutoCompleteExtender (disponível no ASP.NET AJAX Toolkit) ou JavaScript. No entanto, antes de chamar um serviço você precisa habilitar AJAX-lo para que ele pode ser chamado pelo código de script de cliente.

Se você for novo no ASP.NET Web Services, você encontrará ele simples de criar e habilitar AJAX serviços. O .NET framework tem suporte para a criação de serviços Web do ASP.NET desde seu lançamento inicial em 2002 e o ASP.NET AJAX Extensions oferecem funcionalidade adicional do AJAX que aproveita a conjunto de recursos padrão do .NET framework. 2008 Beta 2 do Visual Studio .NET tem suporte interno para a criação de arquivos do serviço Web. asmx e automaticamente o código associado ao lado de classes é derivada da classe WebService. Conforme você adiciona métodos na classe, você deve aplicar o atributo WebMethod para que eles sejam chamados pelos consumidores de serviço Web.

Listagem 4 mostra um exemplo de como aplicar o atributo WebMethod para um método chamado GetCustomersByCountry().

**Listagem 4. Usando o atributo WebMethod em um serviço Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

O método GetCustomersByCountry() aceita um parâmetro de país e retorna matriz de objetos de um cliente. O valor de país passado para o método é encaminhado para uma classe de camada de negócios que por sua vez chama uma classe da camada de dados para recuperar os dados do banco de dados, preencha as propriedades do objeto cliente com dados e retornar a matriz.

## <a name="using-the-scriptservice-attribute"></a>Usando o atributo ScriptService

Ao adicionar o WebMethod atributo permite que o método GetCustomersByCountry() a ser chamado por clientes que enviam mensagens SOAP padrão para o serviço Web, ela não permite chamadas JSON para ser feita a partir de aplicativos do ASP.NET AJAX fora da caixa. Para permitir chamadas JSON ser feito com que você precisa aplicar a extensão ASP.NET AJAX `ScriptService` atributo à classe de serviço da Web. Isso permite que um serviço Web para enviar mensagens de resposta formatadas usando JSON e script do lado do cliente chamar um serviço por meio do envio de mensagens JSON.

Listagem 5 mostra um exemplo de aplicar o atributo ScriptService para uma classe de serviço Web denominada CustomersService.

**Listagem 5. Usando o atributo ScriptService para habilitar um serviço Web com AJAX**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

O atributo ScriptService atua como um marcador que indica que ele pode ser chamado do código de script do AJAX. Na verdade, ele não manipula todas as tarefas de serialização ou desserialização de JSON que ocorrem nos bastidores. O ScriptHandlerFactory (configurado no Web. config) e outras classes relacionadas para fazer a maior parte do processamento de JSON.

## <a name="using-the-scriptmethod-attribute"></a>Usando o atributo ScriptMethod

O atributo ScriptService é o único atributo do ASP.NET AJAX que deve ser definida em um serviço da Web .NET para que ele a ser usado pelas páginas do ASP.NET AJAX. No entanto, outro atributo chamado ScriptMethod também pode ser aplicado diretamente para os métodos da Web em um serviço. ScriptMethod define três propriedades, incluindo `UseHttpGet`, `ResponseFormat` e `XmlSerializeString`. Alterando os valores dessas propriedades pode ser útil em casos em que o tipo de solicitação aceito por um método da Web precisa ser alterado para GET, quando um método da Web precisa retornar dados XML brutos na forma de um `XmlDocument` ou `XmlElement` objeto ou quando os dados retornados de um  serviço sempre deve ser serializado como XML, em vez de JSON.

A propriedade pode ser usada quando um método da Web deve aceitar de UseHttpGet obter solicitações em vez de solicitações POST. As solicitações são enviadas usando uma URL com parâmetros de entrada do método Web convertido em parâmetros de QueryString. O UseHttpGet propriedade assume false como padrão e só deverá ser definido como `true` quando as operações são conhecidas ser seguras e quando os dados confidenciais não são passados para um serviço Web. Listagem 6 mostra um exemplo de como usar o atributo ScriptMethod com a propriedade UseHttpGet.

**Listagem 6. Usando o atributo ScriptMethod com a propriedade UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Um exemplo dos cabeçalhos enviados quando o método de Web HttpGetEcho mostrado na listagem 6 é chamado são mostrado a seguir:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Além de permitir que os métodos da Web aceitar solicitações HTTP GET, o atributo ScriptMethod também pode ser usado quando respostas XML precisam ser retornado de um serviço em vez de JSON. Por exemplo, um serviço Web pode recuperar um RSS feed de um site remoto e retorná-lo como um objeto XmlDocument ou XmlElement. Processamento do XML dados, em seguida, podem ocorrer no cliente.

Listagem 7 mostra um exemplo de como usar a propriedade ResponseFormat para especificar que os dados XML devem ser retornados de um método Web.

**Listagem 7. Usando o atributo ScriptMethod com a propriedade ResponseFormat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

A propriedade ResponseFormat também pode ser usada junto com a propriedade XmlSerializeString. A propriedade XmlSerializeString tem um valor padrão de false, que significa que todos retornam tipos exceto cadeias de caracteres retornadas de um método Web são serializados como XML quando o `ResponseFormat` estiver definida como `ResponseFormat.Xml`. Quando `XmlSerializeString` é definido como `true`, todos os tipos retornados de um método Web são serializados como XML, incluindo tipos de cadeia de caracteres. Se a propriedade ResponseFormat tem um valor de `ResponseFormat.Json` a propriedade XmlSerializeString será ignorada.

Listagem 8 mostra um exemplo de como usar a propriedade XmlSerializeString para forçar as cadeias de caracteres a ser serializado como XML.

**Listagem 8. Usando o atributo ScriptMethod com a propriedade XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

O valor retornado da chamada do método de Web GetXmlString mostrado na listagem 8 é mostrado a seguir:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Embora o formato JSON padrão minimiza o tamanho total de mensagens de solicitação e resposta e é consumido mais prontamente pelos clientes do ASP.NET AJAX de uma maneira de navegadores, as propriedades ResponseFormat e XmlSerializeString podem ser utilizada ao cliente aplicativos como o Internet Explorer 5 ou superior esperam que os dados XML a ser retornado de um método Web.

## <a name="working-with-complex-types"></a>Trabalhando com tipos complexos

Listagem 5 mostramos um exemplo de retornar um tipo complexo de chamada de cliente de um serviço Web. A classe Customer define vários tipos diferentes de simples internamente como propriedades como FirstName e LastName. Tipos complexos usado como um parâmetro de entrada ou tipo de retorno em um método de Web habilitados para AJAX automaticamente são serializados em JSON antes de serem enviados para o lado do cliente. No entanto, os tipos complexos aninhados (aqueles definidos internamente dentro de outro tipo) não ficam disponíveis para o cliente como objetos autônomos por padrão.

Em casos onde um tipo complexo aninhado usado por um serviço Web também deverá ser usado em uma página de cliente, o atributo GenerateScriptType do AJAX ASP.NET pode ser adicionado ao serviço Web. Por exemplo, a classe de CustomerDetails mostrada na listagem 9 contém propriedades de endereço e sexo que ***representam tipos complexos aninhados.***

**Listagem 9. A classe CustomerDetails mostrada aqui contém dois tipos de complexos aninhados.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Os objetos de endereço e sexo definidos dentro da classe CustomerDetails mostrada na listagem 9 não serão disponibilizados automaticamente para uso no lado do cliente por meio de JavaScript, pois são tipos aninhados (endereço é uma classe e sexo é uma enumeração). Em situações em que um tipo aninhado usado dentro de um serviço Web deve estar disponível no lado do cliente, o atributo GenerateScriptType mencionado anteriormente pode ser usado (consulte a listagem 10). Esse atributo pode ser adicionado várias vezes em casos em que os diferentes tipos complexos aninhados são retornados de um serviço. Ele pode ser aplicado diretamente à classe de serviço Web ou acima de métodos de Web específicos.

**Listagem 10. Usando o atributo GenerateScriptService para definir tipos aninhados que devem estar disponíveis para o cliente.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Aplicando o `GenerateScriptType` atributo para os tipos de serviço Web, o endereço e o gênero será automaticamente disponibilizado para uso pelo código do ASP.NET AJAX JavaScript do lado do cliente. Um exemplo de JavaScript que é automaticamente gerada e enviada ao cliente, adicionando o atributo GenerateScriptType em um serviço Web é mostrado na listagem 11. Você verá como usar tipos complexos aninhados posteriormente neste artigo.

**Listagem 11. Tipos complexos aninhados disponibilizados para uma página ASP.NET AJAX.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Agora que você já viu como criar serviços Web e torná-los acessíveis para páginas ASP.NET AJAX, vamos dar uma olhada em como criar e usar os proxies JavaScript para que os dados podem ser recuperados ou enviados aos serviços da Web.

## <a name="creating-javascript-proxies"></a>Criação de Proxies JavaScript

Chamar um serviço da Web padrão (.NET ou outra plataforma) normalmente envolve a criação de um objeto proxy que protege você contra as complexidades de envio de mensagens de solicitação e resposta SOAP. Com chamadas de Web Service do ASP.NET AJAX, os proxies JavaScript podem ser criados e usados para chamar facilmente serviços sem se preocupar sobre a serialização e desserialização de mensagens JSON. Proxies JavaScript podem ser gerados automaticamente, usando o controle ScriptManager do AJAX ASP.NET.

Criando um proxy JavaScript que pode chamar serviços da Web é realizado usando a propriedade de serviços do ScriptManager. Essa propriedade permite que você defina um ou mais serviços que uma página ASP.NET AJAX pode chamar de forma assíncrona para enviar ou receber dados sem a necessidade de operações de postback. Definir um serviço usando o ASP.NET AJAX `ServiceReference` controle e atribuindo a URL do serviço Web para o controle `Path` propriedade. Listagem 12 mostra um exemplo de como fazer referência a um serviço chamado CustomersService.asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Listagem 12. Definindo um serviço Web usado em uma página ASP.NET AJAX.**

Adicionando uma referência para o CustomersService.asmx por meio do controle ScriptManager faz com que um proxy JavaScript sejam gerados dinamicamente e referenciado pela página. O proxy é inserido usando o &lt;script&gt; marcar e carregados dinamicamente chamando o arquivo CustomersService.asmx e acrescentando /js ao final dele. O exemplo a seguir mostra como o proxy JavaScript é inserido na página quando a depuração está desabilitada no Web. config:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Se você gostaria de ver o código de proxy JavaScript real que é gerado você pode digitar a URL para o serviço de Web do .NET desejada na caixa de endereço do Internet Explorer e acrescentar /js ao final dele.*


Se a depuração está habilitada no Web. config, que uma versão de depuração de proxy JavaScript será inserida na página como mostrado a seguir:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

O proxy JavaScript criado pelo ScriptManager também podem ser inserido diretamente na página, em vez de referenciados usando o &lt;script&gt; atributo src da marca. Isso pode ser feito definindo a propriedade do InlineScript ServiceReference do controle como true (o padrão é false). Isso pode ser útil quando um proxy não é compartilhado por várias páginas e você gostaria de reduzir o número de chamadas de rede feitas no servidor. Quando InlineScript é definido como true, o script de proxy não ser armazenado em cache pelo navegador para que o valor padrão de false é recomendado em casos em que o proxy é usado por várias páginas em um aplicativo ASP.NET AJAX. Um exemplo de como usar a propriedade InlineScript é mostrado a seguir:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Uso de Proxies de JavaScript

Depois que um serviço Web é referenciado por uma página ASP.NET AJAX usando o controle do ScriptManager, pode ser feita uma chamada para o serviço Web e os dados retornados podem ser tratados usando funções de retorno de chamada. Um serviço Web é chamado referenciando seu namespace (se houver), nome de classe e nome do método Web. Todos os parâmetros passados para o serviço Web podem ser definidos juntamente com uma função de retorno de chamada que manipula os dados retornados.

Um exemplo de como usar um proxy JavaScript para chamar um método de Web chamado GetCustomersByCountry() é mostrado na listagem 13. A função GetCustomersByCountry() é chamada quando um usuário final clica em um botão na página.

**Listagem 13. Chamando um serviço Web com um proxy JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

Essa chamada faz referência ao namespace de InterfaceTraining, CustomersService classe e método de Web GetCustomersByCountry definidos no serviço. Ele passa um valor de país obtido de uma caixa de texto, bem como uma função de retorno de chamada chamado OnWSRequestComplete que deve ser invocado quando a chamada de serviço da Web assíncrona retorna. OnWSRequestComplete manipula a matriz de objetos de cliente retornados do serviço e converte-os em uma tabela que é exibida na página. A saída gerada a partir da chamada é mostrada na Figura 1.


[![Associando dados obtidos ao realizar uma chamada AJAX assíncrona para um serviço Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Figura 1**: associação de dados obtido fazendo uma chamada AJAX assíncrona para um serviço Web.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-web-services/_static/image3.png))


Proxies JavaScript também podem fazer chamadas unidirecionais para serviços da Web em casos em que um método da Web deve ser chamado, mas o proxy não deve esperar por uma resposta. Por exemplo, você talvez queira chamar um Web Service para iniciar um processo como um fluxo de trabalho, mas não esperar por um valor de retorno do serviço. Em casos em que uma chamada unidirecional precisa ser feita a um serviço, a função de retorno de chamada, mostrada na listagem 13 simplesmente pode ser omitida. Como nenhuma função de retorno de chamada é definida não esperará o objeto de proxy para o serviço Web retornar dados.

## <a name="handling-errors"></a>Manipulando erros

Retornos de chamada assíncronos para serviços Web podem encontrar diferentes tipos de erros, como a rede está inoperante, o serviço Web está indisponível ou uma exceção que está sendo retornado. Felizmente, os objetos de proxy JavaScript gerados pelo ScriptManager permitem que vários retornos de chamada a ser definido para tratar erros e falhas, além do retorno de chamada bem-sucedido mostrado anteriormente. Uma função de retorno de chamada de erro pode ser definida imediatamente após a função de retorno de chamada padrão na chamada para o método da Web, conforme mostrado na listagem 14.

**Listagem 14. Definir uma função de retorno de chamada do erro e exibição de erros.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Todos os erros que ocorrem quando o serviço Web é chamado disparará a função de retorno de chamada de OnWSRequestFailed() para ser chamado que aceita um objeto que representa o erro como um parâmetro. O objeto de erro expõe várias funções diferentes para determinar a causa do erro, bem como se a chamada atingiu o tempo limite. Listagem 14 mostra um exemplo de como usar as funções de erro diferentes e a Figura 2 mostra um exemplo da saída gerada pelas funções.


[![Saída gerada ao chamar funções de erro do ASP.NET AJAX.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Figura 2**: saída gerada ao chamar funções de erro do ASP.NET AJAX.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Manipulação de dados XML retornados de um serviço Web

Vimos anteriormente como um método Web poderia retornar dados XML brutos, usando o atributo ScriptMethod, juntamente com sua propriedade ResponseFormat. Quando ResponseFormat for definido como ResponseFormat.Xml, os dados retornados do serviço Web são serializados como XML em vez de JSON. Isso pode ser útil quando os dados XML devem ser passados diretamente para o cliente para o processamento usando JavaScript ou XSLT. No momento, o Internet Explorer 5 ou superior fornece o melhor modelo de objeto do lado do cliente para análise e filtragem de dados XML devido a seu suporte interno para o MSXML.

Recuperando dados XML de um serviço Web não é diferente de recuperar outros tipos de dados. Iniciar, invocando o proxy do JavaScript para definir uma função de retorno de chamada e chamar a função apropriada. Depois que a chamada retorna você pode processar os dados na função de retorno de chamada.

Listagem 15 mostra um exemplo de como chamar um método de Web chamado GetRssFeed() que retorna um objeto XmlElement. GetRssFeed() aceita um único parâmetro que representa a URL para o RSS feed para recuperar.

**Listagem 15. Trabalhando com dados XML retornados de um serviço Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

Este exemplo passa uma URL para um RSS feed e processa os dados XML retornados na função OnWSRequestComplete(). OnWSRequestComplete() primeiro verifica para ver se o navegador é saber se o analisador MSXML está disponível, o Internet Explorer. Se for, uma instrução XPath é usada para localizar todos os &lt;item&gt; marcas dentro do RSS feed. Cada item, em seguida, fazer a iteração com e os respectivos &lt;title&gt; e &lt;link&gt; marcas são localizadas e processadas para exibir dados de cada item. Figura 3 mostra um exemplo da saída gerada de fazer um ASP.NET AJAX chamar por meio de um proxy JavaScript para o método de Web GetRssFeed().

## <a name="handling-complex-types"></a>Tratamento de tipos complexos

Tipos complexos aceito ou retornado por um serviço Web são automaticamente expostos por meio de um proxy JavaScript. No entanto, os tipos complexos aninhados não são diretamente acessíveis no lado do cliente, a menos que o atributo GenerateScriptType é aplicado ao serviço, conforme discutido anteriormente. Por que você deseja usar um tipo complexo aninhado no lado do cliente?

Para responder essa pergunta, suponha que uma página ASP.NET AJAX exibe os dados do cliente e permite que os usuários finais atualizar o endereço do cliente. Se o serviço da Web Especifica que o tipo de endereço (um tipo complexo definido dentro de uma classe CustomerDetails) possa ser enviado ao cliente, em seguida, o processo de atualização pode ser dividido em funções separadas melhor para reutilização de códigos.


[![Saída de criação de chamar um serviço Web que retorna dados RSS.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Figura 3**: criação de chamar um serviço Web que retorna dados RSS de saída.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-web-services/_static/image9.png))


Listagem 16 mostra um exemplo de código do lado do cliente que invoca um objeto de endereço definido em um namespace de modelo, preenche com dados atualizados e o atribui a propriedade de endereço de um objeto CustomerDetails. O objeto CustomerDetails é então passado para o serviço Web para processamento.

**Listagem 16. Usando tipos complexos aninhados**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Criando e usando os métodos de página

Serviços Web fornecem uma maneira excelente para expor serviços utilizáveis novamente a uma variedade de clientes, incluindo páginas ASP.NET AJAX. No entanto, pode haver casos em que uma página precisa recuperar dados que já não usados ou compartilhados por outras páginas. Nesse caso, tornar um arquivo. asmx para permitir que a página para acessar os dados pode parecer um exagero, pois o serviço é usado somente por uma única página.

ASP.NET AJAX fornece outro mecanismo para fazer chamadas de tipo de serviço Web sem criar arquivos. asmx de autônomo. Isso é feito usando uma técnica conhecida como "métodos de página". Métodos de página são métodos estáticos (compartilhado no VB.NET) inseridos diretamente em um arquivo de página ou ao lado do código que têm o atributo WebMethod aplicado a eles. Aplicando o atributo WebMethod eles podem ser chamados usando um objeto JavaScript especial chamado PageMethods que é criada dinamicamente em tempo de execução. O objeto PageMethods atua como um proxy que protege você contra o processo de serialização/desserialização JSON. Observe que, para usar o objeto PageMethods você deve definir propriedade de EnablePageMethods do ScriptManager como true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Listagem 17 mostra um exemplo de definição de dois métodos de página em uma classe do lado do código do ASP.NET. Esses métodos recuperam dados de uma classe de camada de negócios localizada no aplicativo do\_pasta de código do site.

**Listagem 17. Definindo os métodos de página.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Quando o ScriptManager detecta a presença dos métodos Web na página, ele gera uma referência dinâmica para o objeto PageMethods mencionado anteriormente. Chamando um método Web é realizado pela referência à classe PageMethods seguida do nome do método e quaisquer dados de parâmetro necessários que devem ser passados. Listagem 18 mostra exemplos de como chamar os dois métodos de página mostrados anteriormente.

**Listagem 18. Chamando métodos de página com o objeto PageMethods JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Usando o objeto PageMethods é muito semelhante ao uso de um objeto de proxy do JavaScript. Você primeiro especificar todos os dados de parâmetro que devem ser passados para o método de página e, em seguida, definem a função de retorno de chamada que deve ser chamada quando a chamada assíncrona retorna. Também pode ser especificado um retorno de chamada de falha (consulte a listagem 14 para obter um exemplo de tratamento de falhas).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>O AutoCompleteExtender e o Kit de ferramentas do ASP.NET AJAX

O Kit de ferramentas do ASP.NET AJAX (disponível no [ http://ajax.asp.net ](http://ajax.asp.net)) oferece vários controles que podem ser usados para acessar serviços Web. Especificamente, o Kit de ferramentas contém um controle útil chamado `AutoCompleteExtender` que pode ser usado para chamar serviços Web e mostrar dados em páginas sem escrever qualquer código JavaScript em todos os.

O controle AutoCompleteExtender pode ser usado para estender a funcionalidade existente de uma caixa de texto e ajudam os usuários mais localizar facilmente os dados que estão procurando. Como ele digita em uma caixa de texto o controle pode ser usado para consultar um serviço Web e mostra os resultados abaixo da caixa de texto dinamicamente. Figura 4 mostra um exemplo de como usar o controle AutoCompleteExtender para exibir as ids de cliente para um aplicativo de suporte. Conforme o usuário digita caracteres diferentes na caixa de texto, os itens diferentes serão exibidos abaixo dele com base em suas entradas. Os usuários podem selecionar a id do cliente desejado.

Usar o AutoCompleteExtender dentro de uma página ASP.NET AJAX exige que o assembly de AjaxControlToolkit ser adicionado à pasta de compartimento do site. Depois que o assembly do Kit de ferramentas tiver sido adicionado, você desejará fazer referência a ele em Web. config para que os controles que ele contém estão disponíveis para todas as páginas em um aplicativo. Isso pode ser feito adicionando a seguinte marca dentro do Web. config &lt;controles&gt; marca:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

Em casos em que você só precisa usar o controle em uma página específica, você pode referenciá-la adicionando a diretiva de referência na parte superior de uma página, conforme mostrado a seguir em vez de atualizar Web. config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Usando o controle AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Figura 4**: usando o controle AutoCompleteExtender.  ([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-web-services/_static/image12.png))


Depois que o site foi configurado para usar o Kit de ferramentas do ASP.NET AJAX, um controle AutoCompleteExtender pode ser adicionado na página muito como você adicionaria um controle de servidor ASP.NET regular. Listagem 19 mostra um exemplo de como usar o controle para chamar um serviço Web.

**Listagem 19. Usando o controle AutoCompleteExtender de kit de ferramentas do ASP.NET AJAX.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

O AutoCompleteExtender tem várias propriedades diferentes, incluindo as propriedades padrão de ID e runat encontradas nos controles de servidor. Além disso, ele permite que você defina quantos caracteres de um tipo de usuário final antes que o serviço Web é consultada para obter dados. A propriedade MinimumPrefixLength mostrada na listagem 19 faz com que o serviço a ser chamado sempre que um caractere é digitado na caixa de texto. Você vai querer ter cuidado para definir esse valor, pois cada vez que o usuário digita um caractere, o serviço Web será chamado para pesquisar valores que correspondem aos caracteres na caixa de texto. O serviço Web para chamar, bem como o método da Web de destino é definido usando as propriedades ServicePath e ServiceMethod respectivamente. Por fim, a propriedade TargetControlID identifica qual caixa de texto para vincular o controle AutoCompleteExtender.

O serviço Web que está sendo chamado deve ter o atributo ScriptService aplicado conforme discutido anteriormente e o método da Web de destino deve aceitar dois parâmetros nomeados prefixText e contagem. O parâmetro prefixText representa os caracteres digitados pelo usuário final e o parâmetro de contagem que representa quantos itens devem para retornar (o padrão é 10). Listagem 20 mostra um exemplo do método Web GetCustomerIDs chamado pelo controle AutoCompleteExtender mostrado anteriormente na listagem 19. O método da Web chama um método de camada de negócios que por sua vez chama da camada de dados método que manipula a filtragem dos dados e retornar os resultados de correspondência. O código para o método de camada de dados é mostrado na listagem 21.

**Listagem 20. Filtrando dados enviados do controle AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Listagem 21. Filtrando os resultados com base na entrada do usuário final.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Conclusão

ASP.NET AJAX fornece excelente suporte para chamar serviços da Web sem escrever uma grande quantidade de código JavaScript personalizado para manipular as mensagens de solicitação e resposta. Neste artigo, você viu como habilitar com AJAX .NET Web Services para habilitá-los para processar as mensagens JSON e como definir os proxies JavaScript usando o controle ScriptManager. Você também viu como o JavaScript proxies podem ser usados para chamar serviços Web, lidar com tipos complexos e simples e lidar com falhas. Por fim, você viu como os métodos de página podem ser usados para simplificar o processo de criação e fazer chamadas de serviço Web e como o controle AutoCompleteExtender pode fornecer ajuda aos usuários finais enquanto digitam. Embora o UpdatePanel disponível no ASP.NET AJAX certamente será o controle preferencial para muitos programadores AJAX devido à sua simplicidade, a saber como chamar serviços da Web por meio de proxies JavaScript pode ser útil em muitos aplicativos.

## <a name="bio"></a>Biografia

Dan Wahlin (Microsoft Most Valuable Professional do ASP.NET e XML Web Services) é desenvolvimento instrutor e arquitetura consultor .NET no treinamento técnico de Interface ([http://www.interfacett.com](http://www.interfacett.com)). Dan fundou o XML para o site da Web de desenvolvedores do ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), está na agência do palestrante da INETA e dá palestras em várias conferências. Dan é co-autor Professional Windows DNA (Wrox), ASP.NET: dicas, tutoriais e código (Sams), soluções do ASP.NET 1.1 Insider, Professional ASP.NET 2.0 AJAX (Wrox), Hacks do ASP.NET 2.0 MVP e XML criado para desenvolvedores do ASP.NET (Sams). Quando ele não está escrevendo código, artigos ou livros, Dan gosta de escrever e gravando música e reprodução de Golfe e o basquete com sua esposa e filhos.

Scott Cate tem trabalhado com tecnologias Web Microsoft desde 1997 e é o presidente da myKB.com ([www.myKB.com](http://www.myKB.com)) onde ele é especialista na escrita de ASP.NET com base em aplicativos com foco em soluções de Software da Base de dados de Conhecimento. Scott pode ser contatado através do email [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) ou em seu blog em [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-localization.md)
> [Próximo](understanding-asp-net-ajax-debugging-capabilities.md)
