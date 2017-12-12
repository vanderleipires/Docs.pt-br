---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: "Implantação de Web do ASP.NET usando o Visual Studio: transformações do arquivo Web. config | Microsoft Docs"
author: tdykstra
description: "Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, por usin..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a88d8f35c770b362b74f787fee2c60a7577bccb2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Implantação de Web do ASP.NET usando o Visual Studio: transformações do arquivo Web. config
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto Starter](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutorial mostra como implantar (publicação) de uma ASP.NET web do aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão Geral

Este tutorial mostra como automatizar o processo de alteração de *Web. config* arquivo quando você o implantar em ambientes de destino diferente. A maioria dos aplicativos têm configurações de *Web. config* arquivo deve ser diferente quando o aplicativo é implantado. Automatizando o processo de fazer essas alterações mantém você precise fazê-las manualmente sempre que você implanta, qual seria tedioso e propenso a erros.

Lembrete: Se você receber uma mensagem de erro ou algo não funciona ao percorrer o tutorial, certifique-se verificar a [página de solução de problemas](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformações de Web. config em vez de parâmetros de implantação da Web

Há duas maneiras de automatizar o processo de alteração *Web. config* configurações de arquivo: [transformações de Web. config](https://msdn.microsoft.com/en-us/library/dd465326.aspx) e [parâmetros de implantação da Web](https://msdn.microsoft.com/en-us/library/ff398068.aspx). Um *Web. config* arquivo de transformação contém marcação XML que especifica como alterar o *Web. config* arquivo quando ele é implantado. Você pode especificar diferentes alterações para determinado configurações de compilação e perfis de publicação para determinado. As configurações de compilação padrão são Debug e Release e você pode criar configurações de compilação personalizada. Um perfil de publicação geralmente corresponde a um ambiente de destino. (Você aprenderá mais sobre como publicar perfis no [implantando para o IIS como um ambiente de teste](deploying-to-iis.md) tutorial.)

Parâmetros de implantação da Web podem ser usados para especificar vários tipos diferentes de configurações que devem ser configurados durante a implantação, incluindo as configurações que se encontram em *Web. config* arquivos. Quando usado para especificar *Web. config* alterações no arquivo, parâmetros de implantação da Web são mais complexos para configurar, mas eles são úteis quando você não souber o valor a ser definido até que você implante. Por exemplo, em um ambiente corporativo, você pode criar um *pacote de implantação* e dê a ele uma pessoa no departamento de TI para instalar o em produção, e essa pessoa tem que inserir cadeias de caracteres de conexão ou senhas que você não sabe.

Para o cenário que abrange essa série de tutoriais, já sabe tudo o que deve ser feita para o *Web. config* de arquivos, portanto você não precisa usar parâmetros de implantação da Web. Você vai configurar algumas transformações que são diferentes dependendo da configuração de compilação usada, e alguns que são diferentes dependendo do perfil de publicação usado.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Especificando as configurações de Web. config no Azure

Se o *Web. config* são configurações do arquivo que você deseja alterar no `<connectionStrings>` ou `<appSettings>` elemento, e se você estiver implantando em aplicativos Web no serviço de aplicativo do Azure, você terá outra opção para automatizar as alterações durante implantação. Você pode inserir as configurações que você deseja em vigor no Azure no **configurar** guia da página do portal de gerenciamento para seu aplicativo web (Role para baixo até o **configurações do aplicativo** e **cadeias de caracteres de conexão**  seções). Quando você implanta o projeto, o Azure aplica automaticamente as alterações. Para obter mais informações, consulte [Sites do Windows Azure: como cadeias de caracteres de aplicativo e o trabalho de cadeias de caracteres de Conexão](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Arquivos de transformação padrão

Em **Solution Explorer**, expanda *Web. config* para ver o *Web.Debug.config* e *Web.Release.config* arquivos de transformação são criados por padrão para as configurações de compilação padrão de dois.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Você pode criar arquivos de transformação para configurações de compilação personalizada clicando duas vezes o arquivo Web. config e escolhendo **adicionar transformações Config** no menu de contexto. Para este tutorial, você não precisa fazer isso, e a opção de menu é desabilitada, porque você não tiver criado as configurações de compilação personalizada.

Mais tarde você criará três arquivos mais de transformação, um para o teste, preparo e produção de perfis de publicação. Um exemplo típico de uma configuração que você usaria em um arquivo de transformação do perfil de publicação porque ela depende do ambiente de destino é um ponto de extremidade do WCF que é diferente para teste e produção. Você criará publicar arquivos de perfil de transformação em tutoriais subsequentes depois de criar os perfis de publicação que entram com.

## <a name="disable-debug-mode"></a>Desabilitar modo de depuração

Um exemplo de uma configuração que depende da configuração de compilação em vez de ambiente de destino é o `debug` atributo. Para uma versão de compilação, normalmente deseja depuração desabilitada, independentemente de qual ambiente você está implantando. Portanto, por padrão, o Visual Studio modelos de projeto criam *Web.Release.config* transformar arquivos com o código que remove o `debug` de atributo do `compilation` elemento. Aqui é o padrão *Web.Release.config*: além de algum código de transformação de exemplo que é comentado, ele inclui código de `compilation` elemento remove o `debug` atributo:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

O `xdt:Transform="RemoveAttributes(debug)"` atributo especifica que você deseja que o `debug` atributo a ser removido do `system.web/compilation` elemento implantado *Web. config* arquivo. Isso será feito sempre que você implantar uma compilação de versão.

## <a name="limit-error-log-access-to-administrators"></a>Limitar o acesso ao log de erros para administradores

Se houver um erro enquanto o aplicativo é executado, o aplicativo exibe uma página de erro genérico no lugar da página de erro gerado pelo sistema e ele usa o [pacote Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) para emissão de relatórios e o log de erros. O `customErrors` elemento no aplicativo *Web. config* arquivo Especifica a página de erro:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Para ver a página de erro, altere temporariamente o `mode` atributo o `customErrors` elemento de "RemoteOnly" como "On" e execute o aplicativo do Visual Studio. Causar um erro ao solicitar uma URL inválida, como *Studentsxxx.aspx*. Em vez de um erro gerado pelo IIS "não é possível encontrar o recurso" página, você verá o *GenericErrorPage* página.

![Página de erro](web-config-transformations/_static/image2.png)

Para ver o log de erros, substituir tudo na URL após o número da porta com *elmah.axd* (por exemplo, `http://localhost:51130/elmah.axd`) e pressione Enter:

![Página ELMAH](web-config-transformations/_static/image3.png)

Não se esqueça de definir o `customErrors` elemento para o modo de "RemoteOnly" quando estiver pronto.

No computador de desenvolvimento é conveniente permitir o acesso livre para a página de erro do log, mas em produção que poderia ser um risco à segurança. Para o site de produção, você deseja adicionar uma regra de autorização que restringe o acesso ao log de erros para administradores e para certificar-se de que a restrição funciona você desejar no teste e de preparo também. Portanto, isso é outra alteração que você deseja implementar toda vez que você implantar uma compilação de versão e, portanto, ele pertence a *Web.Release.config* arquivo.

Abra *Web.Release.config* e adicione um novo `location` elemento imediatamente antes do fechamento `configuration` marca, conforme mostrado aqui.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

O `Transform` valor do atributo de "Inserção" faz com que essa `location` elemento a ser adicionado como um irmão qualquer existente `location` elementos o *Web. config* arquivo. (Já existe um `location` regras de elemento que especifica a autorização para o **atualização créditos** página.)

Agora você pode visualizar a transformação para certificar-se de que o codificados corretamente.

Em **Solution Explorer**, clique com botão direito *Web.Release.config* e clique em **visualização transformar**.

![Menu de transformação de visualização](web-config-transformations/_static/image4.png)

Abre uma página que mostra o desenvolvimento *Web. config* arquivo à esquerda e o que implantado *Web. config* arquivo será semelhante à direita, com as alterações destacadas.

![Visualização de transformação de depuração](web-config-transformations/_static/image5.png)

![Visualização de transformação de local](web-config-transformations/_static/image6.png)

(Na visualização, você poderá notar que algumas alterações adicionais que você não grava transformações para: eles normalmente envolvem a remoção de espaço em branco que não afetam a funcionalidade.)

Quando você testar o site após a implantação, você também vai testar para verificar se a regra de autorização é eficiente.

> [!NOTE] 
> 
> **Observação de segurança** nunca exibir detalhes do erro para o público em um aplicativo de produção ou armazenar essas informações em um local público. Os invasores poderão usar as informações de erro para detectar vulnerabilidades em um site. Se você usar ELMAH em seu próprio aplicativo, configure ELMAH para minimizar os riscos de segurança. O exemplo ELMAH neste tutorial não deve ser considerado uma configuração recomendada. É um exemplo que foi escolhido para ilustrar como lidar com uma pasta que o aplicativo deve ser capaz de criar arquivos. Para obter mais informações, consulte [proteger o ponto de extremidade do ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Uma configuração que você vai manipular em Publicar arquivos de perfil de transformação

Um cenário comum é ter *Web. config* configurações que devem ser diferentes em cada ambiente que você implantar arquivos. Por exemplo, um aplicativo que chama um serviço WCF talvez seja necessário um ponto de extremidade diferente em ambientes de teste e produção. O aplicativo Contoso University inclui uma configuração desse tipo também. Essa configuração controla um indicador visível nas páginas de um site que informa qual ambiente são in, como desenvolvimento, teste ou produção. O valor da configuração determina se o aplicativo acrescentará "(desenvolvimento)" ou "(teste)" para o título principal a *Site.Master* página mestre:

![Indicador de ambiente](web-config-transformations/_static/image7.png)

O indicador de ambiente é omitido quando o aplicativo é executado na preparação ou produção.

Páginas da web Contoso University ler um valor que é definido em `appSettings` no *Web. config* arquivo a fim de determinar qual o ambiente em que o aplicativo é executado no:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

O valor deve ser "Teste" no ambiente de teste e "Produção" para preparação e produção.

O código a seguir em um arquivo de transformação implementar essa transformação:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

O `xdt:Transform` valor "SetAttributes" indica que a finalidade dessa transformação é alterar os valores de atributo de um elemento existente no atributo de *Web. config* arquivo. O `xdt:Locator` "Match(key)" indica que o elemento a ser modificado é o de valor de atributo cujo `key` atributo corresponde a `key` atributo especificado aqui. O somente outro atributo do `add` elemento `value`, e que é o que será alterado em implantado *Web. config* arquivo. O código mostrado aqui faz com que o `value` atributo do `Environment` `appSettings` elemento a ser definido como "Test" *Web. config* arquivo implantado.

Esta transformação pertence nos arquivos de transformação do perfil de publicação, que você ainda não tenha criado. Você criará e atualizar os arquivos de transformação que implementam essa alteração, quando você criar os perfis de publicação para os ambientes de teste, preparação e produção. Você vai fazer que a [implantar em IIS](deploying-to-iis.md) e [implantar na produção](deploying-to-production.md) tutoriais.

> [!NOTE]
> Porque esta configuração está a `<appSettings>` elemento, você tem outra alternativa para especificar a transformação quando você estiver implantando em aplicativos Web no serviço de aplicativo do Azure consulte [Web. config especificando configurações no Azure](#watransforms) anteriormente neste tópico.


## <a name="setting-connection-strings"></a>Cadeias de caracteres de conexão de configuração

Embora o arquivo de transformação padrão contém um exemplo que mostra como atualizar uma cadeia de caracteres de conexão, na maioria dos casos você não precisa configurar transformações de cadeia de caracteres de conexão, porque você pode especificar cadeias de caracteres de conexão no perfil de publicação. Você vai fazer que a [implantar em IIS](deploying-to-iis.md) e [implantar na produção](deploying-to-production.md) tutoriais.

## <a name="summary"></a>Resumo

Agora você ter feito tanto quanto possível com *Web. config* transformações antes de criar os perfis de publicação, e você viu uma visualização da qual será o arquivo Web. config implantado.

![Visualização de transformação de local](web-config-transformations/_static/image8.png)

O tutorial a seguir, você vai ter cuidado das tarefas de configuração de implantação que requerem a definição de propriedades do projeto.

## <a name="more-information"></a>Mais informações

Para obter mais informações sobre os tópicos abordados por este tutorial, consulte [Web. config usando transformações para alterar as configurações no arquivo App. config ou o arquivo Web. config de destino durante a implantação](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) do mapa de conteúdo de implantação da Web para O Visual Studio e ASP.NET.

>[!div class="step-by-step"]
[Anterior](preparing-databases.md)
[Próximo](project-properties.md)
