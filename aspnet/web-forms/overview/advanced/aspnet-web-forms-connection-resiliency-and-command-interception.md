---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: Resiliência de Conexão de formulários da Web do ASP.NET e interceptação de comando | Microsoft Docs
author: Erikre
description: Este tutorial descreve como modificar um aplicativo de exemplo para oferecer suporte a resiliência de conexão e interceptação de comando.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: d5c4e46209e1b21a303fdf1fb16c6c868b3ca923
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879693"
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>Resiliência de Conexão de formulários da Web do ASP.NET e interceptação de comando
====================
por [Erik Reitan](https://github.com/Erikre)

Neste tutorial, você modificará o aplicativo de exemplo Wingtip Toys para dar suporte a resiliência de conexão e interceptação de comando. Habilitando a resiliência de conexão, o aplicativo de exemplo Wingtip Toys automaticamente tentará novamente a chamadas de dados quando ocorrem erros transitórios típicos de um ambiente de nuvem. Além disso, com a implementação de interceptação de comando, o aplicativo de exemplo Wingtip Toys irá capturar todas as consultas SQL enviadas ao banco de dados para fazer logon ou alterá-los.

> [!NOTE] 
> 
> Este tutorial de Web Forms baseia tutorial a seguir MVC de Tom Dykstra:  
> [Resiliência de Conexão e interceptação de comando com o Entity Framework em um aplicativo ASP.NET MVC](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>O que você aprenderá:

- Como fornecer resiliência de conexão.
- Como implementar a interceptação de comando.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar, certifique-se de que você tenha o seguinte software instalado em seu computador:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) ou [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). O .NET Framework é instalado automaticamente.
- Wingtip Toys exemplo de projeto, para que você pode implementar a funcionalidade mencionada neste tutorial dentro do projeto Wingtip Toys. O link a seguir fornece detalhes de download:

    - [Introdução ao ASP.NET 4.5.1 Web Forms - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (c#)
- Antes de concluir este tutorial, considere a possibilidade de revisar a série de tutoriais relacionado, [Introdução ao Web Forms do ASP.NET 4.5 e o Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). A série de tutoriais ajudarão você a se familiarizar com o **WingtipToys** projeto e o código.

## <a name="connection-resiliency"></a>Resiliência da conexão

Ao considerar a implantação de um aplicativo no Windows Azure, uma opção a ser considerada é implantando o banco de dados **Windows** **banco de dados do SQL Azure**, um serviço de banco de dados de nuvem. Erros transitórios de conexão são geralmente mais frequentes, quando você se conectar a um serviço de banco de dados de nuvem que quando o servidor web e o servidor de banco de dados estão diretamente conectados juntos no mesmo data center. Mesmo se um servidor de web de nuvem e um serviço de banco de dados de nuvem são hospedadas no mesmo data center, há mais conexões de rede entre elas que pode ter problemas, como balanceadores de carga.

Também um serviço de nuvem é normalmente compartilhado por outros usuários, que significa que sua capacidade de resposta pode ser afetada por eles. E seu acesso ao banco de dados pode estar sujeito a limitação. Limitação significa que o serviço de banco de dados gera exceções quando você tentar acessá-lo com mais frequência do que o permitido na sua *contrato de nível de serviço* (SLA).

Muitos ou a maioria dos problemas de conexão que ocorrem quando você estiver acessando um serviço de nuvem são transitórios, ou seja, se eles resolvem em um curto período de tempo. Então quando você tentar uma operação de banco de dados e obter um tipo de erro que é normalmente transitório, poderia tente a operação novamente depois que uma espera de curta e a operação seja bem-sucedida. Você pode fornecer uma melhor experiência para os usuários se tratar erros transitórios automaticamente tentando novamente, tornando a maioria deles invisível para o cliente. O recurso de resiliência de conexão no Entity Framework 6 automatiza o processo de repetição falha consultas SQL.

O recurso de resiliência de conexão deve ser configurado corretamente para um serviço de banco de dados específico:

1. É necessário saber quais exceções são provavelmente transitório. Você deseja repetir erros causados por perda temporária de conectividade de rede, não os erros causados por erros de programa, por exemplo.
2. Ele deve esperar uma quantidade apropriada de tempo entre as repetições de uma operação com falha. Você pode esperar mais entre as tentativas de um processo em lote do que para uma página da web online em que um usuário está aguardando uma resposta.
3. Ele tem que repetir um número apropriado de vezes antes de desistir. Você talvez queira repetir mais vezes em um processo em lote que você faria em um aplicativo online.

Você pode definir essas configurações manualmente para qualquer ambiente de banco de dados com suporte por um provedor do Entity Framework.

Você precisa fazer para habilitar a resiliência de conexão é criar uma classe em seu assembly que deriva de `DbConfiguration` classe e, na classe, defina a estratégia de execução de banco de dados SQL, que é outro termo para política de repetição no Entity Framework.

### <a name="implementing-connection-resiliency"></a>Implementando a resiliência de conexão

1. Baixe e abra o [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) exemplo de aplicativo de Web Forms no Visual Studio.
2. No *lógica* pasta o **WingtipToys** aplicativo, adicione um arquivo de classe chamado *WingtipToysConfiguration.cs*.
3. Substitua o código existente pelo seguinte código:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

O Entity Framework executa automaticamente o código encontrados em uma classe que deriva de `DbConfiguration`. Você pode usar o `DbConfiguration` classe para realizar tarefas de configuração no código que você faria no caso contrário, o *Web. config* arquivo. Para obter mais informações, consulte [EntityFramework configuração baseada em código](https://msdn.microsoft.com/data/jj680699).

1. No *lógica* pasta, abra o *AddProducts.cs* arquivo.
2. Adicionar um `using` instrução `System.Data.Entity.Infrastructure` conforme realçada em amarelo:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Adicionar um `catch` bloquear o `AddProduct` método para que o `RetryLimitExceededException` é registrado como realçada em amarelo:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Adicionando o `RetryLimitExceededException` exceção, você pode fornecer melhor log ou exibir uma mensagem de erro para o usuário onde eles podem escolher tente o processo novamente. Capturando o `RetryLimitExceededException` exceção, os únicos erros provavelmente transitório terá já foi tentada e falhou várias vezes. A exceção real retornada será encapsulada no `RetryLimitExceededException` exceção. Além disso, você também adicionou um bloco catch geral. Para obter mais informações sobre o `RetryLimitExceededException` exceção, consulte [resiliência de Conexão do Entity Framework / lógica de repetição](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Interceptação de comando

Agora que você ativou a uma política de repetição, como você testar para verificar se ele está funcionando conforme o esperado? Não é tão fácil forçar um erro transitório acontecer, especialmente quando você estiver executando localmente, e poderá ser especialmente difíceis de integrar erros transitórios reais em um teste de unidade automatizado. Para testar o recurso de resiliência de conexão, você precisa de uma maneira para interceptar consultas do Entity Framework envia para o SQL Server e substitua a resposta do SQL Server com um tipo de exceção que é normalmente transitório.

Você também pode usar a interceptação de consulta para implementar uma prática recomendada para aplicativos em nuvem: log a latência e o êxito ou a falha de todas as chamadas externa para os serviços, como serviços de banco de dados.

Esta seção do tutorial, você usará o Entity Framework [ *recurso interceptação* ](https://msdn.microsoft.com/data/dn469464) para registro em log e para simular erros transitórios.

### <a name="create-a-logging-interface-and-class"></a>Criar uma interface de log e uma classe

É uma prática recomendada para registro em log fazer isso usando um [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) em vez de embutir em código chamadas para `System.Diagnostics.Trace` ou uma classe de registro em log. Isso torna mais fácil alterar o mecanismo de registro mais tarde, se você precisar fazer isso. Portanto nesta seção, você criará a interface de log e uma classe para implementá-lo.

Com base no procedimento acima, você tiver baixado e abrir o **WingtipToys** exemplo de aplicativo no Visual Studio.

1. Criar uma pasta no **WingtipToys** do projeto e nomeie-o *log*.
2. No *log* pasta, crie um arquivo de classe chamado *ILogger.cs* e substitua o código padrão pelo seguinte código:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   A interface fornece três níveis de rastreamento para indicar a importância relativa dos logs e um projetada para fornecer informações de latência para chamadas de serviço externo, como consultas de banco de dados. Os métodos de registro em log têm sobrecargas que permitem que você passe em uma exceção. Isso é para que as informações de exceção incluindo exceções interna e o rastreamento de pilha confiável são registradas pela classe que implementa a interface, em vez de usar que está sendo feito em cada chamada de método de registro em log em todo o aplicativo.  
  
   O `TraceApi` métodos permitem rastrear a latência de cada chamada para um serviço externo, como o banco de dados SQL.
3. No *log* pasta, crie um arquivo de classe chamado *Logger.cs* e substitua o código padrão pelo seguinte código:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

A implementação usa `System.Diagnostics` para fazer o rastreamento. Este é um recurso interno do .NET que torna mais fácil gerar e usar as informações de rastreamento. Existem várias &quot;ouvintes&quot; você pode usar com `System.Diagnostics` rastreamento, para gravar logs de arquivos, por exemplo, ou para gravá-los para o armazenamento de blob no Windows Azure. Algumas das opções e links para outros recursos para obter mais informações, consulte [Solucionando problemas de Windows Azure Web Sites no Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Para este tutorial, você só verá logs no Visual Studio **saída** janela.

Em um aplicativo de produção você talvez queira considerar o uso de estruturas de rastreamento diferente `System.Diagnostics`e o `ILogger` interface torna relativamente fácil alternar para um mecanismo de rastreamento diferente se você optar por fazer isso.

### <a name="create-interceptor-classes"></a>Criar classes interceptador

Em seguida, você criará as classes que o Entity Framework chamará toda vez que ele irá enviar uma consulta para o banco de dados, uma para simular erros transitórios e outra para fazer o registro em log. Essas classes interceptador devem derivar de `DbCommandInterceptor` classe. -Los, você escreve as substituições de método chamadas automaticamente quando a consulta está prestes a ser executada. Nesses métodos, você pode examinar ou a consulta que está sendo enviada para o banco de dados de log, e você pode alterar a consulta antes de serem enviado para o banco de dados ou retornar algo para o Entity Framework você mesmo sem passar até mesmo a consulta ao banco de dados.

1. Para criar a classe interceptor que registrará todas as consultas SQL antes de serem enviado para o banco de dados, crie um arquivo de classe chamado *InterceptorLogging.cs* no *lógica* pasta e substitua o padrão de código com o código a seguir:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Para comandos ou consultas com êxito, esse código grava um log de informações com informações de latência. Para exceções, ele cria um log de erros.
2. Para criar a classe interceptor que gerarão erros transitórios fictícios quando você insere &quot;gerar&quot; no **nome** caixa de texto na página chamada *AdminPage.aspx*, crie uma classe arquivo chamado *InterceptorTransientErrors.cs* no *lógica* pasta e substitua o padrão de código com o código a seguir:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Esse código apenas substituições de `ReaderExecuting` método, que é chamado para consultas que podem retornar várias linhas de dados. Se você quiser verificar resiliência de conexão para outros tipos de consultas, você também pode substituir o `NonQueryExecuting` e `ScalarExecuting` métodos, como o interceptador de log faz.  
  
   Posteriormente, você faça logon como o "administrador" e selecione o **Admin** link na barra de navegação superior. Em seguida, no *AdminPage.aspx* página você adicionará um produto chamado &quot;gerar&quot;. O código cria uma exceção de banco de dados SQL fictícia do número de erro 20, um tipo conhecido por ser normalmente transitórios. Outros números de erro reconhecidos atualmente como transitório são 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 e 40613, mas eles estão sujeitos a alterações em novas versões do banco de dados SQL. O produto será renomeado para "TransientErrorExample", você pode seguir no código do *InterceptorTransientErrors.cs* arquivo.  
  
   O código retorna a exceção para o Entity Framework em vez de executar a consulta e passando os resultados. A exceção transitória é retornada *quatro* vezes, e, em seguida, o código é revertida para o procedimento normal de passar a consulta para o banco de dados.

    Como tudo o que está conectado, você poderá ver que o Entity Framework tenta executar a consulta quatro vezes antes de finalmente ter êxito e a única diferença no aplicativo é que ele leva mais tempo para renderizar uma página de resultados da consulta.  
  
   O número de vezes que o Entity Framework tentará é configurável; o código especifica quatro vezes, porque esse é o valor padrão para a política de execução do banco de dados SQL. Se você alterar a política de execução, você precisava também alterar o código que especifica o número de vezes que são gerados erros transitórios. Você também pode alterar o código para gerar mais exceções para que o Entity Framework gerará o `RetryLimitExceededException` exceção.
3. Em *global. asax*, adicione o seguinte usando instruções:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Em seguida, adicione as linhas destacadas para o `Application_Start` método:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Essas linhas de código são o que faz com que seu código interceptador a ser executado quando o Entity Framework envia consultas ao banco de dados. Observe que, como você criou classes interceptador separado para simulação de erro transitório e registro em log, você pode independentemente habilitar e desabilitá-las.   
  
 Você pode adicionar interceptores usando o `DbInterception.Add` método em qualquer lugar no seu código; ele não precisa estar no `Application_Start` método. Outra opção, se você não adicionar interceptadores no `Application_Start` seria método, como atualizar ou adicionar a classe denominada *WingtipToysConfiguration.cs* e coloque o código acima no final do construtor de `WingtipToysbConfiguration` classe.

Onde colocar esse código, tenha cuidado em não para executar `DbInterception.Add` para o mesmo interceptador mais de uma vez, ou você receberá instâncias interceptador adicionais. Por exemplo, se você adicionar duas vezes o interceptador de registro em log, você verá dois logs para cada consulta SQL.

Interceptores são executados na ordem de registro (a ordem na qual o `DbInterception.Add` método é chamado). A ordem pode importa dependendo do que você está fazendo o interceptador. Por exemplo, um interceptor pode alterar o comando SQL que ele obtém no `CommandText` propriedade. Se isso altera o comando SQL, o próximo interceptador receberá o comando SQL alterados, não o comando SQL original.

Você gravou o código de erro transitório simulação de uma maneira que permite que causam erros transitórios, inserindo um valor diferente na interface de usuário. Como alternativa, você pode escrever o código de interceptador para gerar sempre a sequência de exceções transitórias sem verificar se há um valor de parâmetro específico. Em seguida, você pode adicionar o interceptador somente quando você deseja gerar erros transitórios. Se você fizer isso, no entanto, não adicione o interceptador até após a inicialização do banco de dados. Em outras palavras, execute a operação de pelo menos um banco de dados, como uma consulta em um de seus conjuntos de entidade antes de começar a gerar erros transitórios. O Entity Framework executa várias consultas durante a inicialização do banco de dados, e eles não são executados em uma transação, para que erros durante a inicialização podem fazer com que o contexto para entrar em um estado inconsistente.

## <a name="test-logging-and-connection-resiliency"></a>Resiliência de registro em log e conexão de teste

1. No Visual Studio, pressione **F5** para executar o aplicativo no modo de depuração e, em seguida, faça logon como "Admin" com "Pa$ $word" a senha.
2. Selecione **Admin** na barra de navegação na parte superior.
3. Insira um novo produto chamado "Throw" com o arquivo de imagem, descrição e preço apropriado.
4. Pressione a **adicionar produto** botão.  
   Você observará que o navegador parece parar de responder por alguns segundos durante o Entity Framework está repetindo a consulta várias vezes. A primeira nova tentativa ocorre rapidamente, em seguida, aumenta o tempo de espera antes de cada repetição adicional. Esse processo de espera mais antes de cada repetição é chamada *retirada exponencial* .
5. Aguarde até que a página não está mais atttempting para carregar.
6. Parar o projeto e examinar o Visual Studio **saída** janela para ver a saída de rastreamento. Você pode encontrar o **saída** selecionando **depurar**  - &gt; **Windows**  - &gt;  **Saída**. Talvez você precise rolar após vários outros logs gravados pelo seu agente de log.  
  
   Observe que você pode ver as consultas SQL reais enviadas ao banco de dados. Você verá algumas consultas inicias e os comandos que o Entity Framework para começar, verificação da tabela de histórico de versão e a migração do banco de dados.   
    ![Janela de Saída](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Observe que você não pode repetir este teste, a menos que você interrompa o aplicativo e reiniciá-lo. Se você quiser ser capaz de testar a resiliência de conexão várias vezes em uma única execução do aplicativo, você pode escrever código para redefinir o contador de erros em `InterceptorTransientErrors` .
7. Para ver a diferença a estratégia de execução (política de repetição) transforma, comentário a `SetExecutionStrategy` linha *WingtipToysConfiguration.cs* do arquivo no *lógica* pasta, execute o **Admin**  página no modo de depuração novamente e adicione o produto chamado &quot;gerar&quot; novamente.  
  
   Neste momento o depurador interrompe na primeira exceção gerada imediatamente ao tentar executar a consulta na primeira vez.  
    ![Depuração - exibir detalhes](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Remova o `SetExecutionStrategy` linha o *WingtipToysConfiguration.cs* arquivo.

## <a name="summary"></a>Resumo

Neste tutorial, você viu como modificar um aplicativo de exemplo de formulários da Web para dar suporte a resiliência de conexão e interceptação de comando.

## <a name="next-steps"></a>Próximas etapas

Depois que você revisou resiliência de conexão e interceptação de comando em Web Forms do ASP.NET, consulte o tópico de Web Forms do ASP.NET [métodos assíncronos no ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). O tópico ensina os conceitos básicos da criação de um aplicativo ASP.NET Web Forms assíncrono usando o Visual Studio.
