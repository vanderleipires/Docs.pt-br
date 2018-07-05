---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: as transformações do arquivo Web. config - 3 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: a5afcf4d05d6fa51ad70f7e5bdfafd20002f188a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841752"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Implantando um aplicativo da Web ASP.NET com o SQL Server Compact usando o Visual Studio ou Visual Web Developer: as transformações do arquivo Web. config - 3 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET projeto de aplicativo web que inclui um banco de dados do SQL Server Compact usando o Visual Studio 2012 RC ou Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se você instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial na série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para obter um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar as edições do SQL Server que não seja o SQL Server Compact e mostra como implantar aplicativos de Web do serviço de aplicativo do Azure, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como automatizar o processo de alteração de *Web. config* arquivo quando você implantá-lo em ambientes de destino diferente. A maioria dos aplicativos têm configurações na *Web. config* arquivos que devem ser diferentes quando o aplicativo é implantado. Automatizando o processo de fazer essas alterações evita a necessidade para fazê-las manualmente sempre que você implanta, qual seria entediante e sujeito a erros.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformações de Web. config versus Web implantar parâmetros

Há duas maneiras para automatizar o processo de alteração *Web. config* configurações do arquivo: [transformações de Web. config](https://msdn.microsoft.com/library/dd465326.aspx) e [parâmetros de implantação da Web](https://msdn.microsoft.com/library/ff398068.aspx). Um *Web. config* arquivo de transformação contém marcação XML que especifica como alterar a *Web. config* arquivo quando ele é implantado. Você pode especificar alterações diferentes para determinado compila as configurações e perfis de publicação para específico. As configurações de compilação padrão são Debug e Release e você pode criar configurações de compilação personalizada. Um perfil de publicação geralmente corresponde a um ambiente de destino. (Você aprenderá mais sobre como publicar perfis na [implantando no IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.)

Parâmetros de implantação da Web podem ser usados para especificar vários tipos diferentes de configurações que devem ser configurados durante a implantação, incluindo as configurações que são encontradas em *Web. config* arquivos. Quando usado para especificar *Web. config* alterações de arquivo, os parâmetros de implantação da Web são mais complexos para configurar, mas eles são úteis quando você não souber o valor a ser definido até que você implante. Por exemplo, em um ambiente corporativo, você pode criar uma *pacote de implantação* e dê a ele a uma pessoa no departamento de TI para instalar em produção e que a pessoa deve ser capaz de inserir cadeias de caracteres de conexão ou senhas que você não fizer isso sabe.

Para o cenário que abrange este tutorial, você sabe tudo o que precisa ser feito para o *Web. config* de arquivos, portanto você não precisa usar parâmetros de implantação da Web. Você vai configurar algumas transformações que são diferentes dependendo da configuração de compilação usada, e algumas que são diferentes dependendo do perfil de publicação usado.

## <a name="creating-transformation-files-for-publish-profiles"></a>Criando arquivos de transformação para perfis de publicação

Na **Gerenciador de soluções**, expanda *Web. config* para ver os *Debug* e *Release* arquivos de transformação são criados por padrão para as configurações de compilação padrão de dois.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Você pode criar arquivos de transformação para configurações de compilação personalizada clicando duas vezes o arquivo Web. config e escolhendo **adicionar transformações de Config** no menu de contexto, mas para este tutorial não precisa fazer isso.

Mais dois arquivos transformação, é necessário para configurar as alterações que estão relacionadas para o destino de implantação em vez da configuração de compilação. Um exemplo típico desse tipo de configuração é um ponto de extremidade do WCF que é diferente para teste versus produção. Em tutoriais posteriores, você criará o chamado teste e produção, portanto, você precisa de perfis de publicação um *Web.Test.config* arquivo e um *Web.Production.config* arquivo.

Arquivos de transformação que estão vinculados para perfis de publicação devem ser criados manualmente. Na **Gerenciador de soluções**, clique com botão direito no projeto ContosoUniversity e selecione **Abrir pasta no Windows Explorer**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

Na **Windows Explorer**, selecione o *Release* , copie o arquivo e, em seguida, cole as duas cópias dele. Renomear essas cópias *Web.Production.config* e *Web.Test.config*, em seguida, feche **Windows Explorer**.

Na **Gerenciador de soluções**, clique em **atualizar** para ver os novos arquivos.

Selecione os novos arquivos, clique com botão direito e, em seguida, clique **Include in Project** no menu de contexto.

![Incluindo arquivos de configuração de teste e produção no projeto](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Para impedir que esses arquivos que está sendo implantado, selecione-os na **Gerenciador de soluções**e, em seguida, no **propriedades** alteração de janela a **Build Action** propriedade **Conteúdo** para **None**. (Os arquivos de transformação se baseiam em configurações de build são impedidos automaticamente da implantação).

Agora você está pronto para inserir *Web. config* transformações a *Web. config* arquivos de transformação.

## <a name="limiting-error-log-access-to-administrators"></a>Limitando o acesso de Log de erros para administradores

Se houver um erro enquanto o aplicativo é executado, o aplicativo exibe uma página de erro genérico no lugar da página de erro gerada pelo sistema e ele usa [pacote do NuGet do Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) para emissão de relatórios e log de erros. O `customErrors` elemento na *Web. config* arquivo Especifica a página de erro:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Para ver a página de erro, altere temporariamente o `mode` atributo do `customErrors` elemento do "RemoteOnly" para "Ativado" e execute o aplicativo do Visual Studio. Causar um erro ao solicitar uma URL inválida, como *Studentsxxx.aspx*. Em vez de uma página de erro gerado pelo IIS "página não encontrada", você verá a *GenericErrorPage* página.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Para ver o log de erros, substitua tudo da URL após o número da porta com *elmah.axd* (por exemplo, a captura de tela, `http://localhost:51130/elmah.axd`) e pressione Enter:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Não se esqueça de definir o `customErrors` elemento para o modo de "RemoteOnly" quando terminar.

No computador de desenvolvimento é conveniente permitir o acesso gratuito à página de log de erro, mas em produção que seria um risco à segurança. Para o site de produção, você pode adicionar uma regra de autorização que restringe o acesso ao log de erros apenas para administradores, configurando uma transformação na *Web.Production.config* arquivo.

Abra *Web.Production.config* e adicione um novo `location` elemento imediatamente após a abertura `configuration` de marca, conforme mostrado aqui. (Certifique-se de que você adicione apenas o `location` elemento e não a marcação próxima que é mostrada aqui somente para fornecer algum contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

O `Transform` valor de atributo de "Insert" faz com que essa `location` elemento a ser adicionado como um irmão qualquer existente `location` elementos no *Web. config* arquivo. (Já houver um `location` regras de elemento que especifica a autorização para o **atualização créditos** página.) Quando você testar o site de produção após a implantação, você vai testar para verificar se essa regra de autorização é eficiente.

Não é necessário restringir o acesso ao log de erros no ambiente de teste, portanto, você não precisa adicionar esse código para o *Web.Test.config* arquivo.

> [!NOTE] 
> 
> **Observação de segurança** nunca exibir detalhes do erro para o público em um aplicativo de produção, ou armazenar essas informações em um local público. Os invasores podem usar informações de erro para descobrir vulnerabilidades em um site. Se você usar o ELMAH no seu próprio aplicativo, certifique-se de investigar maneiras em que o ELMAH pode ser configurado para minimizar os riscos de segurança. O exemplo do ELMAH neste tutorial não deve ser considerado uma configuração recomendada. É um exemplo que foi escolhido para ilustrar como lidar com uma pasta que o aplicativo deve ser capaz de criar arquivos.


## <a name="setting-an-environment-indicator"></a>Definir um indicador de ambiente

Um cenário comum é ter *Web. config* as configurações que devem ser diferentes em cada ambiente que você implanta em arquivos. Por exemplo, um aplicativo que chama um serviço WCF talvez seja necessário um ponto de extremidade diferente em ambientes de teste e produção. O aplicativo Contoso University inclui uma configuração desse tipo também. Essa configuração controla um indicador visível nas páginas de um site que informa qual ambiente você está no, como desenvolvimento, teste ou produção. O valor da configuração determina se o aplicativo acrescentará "(desenvolvimento)" ou "(teste)" no cabeçalho principal do *Master* página mestra:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

O indicador de ambiente é omitido quando o aplicativo está em execução na produção.

Páginas da web Contoso University ler um valor que é definido em `appSettings` no *Web. config* arquivo a fim de determinar qual o ambiente em que o aplicativo é executado no:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

O valor deve ser "Teste" no ambiente de teste e "Prod" no ambiente de produção.

Abra *Web.Production.config* e adicione um `appSettings` elemento imediatamente antes da marca de abertura do `location` elemento que você adicionou anteriormente:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

O `xdt:Transform` valor "SetAttributes" indica que a finalidade dessa transformação é alterar os valores de atributo de um elemento existente no atributo o *Web. config* arquivo. O `xdt:Locator` "Match(key)" indica que o elemento a ser modificado é aquele do valor do atributo cujo `key` atributo corresponde a `key` atributo especificado aqui. O apenas outro atributo do `add` elemento é `value`, e isso é o que será alterado em implantado *Web. config* arquivo. Esse código faz com que o `value` atributo do `Environment` `appSettings` elemento a ser definido como "Prod" no *Web. config* arquivo que é implantado para produção.

Em seguida, aplique a mesma alteração *Web.Test.config* arquivo, exceto o conjunto de `value` para "Test" em vez de "Prod". Quando você terminar, o `appSettings` seção *Web.Test.config* se parecerá com o exemplo a seguir:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Desabilitar o modo de depuração

Para um build de versão, você não deseja depuração habilitada, independentemente de qual ambiente você está implantando. Por padrão o *Release* arquivo de transformação é criado automaticamente com o código que remove as `debug` de atributos do `compilation` elemento:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

O `Transform` atributo faz com que o `debug` atributo seja omitido da implantado *Web. config* sempre que você implantar uma compilação de versão do arquivo.

Essa mesma transformação está em teste e produção arquivos de transformação porque você criou, copiando o arquivo de transformação de versão. Você não precisa dela duplicado lá, então, abrir cada um desses arquivos, remova os **compilação** elemento e salve e feche cada arquivo.

## <a name="setting-connection-strings"></a>Configurar cadeias de Conexão

Na maioria dos casos você não precisa configurar transformações de cadeia de caracteres de conexão, porque você pode especificar cadeias de caracteres de conexão no perfil de publicação. Mas há uma exceção quando você estiver implantando um banco de dados do SQL Server Compact e você estiver usando o Entity Framework Code First Migrations para atualizar o banco de dados no servidor de destino. Para este cenário, você precisa especificar uma cadeia de caracteres de conexão adicionais que será usada no servidor para atualizar o esquema de banco de dados. Para configurar essa transformação, adicione uma **&lt;connectionStrings&gt;** elemento imediatamente após a abertura **&lt;configuração&gt;** marca em ambos os o *Web.Test.config* e o *Web.Production.config* transformar arquivos:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

O `Transform` atributo especifica que essa cadeia de caracteres de conexão será adicionada para o *connectionStrings* elemento no implantado *Web. config* arquivo. (O processo de publicação cria essa cadeia de caracteres de conexão adicionais automaticamente para você se ele não existir, mas por padrão o **providerName** atributo é definido como `System.Data.SqlClient`, que não funciona não para o SQL Server Compact. Ao adicionar manualmente a cadeia de caracteres de conexão, você mantenha o processo de implantação desde a criação de um elemento de cadeia de caracteres de conexão com o nome do provedor errado.)

Agora você tiver especificado todas as *Web. config* transformações que você precisa para implantar o aplicativo Contoso University para teste e produção. No tutorial a seguir, você vai cuidar das tarefas de configuração de implantação que requerem a definição de propriedades do projeto.

## <a name="more-information"></a>Mais informações

Para obter mais informações sobre os tópicos abordados por este tutorial, consulte o cenário de transformação Web. config no [mapa de conteúdo de implantação do ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
