---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Apêndice: A correção-aplicativo de exemplo (compilação de aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs'
author: MikeWasson
description: Os aplicativos de nuvem criando Real World com livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que ele...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 9a1fa36b34c4783b101bb27bc6931241e9251e10
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876469"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Apêndice: A correção-aplicativo de exemplo (compilação de aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Baixe a correção-projeto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> O **nuvem aplicativos do mundo Real criando com o Azure** livro eletrônico baseia-se em uma apresentação desenvolvida por Scott Guthrie. Explica as 13 padrões e práticas que podem ajudá-lo a ser bem-sucedido de desenvolvimento de aplicativos da web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [primeiro capítulo](introduction.md).


Este apêndice a aplicativos de nuvem criando Real World com livro eletrônico do Azure contém as seções a seguir fornecem informações adicionais sobre como corrigir aplicativo de exemplo que você pode baixar:

- [Problemas conhecidos](#knownissues)
- [Práticas recomendadas](#bestpractices)
- [Como executar o aplicativo do Visual Studio no computador local](#run-in-vs)
- [Como implantar o aplicativo base para aplicativos de Web do serviço de aplicativo do Azure usando os scripts do Windows PowerShell](#deploybase)
- [Solucionando problemas de scripts do Windows PowerShell](#troubleshooting)
- [Como implantar o aplicativo com a fila de processamento para aplicativos de Web do serviço de aplicativo do Azure e um serviço de nuvem do Azure](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Problemas conhecidos

O aplicativo corrigir foi originalmente desenvolvido para ilustrar como mais simples possível alguns dos padrões apresentados neste livro eletrônico. No entanto, como o livro eletrônico é sobre a criação de aplicativos do mundo real, estamos sujeitos código corrigir para revisão e processo de teste semelhante ao que seria fazemos para software lançado. Encontramos um número de problemas e, assim como acontece com qualquer aplicativo do mundo real, algumas delas corrigimos e algumas delas adiada para uma versão posterior.

A lista a seguir inclui os problemas que devem ser tratados em um aplicativo de produção, mas por um motivo ou outro que decidimos não endereço na versão inicial do aplicativo exemplo corrigi-lo.

### <a name="security"></a>Segurança

- Certifique-se de que você não pode atribuir uma tarefa a um proprietário inexistente.
- Certifique-se de que você só pode exibir e modificar as tarefas que você criou ou atribuídos a você.
- Use HTTPS para páginas de entrada e os cookies de autenticação.
- Especifique um tempo limite de cookies de autenticação.

### <a name="input-validation"></a>Validação de entrada

Em geral, um aplicativo de produção faria mais validação de entrada que o aplicativo para corrigi-lo. Por exemplo, o tamanho da imagem / imagem de tamanho de arquivo permitido para carregamento deve ser limitado.

### <a name="administrator-functionality"></a>Funcionalidade de administrador

Um administrador deve ser capaz de alterar a propriedade de tarefas existentes. Por exemplo, o criador de uma tarefa pode deixar a empresa, deixando ninguém com autoridade para manter a tarefa, a menos que o acesso administrativo está habilitado.

### <a name="queue-message-processing"></a>Processamento de mensagem da fila

Mensagem da fila de processamento no aplicativo corrigir foi projetada para ser simples para ilustrar o padrão de trabalho centrado em fila com uma quantidade mínima de código. Esse código simple não seria adequado para um aplicativo de produção real.

- O código não garante que cada mensagem da fila será processada no máximo uma vez. Quando você receber uma mensagem da fila, há um período de tempo limite, durante o qual a mensagem está invisível para outros ouvintes de fila. Se o tempo limite expirar antes que a mensagem é excluída, a mensagem se torna visível novamente. Portanto, se uma instância de função de trabalho gasta muito tempo processando uma mensagem, é teoricamente possível para a mesma mensagem ser processado duas vezes, resultando em uma tarefa duplicada no banco de dados. Para obter mais informações sobre esse problema, consulte [filas de armazenamento do Azure usando](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- A lógica de sondagem de fila pode ser mais econômica, pela recuperação de mensagens de envio em lote. Toda vez que você chamar [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), há um custo de transações. Em vez disso, você pode chamar [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (Observe o plural'), que obtém várias mensagens em uma única transação. Os custos de transações para filas de armazenamento do Azure são muito baixos, portanto, o impacto sobre os custos não é significativo na maioria dos cenários.
- O loop estreito no código de processamento de mensagens de fila faz com que a afinidade de CPU, o que não utilizam as VMs de vários núcleos com eficiência. Um melhor design usaria o paralelismo de tarefas para executar várias tarefas assíncronas em paralelo.
- Processamento de mensagens da fila tem o tratamento de exceção apenas rudimentares. Por exemplo, o código não manipula [mensagens suspeitas](https://msdn.microsoft.com/library/ms789028.aspx). (Quando o processamento da mensagem faz com que uma exceção, você precisa registrar o erro e excluir a mensagem, ou a função de trabalho tentará processá-la novamente, e o loop continuará indefinidamente.)

### <a name="sql-queries-are-unbounded"></a>Consultas SQL são não associadas

Corrigir código atual não impõe nenhum limite as consultas de páginas de índice podem retornar o número de linhas. Se um grande volume de tarefas é inserido no banco de dados, o tamanho das listas resultantes recebida pode causar problemas de desempenho. A solução é implementar a paginação. Para obter um exemplo, consulte [classificação, filtragem e paginação com o Entity Framework em um aplicativo ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Exibir modelos recomendados

O aplicativo corrigir usa a classe da entidade FixItTask para transmitir informações entre o controlador e o modo de exibição. Uma prática recomendada é usar modelos de exibição. O modelo de domínio (por exemplo, a classe da entidade FixItTask) foi projetado para o que é necessário para a persistência de dados, enquanto um modelo de exibição pode ser projetado para apresentação de dados. Para obter mais informações, consulte [12 ASP.NET MVC práticas recomendadas](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Blob de imagem seguro recomendado

As lojas de aplicativos corrigir carregado imagens como público, o que significa que qualquer pessoa que localiza a URL pode acessar as imagens. As imagens poderiam ser protegidas em vez de público.

### <a name="no-powershell-automation-scripts-for-queues"></a>Nenhum script de automação do PowerShell para filas

Scripts de automação do PowerShell de exemplo foram desenvolvidos apenas para a versão base da Fix It executado totalmente em aplicativos de Web do serviço de aplicativo do Azure. Ainda não fornecemos scripts para configurar e implantar para o aplicativo web além de ambiente de serviço de nuvem necessária para processamento de fila.

### <a name="special-handling-for-html-codes-in-user-input"></a>Tratamento especial para os códigos HTML na entrada do usuário

ASP.NET impede automaticamente muitas maneiras em que usuários mal-intencionados podem tentar ataques de script entre sites inserindo o script em caixas de texto de entrada do usuário. E o MVC `DisplayFor` auxiliar usada para exibir tarefas títulos e notas automaticamente valores codifica o HTML que ele envia para o navegador. Mas, em um aplicativo de produção, você talvez queira adotar medidas adicionais. Para obter mais informações, consulte [validação de solicitação no ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Práticas recomendadas

A seguir estão alguns problemas que foram corrigidos depois de ser descobertos na revisão de código e teste da versão original do aplicativo corrigi-lo. Alguns foram causados pelo codificador original não percebam determinada melhor prática, alguns simplesmente porque o código foi criado rapidamente e não foi destinado a software lançado. Nós estiver listando os problemas aqui caso ocorra algo que aprendemos com essa revisão e teste que pode ser útil para outras pessoas que também está desenvolvendo aplicativos web.

### <a name="dispose-the-database-repository"></a>Descartar o repositório de banco de dados

O `FixItTaskRepository` classe deve descartar o Entity Framework `DbContext` instância. Isso ocorria implementando `IDisposable` no `FixItTaskRepository` classe:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Observe que AutoFac será descarta automaticamente a `FixItTaskRepository` instância, portanto, não precisamos dispose-lo explicitamente.

Outra opção é remover o `DbContext` variável de membro de `FixItTaskRepository`e em vez disso, crie um local `DbContext` variável dentro de cada método de repositório, dentro de um `using` instrução. Por exemplo:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrar singletons assim com DI

Desde que apenas uma instância do `PhotoService` classe e `Logger` classe é necessária, essas classes devem ser [registrado como instâncias únicas para injeção de dependência](https://code.google.com/p/autofac/wiki/InstanceScope) na *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Segurança: Não exibir detalhes do erro para os usuários

O corrigir aplicativo original não tem uma página de erro genérica e deixar todas as bolhas exceções até a interface do usuário, para que algumas exceções, como erros de conexão de banco de dados podem resultar em um rastreamento de pilha completos que está sendo exibido no navegador. Informações de erro detalhadas, às vezes, podem facilitar a ataques por usuários mal-intencionados. A solução é registrar em log os detalhes de exceção e exibir uma página de erro para o usuário que não inclui detalhes do erro. O aplicativo corrigir já foi o log e para exibir uma página de erro, adicionamos `<customErrors mode=On>` no arquivo Web. config.

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Por padrão, isso faz *Views\Shared\Error.cshtml* a ser exibido para erros. Você pode personalizar *Error.cshtml* ou criar seu próprio modo de exibição de página de erro e adicionar um `defaultRedirect` atributo. Você também pode especificar diferentes páginas de erro para erros específicos.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Segurança: permitir que somente uma tarefa a ser editado pelo seu criador

A página de índice de painel mostra apenas tarefas criadas pelo usuário conectado, mas um usuário mal-intencionado pode criar uma URL com uma ID de tarefa do outro usuário. Adicionamos código *DashboardController.cs* para retornar 404 nesse caso:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Não assimilar exceções

O corrigir aplicativo original apenas retornou nulo após o logon a uma exceção que resultou de uma consulta SQL:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Isso tornaria Vista pelo usuário como se a consulta foi bem-sucedida, mas apenas não retornou nenhuma linha. A solução é para gerar novamente a exceção após capturando e registro em log:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Capturar todas as exceções em funções de trabalho

Todas as exceções sem tratamento em uma função de trabalho fará com que a máquina virtual seja reciclado, então você deseja incluir tudo o que fazer em um bloco try-catch e tratar todas as exceções.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Especificar o tamanho para propriedades de cadeia de caracteres em classes de entidade

Para exibir código simples, a versão original do aplicativo corrigir não especifica tamanhos para os campos da entidade FixItTask e como resultado, eles foram definidos como varchar (max) no banco de dados. Como resultado, a interface do usuário aceite praticamente qualquer valor de entrada. Especificando comprimentos define limites que se aplicam tanto ao usuário de entrada na página da web e tamanho da coluna no banco de dados:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Marcar membros particulares como somente leitura quando elas não são esperadas para alterar

Por exemplo, o `DashboardController` classe uma instância do `FixItTaskRepository` é criada e não é esperado para alterar, então a definimos como [readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Use a lista. Any () em vez de lista. Contagem &gt; 0

Se você importante é se um ou mais itens em uma lista de ajustam os critérios especificados, use o [qualquer](https://msdn.microsoft.com/library/bb534972.aspx) método, porque retorna assim que um ajuste os critérios de item for encontrado, enquanto o `Count` sempre tem método iterar em cada item. O painel *cshtml* arquivo originalmente tinha este código:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

É alterado para isso:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Gerar URLs nas exibições do MVC usando auxiliares do MVC

Para o **criar um Fix It** botão na home page, corrigir aplicativo rígido codificado um elemento âncora:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Para links de ação/exibição como isso é melhor usar o [URL](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) auxiliar HTML, por exemplo:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Use Task.Delay em vez de thread. Sleep em função de trabalho

O modelo de projeto novo coloca `Thread.Sleep` no exemplo de código para uma função de trabalho, mas fazendo com que o thread em suspensão pode causar o pool de threads gerar mais threads desnecessários. Você pode evitar que usando [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) em vez disso.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Evite async void

Se um método assíncrono não precisa retornar um valor, retornar um `Task` tipo em vez de `void`.

Este exemplo é proveniente de `FixItQueueManager` classe: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Você deve usar `async void` apenas para manipuladores de eventos de nível superior. Se você definir um método como `async void`, o chamador não é possível **await** o método ou capturar qualquer exceção que o método gera. Para obter mais informações, consulte [as práticas recomendadas de programação assíncrona](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Use um token de cancelamento para quebra de loop de função de trabalho

Normalmente, o **executar** método em uma função de trabalho contém um loop infinito. Quando a função de trabalho está sendo interrompido, o [RoleEntryPoint](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) método é chamado. Você deve usar esse método para cancelar o trabalho que está sendo feito dentro de **executar** método e sair normalmente. Caso contrário, o processo pode ser finalizado no meio de uma operação.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Recusar o procedimento de detecção automática de MIME

Em alguns casos, o Internet Explorer relata um tipo MIME diferente do tipo especificado pelo servidor web. Por exemplo, se o Internet Explorer encontrar HTML conteúdo em um arquivo fornecido com o cabeçalho de resposta HTTP Content-Type: texto/sem formatação, o Internet Explorer determina se o conteúdo deve ser renderizado como HTML. Infelizmente, esse "MIME-sniffing" também pode levar a problemas de segurança para servidores que hospedam o conteúdo não confiável. Para combater esse problema, o Internet Explorer 8 fez algumas alterações ao código de determinação do tipo de MIME e permite que os desenvolvedores de aplicativos [recusar a detecção de MIME](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). O código a seguir foi adicionado para o *Web. config* arquivo.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Habilitar o empacotamento e minimização

Quando o Visual Studio cria um novo projeto web, empacotamento e minimização dos arquivos JavaScript não está habilitado por padrão. Adicionamos uma linha de código em BundleConfig.cs:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Definir um tempo limite de expiração de cookies de autenticação

Por padrão, os cookies de autenticação expiram em duas semanas. Um tempo mais curto é mais seguro. Você pode alterar essa configuração no *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Como executar o aplicativo do Visual Studio no computador local

Há duas maneiras de executar o aplicativo corrigir:

- Execute o aplicativo de base que grava novas tarefas diretamente ao banco de dados SQL.
- Execute o aplicativo usando uma fila de adição de um serviço de back-end para criar tarefas. O padrão de fila é descrito no capítulo [padrão centrado em fila de trabalho](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Executar o aplicativo básico

1. Instalar [Visual Studio 2013 ou o Visual Studio 2013 Express para Web](https://www.visualstudio.com/downloads).
2. Instalar o [SDK do Azure para .NET para Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. Baixe o arquivo. zip do [Galeria de códigos do MSDN](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. No Explorador de arquivos, clique com botão direito no arquivo. zip e clique em propriedades, na janela Propriedades, clique em Desbloquear.
5. Descompacte o arquivo.
6. Clique duas vezes no arquivo. sln para iniciar o Visual Studio.
7. No menu Ferramentas, clique em Gerenciador de biblioteca de pacote e, em seguida, Package Manager Console.
8. No pacote Manager Console (PMC), clique em Restaurar.
9. Saia do Visual Studio.
10. Iniciar o [emulador de armazenamento do Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Reinicie o Visual Studio, abrir o arquivo de solução que você encerrou na etapa anterior.
12. Verifique se que o projeto FixIt está definido como o projeto de inicialização e, em seguida, pressione CTRL + F5 para executar o projeto.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Executar o aplicativo de processamento de fila

1. Siga as orientações para [executar o aplicativo base](#runbase)e, em seguida, feche o navegador e feche o Visual Studio.
2. Inicie o Visual Studio com privilégios de administrador. (Você vai usar o emulador de computação do Azure, e que requer privilégios de administrador).
3. No aplicativo *Web. config* arquivo o *MyFixIt* (o projeto da web) do projeto, altere o valor de `appSettings/UseQueues` como "true": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Se o [emulador de armazenamento do Azure](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) não está em execução, inicie-o novamente.
5. Execute o projeto web FixIt e o projeto MyFixItCloudService simultaneamente.

    Usando o Visual Studio 2013:

   1. Pressione F5 para executar o projeto FixIt.
   2. Em **Solution Explorer**, com o botão direito no projeto MyFixItCloudService e, em seguida, clique em **depurar** -- **iniciar uma nova instância**.

      Usando o Visual Studio 2013 Express para Web:

   3. No Gerenciador de soluções, a solução FixIt e selecione **propriedades**.
   4. Selecione **vários projetos de inicialização**...
   5. No **ação** selecione da lista suspensa em MyFixIt e MyFixItCloudService, **iniciar**.
   6. Clique em **OK**.
   7. Pressione F5 para executar os dois projetos.

      Quando você executar o projeto MyFixItCloudService, o Visual Studio inicia o emulador de computação do Azure. Dependendo da configuração do firewall, você precisará permitir que o emulador através do firewall.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Como implantar o aplicativo base para aplicativos de Web do serviço de aplicativo do Azure usando os scripts do Windows PowerShell

Para ilustrar o [automatizar tudo](automate-everything.md) padrão, o aplicativo corrigir é fornecido com scripts que configurar um ambiente no Azure e implantar o projeto para o novo ambiente. As instruções a seguir explicam como usar os scripts.

Se você deseja executar no Azure sem o uso de filas, e você fez as alterações para executar localmente com filas, verifique se que você definir o valor de appSetting UseQueues para false antes de prosseguir com as instruções a seguir.

Essas instruções presumem que você já tiver baixado e executar a solução corrigir localmente, e se você tem um Azure conta ou tem uma assinatura do Azure que você está autorizado a gerenciar.

1. Instalar o **Azure PowerShell** console. Para obter instruções, consulte [como instalar e configurar o Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Esse console personalizado está configurado para trabalhar com sua assinatura do Azure. O módulo do Azure está instalado no *arquivos de programa* diretório e é importado automaticamente em todo o uso do console do PowerShell do Azure.

    Se você preferir trabalhar em um programa de host diferente, como o Windows PowerShell ISE, certifique-se de usar o [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) cmdlet para importar o módulo do Azure ou usar um comando no módulo do Azure para disparar a importação automática do módulo.
2. Inicie o PowerShell do Azure com o **executar como administrador** opção.
3. Execute o [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) cmdlet para definir a política de execução do PowerShell do Azure para `RemoteSigned`. Digite **Y** (para Sim) para concluir a alteração da política.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Essa configuração permite que você execute scripts locais que não são assinados digitalmente. (Você também pode definir a política de execução `Unrestricted`, que seria eliminar a necessidade para a etapa de desbloqueio, mas isso não é recomendado por motivos de segurança.)
4. Execute o `Add-AzureAccount` cmdlet para configurar o PowerShell com as credenciais de sua conta.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Essas credenciais expirarem após um período de tempo e você precisa executar novamente o `Add-AzureAccount` cmdlet. Como este livro eletrônico está sendo gravado, o limite de tempo antes de expirarem credenciais é 12 horas.
5. Se você tiver várias assinaturas, use o cmdlet Select-AzureSubscription para especificar a assinatura que você deseja criar o ambiente de teste.
6. Importar um certificado de gerenciamento para a mesma assinatura do Azure usando o `Get-AzurePublishSettingsFile` e `Import-AzurePublishSettingsFile` cmdlets. O primeiro desses cmdlets baixa um arquivo de certificado e, no segundo, você especificar o local do arquivo para importá-lo. > [!IMPORTANT]
   > Manter o arquivo baixado em um local seguro ou excluí-lo quando você tiver concluído, porque ele contém um certificado que pode ser usado para gerenciar os serviços do Azure.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    O certificado é usado para uma chamada à API REST que detecta o endereço IP do computador de desenvolvimento para definir uma regra de firewall no servidor de banco de dados SQL.
7. Execute o [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) cmdlet (aliases são `cd`, `chdir`, e `sl`) para navegar até o diretório que contém os scripts. (Eles estão localizados no *automação* na pasta solução corrigir.) Coloque o caminho entre aspas se qualquer um dos nomes de diretório contiver espaços. Por exemplo, para navegar até o `c:\Sample Apps\FixIt\Automation` diretório, você pode digitar o seguinte comando:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Para permitir que o Windows PowerShell para executar esses scripts, use o [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) cmdlet. (Os scripts estão bloqueados porque eles foram baixados da Internet).

    > [!WARNING]
    > Segurança - antes de executar `Unblock-File` em qualquer script ou arquivo executável, abra o arquivo no bloco de notas, examine os comandos e verificar se eles não contêm código mal-intencionado.

    Por exemplo, o comando a seguir executa o `Unblock-File` cmdlet em todos os scripts no diretório atual.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Para criar o aplicativo web para a base (nenhuma fila de processamento) corrija-o aplicativo, execute o script de criação do ambiente.

    Obrigatório `Name` parâmetro especifica o nome do banco de dados e também é usado para a conta de armazenamento que cria o script. O nome deve ser exclusivo dentro do domínio azurewebsites.net. Se você especificar um nome que não é exclusivo, como Fixit ou teste (ou até mesmo como no exemplo, fixitdemo), o `New-AzureWebsite` cmdlet falhará com um erro interno que relata um conflito. O script converte o nome em letras minúsculas para atender aos requisitos de nome para aplicativos web, contas de armazenamento e bancos de dados.

    Obrigatório `SqlDatabasePassword` parâmetro especifica a senha para a conta de administrador que será criada para o banco de dados SQL. Não inclua caracteres XML especiais na senha (&amp; &lt; &gt; ;). Essa é uma limitação do modo como os scripts foram escritos, não uma limitação do Azure.

    Por exemplo, se você quiser criar um aplicativo web chamado "fixitdemo" e usar uma senha de administrador do SQL Server de "Passw0rd1", você poderia digite o seguinte comando:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    O nome deve ser exclusivo no domínio azurewebsites.net e a senha deve atender aos requisitos de banco de dados SQL de complexidade de senha. (O exemplo Passw0rd1 atende aos requisitos).

    Observe que o comando começa com ". \". Para ajudar a evitar a execução de scripts mal-intencionados, o Windows PowerShell requer que você forneça o caminho totalmente qualificado para o arquivo de script quando você executa um script. Você pode usar um ponto para indicar o diretório atual (".\") ou, forneça o caminho totalmente qualificado, como:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Para obter mais informações sobre o script, use o `Get-Help` cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Você pode usar o `Detailed`, `Full`, `Parameters`, e `Examples` parâmetros do cmdlet Get-Help para filtrar a Ajuda que é retornada.

    Se o script falha ou gera erros, como "New-AzureWebsite: chamar Set-AzureSubscription e Select-AzureSubscription primeiro", você pode não concluiu a configuração do Azure PowerShell.

    Depois que o script for concluído, você pode usar o Portal de gerenciamento para ver os recursos que foram criados, como mostra o [automatizar tudo](automate-everything.md) capítulo.
10. Para implantar o projeto FixIt para o novo ambiente do Azure, use o *AzureWebsite.ps1* script. Por exemplo:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Quando a implantação é feita, o navegador é aberto com corrigi-lo em execução no Azure.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Solucionando problemas de scripts do Windows PowerShell

Os erros mais comuns encontrados durante a execução desses scripts estão relacionados às permissões. Verifique se `Add-AzureAccount` e `Import-AzurePublishSettingsFile` foram bem-sucedidas e usado para a mesma assinatura do Azure. Mesmo se `Add-AzureAccount` foi bem-sucedida você precisará executá-lo novamente. As permissões adicionadas por `Add-AzureAccount` expirar em 12 horas.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Referência de objeto não definida para uma instância de um objeto.

Se o script retorna erros, como "Referência de objeto não definida para uma instância de um objeto," significando que o Windows PowerShell não é possível localizar um objeto de processo (essa é uma exceção de referência nula), execute o `Add-AzureAccount` cmdlet e repita o script.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: O servidor encontrou um erro interno.

O `New-AzureWebsite` cmdlet retorna um erro interno quando o nome não é exclusivo no domínio azurewebsites.net. Para resolver o erro, use um valor diferente para o nome, o que é o parâmetro Name do *AzureWebsiteEnv.ps1 novo*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Reiniciar o script

Se você precisa reiniciar o *AzureWebsiteEnv.ps1 novo* script porque ele falhou antes que ele impresso a mensagem "O Script for concluído", você talvez queira excluir recursos que o script criado antes que ele é interrompido. Por exemplo, se o script já criado o aplicativo web ContosoFixItDemo e executar o script novamente com o mesmo nome, o script falhará porque o nome está em uso.

Para determinar quais recursos o script criado antes de interrompido, use os seguintes cmdlets:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Para executar esse cmdlet, redirecione o nome do servidor de banco de dados para `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Para excluir esses recursos, use os comandos a seguir. Observe que se você excluir o servidor de banco de dados, você excluir automaticamente os bancos de dados associados ao servidor.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Como implantar o aplicativo com a fila de processamento para aplicativos de Web do serviço de aplicativo do Azure e um serviço de nuvem do Azure

Para habilitar as filas, faça a seguinte alteração no arquivo MyFixIt\Web.config. Em `appSettings`, altere o valor de `UseQueues` como "true": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Implantar o aplicativo MVC a um aplicativo web no serviço de aplicativo do Azure, conforme descrito [anterior](#deploybase).

Em seguida, crie um novo serviço de nuvem do Azure. Os scripts incluídos com o aplicativo corrigir não criar ou implantar o serviço de nuvem, você deve usar o portal do Azure para isso. No portal, clique em **novo** -- **de computação** – **serviço de nuvem** -- **criação rápida**e, em seguida, insira uma URL e um local de centro de dados. Use o mesmo data center onde você implantou o aplicativo web.

![](the-fix-it-sample-application/_static/image1.png)

Antes de implantar o serviço de nuvem, você precisa atualizar alguns dos arquivos de configuração.

Em MyFixIt.WorkerRoler\app.config, em `connectionStrings`, substitua o valor da `appdb` cadeia de caracteres de conexão com a cadeia de caracteres de conexão real do banco de dados SQL. Você pode obter a cadeia de caracteres de conexão do portal. No portal, clique em **bancos de dados SQL** - **appdb** - **cadeias de caracteres de conexão de banco de dados do modo de exibição SQL para ADO .net, ODBC, PHP e JDBC**. Copie a cadeia de caracteres de conexão ADO.NET e cole o valor no arquivo App. config. Substitua "{sua\_senha\_aqui}" com a senha do banco de dados. (Supondo que os scripts são usados para implantar o aplicativo MVC, você especificou a senha do banco de dados no `SqlDatabasePassword` parâmetro do script.)

O resultado deve ser semelhante ao seguinte:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

No mesmo arquivo MyFixIt.WorkerRoler\app.config, em `appSettings`, substitua os dois valores de espaço reservado para a conta de armazenamento do Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Você pode obter a chave de acesso a partir do portal. Consulte [como gerenciar contas de armazenamento](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

No MyFixItCloudService\ServiceConfiguration.Cloud.cscfg, substitua os mesmos dois valores de espaços reservados para a conta de armazenamento do Azure.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Agora você está pronto para implantar o serviço de nuvem. No Gerenciador de soluções, clique com botão direito no projeto MyFixItCloudService e selecione **publicar**. Para obter mais informações, consulte "[implantar o aplicativo no Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", que está na parte 2 de [este tutorial](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Anterior](more-patterns-and-guidance.md)
