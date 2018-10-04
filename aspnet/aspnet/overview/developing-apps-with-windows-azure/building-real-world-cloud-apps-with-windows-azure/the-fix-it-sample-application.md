---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Apêndice: A correção-aplicativo de exemplo (criação de aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs'
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 435ee61a9c28ad0035457990cd3a889f5b240517
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795532"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Apêndice: A correção-aplicativo de exemplo (criação de aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Baixe a correção-lo do projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Este apêndice para os aplicativos de nuvem construção Real World com o Azure de livro eletrônico contém as seções a seguir fornecem informações adicionais sobre o Fix It aplicativo de exemplo que você pode baixar:

- [Problemas conhecidos](#knownissues)
- [Práticas recomendadas](#bestpractices)
- [Como executar o aplicativo do Visual Studio no computador local](#run-in-vs)
- [Como implantar o aplicativo base para aplicativos de Web do serviço de aplicativo do Azure usando os scripts do Windows PowerShell](#deploybase)
- [Solução de problemas de scripts do Windows PowerShell](#troubleshooting)
- [Como implantar o aplicativo com a fila de processamento para aplicativos de Web do serviço de aplicativo do Azure e um serviço de nuvem do Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problemas conhecidos

O aplicativo Fix It foi originalmente desenvolvido para ilustrar a maneira mais simples possível alguns dos padrões apresentados neste livro. No entanto, como o livro eletrônico é sobre a criação de aplicativos do mundo real, podemos sujeitos o código para corrigi-lo a uma revisão e o processo de teste semelhante ao que seria fazemos para a versão do software. Encontramos uma série de problemas e assim como acontece com qualquer aplicativo do mundo real, algumas delas corrigimos e alguns deles adiado para uma versão posterior.

A lista a seguir inclui problemas que devem ser resolvidos em um aplicativo de produção, mas por um motivo ou outro, decidimos não endereço na versão inicial do que o aplicativo de exemplo Fix It.

### <a name="security"></a>Segurança

- Certifique-se de que você não pode atribuir uma tarefa a um proprietário de inexistente.
- Certifique-se de que você só pode exibir e modificar as tarefas que você criou ou que é atribuídos a você.
- Use HTTPS para páginas de entrada e cookies de autenticação.
- Especifique um tempo limite para cookies de autenticação.

### <a name="input-validation"></a>Validação de entrada

Em geral, um aplicativo de produção faria mais validação de entrada que o aplicativo de corrigi-lo. Por exemplo, o tamanho da imagem / imagem de tamanho de arquivo permitido para carregamento deve ser limitado.

### <a name="administrator-functionality"></a>Funcionalidade de administrador

Um administrador deve ser capaz de alterar a propriedade em tarefas existentes. Por exemplo, o criador de uma tarefa pode deixar a empresa, deixando ninguém com autoridade para manter a tarefa, a menos que o acesso administrativo está habilitado.

### <a name="queue-message-processing"></a>Processamento de mensagens da fila

Mensagem da fila de processamento no aplicativo Fix It foi projetada para ser simples para ilustrar o padrão de trabalho centrado em fila com uma quantidade mínima de código. Esse código simple não seria adequado para um aplicativo de produção real.

- O código não garante que cada mensagem da fila será processada no máximo uma vez. Quando você receber uma mensagem da fila, há um período de tempo limite durante o qual a mensagem é invisível para outros ouvintes de fila. Se o tempo limite expirar antes que a mensagem é excluída, a mensagem se torna visível novamente. Portanto, se uma instância de função de trabalho gasta muito tempo de processamento de uma mensagem, ele é teoricamente possível que a mesma mensagem seja processada duas vezes, resultando em uma tarefa duplicada no banco de dados. Para obter mais informações sobre esse problema, consulte [filas de armazenamento do Azure usando](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- A lógica de sondagem de fila pode ser mais econômica, pela recuperação de mensagens de envio em lote. Sempre que você chame [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), há um custo de transação. Em vez disso, você pode chamar [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (Observe o plural'), que obtém várias mensagens em uma única transação. Os custos de transação para as filas do armazenamento do Azure são muito baixos, portanto, o impacto sobre os custos não é significativo na maioria dos cenários.
- O loop estreito no código de processamento de mensagens de fila faz com que a afinidade de CPU, que não utiliza com eficiência as VMs de vários núcleos. Um design melhor seria usar paralelismo de tarefas para executar várias tarefas assíncronas em paralelo.
- Processamento de mensagens da fila tem o tratamento de exceção apenas rudimentar. Por exemplo, o código não tratará [mensagens suspeitas](https://msdn.microsoft.com/library/ms789028.aspx). (Quando o processamento da mensagem faz com que uma exceção, você precisa registrar o erro e excluir a mensagem, ou a função de trabalho tentará processá-lo novamente, e o loop continuará indefinidamente.)

### <a name="sql-queries-are-unbounded"></a>Consultas SQL são ilimitadas

Corrigir código atual não impõe nenhum limite as consultas para as páginas de índice podem retornar o número de linhas. Se um grande volume de tarefas é inserido no banco de dados, o tamanho das listas resultantes recebida poderá causar problemas de desempenho. A solução é implementar a paginação. Por exemplo, consulte [classificação, filtragem e paginação com o Entity Framework em um aplicativo ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Modelos de exibição recomendados

O aplicativo Fix It usa a classe de entidade FixItTask para passar informações entre o controlador e o modo de exibição. Uma prática recomendada é usar modelos de exibição. O modelo de domínio (por exemplo, a classe de entidade FixItTask) foi projetado em torno do que é necessário para persistência de dados, enquanto um modelo de exibição pode ser projetado para apresentação de dados. Para obter mais informações, consulte [12 ASP.NET MVC práticas recomendadas](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Blob de imagem segura recomendado

Corrigir lojas de aplicativos carregados imagens como público, que significa que qualquer pessoa que encontre a URL pode acessar as imagens. As imagens podem ser protegidas em vez de público.

### <a name="no-powershell-automation-scripts-for-queues"></a>Nenhum script de automação do PowerShell para filas

Scripts de automação do PowerShell de exemplo foram escritos apenas para a versão base do Fix It que é executado totalmente no serviço de aplicativos Web do aplicativo. Ainda não fornecemos scripts para configurar e implantar o aplicativo web além de um ambiente de serviço de nuvem necessária para processamento de fila.

### <a name="special-handling-for-html-codes-in-user-input"></a>Tratamento especial para códigos HTML na entrada do usuário

ASP.NET impede automaticamente várias maneiras em que usuários mal-intencionados podem tentar ataques de script entre sites, inserindo um script em caixas de texto de entrada do usuário. E o MVC `DisplayFor` auxiliar usada para exibir a tarefa de títulos e notas automaticamente valores codifica em HTML que ele envia para o navegador. Mas, em um aplicativo de produção, você talvez queira tomar medidas adicionais. Para obter mais informações, consulte [validação de solicitação no ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Práticas recomendadas

A seguir estão alguns problemas que foram corrigidos depois de serem descobertos na revisão de código e teste da versão original do aplicativo corrigi-lo. Alguns foram causados pelo codificador original não esteja ciente de que uma prática recomendada em particular, alguns simplesmente porque o código foi escrito rapidamente e não foi destinado para a versão do software. Podemos está listando problemas aqui, caso haja alguma coisa que aprendemos com essa revisão e teste que pode ser úteis para outras pessoas que também estão desenvolvendo aplicativos web.

### <a name="dispose-the-database-repository"></a>Descartar o repositório de banco de dados

O `FixItTaskRepository` classe deve descartar o Entity Framework `DbContext` instância. Fizemos isso Implementando `IDisposable` no `FixItTaskRepository` classe:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Observe que o AutoFac automaticamente descartará o `FixItTaskRepository` da instância, portanto, não precisamos explicitamente descartá-lo.

Outra opção é remover o `DbContext` variável de membro de `FixItTaskRepository`e, em vez disso, criar um local `DbContext` variável dentro de cada método de repositório, dentro de um `using` instrução. Por exemplo:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrar singletons assim com a DI

Desde que apenas uma instância das `PhotoService` classe e `Logger` classe é necessária, essas classes devem ser [registrado como instâncias únicas para injeção de dependência](https://code.google.com/p/autofac/wiki/InstanceScope) em *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Segurança: Não mostrar detalhes do erro para os usuários

Fix It aplicativo original não tem uma página de erro genérica e deixar todas as bolhas de exceções até a interface do usuário, portanto, algumas exceções, como erros de conexão de banco de dados pode resultar em um rastreamento de pilha completa que está sendo exibido no navegador. Informações detalhadas do erro, às vezes, podem facilitar a ataques por usuários mal-intencionados. A solução é registrar os detalhes da exceção e exibir uma página de erro para o usuário que não inclui os detalhes do erro. O aplicativo Fix It já foi registro em log e para exibir uma página de erro, adicionamos `<customErrors mode=On>` no arquivo Web. config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Por padrão, isso causa *Views\Shared\Error.cshtml* a ser exibido para erros. Você pode personalizar *Error.cshtml* ou crie seu próprio modo de exibição de página de erro e adicione um `defaultRedirect` atributo. Você também pode especificar diferentes páginas de erro para erros específicos.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Segurança: permitir apenas uma tarefa a ser editado por seu criador

A página de índice do painel mostra apenas as tarefas criadas pelo usuário conectado, mas um usuário mal-intencionado poderia criar uma URL com uma ID para a tarefa de outro usuário. Adicionamos o código em *DashboardController.cs* para retornar um erro 404 nesse caso:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Não assimilar as exceções

Fix It aplicativo original apenas retornou nulo após o registro em log uma exceção que resultou de uma consulta SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Isso tornaria a vista pelo usuário como se a consulta foi bem-sucedida, mas apenas não retornou nenhuma linha. Solução é relançar a exceção depois de capturar e registro em log:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Capturar todas as exceções em funções de trabalho

Todas as exceções sem tratamento em uma função de trabalho fará com que a máquina virtual seja reciclado, portanto, deseja encapsular tudo o que fazer em um bloco try-catch e lidar com todas as exceções.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Especificar o tamanho para propriedades de cadeia de caracteres em classes de entidade

Para exibir o código simples, a versão original do aplicativo Fix It não especificou os comprimentos dos campos da entidade FixItTask e, assim elas foram definidas como varchar (max) no banco de dados. Como resultado, a interface do usuário aceitaria quase qualquer quantidade de entrada. Especificando comprimentos define limites que se aplicam tanto ao usuário de entrada na página da web e tamanho da coluna no banco de dados:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Marcar membros privados como somente leitura quando elas não são esperadas para alterar

Por exemplo, no `DashboardController` classe de uma instância do `FixItTaskRepository` é criado e não deve ser alterada, então a definimos como [readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Use a lista. Any () em vez de lista. Count () &gt; 0

Se você você se preocupa se um ou mais itens em uma lista de acordo com os critérios especificados, use o [qualquer](https://msdn.microsoft.com/library/bb534972.aspx) método, porque ele retorna assim que um item ajustando-se os critérios for encontrado, enquanto o `Count` sempre tem método iterar por meio de cada item. O painel *index. cshtml* arquivo originalmente tinha este código:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Nós o alteramos a este:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Gerar URLs em exibições MVC usando auxiliares do MVC

Para o **criar um Fix It** botão na home page, Fix It aplicativo rígido codificado a um elemento de âncora:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Para obter links de exibição/ação assim é melhor usar o [Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) auxiliar HTML, por exemplo:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Usar Delay, em vez de Sleep em função de trabalho

Coloca o novo modelo de projeto `Thread.Sleep` no exemplo de código para uma função de trabalho, mas fazendo com que o thread em suspensão pode causar o pool de threads gerar segmentos desnecessários adicionais. Você pode evitar isso usando [Delay](https://msdn.microsoft.com/library/hh139096.aspx) em vez disso.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Evitar async void

Se um método assíncrono não precisa retornar um valor, retornam um `Task` tipo em vez de `void`.

Este exemplo é do `FixItQueueManager` classe:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Você deve usar `async void` somente para manipuladores de eventos de nível superior. Se você definir um método como `async void`, o chamador não é possível **await** o método ou capturar todas as exceções que o método gera. Para obter mais informações, consulte [práticas recomendadas na programação assíncrona](https://msdn.microsoft.com/magazine/jj991977.aspx).

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Use um token de cancelamento para interromper o loop de função de trabalho

Normalmente, o **executar** método em uma função de trabalho contém um loop infinito. Quando a função de trabalho está sendo interrompido, o [RoleEntryPoint](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) método é chamado. Você deve usar esse método para cancelar o trabalho que está sendo feito dentro de **executar** método e saia normalmente. Caso contrário, o processo pode ser terminado no meio de uma operação.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Recusar o procedimento de detecção automática de MIME

Em alguns casos, o Internet Explorer relata um tipo MIME diferente do tipo especificado pelo servidor web. Por exemplo, se o Internet Explorer localiza HTML conteúdo em um arquivo fornecido com o cabeçalho de resposta HTTP Content-Type: texto/simples, o Internet Explorer determina se o conteúdo deve ser renderizado como HTML. Infelizmente, esse "detecção de MIME" também pode levar a problemas de segurança para servidores que hospedam conteúdo não confiável. Para combater esse problema, o Internet Explorer 8 fez inúmeras alterações ao código de determinação de tipo de MIME e permite que os desenvolvedores de aplicativos [recusar a detecção de MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). O código a seguir foi adicionado para o *Web. config* arquivo.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Habilitar o agrupamento e minificação

Quando o Visual Studio cria um novo projeto web, agrupamento e minificação de arquivos JavaScript não está habilitado por padrão. Adicionamos uma linha de código no BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Definir um tempo limite de expiração para cookies de autenticação

Por padrão, os cookies de autenticação expirarem em duas semanas. Um tempo mais curto é mais seguro. Você pode alterar essa configuração no *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Como executar o aplicativo do Visual Studio no computador local

Há duas maneiras de executar o aplicativo Fix It:

- Execute o aplicativo básico que grava novas tarefas diretamente no banco de dados SQL.
- Execute o aplicativo usando uma fila além de um serviço de back-end para criar tarefas. O padrão de fila é descrito no capítulo [padrão de trabalho centrado em fila](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Execute o aplicativo de base

1. Instale [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017).
2. Instalar o [SDK do Azure para .NET para o Visual Studio](https://azure.microsoft.com/downloads/).
3. Baixe o arquivo. zip do [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. No Explorador de arquivos, clique com botão direito no arquivo. zip e clique em propriedades, na janela Propriedades, clique em Desbloquear.
5. Descompacte o arquivo.
6. Clique duas vezes no arquivo. sln para iniciar o Visual Studio.
7. No menu Ferramentas, clique em Gerenciador de pacotes de biblioteca e, em seguida, Package Manager Console.
8. No pacote Manager Console (PMC), clique em Restaurar.
9. Saia do Visual Studio.
10. Iniciar o [emulador de armazenamento do Azure](/azure/storage/common/storage-use-emulator).
11. Reinicie o Visual Studio, abrir o arquivo de solução que você fechou na etapa anterior.
12. Verifique se que o projeto do FixIt é definido como o projeto de inicialização e, em seguida, pressione CTRL + F5 para executar o projeto.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Execute o aplicativo com o processamento de fila

1. Siga as instruções para [executar o aplicativo base](#runbase)e, em seguida, feche o navegador e feche o Visual Studio.
2. Inicie o Visual Studio com privilégios de administrador. (Você estará usando o emulador de computação do Azure, e que requer privilégios de administrador).
3. No aplicativo *Web. config* arquivo na *MyFixIt* (no projeto da web) do projeto, altere o valor de `appSettings/UseQueues` como "true":

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Se o [emulador de armazenamento do Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) não está ainda em execução, inicie-o novamente.
5. Execute o projeto de web do FixIt e o projeto MyFixItCloudService simultaneamente.

    Usando o Visual Studio:

   1. Pressione **F5** para executar o projeto FixIt.
   2. Na **Gerenciador de soluções**, clique com botão direito no projeto MyFixItCloudService e, em seguida, clique em **Debug** > **iniciar uma nova instância**.

    Usando o Visual Studio 2013 Express para Web:

   3. No Gerenciador de soluções, a solução do FixIt com o botão direito e selecione **propriedades**.
   4. Selecione **vários projetos de inicialização**.
   5. No **ação** lista suspensa em MyFixIt e MyFixItCloudService, selecione **iniciar**.
   6. Clique em **OK**.
   7. Pressione **F5** para executar os dois projetos.

      Quando você executar o projeto MyFixItCloudService, o Visual Studio inicia o emulador de computação do Azure. Dependendo da configuração do firewall, você precisa permitir que o emulador por meio do firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Como implantar o aplicativo base para aplicativos de Web do serviço de aplicativo do Azure usando os scripts do Windows PowerShell

Para ilustrar os [automatizar tudo](automate-everything.md) padrão, o aplicativo Fix It é fornecido com scripts que configurar um ambiente no Azure e implantar o projeto para o novo ambiente. As instruções a seguir explicam como usar os scripts.

Se você deseja executar no Azure sem o uso de filas, e você fez as alterações para ser executado localmente com filas, verifique se que você definir o valor da appSetting UseQueues volta como false antes de prosseguir com as instruções a seguir.

Essas instruções pressupõem que você já tiver baixado e execute a solução corrigi-lo localmente, e se você tem um Azure da conta ou tem uma assinatura do Azure que você está autorizado a gerenciar.

1. Instalar o **Azure PowerShell** console. Para obter instruções, consulte [como instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Esse console personalizado está configurado para funcionar com sua assinatura do Azure. O módulo do Azure é instalado na *arquivos de programas* diretório e é importado automaticamente em todo o uso do console do PowerShell do Azure.

    Se você preferir trabalhar em um programa de host diferente, como o Windows PowerShell ISE, certifique-se de usar o [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) cmdlet para importar o módulo do Azure ou usar um comando no módulo do Azure para disparar a importação automática do módulo.
2. Inicie o PowerShell do Azure com o **executar como administrador** opção.
3. Execute o [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet para definir a política de execução do PowerShell do Azure para `RemoteSigned`. Insira **Y** (para Sim) para concluir a alteração da política.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Essa configuração permite que você execute scripts locais que não são assinados digitalmente. (Você também pode definir a política de execução `Unrestricted`, o que eliminaria a necessidade para a etapa de desbloqueio, mas isso não é recomendado por motivos de segurança.)
4. Execute o `Add-AzureAccount` cmdlet para configurar o PowerShell com credenciais de sua conta.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Essas credenciais expiram após um período de tempo e você precisará executar novamente o `Add-AzureAccount` cmdlet. Este livro eletrônico está sendo escrito, o limite de tempo antes que as credenciais expiram é 12 horas.
5. Se você tiver várias assinaturas, use o cmdlet Select-AzureSubscription para especificar a assinatura que você deseja criar o ambiente de teste.
6. Importar um certificado de gerenciamento para a mesma assinatura do Azure usando o `Get-AzurePublishSettingsFile` e `Import-AzurePublishSettingsFile` cmdlets. O primeiro desses cmdlets baixa um arquivo de certificado e, em um segundo, você especificar o local desse arquivo para importá-lo. > [!IMPORTANT]
   > Manter o arquivo baixado em um local seguro ou excluí-lo quando você terminar com ele, pois ela contém um certificado que pode ser usado para gerenciar seus serviços do Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    O certificado é usado para uma chamada à API REST que detecta o endereço IP da máquina de desenvolvimento para definir uma regra de firewall no servidor de banco de dados SQL.
7. Execute o [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (aliases são `cd`, `chdir`, e `sl`) para navegar até o diretório que contém os scripts. (Eles estão localizados na *automação* pasta na pasta de solução de corrigi-lo.) Coloque o caminho entre aspas se qualquer um dos nomes de diretório contiver espaços. Por exemplo, para navegar até o `c:\Sample Apps\FixIt\Automation` diretório, você poderia digitar o seguinte comando:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Para permitir que o Windows PowerShell executar esses scripts, use o [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet. (Os scripts estão bloqueados porque eles foram baixados da Internet).

    > [!WARNING]
    > Security - antes de executar `Unblock-File` em qualquer script ou arquivo executável, abra o arquivo no bloco de notas, examine os comandos e verificar que eles não contêm nenhum código mal-intencionado.

    Por exemplo, o comando a seguir executa o `Unblock-File` cmdlet em todos os scripts no diretório atual.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Para criar o aplicativo web para a base (não há filas de processamento) corrigi-lo o aplicativo, execute o script de criação do ambiente.

    Necessário `Name` parâmetro especifica o nome do banco de dados e também é usado para a conta de armazenamento que cria o script. O nome deve ser globalmente exclusivo no domínio azurewebsites.net. Se você especificar um nome que não é exclusivo, como Fixit ou teste (ou até mesmo como no exemplo, fixitdemo), o `New-AzureWebsite` cmdlet falhará com um erro interno que relata um conflito. O script converte o nome para minúsculos para atender aos requisitos de nome para aplicativos web, contas de armazenamento e bancos de dados.

    Necessário `SqlDatabasePassword` parâmetro especifica a senha da conta de administrador que será criada para o banco de dados SQL. Não inclua caracteres XML especiais na senha (&amp; &lt; &gt; ;). Essa é uma limitação da maneira como os scripts foram escritos, não uma limitação do Azure.

    Por exemplo, se você quiser criar um aplicativo web chamado "fixitdemo" e usar uma senha de administrador do SQL Server de "Passw0rd1", você pode inserir o comando a seguir:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    O nome deve ser exclusivo no domínio azurewebsites.net, e a senha deve atender aos requisitos de banco de dados SQL complexidade de senha. (O exemplo Passw0rd1 atender os requisitos).

    Observe que o comando começa com ". \". Para ajudar a impedir a execução de scripts mal-intencionados, o Windows PowerShell exige que você forneça o caminho totalmente qualificado para o arquivo de script quando você executa um script. Você pode usar um ponto para indicar o diretório atual (".\") ou, forneça o caminho totalmente qualificado, como:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Para obter mais informações sobre o script, use o `Get-Help` cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Você pode usar o `Detailed`, `Full`, `Parameters`, e `Examples` parâmetros do cmdlet Get-Help para filtrar a Ajuda que é retornada.

    Se o script falha nem gera erros, como "New-AzureWebsite: chamar Set-AzureSubscription e Select-AzureSubscription primeiro", você talvez não concluiu a configuração do Azure PowerShell.

    Depois que o script for concluído, você pode usar o Portal de gerenciamento do Azure para ver os recursos que foram criados, conforme mostrado na [automatizar tudo](automate-everything.md) capítulo.
10. Para implantar o projeto do FixIt para o novo ambiente do Azure, use o *AzureWebsite.ps1* script. Por exemplo:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Quando a implantação estiver concluída, o navegador é aberto com corrigi-lo em execução no Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Solução de problemas de scripts do Windows PowerShell

Os erros mais comuns encontrados ao executar esses scripts estão relacionados a permissões. Certifique-se de que `Add-AzureAccount` e `Import-AzurePublishSettingsFile` foram bem-sucedidas e usá-los para a mesma assinatura do Azure. Mesmo se `Add-AzureAccount` foi bem-sucedida talvez seja necessário executá-lo novamente. As permissões adicionadas por `Add-AzureAccount` expira em 12 horas.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Referência de objeto não definida para uma instância de um objeto.

Se o script retorna erros, como a execução de "Referência de objeto não definida para uma instância de um objeto," que significa que o Windows PowerShell não é possível localizar um objeto ao processo (essa é uma exceção de referência nula), o `Add-AzureAccount` cmdlet e tente novamente o script.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>Erro interno: O servidor encontrou um erro interno.

O `New-AzureWebsite` cmdlet retorna um erro interno quando o nome não é exclusivo no domínio azurewebsites.net. Para resolver o erro, use um valor diferente para o nome, o que é o parâmetro Name do *New-AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Reiniciar o script

Se você precisa reiniciar o *New-AzureWebsiteEnv.ps1* script porque falhou antes-impresso a mensagem "O Script for concluído", você talvez queira excluir os recursos que o script criado antes que ele é interrompido. Por exemplo, se o script criou o aplicativo web ContosoFixItDemo e executar o script novamente com o mesmo nome, o script falhará porque o nome está em uso.

Para determinar quais recursos o script criado antes de ter parado, use os seguintes cmdlets:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Para executar este cmdlet, canalizar o nome do servidor de banco de dados para `Get-AzureSqlDatabase`:   `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Para excluir esses recursos, use os comandos a seguir. Observe que se você excluir o servidor de banco de dados, você automaticamente exclui os bancos de dados associados ao servidor.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Como implantar o aplicativo com a fila de processamento para aplicativos de Web do serviço de aplicativo do Azure e um serviço de nuvem do Azure

Para habilitar as filas, faça a seguinte alteração no arquivo MyFixIt\Web.config. Sob `appSettings`, altere o valor de `UseQueues` como "true":

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Em seguida, implante o aplicativo MVC em um aplicativo web no serviço de aplicativo do Azure, conforme descrito [anteriores](#deploybase).

Em seguida, crie um novo serviço de nuvem do Azure. Os scripts incluídos com o aplicativo Fix It não criar ou implantar o serviço de nuvem, portanto, você deve usar o portal do Azure para isso. No portal, clique em **New** -- **computação** – **serviço de nuvem** -- **criação rápida**e, em seguida, digite uma URL e um local de data center. Use o mesmo data center onde você implantou o aplicativo web.

![](the-fix-it-sample-application/_static/image1.png)

Antes de implantar o serviço de nuvem, você precisa atualizar alguns dos arquivos de configuração.

No MyFixIt.WorkerRoler\app.config, sob `connectionStrings`, substitua o valor da `appdb` cadeia de caracteres de conexão com a cadeia de caracteres de conexão real do banco de dados SQL. Você pode obter a cadeia de caracteres de conexão do portal. No portal, clique em **bancos de dados SQL** - **appdb** - **cadeias de caracteres de conexão de banco de dados do modo de exibição SQL para ADO .net, ODBC, PHP e JDBC**. Copie a cadeia de caracteres de conexão do ADO.NET e cole o valor no arquivo App. config. Substitua "{sua\_senha\_aqui}" com a senha de banco de dados. (Supondo que você usou os scripts para implantar o aplicativo MVC, você especificou a senha do banco de dados no `SqlDatabasePassword` parâmetro do script.)

O resultado deve ser semelhante ao seguinte:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

No mesmo arquivo MyFixIt.WorkerRoler\app.config, sob `appSettings`, substitua os dois valores de espaço reservado para a conta de armazenamento do Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Você pode obter a chave de acesso no portal. Ver [como gerenciar contas de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

No MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, substitua os mesmos dois valores de espaços reservados para a conta de armazenamento do Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Agora você está pronto para implantar o serviço de nuvem. No Gerenciador de soluções, clique com botão direito no projeto MyFixItCloudService e selecione **publicar**. Para obter mais informações, consulte "[implantar o aplicativo no Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", que é na parte 2 [neste tutorial](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Anterior](more-patterns-and-guidance.md)
