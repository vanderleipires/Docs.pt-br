---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: "ASP.NET e Web ferramentas 2013.2 para notas de versão do Visual Studio 2013 | Microsoft Docs"
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: d3a8183fecaf830b2ee1211acd56da86454b4437
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET e Web Tools 2013.2 para notas de versão do Visual Studio 2013
====================
por [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Notas de instalação

ASP.NET e Web Tools para 2013.2 do Visual Studio são empacotadas no instalador do principal e pode ser baixado como parte do [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentação

Tutoriais e outras informações sobre o ASP.NET e Web Tools para 2013.2 do Visual Studio estão disponíveis no [site da web ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Requisitos de software

ASP.NET e Web Tools para 2013.2 do Visual Studio requer o Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Novos recursos no ASP.NET e Web Tools para 2013.2 do Visual Studio

As seções a seguir descrevem os recursos que foram introduzidos na versão.

- [Modelos de um projeto do ASP.NET](#oneaspnet)
- [Dá suporte a SSL ao iniciar os aplicativos Web no IIS Express](#ssl)
- [Aprimoramentos do Editor do Visual Studio Web](#vswebeditor)
- [Link do navegador](#browserlink)
- [Suporte para aplicativos de Web do serviço de aplicativo do Azure no Visual Studio](#waws)
- [Criar recursos do Azure remotos ao criar um novo projeto da Web](#AzureResources)
- [Aprimoramentos de publicação da Web](#webpublish)
- [Estrutura do ASP.NET](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Web Forms do ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [Páginas da Web do ASP.NET 3.1.2](#webpages)
- [Entity Framework 6.1](#ef)
- [Identidade do ASP.NET 2.0.0](#identity)
- [Componentes do Microsoft OWIN](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Modelos de um projeto do ASP.NET

- Atualizações para modelos de projeto do ASP.NET para oferecer suporte a confirmação de conta e redefinição de senha.
- Atualize o modelo de API da Web ASP.NET para oferecer suporte à autenticação usando no local contas organizacionais.
- O modelo ASP.NET SPA agora contém a autenticação se baseia em modos de exibição do MVC e servidor. O modelo tem um controlador de WebAPI que só pode ser acessado por usuários autenticados.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Dá suporte a SSL ao iniciar os aplicativos Web no IIS Express

Para eliminar o aviso de segurança ao navegar e depuração HTTPS no localhost, adicionamos uma caixa de diálogo para permitir que o Internet Explorer e o Chrome confiar autoassinado IIS express certificado SSL.

Por exemplo, uma propriedade de projeto da web pode ser definida para usar SSL. Clique em F4 para abrir a caixa de diálogo de propriedades. Alterar **SSL habilitado** como true. Copie a URL de SSL.

![Propriedade Enabled do SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Conjunto de guia web projeto propriedade página da web para usar o HTTPS com base em URL (o URL de SSL será `https://localhost:44300/` , a menos que você criou anteriormente SSL Web Sites.)

![Definir a URL do projeto (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Pressione CTRL+F5 para executar o aplicativo. Siga as instruções para confiar no certificado autoassinado que o IIS Express gerou.

![Aviso de SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Leitura de **aviso de segurança** caixa de diálogo e clique **Sim** se você deseja instalar o certificado que representa o localhost.

![Aviso de segurança](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

O site será mostrado no Internet Explorer ou Chrome sem aviso de certificado no navegador.

![Página HTTPS sem avisos](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox usa seu próprio repositório de certificados, portanto ele exibirá um aviso.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Aprimoramentos do Editor do Visual Studio Web

- **Novo item de projeto JSON e editor**: adicionamos um item de projeto JSON e o editor para o Visual Studio. Recursos do editor de JSON atuais incluem colorização, validação de sintaxe, preenchimento de chave, estrutura de tópicos, configuração da opção de ferramentas e muito mais.

    ![Editor de JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense agora dá suporte a [esquema JSON](http://json-schema.org/) v3 e v4. Há uma caixa de combinação de esquema para escolher esquemas existentes, edite o caminho do local do esquema, ou simplesmente arrastar e soltar um arquivo JSON de projeto-o para obter o caminho relativo.

    ![Intellisense JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor de esquema JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Novo editor de Sass (SCSS)**: adicionamos menor no VS2013 RTM, e agora temos um item de projeto Sass e editor. Editor de sass recursos são comparáveis ao menos editor e incluem colorização, variável e Mixins IntelliSense, remova os comentários /, informações rápidas, formatação, validação de sintaxe, estrutura de tópicos, ir para definição, seletor de cores, ferramentas de configuração de opção etc.

    ![Adicionar Novo Item: Folha de estilo SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor de folha de estilo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Novo seletor de URL em HTML, Razor, CSS, menor e documentos Sass:** VS 2013 acompanha nenhum seletor de URL fora páginas Web Forms. O novo seletor de URL para HTML, Razor, CSS, menor e Sass editores é um fluente, livre de caixa de diálogo Seletor de digitação compreende '... ' e listas de arquivo de filtros para obter links e marcas img adequadamente.

    ![Seletor de URL para a marca de imagem](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Seletor de URL para modos de exibição](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![URL de seletor de CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Atualizações para o editor LESS com a adição de mais recursos**
- **Atualização do Intellisense Knockout**: adicionamos uma sintaxe de separação não padrão para VS intelliSense, "ko-vs-editor viewModel:" sintaxe. Ele pode ser usado para associar a vários modelos de exibição em uma página com comentários no formulário:

    ![Intellisense de separação](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    Também adicionamos suporte para ViewModel IntelliSense aninhadas, portanto você pode analisar os objetos profundamente aninhados no ViewModel.

    `<div data-bind="text: foo.bar.baz.etc" />`

    O IntelilSense exibido é o IntelliSense completo do objeto JavaScript.

    ![IntelliSense mostrando completa do objeto JavaScript](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Novo seletor de URL em HTML, Razor, CSS, menor e documentos Sass**: VS 2013 acompanha nenhum seletor de URL fora páginas Web Forms. O novo seletor de URL para HTML, Razor, CSS, menor e Sass editores é um fluente, livre de caixa de diálogo Seletor de digitação compreende '... ' e listas de arquivo de filtros para obter links e marcas img adequadamente.

    ![Seletor de URL para a marca de imagem](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Seletor de URL para modos de exibição](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![URL de seletor de CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Link do navegador

- Link do navegador agora dá suporte a conexões HTTPS e listará que no painel com outras conexões desde que o certificado é confiável pelo navegador.
- Mapeamento de fonte HTML estático
- SPA oferece suporte para dados de mapeamento
- Dados de mapeamento de atualização automática

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Suporte para aplicativos de Web do serviço de aplicativo do Azure no Visual Studio

- **Suporte ao Azure entrar.**
- **Depuração remota e exibição remota dos aplicativos web**: agora temos suporte para [depuração remota para os aplicativos web no serviço de aplicativo do Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) e exibição remota dos arquivos de conteúdo de aplicativo web no Gerenciador de servidores.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Criar recursos do Azure remotos ao criar um novo projeto da Web

Adicionamos um Azure ["Criar recursos remotos"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) caixa de seleção na caixa de diálogo Novo do aplicativo web. Escolhendo-lo, você será capaz de integrar a experiência de criação de um novo aplicativo web, configurando o site de publicação do Azure para teste e criação de perfil de publicação em algumas etapas simples.

![Novo projeto com recursos do Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publicação no Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Aprimoramentos de publicação da Web

- Melhore a experiência do usuário para a publicação.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Estrutura do ASP.NET

- **Suporte de enum:** se seu modelo estiver usando Enums, o MVC Scaffolder irá gerar suspenso para Enum. Isso usa os auxiliares de Enum no MVC.
- **Inicializar o suporte**: atualizado os modelos de EditorFor na estrutura do MVC, de modo que eles usam as classes de inicialização.
- **Pacote de suporte**: MVC e Web API Scaffolders adicionará 5.1 pacotes para MVC e a API da Web

As capturas de tela a seguir demonstram os modelos de scaffolding.

- Código de modelo:

     ![Código de modelo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compile o código de modelo, clique com botão direito e selecione **adicionar**, **Novo Item de Scaffold**.

     ![Adicionar Novo Item de scaffolding](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Escolha **MVC5 controlador com modos de exibição usando o Entity Framework**:

     ![Adicionar novo controlador MVC5 com modos de exibição](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Adicionar controlador** usando o modelo:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Verifique o código gerado, por exemplo, Views/WeekdayModels/Edit.cshtml contém `@Html.EnumDropDownListFor`: ![exibir contendo EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Execute a página para ver a caixa de combinação de enum gerada, observe que, se um valor pode ser nulo, uma cadeia de caracteres vazia pode ser escolhida para a caixa de combinação. Por exemplo, o **criar** página mostra o seguinte:

    ![Caixa de combinação, permitindo que a cadeia de caracteres vazia](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 que RTM será lançado em abril de 2014. Aqui estão os pontos principais das notas de versão, mas verifique se o [notas de versão completo](http://docs.nuget.org/docs/release-notes/nuget-2.8) para obter mais informações sobre essas alterações.

- **Destino de aplicativos do Windows Phone 8.1**: NuGet 2.8.1 agora dá suporte ao direcionamento de aplicativos do Windows Phone 8.1 usando os identificadores de framework de destino 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' e 'WPA81'.
- **Resolução de patch para dependências**: ao resolver as dependências do pacote, o NuGet historicamente implementou uma estratégia de selecionar a versão do pacote de principais e secundárias menor que satisfaz as dependências do pacote. Ao contrário da versão principal e secundária, no entanto, a versão de patch sempre foi resolvida para a versão mais recente. Embora o comportamento foi bem intencionado, ele criado uma falta de determinismo para instalar os pacotes com dependências.
- **Comutador DependencyVersion**: 2.8 do NuGet que altera o *padrão* comportamento para resolver as dependências, ele também adiciona um controle mais preciso sobre o processo de resolução de dependência via switch DependencyVersion - na console do Gerenciador de pacote. A opção permite que a resolução de dependências para a versão mais baixa possível (comportamento padrão), a versão mais alta possível, ou o menor mais alto ou versão de patch. Essa opção só funciona para o pacote de instalação no comando do powershell.
- **Atributo DependencyVersion**: além do comutador - DependencyVersion detalhado acima, o NuGet também tem permitido para a capacidade de definir um novo atributo no arquivo de config definindo o que é o valor padrão, se o DependencyVersion - alternar não é especificado em uma invocação do pacote de instalação. Esse valor também será respeitado pela caixa de diálogo Gerenciador de pacotes do NuGet para qualquer operação de pacote de instalação. Para definir esse valor, adicione o atributo abaixo ao seu arquivo de config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Visualizar NuGet operações com - whatif**: NuGet alguns pacotes podem ter gráficos de dependência profunda e como tal, pode ser útil durante a instalação, desinstalar ou operação de atualização para o primeiro consulte o que acontecerá. 2.8 NuGet adiciona o PowerShell standard-se alternar para os comandos de pacote de instalação, desinstale o pacote e pacote de atualização para habilitar visualizando o encerramento inteiro de pacotes para o qual o comando será aplicado.
- **Fazer o downgrade de pacote**: não é incomum para instalar uma versão de pré-lançamento de um pacote para investigar os novos recursos e então decidir se deseja reverter para a última versão estável. Antes do NuGet 2.8, isso era um processo de várias etapa de desinstalar o pacote de pré-lançamento e suas dependências e, em seguida, instalar a versão anterior. Com o NuGet 2.8, no entanto, o pacote de atualização agora reverterá o encerramento de pacote inteiro (por exemplo, árvore de dependência do pacote) para a versão anterior.
- **Dependências de desenvolvimento**: muitos tipos diferentes de recursos podem ser fornecidos como pacotes do NuGet - incluindo ferramentas que são usadas para otimizar o processo de desenvolvimento. Esses componentes, enquanto que podem ser fundamental para o desenvolvimento de um novo pacote, não devem ser consideradas uma dependência do novo pacote quando ele é posterior publicado. 2.8 NuGet permite que um pacote identificar-se no arquivo. NuSpec como um developmentDependency. Quando instalado, esses metadados também serão adicionado ao arquivo Packages. config do projeto no qual o pacote foi instalado. Quando esse arquivo Packages. config é analisado posteriormente para dependências do NuGet durante nuget.exe pacote, ele excluirá essas dependências marcadas como dependências de desenvolvimento.
- **Arquivos individuais Packages para diferentes plataformas**: ao desenvolver aplicativos para várias plataformas de destino, é comum ter diferentes arquivos de projeto para cada um dos ambientes de compilação do respectivos. Também é comum para consumir os diferentes pacotes do NuGet em arquivos de projeto diferente, como pacotes têm vários níveis de suporte para diferentes plataformas. 2.8 NuGet oferece suporte aprimorado para este cenário Criando Packages diferentes arquivos para arquivos de projeto específico da plataforma diferente.
- **Fallback para o Cache Local**: pacotes do NuGet que normalmente são consumidos de uma galeria remota, como o [Galeria NuGet](http://www.nuget.org) usando uma conexão de rede, há muitos cenários em que o cliente não está conectado. Sem uma conexão de rede, o cliente do NuGet não pôde instalar com êxito os pacotes - mesmo quando esses pacotes já estavam na máquina do cliente no cache local NuGet. 2.8 NuGet adiciona automática de cache fallback para o console do Gerenciador de pacote.

    O recurso de fallback de cache não requer quaisquer argumentos de comando específico. Além disso, o cache fallback atualmente funciona apenas no console do Gerenciador de pacote - o comportamento não funciona na caixa de diálogo de Gerenciador de pacote.
- **Correções de bugs**: as principais correções feitas foram melhoria de desempenho no pacote de atualização-reinstale o comando.

    Além desses recursos e a correção de desempenho mencionados acima, esta versão do NuGet também inclui muitas correções de bugs. Não havia 181 questões total na versão. Para obter uma lista completa do trabalho de itens corrigidos no NuGet 2.8,. exibir o [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) para esta versão.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Web Forms do ASP.NET

- Os modelos de Web Forms mostram como fazer a confirmação de conta e redefinição de senha para a identidade do ASP.NET.
- O controle de fonte de dados de entidade e o provedor de dados dinâmico para Entity Framework 6. Para obter mais detalhes, consulte o seguinte blog do MSDN: [provedor de dados dinâmicos e controle de EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Aprimoramentos de roteamentos de atributo](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Suporte de inicialização para modelos do editor](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Suporte a enum nos modos de exibição](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Suporte a Unobstrusive MinLength / MaxLength atributos](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Dando suporte o contexto 'this' em Ajax discreto](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Vários [correções de bugs](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Tratamento de erro global](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Aprimoramentos de roteamentos de atributo](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Aprimoramentos da página de ajuda](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Suporte de IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formatador de tipo de mídia BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Melhor suporte para filtros de async](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Consulta de análise para o cliente de biblioteca de formatação](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Vários [correções de bugs](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>Páginas da Web do ASP.NET 3.1.2

- Vários [correções de bugs](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Foi atualizado para a versão 6.1 para tempo de execução e ferramentas do Entity Framework. 6.1 Entity Framework (EF) é uma pequena atualização para o Entity Framework 6 e inclui uma série de novos recursos e correções de bug. Para obter informações detalhadas sobre EF6.1, incluindo links para documentação para os novos recursos, consulte [histórico de versão do Entity Framework](https://msdn.microsoft.com/en-US/data/jj574253). Os novos recursos nesta versão incluem:

- **Ferramentas de consolidação** fornece uma maneira coerente de criar um novo modelo EF. Esse recurso estende o Assistente de modelo de dados de entidade ADO.NET para dar suporte à criação de modelos Code First, incluindo a engenharia reversa de um banco de dados existente. Esses recursos estavam disponíveis anteriormente na qualidade Beta no EF Power Tools.
- **Tratamento de falhas de confirmação de transação** fornece as novas [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/en-us/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) que usam a capacidade de recentemente introduzida para interceptar operações de transação. O **CommitFailureHandler** permite a recuperação automática de falhas de conexão durante a confirmação de uma transação.
- **Indexattribute múltiplas** permite que os índices seja especificado, colocando um atributo em uma propriedade (ou propriedades) em seu modelo Code First. Código primeiro, em seguida, criará um índice correspondente no banco de dados.
- **A API pública do mapeamento** fornece acesso às informações EF tem como propriedades e tipos são mapeados para colunas e tabelas no banco de dados. Em versões anteriores esta API foi interna.
- **Capacidade de configurar interceptores por meio do arquivo App/Web.config**(permitindo interceptores a ser adicionado sem recompilar o aplicativo).
- **DatabaseLogger** é um novo interceptor que torna mais fácil registrar todas as operações de banco de dados em um arquivo. Em combinação com o recurso anterior, isso permite que você alterne facilmente no log de operações de banco de dados para um aplicativo implantado, sem a necessidade de recompilar.
- **Detecção de alteração do modelo de migrações** foi aprimorada para que as migrações scaffolding são mais precisas; o desempenho do processo de detecção de alteração também foi bastante aprimorado.
- **Melhorias de desempenho** incluindo operações de redução de banco de dados durante a inicialização, otimizações para comparação de igualdade nulo em consultas LINQ, mais rápido exibir geração (criação de modelo) em cenários mais e mais eficiente materialização de entidades controladas com várias associações.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>Identidade do ASP.NET 2.0.0

- **Autenticação de dois fatores**: ASP.NET Identity agora dá suporte a autenticação de dois fatores. Autenticação de dois fatores fornece uma camada extra de segurança para as contas de usuário no caso em que a senha for comprometida. Também há proteção contra ataques de força bruta contra os códigos de dois fatores.
- **Bloqueio de conta:** fornece uma maneira de bloquear o usuário se o usuário insere sua senha ou códigos de dois fatores incorretamente. O número de tentativas inválidas e o período de tempo para que os usuários estão bloqueados pode ser configurado. Um desenvolvedor pode, opcionalmente, desativar o bloqueio de conta para determinadas contas de usuário caso precisem.
- **Confirmação de conta:** o sistema de identidade do ASP.NET agora oferece suporte à confirmação da conta. Este é um cenário bastante comum na maioria dos sites hoje onde, quando você registrar uma nova conta no site, você deve confirmar seu email antes de poder fazer qualquer coisa no site. Email de confirmação é útil porque evita que as contas falsas que está sendo criado. Isso é muito útil se você estiver usando o email como um método de comunicação com os usuários do seu site, como sites de fórum, transações bancárias, comércio eletrônico ou sites sociais do web.
- **Redefinição de senha:** redefinição de senha é um recurso em que o usuário pode redefinir suas senhas, se eles esqueceu sua senha.
- **Carimbo de segurança (sair everywhere):** oferece suporte a um modo para gerar o Token de segurança para o usuário em casos quando o usuário altera sua senha ou qualquer outro tipo de segurança relacionadas a informações sobre como remover um logon associado (como o Facebook, Google, Conta da Microsoft e assim por diante). Isso é necessário para garantir que todos os tokens gerados com a senha antiga são invalidados. O projeto de exemplo, se você alterar a senha do usuário, em seguida, um novo token é gerado para o usuário em todos os tokens anteriores são invalidados. Este recurso fornece uma camada extra de segurança para seu aplicativo desde quando você alterar sua senha, você será desconectado de todos os locais (para todos os outros navegadores) em que você fez no aplicativo.
- **Verifique o tipo de chave primária a ser extensível para usuários e funções**: no ASP.NET Identity 1.0, o tipo de chave primária para a tabela de usuários e funções era cadeias de caracteres. Isso significa que quando o sistema de identidade do ASP.NET foi mantido no SQL Server usando o Entity Framework, que estavam usando nvarchar. Havia muitos discussões sobre esta implementação padrão no estouro de pilha e com base nos comentários recebidos. Fornecemos um gancho de extensibilidade onde você pode especificar o que deve ser a chave primária da tabela de usuários e funções. Esse gancho de extensibilidade é particularmente útil que se você estiver migrando seu aplicativo e o aplicativo foi UserIds armazenando são GUIDs ou ints.
- **Suporte a IQueryable de usuários e funções**: suporte adicionado para IQueryable em UsersStore e RolesStore, você pode obter facilmente a lista de usuários e funções.
- **Operação de exclusão de suporte por meio de UserManager**
- **Indexação em nome de usuário**: implementação no ASP.NET Identity Entity Framework, adicionamos um índice exclusivo no nome de usuário usando o indexattribute múltiplas novo no EF 6.1.0. Isso torna-se de que os nomes de usuário sempre são exclusivos e não havia nenhuma condição de corrida em que você pode acabar com nomes duplicados.
- **Validador de senha avançado:** o validador de senha foi fornecida no ASP.NET Identity 1.0 foi um validador de senha bem básica que só foi Validando o comprimento mínimo. Há um validador de senha nova que oferece mais controle sobre a complexidade da senha. Observe que mesmo se você ativa todas as configurações de senha, é recomendável habilitar a autenticação de dois fatores para as contas de usuário.
- **IdentityFactory Middleware / CreatePerOwinContext**:

    - **Gerenciador de usuários**: você pode usar a implementação de fábrica para obter uma instância de UserManager do contexto OWIN. Esse padrão é semelhante ao que usamos para obtenção de AuthenticationManager para entrar e sair do contexto do OWIN. Essa é uma maneira recomendada de obtenção de uma instância do UserManager por solicitação para o aplicativo.
    - **DbContextFactory**: usa o ASP.NET Identity Entity Framework para manter o sistema de identidade no SQL Server. Para fazer isso o sistema de identidade tem uma referência para o ApplicationDbContext. O DbContextFactory Middleware retorna uma instância de ApplicationDbContext por solicitação que você pode usar em seu aplicativo.
- **Pacote de NuGet de exemplos de identidade do ASP.NET**: O exemplos pacote NuGet pode tornar mais fácil de instalar e executar os exemplos para ASP.NET Identity e siga as práticas recomendadas. Este é um exemplo de aplicativo ASP.NET MVC. Modifique o código de acordo com seu aplicativo antes de implantar na produção. O exemplo deve ser instalado em um aplicativo ASP.NET vazio. Para obter mais informações sobre o pacote, vá para a seguinte postagem de blog: [anunciando RTM do ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Componentes do Microsoft OWIN

Havia muitos erros que foram corrigidos nesta versão. Consulte o [notas de versão para o 2.1.0 versão](https://katanaproject.codeplex.com/releases/view/113281) para obter mais informações.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Havia muitos erros que foram corrigidos nesta versão. Consulte o [notas de versão para o 2.0.2 versão](https://github.com/SignalR/SignalR/releases/tag/2.0.2) para obter mais informações.
