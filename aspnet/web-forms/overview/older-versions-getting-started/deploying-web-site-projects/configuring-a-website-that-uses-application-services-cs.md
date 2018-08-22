---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
title: Configuração de um site que usa os serviços de aplicativo (c#) | Microsoft Docs
author: rick-anderson
description: Versão do ASP.NET 2.0 introduziu uma série de serviços de aplicativos, que fazem parte do .NET Framework e servir como um conjunto de blocos de construção de serviços que yo...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 1e33d1c6-3f9f-4c26-81e2-2a8f8907bb05
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs
msc.type: authoredcontent
ms.openlocfilehash: 4cec939795c2b3abfd51c894f985dfd2eb7bc361
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825488"
---
<a name="configuring-a-website-that-uses-application-services-c"></a>Configuração de um site que usa os serviços de aplicativo (c#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Baixar o código](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_CS.zip) ou [baixar PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_cs.pdf)

> Versão do ASP.NET 2.0 introduziu uma série de serviços de aplicativos, que fazem parte do .NET Framework e servir como um pacote de serviços de bloco de construção que você pode usar para adicionar uma funcionalidade avançada ao seu aplicativo web. Este tutorial explora como configurar um site da Web no ambiente de produção para usar serviços de aplicativos e aborda problemas comuns com o gerenciamento de contas de usuário e funções no ambiente de produção.


## <a name="introduction"></a>Introdução

Versão do ASP.NET 2.0 introduziu uma série de *serviços de aplicativo*, que fazem parte do .NET Framework e serve como um conjunto de blocos de construção de serviços que você pode usar para adicionar uma funcionalidade avançada ao seu aplicativo web. Os serviços de aplicativo incluem:

- **Associação** – uma API para criar e gerenciar contas de usuário.
- **Funções** – uma API para categorizar os usuários em grupos.
- **Perfil** – uma API para o armazenamento de conteúdo personalizado, específicos do usuário.
- **Mapa de site** – uma API para definir uma estrutura lógica do site s na forma de uma hierarquia, o que pode ser exibida por meio de controles de navegação, como menus e trilhas.
- **Personalização** – uma API para manter as preferências de personalização, usadas com mais frequência [ *WebParts*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Monitoramento de integridade** – uma API para monitoramento de desempenho, segurança, erros e outras métricas de integridade do sistema para um aplicativo web em execução.
  

As APIs de serviços de aplicativo não estão vinculadas a uma implementação específica. Em vez disso, você pode instruir os serviços de aplicativo para usar um determinado *provedor*, e esse provedor implementa o serviço usando uma tecnologia específica. Os provedores mais comumente usados para aplicativos web baseados na Internet hospedados em uma empresa de hospedagem na web são os provedores que usam uma implementação de banco de dados do SQL Server. Por exemplo, o `SqlMembershipProvider` é um provedor para a API de associação que armazena informações de conta de usuário em um banco de dados do Microsoft SQL Server.

Usar os serviços de aplicativo e os provedores SQL Server adiciona alguns desafios ao implantar o aplicativo. Para os iniciantes, os objetos de banco de dados de serviços de aplicativo devem ser criados corretamente em bancos de dados de produção e de desenvolvimento e inicializados corretamente. Também há definições de configuração importantes que precisam ser feitas.

> [!NOTE]
> Os serviços de aplicativo APIs foram criadas usando o [ *modelo de provedor*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), um padrão de design que permite que uma API s detalhes de implementação a ser fornecido em tempo de execução. O .NET Framework vem com um número de provedores de serviço de aplicativo que pode ser usado, como o `SqlMembershipProvider` e `SqlRoleProvider`, que são provedores para a associação e a implementação de banco de dados de APIs de funções que usam um SQL Server. Você também pode criar e plug-in um provedor personalizado. Na verdade, o aplicativo da web de resenhas de livros já contém um provedor personalizado para a API de mapa de Site (`ReviewSiteMapProvider`), que constrói o mapa do site dos dados do `Genres` e `Books` tabelas no banco de dados.


Este tutorial começa com uma olhada em como eu estendido que o aplicativo web de resenhas de livros para usar as APIs de funções e associação. Em seguida, percorre Implantando um aplicativo web que usa serviços de aplicativos com uma implementação de banco de dados do SQL Server e é concluído com a resolver problemas comuns com o gerenciamento de contas de usuário e funções no ambiente de produção.

## <a name="updates-to-the-book-reviews-application"></a>Atualizações para o aplicativo de revisões do livro

Nos últimos tutoriais que o aplicativo web de livro examina foi atualizado de um site estático para um aplicativo da web dinâmicos, controlados por dados, conclua com um conjunto de páginas de administração para gerenciar gêneros e revisões. No entanto, esta seção Administração atualmente não está protegida - qualquer usuário que sabe (ou adivinha) a URL da página de administração pode waltz em e criar, editar ou excluir revisões em nosso site. Uma maneira comum de proteger a determinadas partes de um site é implementar contas de usuário e, em seguida, usar as regras de autorização de URL para restringir o acesso a determinados usuários ou funções. O aplicativo web de resenhas de livros disponível para download com este tutorial oferece suporte a funções e contas de usuário. Ele tem uma única chamada de função definida pela Admin e somente os usuários nesta função podem acessar as páginas de administração.

> [!NOTE]
> Eu ve criou três contas de usuário no aplicativo web resenhas de livros: Scott, Jisun e Alice. Todos os três usuários têm a mesma senha: **senha!** Scott e Jisun estão na função de administrador, Alice não é. As páginas não-administração do site s ainda são acessíveis a usuários anônimos. Ou seja, não é necessário entrar para visitar o site, a menos que você deseja administrar, nesse caso, você deve entrar como um usuário na função de administrador.


A página mestra do resenhas de livros aplicativo s foi atualizada para incluir uma interface do usuário diferentes para usuários anônimos e autenticados. Se um usuário anônimo visita o site, ela vê um link de logon no canto superior direito. Um usuário autenticado vê a mensagem "Bem-vindo novamente, *nome de usuário*!" e um link para fazer logoff. Há também uma página de logon do s (`~/Login.aspx`), que contém um controle de Web de logon que fornece a interface do usuário e a lógica para autenticar um visitante. Somente administradores podem criar novas contas. (Há páginas para criar e gerenciar contas de usuário a `~/Admin` pasta.)

### <a name="configuring-the-membership-and-roles-apis"></a>Configurando a associação e funções APIs

O aplicativo web de resenhas de livros usa as APIs de funções e a associação para dar suporte a contas de usuário e agrupar os usuários em funções (ou seja, a função de administrador). O `SqlMembershipProvider` e `SqlRoleProvider` classes de provedor são usadas porque queremos armazenar informações de conta e a função em um banco de dados do SQL Server.

> [!NOTE]
> Este tutorial não pretende ser um exame detalhado sobre a configuração de um aplicativo web para dar suporte a associação e funções de APIs. Para obter uma visão completa dessas APIs e as etapas necessárias para configurar um site da Web para usá-las, leia minha [ *tutoriais de segurança do site*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Para usar os serviços de aplicativo com um banco de dados do SQL Server que é necessário primeiro adicionar os objetos de banco de dados usados por esses provedores para o banco de dados onde você deseja que a conta de usuário e informações de função armazenados. Esses objetos de banco de dados necessárias incluem uma variedade de tabelas, exibições e procedimentos armazenados. A menos que especificado de outra forma, o `SqlMembershipProvider` e `SqlRoleProvider` classes de provedor usam um banco de dados do SQL Server Express Edition denominado `ASPNETDB` localizado em aplicativo s `App_Data` pasta; se um banco de dados não existir, ele será criado automaticamente com os objetos de banco de dados necessários por esses provedores em tempo de execução.

É possível, e geralmente ideal para criar o aplicativo Serviços de objetos de banco de dados no mesmo banco de dados onde os dados do site s específicos do aplicativo são armazenados. O .NET Framework vem com uma ferramenta chamada `aspnet_regsql.exe` que instala os objetos de banco de dados em um banco de dados especificado. Adiantamos e usado essa ferramenta para adicionar esses objetos para o `Reviews.mdf` banco de dados a `App_Data` pasta (o banco de dados de desenvolvimento). Veremos como usar essa ferramenta posteriormente no tutorial quando adiciono esses objetos no banco de dados de produção.

Se você adicionar o aplicativo de serviços de objetos de banco de dados para um banco de dados diferente de `ASPNETDB` você precisará personalizar o `SqlMembershipProvider` e `SqlRoleProvider` classes de provedor de configurações para que eles usem o banco de dados apropriado. Para personalizar o provedor de associação, adicione uma [  *&lt;associação&gt; elemento* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) dentro de `<system.web>` seção `Web.config`; usar o [  *&lt;roleManager&gt; elemento* ](https://msdn.microsoft.com/library/ms164660.aspx) para configurar o provedor de funções. O trecho a seguir é obtido de resenhas de livros aplicativo s `Web.config` e mostra as definir configurações para a associação e funções de APIs. Observe que ambos registrar um novo provedor - `ReviewMembership` e `ReviewRole` -que usam o `SqlMembershipProvider` e `SqlRoleProvider` provedores, respectivamente.

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample1.xml)]

O `Web.config` arquivo s `<authentication>` elemento também foi configurado para dar suporte à autenticação baseada em formulários.
  

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Limitando o acesso às páginas de administração

ASP.NET torna mais fácil conceder ou negar acesso a um determinado arquivo ou pasta por usuário ou função por meio de seu *autorização de URL* recurso. (Brevemente discutimos autorização de URL na *principais diferenças entre o IIS e o ASP.NET Development Server* tutorial e mostrou como o IIS e o ASP.NET Development Server aplicam regras de autorização de URL diferentes para estático em comparação com conteúdo dinâmico.) Como queremos proibir o acesso ao `~/Admin` pasta, exceto para os usuários na função de administrador, é necessário adicionar regras de autorização de URL para essa pasta. Especificamente, as regras de autorização de URL é necessário permitir que os usuários na função de administrador e negar todos os outros usuários. Isso é feito pela adição de um `Web.config` o arquivo para o `~/Admin` pasta com o seguinte conteúdo:

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample3.xml)]

Para obter mais informações sobre o recurso de autorização de URL do ASP.NET s e como usá-lo para descrever as regras de autorização para usuários em funções, certifique-se de ler o [ *autorização baseada em usuário* ](../../older-versions-security/membership/user-based-authorization-cs.md) e [ *Autorização baseada em função* ](../../older-versions-security/roles/role-based-authorization-cs.md) tutoriais do meu [ *tutoriais de segurança de site*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Implantar um aplicativo Web que usa os serviços de aplicativo

Ao implantar um site que usa serviços de aplicativo e um provedor que armazena as informações de serviços de aplicativo em um banco de dados, é imperativo que os objetos de banco de dados necessários para os serviços de aplicativo criados no banco de dados de produção. Inicialmente o banco de dados de produção não contém esses objetos, isso quando o aplicativo é implantado pela primeira vez (ou quando ele é implantado pela primeira vez depois que os serviços de aplicativo foram adicionados), você deve executar etapas adicionais para obter esses objetos de banco de dados necessárias sobre o banco de dados de produção.

Outro desafio pode surgir ao implantar um site que usa serviços de aplicativos, se você pretende replicar as contas de usuário criadas no ambiente de desenvolvimento para o ambiente de produção. Dependendo da configuração de associação e funções, é possível que, mesmo se você copia com êxito as contas de usuário que foram criadas no ambiente de desenvolvimento para o banco de dados de produção, esses usuários não podem entrar no aplicativo web em produção. Vamos examinar a causa desse problema e discutir como impedir que isso aconteça.

ASP.NET é fornecido com um bom [ *ferramenta de administração de Site da Web (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) que pode ser iniciado no Visual Studio e permite que o usuário as regras de autorização, funções e conta a ser gerenciado por meio de um baseado na web interface. Infelizmente, o WSAT funciona somente para sites da Web local, que significa que ele não pode ser usado para gerenciar remotamente as contas de usuário, funções e regras de autorização para o aplicativo web no ambiente de produção. Vamos examinar diferentes maneiras de implementar o comportamento do tipo WSAT do seu site de produção.

### <a name="adding-the-database-objects-using-aspnetregsqlexe"></a>Adicionando o objetos de banco de dados usando aspnet\_regsql.exe

O *Implantando um banco de dados* tutorial mostrou como copiar tabelas e dados do banco de dados de desenvolvimento para o banco de dados de produção, e essas técnicas certamente podem ser usadas para copiar os objetos de banco de dados de serviços de aplicativo para o banco de dados de produção. Outra opção é o `aspnet_regsql.exe` ferramenta, que adiciona ou remove os objetos de banco de dados de serviços de aplicativo de um banco de dados.

> [!NOTE]
> O `aspnet_regsql.exe` ferramenta cria os objetos de banco de dados em um banco de dados especificado. Ele não migra dados nesses objetos de banco de dados do banco de dados de desenvolvimento para o banco de dados de produção. Se quiser copiar as informações de conta e a função de usuário no banco de dados de desenvolvimento para o banco de dados de produção, use as técnicas abordadas as *Implantando um banco de dados* tutorial.


Permitir que o s examinar como adicionar os objetos de banco de dados para o banco de dados de produção usando o `aspnet_regsql.exe` ferramenta. Comece abrindo o Windows Explorer e navegando até o diretório da versão 2.0 do .NET Framework no seu computador, %WINDIR%\ Microsoft.NET\Framework\v2.0.50727. Lá você deve encontrar o `aspnet_regsql.exe` ferramenta. Essa ferramenta pode ser usada na linha de comando, mas ele também inclui uma interface gráfica do usuário; Clique duas vezes o `aspnet_regsql.exe` arquivo para iniciar seu componente de gráfico.

A ferramenta é iniciada, exibindo uma tela de abertura, explicando sua finalidade. Clique em Avançar para ir à tela de "Selecione uma opção de instalação", que é mostrada na Figura 1. A partir daqui, você pode optar por adicionar os serviços de aplicativo objetos de banco de dados ou removê-los de um banco de dados. Como queremos adicionar esses objetos no banco de dados de produção, selecione a opção "Configurar o SQL Server para serviços de aplicativos" e clique em Avançar.


[![Optar por configurar o SQL Server para serviços de aplicativos](configuring-a-website-that-uses-application-services-cs/_static/image2.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image1.jpg)

**Figura 1**: escolha para configurar o SQL Server para serviços de aplicativo ([clique para exibir a imagem em tamanho normal](configuring-a-website-that-uses-application-services-cs/_static/image3.jpg))


Em "Selecionar o servidor e banco de dados" tela solicita informações para se conectar ao banco de dados. Insira o nome do banco de dados fornecido a você por sua empresa de hospedagem de web, as credenciais de segurança e o servidor de banco de dados e clique em Avançar.

> [!NOTE]
> Depois de inserir o seu servidor de banco de dados e as credenciais, você poderá receber um erro ao expandir a lista suspensa de banco de dados. O `aspnet_regsql.exe` ferramenta de consultas a `sysdatabases` tabela do sistema para recuperar uma lista de bancos de dados do servidor, mas alguns web seus servidores de banco de dados de bloqueio de empresas de hospedagem para que essas informações não estão disponíveis publicamente. Se você receber esse erro, você pode digitar o nome do banco de dados diretamente na lista suspensa.


[![Forneça a ferramenta com suas informações de Conexão do banco de dados s](configuring-a-website-that-uses-application-services-cs/_static/image5.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image4.jpg)

**Figura 2**: fornecer o informações de Conexão do s ferramenta com seu banco de dados ([clique para exibir a imagem em tamanho normal](configuring-a-website-that-uses-application-services-cs/_static/image6.jpg))


A tela subsequente resume as ações que estão prestes a ocorrer, ou seja, que serão os objetos de banco de dados de serviços de aplicativo a ser adicionado ao banco de dados especificado. Clique em Avançar para concluir esta ação. Após alguns instantes, a tela final é exibida, indicando que os objetos de banco de dados foram adicionados (veja a Figura 3).


[![Sucesso! Os objetos de banco de dados de serviços de aplicativo foram adicionados ao banco de dados de produção](configuring-a-website-that-uses-application-services-cs/_static/image8.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image7.jpg)

**Figura 3**: sucesso! O aplicativo Serviços de banco de dados de objetos foram adicionados ao banco de dados de produção ([clique para exibir a imagem em tamanho normal](configuring-a-website-that-uses-application-services-cs/_static/image9.jpg))


Para verificar se os objetos de banco de dados de serviços de aplicativo foram adicionados com êxito no banco de dados de produção, abra o SQL Server Management Studio e conecte-se ao banco de dados de produção. Como mostra a Figura 4, agora você deve ver as tabelas de banco de dados de serviços de aplicativo em seu banco de dados `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`e assim por diante.


[![Confirme se os objetos de banco de dados foram adicionados ao banco de dados de produção](configuring-a-website-that-uses-application-services-cs/_static/image11.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image10.jpg)

**Figura 4**: Confirme se os objetos de banco de dados foram adicionados ao banco de dados de produção ([clique para exibir a imagem em tamanho normal](configuring-a-website-that-uses-application-services-cs/_static/image12.jpg))


Você só precisará usar o `aspnet_regsql.exe` ferramenta ao implantar seu aplicativo da web pela primeira vez ou pela primeira vez depois de iniciar usando os serviços de aplicativo. Depois que esses objetos de banco de dados estão no banco de dados de produção que eles ganharam t é preciso ser adicionado novamente ou modificado.

### <a name="copying-user-accounts-from-development-to-production"></a>Copiar contas de usuário de desenvolvimento para produção

Ao usar o `SqlMembershipProvider` e `SqlRoleProvider` classes de provedor para armazenar as informações de serviços de aplicativo em um banco de dados do SQL Server, as informações de conta e a função de usuário são armazenadas em uma variedade de tabelas do banco de dados, incluindo `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`, e `aspnet_UsersInRoles`, entre outros. Se, durante o desenvolvimento de criar contas de usuário no ambiente de desenvolvimento, você pode replicar essas contas de usuário na produção, copiando os registros correspondentes das tabelas do banco de dados aplicável. Se você usou o Assistente de publicação de banco de dados para implantar os objetos de banco de dados de serviços de aplicativo que também eleita para copiar os registros, o que resultam em contas de usuário criadas no desenvolvimento também esteja em produção. Mas, dependendo de suas definições de configuração, você pode achar que os usuários cujas contas foram criadas no desenvolvimento e copiadas para a produção são incapazes de fazer logon no site de produção. O que acontece?

O `SqlMembershipProvider` e `SqlRoleProvider` classes de provedor foram criadas, de modo que único banco de dados pode servir como um repositório de usuários para vários aplicativos, onde cada aplicativo poderia, teoricamente, tem usuários com sobreposição de nomes de usuário e funções com o mesmo nome. Para permitir essa flexibilidade, o banco de dados mantém uma lista de aplicativos na `aspnet_Applications` tabela e cada usuário está associado com um desses aplicativos. Especificamente, o `aspnet_Users` a tabela tem uma `ApplicationId` coluna que vincula a cada usuário a um registro no `aspnet_Applications` tabela.

Além de `ApplicationId` coluna, o `aspnet_Applications` tabela também inclui um `ApplicationName` coluna, que fornece um nome mais amigável a humanos para o aplicativo. Quando um site tenta trabalhar com uma conta de usuário, como validar uma credencial de usuário s da página de logon, ele deve informar o `SqlMembershipProvider` classe o aplicativo para trabalhar com. Ele normalmente faz isso fornecendo o nome do aplicativo e este valor vem da configuração do provedor s no `Web.config` – especificamente por meio de `applicationName` atributo.

Mas o que acontece se o `applicationName` atributo não for especificado em `Web.config`? Nesse caso, a associação de sistema usa o caminho raiz do aplicativo como o `applicationName` valor. Se o `applicationName` atributo não for definido explicitamente `Web.config`, em seguida, há a possibilidade de que o ambiente de desenvolvimento e o ambiente de produção usam uma raiz de aplicativo diferente e, portanto, será associados com um aplicativo diferente nomes dos serviços de aplicativos. Se tal divergência ocorre em seguida, os usuários criados no ambiente de desenvolvimento terá um `ApplicationId` valor que não coincide com o `ApplicationId` valor para o ambiente de produção. O resultado líquido é que os usuários ganhas t poderão fazer logon.

> [!NOTE]
> Se você estiver nessa situação – com contas de usuário copiadas para a produção com um incompatíveis `ApplicationId` valor – você pode escrever uma consulta para atualizar esses incorreto `ApplicationId` valores para o `ApplicationId` usado na produção. Depois de atualizado, os usuários cujas contas foram criadas no ambiente de desenvolvimento agora será capazes de entrar no aplicativo web em produção.


A boa notícia é que há uma etapa simples, você pode tomar para garantir que os dois ambientes usam o mesmo `ApplicationId` – defina explicitamente o `applicationName` atributo em `Web.config` para todos os seus provedores de serviços de aplicativo. Posso definir explicitamente o `applicationName` do atributo como "BookReviews" no `<membership>` e `<roleManager>` elementos como este trecho de `Web.config` mostra.

[!code-xml[Main](configuring-a-website-that-uses-application-services-cs/samples/sample4.xml)]

Para obter mais discussões sobre a configuração o `applicationName` atributo e a lógica por trás dele, consulte [ *Scott Guthrie* ](https://weblogs.asp.net/scottgu/) postagem de blog de s [ *sempre definir applicationName propriedade ao configurar a associação do ASP.NET e outros provedores*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Gerenciar contas de usuário no ambiente de produção

A ferramenta de administração de Site da Web do ASP.NET (WSAT) torna mais fácil criar e gerenciar contas de usuário, definir e aplicar funções e descrever as regras de autorização baseada em função e usuário. Você pode iniciar o WSAT do Visual Studio indo para o Gerenciador de soluções e clicando no ícone de configuração do ASP.NET ou indo para os site ou projeto menus e selecionando o item de menu de configuração do ASP.NET. Infelizmente, o WSAT só funcionam com sites da Web local. Portanto, é possível usar o WSAT da estação de trabalho para gerenciar o site no ambiente de produção.

A boa notícia é que todas as funcionalidades expostas fornecidas pelo WSAT está disponível programaticamente por meio de associação e funções APIs; Além disso, muitas das telas WSAT usam controles padrão relacionados ao logon do ASP.NET. Em resumo, você pode adicionar páginas ASP.NET ao seu site que oferecem os recursos de gerenciamento necessários.

Lembre-se de que um tutorial anterior atualizado o aplicativo web de resenhas de livros para incluir um `~/Admin` pasta e essa pasta foi configurado para permitir que somente usuários na função de administrador. Eu adicionei uma página para essa pasta chamada `CreateAccount.aspx` do que um administrador pode criar uma nova conta de usuário. Essa página usa o controle CreateUserWizard para exibir a lógica de back-end e de interface do usuário para criar uma nova conta de usuário. Novidades mais, eu personalizado que o controle para incluir uma caixa de seleção se o novo usuário também deve ser adicionado à função de administrador (consulte a Figura 5). Com um pouco de trabalho, você pode criar um conjunto personalizado de páginas que implementa os usuário e a função gerenciamento tarefas relacionadas que caso contrário, pode ser fornecidas pelo WSAT.

> [!NOTE]
> Para obter mais informações sobre como usar a associação e funções de APIs, juntamente com os controles relacionados ao logon da Web do ASP.NET, certifique-se de ler minha [ *tutoriais de segurança do site*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Para obter mais informações sobre como personalizar o controle CreateUserWizard, consulte o [ *criando contas de usuário* ](../../older-versions-security/membership/creating-user-accounts-cs.md) e [ *armazenar informações de usuário adicionais* ](../../older-versions-security/membership/storing-additional-user-information-cs.md) tutoriais ou check-out [ *Erich Peterson* ](http://www.erichpeterson.com/) artigo de s [ *Personalizando o controle CreateUserWizard* ](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).


[![Os administradores podem criar novas contas de usuário](configuring-a-website-that-uses-application-services-cs/_static/image14.jpg)](configuring-a-website-that-uses-application-services-cs/_static/image13.jpg)

**Figura 5**: os administradores podem criar novas contas de usuário ([clique para exibir a imagem em tamanho normal](configuring-a-website-that-uses-application-services-cs/_static/image15.jpg))


Se você precisar da funcionalidade completa do check-out no WSAT [ *sem interrupção sua própria ferramenta Web Site Administration*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx), no qual o autor Dan Clem percorre o processo de criação de uma ferramenta semelhante WSAT personalizada. Dan compartilha seu código-fonte aplicativo s (em c#) e fornece instruções passo a passo para adicioná-lo ao seu site hospedado.

## <a name="summary"></a>Resumo

Ao implantar um aplicativo web que usa a implementação de banco de dados de serviços de aplicativo primeiro você deve garantir que o banco de dados de produção tem os objetos de banco de dados necessárias. Esses objetos podem ser adicionados usando as técnicas discutidas a *Implantando um banco de dados* tutorial; como alternativa, você pode usar o `aspnet_regsql.exe` ferramenta, como vimos neste tutorial. Outros desafios que abordamos na giram em torno de sincronização usado em ambientes de desenvolvimento e produção (que é importante se você quiser que os usuários e as funções criadas no ambiente de desenvolvimento seja válido em produção) e técnicas para o nome do aplicativo Gerenciando os usuários e funções no ambiente de produção.

Boa programação!

### <a name="further-reading"></a>Leitura adicional

Para obter mais informações sobre os tópicos abordados neste tutorial, consulte os seguintes recursos:

- [*Ferramenta de registro do servidor SQL do ASP.NET (aspnet_regsql.exe)*](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Criando o banco de dados de serviços de aplicativo para o SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Criando o esquema de associação no SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md)
- [*Examinando o perfil, funções e associação do ASP.NET s*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Sem interrupção de sua própria ferramenta de administração de Site da Web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Tutoriais de segurança do site*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Visão geral da ferramenta de administração de Site da Web*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Anterior](configuring-the-production-web-application-to-use-the-production-database-cs.md)
> [Próximo](strategies-for-database-development-and-deployment-cs.md)
