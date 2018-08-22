---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: Resiliência de Conexão de formulários da Web do ASP.NET e interceptação de comando | Microsoft Docs
author: Erikre
description: Este tutorial descreve como modificar um aplicativo de exemplo para dar suporte a resiliência de conexão e interceptação de comando.
ms.author: riande
ms.date: 03/31/2014
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 039923a91d957765fa8b2c0cfe11abc8790c1e88
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824678"
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Resiliência de Conexão de formulários da Web do ASP.NET e interceptação de comando
====================
por [Erik Reitan](https://github.com/Erikre)

Neste tutorial, você modificará o aplicativo de exemplo Wingtip Toys para dar suporte a resiliência de conexão e interceptação de comando. Habilitando a resiliência de conexão, o aplicativo de exemplo Wingtip Toys repetirá automaticamente chamadas de dados quando ocorrem erros transitórios típicos de um ambiente de nuvem. Além disso, ao implementar a interceptação de comando, o aplicativo de exemplo Wingtip Toys irá capturar todas as consultas SQL enviadas ao banco de dados para fazer logon ou alterá-los.

> [!NOTE] 
> 
> Este tutorial de formulários da Web foi baseado no tutorial a seguir MVC de Tom Dykstra:  
> [Resiliência de Conexão e interceptação de comando com o Entity Framework em um aplicativo ASP.NET MVC](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>O que você aprenderá:

- Como fornecer resiliência de conexão.
- Como implementar a interceptação de comando.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que você tenha o seguinte software instalado em seu computador:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). O .NET Framework é instalado automaticamente.
- A Wingtip Toys o projeto, de exemplo para que você possa implementar a funcionalidade mencionada neste tutorial dentro do projeto Wingtip Toys. O link a seguir fornece detalhes de download:

    - [Introdução ao ASP.NET 4.5.1 Web Forms - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (c#)
- Antes de concluir este tutorial, considere revisar a série de tutoriais relacionada [Introdução ao Web Forms do ASP.NET 4.5 e Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). A série de tutoriais ajudarão você a se familiarizar com o **WingtipToys** projeto e o código.

## <a name="connection-resiliency"></a>Resiliência da conexão

Quando você considera que a implantação de um aplicativo para o Windows Azure, uma opção a ser considerada é implantar o banco de dados **Windows** **banco de dados SQL**, um serviço de banco de dados de nuvem. Erros transitórios de conexão são geralmente mais frequentes quando você se conecta a um serviço de banco de dados de nuvem que quando o servidor web e o servidor de banco de dados estão diretamente conectados juntos no mesmo data center. Mesmo se um servidor web na nuvem e um serviço de banco de dados de nuvem são hospedadas no mesmo data center, há mais conexões de rede entre elas, pode ter problemas, como balanceadores de carga.

Também um serviço de nuvem é normalmente compartilhado por outros usuários, que significa que sua capacidade de resposta pode ser afetada por eles. E seu acesso ao banco de dados pode estar sujeito à limitação. Limitação significa que o serviço de banco de dados gera exceções quando você tentar acessá-lo com mais frequência do que é permitido em sua *contrato de nível de serviço* (SLA).

Muitos ou a maioria dos problemas de conexão que ocorrem quando você estiver acessando um serviço de nuvem são transitórios, ou seja, eles resolvem sozinho em um curto período de tempo. Portanto, quando você tentar uma operação de banco de dados e obter um tipo de erro que é normalmente transitório, você poderia Repita a operação após uma breve espera e a operação podem ser bem-sucedida. Você pode fornecer uma experiência muito melhor para seus usuários, se você tratar erros transitórios automaticamente tentando novamente, tornando a maioria deles invisível para o cliente. O recurso de resiliência de conexão no Entity Framework 6 automatiza a que o processo de repetição falhou consultas SQL.

O recurso de resiliência de conexão deve ser configurado adequadamente para um serviço de banco de dados específico:

1. Ele precisa saber quais exceções são possivelmente é transitório. Você deseja repetir erros causados por uma perda temporária na conectividade de rede, não há suporte para os erros causados por bugs de programa, por exemplo.
2. Ele deve esperar uma quantidade apropriada de tempo entre as repetições de uma operação com falha. Você pode esperar mais entre as repetições para um processo em lote, que é possível para uma página da web online em que um usuário está aguardando uma resposta.
3. Ele tem que repetir um número apropriado de vezes antes de ele desiste. Talvez você queira repetir mais vezes em um processo em lote que você usaria em um aplicativo online.

Você pode definir essas configurações manualmente para qualquer ambiente de banco de dados com suporte por um provedor de Entity Framework.

Tudo o que você precisa fazer para habilitar a resiliência de conexão é criar uma classe em seu assembly que deriva de `DbConfiguration` classe e na classe definir a estratégia de execução de banco de dados SQL, que é outro termo para política de repetição no Entity Framework.

### <a name="implementing-connection-resiliency"></a>Implementando a resiliência de conexão

1. Baixe e abra o [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) exemplo de aplicativo de Web Forms no Visual Studio.
2. No *lógica* pasta da **WingtipToys** aplicativo, adicione um arquivo de classe chamado *WingtipToysConfiguration.cs*.
3. Substitua o código existente pelo código a seguir:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

O Entity Framework é executado automaticamente o código que ele se encontra em uma classe que deriva de `DbConfiguration`. Você pode usar o `DbConfiguration` classe para realizar tarefas de configuração no código que você faria no caso contrário, o *Web. config* arquivo. Para obter mais informações, consulte [EntityFramework configuração baseada em código](https://msdn.microsoft.com/data/jj680699).

1. No *lógica* pasta, abra o *AddProducts.cs* arquivo.
2. Adicionar um `using` instrução para `System.Data.Entity.Infrastructure` conforme realçado em amarelo:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Adicionar um `catch` bloco para o `AddProduct` método para que o `RetryLimitExceededException` é registrado como realçado em amarelo:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Adicionando o `RetryLimitExceededException` exceção, você pode fornecer melhor registro em log ou exibir uma mensagem de erro para o usuário em que eles podem optar por experimentar o processo novamente. Capturando o `RetryLimitExceededException` exceção, as probabilidade de serem transitórias somente os erros serão já ter sido tentada e falhada várias vezes. A exceção real retornada será encapsulada no `RetryLimitExceededException` exceção. Além disso, você também adicionou um bloco catch geral. Para obter mais informações sobre o `RetryLimitExceededException` exceção, consulte [resiliência de Conexão do Entity Framework / lógica de repetição](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Interceptação de comando

Agora que você ativou a em uma política de repetição, como você testar para verificar se ele está funcionando conforme o esperado? Não é tão fácil forçar um erro transitório ocorrer, especialmente quando você estiver executando localmente, e ele seria especialmente difícil de integrar erros transitórios reais em um teste de unidade automatizados. Para testar o recurso de resiliência de conexão, você precisa de uma maneira de interceptar consultas do Entity Framework envia para o SQL Server e substituir a resposta do SQL Server com um tipo de exceção que é normalmente transitório.

Você também pode usar a interceptação de consulta para implementar a prática recomendada para aplicativos em nuvem: log a latência e o êxito ou a falha de todas as chamadas externa para os serviços, como serviços de banco de dados.

Nesta seção do tutorial, você usará o Entity Framework [ *recurso de interceptação* ](https://msdn.microsoft.com/data/dn469464) para registro em log e para simular erros transitórios.

### <a name="create-a-logging-interface-and-class"></a>Criar uma interface de registro em log e a classe

Uma prática recomendada para registro em log é fazê-lo usando um [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) em vez de embutir chamadas para `System.Diagnostics.Trace` ou uma classe de registro em log. Isso facilita alterar seu mecanismo de log mais tarde, se você precisar fazer isso. Portanto, nesta seção, você criará a interface de registro em log e uma classe para implementá-lo.

Com base no procedimento acima, você tenha baixado e aberto a **WingtipToys** exemplo de aplicativo no Visual Studio.

1. Crie uma pasta na **WingtipToys** do projeto e denomine- *log*.
2. No *registro em log* pasta, crie um arquivo de classe chamado *ILogger.cs* e substitua o código padrão pelo código a seguir:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   A interface fornece três níveis de rastreamento para indicar a importância relativa de logs e uma projetada para fornecer informações de latência para chamadas de serviço externo, como consultas de banco de dados. Os métodos de registro em log têm sobrecargas que permitem que você passe uma exceção. Isso é para que informações de exceção, incluindo as exceções interna e o rastreamento de pilha confiável são registradas pela classe que implementa a interface, em vez de depender de ter feito isso em cada chamada de método de registro em log em todo o aplicativo.  
  
   O `TraceApi` métodos que você possa acompanhar a latência de cada chamada para um serviço externo, como o banco de dados SQL.
3. No *registro em log* pasta, crie um arquivo de classe chamado *Logger.cs* e substitua o código padrão pelo código a seguir:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

A implementação usa `System.Diagnostics` para fazer o rastreamento. Isso é um recurso interno do .NET que torna mais fácil gerar e usar informações de rastreamento. Existem várias &quot;ouvintes&quot; você pode usar com `System.Diagnostics` rastreamento, para gravar logs em arquivos, por exemplo, ou para gravá-los no armazenamento de blob no Windows Azure. Veja algumas das opções e links para outros recursos para obter mais informações, no [solução de problemas de Windows Azure Web Sites no Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Para este tutorial, você só verá logs no Visual Studio **saída** janela.

Em um aplicativo de produção você talvez queira considerar o uso de estruturas de rastreamento diferente `System.Diagnostics`e o `ILogger` interface torna relativamente fácil alternar para um mecanismo de rastreamento diferente se você optar por fazer isso.

### <a name="create-interceptor-classes"></a>Criar classes de interceptor

Em seguida, você criará as classes que o Entity Framework irá chamar toda vez que ele vai enviar uma consulta para o banco de dados, um para simular erros transitórios e para fazer o registro em log. Essas classes de interceptor devem derivar de `DbCommandInterceptor` classe. -Los, você escreve as substituições de métodos que são chamadas automaticamente quando a consulta está prestes a ser executada. Esses métodos, você pode examinar ou a consulta que está sendo enviada ao banco de dados de log, e você pode alterar a consulta antes de serem enviado ao banco de dados ou retornar algo para o Entity Framework por conta própria sem nem mesmo a passagem da consulta ao banco de dados.

1. Para criar a classe de interceptador que registrará em log todas as consultas de SQL antes de serem enviado ao banco de dados, crie um arquivo de classe chamado *InterceptorLogging.cs* na *lógica* pasta e substitua o padrão de código com o o código a seguir:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Para comandos ou consultas com êxito, esse código grava um log de informações com as informações de latência. Para exceções, ele cria um log de erros.
2. Para criar a classe de interceptador que gerarão erros transitórios fictícios quando você insere &quot;lançar&quot; na **nome** caixa de texto na página chamada *AdminPage.aspx*, crie uma classe arquivo denominado *InterceptorTransientErrors.cs* na *lógica* pasta e substitua o padrão de código com o código a seguir:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Esse código substitui apenas o `ReaderExecuting` método, que é chamado para consultas que podem retornar várias linhas de dados. Se você quiser verificar a resiliência de conexão para outros tipos de consultas, você também pode substituir a `NonQueryExecuting` e `ScalarExecuting` métodos, como o interceptador de registro em log faz.  
  
   Posteriormente, você faça logon como "Admin" e selecione o **Admin** link na barra de navegação superior. Em seguida, na *AdminPage.aspx* página, você adicionará um produto chamado &quot;Throw&quot;. O código cria uma exceção de banco de dados SQL fictícia do número de erro 20, um tipo conhecido por ser normalmente transitórios. Outros números de erro reconhecidos atualmente como transitório são 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 e 40613, mas eles estão sujeitos a alterações nas novas versões do banco de dados SQL. O produto será renomeado para "TransientErrorExample", você pode seguir o código do *InterceptorTransientErrors.cs* arquivo.  
  
   O código retorna a exceção para o Entity Framework em vez de executar a consulta e passando devolver os resultados. A exceção transitória é retornada *quatro* vezes, e, em seguida, o código será revertido para o procedimento normal de passar a consulta ao banco de dados.

    Porque tudo o que está conectado, você poderá ver que o Entity Framework tenta executar a consulta quatro vezes antes de finalmente ter êxito e a única diferença no aplicativo é que ele leva mais tempo para renderizar uma página com os resultados da consulta.  
  
   O número de vezes que o Entity Framework tentará é configurável; o código especifica quatro vezes, porque esse é o valor padrão para a política de execução do banco de dados SQL. Se você alterar a política de execução, você precisava também alterar o código que especifica quantas vezes os erros transitórios são gerados. Você também pode alterar o código para gerar mais exceções para que o Entity Framework gerará o `RetryLimitExceededException` exceção.
3. Na *global. asax*, adicione as seguintes instruções using:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Em seguida, adicione as linhas destacadas para o `Application_Start` método:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Essas linhas de código são o que faz com que seu código de interceptador ser executada quando o Entity Framework envia consultas ao banco de dados. Observe que, como você criou classes de interceptador separado para a simulação de erro transitório e registro em log, você pode independentemente habilitar e desabilitá-los.   
  
 Você pode adicionar interceptores usando o `DbInterception.Add` método em qualquer lugar no seu código; ele não precisa estar no `Application_Start` método. Outra opção, se você não adicionou interceptadores na `Application_Start` método, seria atualizar ou adicionar a classe denominada *WingtipToysConfiguration.cs* e coloque o código acima no final do construtor a `WingtipToysbConfiguration` classe.

Onde quer que você coloque esse código, tome cuidado para executar não `DbInterception.Add` para o interceptador mesmo mais de uma vez, ou você obterá instâncias adicionais do interceptor. Por exemplo, se você adicionar o interceptador de registro em log duas vezes, você verá dois logs para todas as consultas SQL.

Os interceptores são executados na ordem de registro (a ordem na qual o `DbInterception.Add` método é chamado). A ordem pode ser relevante, dependendo do que você está fazendo no interceptador. Por exemplo, um interceptador pode alterar o comando SQL que ele obtém no `CommandText` propriedade. Se ele altera o comando SQL, o interceptador próxima receberá o comando SQL alterados, não o comando SQL original.

Você escreveu o código de erro transitório de simulação de uma maneira que permite que você causar erros transitórios, inserindo um valor diferente na interface do usuário. Como alternativa, você poderia escrever o código de interceptador para sempre gerar a sequência de exceções transitórias sem verificar se há um valor de parâmetro específico. Em seguida, você pode adicionar o interceptador somente quando você deseja gerar erros transitórios. Se você fizer isso, no entanto, não adicione o interceptador até após a inicialização do banco de dados foi concluída. Em outras palavras, fazer a operação de pelo menos um banco de dados como uma consulta em um dos seus conjuntos de entidades antes de começar gerando erros transitórios. O Entity Framework executa várias consultas durante a inicialização do banco de dados, e não são executadas em uma transação, portanto, erros durante a inicialização pode fazer com que o contexto para entrar em um estado inconsistente.

## <a name="test-logging-and-connection-resiliency"></a>Resiliência de conexão e de registro em log de teste

1. No Visual Studio, pressione **F5** para executar o aplicativo no modo de depuração e, em seguida, faça logon como "Admin" com "Pa$ $word" a senha.
2. Selecione **Admin** na barra de navegação na parte superior.
3. Insira um novo produto chamado "Throw" com o arquivo de descrição, preço e a imagem apropriado.
4. Pressione a **adicionar produto** botão.  
   Você observará que o navegador parece parar de responder por alguns segundos, enquanto o Entity Framework está tentando novamente a consulta várias vezes. A primeira nova tentativa acontece muito rapidamente e, em seguida, aumenta o tempo de espera antes de cada repetição adicional. Esse processo de espera mais antes de é chamada a cada repetição *retirada exponencial* .
5. Aguarde até que a página não está mais atttempting para carregar.
6. Parar o projeto e examine o Visual Studio **saída** janela para ver a saída de rastreamento. Você pode encontrar o **saída** janela selecionando **Debug**  - &gt; **Windows**  - &gt;  **Saída**. Talvez você precise rolarem vários outros logs gravados pelo seu agente.  
  
   Observe que você pode ver as consultas SQL reais enviadas para o banco de dados. Você verá algumas consultas inicias e comandos que o Entity Framework para começar, verificação da tabela de histórico de versão e a migração de banco de dados.   
    ![Janela de Saída](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Observe que você não pode repetir esse teste, a menos que você interrompa o aplicativo e reiniciá-lo. Se você quiser ser capaz de testar a resiliência de conexão várias vezes em uma única execução do aplicativo, você pode escrever código para redefinir o contador de erro em `InterceptorTransientErrors` .
7. Para ver a diferença a estratégia de execução (política de repetição) faz, comentário a `SetExecutionStrategy` de linha no *WingtipToysConfiguration.cs* do arquivo no *lógica* pasta, execute o **Admin**  página no modo de depuração novamente e adicione o produto chamado &quot;lançar&quot; novamente.  
  
   Desta vez o depurador para na primeira exceção gerada imediatamente quando ele tenta executar a consulta na primeira vez.  
    ![Depuração – exibir detalhes](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Remova os `SetExecutionStrategy` de linha na *WingtipToysConfiguration.cs* arquivo.

## <a name="summary"></a>Resumo

Neste tutorial, você viu como modificar um aplicativo de exemplo de formulários da Web para dar suporte a resiliência de conexão e interceptação de comando.

## <a name="next-steps"></a>Próximas etapas

Depois de revisar a resiliência de conexão e interceptação de comando no Web Forms do ASP.NET, revise o tópico de Web Forms do ASP.NET [métodos assíncronos no ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). O tópico ensinará os conceitos básicos da criação de um aplicativo Web Forms do ASP.NET assíncrono usando o Visual Studio.
