---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET e Web Tools 2013.2 para notas de versão do Visual Studio 2013 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 59e33d9bb1cef79e6d0e83fb3560e964723ad5e0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391438"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET e Web Tools 2013.2 para notas de versão do Visual Studio 2013
====================
por [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Notas de instalação

ASP.NET e Web Tools para Visual Studio 2013.2 são empacotadas no instalador do principal e pode ser baixado como parte da [Visual Studio 2013 atualização 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre o ASP.NET and Web Tools para Visual Studio 2013.2 estão disponíveis na [site da web ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Requisitos de software

ASP.NET e Web Tools para Visual Studio 2013.2 requer o Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Novos recursos no ASP.NET e Web Tools para Visual Studio 2013.2

As seções a seguir descrevem os recursos que foram introduzidos na versão.

- [Modelos de um projeto do ASP.NET](#oneaspnet)
- [Suporte a SSL ao iniciar os aplicativos Web no IIS Express](#ssl)
- [Aprimoramentos do Editor do Visual Studio Web](#vswebeditor)
- [Link do navegador](#browserlink)
- [Suporte para aplicativos de Web do serviço de aplicativo do Azure no Visual Studio](#waws)
- [Criar recursos do Azure remotos ao criar um novo projeto Web](#AzureResources)
- [Aprimoramentos de publicar Web](#webpublish)
- [Scaffolding do ASP.NET](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Web Forms do ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [2.1.2 da API Web ASP.NET](#webapi)
- [3.1.2 de páginas da Web ASP.NET](#webpages)
- [Entity Framework 6.1](#ef)
- [O ASP.NET Identity 2.0.0](#identity)
- [Componentes do Microsoft OWIN](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Modelos de um projeto do ASP.NET

- Atualizações para modelos de projeto do ASP.NET para dar suporte à confirmação de conta e redefinição de senha.
- Atualize o modelo de API Web do ASP.NET para dar suporte à autenticação usando no local contas organizacionais.
- O modelo de SPA ASP.NET agora contém a autenticação baseada em modos de exibição do MVC e o servidor. O modelo tem um controlador de API da Web que pode ser acessado somente por usuários autenticados.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Suporte a SSL ao iniciar os aplicativos Web no IIS Express

Para eliminar o aviso de segurança quando a navegação e depuração HTTPS no localhost, adicionamos uma caixa de diálogo para permitir que o Internet Explorer e Chrome para confiar o autoassinado do IIS express certificado SSL.

Por exemplo, uma propriedade de projeto da web pode ser definida para usar SSL. Clique em F4 para abrir a caixa de diálogo de propriedades. Alteração **SSL habilitado** como true. Copie a URL do SSL.

![Propriedade Enabled do SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Definir guia web projeto propriedade página da web para usar o HTTPS com base em URL (a URL do SSL será `https://localhost:44300/` , a menos que você criou Sites da Web SSL anteriormente.)

![Definir a URL do projeto (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Pressione CTRL+F5 para executar o aplicativo. Siga as instruções para confiar no certificado autoassinado que o IIS Express gerou.

![Aviso de SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Leia as **aviso de segurança** caixa de diálogo e clique **Sim** se você quiser instalar o certificado que representa o localhost.

![Aviso de segurança](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

O site será mostrado no Internet Explorer ou Chrome sem aviso de certificado no navegador.

![Página HTTPS sem avisos](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

O Firefox usa seu próprio repositório de certificados, portanto, ele exibirá um aviso.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Aprimoramentos do Editor do Visual Studio Web

- **Novo item de projeto do JSON e editor**: adicionamos um item de projeto do JSON e o editor para o Visual Studio. Recursos do editor de JSON atuais incluem colorização, validação de sintaxe, preenchimento de chaves, de estrutura de tópicos, configuração da opção de ferramentas e muito mais.

    ![JSON Editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    O IntelliSense agora dá suporte à [esquema JSON](http://json-schema.org/) v3 e v4. Há uma caixa de combinação de esquema para escolher esquemas existentes, edite o caminho do local de esquema, ou simplesmente arrastar e soltar um arquivo de projeto JSON a ele para obter o caminho relativo.

    ![Intellisense JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor de esquema JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Novo editor de Sass (SCSS)**: adicionamos menor no VS2013 RTM, e agora temos um item de projeto do Sass e editor. Editor do sass recursos são comparáveis para o editor do LESS e incluem colorização, variável e Mixins IntelliSense, remova os comentários comentário /, informações rápidas, formatação, validação de sintaxe, estrutura de tópicos, ir para definição, seletor de cores, as ferramentas de configuração de opção de etc.

    ![Adicionar Novo Item: Folha de estilo SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor de folhas de estilo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Novo seletor de URL em HTML, Razor, CSS, menos e documentos de Sass:** VS 2013 é fornecido com nenhum seletor de URL fora de páginas de Web Forms. O novo seletor de URL para HTML, Razor, CSS, LESS e Sass editores é um seletor de digitação livre de caixa de diálogo, fluente que compreende '.. ' e listas de arquivo de filtros para obter links e marcas img adequadamente.

    ![Seletor de URL para a marca de imagem](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Seletor de URL para modos de exibição](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Seletor de URL para o CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Atualizações para o editor do LESS, adicionando mais recursos**
- **Atualização de Intellisense do Knockout**: adicionamos uma sintaxe KnockOut não padrão para o intelliSense do VS, "viewModel ko-vs-editor:" sintaxe. Ele pode ser usado para associar a vários modelos de exibição em uma página usando comentários na forma:

    ![Intellisense do Knockout](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Também adicionamos suporte ao IntelliSense, ViewModel aninhados, portanto, você pode analisar os objetos profundamente aninhados no ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    O IntelilSense exibido é o IntelliSense completo do objeto JavaScript.

    ![IntelliSense mostrando completa do objeto JavaScript](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Novo seletor de URL em HTML, Razor, CSS, menos e documentos de Sass**: VS 2013 é fornecido com nenhum seletor de URL fora de páginas de Web Forms. O novo seletor de URL para HTML, Razor, CSS, LESS e Sass editores é um seletor de digitação livre de caixa de diálogo, fluente que compreende '.. ' e listas de arquivo de filtros para obter links e marcas img adequadamente.

    ![Seletor de URL para a marca de imagem](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Seletor de URL para modos de exibição](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Seletor de URL para o CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Link do navegador

- Link do navegador agora dá suporte a conexões HTTPS e listará que no painel com outras conexões, desde o certificado é confiável pelo navegador.
- Mapeamento de código-fonte HTML estático
- SPA suporte para mapeamento de dados
- Dados de mapeamento de atualização automática

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Suporte para aplicativos de Web do serviço de aplicativo do Azure no Visual Studio

- **Suporte Azure entrar.**
- **Exibição remota para aplicativos web e depuração remota**: agora, damos suporte [depuração remota para aplicativos web no serviço de aplicativo do Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) e visualização remota de arquivos de conteúdo de aplicativo da web no Gerenciador de servidores.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Criar recursos do Azure remotos ao criar um novo projeto Web

Adicionamos do Azure ["Criar recursos remotos"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) caixa de seleção na caixa de diálogo Novo do aplicativo web. Escolhendo-lo, você será capaz de se integrar a experiência de criar um novo aplicativo web, como configurar o site de publicação do Azure para teste e criação de perfil de publicação em poucas etapas simples.

![Novo projeto com recursos do Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publicação no Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Aprimoramentos de publicar Web

- Melhore a experiência do usuário para a publicação.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding do ASP.NET

- **Suporte a enum:** se seu modelo estiver usando enumerações, o Scaffolder de MVC irá gerar o menu suspenso de Enum. Isso usa os auxiliares de Enum no MVC.
- **Inicializar o suporte**: atualizados os modelos de EditorFor no Scaffolding do MVC para que eles usam as classes de inicialização.
- **Suporte de pacote**: MVC e os Scaffolders do API da Web adicionará 5.1 pacotes de MVC e API da Web

As capturas de tela a seguir demonstram os modelos de scaffolding.

- Código de modelo:

     ![Código de modelo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compilar o código de modelo, clique com botão direito e selecione **Add**, **Novo Item de Scaffold**.

     ![Adicionar Novo Item com Scaffold](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Escolher **MVC5 controlador com exibições, usando o Entity Framework**:

     ![Adicionar novo controlador MVC5 com modos de exibição](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Adicionar controlador** usando o modelo:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Verifique o código gerado, por exemplo contém Views/WeekdayModels/Edit.cshtml `@Html.EnumDropDownListFor`: ![exibição contendo EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Execute a página para ver a caixa de combinação de enum gerada, observe que, se um valor pode ser nulo, uma cadeia de caracteres vazia pode ser escolhida para a caixa de combinação. Por exemplo, o **criar** página mostra o seguinte:

    ![Caixa de combinação, permitindo que a cadeia de caracteres vazia](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 que RTM será lançado em abril de 2014. Aqui estão os pontos de destaque das notas de versão, mas verifique se o [notas de versão completas](http://docs.nuget.org/docs/release-notes/nuget-2.8) para obter mais informações sobre essas alterações.

- **Destinar aplicativos do Windows Phone 8.1**: 2.8.1 o NuGet agora dá suporte ao direcionamento de aplicativos do Windows Phone 8.1 usando os monikers da estrutura de destino 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' e 'WPA81'.
- **Resolução de patch para dependências**: para resolver as dependências do pacote, o NuGet historicamente implementou uma estratégia de selecionar a versão mais antiga do pacote principal e secundária que satisfaça as dependências do pacote. Ao contrário da versão principal e secundária, no entanto, a versão de patch sempre foi resolvida para a versão mais recente. Embora o comportamento era bem-intencionado, ele criou uma falta de determinismo para instalar pacotes com dependências.
- **A opção DependencyVersion**: Embora o NuGet 2.8 altera o *padrão* comportamento para resolver as dependências, ele também adiciona um controle mais preciso sobre o processo de resolução de dependência por meio da opção DependencyVersion - na console do Gerenciador de pacotes. O comutador permite resolver as dependências para a versão mais antiga possível (comportamento padrão), a versão mais alta possível, ou o mais alto minor ou versão de patch. Essa opção só funciona para o pacote de instalação no comando do powershell.
- **Atributo DependencyVersion**: além do comutador - DependencyVersion detalhado acima, o NuGet também é possível para a capacidade de definir um novo atributo no arquivo NuGet. config definindo o que é o valor padrão, se o DependencyVersion - alternar não foi especificado em uma invocação de um pacote de instalação. Esse valor também será respeitado pela caixa de diálogo Gerenciador de pacotes NuGet para operações de pacote de instalação. Para definir esse valor, adicione o atributo abaixo ao seu arquivo NuGet. config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Visualizar as operações do NuGet com - whatif**: pacotes NuGet alguns podem ter gráficos de dependência profunda, e como tal, ele pode ser útil durante a instalação, desinstalar ou operação de atualização para o primeiro ver o que acontecerá. NuGet 2.8 adiciona o PowerShell padrão-se mudar para os comandos do pacote de atualização, desinstalar-package e install-package para habilitar a visualizar o fechamento inteiro de pacotes para o qual o comando será aplicado.
- **Fazer o downgrade de pacote**: não é incomum para instalar uma versão de pré-lançamento de um pacote para investigar os novos recursos e então decidir se deseja reverter para a última versão estável. Antes do NuGet 2.8, isso era um processo de várias etapa de desinstalar o pacote de pré-lançamento e suas dependências e, em seguida, instalar a versão anterior. Com o NuGet 2.8, no entanto, o pacote de atualização agora reverterá o fechamento do pacote inteiro (por exemplo, árvore de dependência do pacote) para a versão anterior.
- **Dependências de desenvolvimento**: muitos tipos diferentes de recursos podem ser entregues como pacotes do NuGet – incluindo as ferramentas que são usadas para otimizar o processo de desenvolvimento. Esses componentes, embora eles podem ser muito útil no desenvolvimento de um novo pacote, não devem ser consideradas uma dependência do novo pacote quando ele é mais recente publicada. NuGet 2.8 habilita um pacote para se identificar no arquivo. NuSpec como um developmentDependency. Quando instalado, esses metadados também serão adicionados ao arquivo Packages. config do projeto no qual o pacote foi instalado. Quando esse arquivo Packages. config é posteriormente analisado para as dependências do NuGet durante nuget.exe pack, ele excluirá essas dependências marcadas como dependências de desenvolvimento.
- **Arquivos Packages. config individuais para diferentes plataformas**: ao desenvolver aplicativos para várias plataformas de destino, é comum ter diferentes arquivos de projeto para cada um dos ambientes de compilação respectivo. Também é comum para consumir diferentes pacotes do NuGet em arquivos de projeto diferentes, como pacotes têm diferentes níveis de suporte para plataformas diferentes. NuGet 2.8 oferece suporte aprimorado para este cenário, criando arquivos Packages. config diferente para arquivos de projeto diferente de específico da plataforma.
- **Fallback para o Cache Local**: os pacotes NuGet que normalmente são consumidos de uma galeria remota, como o [Galeria do NuGet](http://www.nuget.org) usando uma conexão de rede, há muitos cenários em que o cliente não está conectado. Sem uma conexão de rede, o cliente do NuGet não foi capaz de instalar com êxito pacotes – mesmo quando esses pacotes já estavam na máquina do cliente no cache local do NuGet. NuGet 2.8 adiciona o fallback de cache automático para o console do Gerenciador de pacotes.

    O recurso de fallback de cache não requer nenhum argumento de comando específico. Além disso, o cache fallback atualmente funciona apenas no console de Gerenciador de pacotes – o comportamento não funciona no momento na caixa de diálogo de Gerenciador de pacote.
- **Correções de bug**: uma das principais correções de bug feitas foi melhoria de desempenho no pacote de atualização-reinstale o comando.

    Além desses recursos e a correção de desempenho mencionado anteriormente, esta versão do NuGet também inclui muitas correções de bugs. Havia 181 total de problemas abordados na versão. Para obter uma lista completa de trabalho itens corrigidos no NuGet 2.8, por favor, modo de exibição de [rastreador de problemas do NuGet](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) para esta versão.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Web Forms do ASP.NET

- Os modelos de formulários da Web agora mostram como fazer a confirmação de conta e redefinição de senha para a identidade do ASP.NET.
- O controle de fonte de dados de entidade e o provedor de dados dinâmico para o Entity Framework 6. Para obter mais detalhes, consulte o seguinte blog do MSDN: [provedor de dados dinâmicos e controle EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Aprimoramentos de roteamento de atributo](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Suporte de inicialização para modelos de editor](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Suporte a enum nos modos de exibição](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Suporte a Unobstrusive MinLength / MaxLength atributos](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Suporte a contexto 'this' em Ajax discreto](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Vários [correções de bugs](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>2.1.2 da API Web ASP.NET

- [Tratamento de erro global](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Aprimoramentos de roteamentos de atributo](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Melhorias na página de ajuda](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Suporte de IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formatador de tipo de mídia BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Melhor suporte para filtros assíncronos](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Análise para o cliente de biblioteca de formatação de consulta](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Vários [correções de bugs](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>3.1.2 de páginas da Web ASP.NET

- Vários [correções de bugs](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework foi atualizado para a versão 6.1 para tempo de execução e ferramentas. 6.1 Entity Framework (EF) é uma pequena atualização para o Entity Framework 6 e inclui uma série de correções de bugs e novos recursos. Para obter informações detalhadas sobre o EF6.1, incluindo links para documentação para os novos recursos, consulte [histórico de versão do Entity Framework](https://msdn.microsoft.com/data/jj574253). Os novos recursos nesta versão incluem:

- **Consolidação de ferramentas** fornece uma maneira consistente para criar um novo modelo do EF. Esse recurso estende o Assistente de modelo de dados de entidade ADO.NET para dar suporte à criação de modelos Code First, incluindo engenharia reversa de um banco de dados existente. Esses recursos estavam disponíveis anteriormente na qualidade Beta no EF Power Tools.
- **Tratamento de falhas de confirmação da transação** fornece as novas [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) que usa a capacidade de recentemente introduzida para interceptar operações de transação. O **CommitFailureHandler** permite a recuperação automática de falhas de conexão enquanto confirmar uma transação.
- **IndexAttribute** permite que os índices seja especificado, colocando um atributo em uma propriedade (ou propriedades) em seu modelo Code First. Código pela primeira vez, em seguida, criará um índice correspondente no banco de dados.
- **A API pública do mapeamento** fornece acesso às informações de EF tem sobre como as propriedades e tipos são mapeados para colunas e tabelas no banco de dados. Em versões anteriores dessa API foi interna.
- **Capacidade de configurar interceptores por meio do arquivo App/Web.config**(permitindo que os interceptadores a ser adicionado sem recompilar o aplicativo).
- **DatabaseLogger** é um novo interceptador que torna mais fácil registrar todas as operações de banco de dados em um arquivo. Em combinação com o recurso anterior, isso permite que você alterne facilmente o registro em log de operações de banco de dados para um aplicativo implantado, sem a necessidade de recompilar.
- **Detecção de alteração do modelo de migrações** foi aprimorado para que as migrações gerado por scaffolding sejam mais precisas; o desempenho do processo de detecção de alteração também foi bastante aprimorado.
- **Melhorias de desempenho** incluindo operações de redução do banco de dados durante a inicialização, otimizações para comparação de igualdade nulo em consultas LINQ, mais rápido exibir geração (criação de modelo) em mais cenários e mais eficiente materialização de entidades controladas com várias associações.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>O ASP.NET Identity 2.0.0

- **Autenticação de dois fatores**: identidade do ASP.NET agora dá suporte à autenticação de dois fatores. Autenticação de dois fatores fornece uma camada extra de segurança para suas contas de usuário no caso em que sua senha for comprometida. Também há proteção para ataques de força bruta contra os códigos de dois fatores.
- **Bloqueio de conta:** fornece uma maneira de bloquear o usuário se o usuário digitar incorretamente sua senha ou códigos de dois fatores. O número de tentativas inválidas e o período de tempo para que os usuários estão bloqueados pode ser configurado. Um desenvolvedor pode, opcionalmente, desativar o bloqueio de conta para determinadas contas de usuário caso precisem.
- **Confirmação de conta:** o sistema de identidade do ASP.NET agora dá suporte à confirmação da conta. Isso é um cenário bastante comum na maioria dos sites hoje em dia em que, quando você registra uma nova conta no site, é necessário confirmar seu email antes de poder fazer qualquer coisa no site do. Email de confirmação é útil porque ele evita que contas falsas que está sendo criado. Isso é extremamente útil se você estiver usando o email como um método de comunicação com os usuários do seu site, como sites, serviços bancários, comércio eletrônico ou sites sociais do Fórum.
- **A redefinição de senha:** redefinição de senha é um recurso em que o usuário pode redefinir sua senha se eles tem esquecido sua senha.
- **Carimbo de segurança (sinal de saída em todos os lugares):** dá suporte a uma maneira para regenerar o Token de segurança para o usuário em casos quando o usuário altera sua senha ou qualquer outro tipo de segurança de informações, como remover um logon associado (como Facebook, Google, relacionadas ao Conta da Microsoft e assim por diante). Isso é necessário para garantir que todos os tokens gerados com a senha antiga são invalidados. No projeto de exemplo, no se você alterar a senha do usuário, em seguida, um novo token é gerado para o usuário e todos os tokens anteriores são invalidados. Esse recurso fornece uma camada extra de segurança para seu aplicativo desde quando você alterar sua senha, você será desconectado de todos os lugares (todos os outros navegadores) em que você fez logon no aplicativo.
- **Verifique o tipo de chave primária a ser extensível para usuários e funções**: no ASP.NET Identity 1.0, o tipo de chave primária para a tabela de usuários e funções era cadeias de caracteres. Isso significa que quando o sistema ASP.NET Identity foi mantido no SQL Server usando o Entity Framework, estávamos usando nvarchar. Havia muitas discussões sobre esta implementação padrão no Stack Overflow e com base nos comentários recebidos. Nós fornecemos um gancho de extensibilidade onde você pode especificar qual deve ser a chave primária da tabela de usuários e funções. Esse gancho de extensibilidade é particularmente útil que se você estiver migrando seu aplicativo e o aplicativo estava armazenando UserIds são GUIDs ou ints.
- **Suporte a IQueryable em usuários e funções**: suporte adicionado para IQueryable em UsersStore e RolesStore, você pode obter facilmente a lista de usuários e funções.
- **Operação de exclusão de suporte por meio do UserManager**
- **A indexação em nome de usuário**: implementação de estrutura de entidade no ASP.NET Identity, adicionamos um índice exclusivo no nome de usuário usando o novo IndexAttribute no EF 6.1.0. Isso torna-se de que os nomes de usuário são sempre exclusivos e não havia nenhuma condição de corrida em que você pode acabar com nomes de usuário duplicado.
- **Validador de senha avançada:** o validador de senha que foi entregue no ASP.NET Identity 1.0 foi um validador de senha bem básica que só foi Validando o comprimento mínimo. Há um novo validador de senha que lhe dá mais controle sobre a complexidade da senha. Observe que, mesmo se você ativar todas as configurações nessa senha, incentivamos você a habilitar a autenticação de dois fatores para as contas de usuário.
- **IdentityFactory Middleware / CreatePerOwinContext**:

    - **Gerenciador de usuários**: você pode usar a implementação de fábrica para obter uma instância do UserManager do contexto OWIN. Esse padrão é semelhante ao que usamos para obter AuthenticationManager de contexto do OWIN para entrar e sair. Essa é uma maneira recomendada de obtenção de uma instância do UserManager por solicitação para o aplicativo.
    - **DbContextFactory**: identidade do ASP.NET usa o Entity Framework para manter o sistema de identidade no SQL Server. Para fazer isso no sistema de identidade tem uma referência para o ApplicationDbContext. O DbContextFactory Middleware retorna uma instância do ApplicationDbContext por solicitação que você pode usar em seu aplicativo.
- **Pacote de NuGet de exemplos de identidade do ASP.NET**: O exemplos pacote NuGet pode tornar mais fácil de instalar e executar os exemplos para a identidade do ASP.NET e siga as práticas recomendadas. Este é um aplicativo ASP.NET MVC de exemplo. Modifique o código de acordo com seu aplicativo antes de implantá-lo em produção. O exemplo deve ser instalado em um aplicativo ASP.NET vazio. Para obter mais informações sobre o pacote, vá para a seguinte postagem de blog: [anunciando o RTM do ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Componentes do Microsoft OWIN

Havia muitos bugs que foram corrigidos nesta versão. Consulte a [notas de versão para o 2.1.0 versão](https://katanaproject.codeplex.com/releases/view/113281) para obter mais informações.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Havia muitos bugs que foram corrigidos nesta versão. Consulte a [notas de versão para o 2.0.2 versão](https://github.com/SignalR/SignalR/releases/tag/2.0.2) para obter mais informações.
