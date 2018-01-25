---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
title: "Cache de dados na inicialização do aplicativo (c#) | Microsoft Docs"
author: rick-anderson
description: "Em um aplicativo Web alguns dados serão usados com frequência e alguns dados serão usados com pouca frequência. Podemos melhorar o desempenho do nosso aplicativo b ASP.NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 22ca8efa-7cd1-45a7-b9ce-ce6eb3b3ff95
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-cs
msc.type: authoredcontent
ms.openlocfilehash: 3a618ad702763a59b87336784afd1cb74de06d4c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2018
---
<a name="caching-data-at-application-startup-c"></a>Cache de dados na inicialização do aplicativo (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](caching-data-at-application-startup-cs/_static/datatutorial60cs1.pdf)

> Em um aplicativo Web alguns dados serão usados com frequência e alguns dados serão usados com pouca frequência. Podemos melhorar o desempenho do nosso aplicativo ASP.NET com antecedência carregando os dados usados com frequência, uma técnica conhecida como. Este tutorial demonstra uma abordagem para carregar pró-ativo, que é carregar dados em cache na inicialização do aplicativo.


## <a name="introduction"></a>Introdução

Os tutoriais anteriores dois pesquisados no cache de dados na apresentação e camadas de armazenamento em cache. Em [cache de dados com o ObjectDataSource](caching-data-with-the-objectdatasource-cs.md), analisamos usando o s ObjectDataSource recursos de cache de dados na camada de apresentação de cache. [Cache de dados na arquitetura](caching-data-in-the-architecture-cs.md) examinou o armazenamento em cache em uma camada de cache novo e separado. Ambos esses tutoriais usados *carregamento reativo* ao trabalhar com o cache de dados. Com o carregamento reativo, cada vez que os dados são solicitados, o sistema verifica primeiro se ele s no cache. Caso contrário, ele obtém os dados da fonte de origem, como o banco de dados e os armazena no cache. A principal vantagem de carregamento reativo é a facilidade de implementação. Uma das suas desvantagens é seu desempenho irregular em solicitações. Imagine que uma página que usa a camada de cache do tutorial anterior para exibir informações sobre o produto. Quando esta página é visitada pela primeira vez ou visitou pela primeira vez depois que os dados em cache foi removidos devido a restrições de memória ou a expiração especificada ter sido atingido, os dados devem ser recuperados do banco de dados. Portanto, essas solicitações de usuários levará mais tempo do que as solicitações de usuários que podem ser atendidas pelo cache.

*Carregamento pró-ativo* fornece uma estratégia de gerenciamento de cache alternativo que suaviza o desempenho em solicitações ao carregar os dados em cache antes de ele s necessários. Normalmente, o carregamento pró-ativo usa algum processo que periodicamente verifica ou é notificado quando houve uma atualização para os dados subjacentes. Esse processo, em seguida, atualiza o cache para mantê-la atualizada. Carregamento pró-ativo é especialmente útil se os dados subjacentes for proveniente de uma conexão lenta do banco de dados, um serviço Web ou alguma outra fonte de dados muito lento. Mas essa abordagem proativa carregamento é mais difícil de implementar, pois ela requer criar, gerenciar e implantar um processo para verificar as alterações e atualizar o cache.

Outro tipo de carregamento proativo e o tipo que vamos explorar neste tutorial, é carregar dados em cache na inicialização do aplicativo. Essa abordagem é especialmente útil para armazenar em cache dados estáticos, como os registros em tabelas de pesquisa do banco de dados.

> [!NOTE]
> Para obter uma visão mais detalhada as diferenças entre carregamento proativo e reativo, bem como listas de profissionais, contras e recomendações de implementação, consulte o [gerenciar o conteúdo de um Cache](https://msdn.microsoft.com/library/ms978503.aspx) seção o [ Cache de guia de arquitetura para aplicativos do .NET Framework](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Etapa 1: Determinar quais dados em Cache na inicialização do aplicativo

Os exemplos de cache usando o carregamento reativo examinamos no trabalho dois tutoriais anteriores bem com dados que podem ser alteradas periodicamente e não é demorado exorbitantly para gerar. Mas se os dados em cache nunca mudarem, a expiração usada pelo carregamento reativo supérflua. Da mesma forma, se os dados sejam armazenados em cache levam um tempo muito longo para gerar, esses usuários cujas solicitações localizar esvaziar o cache será necessário que suportar uma longa espera enquanto os dados subjacentes são recuperados. Considere a possibilidade de cache de dados estáticos e dados que levam muito tempo para gerar na inicialização do aplicativo.

Enquanto os bancos de dados têm muitos dinâmico, valores alterados com frequência, a maioria também tem uma quantidade razoável de dados estáticos. Por exemplo, praticamente todos os modelos de dados tem uma ou mais colunas que contêm um valor específico em um conjunto fixo de opções. Um `Patients` tabela de banco de dados pode ter um `PrimaryLanguage` coluna, cujo conjunto de valores pode estar em inglês, espanhol, francês, russo, japonês e assim por diante. Muitas vezes, esses tipos de colunas são implementados usando *tabelas de pesquisa*. Em vez de armazenar a cadeia de caracteres em inglês ou francês no `Patients` tabela, uma segunda tabela é criada e possui, normalmente, duas colunas - um identificador exclusivo e uma descrição de cadeia de caracteres - com um registro para cada valor possível. O `PrimaryLanguage` coluna o `Patients` tabela armazena o identificador exclusivo correspondente na tabela de pesquisa. Na Figura 1, paciente idioma principal do John Doe s seja o inglês, enquanto Ed Johnson é russo.


![A tabela de idiomas é uma tabela de pesquisa usada pela tabela pacientes](caching-data-at-application-startup-cs/_static/image1.png)

**Figura 1**: O `Languages` tabela é uma tabela de pesquisa usado pelo `Patients` tabela


A interface do usuário para edição ou criação de um novo paciente inclui uma lista suspensa de idiomas permitidos preenchida por registros no `Languages` tabela. Sem cache, cada vez que esta interface é visitado o sistema deve consultar o `Languages` tabela. Isso é desnecessário e desnecessários como valores de tabela de pesquisa é alterado com muita frequência, nunca.

Podemos pode armazenar em cache o `Languages` dados usando as mesmas técnicas de carregamento reativo examinadas nos tutoriais anteriores. No entanto, carregar reativo, usa uma expiração baseada em tempo, não é necessário para dados da tabela de pesquisa estática. Enquanto o cache usando o carregamento reativo seria melhor do que nenhum cache em todos os, a melhor abordagem seria proativamente carregar os dados da tabela de pesquisa no cache na inicialização do aplicativo.

Este tutorial examinaremos como dados de tabela de pesquisa de cache e outras informações estáticas.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Etapa 2: Examinar os diferentes modos para dados armazenados em Cache

Informações podem ser programaticamente armazenados em cache em um aplicativo ASP.NET usando uma variedade de abordagens. Nós já viu como usar o cache de dados nos tutoriais anteriores. Como alternativa, objetos podem ser programaticamente armazenados em cache usando *membros estáticos* ou *o estado do aplicativo*.

Ao trabalhar com uma classe, normalmente a classe deve primeiro ser instanciada antes de seus membros podem ser acessados. Por exemplo, para invocar um método de uma das classes em nossa camada de lógica comercial, podemos deve primeiro criar uma instância da classe:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample1.cs)]

Antes de nós pode invocar *SomeMethod* ou trabalhar com *SomeProperty*, podemos deve primeiro criar uma instância da classe usando o `new` palavra-chave. *SomeMethod* e *SomeProperty* estão associados uma determinada instância. O tempo de vida desses membros está ligado ao tempo de vida de seu objeto associado. *Membros estáticos*, por outro lado, são variáveis, propriedades e métodos que são compartilhados entre *todos os* instâncias da classe e, consequentemente, têm um tempo de vida, contanto que a classe. Membros estáticos são indicados pela palavra-chave `static`.

Além dos membros estáticos, dados podem ser armazenados em cache usando o estado do aplicativo. Cada aplicativo ASP.NET mantém uma coleção de nome/valor que s compartilhada entre todos os usuários e as páginas do aplicativo. Essa coleção pode ser acessada usando o [ `HttpContext` classe](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` propriedade](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)e usados em uma classe de code-behind de páginas ASP.NET da seguinte forma:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample2.cs)]

O cache de dados fornece uma API muito mais rica para cache de dados, fornecendo mecanismos para expirações baseada em tempo e dependência, prioridades de item de cache e assim por diante. Com membros estáticos e estado do aplicativo, esses recursos devem ser adicionados manualmente pelo desenvolvedor de página. Ao armazenar em cache dados na inicialização do aplicativo para o tempo de vida do aplicativo, no entanto, as vantagens do cache s de dados são sentidas. Neste tutorial, examinaremos o código que usa todas as três técnicas para armazenar em cache dados estáticos.

## <a name="step-3-caching-thesupplierstable-data"></a>Etapa 3: Cache de`Suppliers`dados da tabela

A Northwind tabelas de banco de dados é var implementado para data não incluem quaisquer tabelas de pesquisa tradicionais. O quatro DataTables implementado em nosso DAL todas as tabelas de modelo cujos valores são não-estático. Em vez de gastar tempo para adicionar uma nova DataTable a DAL e, em seguida, uma nova classe e métodos para BLL, para este tutorial permitem s apenas suponhamos que a `Suppliers` dados da tabela s são estáticos. Portanto, esses dados na inicialização do aplicativo foi em cache.

Para começar, crie uma nova classe chamada `StaticCache.cs` no `CL` pasta.


![Criar a classe StaticCache.cs na pasta CL](caching-data-at-application-startup-cs/_static/image2.png)

**Figura 2**: criar o `StaticCache.cs` classe no `CL` pasta


Precisamos adicionar um método que carrega os dados na inicialização para o armazenamento em cache apropriada, bem como métodos que retornam dados desse cache.


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample3.cs)]

O código acima usa uma variável de membro estático, `suppliers`, para armazenar os resultados do `SuppliersBLL` classe s `GetSuppliers()` método, que é chamado a partir de `LoadStaticCache()` método. O `LoadStaticCache()` método deve ser chamado durante o início do aplicativo s. Depois que esses dados forem carregados na inicialização do aplicativo, qualquer página que precisa para trabalhar com dados do fornecedor pode chamar o `StaticCache` classe s `GetSuppliers()` método. Portanto, a chamada para o banco de dados para obter os fornecedores ocorre apenas uma vez, no início do aplicativo.

Em vez de usar uma variável de membro estático como o armazenamento em cache, pode também usamos o estado do aplicativo ou o cache de dados. O código a seguir mostra a classe alterada para usar o estado do aplicativo:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample4.cs)]

Em `LoadStaticCache()`, as informações do fornecedor são armazenadas para a variável de aplicativo *chave*. -S é retornado como o tipo apropriado (`Northwind.SuppliersDataTable`) de `GetSuppliers()`. Enquanto o estado do aplicativo pode ser acessado de classes code-behind de páginas ASP.NET usando `Application["key"]`, da arquitetura que devemos usar `HttpContext.Current.Application["key"]` para obter atual `HttpContext`.

Da mesma forma, o cache de dados pode ser usado como um repositório de cache, como mostra o seguinte código:


[!code-csharp[Main](caching-data-at-application-startup-cs/samples/sample5.cs)]

Para adicionar um item para o cache de dados com nenhuma expiração baseada em tempo, use o `System.Web.Caching.Cache.NoAbsoluteExpiration` e `System.Web.Caching.Cache.NoSlidingExpiration` valores como parâmetros de entrada. Essa sobrecarga específica do cache de dados s `Insert` método foi selecionado para que seria possível especificar o *prioridade* do item de cache. A prioridade é usada para determinar quais itens serão eliminados do cache quando há pouca memória disponível. Aqui usamos a prioridade `NotRemovable`, que garante que este item de cache ganha t ser eliminado.

> [!NOTE]
> Este download tutorial s implementa o `StaticCache` classe usando a abordagem de variável de membro estático. O código para as técnicas de cache de estado e os dados de aplicativo está disponível nos comentários no arquivo de classe.


## <a name="step-4-executing-code-at-application-startup"></a>Etapa 4: Executando código na inicialização do aplicativo

Para executar código quando um aplicativo da web é iniciado pela primeira vez, é preciso criar um arquivo especial chamado `Global.asax`. Esse arquivo pode conter os manipuladores de eventos de aplicativo-sessão-, e eventos em nível de solicitação e é aqui onde podemos adicionar código que será executado sempre que o aplicativo for iniciado.

Adicionar o `Global.asax` arquivo para o diretório raiz da web aplicativo s clicando duas vezes no nome do projeto de site no Visual Studio s Gerenciador de soluções e escolha Adicionar Novo Item. Na caixa de diálogo Adicionar Novo Item, selecione o tipo de item da classe de aplicativo Global e, em seguida, clique no botão Adicionar.

> [!NOTE]
> Se você já tiver um `Global.asax` arquivo em seu projeto, a classe de aplicativo Global, tipo de item não será listado na caixa de diálogo Adicionar Novo Item.


[![Adicione o arquivo global. asax ao seu diretório de raiz de s do aplicativo da Web](caching-data-at-application-startup-cs/_static/image4.png)](caching-data-at-application-startup-cs/_static/image3.png)

**Figura 3**: adicionar o `Global.asax` arquivo ao seu aplicativo Web s diretório raiz ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-cs/_static/image5.png))


O padrão `Global.asax` arquivo modelo inclui cinco métodos em um servidor- `<script>` marca:

- **`Application_Start`**executa quando o aplicativo web é iniciado pela primeira vez
- **`Application_End`**é executado quando o aplicativo está sendo desligado
- **`Application_Error`**sempre que uma exceção sem tratamento alcança o aplicativo é executado
- **`Session_Start`**executa quando é criada uma nova sessão
- **`Session_End`**é executado quando uma sessão expirou ou foi abandonada

O `Application_Start` manipulador de eventos é chamado apenas uma vez durante um ciclo de vida do aplicativo s. O aplicativo inicia na primeira vez um recurso do ASP.NET é solicitado do aplicativo e continuará a ser executado até que o aplicativo é reiniciado, o que pode ocorrer ao modificar o conteúdo do `/Bin` pasta, modificar `Global.asax`, modificando o o conteúdo no `App_Code` pasta ou modificando o `Web.config` arquivo, entre outras causas. Consulte [visão geral do ciclo de vida de aplicativos ASP.NET](https://msdn.microsoft.com/library/ms178473.aspx) para uma discussão mais detalhada sobre o ciclo de vida do aplicativo.

Para esses tutoriais que apenas precisamos adicionar código para o `Application_Start` método, portanto à vontade remover os outros. Em `Application_Start`, simplesmente chamar o `StaticCache` classe s `LoadStaticCache()` método, que irá carregar e armazenar em cache as informações do fornecedor:


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample6.aspx)]

Tudo lá que s é a ele! Na inicialização do aplicativo, o `LoadStaticCache()` método capturar as informações do fornecedor de BLL e armazená-lo em uma variável de membro estático (ou qualquer cache armazenar você estiver usando no `StaticCache` classe). Para verificar se esse comportamento, defina um ponto de interrupção `Application_Start` método e execute seu aplicativo. Observe que o ponto de interrupção é atingido durante a inicialização do aplicativo. As solicitações subsequentes, no entanto, não causam o `Application_Start` método a ser executado.


[![Use um ponto de interrupção Verifique se o manipulador de eventos Application_Start está sendo executado](caching-data-at-application-startup-cs/_static/image7.png)](caching-data-at-application-startup-cs/_static/image6.png)

**Figura 4**: usar um ponto de interrupção para verificar que o `Application_Start` manipulador de eventos está sendo executado ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-cs/_static/image8.png))


> [!NOTE]
> Se você não tiver o `Application_Start` ponto de interrupção, quando você começa a depuração, é porque o aplicativo já foi iniciado. Forçar o aplicativo fosse reiniciado modificando o `Global.asax` ou `Web.config` arquivos e, em seguida, tente novamente. Você pode simplesmente adicionar (ou remover) uma linha em branco no final de um desses arquivos rapidamente reiniciar o aplicativo.


## <a name="step-5-displaying-the-cached-data"></a>Etapa 5: Exibir os dados armazenados em cache

Neste ponto o `StaticCache` classe tem uma versão dos dados do fornecedor armazenados em cache na inicialização do aplicativo que pode ser acessada por meio de seu `GetSuppliers()` método. Para trabalhar com dados da camada de apresentação, podemos usar um ObjectDataSource ou chamar programaticamente o `StaticCache` classe s `GetSuppliers()` método de uma classe de code-behind de páginas do ASP.NET. Permitir que o s examinar usando os controles ObjectDataSource e GridView para exibir as informações do fornecedor armazenados em cache.

Comece abrindo o `AtApplicationStartup.aspx` página o `Caching` pasta. Arraste um controle GridView da caixa de ferramentas para o designer, definindo seu `ID` propriedade `Suppliers`. Em seguida, marca inteligente s de GridView optar por criar um novo ObjectDataSource denominado `SuppliersCachedDataSource`. Configurar o ObjectDataSource para usar o `StaticCache` classe s `GetSuppliers()` método.


[![Configurar o ObjectDataSource para usar a classe StaticCache](caching-data-at-application-startup-cs/_static/image10.png)](caching-data-at-application-startup-cs/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar o `StaticCache` classe ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-cs/_static/image11.png))


[![Use o método GetSuppliers() para recuperar os dados do fornecedor armazenados em cache](caching-data-at-application-startup-cs/_static/image13.png)](caching-data-at-application-startup-cs/_static/image12.png)

**Figura 6**: Use o `GetSuppliers()` método para recuperar os dados do fornecedor armazenados em cache ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-cs/_static/image14.png))


Depois de concluir o assistente, o Visual Studio automaticamente adicionar BoundFields para cada um dos campos de dados em `SuppliersDataTable`. A marcação de declarativa s GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](caching-data-at-application-startup-cs/samples/sample7.aspx)]

A Figura 7 mostra a página quando exibida por meio de um navegador. A saída é o mesmo tinha removemos os dados de s BLL `SuppliersBLL` classe, mas usando o `StaticCache` classe retorna os dados do fornecedor como armazenado em cache na inicialização do aplicativo. Você pode definir pontos de interrupção de `StaticCache` classe s `GetSuppliers()` método para verificar se esse comportamento.


[![Os dados do fornecedor armazenados em cache é exibido em um GridView](caching-data-at-application-startup-cs/_static/image16.png)](caching-data-at-application-startup-cs/_static/image15.png)

**Figura 7**: os dados do fornecedor armazenados em cache é exibido em um controle GridView ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-cs/_static/image17.png))


## <a name="summary"></a>Resumo

A maioria das cada modelo de dados contém uma quantidade razoável de dados estáticos, geralmente implementados na forma de tabelas de pesquisa. Como essa informação é estática, há s nenhum motivo para acessar o banco de dados continuamente a cada vez que essas informações precisam ser exibidos. Além disso, devido à natureza estática, ao armazenar em cache os dados lá s sem a necessidade de uma expiração. Neste tutorial, vimos como utilizar esses dados e armazene em cache no cache de dados, o estado do aplicativo e por meio de uma variável de membro estático. Essa informação é armazenada em cache na inicialização do aplicativo e permanece no cache durante o tempo de vida do aplicativo s.

Este tutorial e os dois últimos, podemos ve pesquisados no cache de dados durante o tempo de vida do aplicativo s, bem como usar Expirações do tempo. Ao armazenar em cache dados do banco de dados, no entanto, uma expiração de tempo pode ser inferior ao ideal. Em vez de periodicamente, liberando o cache, seria ideal para remover apenas o item em cache quando os dados do banco de dados subjacente são modificados. Esse ideal é possível com o uso de dependências de cache do SQL, que vamos examinar nosso tutorial Avançar.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla. com](http://www.4guysfromrolla.com), trabalha com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e gravador. Seu livro mais recente é [ *Sams ensinar por conta própria ASP.NET 2.0 nas 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado em [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisado por vários revisores úteis. Revisores levar para este tutorial foram Teresa Murphy e Zack Jones. Interessado em examinar meu artigos futuros do MSDN? Nesse caso, me enviar uma linha no [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Anterior](caching-data-in-the-architecture-cs.md)
[Próximo](using-sql-cache-dependencies-cs.md)
