---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: transformações do arquivo Web. config - 3 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 86eb74ca35e8804978127412e2276eeee9d615dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: transformações do arquivo Web. config - 3 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar um ASP.NET (publicar) projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Se você instalar a atualização de publicação na Web, você também pode usar o Visual Studio 2010. Para obter uma introdução à série, consulte [primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server diferente do SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão Geral

Este tutorial mostra como automatizar o processo de alteração de *Web. config* arquivo quando você o implantar em ambientes de destino diferente. A maioria dos aplicativos têm configurações de *Web. config* arquivo deve ser diferente quando o aplicativo é implantado. Automatizando o processo de fazer essas alterações mantém você precise fazê-las manualmente sempre que você implanta, qual seria tedioso e propenso a erros.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformações de Web. config e Web implantar parâmetros

Há duas maneiras de automatizar o processo de alteração *Web. config* configurações de arquivo: [transformações de Web. config](https://msdn.microsoft.com/library/dd465326.aspx) e [parâmetros de implantação da Web](https://msdn.microsoft.com/library/ff398068.aspx). Um *Web. config* arquivo de transformação contém marcação XML que especifica como alterar o *Web. config* arquivo quando ele é implantado. Você pode especificar diferentes alterações para determinado configurações de compilação e perfis de publicação para determinado. As configurações de compilação padrão são Debug e Release e você pode criar configurações de compilação personalizada. Um perfil de publicação geralmente corresponde a um ambiente de destino. (Você aprenderá mais sobre como publicar perfis no [implantando para o IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.)

Parâmetros de implantação da Web podem ser usados para especificar vários tipos diferentes de configurações que devem ser configurados durante a implantação, incluindo as configurações que se encontram em *Web. config* arquivos. Quando usado para especificar *Web. config* alterações no arquivo, parâmetros de implantação da Web são mais complexos para configurar, mas eles são úteis quando você não souber o valor a ser definido até que você implante. Por exemplo, em um ambiente corporativo, você pode criar um *pacote de implantação* e dê a ele uma pessoa no departamento de TI para instalar o em produção, e essa pessoa tem que inserir cadeias de caracteres de conexão ou senhas que você não sabe.

Para o cenário que abrange neste tutorial, você sabe que tudo o que deve ser feita para o *Web. config* de arquivos, portanto você não precisa usar parâmetros de implantação da Web. Você vai configurar algumas transformações que são diferentes dependendo da configuração de compilação usada, e alguns que são diferentes dependendo do perfil de publicação usado.

## <a name="creating-transformation-files-for-publish-profiles"></a>Criando arquivos de transformação para perfis de publicação

Em **Solution Explorer**, expanda *Web. config* para ver o *Web.Debug.config* e *Web.Release.config* arquivos de transformação são criados por padrão para as configurações de compilação padrão de dois.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Você pode criar arquivos de transformação para configurações de compilação personalizada clicando duas vezes o arquivo Web. config e escolhendo **adicionar transformações Config** no menu de contexto, mas para este tutorial não é necessário fazer isso.

São necessários dois arquivos de transformação mais, para configurar as alterações que são relacionadas para o destino de implantação em vez da configuração de compilação. Um exemplo típico desse tipo de configuração é um ponto de extremidade do WCF que é diferente para teste e produção. Em tutoriais subsequentes, você criará chamados teste e produção, portanto, você precisa de perfis de publicação um *Web.Test.config* arquivo e um *Web.Production.config* arquivo.

Arquivos de transformação que estão vinculados para perfis de publicação devem ser criados manualmente. Em **Solution Explorer**, com o botão direito no projeto ContosoUniversity e selecione **Abrir pasta no Windows Explorer**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

Em **Windows Explorer**, selecione o *Web.Release.config* arquivo, copie o arquivo e, em seguida, cole duas cópias do mesmo. Renomear essas cópias *Web.Production.config* e *Web.Test.config*, em seguida, feche **Windows Explorer**.

Em **Solution Explorer**, clique em **atualizar** para ver os novos arquivos.

Selecione os novos arquivos, com o botão direito e clique **incluir no projeto** no menu de contexto.

![Incluindo arquivos de configuração de teste e de produção no projeto](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Para impedir que esses arquivos que está sendo implantado, selecione-os na **Solution Explorer**e, em seguida, no **propriedades** alteração de janela a **ação de compilação** propriedade **Conteúdo** para **nenhum**. (Os arquivos de transformação com base em configurações de compilação são impedidos automaticamente da implantação).

Agora você está pronto para inserir *Web. config* transformações no *Web. config* arquivos de transformação.

## <a name="limiting-error-log-access-to-administrators"></a>Limitar o acesso ao Log de erros para administradores

Se houver um erro enquanto o aplicativo é executado, o aplicativo exibe uma página de erro genérico no lugar da página de erro gerado pelo sistema e ele usa [pacote Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) para emissão de relatórios e o log de erros. O `customErrors` elemento o *Web. config* arquivo Especifica a página de erro:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Para ver a página de erro, altere temporariamente o `mode` atributo o `customErrors` elemento de "RemoteOnly" como "On" e execute o aplicativo do Visual Studio. Causar um erro ao solicitar uma URL inválida, como *Studentsxxx.aspx*. Em vez de uma página de erro gerado pelo IIS "página não encontrada", você verá o *GenericErrorPage* página.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Para ver o log de erros, substituir tudo na URL após o número da porta com *elmah.axd* (por exemplo, na captura de tela, `http://localhost:51130/elmah.axd`) e pressione Enter:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Não se esqueça de definir o `customErrors` elemento para o modo de "RemoteOnly" quando estiver pronto.

No computador de desenvolvimento é conveniente permitir o acesso livre para a página de erro do log, mas em produção que poderia ser um risco à segurança. Para o site de produção, você pode adicionar uma regra de autorização que restringe o acesso ao log de erros apenas para os administradores Configurando uma transformação no *Web.Production.config* arquivo.

Abra *Web.Production.config* e adicione um novo `location` elemento imediatamente após a abertura `configuration` marca, conforme mostrado aqui. (Certifique-se de que você adicione somente o `location` elemento e não ao redor marcação que é mostrada aqui apenas para fornecer algum contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

O `Transform` valor do atributo de "Inserção" faz com que essa `location` elemento a ser adicionado como um irmão qualquer existente `location` elementos o *Web. config* arquivo. (Já existe um `location` regras de elemento que especifica a autorização para o **atualização créditos** página.) Quando você testar o site de produção após a implantação, você vai testar para verificar se essa regra de autorização é eficiente.

Não é necessário restringir o acesso ao log de erros no ambiente de teste, você não precisará adicionar este código para o *Web.Test.config* arquivo.

> [!NOTE] 
> 
> **Observação de segurança** nunca exibir detalhes do erro para o público em um aplicativo de produção ou armazenar essas informações em um local público. Os invasores poderão usar as informações de erro para detectar vulnerabilidades em um site. Se você usar ELMAH em seu próprio aplicativo, certifique-se de investigar maneiras em que o ELMAH pode ser configurado para minimizar riscos de segurança. O exemplo ELMAH neste tutorial não deve ser considerado uma configuração recomendada. É um exemplo que foi escolhido para ilustrar como lidar com uma pasta que o aplicativo deve ser capaz de criar arquivos.


## <a name="setting-an-environment-indicator"></a>Definir um indicador de ambiente

Um cenário comum é ter *Web. config* configurações que devem ser diferentes em cada ambiente que você implantar arquivos. Por exemplo, um aplicativo que chama um serviço WCF talvez seja necessário um ponto de extremidade diferente em ambientes de teste e produção. O aplicativo Contoso University inclui uma configuração desse tipo também. Essa configuração controla um indicador visível nas páginas de um site que informa qual ambiente são in, como desenvolvimento, teste ou produção. O valor da configuração determina se o aplicativo acrescentará "(desenvolvimento)" ou "(teste)" para o título principal a *Site.Master* página mestre:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

O indicador de ambiente é omitido quando o aplicativo é executado em produção.

Páginas da web Contoso University ler um valor que é definido em `appSettings` no *Web. config* arquivo a fim de determinar qual o ambiente em que o aplicativo é executado no:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

O valor deve ser "Teste" no ambiente de teste e "Produção" no ambiente de produção.

Abra *Web.Production.config* e adicione um `appSettings` elemento imediatamente antes da marca de abertura do `location` elemento que você adicionou anteriormente:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

O `xdt:Transform` valor "SetAttributes" indica que a finalidade dessa transformação é alterar os valores de atributo de um elemento existente no atributo de *Web. config* arquivo. O `xdt:Locator` "Match(key)" indica que o elemento a ser modificado é o de valor de atributo cujo `key` atributo corresponde a `key` atributo especificado aqui. O somente outro atributo do `add` elemento `value`, e que é o que será alterado em implantado *Web. config* arquivo. Este código faz com que o `value` atributo do `Environment` `appSettings` elemento a ser definido como "Produção" *Web. config* arquivo implantado para produção.

Em seguida, aplicar a mesma alteração em *Web.Test.config* arquivo, exceto o `value` para "Test" em vez de "Produção". Quando terminar, o `appSettings` seção *Web.Test.config* se parecerá com o exemplo a seguir:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Desabilitar o modo de depuração

Para uma versão de compilação, você não deseja depuração habilitada, independentemente de qual ambiente você está implantando. Por padrão o *Web.Release.config* arquivo de transformação é criado automaticamente com o código que remove o `debug` de atributo do `compilation` elemento:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

O `Transform` atributo faz com que o `debug` atributos a serem omitidos do implantado *Web. config* sempre que você implantar uma compilação de versão de arquivo.

Essa transformação mesmo é no teste e produção transformar arquivos porque você os criou copiando o arquivo de transformação de versão. Você não precisa duplicado existe, isso, abra cada um dos arquivos, remova o **compilação** elemento, salvar e fechar cada arquivo.

## <a name="setting-connection-strings"></a>Cadeias de caracteres de Conexão de configuração

Na maioria dos casos você não precisa configurar transformações de cadeia de caracteres de conexão, porque você pode especificar cadeias de caracteres de conexão no perfil de publicação. Mas há uma exceção quando você estiver implantando um banco de dados do SQL Server Compact e você estiver usando migrações do Entity Framework Code First para atualizar o banco de dados no servidor de destino. Para este cenário, você precisa especificar uma cadeia de caracteres de conexão adicionais que será usada no servidor para atualizar o esquema de banco de dados. Para configurar essa transformação, adicione um **&lt;connectionStrings&gt;** elemento imediatamente após a abertura **&lt;configuração&gt;** marca no o *Web.Test.config* e *Web.Production.config* arquivos de transformação:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

O `Transform` atributo especifica que essa cadeia de caracteres de conexão será adicionada para o *connectionStrings* elemento implantado *Web. config* arquivo. (O processo de publicação cria essa cadeia de caracteres de conexão adicionais automaticamente para você se ele não existir, mas, por padrão o **providerName** atributo é definido como `System.Data.SqlClient`, que não funciona não para o SQL Server Compact. Adicionando a cadeia de caracteres de conexão manualmente, você mantém o processo de implantação da criação de um elemento de cadeia de caracteres de conexão com o nome de provedor incorreto.)

Você especificou agora todos os *Web. config* transformações que você precisa para implantar o aplicativo Contoso University para teste e produção. O tutorial a seguir, você vai ter cuidado das tarefas de configuração de implantação que requerem a definição de propriedades do projeto.

## <a name="more-information"></a>Mais Informações

Para obter mais informações sobre os tópicos abordados por este tutorial, consulte o cenário de transformação do Web. config em [mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
