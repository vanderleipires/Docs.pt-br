---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: Interceptação de comando com o Entity Framework em um aplicativo ASP.NET MVC e resiliência de Conexão | Microsoft Docs
author: tdykstra
description: O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2015
ms.topic: article
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 4a43a9120bf3fa69b00b234d65d0f59d3ce9975b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875338"
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Resiliência de Conexão e interceptação de comando com o Entity Framework em um aplicativo ASP.NET MVC
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto concluído](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) ou [baixar PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> O aplicativo web de exemplo Contoso University demonstra como criar aplicativos ASP.NET MVC 5 usando o Entity Framework 6 Code First e o Visual Studio 2013. Para obter informações sobre a série de tutoriais, consulte [primeiro tutorial na série](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Até o aplicativo está sendo executado localmente no IIS Express no computador de desenvolvimento. Para disponibilizar um aplicativo real para outros usuários na Internet, você precisa implantá-lo em um provedor de hospedagem na web, e você precisa implantar o banco de dados em um servidor de banco de dados.

Neste tutorial, você aprenderá a usar os dois recursos são especialmente importantes quando você estiver implantando em um ambiente de nuvem do Entity Framework 6: resiliência de conexão (repetições automáticas para erros transitórios) e interceptação de comando (captura todas as consultas SQL enviado para o banco de dados para fazer logon ou alterá-los).

Este tutorial de interceptação de resiliência e o comando de conexão é opcional. Se você ignorar este tutorial, alguns ajustes pequenos terá de ser feitos em tutoriais subsequentes.

## <a name="enable-connection-resiliency"></a>Habilitar a resiliência de conexão

Quando você implanta o aplicativo no Windows Azure, você implantará o banco de dados para o Windows Azure SQL Database, um serviço de banco de dados de nuvem. Erros transitórios de conexão são geralmente mais frequentes, quando você se conectar a um serviço de banco de dados de nuvem que quando o servidor web e o servidor de banco de dados estão diretamente conectados juntos no mesmo data center. Mesmo se um servidor de web de nuvem e um serviço de banco de dados de nuvem são hospedadas no mesmo data center, há mais conexões de rede entre elas que pode ter problemas, como balanceadores de carga.

Também um serviço de nuvem é normalmente compartilhado por outros usuários, que significa que sua capacidade de resposta pode ser afetada por eles. E seu acesso ao banco de dados pode estar sujeito a limitação. O serviço de banco de dados significa a limitação lança exceções quando você tentar acessá-lo mais frequentemente que é permitido no contrato de nível de serviço (SLA).

Muitos ou a maioria dos problemas de conexão quando você estiver acessando um serviço de nuvem são transitórios, ou seja, se eles resolvem em um curto período de tempo. Então quando você tentar uma operação de banco de dados e obter um tipo de erro que é normalmente transitório, poderia tente a operação novamente depois que uma espera de curta e a operação seja bem-sucedida. Você pode fornecer uma melhor experiência para os usuários se tratar erros transitórios automaticamente tentando novamente, tornando a maioria deles invisível para o cliente. O recurso de resiliência de conexão no Entity Framework 6 automatiza o processo de repetição falha consultas SQL.

O recurso de resiliência de conexão deve ser configurado corretamente para um serviço de banco de dados específico:

- É necessário saber quais exceções são provavelmente transitório. Você deseja repetir erros causados por perda temporária de conectividade de rede, não os erros causados por erros de programa, por exemplo.
- Ele deve esperar uma quantidade apropriada de tempo entre as repetições de uma operação com falha. Você pode esperar mais entre as tentativas de um processo em lote do que para uma página da web online em que um usuário está aguardando uma resposta.
- Ele tem que repetir um número apropriado de vezes antes de desistir. Você talvez queira repetir mais vezes em um processo em lote que você faria em um aplicativo online.

Você pode definir essas configurações manualmente para qualquer ambiente de banco de dados com suporte por um provedor do Entity Framework, mas os valores padrão que geralmente funcionam bem para um aplicativo online que usa o banco de dados SQL do Windows Azure já foram configurados para você, e Essas são as configurações que você implementará para o aplicativo da Contoso University.

Você precisa fazer para habilitar a resiliência de conexão é criar uma classe em seu assembly que deriva de [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) classe e, na classe, defina o banco de dados do SQL *estratégia de execução*, que EF é outro termo para *política de repetição*.

1. Na pasta DAL, adicione um arquivo de classe chamado *SchoolConfiguration.cs*.
2. Substitua o código de modelo com o código a seguir:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    O Entity Framework executa automaticamente o código encontrados em uma classe que deriva de `DbConfiguration`. Você pode usar o `DbConfiguration` classe para realizar tarefas de configuração no código que você faria no caso contrário, o *Web. config* arquivo. Para obter mais informações, consulte [EntityFramework configuração baseada em código](https://msdn.microsoft.com/data/jj680699).
3. Em *StudentController.cs*, adicione um `using` instrução para `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Alterar todos os `catch` bloqueia esse catch `DataException` exceções para que eles catch `RetryLimitExceededException` exceções em vez disso. Por exemplo:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Você estava usando `DataException` para tentar identificar erros que podem ser transitórios para fornecer uma mensagem amigável "tente novamente". Mas agora que você ativou a uma política de repetição, os únicos erros provavelmente transitório terá já foi tentados e falhou várias vezes e a exceção real retornada será encapsulada no `RetryLimitExceededException` exceção.

Para obter mais informações, consulte [resiliência de Conexão do Entity Framework / lógica de repetição](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Habilitar a intercepção de comando

Agora que você ativou a uma política de repetição, como você testar para verificar se ele está funcionando conforme o esperado? Não é tão fácil forçar um erro transitório acontecer, especialmente quando você estiver executando localmente, e poderá ser especialmente difíceis de integrar erros transitórios reais em um teste de unidade automatizado. Para testar o recurso de resiliência de conexão, você precisa de uma maneira para interceptar consultas do Entity Framework envia para o SQL Server e substitua a resposta do SQL Server com um tipo de exceção que é normalmente transitório.

Você também pode usar a interceptação de consulta para implementar uma prática recomendada para aplicativos em nuvem: [efetuar a latência e o êxito ou a falha de todas as chamadas para os serviços externos](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) como os serviços de banco de dados. EF6 fornece um [dedicado a API de registro em log](https://msdn.microsoft.com/data/dn469464) que pode tornar mais fácil de fazer o registro em log, mas essa seção do tutorial, você aprenderá a usar o Entity Framework [recurso interceptação](https://msdn.microsoft.com/data/dn469464) diretamente, tanto para log e para simular erros transitórios.

### <a name="create-a-logging-interface-and-class"></a>Criar uma interface de log e uma classe

Um [práticas recomendadas para registro em log](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) deve fazê-lo usando uma interface em vez de embutir em código chamadas trace ou uma classe de registro em log. Isso torna mais fácil alterar o mecanismo de registro mais tarde, se você precisar fazer isso. Portanto, nesta seção você criará a interface de log e uma classe para implementar o proprietário. / p > 

1. Crie uma pasta no projeto e nomeie- *log*.
2. No *log* pasta, crie um arquivo de classe chamado *ILogger.cs*e substitua o código de modelo com o código a seguir:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    A interface fornece três níveis de rastreamento para indicar a importância relativa dos logs e um projetada para fornecer informações de latência para chamadas de serviço externo, como consultas de banco de dados. Os métodos de registro em log têm sobrecargas que permitem que você passe em uma exceção. Isso é para que as informações de exceção incluindo exceções interna e o rastreamento de pilha confiável são registradas pela classe que implementa a interface, em vez de usar que está sendo feito em cada chamada de método de registro em log em todo o aplicativo.

    Os métodos TraceApi permitem controlar a latência de cada chamada para um serviço externo, como o banco de dados SQL.
3. No *log* pasta, crie um arquivo de classe chamado *Logger.cs*e substitua o código de modelo com o código a seguir:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    A implementação usa System. Diagnostics para fazer o rastreamento. Este é um recurso interno do .NET que torna mais fácil gerar e usar as informações de rastreamento. Há muitos "ouvintes" você pode usar com o rastreamento de System. Diagnostics para gravar logs de arquivos, por exemplo, ou para gravá-los para o armazenamento de blob no Azure. Algumas das opções e links para outros recursos para obter mais informações, consulte [de solução de problemas do Azure Web Sites no Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Para este tutorial, você só verá logs no Visual Studio **saída** janela.

    Em um aplicativo de produção você talvez queira considerar a pacotes de rastreamento diferente de System. Diagnostics e a interface de ILogger torna relativamente fácil alternar para um mecanismo de rastreamento diferente se você optar por fazer isso.

### <a name="create-interceptor-classes"></a>Criar classes interceptador

Em seguida, você criará as classes que o Entity Framework chamará toda vez que ele irá enviar uma consulta para o banco de dados, uma para simular erros transitórios e outra para fazer o registro em log. Essas classes interceptador devem derivar de `DbCommandInterceptor` classe. Neles, você escreve substituições de métodos que são chamadas automaticamente quando a consulta está prestes a ser executada. Nesses métodos, você pode examinar ou a consulta que está sendo enviada para o banco de dados de log, e você pode alterar a consulta antes de serem enviado para o banco de dados ou retornar algo para o Entity Framework você mesmo sem passar até mesmo a consulta ao banco de dados.

1. Para criar a classe interceptor que registrará todas as consultas SQL que é enviada para o banco de dados, crie um arquivo de classe chamado *SchoolInterceptorLogging.cs* no *DAL* pasta e substitua com o modelo de código o código a seguir:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Para comandos ou consultas com êxito, esse código grava um log de informações com informações de latência. Para exceções, ele cria um log de erros.
2. Para criar a classe interceptor que gerarão erros transitórios fictícios quando você digitar "Throw" no **pesquisa** caixa, crie um arquivo de classe chamado *SchoolInterceptorTransientErrors.cs* no *DAL* pasta e substitua o código de modelo com o código a seguir:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Esse código apenas substituições de `ReaderExecuting` método, que é chamado para consultas que podem retornar várias linhas de dados. Se você quiser verificar resiliência de conexão para outros tipos de consultas, você também pode substituir o `NonQueryExecuting` e `ScalarExecuting` métodos, como o interceptador de log faz.

    Quando você executa a página do aluno e digite "Throw" como a cadeia de caracteres de pesquisa, esse código cria uma exceção de banco de dados SQL fictícia do número de erro 20, um tipo conhecido por ser normalmente transitórios. Outros números de erro reconhecidos atualmente como transitório são 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 e 40613, mas eles estão sujeitos a alterações em novas versões do banco de dados SQL.

    O código retorna a exceção para o Entity Framework em vez de executar a consulta e passando os resultados da consulta voltar. A exceção transitória é retornada quatro vezes e, em seguida, o código será revertido para o procedimento normal de passar a consulta para o banco de dados.

    Como tudo o que está conectado, você poderá ver que o Entity Framework tenta executar a consulta quatro vezes antes de finalmente ter êxito e a única diferença no aplicativo é que ele leva mais tempo para renderizar uma página de resultados da consulta.

    O número de vezes que o Entity Framework tentará é configurável; o código especifica quatro vezes, porque esse é o valor padrão para a política de execução do banco de dados SQL. Se você alterar a política de execução, você precisava também alterar o código que especifica o número de vezes que são gerados erros transitórios. Você também pode alterar o código para gerar mais exceções para que o Entity Framework gerará o `RetryLimitExceededException` exceção.

    O valor inserido na caixa de pesquisa será em `command.Parameters[0]` e `command.Parameters[1]` (um é usado para o nome e outro para o último nome). Quando o valor "Throw %" for encontrado, "Throw" é substituída esses parâmetros por "um" para que alguns alunos serão encontrados e retornados.

    Isso é apenas uma maneira conveniente de resiliência de conexão com base na alteração de uma entrada para o aplicativo da interface do usuário de teste. Você também pode escrever código que gera erros transitórios para todas as consultas ou atualizações, conforme explicado mais adiante os comentários sobre o *DbInterception.Add* método.
3. Em *global. asax*, adicione o seguinte `using` instruções:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Adicione as linhas realçadas para o `Application_Start` método:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Essas linhas de código são o que faz com que seu código interceptador a ser executado quando o Entity Framework envia consultas ao banco de dados. Observe que, como você criou classes interceptador separado para simulação de erro transitório e registro em log, você pode independentemente habilitar e desabilitá-las.

    Você pode adicionar interceptores usando o `DbInterception.Add` método em qualquer lugar no seu código; ele não precisa estar no `Application_Start` método. Outra opção é colocar esse código na classe DbConfiguration que você criou anteriormente para configurar a política de execução.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Onde colocar esse código, tenha cuidado em não para executar `DbInterception.Add` para o mesmo interceptador mais de uma vez, ou você receberá instâncias interceptador adicionais. Por exemplo, se você adicionar duas vezes o interceptador de registro em log, você verá dois logs para cada consulta SQL.

    Interceptores são executados na ordem de registro (a ordem na qual o `DbInterception.Add` método é chamado). A ordem pode importa dependendo do que você está fazendo o interceptador. Por exemplo, um interceptor pode alterar o comando SQL que ele obtém no `CommandText` propriedade. Se isso altera o comando SQL, o próximo interceptador receberá o comando SQL alterados, não o comando SQL original.

    Você gravou o código de erro transitório simulação de uma maneira que permite que causam erros transitórios, inserindo um valor diferente na interface de usuário. Como alternativa, você pode escrever o código de interceptador para gerar sempre a sequência de exceções transitórias sem verificar se há um valor de parâmetro específico. Em seguida, você pode adicionar o interceptador somente quando você deseja gerar erros transitórios. Se você fizer isso, no entanto, não adicione o interceptador até após a inicialização do banco de dados. Em outras palavras, execute a operação de pelo menos um banco de dados, como uma consulta em um de seus conjuntos de entidade antes de começar a gerar erros transitórios. O Entity Framework executa várias consultas durante a inicialização do banco de dados, e eles não são executados em uma transação, para que erros durante a inicialização podem fazer com que o contexto para entrar em um estado inconsistente.

## <a name="test-logging-and-connection-resiliency"></a>Resiliência de registro em log e conexão de teste

1. Pressione F5 para executar o aplicativo no modo de depuração e, em seguida, clique no **alunos** guia.
2. Examinar o Visual Studio **saída** janela para ver a saída de rastreamento. Talvez você precise rolar para cima alguns erros de JavaScript para acessar os logs sejam gravados por seu agente de log anteriores.

    Observe que você pode ver as consultas SQL reais enviadas ao banco de dados. Você ver algumas consultas inicias e comandos que o Entity Framework para começar, verificar a versão do banco de dados e tabela de histórico de migração (você saberá mais sobre a migração no tutorial próximo). E você verá uma consulta para paginação, para descobrir os alunos quantos houver, e, por fim, você pode ver a consulta que obtém os dados do aluno.

    ![O log de consulta normal](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. No **alunos** , digite "Throw" como a cadeia de caracteres de pesquisa e, em **pesquisa**.

    ![Gerar a cadeia de caracteres de pesquisa](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Você observará que o navegador parece parar de responder por alguns segundos durante o Entity Framework está repetindo a consulta várias vezes. A primeira nova tentativa, em seguida, acontece muito rapidamente, a espera antes de aumentar antes de cada repetição adicional. Esse processo de espera mais antes de cada repetição é chamada *retirada exponencial*.

    Quando a página é exibida, mostrando os alunos, que tenham "um" em seus nomes, observe a janela de saída, e você verá que a mesma consulta foi tentada cinco vezes, os primeiros quatro vezes retornando transitório exceções. Para cada erro transitório, você verá o log de gravação ao gerar o erro transitório de `SchoolInterceptorTransientErrors` classe ("retornando erro transitório para comando...") e você verá o log gravado quando `SchoolInterceptorLogging` obtém a exceção.

    ![Saída de log, mostrando as novas tentativas](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Desde que você inseriu uma cadeia de caracteres de pesquisa, a consulta que retorna dados do aluno é parametrizada:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Você não estiver fazendo o valor dos parâmetros, mas você pode fazer isso. Se você quiser ver os valores de parâmetro, você pode escrever código de log para obter valores de parâmetro a `Parameters` propriedade o `DbCommand` objeto obter nos métodos interceptador.

    Observe que você não pode repetir este teste, a menos que você interrompa o aplicativo e reiniciá-lo. Se você quiser ser capaz de testar a resiliência de conexão várias vezes em uma única execução do aplicativo, você pode escrever código para redefinir o contador de erros em `SchoolInterceptorTransientErrors`.
4. Para ver a diferença a estratégia de execução (política de repetição) transforma, comentário a `SetExecutionStrategy` linha *SchoolConfiguration.cs*, execute novamente a página de alunos no modo de depuração e procure "Throw" novamente.

    Neste momento o depurador interrompe na primeira exceção gerada imediatamente ao tentar executar a consulta na primeira vez.

    ![Exceção fictícia](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Remova o *SetExecutionStrategy* linha *SchoolConfiguration.cs*.

## <a name="summary"></a>Resumo

Neste tutorial, você viu como habilitar a resiliência de conexão e comandos SQL que compõe Entity Framework e o envia para o banco de dados de log. O seguinte tutorial, você implantará o aplicativo com a Internet, usando migrações do Code First para implantar o banco de dados.

Deixe comentários em como você gostou neste tutorial e nós poderíamos melhorar. Você também pode solicitar novos tópicos em [Mostrar-Me como com código](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links para outros recursos do Entity Framework podem ser encontradas no [acesso a dados ASP.NET - recomendado recursos](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Anterior](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Próximo](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
