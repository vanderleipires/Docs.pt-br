---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Implantação de Web do ASP.NET usando o Visual Studio: as transformações do arquivo Web. config | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web de aplicativo para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usin...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 0a52530396efa3612d19817eeaa0498cffdac38f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842433"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Implantação de Web do ASP.NET usando o Visual Studio: as transformações do arquivo Web. config
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Baixe o projeto inicial](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um ASP.NET web application para aplicativos de Web do serviço de aplicativo do Azure ou para um provedor de hospedagem de terceiros, usando o Visual Studio 2012 ou Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial na série](introduction.md).


## <a name="overview"></a>Visão geral

Este tutorial mostra como automatizar o processo de alteração de *Web. config* arquivo quando você implantá-lo em ambientes de destino diferente. A maioria dos aplicativos têm configurações na *Web. config* arquivos que devem ser diferentes quando o aplicativo é implantado. Automatizando o processo de fazer essas alterações evita a necessidade para fazê-las manualmente sempre que você implanta, qual seria entediante e sujeito a erros.

Lembrete: Se você receber uma mensagem de erro ou se algo não funciona ao percorrer o tutorial, não se esqueça de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformações de Web. config em comparação com os parâmetros de implantação da Web

Há duas maneiras para automatizar o processo de alteração *Web. config* configurações do arquivo: [transformações de Web. config](https://msdn.microsoft.com/library/dd465326.aspx) e [parâmetros de implantação da Web](https://msdn.microsoft.com/library/ff398068.aspx). Um *Web. config* arquivo de transformação contém marcação XML que especifica como alterar a *Web. config* arquivo quando ele é implantado. Você pode especificar alterações diferentes para determinado compila as configurações e perfis de publicação para específico. As configurações de compilação padrão são Debug e Release e você pode criar configurações de compilação personalizada. Um perfil de publicação geralmente corresponde a um ambiente de destino. (Você aprenderá mais sobre como publicar perfis na [implantando no IIS como um ambiente de teste](deploying-to-iis.md) tutorial.)

Parâmetros de implantação da Web podem ser usados para especificar vários tipos diferentes de configurações que devem ser configurados durante a implantação, incluindo as configurações que são encontradas em *Web. config* arquivos. Quando usado para especificar *Web. config* alterações de arquivo, os parâmetros de implantação da Web são mais complexos para configurar, mas eles são úteis quando você não souber o valor a ser definido até que você implante. Por exemplo, em um ambiente corporativo, você pode criar uma *pacote de implantação* e dê a ele a uma pessoa no departamento de TI para instalar em produção e que a pessoa deve ser capaz de inserir cadeias de caracteres de conexão ou senhas que você não fizer isso sabe.

Para o cenário que abrange esta série de tutoriais, você sabe de antemão tudo o que precisa ser feito para o *Web. config* de arquivos, portanto você não precisa usar parâmetros de implantação da Web. Você vai configurar algumas transformações que são diferentes dependendo da configuração de compilação usada, e algumas que são diferentes dependendo do perfil de publicação usado.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Especificando as configurações de Web. config no Azure

Se o *Web. config* configurações do arquivo que você deseja alterar estão na `<connectionStrings>` ou o `<appSettings>` elemento, e se você estiver implantando em aplicativos Web no serviço de aplicativo do Azure, você tem outra opção para automatizar alterações durante implantação. Você pode inserir as configurações que você deseja entrar em vigor no Azure na **configurar** guia da página do portal de gerenciamento para seu aplicativo web (Role para baixo até a **configurações do aplicativo** e **cadeias de caracteres de conexão**  seções). Quando você implanta o projeto, o Azure aplica automaticamente as alterações. Para obter mais informações, consulte [Sites do Windows Azure: como cadeias de caracteres de aplicativo e o trabalho de cadeias de caracteres de Conexão](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Arquivos de transformação padrão

Na **Gerenciador de soluções**, expanda *Web. config* para ver os *Debug* e *Release* arquivos de transformação são criados por padrão para as configurações de compilação padrão de dois.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Você pode criar arquivos de transformação para configurações de compilação personalizada clicando duas vezes o arquivo Web. config e escolhendo **adicionar transformações de Config** no menu de contexto. Para este tutorial, você não precisa fazer isso, e a opção de menu está desabilitada, pois você não tiver criado quaisquer configurações de compilação personalizada.

Mais tarde você criará três arquivos mais de transformação, um para o teste, preparo e produção de perfis de publicação. Um exemplo típico de uma configuração que você usaria em um arquivo de transformação de perfil de publicação porque ela depende do ambiente de destino é um ponto de extremidade do WCF que é diferente para teste versus produção. Você criará publicar arquivos de transformação de perfil em tutoriais posteriores depois de criar perfis de publicação que eles acompanham.

## <a name="disable-debug-mode"></a>Desabilitar o modo de depuração

Um exemplo de uma configuração que depende da configuração de compilação em vez do ambiente de destino é o `debug` atributo. Para um build de versão, você normalmente deseja depuração desabilitada, independentemente de qual ambiente você está implantando em. Portanto, por padrão, o Visual Studio modelos de projeto criam *Release* transformar arquivos com o código que remove as `debug` de atributos do `compilation` elemento. Aqui está o padrão *Release*: além de um código de transformação de exemplo que é comentado, ele inclui código na `compilation` elemento remove o `debug` atributo:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

O `xdt:Transform="RemoveAttributes(debug)"` atributo especifica que você deseja que o `debug` atributo a ser removido do `system.web/compilation` elemento no implantado *Web. config* arquivo. Isso será feito sempre que você implantar uma compilação de versão.

## <a name="limit-error-log-access-to-administrators"></a>Limitar o acesso ao log de erros para administradores

Se houver um erro enquanto o aplicativo é executado, o aplicativo exibe uma página de erro genérico no lugar da página de erro gerada pelo sistema e ele usa o [pacote do NuGet do Elmah](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) para emissão de relatórios e log de erros. O `customErrors` elemento no aplicativo *Web. config* arquivo Especifica a página de erro:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Para ver a página de erro, altere temporariamente o `mode` atributo do `customErrors` elemento do "RemoteOnly" para "Ativado" e execute o aplicativo do Visual Studio. Causar um erro ao solicitar uma URL inválida, como *Studentsxxx.aspx*. Em vez de um erro gerado pelo IIS "o recurso não pode ser encontrado" página, você vê o *GenericErrorPage* página.

![Página de erro](web-config-transformations/_static/image2.png)

Para ver o log de erros, substitua tudo da URL após o número da porta com *elmah.axd* (por exemplo, `http://localhost:51130/elmah.axd`) e pressione Enter:

![Página do ELMAH](web-config-transformations/_static/image3.png)

Não se esqueça de definir o `customErrors` elemento para o modo de "RemoteOnly" quando terminar.

No computador de desenvolvimento é conveniente permitir o acesso gratuito à página de log de erro, mas em produção que seria um risco à segurança. Para o site de produção, você deseja adicionar uma regra de autorização que restringe o acesso ao log de erros para administradores e para certificar-se de que a restrição funciona, você desejar no teste e preparação também. Portanto, isso é outra alteração que você deseja implementar sempre que você implantar uma compilação de versão e, portanto, ele pertence a *Release* arquivo.

Abra *Release* e adicione um novo `location` elemento imediatamente antes do fechamento `configuration` de marca, conforme mostrado aqui.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

O `Transform` valor de atributo de "Insert" faz com que essa `location` elemento a ser adicionado como um irmão qualquer existente `location` elementos no *Web. config* arquivo. (Já houver um `location` regras de elemento que especifica a autorização para o **atualização créditos** página.)

Agora você pode visualizar a transformação para certificar-se de que o codificado corretamente.

Na **Gerenciador de soluções**, clique com botão direito *Release* e clique em **visualização transformar**.

![Menu de transformação de visualização](web-config-transformations/_static/image4.png)

Abre uma página que mostra o desenvolvimento *Web. config* arquivo à esquerda e o que implantado *Web. config* arquivo será semelhante à direita, com as alterações destacadas.

![Visualização de transformação de depuração](web-config-transformations/_static/image5.png)

![Visualização de transformação de local](web-config-transformations/_static/image6.png)

(Na visualização, você pode notar que algumas alterações adicionais que você não escreveu transforma para: elas normalmente envolvem a remoção de espaço em branco que não afeta a funcionalidade.)

Quando você testar o site após a implantação, você também vai testar para verificar se a regra de autorização é eficiente.

> [!NOTE] 
> 
> **Observação de segurança** nunca exibir detalhes do erro para o público em um aplicativo de produção, ou armazenar essas informações em um local público. Os invasores podem usar informações de erro para descobrir vulnerabilidades em um site. Se você usar o ELMAH no seu próprio aplicativo, configure para ELMAH para minimizar os riscos de segurança. O exemplo do ELMAH neste tutorial não deve ser considerado uma configuração recomendada. É um exemplo que foi escolhido para ilustrar como lidar com uma pasta que o aplicativo deve ser capaz de criar arquivos. Para obter mais informações, consulte [proteger o ponto de extremidade do ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Arquivos de transformação de perfil de publicação de uma configuração que você lidará na

Um cenário comum é ter *Web. config* as configurações que devem ser diferentes em cada ambiente que você implanta em arquivos. Por exemplo, um aplicativo que chama um serviço WCF talvez seja necessário um ponto de extremidade diferente em ambientes de teste e produção. O aplicativo Contoso University inclui uma configuração desse tipo também. Essa configuração controla um indicador visível nas páginas de um site que informa qual ambiente você está no, como desenvolvimento, teste ou produção. O valor da configuração determina se o aplicativo acrescentará "(desenvolvimento)" ou "(teste)" no cabeçalho principal do *Master* página mestra:

![Indicador de ambiente](web-config-transformations/_static/image7.png)

O indicador de ambiente é omitido quando o aplicativo é executado na preparação ou produção.

Páginas da web Contoso University ler um valor que é definido em `appSettings` no *Web. config* arquivo a fim de determinar qual o ambiente em que o aplicativo é executado no:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

O valor deve ser "Teste" no ambiente de teste e "Produção" para a preparação e produção.

O código a seguir em um arquivo de transformação implementará essa transformação:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

O `xdt:Transform` valor "SetAttributes" indica que a finalidade dessa transformação é alterar os valores de atributo de um elemento existente no atributo o *Web. config* arquivo. O `xdt:Locator` "Match(key)" indica que o elemento a ser modificado é aquele do valor do atributo cujo `key` atributo corresponde a `key` atributo especificado aqui. O apenas outro atributo do `add` elemento é `value`, e isso é o que será alterado em implantado *Web. config* arquivo. O código mostrado aqui faz com que o `value` atributo do `Environment` `appSettings` elemento a ser definido como "Test" no *Web. config* arquivo que é implantado.

Essa transformação pertence nos arquivos de transformação do perfil de publicação, que você ainda não tenha criado. Você vai criar e atualizar os arquivos de transformação que implementam essa alteração ao criar perfis de publicação para os ambientes de teste, preparação e produção. Você terá de fazer isso [implantar no IIS](deploying-to-iis.md) e [implantar na produção](deploying-to-production.md) tutoriais.

> [!NOTE]
> Porque essa configuração está na `<appSettings>` elemento, você tem outra alternativa para especificar a transformação quando você estiver implantando em aplicativos Web no serviço de aplicativo do Azure ver [Web. config especificando configurações no Azure](#watransforms) anteriormente neste tópico.


## <a name="setting-connection-strings"></a>Configurar cadeias de conexão

Embora o arquivo de transformação padrão contém um exemplo que mostra como atualizar uma cadeia de caracteres de conexão, na maioria dos casos você não precisa configurar transformações de cadeia de caracteres de conexão, porque você pode especificar cadeias de caracteres de conexão no perfil de publicação. Você terá de fazer isso [implantar no IIS](deploying-to-iis.md) e [implantar na produção](deploying-to-production.md) tutoriais.

## <a name="summary"></a>Resumo

Agora você ter feito tanto quanto você pode fazer com *Web. config* transformações antes de você criar os perfis de publicação, e você já viu uma visualização de quais serão no arquivo Web. config implantado.

![Visualização de transformação de local](web-config-transformations/_static/image8.png)

No tutorial a seguir, você vai cuidar das tarefas de configuração de implantação que requerem a definição de propriedades do projeto.

## <a name="more-information"></a>Mais informações

Para obter mais informações sobre os tópicos abordados por este tutorial, consulte [Web. config usando transformações para alterar as configurações no arquivo Web. config de destino ou no arquivo App. config durante a implantação](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) no mapa de conteúdo de implantação da Web para Visual Studio e ASP.NET.

> [!div class="step-by-step"]
> [Anterior](preparing-databases.md)
> [Próximo](project-properties.md)
