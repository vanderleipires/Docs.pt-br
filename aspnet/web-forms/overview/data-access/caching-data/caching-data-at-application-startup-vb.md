---
uid: web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
title: Armazenar em cache dados na inicialização do aplicativo (VB) | Microsoft Docs
author: rick-anderson
description: Em qualquer aplicativo da Web alguns dados serão usados com frequência e alguns dados serão usados com pouca frequência. Podemos melhorar o desempenho do nosso aplicativo b do ASP.NET...
ms.author: aspnetcontent
ms.date: 05/30/2007
ms.assetid: 84afe4ac-cc53-4f2e-a867-27eaf692c2df
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-at-application-startup-vb
msc.type: authoredcontent
ms.openlocfilehash: cf63cafbd4fd3937d18afa0d72a48868d76d3888
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841366"
---
<a name="caching-data-at-application-startup-vb"></a>Armazenar dados em cache na inicialização do aplicativo (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar PDF](caching-data-at-application-startup-vb/_static/datatutorial60vb1.pdf)

> Em qualquer aplicativo da Web alguns dados serão usados com frequência e alguns dados serão usados com pouca frequência. Podemos melhorar o desempenho do nosso aplicativo ASP.NET com antecedência carregando os dados usados com frequência, uma técnica conhecida como. Este tutorial demonstra uma abordagem de carregamento proativo, o que é carregar dados em cache na inicialização do aplicativo.


## <a name="introduction"></a>Introdução

Dois tutoriais anteriores, examinamos dados em cache na apresentação e as camadas de armazenamento em cache. Na [armazenando dados com o ObjectDataSource](caching-data-with-the-objectdatasource-vb.md), analisamos usando o s ObjectDataSource recursos em cache os dados na camada de apresentação de cache. [Armazenar em cache dados na arquitetura](caching-data-in-the-architecture-vb.md) examinado em uma camada de cache novo e separado de cache. Ambos esses tutoriais usados *reativo carregamento* ao trabalhar com o cache de dados. Com o carregamento reativo, cada vez que os dados são solicitados, o sistema verifica primeiro se ele s no cache. Caso contrário, ele captura os dados da fonte de origem, como o banco de dados e os armazena no cache. A principal vantagem de carregamento reativo é sua facilidade de implementação. Uma das desvantagens é seu desempenho irregular entre solicitações. Imagine que uma página que usa a camada de cache do tutorial anterior para exibir informações sobre o produto. Quando esta página é visitada pela primeira vez ou visitada pela primeira vez, após os dados em cache foi removidos devido a restrições de memória ou a expiração especificada ter sido atingido, os dados devem ser recuperados do banco de dados. Portanto, essas solicitações de usuários levará mais tempo do que as solicitações de usuários que podem ser atendidas pelo cache.

*Carregamento proativo* fornece uma estratégia de gerenciamento de cache alternativo que suaviza o desempenho em todas as solicitações por carregar os dados armazenados em cache antes que ele s necessários. Normalmente, o carregamento proativo usa algum processo que periodicamente verifica ou é notificado quando houver uma atualização para os dados subjacentes. Esse processo, em seguida, atualiza o cache para mantê-la atualizada. Carregamento proativo é especialmente útil se os dados subjacentes for proveniente de uma conexão de banco de dados lenta, um serviço Web ou alguma outra fonte de dados especialmente lenta. Mas essa abordagem de carregamento proativo é mais difícil de implementar, pois exige criar, gerenciar e implantar um processo para verificar as alterações e atualizar o cache.

Outro tipo de carregamento proativo e o tipo que vamos explorar neste tutorial, está carregando dados em cache na inicialização do aplicativo. Essa abordagem é especialmente útil para armazenar em cache dados estáticos, como os registros em tabelas de pesquisa de banco de dados.

> [!NOTE]
> Para obter uma visão mais detalhada sobre as diferenças entre carregamento proativo e reativo, bem como listas de prós e contras recomendações de implementação, consulte o [gerenciar o conteúdo de um Cache](https://msdn.microsoft.com/library/ms978503.aspx) seção o [ Guia de arquitetura para aplicativos do .NET Framework de cache](https://msdn.microsoft.com/library/ms978498.aspx).


## <a name="step-1-determining-what-data-to-cache-at-application-startup"></a>Etapa 1: Determinar quais dados de Cache na inicialização do aplicativo

Os exemplos de cache usando o carregamento reativo, examinamos no trabalho dois tutoriais anteriores bem com dados que pode ser alterado periodicamente e não demorarem exorbitantly para gerar. Mas se os dados em cache nunca mudarem, a expiração usada pelo carregamento reativo é supérflua. Da mesma forma, se os dados sejam armazenados em cache levam um tempo extremamente longo para gerar, esses usuários cujas solicitações encontrar a Esvaziar cache terão que suportar uma longa espera enquanto os dados subjacentes é recuperado. Considere o armazenamento em cache os dados estáticos e dados que leva um tempo muito longo para gerar na inicialização do aplicativo.

Enquanto bancos de dados têm muitos dinâmico, valores alterados com frequência, a maioria também tem uma quantidade razoável de dados estáticos. Por exemplo, praticamente todos os modelos de dados tem uma ou mais colunas que contêm um valor específico em um conjunto fixo de opções. Um `Patients` tabela de banco de dados pode ter um `PrimaryLanguage` coluna, cujo conjunto de valores pode ser o inglês, espanhol, francês, russo, japonês e assim por diante. Muitas vezes, esses tipos de colunas são implementados usando *tabelas de pesquisa*. Em vez de armazenar a cadeia de caracteres inglês ou francês no `Patients` tabela, uma segunda tabela é criada que tem, normalmente, duas colunas - um identificador exclusivo e uma descrição de cadeia de caracteres - com um registro para cada valor possível. O `PrimaryLanguage` coluna o `Patients` tabela armazena o identificador exclusivo correspondente na tabela de pesquisa. Na Figura 1, o paciente idioma principal do s John Doe é inglês, enquanto s Ed Johnson é russo.


![A tabela de idiomas é uma tabela de pesquisa usada pela tabela pacientes](caching-data-at-application-startup-vb/_static/image1.png)

**Figura 1**: O `Languages` tabela é uma tabela de pesquisa usado pelo `Patients` tabela


A interface do usuário para editar ou criar um novo paciente inclui uma lista suspensa de idiomas permitidos preenchido pelos registros no `Languages` tabela. Sem cache, cada vez que essa interface é visitado o sistema deve consultar o `Languages` tabela. Isso é um desperdício e desnecessário, pois os valores de tabela de pesquisa mudam com muita frequência, ou nunca.

Podemos pode armazenar em cache o `Languages` dados usando as mesmas técnicas de carregamento reativo examinadas nos tutoriais anteriores. No entanto, carregar reativo, usa uma expiração com base no tempo, não é necessária para dados de tabela de pesquisa estática. Enquanto o cache usando o carregamento reativo seria melhor do que absolutamente nenhum cache, a melhor abordagem seria proativamente carregar os dados da tabela de pesquisa no cache na inicialização do aplicativo.

Neste tutorial vamos examinar como dados de tabela de pesquisa de cache e outras informações estáticas.

## <a name="step-2-examining-the-different-ways-to-cache-data"></a>Etapa 2: Examinar as diferentes maneiras de armazenar dados em Cache

Informações podem ser programaticamente armazenadas em cache em um aplicativo ASP.NET usando uma variedade de abordagens. Podemos var já viu como usar o cache de dados nos tutoriais anteriores. Como alternativa, objetos podem ser programaticamente armazenados em cache usando *membros estáticos* ou *estado do aplicativo*.

Ao trabalhar com uma classe, normalmente a classe deve primeiro ser instanciada antes de seus membros podem ser acessados. Por exemplo, para invocar um método de uma das classes em nossa camada de lógica de negócios, deve primeiro criamos uma instância da classe:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample1.vb)]

Antes de nós pode invocar *SomeMethod* ou trabalhar com *SomeProperty*, primeiro devemos criar uma instância da classe usando o `New` palavra-chave. *SomeMethod* e *SomeProperty* estão associados uma determinada instância. O tempo de vida desses membros está ligado ao tempo de vida de seu objeto associado. *Membros estáticos*, por outro lado, são variáveis, propriedades e métodos que são compartilhados entre *todos os* instâncias da classe e, consequentemente, têm um tempo de vida, desde que a classe. Membros estáticos são indicados pela palavra-chave `Shared`.

Além dos membros estáticos, dados podem ser armazenados em cache usando o estado do aplicativo. Cada aplicativo ASP.NET mantém uma coleção de nome/valor que s compartilhados entre todos os usuários e as páginas do aplicativo. Essa coleção pode ser acessada usando o [ `HttpContext` classe](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) s [ `Application` propriedade](https://msdn.microsoft.com/library/system.web.httpcontext.application.aspx)e usado a partir de uma classe de code-behind s de página ASP.NET da seguinte forma:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample2.vb)]

O cache de dados fornece uma API muito mais rica para o cache de dados, fornecendo mecanismos para expirações baseados em tempo e dependência, as prioridades de item de cache e assim por diante. Com os membros estáticos e o estado do aplicativo, esses recursos devem ser adicionados manualmente pelo desenvolvedor da página. Ao armazenar em cache dados na inicialização do aplicativo para o tempo de vida do aplicativo, no entanto, as vantagens do cache s de dados são questão discutível. Neste tutorial, examinaremos o código que usa todas as três técnicas para armazenar em cache dados estáticos.

## <a name="step-3-caching-thesupplierstable-data"></a>Etapa 3: Armazenamento em cache o`Suppliers`dados da tabela

A Northwind tabelas de banco de dados é ve implementado para data não incluem quaisquer tabelas de pesquisa tradicional. As quatro tabelas de dados implementado em nossa DAL todas as tabelas do modelo cujos valores são não estático. Em vez de gastar tempo para adicionar uma nova DataTable da DAL e, em seguida, uma nova classe e métodos para a BLL, para este tutorial deixe s apenas fingir que o `Suppliers` dados da tabela s são estáticos. Portanto, podemos poderia armazenar em cache esses dados na inicialização do aplicativo.

Para começar, crie uma nova classe chamada `StaticCache.cs` no `CL` pasta.


![Criar a classe StaticCache.vb na pasta CL](caching-data-at-application-startup-vb/_static/image2.png)

**Figura 2**: criar o `StaticCache.vb` classe o `CL` pasta


Precisamos adicionar um método que carrega os dados na inicialização para o armazenamento em cache apropriado, bem como os métodos que retornam dados deste cache.


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample3.vb)]

O código acima usa uma variável de membro estático `suppliers`, para armazenar os resultados do `SuppliersBLL` classe s `GetSuppliers()` método, que é chamado do `LoadStaticCache()` método. O `LoadStaticCache()` método destina-se ser chamado durante a inicialização do aplicativo s. Depois que esses dados forem carregados na inicialização do aplicativo, qualquer página que precisa para trabalhar com dados do fornecedor pode chamar o `StaticCache` classe s `GetSuppliers()` método. Portanto, a chamada para o banco de dados para obter os fornecedores ocorre apenas uma vez, ao iniciar o aplicativo.

Em vez de usar uma variável de membro estático como o armazenamento em cache, poderíamos ter Alternativamente usado o estado do aplicativo ou o cache de dados. O código a seguir mostra a classe alterada para usar o estado do aplicativo:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample4.vb)]

Na `LoadStaticCache()`, as informações de fornecedor são armazenadas para a variável de aplicativo *chave*. -S é retornado como tipo apropriado (`Northwind.SuppliersDataTable`) de `GetSuppliers()`. Enquanto o estado do aplicativo pode ser acessado nas classes de lógica de páginas do ASP.NET usando `Application("key")`, na arquitetura, devemos usar `HttpContext.Current.Application("key")` para obter atual `HttpContext`.

Da mesma forma, o cache de dados pode ser usado como um armazenamento de cache, como mostra o código a seguir:


[!code-vb[Main](caching-data-at-application-startup-vb/samples/sample5.vb)]

Para adicionar um item ao cache de dados com nenhum vencimento baseada em tempo, use o `System.Web.Caching.Cache.NoAbsoluteExpiration` e `System.Web.Caching.Cache.NoSlidingExpiration` valores como parâmetros de entrada. Essa sobrecarga específica do cache de dados s `Insert` método foi selecionado para que pudéssemos especificar a *prioridade* do item de cache. A prioridade é usada para determinar quais itens serão eliminados do cache quando há pouca memória disponível. Aqui, usamos a prioridade `NotRemovable`, que garante que esse item de cache ganhou t ser eliminado.

> [!NOTE]
> Este download tutorial s implementa o `StaticCache` classe usando a abordagem de variável de membro estático. O código para as técnicas de cache de estado e os dados de aplicativo está disponível nos comentários no arquivo de classe.


## <a name="step-4-executing-code-at-application-startup"></a>Etapa 4: Executar o código na inicialização do aplicativo

Para executar código quando um aplicativo web é iniciado pela primeira vez, precisamos criar um arquivo especial chamado `Global.asax`. Esse arquivo pode conter os manipuladores de eventos para o aplicativo-, sessão-, e eventos de nível de solicitação e é aqui onde podemos adicionar código que será executado sempre que o aplicativo é iniciado.

Adicionar o `Global.asax` arquivo para o diretório raiz da web application s clicando duas vezes no nome do projeto de site no Visual Studio s Gerenciador de soluções e escolhendo Adicionar Novo Item. Na caixa de diálogo Add New Item, selecione o tipo de item de classe de aplicativo Global e, em seguida, clique no botão Adicionar.

> [!NOTE]
> Se você já tiver um `Global.asax` arquivo em seu projeto, a classe de aplicativo Global, tipo de item não será listado na caixa de diálogo Adicionar Novo Item.


[![Adicionar o arquivo global asax para seu aplicativo de Web s diretório de raiz](caching-data-at-application-startup-vb/_static/image4.png)](caching-data-at-application-startup-vb/_static/image3.png)

**Figura 3**: Adicione o `Global.asax` arquivo ao seu aplicativo Web s diretório raiz ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-vb/_static/image5.png))


O padrão `Global.asax` modelo de arquivo inclui cinco métodos dentro de um servidor `<script>` marca:

- **`Application_Start`** executa quando o aplicativo web é iniciado pela primeira vez
- **`Application_End`** é executado quando o aplicativo está sendo desligado
- **`Application_Error`** é executado sempre que uma exceção sem tratamento atinge o aplicativo
- **`Session_Start`** é executado quando uma nova sessão é criada.
- **`Session_End`** é executado quando uma sessão expirou ou foi abandonada

O `Application_Start` manipulador de eventos é chamado apenas uma vez durante um ciclo de vida do aplicativo s. O aplicativo é iniciado na primeira vez um recurso do ASP.NET é solicitado do aplicativo e continua a ser executado até que o aplicativo for reiniciado, que pode ocorrer ao modificar o conteúdo do `/Bin` pasta, modificar `Global.asax`, modificando o conteúdo na `App_Code` pasta ou modificando o `Web.config` arquivo, entre outras causas. Consulte a [visão geral do ciclo de vida de aplicativos ASP.NET](https://msdn.microsoft.com/library/ms178473.aspx) para uma discussão mais detalhada sobre o ciclo de vida do aplicativo.

Para esses tutoriais só precisamos adicionar código para o `Application_Start` método, portanto, sinta-se livre para remover os outros. Na `Application_Start`, basta chamar o `StaticCache` classe s `LoadStaticCache()` método, que irá carregar e armazenar em cache as informações do fornecedor:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample6.aspx)]

Tudo que s é a ele! Na inicialização do aplicativo, o `LoadStaticCache()` método pegar as informações do fornecedor da BLL e armazená-lo em uma variável de membro estático (ou qualquer cache armazenar você terminou usando no `StaticCache` classe). Para verificar esse comportamento, defina um ponto de interrupção no `Application_Start` método e executar seu aplicativo. Observe que o ponto de interrupção é atingido durante a inicialização do aplicativo. As solicitações subsequentes, no entanto, não causam o `Application_Start` método a ser executado.


[![Use um ponto de interrupção Verifique se o manipulador de eventos Application_Start está sendo executado](caching-data-at-application-startup-vb/_static/image7.png)](caching-data-at-application-startup-vb/_static/image6.png)

**Figura 4**: Use um ponto de interrupção para verificar que o `Application_Start` manipulador de eventos está sendo executado ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-vb/_static/image8.png))


> [!NOTE]
> Se você não encontrar o `Application_Start` ponto de interrupção quando você começa a depuração, é porque seu aplicativo já foi iniciado. Forçar o aplicativo reinicie modificando sua `Global.asax` ou `Web.config` arquivos e, em seguida, tente novamente. Você pode simplesmente adicionar (ou remover) uma linha em branco no final de um desses arquivos rapidamente reiniciar o aplicativo.


## <a name="step-5-displaying-the-cached-data"></a>Etapa 5: Exibir os dados armazenados em cache

Neste momento a `StaticCache` classe tem uma versão dos dados do fornecedor armazenados em cache na inicialização do aplicativo que pode ser acessada por meio de seu `GetSuppliers()` método. Para trabalhar com esses dados da camada de apresentação, podemos usar um ObjectDataSource ou invocar programaticamente a `StaticCache` classe s `GetSuppliers()` método a partir de uma classe de code-behind s de página ASP.NET. Deixe o s examinar usando os controles ObjectDataSource e GridView para exibir as informações do fornecedor armazenados em cache.

Comece abrindo o `AtApplicationStartup.aspx` página o `Caching` pasta. Arraste um controle GridView da caixa de ferramentas para o designer, definindo sua `ID` propriedade para `Suppliers`. Em seguida, marca inteligente de s de GridView optar por criar um novo ObjectDataSource chamado `SuppliersCachedDataSource`. Configurar o ObjectDataSource para usar o `StaticCache` classe s `GetSuppliers()` método.


[![Configurar o ObjectDataSource para usar a classe StaticCache](caching-data-at-application-startup-vb/_static/image10.png)](caching-data-at-application-startup-vb/_static/image9.png)

**Figura 5**: configurar o ObjectDataSource para usar o `StaticCache` classe ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-vb/_static/image11.png))


[![Use o método GetSuppliers() para recuperar os dados do fornecedor armazenados em cache](caching-data-at-application-startup-vb/_static/image13.png)](caching-data-at-application-startup-vb/_static/image12.png)

**Figura 6**: Use o `GetSuppliers()` método para recuperar os dados do fornecedor armazenados em cache ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-vb/_static/image14.png))


Depois de concluir o assistente, o Visual Studio adicionará automaticamente BoundFields para cada um dos campos de dados em `SuppliersDataTable`. Sua marcação declarativa de s do GridView e ObjectDataSource deve ser semelhante ao seguinte:


[!code-aspx[Main](caching-data-at-application-startup-vb/samples/sample7.aspx)]

Figura 7 mostra a página quando visualizado por meio de um navegador. A saída é o mesmo tinha removemos os dados de s BLL `SuppliersBLL` classe, mas usando o `StaticCache` classe retorna os dados do fornecedor como armazenado em cache na inicialização do aplicativo. Você pode definir pontos de interrupção a `StaticCache` classe s `GetSuppliers()` método para verificar esse comportamento.


[![Os dados do fornecedor armazenados em cache é exibido em um GridView](caching-data-at-application-startup-vb/_static/image16.png)](caching-data-at-application-startup-vb/_static/image15.png)

**Figura 7**: os dados do fornecedor armazenados em cache é exibido em um GridView ([clique para exibir a imagem em tamanho normal](caching-data-at-application-startup-vb/_static/image17.png))


## <a name="summary"></a>Resumo

A maioria dos cada modelo de dados contém uma quantidade razoável de dados estáticos, geralmente é implementados na forma de tabelas de pesquisa. Uma vez que essas informações são estáticas, daí s nenhum motivo para acessar o banco de dados continuamente a cada vez que essas informações precisam ser exibido. Além disso, devido à sua natureza estática, ao armazenar em cache os dados lá s sem a necessidade de uma expiração. Neste tutorial vimos como usar esses dados e o armazena em cache no cache de dados, o estado do aplicativo e por meio de uma variável de membro estático. Essa informação é armazenada em cache na inicialização do aplicativo e permanece no cache durante o tempo de vida do aplicativo s.

Este tutorial e os dois últimos, podemos ve examinou armazenando dados em cache durante o tempo de vida do aplicativo s, bem como usando expirações baseada em tempo. Ao armazenar em cache de banco de dados, no entanto, uma expiração com base no tempo pode ser inferior ao ideal. Em vez de liberar periodicamente o cache, seria ideal para remover apenas o item em cache quando os dados do banco de dados subjacente são modificados. Esse ideal é possível com o uso de dependências de cache SQL, que vamos examinar nosso próximo tutorial.

Boa programação!

## <a name="about-the-author"></a>Sobre o autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de sete livros sobre ASP/ASP.NET e fundador da [4GuysFromRolla.com](http://www.4guysfromrolla.com), tem trabalhado com tecnologias Microsoft Web desde 1998. Scott funciona como um consultor independente, instrutor e escritor. Seu livro mais recente é [ *Sams Teach por conta própria ASP.NET 2.0 em 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Ele pode ser contatado pelo [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) ou por meio de seu blog, que pode ser encontrado em [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimentos especiais a

Esta série de tutoriais foi revisada por muitos revisores úteis. Os revisores de avanço para este tutorial foram Teresa Murphy e Zack Jones. Você está interessado na revisão Meus próximos artigos do MSDN? Nesse caso, me descartar uma linha na [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-in-the-architecture-vb.md)
> [Próximo](using-sql-cache-dependencies-vb.md)
