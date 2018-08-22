---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Logon único (Criando aplicativos de nuvem do mundo Real com o Azure) | Microsoft Docs
author: MikeWasson
description: Os aplicativos de nuvem construção Real World com o livro eletrônico do Azure baseia-se em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem a ele...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: feace28c373a767453b1b984f6fdc5a3d97a9217
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825582"
---
<a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>O logon único (Criando aplicativos de nuvem do mundo Real com o Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download corrigi-lo Project](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [Baixe o livro eletrônico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> O **aos aplicativos de nuvem Real mundo de construção com o Azure** livro eletrônico se baseia em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas recomendadas que podem ajudá-lo a ser bem-sucedido no desenvolvimento de aplicativos web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).


Há muitos problemas de segurança para pensar quando você estiver desenvolvendo um aplicativo de nuvem, mas para esta série nos concentraremos em apenas um: logon único. Pessoas uma pergunta geralmente perguntam é: "Estou principalmente criando aplicativos para os funcionários da minha empresa; como hospedar esses aplicativos na nuvem e ainda permitir que eles usam o mesmo modelo de segurança que Meus funcionários conhecem e usam no ambiente local quando eles estão executando aplicativos que são hospedados dentro do firewall?" Uma das maneiras em que podemos habilitar esse cenário é chamada Azure Active Directory (Azure AD). Azure AD permite que você disponibilizar enterprise linha de negócios (LOB) aplicativos pela Internet e permite que você disponibilizar esses aplicativos para parceiros de negócios também.

## <a name="introduction-to-azure-ad"></a>Introdução ao Azure AD

[Azure AD](https://docs.microsoft.com/azure/active-directory/) fornece [do Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) na nuvem. Principais recursos incluem o seguinte:

- Ele se integra com o Active Directory no local.
- Ele habilita o logon único com seus aplicativos.
- Ele dá suporte a padrões abertos, como [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation), e [OAuth 2.0](http://oauth.net/2/).
- Ele dá suporte ao Enterprise [Graph REST API](https://msdn.microsoft.com/library/hh974476.aspx).

Suponha que você tenha um ambiente do Windows Server Active Directory local que você usa para permitir que os funcionários entrar aplicativos de Intranet:

![](single-sign-on/_static/image1.png)

O que o AD do Azure permite fazer é criar um diretório na nuvem. Ele é um recurso gratuito e fácil de configurar.

Ele pode ser totalmente independente do seu local no Active Directory; Você pode colocar qualquer pessoa que você deseja nele e autenticá-los em aplicativos de Internet.

![Windows Azure Active Directory](single-sign-on/_static/image2.png)

Ou você pode integrá-lo com suas instalações AD.

![AD e a integração do WAAD](single-sign-on/_static/image3.png)

Agora todos os funcionários que podem se autenticar no local também podem autenticar pela Internet – sem ter que abrir um firewall ou implantar novos servidores no seu data center. Você pode continuar a aproveitar todos os o ambiente existente do Active Directory que você conhece e usa hoje para fornecer seu logon único de aplicativos internos no recurso.

Depois que você fez essa conexão entre o AD e o Azure AD, você também pode habilitar seus aplicativos web e seus dispositivos móveis para autenticar os funcionários na nuvem e você pode permitir que aplicativos de terceiros, como aplicativos do Office 365, SalesForce.com ou do Google, para aceitar sua credenciais de funcionários. Se você estiver usando o Office 365, você já configurá-lo com o Azure AD como o Office 365 usa o Azure AD para autenticação e autorização.

![3º aplicativos de terceiros](single-sign-on/_static/image4.png)

A vantagem dessa abordagem é que, sempre que sua organização adiciona ou exclui um usuário ou um usuário altera uma senha, use o mesmo processo que você usa atualmente no seu ambiente local. Todos os de suas instalações AD alterações são propagadas automaticamente para o ambiente de nuvem.

Se sua empresa está usando ou mover para o Office 365, a boa notícia é que você terá configurado automaticamente como Office 365 usa o Azure AD para autenticação de AD do Azure. Portanto, você pode facilmente usar em seus próprios aplicativos a mesma autenticação que usa o Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Configurar um locatário do AD do Azure

um diretório do AD do Azure é chamado de Azure AD [locatário](https://technet.microsoft.com/library/jj573650.aspx), e a configuração de um locatário é muito fácil. Vamos mostrar como isso é feito no Portal de gerenciamento para ilustrar os conceitos, mas claro, como as outras funções de portal você pode também fazê-lo usando um script ou a API de gerenciamento.

No portal de gerenciamento, clique na guia do Active Directory.

![WAAD no portal](single-sign-on/_static/image5.png)

Você tem um locatário do Azure AD automaticamente para sua conta do Azure, e você pode clicar o **adicionar** botão na parte inferior da página para criar diretórios adicionais. Talvez você queira um para um ambiente de teste e outro para produção, por exemplo. Pense cuidadosamente sobre o que você nomear um novo diretório. Se você usar seu nome para o diretório e, em seguida, você usa o nome novamente por um dos usuários, que pode ser confuso.

![Adicionar diretório](single-sign-on/_static/image6.png)

O portal tem suporte completo para criar, excluir e gerenciar usuários nesse ambiente. Por exemplo, para adicionar um usuário, vá para o **os usuários** guia e clique no **adicionar usuário** botão.

![Botão Adicionar usuário](single-sign-on/_static/image7.png)

![Adicionar caixa de diálogo usuário](single-sign-on/_static/image8.png)

Você pode criar um novo usuário que existe somente nesse diretório, ou você pode registrar uma Account da Microsoft como um usuário nesse diretório, ou o registro ou um usuário de outro diretório do AD do Azure como um usuário neste diretório. (Em um diretório real, o domínio padrão seria ContosoTest.onmicrosoft.com. Você também pode usar um domínio de sua escolha, como contoso.com.)

![Tipos de usuário](single-sign-on/_static/image9.png)

![Adicionar caixa de diálogo usuário](single-sign-on/_static/image10.png)

Você pode atribuir o usuário a uma função.

![Perfil de usuário](single-sign-on/_static/image11.png)

E a conta é criada com uma senha temporária.

![Senha temporária](single-sign-on/_static/image12.png)

Os usuários que você cria dessa forma imediatamente podem fazer logon em seus aplicativos web usando esse diretório na nuvem.

O que é ótimo de enterprise single sign-on, no entanto, é o **integração de diretórios** guia:

![Guia de integração de diretório](single-sign-on/_static/image13.png)

Se você habilitar a integração de diretório, e [baixar uma ferramenta](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), você pode sincronizar esse diretório na nuvem com seu existente no Active Directory local que você já estiver usando dentro da sua organização. Em seguida, todos os usuários armazenados no seu diretório aparecerá nesse diretório de nuvem. Seus aplicativos de nuvem agora podem se autenticar todos os seus funcionários usando suas credenciais existentes do Active Directory. E tudo isso é gratuito – a ferramenta de sincronização e o Azure AD em si.

A ferramenta é um assistente que é fácil de usar, como você pode ver nessas capturas de tela. Esses não são instruções completas, apenas um exemplo que mostra o processo básico. Para obter mais informações de how-to--it, consulte os links a [recursos](#resources) seção no final deste capítulo.

![Assistente de configuração de ferramenta de sincronização de WAAD](single-sign-on/_static/image14.png)

Clique em **próxima**e, em seguida, insira suas credenciais do Active Directory do Azure.

![Assistente de configuração de ferramenta de sincronização de WAAD](single-sign-on/_static/image15.png)

Clique em **próxima**e, em seguida, insira seu local as credenciais do AD.

![Assistente de configuração de ferramenta de sincronização de WAAD](single-sign-on/_static/image16.png)

Clique em **próxima**e, em seguida, indique se deseja armazenar um hash de suas senhas do AD na nuvem.

![Assistente de configuração de ferramenta de sincronização de WAAD](single-sign-on/_static/image17.png)

O hash de senha que você pode armazenar na nuvem é um hash unidirecional; as senhas reais nunca são armazenadas no AD do Azure. Se você decidir não armazenar hashes na nuvem, você terá que usar [serviços de Federação do Active Directory](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Também há [outros fatores a serem considerados ao escolher se deseja ou não usar o ADFS](https://technet.microsoft.com/library/jj573653.aspx). A opção de ADFS exige algumas etapas de configuração adicional.

Se você optar por armazenar hashes na nuvem, você terminou e a ferramenta inicia a sincronização de diretórios quando você clica **próxima**.

![Assistente de configuração de ferramenta de sincronização de WAAD](single-sign-on/_static/image18.png)

E, em alguns minutos você está pronto.

![Assistente de configuração de ferramenta de sincronização de WAAD](single-sign-on/_static/image19.png)

Você só precisa executar isso em um controlador de domínio da organização, no Windows 2003 ou superior. E não há necessidade de reiniciar. Quando você terminar, todos os seus usuários estão na nuvem e você pode fazer o logon único de qualquer aplicativo web ou móvel, usando SAML, OAuth ou WS-Fed.

Às vezes, nos fazemos essa pergunta sobre como proteger, isso é – Microsoft usá-lo para seus próprios dados corporativos confidenciais? E a resposta for Sim, que podemos fazer. Por exemplo, se você for para o site do Microsoft SharePoint interno no [ https://microsoft.sharepoint.com/ ](https://microsoft.sharepoint.com/), obter ser solicitado a fazer logon.

![Entrar no Office 365](single-sign-on/_static/image20.png)

Microsoft tiver habilitado o ADFS, portanto, quando você insere uma ID da Microsoft, será redirecionado para uma página de logon do ADFS.

![Entrada do AD FS](single-sign-on/_static/image21.png)

E, depois que você insira as credenciais armazenadas em uma conta interna do Microsoft AD, você tem acesso a esse aplicativo interno.

![Site do SharePoint do MS](single-sign-on/_static/image22.png)

Estamos usando um servidor de entrada do AD principalmente porque já tínhamos ADFS configurar antes do AD do Azure se tornou disponível, mas o processo de log é feito por meio de um diretório do AD do Azure na nuvem. Podemos colocar nossos documentos importantes, controle do código-fonte, arquivos de gerenciamento de desempenho, relatórios de vendas e obter mais informações, na nuvem e estiver usando essa mesma solução exata para protegê-los.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Criar um aplicativo ASP.NET que usa o Azure AD para logon único

Visual Studio torna muito fácil criar um aplicativo que usa o Azure AD para logon único, como você pode ver no algumas capturas de tela.

Quando você cria um novo aplicativo ASP.NET, MVC ou Web Forms, o método de autenticação padrão é a identidade do ASP.NET. Para alterar isso para o Azure AD, você clica em um **alterar autenticação** botão.

![Alterar autenticação](single-sign-on/_static/image23.png)

Selecione as contas organizacionais, insira seu nome de domínio e, em seguida, selecione o logon único.

![Configurar a caixa de diálogo de autenticação](single-sign-on/_static/image24.png)

Você pode também fornecer o aplicativo de leitura ou leitura/gravação permissão para dados do diretório. Se você fizer isso, ele pode usar o [do Azure Graph REST API](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) para consultar o número de telefone dos usuários, descubra se estiverem no escritório, quando o último logon ativa, etc.

Isso é tudo o que você precisa fazer - Visual Studio solicitará as credenciais de um administrador para seu locatário do Azure AD e, em seguida, ele configura seu projeto e o locatário do Azure AD para o novo aplicativo.

Quando você executa o projeto, você verá uma página de entrada, e você pode entrar com credenciais de um usuário no diretório do AD do Azure.

![Entrada na organização conta](single-sign-on/_static/image25.png)

![Conectado](single-sign-on/_static/image26.png)

Quando você implanta o aplicativo no Azure, tudo o que você precisa fazer é selecionar um **habilitar autenticação organizacional** caixa de seleção e, mais uma vez o Visual Studio cuida de toda a configuração para você.

![Publicar na Web](single-sign-on/_static/image27.png)

Essas capturas de tela vêm de um tutorial passo a passo completo que mostra como criar um aplicativo que usa a autenticação do AD do Azure: [desenvolvimento de aplicativos ASP.NET com o Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Resumo

Neste capítulo, você viu que Azure Active Directory, o Visual Studio e o ASP.NET, torná-lo fácil de configurar o logon único em aplicativos da Internet para os usuários da organização. Os usuários podem entrar aplicativos de Internet usando as mesmas credenciais que eles usam para fazer logon usando o Active Directory na sua rede interna.

O [próximo capítulo](data-storage-options.md) examina as opções de armazenamento de dados disponíveis para um aplicativo de nuvem.

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obter mais informações, consulte os seguintes recursos:

- [Documentação do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Página do portal de documentação do AD do Azure no site windowsazure.com. Para obter tutoriais passo a passo, consulte o **desenvolver** seção.
- [Autenticação multifator do Azure](https://docs.microsoft.com/azure/multi-factor-authentication/). Página do portal para obter a documentação sobre a autenticação multifator no Azure.
- [Opções de autenticação de conta organizacional](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Explicação das opções de autenticação do Azure AD na caixa de diálogo de novo projeto do Visual Studio 2013.
- [Microsoft Patterns and Practices - Federated padrão de identidade](https://msdn.microsoft.com/library/dn589790.aspx).
- [Como: Instalar a ferramenta de sincronização do Azure Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Active Directory Federation Services 2.0 mapa de conteúdo](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Links para documentação sobre ADFS 2.0.
- [Autorização baseada em função e baseada em ACL em um aplicativo do Windows Azure AD](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Aplicativo de exemplo.
- [Blog de API do Graph do Active Directory do Azure](https://blogs.msdn.com/b/aadgraphteam/).
- [Controle de acesso no BYOD e integração de diretórios em uma infraestrutura de identidade híbrida](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Tech Ed 2014 sessão vídeo por Gayana Bagdasaryan.

> [!div class="step-by-step"]
> [Anterior](web-development-best-practices.md)
> [Próximo](data-storage-options.md)
